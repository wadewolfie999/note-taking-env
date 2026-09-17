Decision to Execution Method
============================

This is a proposed working method for turning a finalized AI-assisted decision into an organized execution process. Its practical test is whether a fresh thread can resume useful work from current records without reconstructing the original discussion.

The first execution prompt establishes three things together: the agreed work, the rules for managing that work, and the first useful action. Continuity then depends on maintaining those rules and records throughout execution. A well-worded opening prompt helps initialize the process; observable checkpoints make it assessable over time.

The method applies to software, research, writing, learning, and other extended projects. It does not depend on a particular AI model, task manager, or integration.

1. Capture what the decision commits you to.

Record the selected direction, intended outcome, and evidence that will establish completion. Include scope, exclusions, known constraints, and the AI’s delegated authority. Keep the rationale short; retain only the assumptions or tradeoffs that affect execution.

A finalized decision can coexist with incomplete implementation details. Mark those details as open or provisional. Settle them when they affect the next task. Material new evidence may justify revisiting a specific part of the decision; preserve the reason for that change.

Distinguish the overall outcome from the first milestone. For example, “build the complete research workflow” describes a broad outcome; “produce one reproducible run whose inputs and outputs can be inspected” can define an early milestone. This distinction makes action possible before every later detail is known.

2. Give the project one durable entry point.

Use the project’s existing authoritative records when available. Otherwise, start a single execution record, such as EXECUTION.md. The record can be a document, a repository file, or the equivalent view in an existing tracker. File count is a convenience; each kind of information needs a clear authoritative home.

| Part of the record | Minimum useful content | Update when |
| --- | --- | --- |
| Execution brief | Agreed decision, outcome, completion criteria, scope, constraints, delegated authority, accepted assumptions | The owner changes the agreement or an assumption becomes invalid |
| Work map | Milestones with completion checks; actionable tasks with IDs, responsible parties, statuses, dependencies, outputs, and checks | Work is admitted, decomposed, started, blocked, verified, or deferred |
| Current state | Date/revision, active task, relevant artifacts, verified results, uncertainty, exact next action | A meaningful result or change occurs; before a session ends |
| Open items and history | Blockers, questions, decisions needed, proposals, deferred observations, dated reasons for material changes | Something becomes unresolved, changes, or is resolved |

Place a small “start here” entry at the top: the latest checkpoint date/revision, the active task ID, the next action, and the locations of the materials needed to perform it. Link to task and evidence entries instead of maintaining competing summaries.

When the record becomes difficult to use, split the relevant section into a separate record and keep the entry point. Existing issue trackers can own task state while the execution record owns the brief and restart instructions. Avoid copying the same task state into multiple competing systems.

3. Plan far enough to choose the next useful action.

Outline the major milestones and important dependencies. Fully specify the next useful task and any imminent prerequisite. Leave distant work coarse and identify uncertainty explicitly. Avoid treating speculative estimates or a detailed long-range task list as established facts.

For an actionable task, use this compact shape:

| Field | Meaning |
| --- | --- |
| ID and outcome link | A stable reference, plus the milestone or outcome this task advances |
| Responsible party | Who can carry it out or resolve its blocker |
| Status | Queued, ready, active, blocked, verifying, done, or deferred, using existing equivalents where possible |
| Dependencies | Required task IDs, inputs, access, or decisions |
| Output | The concrete artifact or observable result |
| Completion check | The evidence needed to mark it done |

A new task enters the work map with its purpose and dependencies. If it changes the agreed scope or requires new authority, retain it as a proposal until the relevant decision is made. Capture incidental ideas without automatically turning them into active work.

4. Execute, verify, and record as one working cycle.

For each bounded unit of work:

- Read the checkpoint and the evidence needed for the task.
- Confirm the task is ready and within delegated authority.
- Perform the work and inspect its output.
- Verify the completion criterion.
- Update task status, current state, dependencies, and open items.
- Select the next ready action and save a checkpoint.

“Done” means the completion check has passed. A plausible explanation, a generated plan, or uninspected output does not establish that a substantive task is complete. If verification is unavailable, record the output and the unresolved check separately.

Routine decisions within delegated authority should not require repeated owner approval. Ask when a necessary choice would change the agreement or exceed the available authority. A blocker should identify the precise missing input and who or what can supply it. Continue useful independent work where possible.

Checkpoint at meaningful boundaries, during long tasks, and before pausing. If work is interrupted, retain its location, its partial status, and the safe recovery step. A later thread should verify relevant artifacts before retrying an action that may already have succeeded.

5. Resume through evidence and a small restart instruction.

In a fresh thread, provide access to the current entry point and use this instruction:

> Read the current execution record at [location] and its linked brief, tasks, and relevant artifacts. Follow the recorded execution rules and authority limits. Reconcile changes since the latest checkpoint, identify the next ready task, and continue within the existing scope. Update the same records. Ask only for missing information or authority that prevents the next useful action.

A link is useful only if that thread can access it. When access is unavailable, supply the latest record and the specific artifacts required for the next task. Report gaps explicitly. If working entirely in a chat without a writable durable record, the user must save and supply the checkpoint; the AI must label that limitation accurately.

