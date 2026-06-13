# Repository Migration Guide

This document explains how to migrate the repository from the old Git server to the new GitLab instance, including authentication setup, remote update, and pushing all branches and commits.

---

## 1. Clone Source Code

Clone the repository from the old Git server:

```bash
git clone http://192.168.7.152/matin/BeroozIran/backend/cash.git
```

Move into the project directory:

```bash
cd cash
```

---

## 2. Configure Git Authentication

To avoid entering your username and password repeatedly, configure Git credential storage:

```bash
git config --global credential.helper store
```

This will save your credentials locally after the first successful authentication.

---

## 3. Change Remote Repository URL

Update the Git remote URL to point to the new GitLab repository:

```bash
git remote set-url origin https://git.mtyn.ir/berooziran/backend/mono/cash.git
```

Verify the new remote URL:

```bash
git remote -v
```

---

## 4. Check Available Branches

List all local and remote branches:

```bash
git branch -a
```

---

## 5. Push All Existing Branches

Push all local branches to the new repository:

```bash
git push origin --all
```

---

## 6. Move Every Branch and Commit

To migrate all remote branches and preserve their commit history, run:

```bash
for branch in $(git branch -r | grep -v 'HEAD' | sed 's|origin/||'); do
    git push origin "refs/remotes/origin/$branch:refs/heads/$branch"
done
```

This command will:

* Iterate through all remote branches
* Exclude the `HEAD` reference
* Push every branch to the new GitLab repository
* Preserve all commits and branch history

---

## 7. Push Tags (Optional)

If your repository contains tags, push them as well:

```bash
git push origin --tags
```

---

## Notes

* On the first push, Git will ask for your credentials.
* Since `credential.helper store` is enabled, your credentials will be saved for future operations.
* Ensure you have permission to access the target repository before pushing.

Repository destination:

`https://git.mtyn.ir/berooziran/backend/mono/cash.git`
