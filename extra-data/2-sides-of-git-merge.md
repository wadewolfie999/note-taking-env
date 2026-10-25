**`git merge` always merges the named branch into the branch you currently have checked out.** Annotation 1

So, while you’re on `main`:

```sh
git merge --no-edit origin/main
```

means **bring `origin/main` into local `main`**.

To reverse the direction, you would create a local branch starting from `origin/main`, then merge your local `main` into it:

```sh
git switch -c remote-main-review origin/main
git merge --no-edit main
```

Here, `remote-main-review` is an example name for a new local branch.

| Command and current branch | Direction |
|---|---|
| On `main`: `git merge origin/main` | Remote history → local `main` |
| On `remote-main-review`: `git merge main` | Local `main` → the branch based on remote history |

Both approaches can combine the same work, but **the branch receiving the merge—and the merge commit’s first parent—differs**. Neither command updates GitHub; that requires a separate `git push`.

For your current task, stay on `main` and use the original merge direction.
