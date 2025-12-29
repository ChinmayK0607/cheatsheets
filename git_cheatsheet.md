# Git Cheatsheet

| Function | Command | Detailed Explanation & Example |
| --- | --- | --- |
| Set your identity | `git config --global user.name "Jane Doe"`<br>`git config --global user.email "you@example.com"` | Required so commits record who made them. Run once per machine. Verify with `git config --global --get user.name`. |
| Login to GitHub (HTTPS) | `gh auth login` | Guides you through browser-based login or token entry. After login, pushes over HTTPS use cached credentials. |
| SSH key setup | `ssh-keygen -t ed25519 -C "you@example.com"` | Creates a key pair in `~/.ssh`. Add the `.pub` key to GitHub → “SSH and GPG keys”, then test `ssh -T git@github.com`. |
| Clone a repo | `git clone https://github.com/org/repo.git` | Copies the repository locally. Example: `git clone git@github.com:octocat/Hello-World.git` (SSH). |
| Check status | `git status -sb` | Shows branch, staged/unstaged changes in short form. Example output: `## feature/login M app.py ?? notes.txt`. |
| View changes (working tree) | `git diff` | Compares working tree to HEAD. Example: `git diff src/app.py` to see edits before staging. |
| View staged changes | `git diff --staged` | Shows what will be committed. Use after staging to double-check. |
| Stage files | `git add file1.py file2.py` | Adds changes to the next commit. Stage everything modified: `git add -u`. Stage all including new files: `git add .`. |
| Unstage a file | `git restore --staged file1.py` | Removes file from the index without altering working copy. |
| Commit | `git commit -m "Add login validation"` | Records staged changes as a new commit. |
| Amend last commit | `git commit --amend` | Reopens the last commit to edit message or include more staged changes. Avoid after pushing unless you’ll force-push. |
| Push to remote | `git push -u origin feature/login` | Sends commits to remote and sets upstream so future `git push`/`git pull` work without args. |
| Pull with rebase | `git pull --rebase` | Fetches then rebases your local commits on top of remote for a linear history. Example: staying current with `origin/main`. |
| Switch/create branch | `git switch feature/login`<br>`git switch -c feature/login` | Moves HEAD to an existing branch or creates a new one with `-c`. |
| Delete branch | `git branch -d feature/login` | Deletes local branch if merged; force delete with `-D`. |
| Log (graph) | `git log --oneline --graph --decorate --all` | Compact history with branch pointers. Example: see how feature branches diverge. |
| Show a commit | `git show abc1234` | Displays patch, author, and message for commit `abc1234`. |
| Show file at commit | `git show abc1234:path/to/file.py` | Prints file contents as of a specific commit (great for bisecting regressions). |
| Blame a file | `git blame -L 10,30 src/app.py` | Shows who last changed lines 10–30. Add `-M -C` to follow moved/copied lines. |
| Diff two refs | `git diff main..feature/login` | Shows changes needed to turn `main` into `feature/login`. Add `--stat` for summary or `--word-diff` for inline changes. |
| Stash WIP | `git stash push -m "wip: auth flow"` | Saves uncommitted work and cleans the working tree. Restore later with `git stash pop`. List with `git stash list`. |
| Rebase onto main | `git switch feature/login`<br>`git fetch origin`<br>`git rebase origin/main` | Replays your branch commits on top of the latest `main`. Resolve conflicts → `git add <file>` → `git rebase --continue`. Abort with `git rebase --abort`. |
| Interactive rebase (cleanup) | `git rebase -i origin/main` | Opens an editor to reorder, squash, drop, or edit commits before landing a branch. Use `pick`/`squash`/`reword` etc. Finish with `--continue`. |
| Force-push safely after rebase | `git push --force-with-lease` | Updates remote while ensuring you don’t overwrite others’ new commits (safer than `--force`). |
| Merge main into branch | `git switch feature/login`<br>`git merge main` | Brings `main` changes into your branch, creating a merge commit if needed. Abort on conflict with `git merge --abort`. |
| Cherry-pick commits | `git cherry-pick abc1234 def5678` | Applies specific commits onto the current branch—useful for backports or picking fixes without full merges. |
| Tag a release | `git tag -a v1.2.0 -m "Release v1.2.0"`<br>`git push origin v1.2.0` | Creates and pushes an annotated tag marking a release point. |
| Restore file from commit | `git restore --source abc1234 -- path/to/file.py` | Replaces the working copy of a file with its version from `abc1234` (doesn’t change history). |
| Reset (move HEAD/branch) | `git reset --soft abc1234` (keep staged)<br>`git reset abc1234` (unstage, keep worktree)<br>`git reset --hard abc1234` (discard work) | Moves branch pointer to an older commit. Use `--hard` only when you’re certain you don’t need current changes. |
| Reflog recovery | `git reflog` | Lists recent HEAD movements; lets you recover “lost” commits. Example: find old SHA, then `git switch --detach <sha>` or create a branch there. |
| Clean untracked files | `git clean -fd` | Deletes untracked files/dirs (irreversible). Preview first with `git clean -fdn`. |
| Prune stale remotes | `git fetch --prune` | Removes local references to remote branches that were deleted on the server. |
| Inspect remote branches | `git branch -vv` | Shows tracking info and whether you’re ahead/behind the remote. |
| Diff before pushing | `git diff --stat origin/feature/login..` | Compares your branch to its remote tracking branch to see what will be pushed. |
| Search codebase | `git grep "api_key"` | Grep within tracked files (faster than plain `grep`); add `--untracked` to include new files. |
| Bisect a regression | `git bisect start`<br>`git bisect bad`<br>`git bisect good <sha>` | Binary-search history to find the commit that introduced a bug. Mark each step `git bisect good`/`bad`; finish with `git bisect reset`. |
| Worktrees (multiple checkouts) | `git worktree add ../repo-main main` | Maintain multiple branches checked out simultaneously (useful for benchmarking or running long jobs). Remove with `git worktree remove <path>`. |
| Sparse checkout (large repos) | `git sparse-checkout set path1 path2` | Limit checkout to subpaths (great when large model assets live elsewhere). Enable with `git sparse-checkout init --cone`. |
| Submodules | `git submodule update --init --recursive` | Pulls nested repos (common for model weights/configs). Clone with `--recurse-submodules` to avoid missing content. |
| Large files (Git LFS) | `git lfs install`<br>`git lfs track "*.bin"` | Set up Git LFS for model weights/checkpoints. Commit the `.gitattributes` change; push normally (LFS handles storage). |
| Revert a commit | `git revert abc1234` | Creates a new commit that undoes `abc1234` (safer than reset on shared branches). Use `--no-commit` to batch multiple reverts. |
| Export a clean archive | `git archive --format=zip HEAD > repo.zip` | Creates a zip/tar of the current tree without `.git` metadata—handy for sharing snapshots. |
| Show repo root | `git rev-parse --show-toplevel` | Prints the repository root path (useful in scripts/automation). |

Tips: keep feature work off `main`; prefer `--force-with-lease` when rewriting history; stash or commit before risky operations like rebases or resets; rerun tests after resolving conflicts.***
