---
title: Git Commands Cheat Sheet
date: 2016-02-25 00:17:21
tags: [git, cli]
category: ["git"]
---

This guide contains common Git commands and practical examples for day-to-day development.

<!--more-->

---

## 1. Check Git version

```bash
git --version
```

---

## 2. Configure Git identity

Set your global username:

```bash
git config --global user.name "Your Name"
```

Set your global email:

```bash
git config --global user.email "you@example.com"
```

Check configuration:

```bash
git config --list
```

Check a specific value:

```bash
git config user.name
git config user.email
```

---

## 3. Initialize a repository

```bash
git init
```

Initialize a repository in a specific directory:

```bash
git init my-project
```

---

## 4. Clone a repository

HTTPS:

```bash
git clone https://github.com/user/repository.git
```

SSH:

```bash
git clone git@github.com:user/repository.git
```

Clone into a custom directory:

```bash
git clone git@github.com:user/repository.git my-folder
```

---

## 5. Check repository status

```bash
git status
```

Short output:

```bash
git status -s
```

---

## 6. View changes

Show unstaged changes:

```bash
git diff
```

Show staged changes:

```bash
git diff --staged
```

Show changes in a specific file:

```bash
git diff path/to/file
```

---

## 7. Stage files

Stage one file:

```bash
git add file.txt
```

Stage multiple files:

```bash
git add file1.txt file2.txt
```

Stage all changes:

```bash
git add .
```

Stage all tracked and untracked changes:

```bash
git add -A
```

---

## 8. Unstage files

Unstage one file:

```bash
git restore --staged file.txt
```

Unstage everything:

```bash
git restore --staged .
```

Older syntax:

```bash
git reset HEAD file.txt
```

---

## 9. Commit changes

```bash
git commit -m "Add feature"
```

Commit tracked file changes without running `git add` manually:

```bash
git commit -am "Fix bug"
```

> `-a` does not include new untracked files.

---

## 10. Amend the last commit

Change the last commit message:

```bash
git commit --amend -m "New commit message"
```

Add forgotten staged changes to the last commit:

```bash
git add forgotten-file.txt
git commit --amend --no-edit
```

> Avoid amending commits that have already been shared unless you understand the consequences of rewriting history.

---

## 11. View commit history

```bash
git log
```

Compact format:

```bash
git log --oneline
```

Graph view:

```bash
git log --oneline --graph --decorate --all
```

Show a limited number of commits:

```bash
git log -10 --oneline
```

Show commits for a file:

```bash
git log -- path/to/file
```

---

## 12. Show a commit

```bash
git show COMMIT_HASH
```

Show the latest commit:

```bash
git show HEAD
```

---

## 13. List branches

Local branches:

```bash
git branch
```

Remote branches:

```bash
git branch -r
```

All branches:

```bash
git branch -a
```

---

## 14. Create a branch

```bash
git branch feature/my-feature
```

Create and switch:

```bash
git switch -c feature/my-feature
```

Older syntax:

```bash
git checkout -b feature/my-feature
```

---

## 15. Switch branches

```bash
git switch main
```

Older syntax:

```bash
git checkout main
```

Switch back to the previous branch:

```bash
git switch -
```

---

## 16. Rename a branch

Rename the current branch:

```bash
git branch -m new-name
```

Rename another local branch:

```bash
git branch -m old-name new-name
```

---

## 17. Delete a branch

Delete a merged local branch:

```bash
git branch -d feature/my-feature
```

Force-delete a local branch:

```bash
git branch -D feature/my-feature
```

Delete a remote branch:

```bash
git push origin --delete feature/my-feature
```

---

## 18. Merge a branch

Switch to the target branch:

```bash
git switch main
```

Merge another branch:

```bash
git merge feature/my-feature
```

---

## 19. Abort a merge

If a merge has conflicts and you want to cancel it:

```bash
git merge --abort
```

---

## 20. Rebase a branch

Rebase the current branch onto `main`:

```bash
git rebase main
```

Typical workflow:

```bash
git switch feature/my-feature
git fetch origin
git rebase origin/main
```

---

## 21. Continue or abort a rebase

After resolving conflicts:

```bash
git add .
git rebase --continue
```

Abort:

```bash
git rebase --abort
```

Skip the current commit:

```bash
git rebase --skip
```

---

## 22. Interactive rebase

Edit recent commits:

```bash
git rebase -i HEAD~3
```

Common actions:

```text
pick
reword
edit
squash
fixup
drop
```

---

## 23. Pull changes

```bash
git pull
```

Pull from a specific remote branch:

```bash
git pull origin main
```

Pull using rebase:

```bash
git pull --rebase
```

