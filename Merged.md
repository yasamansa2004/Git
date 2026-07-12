# Git Branch Merge Guide

This document describes the standard process for merging one branch into another.

## Merge Flow

Replace the placeholders below with your branch names:

- `<source-branch>`: The branch containing the changes.
- `<target-branch>`: The branch that will receive the changes.

Example:

- `feature/login` → `develop`
- `develop` → `stage`
- `stage` → `main`
- `hotfix/payment` → `main`

---

## 1. Fetch the Latest Changes

```bash
git fetch origin
```

---

## 2. Checkout the Target Branch

```bash
git checkout <target-branch>
```

Example:

```bash
git checkout stage
```

---

## 3. Update the Target Branch

```bash
git pull origin <target-branch>
```

---

## 4. Merge the Source Branch

```bash
git merge origin/<source-branch>
```

Example:

```bash
git merge origin/develop
```

If merge conflicts occur:

1. Resolve the conflicts.
2. Stage the resolved files.

```bash
git add .
```

3. Complete the merge.

```bash
git commit
```

---

## 5. Push the Target Branch

```bash
git push origin <target-branch>
```

---

# Deployment Tags (Optional)

If your CI/CD pipeline is triggered by tags, create a deployment tag after the merge.

Example:

```bash
git tag <target-branch>-1.0.0
git push origin <target-branch>-1.0.0
```

Examples:

```bash
git tag stage-1.5.0
git push origin stage-1.5.0
```

```bash
git tag production-2.3.1
git push origin production-2.3.1
```

---

# Merge Using Pull/Merge Request

If the target branch is protected:

1. Create a Pull Request / Merge Request.
2. Source branch: `<source-branch>`
3. Target branch: `<target-branch>`
4. Review and merge.
5. Create the deployment tag (if required).

---

# Verify the Merge

Latest commit:

```bash
git log -1
```

Graph view:

```bash
git log --graph --oneline --decorate --all
```

List tags:

```bash
git tag
```

---

# Complete Workflow

```bash
git fetch origin

git checkout <target-branch>
git pull origin <target-branch>

git merge origin/<source-branch>

git push origin <target-branch>
```

If deployment uses tags:

```bash
git tag <target-branch>-<version>
git push origin <target-branch>-<version>
```

---

# Examples

### Feature → Develop

```bash
git checkout develop
git pull origin develop
git merge origin/feature/login
git push origin develop
```

### Develop → Stage

```bash
git checkout stage
git pull origin stage
git merge origin/develop
git push origin stage
```

### Stage → Main

```bash
git checkout main
git pull origin main
git merge origin/stage
git push origin main
```

### Hotfix → Main

```bash
git checkout main
git pull origin main
git merge origin/hotfix/payment
git push origin main
```

---

## Notes

- Always update the target branch before merging.
- Resolve merge conflicts before pushing.
- Verify your changes before creating deployment tags.
- Follow your project's branching strategy and CI/CD requirements.

### For Example

# Merging `develop` into `stage`

This guide explains how to merge the `develop` branch into the `stage` branch and create a deployment tag following the `stage-*` naming convention.

---

## Prerequisites

- Git is installed.
- You have permission to push to the `stage` branch.
- Your local repository is up to date.

---

## 1. Fetch the Latest Changes

```bash
git fetch origin
```

---

## 2. Switch to the `stage` Branch

```bash
git checkout stage
```

---

## 3. Update the Local `stage` Branch

```bash
git pull origin stage
```

---

## 4. Merge `develop` into `stage`

```bash
git merge origin/develop
```

If there are merge conflicts:

1. Resolve the conflicts.
2. Stage the resolved files.

```bash
git add .
```

3. Complete the merge.

```bash
git commit
```

---

## 5. Push the Updated `stage` Branch

```bash
git push origin stage
```

---

## 6. Create a Stage Tag

If your deployment pipeline is triggered by tags matching `stage-*`, create a new tag.

Example:

```bash
git tag stage-1.0.0
git push origin stage-1.0.0
```

or with a date-based version:

```bash
git tag stage-2026.07.12
git push origin stage-2026.07.12
```

---

# Alternative: Using GitLab Merge Request

If the `stage` branch is protected:

1. Create a Merge Request.
2. Source branch: `develop`
3. Target branch: `stage`
4. Review and merge the changes.
5. After the merge, create and push the deployment tag.

```bash
git checkout stage
git pull origin stage

git tag stage-1.0.0
git push origin stage-1.0.0
```

---

# Verify the Merge

View the merge history:

```bash
git log --graph --oneline --decorate --all
```

Verify the latest commit:

```bash
git log -1
```

List all tags:

```bash
git tag
```

---

# Complete Workflow

```bash
git fetch origin

git checkout stage
git pull origin stage

git merge origin/develop

git push origin stage

git tag stage-1.0.0
git push origin stage-1.0.0
```

---

# Notes

- Ensure your working directory is clean before merging.
- Resolve all merge conflicts before pushing.
- Replace `stage-1.0.0` with your project's versioning scheme.
- If your CI/CD pipeline is configured to deploy on `stage-*` tags, deployment will begin automatically after the tag is pushed.
