---
name: yolo
description: Verify completed work, commit it, and push it directly to the remote default branch without creating a branch, worktree, or pull request. Use only when the user explicitly invokes `$yolo`, says "yolo," or explicitly asks to commit and push directly to main.
---

# Yolo: publish directly to the default branch

Finish with the intended change committed and pushed directly to the remote
default branch. Invoking `$yolo` authorizes verification, an appropriate commit,
and a normal push. It does not authorize force-pushing, history rewriting,
merging remote changes, or including unrelated work.

Never create a branch, worktree, or pull request during this workflow.

## 1. Check the repository state

Read the repository instructions. Inspect the full working tree, staged diff,
unstaged diff, and recent commits. Preserve unrelated changes and stage only
explicit paths.

Identify the push remote and its default branch from Git configuration. Use the
repository's configured remote when clear, otherwise use `origin`. Stop when the
remote or default branch is ambiguous.

Confirm that `HEAD` is attached to the default branch. Do not switch branches
automatically. Fetch the remote default branch and confirm its current tip is an
ancestor of `HEAD`. Stop if the remote is ahead or the histories diverged. Do
not pull, merge, rebase, reset, or force-push to resolve this condition.

## 2. Verify the work

Run the repository's applicable formatter, linter, tests, and build. Start with
the cheapest relevant checks. Fix failures caused by the intended change and
run the checks again.

If the repository has no automated checks, inspect the resulting artifact or
behavior directly and report that no automated checks were available.

## 3. Commit the intended change

Review the complete diff before staging. If unrelated staged changes cannot be
separated safely, stop and ask the user how to proceed.

Stage explicit paths and create one logical commit. Follow the active commit
message and attribution instructions. If the intended change is already
committed, do not create an empty commit.

## 4. Push normally

Push `HEAD` to the remote default branch without any force option. If Git
rejects the push, stop and report the rejection. Do not weaken repository
protections or change remote history.

## 5. Report

Confirm the remote, branch, commit, push result, and checks performed. Report
any verification that could not run.