---

## 24. Fetch changes

Fetch without modifying your working branch:

```bash
git fetch
```

Fetch all remotes:

```bash
git fetch --all
```

Remove references to deleted remote branches:

```bash
git fetch --prune
```

---

## 25. Push changes

Push the current branch:

```bash
git push
```

Push and set upstream:

```bash
git push -u origin feature/my-feature
```

After that:

```bash
git push
```

is enough.

---

## 26. Force push safely

After rewriting branch history:

```bash
git push --force-with-lease
```

Prefer this over:

```bash
git push --force
```

`--force-with-lease` helps prevent accidentally overwriting remote changes made by someone else.

---

## 27. List remotes

```bash
git remote -v
```

---

## 28. Add a remote

```bash
git remote add origin git@github.com:user/repository.git
```

---

## 29. Change remote URL

```bash
git remote set-url origin git@github.com:user/repository.git
```

Verify:

```bash
git remote -v
```

---

## 30. Remove a remote

```bash
git remote remove origin
```

---

## 31. Show remote information

```bash
git remote show origin
```

---

## 32. Stash changes

Temporarily save working changes:

```bash
git stash
```

With a message:

```bash
git stash push -m "Work in progress"
```

Include untracked files:

```bash
git stash -u
```

---

## 33. List stashes

```bash
git stash list
```

---

## 34. Restore stashed changes

Apply the latest stash and keep it:

```bash
git stash apply
```

Apply and remove it from the stash list:

```bash
git stash pop
```

Apply a specific stash:

```bash
git stash apply stash@{1}
```

---

## 35. Delete stashes

Delete one stash:

```bash
git stash drop stash@{0}
```

Delete all stashes:

```bash
git stash clear
```

---

## 36. Discard local file changes

Restore one file:

```bash
git restore file.txt
```

Restore all tracked files:

```bash
git restore .
```

> This discards unstaged changes.

---

## 37. Restore a file from another branch

```bash
git restore --source main path/to/file
```

From a commit:

```bash
git restore --source COMMIT_HASH path/to/file
```

---

## 38. Reset commits

### Soft reset

Move HEAD but keep changes staged:

```bash
git reset --soft HEAD~1
```

### Mixed reset

Move HEAD and keep changes unstaged:

```bash
git reset HEAD~1
```

or:

```bash
git reset --mixed HEAD~1
```

### Hard reset

Move HEAD and discard changes:

```bash
git reset --hard HEAD~1
```

> Be very careful with `--hard`.

---

## 39. Reset local branch to remote

Fetch first:

```bash
git fetch origin
```

Then:

```bash
git reset --hard origin/main
```

This makes the current branch match the remote branch exactly.

---

## 40. Revert a commit

Create a new commit that reverses another commit:

```bash
git revert COMMIT_HASH
```

Revert the latest commit:

```bash
git revert HEAD
```

This is safer than reset for commits already pushed to a shared branch.

---

## 41. Cherry-pick a commit

Apply a specific commit to the current branch:

```bash
git cherry-pick COMMIT_HASH
```

Abort after conflicts:

```bash
git cherry-pick --abort
```

Continue after resolving conflicts:

```bash
git add .
git cherry-pick --continue
```

---

## 42. Tag a commit

Create a lightweight tag:

```bash
git tag v1.0.0
```

Create an annotated tag:

```bash
git tag -a v1.0.0 -m "Release v1.0.0"
```

Tag a specific commit:

```bash
git tag -a v1.0.0 COMMIT_HASH -m "Release v1.0.0"
```

---

## 43. Push tags

Push one tag:

```bash
git push origin v1.0.0
```

Push all tags:

```bash
git push origin --tags
```

---

## 44. Delete tags

Delete locally:

```bash
git tag -d v1.0.0
```

Delete remotely:

```bash
git push origin --delete v1.0.0
```

---

## 45. Ignore files

Create or edit:

```text
.gitignore
```

Example:

```text
node_modules/
bin/
obj/
.env
*.log
.vscode/
.idea/
```

If a file is already tracked, adding it to `.gitignore` is not enough.

Stop tracking it:

```bash
git rm --cached file.txt
```

For a directory:

```bash
git rm -r --cached directory/
```

---

## 46. Clean untracked files

Preview what would be deleted:

```bash
git clean -n
```

Delete untracked files:

```bash
git clean -f
```

Delete untracked directories too:

```bash
git clean -fd
```

> Always run `git clean -n` first.

---

## 47. Find who changed a line

```bash
git blame file.txt
```

Specific line range:

```bash
git blame -L 10,20 file.txt
```

---

## 48. Search commit messages

```bash
git log --grep="keyword"
```

