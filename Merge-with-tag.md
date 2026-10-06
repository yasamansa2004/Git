# Stage Release

## Merge `staging` into `stage` and Create Release Tag

This workflow merges the latest `staging` branch into `stage`, pushes the updated `stage` branch, and creates the release tag `stage-0.0.4`.

### Commands

```bash
# Fetch latest branches and tags
git fetch origin --prune --tags

# Switch to stage
git checkout stage

# Update local stage
git pull --ff-only origin stage

# Merge staging into stage
git merge origin/staging

# Push updated stage
git push origin stage

# Create release tag
git tag -a stage-0.0.4 -m "Release stage-0.0.4"

# Push release tag
git push origin stage-0.0.4
```

## Workflow

```text
staging
   │
   │ merge
   ▼
 stage
   │
   │ tag
   ▼
stage-0.0.4
```

## Release

**Branch:** `stage`

**Source:** `staging`

**Tag:** `stage-0.0.4`

**Tag message:** `Release stage-0.0.4`