With overlapping threads, avoid having several threads overwrite the same state independently. Allocate distinct task IDs and one responsibility for reconciling the shared checkpoint. Record a transfer of that responsibility when switching threads. This coordination is only needed if simultaneous work actually occurs.

At a major milestone or after a long pause, recheck the outcome, constraints, important dependencies, and evidence that may have become stale. Update the work map as needed. Reopen an agreed decision only when a concrete reason warrants it.

6. Keep second-order consequences useful and limited.

Raise an outside consequence when there is a concrete connection to execution or another known commitment. For example: the next milestone’s estimated effort conflicts with an already established deadline in another project. State the basis, uncertainty, consequence, and smallest useful response.

Limit these observations to at most two at a checkpoint. If none matter, omit the section. Keep observations and proposed new work separate from authorized project work. Do not invent a wider personal agenda or expand the project merely because another improvement is imaginable.

The opening session passes its practical check when it produces:

- An accessible execution brief and current record, or an explicitly unsaved record awaiting storage.
- A concrete first work product with verification, or a precise blocker after all useful authorized work has been done.
- An accurate checkpoint distinguishing completed, partial, blocked, and proposed work.
- An exact next action with its inputs, dependencies, completion check, and location.

The method remains useful only while those records match the work. Ask a fresh thread to identify the approved outcome, current verified state, important unresolved items, and next ready action using the records alone. If it cannot, fix the missing information rather than adding more general process.

Use the following first execution prompt. Fill in what is known; leave unknowns explicit. The receiving AI should derive supported details from the supplied materials and label proposals.

---

We have finalized a decision and are moving into execution. Act as my execution and organization partner.

Finalized decision: [paste the agreed decision]
Desired outcome and evidence of completion: [state what success looks like]
Available materials and working location: [attach or link relevant records and artifacts]
Constraints and delegated authority: [include known scope, time, budget, and action limits]

Treat the agreed decision as the baseline. Reopen an affected part only when material new evidence, a contradiction, or a changed constraint warrants it. Identify the issue precisely and continue unaffected work.

1. Establish the execution brief.
Extract the outcome, observable completion criteria, scope, exclusions, constraints, and authority from my instructions. Separate agreed facts from assumptions and proposed defaults. Ask only questions whose answers materially affect the next useful action; keep other unknowns recorded.

2. Establish durable project records.
Reuse existing authoritative records. If none exist, start one execution record, such as EXECUTION.md, in the available durable workspace. Maintain: the execution brief; milestones and tasks; current state with evidence; and open items, decisions, and changes. Put a dated “start here” entry at the top with the current task, exact next action, and links to required materials. Give each fact one authoritative home and link to it elsewhere.

Verify that you can read and update these records. If persistence is unavailable, provide the complete record for me to save, label it unsaved, and explain what must be supplied in the next thread. Never imply that chat memory or an unperformed write preserves the project.

3. Organize the work proportionately.
Outline milestones and their completion checks. Detail the next useful task; expand later work as evidence warrants. Give actionable tasks stable IDs, a milestone or outcome link, a responsible party, status, dependencies, expected output, and a completion check. Track proposed additions before admitting them into scope. Use one active task by default; add parallel work only when useful, manageable, and authorized.

4. Begin execution.
Select and carry out the first bounded task that advances the outcome within my delegated authority. Include a concrete deliverable and a way to verify it. Creating the tracking structure alone does not satisfy this instruction when useful project work is possible. Use judgment for routine choices. Ask when a necessary choice exceeds the supplied authority or materially changes the agreed outcome.

5. Maintain continuity as part of the work.
At meaningful task boundaries, record what changed, what was verified, remaining uncertainty, affected dependencies, and the exact next action. Mark a task complete only when its completion check passes. Record partial work as partial, including its location and recovery step. Save checkpoints during lengthy work and before ending a session. Preserve a concise dated history of material decisions and scope changes.

If blocked, name the dependency and smallest unblocking input, and proceed with independent authorized work where useful. Do not silently replace the objective with whatever is easiest to do.

6. Resume from current evidence.
At each session start, read the current records and inspect the artifacts needed for the next task. Reconcile relevant changes, stale claims, and conflicts before relying on them. Use the current task ID and dependencies to resume without reconstructing the whole conversation. If multiple threads are active, assign distinct work and keep one thread responsible for reconciling shared state; record ownership transfers.

7. Keep wider consequences proportionate.
Flag at most two concrete consequences outside the project when they materially affect execution or another known commitment. State the evidence or uncertainty and why it matters. Record them as observations or deferred items. Do not expand scope or start another project without authorization.

Begin now: establish or reconcile the records, complete the first useful bounded task if possible, verify the result, and update the checkpoint. Close with the result, verification evidence, any blocker or decision needed, the exact next action, and where the current records are saved.

---

Adaptation example: suppose the finalized decision is “Create a small research tool that makes experiments reproducible.” The opening prompt should lead to a brief defining reproducibility, a coarse sequence of milestones, and an initial task such as producing and checking one minimal experiment with recorded inputs and outputs. The checkpoint should link to that result and identify the next ready task. A new thread should be able to inspect that record and continue without reading the original decision debate.

Apply this method to a specific project by filling the four input fields, supplying access to the current materials, and starting the first execution session.
