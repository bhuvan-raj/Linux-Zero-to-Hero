# 📌 Process Management in Linux

Processes are at the core of Linux. A **process** is simply a running instance of a program, with its own memory, CPU usage, and resources.

Linux provides powerful tools to **create, monitor, manage, and terminate processes**.

---

## 1. 🔍 Viewing Processes

### `ps` — Process Status

Displays running processes.

```bash
ps          # show processes for current shell
ps -e       # all processes
ps -f       # full-format listing
ps aux      # detailed system-wide processes
```

* **Common Options**:

  * `a` → show processes for all users
  * `u` → show user-oriented output (CPU, MEM usage, start time, etc.)
  * `x` → show processes without controlling terminal (like daemons)

**Example:**

```bash
ps aux | grep nginx
```

Finds if **nginx** is running.

---

### `top` — Real-Time Process Viewer

Displays running processes in real-time (interactive).

```bash
top
```

* **Key Controls:**

  * `k` → kill a process
  * `r` → renice (change priority)
  * `q` → quit
  * `M` → sort by memory
  * `P` → sort by CPU

**Use case:** Monitoring server load and managing runaway processes.

---

### `htop` — Enhanced Top

A more user-friendly, interactive version of `top`.

```bash
htop
```

* Supports scrolling, searching, tree view.
* Easier to kill/renice processes.

---

### `pgrep` — Process Search

Searches for processes by name/pattern.

```bash
pgrep nginx
pgrep -u alice
```

* **Example:** `pgrep -l ssh` → lists processes with their names.

---

### `pidof` — Get Process ID

```bash
pidof sshd
```

Returns PID(s) of a running process.

---

---

## 2. ⚙️ Starting & Controlling Processes

### Foreground vs Background

* **Foreground process:** occupies the terminal until it finishes.
* **Background process:** runs detached from terminal.

```bash
./script.sh        # foreground
./script.sh &      # background
```

### `jobs` — List Background Jobs

```bash
jobs
```

### `fg` / `bg` — Bring Process to Foreground/Background

```bash
fg %1   # bring job 1 to foreground
bg %1   # resume job 1 in background
```

### `&`, `CTRL+Z`,`disown`

* `&` → run in background
* `CTRL+Z` → suspend process
  
  ```bash
  nohup ./longtask.sh &
  ```
* `disown` → remove job from shell’s job table

---

---

## 3. 🛑 Killing Processes

### `kill` — Send Signal

```bash
kill -SIGTERM <pid>
kill -9 <pid>   # force kill
```

* Default signal: **SIGTERM (15)**
* Can specify by name or number.

---

### `pkill` — Kill by Name

```bash
pkill nginx
```

Kills all processes with matching name.

---

### `killall` — Kill All Instances

```bash
killall firefox
```

Kills all processes named `firefox`.

---

---

## 4. 🎯 Process Priorities

Linux uses **niceness (priority value)**:

* Range: **-20 (highest priority)** → **19 (lowest priority)**
* Default = 0

### `nice` — Start Process with Priority

```bash
nice -n 10 ./task.sh
```

### `renice` — Change Priority of Running Process

```bash
renice -n -5 -p 1234
```

---

---

## 5. 📂 Process Hierarchy

Every process has a **Parent Process (PPID)** and may spawn **child processes**.

### `pstree` — Tree of Processes

```bash
pstree -p
```

Shows hierarchical structure with PIDs.

---

---

## 6. 📡 Linux Signals

A **signal** is a message sent to a process to instruct it to do something (stop, terminate, restart, ignore, etc.).

### Common Signals

| Signal      | Number | Meaning                                           |
| ----------- | ------ | ------------------------------------------------- |
| `SIGTERM`   | 15     | Terminate process gracefully (default for `kill`) |
| `SIGKILL`   | 9      | Force kill process (cannot be caught/ignored)     |
| `SIGSTOP`   | 19     | Stop (pause) process (cannot be caught/ignored)   |
| `SIGCONT`   | 18     | Continue a stopped process                        |
| `SIGINT`    | 2      | Interrupt (Ctrl+C)                                |
| `SIGHUP`    | 1      | Hang up (restart or reload config)                |
| `SIGQUIT`   | 3      | Quit and dump core                                |
| `SIGUSR1/2` | 10/12  | User-defined signals                              |
| `SIGCHLD`   | 17     | Sent to parent when child terminates              |
| `SIGTSTP`   | 20     | Stop from terminal (Ctrl+Z)                       |

---

### Example Usage

```bash
kill -SIGTERM 1234   # graceful stop
kill -9 1234         # force kill
kill -STOP 1234      # pause process
kill -CONT 1234      # resume process
```

---

## 7. 🧠 Daemons & Background Services

* **Daemon**: Background process with no controlling terminal (e.g., sshd, systemd).
* Managed via:

  * `systemctl start/stop/status <service>`
  * `service <name> start/stop`

---

## 8. ⚡ Monitoring Process Resource Usage

### `time` — Execution Time

```bash
time ./script.sh
```


### `lsof` — List Open Files by Process

```bash
lsof -p 1234
```

### `vmstat`, `iostat`, `sar` — System-wide Monitoring

---

## 🔑 Summary

* **Viewing processes** → `ps`, `top`, `htop`, `pgrep`
* **Managing jobs** → `jobs`, `fg`, `bg`, `nohup`, `disown`
* **Killing processes** → `kill`, `pkill`, `killall`
* **Priorities** → `nice`, `renice`
* **Hierarchy** → `pstree`
* **Signals** → graceful (`SIGTERM`), force (`SIGKILL`), pause (`SIGSTOP`), resume (`SIGCONT`)

---
