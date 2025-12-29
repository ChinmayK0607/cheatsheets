# Linux/Server Cheatsheet

| Function | Command | Detailed Explanation & Example |
| --- | --- | --- |
| SSH into server | `ssh user@host` | Connect to a remote machine. Add `-i ~/.ssh/key` for a specific key; `-p 2222` for non-default port. |
| Copy local → remote | `scp file.txt user@host:/path/` | Simple file transfer. For directories: `scp -r mydir user@host:/path/`. |
| Copy remote → local | `scp user@host:/path/file.txt ./` | Pulls a file down. Use `-r` for directories. |
| Sync folders efficiently | `rsync -avh --progress src/ user@host:/path/` | Preserves metadata, transfers only deltas. Pulling is the same with reversed src/dst. Add `--delete` to mirror exactly. |
| Stream a remote file | `ssh user@host "tail -f /var/log/app.log"` | Follows a log live over SSH. Add `-n 200` to show last 200 lines first. |
| Search text in files | `rg "needle"` | Ripgrep across a tree (fast). Remote: `ssh user@host "cd /project && rg 'needle'"`. |
| Inspect file safely | `less file.txt` | Paged view; `/pattern` to search, `n` for next, `G` to end. Use `zless` for compressed logs. |
| Show disk usage (mounts) | `df -h` | Human-readable disk usage by filesystem. |
| Show disk usage (per dir) | `du -sh *` | Sizes of directories/files in current path. |
| Check free memory | `free -h` | Summarized RAM usage. |
| List processes | `ps aux | head` | Snapshot of running processes. Filter: `ps aux | grep python`. |
| Top/htop | `top` | Live CPU/mem view; `htop` is friendlier if installed. |
| GPU usage | `nvidia-smi` | Shows GPU utilization and processes (if NVIDIA GPUs present). |
| Check ports/listen | `ss -tulpn | head` | Lists listening sockets and owning processes (needs sudo for full info). |
| Kill a process | `kill <pid>` | Sends SIGTERM. Use `kill -9 <pid>` only if it refuses to exit. |
| Background long jobs | `nohup python train.py > out.log 2>&1 &` | Runs detached; output goes to `out.log`. |
| Persistent sessions | `tmux new -s work` | Start tmux session for long tasks. Reattach: `tmux attach -t work`; list: `tmux ls`. |
| Archive/compress | `tar -czf logs.tar.gz logs/` | Compress directory. Extract: `tar -xzf logs.tar.gz`. |
| Decompress common formats | `gzip -d file.gz` or `zcat file.gz` | `zcat` streams decompressed content (useful with pipes: `zcat file.gz | rg error`). |
| Download file | `wget URL` | Fetches via HTTP/HTTPS. Add `-O name` to rename; `-c` to continue partial. |
| Curl with headers | `curl -H "Authorization: Bearer TOKEN" https://api.example.com` | Handy for API testing. Add `-d @data.json` for payloads. |
| Check path/where you are | `pwd` | Prints working directory. |
| Find files by name | `find . -name "*.log"` | Recursive file search. Combine with size: `find . -size +500M`. |
| Count lines quickly | `wc -l file.txt` | Line count; use `zcat file.gz | wc -l` for compressed. |
| Quick HTTP server | `python -m http.server 8000` | Serves current dir on port 8000 (handy for quick file sharing). |
| Environment variables | `printenv | head` | Show env vars. Set for one command: `ENV=value command`. |
| Add to PATH temporarily | `PATH=$HOME/bin:$PATH command` | Useful for per-invocation PATH tweaks. |
| Show system info | `uname -a` | Kernel and OS info. Add `lsb_release -a` (if available) for distro details. |
| Permissions quick fix | `chmod +x script.sh` | Makes a script executable. Use `chown user:group file` to change ownership (sudo may be required). |
| Disk hotspot hunt | `du -sh /var/* | sort -h` | Finds large directories under /var. Adjust path as needed. |
| Remote tar streaming (copy without temp files) | `ssh user@host "tar -czf - -C /path/to/data ." | tar -xzf - -C ./local_dir` | Streams a compressed tarball over SSH to local without storing intermediate files. Reverse directions to send local → remote. |
| Port forwarding (local) | `ssh -L 8000:localhost:8000 user@host` | Forwards your local port 8000 to remote port 8000 (great for dashboards). Keep SSH session open while using. |
| Port forwarding (remote) | `ssh -R 9000:localhost:3000 user@host` | Exposes your local port 3000 to remote port 9000 (check server policy). |
| Check available space in a directory tree | `sudo ncdu /path` | Interactive disk usage explorer (install `ncdu`). |
| Quick JSON pretty-print | `jq . file.json | head` | Pretty-prints JSON; combine with curl: `curl -s URL | jq '.field'`. |
