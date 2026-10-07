# Git Merge Conflict Guide

This guide explains how to resolve Git merge conflicts when merging changes from `origin/stage` into the current branch.

---

## 1. Start the Merge

First, check the current branch:

```bash
git branch --show-current
```

Fetch the latest changes from the remote:

```bash
git fetch origin
```

Merge `origin/stage` into the current branch:

```bash
git merge origin/stage
```

If conflicts occur, Git may show something like:

```text
Auto-merging .gitlab-ci.yml
CONFLICT (content): Merge conflict in .gitlab-ci.yml
Auto-merging Dockerfile
CONFLICT (content): Merge conflict in Dockerfile
Auto-merging docker-compose.yml
CONFLICT (add/add): Merge conflict in docker-compose.yml

Automatic merge failed; fix conflicts and then commit the result.
```

**Do not run `git merge` again.**

The current merge must be resolved first.

---

## 2. Check the Conflicts

Check the repository status:

```bash
git status
```

Show only the files that have unresolved conflicts:

```bash
git diff --name-only --diff-filter=U
```

Show the conflict details:

```bash
git diff
```

For a specific file:

```bash
git diff -- Dockerfile
```

---

## 3. Understanding `ours` and `theirs`

When running:

```bash
git merge origin/stage
```

Git considers:

| Option   | Meaning            |
| -------- | ------------------ |
| `ours`   | The current branch |
| `theirs` | `origin/stage`     |

For example:

```bash
git checkout --ours Dockerfile
```

means:

> Keep the `Dockerfile` from the current branch.

While:

```bash
git checkout --theirs Dockerfile
```

means:

> Keep the `Dockerfile` from `origin/stage`.

---

# 4. Prefer the Current Branch

If the current branch should take priority for the conflicted files, use `ours`.

For example, if these files have conflicts:

```text
.gitlab-ci.yml
Dockerfile
docker-compose.yml
```

Run:

```bash
git checkout --ours .gitlab-ci.yml
git checkout --ours Dockerfile
git checkout --ours docker-compose.yml
```

You can also apply `ours` to all currently conflicted files:

```bash
git checkout --ours .
```

---

## 5. Verify the Files

Before committing, check the repository:

```bash
git status
```

Check for whitespace errors:

```bash
git diff --check
```

You can also check for remaining conflict markers:

```bash
grep -R -n -E '^(<<<<<<<|=======|>>>>>>>)' \
  .gitlab-ci.yml Dockerfile docker-compose.yml
```

If there is no output, there are no conflict markers in those files.

---

## 6. Mark Conflicts as Resolved

After selecting the current branch's version:

```bash
git add .gitlab-ci.yml Dockerfile docker-compose.yml
```

Or add all resolved files:

```bash
git add .
```

Check the status again:

```bash
git status
```

---

## 7. Complete the Merge

Complete the merge with:

```bash
git commit
```

Git will usually create a merge commit with a message similar to:

```text
Merge remote-tracking branch 'origin/stage'
```

Finally:

```bash
git status
```

The expected result is:

```text
nothing to commit, working tree clean
```

---

# 8. Prefer `origin/stage`

If the `origin/stage` version should take priority instead, use `theirs`:

```bash
git checkout --theirs .gitlab-ci.yml
git checkout --theirs Dockerfile
git checkout --theirs docker-compose.yml
```

Or for all conflicted files:

```bash
git checkout --theirs .
```

Then:

```bash
git add .
git commit
```

---

# 9. Manually Resolve a Conflict

Sometimes neither side should completely replace the other.

Git may add conflict markers to the file:

```text
<<<<<<< HEAD

Changes from the current branch

=======

Changes from origin/stage

>>>>>>> origin/stage
```

Edit the file and create the correct final version.

Remove the conflict markers:

```text
<<<<<<< HEAD
=======
>>>>>>> origin/stage
```

Then:

```bash
git add <file>
git commit
```

---

# 10. Important: `ours` Replaces the Entire File

Be careful when using:

```bash
git checkout --ours Dockerfile
```

This selects the **entire version of the file from the current branch**.

It does not only select one side of the conflicting lines.

The same applies to:

```bash
git checkout --theirs Dockerfile
```

Therefore, for important configuration files such as:

```text
.gitlab-ci.yml
Dockerfile
docker-compose.yml
```

you should manually review the changes if both branches contain important modifications.

---

# 11. Abort the Merge

If you decide that the merge should not continue and the merge has not been committed yet:

```bash
git merge --abort
```

Then check:

```bash
git status
```

This returns the repository to the state it was in before the merge started.

---

# 12. Recommended Workflow

### Fetch the latest changes

```bash
git fetch origin
```

### Merge stage

```bash
git merge origin/stage
```

### Check conflicts

```bash
git status
```

```bash
git diff --name-only --diff-filter=U
```

### If the current branch should take priority

```bash
git checkout --ours .
```

### Verify

```bash
git diff --check
```

### Mark as resolved

```bash
git add .
```

### Complete the merge

```bash
git commit
```

### Verify the result

```bash
git status
```

```bash
git log --oneline --graph -10
```

---

# Quick Reference

## Keep the Current Branch

```bash
git checkout --ours .
git add .
git commit
```

## Keep `origin/stage`

```bash
git checkout --theirs .
git add .
git commit
```

## Show Conflict Files

```bash
git status
```

```bash
git diff --name-only --diff-filter=U
```

## Check Conflict Details

```bash
git diff
```

## Abort the Merge

```bash
git merge --abort
```

## Check for Conflict Markers

```bash
grep -R -n -E '^(<<<<<<<|=======|>>>>>>>)' .
```

---

# Example: Current `user-mng` Merge

Suppose we are currently on the `user-mng` branch and run:

```bash
git merge origin/stage
```

Git reports:

```text
CONFLICT (content): Merge conflict in .gitlab-ci.yml
CONFLICT (content): Merge conflict in Dockerfile
CONFLICT (add/add): Merge conflict in docker-compose.yml
```

If the **current `user-mng` branch should take priority** for these files:

```bash
git checkout --ours .gitlab-ci.yml
git checkout --ours Dockerfile
git checkout --ours docker-compose.yml

git diff --check

git add .gitlab-ci.yml Dockerfile docker-compose.yml

git commit

git status
```

This completes the merge while keeping the current branch's versions of the conflicted files.

Changes from `origin/stage` in files that did **not** have conflicts are still merged normally.

---

# Summary

| Goal                 | Command                                |
| -------------------- | -------------------------------------- |
| Keep current branch  | `git checkout --ours <file>`           |
| Keep `origin/stage`  | `git checkout --theirs <file>`         |
| See conflicts        | `git status`                           |
| See unresolved files | `git diff --name-only --diff-filter=U` |
| Mark resolved        | `git add <file>`                       |
| Complete merge       | `git commit`                           |
| Cancel merge         | `git merge --abort`                    |

**Always review important configuration files before committing a merge, especially CI/CD, Docker, and deployment configuration files.**
