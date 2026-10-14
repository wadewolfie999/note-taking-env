# PySR Process and TensorBoard Event Inspection Commands

**Version:** 0.0.1 *(document metadata)* · **Revised:** 2026-10-15 · **Status:** Historical command record

Dated 2026-10-15. This note preserves the complete command response from the earlier PySR diagnosis. The PID, project paths, and capture path below refer to that historical run; verify a process identity before using any PID again. The local stack capture itself, training outputs, datasets, and session logs are not included here.

At the final historical diagnostic check, PID 33212 was still running at about 98% CPU, and the sampled Python stack placed it inside `model.fit()` at line 80, in PySR's equation-export path: `get_hof() → add_export_formats() → pysr2sympy()`. The sampled SymPy path included `tanh` assumption checks, `Mod.eval()`, and polynomial GCD routines. The exact equation row, remaining runtime, and amount of useful progress were unknown. This is a historical finding, not a current process check.

TensorBoard event files were read directly with a standard-library reader; the TensorBoard server and CLI were not started. The reader checks record lengths but does not verify TFRecord CRCs. No model or run output is included.

## Complete saved command response

These are the commands I used. They ran with the working directory set to `/Users/vaheedgorgeen/libs/train-symexpr-bsm`. PID `33212` was verified during that diagnosis; I have not checked its identity again in this turn.

For **py-spy**, I installed it into the project’s `.venv` and checked its options:

```zsh
.venv/bin/python -m pip install py-spy

.venv/bin/py-spy --version
.venv/bin/py-spy dump --help
.venv/bin/py-spy record --help

.venv/bin/python -m pip check
```

I inspected `record --help`, but did **not** run a recording.

The first stack-dump attempt was:

```zsh
.venv/bin/py-spy dump \
  --pid 33212 \
  --nonblocking \
  --full-filenames
```

That reported that root access was required on macOS. I then tried:

```zsh
sudo -n /Users/vaheedgorgeen/libs/train-symexpr-bsm/.venv/bin/py-spy dump \
  --pid 33212 \
  --nonblocking \
  --full-filenames
```

That failed because `sudo` required a password. The successful command used the native macOS administrator authentication dialog:

```zsh
/usr/bin/osascript -e 'do shell script "/Users/vaheedgorgeen/libs/train-symexpr-bsm/.venv/bin/py-spy dump --pid 33212 --nonblocking --full-filenames" with administrator privileges'
```

`--nonblocking` tells py-spy to collect the frames without pausing the target process. I saved the successful command’s output at `~/libs/train-symexpr-bsm/temp/pysr-diagnosis-v0.0.1-46DosvJI/py-spy-dump-03.txt`.

For **TensorBoard**, I did **not** start its server or run its CLI. I inspected the existing event files directly.

This was the timestamp and size check:

```zsh
stat -f '%N | bytes=%z | modified=%Sm' \
  -t '%Y-%m-%d %H:%M:%S %z' \
  logs/_2/*
```

To inspect the event contents, I ran the following Python reader through stdin. It uses only the standard library, reads the TFRecord framing and protobuf fields, and prints the last 12 events. This is what exposed the final `_hparams_/experiment`, `_hparams_/session_start_info`, and `_hparams_/session_end_info` records.

```zsh
.venv/bin/python -B - <<'PY'
from pathlib import Path
from datetime import datetime
from zoneinfo import ZoneInfo
import struct

def varint(b,i):
 v=0; shift=0
 while i<len(b):
  x=b[i]; i+=1; v|=(x&127)<<shift
  if not x&128:return v,i
  shift+=7
 raise ValueError('short varint')

def fields(b):
 i=0
 while i<len(b):
  key,i=varint(b,i); n=key>>3; t=key&7
  if t==0: val,i=varint(b,i)
  elif t in (1,5):
   size={1:8,5:4}[t]; val=b[i:i+size]; i+=size
  elif t==2:
   size,i=varint(b,i); val=b[i:i+size]; i+=size
  else:raise ValueError((n,t))
  yield n,t,val

p=next(Path('logs/_2').glob('*'))
last=[]
with p.open('rb') as f:
 while hdr:=f.read(12):
  if len(hdr)!=12:break
  size=struct.unpack('<Q',hdr[:8])[0]; b=f.read(size); crc=f.read(4)
  if len(b)!=size or len(crc)!=4:break
  last.append(b); last=last[-12:]

for b in last:
 desc=[]
 for n,t,v in fields(b):
  if n==1:
   desc.append(('wall',datetime.fromtimestamp(
    struct.unpack('<d',v)[0],ZoneInfo('Asia/Tehran')
   ).isoformat()))
  if n==2:desc.append(('step',v))
  if n==5:
   for nn,tt,vv in fields(v):
    if nn!=1:continue
    tag=''; meta=[]; simple=None
    for a,ttt,c in fields(vv):
     if a==1:tag=c.decode(errors='replace')
     elif a==2:simple=struct.unpack('<f',c)[0]
     elif a==9:
      for m,mt,mv in fields(c):
       if m==1:
        meta.extend([
         (z,zz.decode(errors='replace')
          if z==1 else ('bytes '+str(len(zz))))
         for z,zt,zz in fields(mv)
        ])
    desc.append(('tag',tag,'simple',simple,'metadata',meta))
 print(desc)
PY
```

That reader checks record lengths but does not validate the TFRecord CRCs. It does not import PySR, initialize Julia, or load the model.