---

## 49. Search code history

Find commits that added or removed a specific string:

```bash
git log -S "SomeText"
```

Show patches too:

```bash
git log -S "SomeText" -p
```

---

## 50. Compare branches

```bash
git diff main..feature/my-feature
```

Compare commits:

```bash
git diff COMMIT_A COMMIT_B
```

---

## 51. Check which branches contain a commit

```bash
git branch --contains COMMIT_HASH
```

Remote branches too:

```bash
git branch -a --contains COMMIT_HASH
```

---

## 52. Reflog — recover lost commits

Git reflog records movements of HEAD:

```bash
git reflog
```

Example:

```text
abc1234 HEAD@{0}: reset: moving to HEAD~1
def5678 HEAD@{1}: commit: Important work
```

Recover a lost commit by creating a branch:

```bash
git branch recovered-work def5678
```

Or reset back to it:

```bash
git reset --hard def5678
```

`git reflog` is extremely useful after accidental reset, rebase, or branch deletion.

---

## 53. Remove the last local commit but keep changes

Keep changes staged:

```bash
git reset --soft HEAD~1
```

Keep changes unstaged:

```bash
git reset HEAD~1
```

---

## 54. Undo the last pushed commit safely

If the commit is already shared:

```bash
git revert HEAD
git push
```

Prefer this over rewriting shared history.

---

## 55. Update a feature branch with latest main

Using rebase:

```bash
git switch feature/my-feature
git fetch origin
git rebase origin/main
```

Then, if the branch was already pushed:

```bash
git push --force-with-lease
```

Alternative using merge:

```bash
git switch feature/my-feature
git fetch origin
git merge origin/main
```

---

## 56. Squash recent commits

For the last 3 commits:

```bash
git rebase -i HEAD~3
```

Example editor:

```text
pick   abc123 First change
squash def456 Second change
squash ghi789 Third change
```

Save and edit the final commit message.

---

## 57. Rename the default branch

Rename locally:

```bash
git branch -m master main
```

Push:

```bash
git push -u origin main
```

Delete the old remote branch after changing the repository's default branch:

```bash
git push origin --delete master
```

---

## 58. Show tracked files

```bash
git ls-files
```

---

## 59. Show current branch

```bash
git branch --show-current
```

---

## 60. Show repository root

```bash
git rev-parse --show-toplevel
```

---

## 61. Show current commit hash

Full hash:

```bash
git rev-parse HEAD
```

Short hash:

```bash
git rev-parse --short HEAD
```

---

## 62. Create an empty commit

Useful for testing CI/CD pipelines:

```bash
git commit --allow-empty -m "Trigger pipeline"
```

Push it:

```bash
git push
```

---

## 63. Common workflow

Create a feature branch:

```bash
git switch main
git pull
git switch -c feature/my-feature
```

Work and commit:

```bash
git status
git add .
git commit -m "Implement feature"
```

Push:

```bash
git push -u origin feature/my-feature
```

Update with latest main:

```bash
git fetch origin
git rebase origin/main
```

Push rewritten history if needed:

```bash
git push --force-with-lease
```

---

## 64. Common recovery workflow

Check what happened:

```bash
git status
git log --oneline --graph --decorate -20
git reflog
```

If you accidentally changed a file:

```bash
git restore file.txt
```

If you accidentally staged a file:

```bash
git restore --staged file.txt
```

If you need to undo a local commit but keep the work:

```bash
git reset --soft HEAD~1
```

If a shared commit must be undone:

```bash
git revert COMMIT_HASH
```

If a commit seems lost:

```bash
git reflog
```

---

## 65. Useful aliases

Create a compact log alias:

```bash
git config --global alias.lg "log --oneline --graph --decorate --all"
```

Then use:

```bash
git lg
```

Status shortcut:

```bash
git config --global alias.st status
```

Branch shortcut:

```bash
git config --global alias.br branch
```

Commit shortcut:

```bash
git config --global alias.cm commit
```

---

## Useful daily commands

```bash
git status
git diff
git add .
git commit -m "Message"
git log --oneline --graph --decorate --all
git switch branch-name
git switch -c new-branch
git fetch origin
git pull --rebase
git push
git stash
git stash pop
git rebase origin/main
git reflog
```

---

## Safety recommendations

- Use `git status` before destructive operations.
- Prefer `git revert` for commits already shared with others.
- Prefer `git push --force-with-lease` over `git push --force`.
- Run `git clean -n` before `git clean -f`.
- Be careful with `git reset --hard`.
- Use `git reflog` when you think a commit has been lost.
- Never commit passwords, private keys, API tokens, or production `.env` files.
