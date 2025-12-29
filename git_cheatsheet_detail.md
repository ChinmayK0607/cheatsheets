# Git Cheatsheet

Quick reference for everyday Git + GitHub work.

## Setup & Auth
- Install Git: `sudo apt install git` (Linux); `brew install git` (macOS).
- Configure identity: `git config --global user.name "Your Name"`; `git config --global user.email "you@example.com"`.
- Set editor (optional): `git config --global core.editor "code --wait"` or `nano`.
- Cache credentials for HTTPS: `git config --global credential.helper store` (plain text) or use a manager (macOS Keychain/Windows Manager).
- GitHub CLI login (nice UX): `gh auth login` (choose HTTPS + browser auth or token).
- SSH setup (optional): `ssh-keygen -t ed25519 -C "you@example.com"` → add `~/.ssh/id_ed25519.pub` to GitHub → test with `ssh -T git@github.com`.

## Getting Code
- Clone: `git clone <url>` (HTTPS or SSH).
- Add remote to existing repo: `git remote add origin <url>`; view remotes `git remote -v`.
- Fetch latest without merging: `git fetch` (all) or `git fetch origin main`.

## Everyday Workflow
- Status: `git status`.
- See pending changes: `git diff` (working tree), `git diff --staged`.
- Stage changes: `git add <file>`; stage all tracked: `git add -u`; unstage: `git restore --staged <file>`.
- Commit: `git commit -m "message"`; amend (replace last commit): `git commit --amend` (avoid if already pushed).
- Push: `git push origin <branch>`; set upstream first time: `git push -u origin <branch>`.
- Pull (fetch + merge): `git pull`; with rebase (cleaner history): `git pull --rebase`.
- Stash work-in-progress: `git stash push -m "note"`; list: `git stash list`; apply+drop latest: `git stash pop`; drop without apply: `git stash drop`.

## Branches
- List: `git branch` (local); `git branch -r` (remote).
- Create/switch: `git switch -c feature/foo` (new); `git switch feature/foo` (existing).
- Delete: `git branch -d feature/foo` (safe); force: `git branch -D feature/foo`.
- Rename current: `git branch -m new-name`.

## Inspecting History
- Log (compact): `git log --oneline --decorate --graph --all`.
- Log with patch: `git log -p`.
- Filter by file: `git log -- <file>`.
- Show commit details: `git show <commit>`; specific file then: `git show <commit>:path/to/file`.
- Blame: `git blame <file>`; ignore moves: `git blame -M -C <file>`.
- Search history for text: `git log -S "needle"` or `git log -G "regex"`.

## Diffs
- Working tree vs HEAD: `git diff`.
- Staged vs HEAD: `git diff --staged`.
- Between commits/branches: `git diff <commit1> <commit2>` or `git diff main..feature`.
- Word diff: `git diff --word-diff`.
- File statistics: `git diff --stat`.

## Time-Travel & Recovery
- Check out a commit (detached HEAD): `git switch --detach <commit>`; return to branch: `git switch <branch>`.
- Create branch from old commit: `git switch -c hotfix <commit>`.
- Restore file from commit: `git restore --source <commit> -- <file>`.
- Reflog (undo lost pointers): `git reflog` then `git switch <sha>` to recover.
- Soft reset (keep changes staged): `git reset --soft <commit>`; mixed (default, unstages): `git reset <commit>`; hard (discard work—danger): `git reset --hard <commit>`.

## Rebasing (what & how)
- What: Replay commits onto a new base to create a linear history.
- Update feature onto latest main: `git switch feature` → `git fetch origin` → `git rebase origin/main`.
- Resolve conflicts, then continue: fix files → `git add <file>` → `git rebase --continue`; abort: `git rebase --abort`.
- Interactive cleanup (squash/reorder/edit): `git rebase -i <base>` (e.g., `git rebase -i origin/main`), then choose actions (`pick`, `squash`, `edit`, `drop`).
- After rebasing pushed commits, force-push with lease: `git push --force-with-lease`.

## Merging
- Standard merge: `git switch feature` → `git merge main`.
- No-fast-forward (keeps merge commit): `git merge --no-ff main`.
- Abort merge on conflict: `git merge --abort`.

## Cherry-Pick & Backporting
- Apply specific commit onto current branch: `git cherry-pick <commit>`.
- Multiple: `git cherry-pick A B C` or range: `git cherry-pick A^..C`.

## Tags & Releases
- List tags: `git tag`.
- Create annotated tag: `git tag -a v1.2.0 -m "Release v1.2.0" <commit>`.
- Push tags: `git push origin --tags` (all) or `git push origin v1.2.0`.

## Cleaning Up
- Remove untracked files/dirs (careful): `git clean -fd`.
- Prune remote-tracking branches gone on server: `git fetch --prune`.

## Remotes & Sync
- Change remote URL: `git remote set-url origin <new-url>`.
- See remote branches locally: `git branch -vv`.
- Track a remote branch explicitly: `git switch --track origin/<branch>`.

## Common Patterns
- Quick status + graph: `git status -sb` and `git log --oneline --graph --decorate -5`.
- Review before pushing: `git diff --stat origin/<branch>..` and `git diff origin/<branch>..`.
- Last commit summary: `git show --stat --summary`.
- Who changed a line: `git blame -L <start>,<end> <file>`.

## Safety Tips
- Avoid `git reset --hard` unless you are sure; stash or commit first.
- Prefer `--force-with-lease` over `--force`.
- After conflicts: rerun tests before push.
- Keep `main` clean: do feature work on branches.
