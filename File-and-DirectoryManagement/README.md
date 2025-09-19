
# 📚 In-depth Study Notes — File Management Commands in Linux

---

## 1. 📁 Navigating the File System

These commands help you move around and explore the directory structure.

### `pwd` — Print Working Directory

* **Purpose:** Displays the full absolute path of your current directory.
* **Syntax:**

  ```bash
  pwd
  ```
* **Example Output:**
  `/home/bubu/projects`

---

### `cd` — Change Directory

* **Purpose:** Move between directories.
* **Syntax:**

  ```bash
  cd <directory_path>
  ```
* **Usage Examples:**

  ```bash
  cd /etc         # absolute path
  cd ..            # go one directory up
  cd ~             # go to home directory
  cd -             # go to previous directory
  ```

---

### `ls` — List Files and Directories

* **Purpose:** Lists the contents of a directory.
* **Important Options:**

  * `-l` : long listing (permissions, owner, size, time)
  * `-a` : show hidden files
  * `-h` : human-readable sizes
  * `-R` : list recursively
* **Example:**

  ```bash
  ls -lah /var/log
  ```

---

## 2. 📝 File and Directory Creation/Deletion

### `touch` — Create Empty File or Update Timestamp

```bash
touch notes.txt
```

### `mkdir` — Create Directory

```bash
mkdir project
mkdir -p project/src/assets   # creates nested directories
```

### `rm` — Remove Files/Directories

* `rm file.txt` → delete file
* `rm -r folder` → delete directory recursively
* `rm -rf folder` → delete forcefully (⚠️ dangerous)

### `rmdir` — Remove Empty Directory

```bash
rmdir tempdir
```

---

## 📖 3. Viewing File Content Commands

---

### 📝 `cat` — Concatenate and Display File Content

**Purpose:**

* Displays the entire content of a file on the terminal.
* Can also combine (concatenate) multiple files and redirect output to another file.

**Syntax:**

```bash
cat [options] filename
```

**Examples:**

```bash
cat file.txt
cat file1.txt file2.txt > combined.txt
```

**Useful options:**

* `-n` → Show line numbers

---

### 🔁 `tac` — Reverse `cat`

**Purpose:**

* Displays the content of a file **from last line to first line**.

**Syntax:**

```bash
tac filename
```

**Example:**

```bash
tac messages.log
```

**Notes:**

* Very useful when you want to **read bottom-up**, such as checking the most recent entries first.

---

### 📜 `less` — View File Page by Page

**Purpose:**

* Allows you to scroll through large files **one screen at a time** without opening them in an editor.

**Syntax:**

```bash
less filename
```

**Controls inside less:**

* `Space` → Next page
* `b` → Previous page
* `/word` → Search forward
* `?word` → Search backward
* `n` → Repeat last search
* `q` → Quit

**Notes:**

* Loads the file **on demand**, so it’s faster than opening a huge file with `cat`.

---

### 📃 `more` — Older Pager (Like less)

**Purpose:**

* Displays text one screen at a time, similar to `less` but with fewer features.

**Syntax:**

```bash
more filename
```

**Controls:**

* `Space` → Next page
* `Enter` → Next line
* `q` → Quit

**Notes:**

* `less` is generally preferred over `more` because it allows backward movement and search.

---

### ⬆️ `head` — Show Beginning of a File

**Purpose:**

* Shows the **first few lines** of a file (default is 10 lines).

**Syntax:**

```bash
head [options] filename
```

**Examples:**

```bash
head file.txt          # first 10 lines
head -n 20 file.txt    # first 20 lines
```

**Notes:**

* Useful to quickly inspect the top of logs, config files, etc.

---

### ⬇️ `tail` — Show End of a File

**Purpose:**

* Shows the **last few lines** of a file (default is 10 lines).
* Can **follow a file in real-time** as it grows (great for logs).

**Syntax:**

```bash
tail [options] filename
```

**Examples:**

```bash
tail file.txt          # last 10 lines
tail -n 50 file.txt    # last 50 lines
tail -f log.txt        # follow new lines as they are added
```

**Notes:**

* `tail -f` is very commonly used to monitor log files during deployments or debugging.

---

**Editor Commands:**

* `nano filename` → beginner friendly
* `vi` / `vim filename` → powerful modal editor

---

## 4. 🗃️ Copying, Moving, and Renaming

### `cp` — Copy Files

```bash
cp source.txt destination.txt
cp -r dir1 dir2   # copy directories recursively
```

### `mv` — Move or Rename Files

```bash
mv old.txt new.txt         # rename
mv file.txt /tmp/           # move to different location
```

---

## 5. 🔐 File Permissions & Ownership

### Viewing Permissions

```bash
ls -l
```

**Example:**

```
-rwxr-xr-- 1 user group 512 Sep 18 09:00 script.sh
```

* `r` = read, `w` = write, `x` = execute
* three triplets: user / group / others


### Changing Ownership — `chown` and `chgrp`

```bash
chown alice file.txt
chgrp developers file.txt
```

---

## 6. 🧭 Finding and Locating Files

### `find`

* Search for files by name, size, time, type, etc.

```bash
find /etc -name hosts
find /var/log -type f -size +10M
find . -type f -mtime -2    # modified in last 2 days
```

### `locate`

* Uses a pre-built database for fast search (run `updatedb` first).

```bash
locate passwd
```

### `which`

```bash
which bash          # shows full path of command
```

---
# 📦 8. Archiving and Compression — In-depth Study Notes

---

## 📁 `tar` — Tape Archive

**Purpose:**

* Combines multiple files and directories into a **single archive file** (`.tar`).
* It **does not compress** by default — just groups files together.

---

### 🧩 Basic Syntax

```bash
tar [options] -f <archive_name.tar> <file(s)/dir(s)>
```

### ⚙️ Common Options

| Option | Meaning                  |
| ------ | ------------------------ |
| `-c`   | Create archive           |
| `-x`   | Extract archive          |
| `-v`   | Verbose (show progress)  |
| `-f`   | File name of archive     |
| `-t`   | List contents of archive |

---

### 📌 Examples

**Create an archive:**

```bash
tar -cvf backup.tar file1.txt dir1
```

**Extract an archive:**

```bash
tar -xvf backup.tar
```

**List contents without extracting:**

```bash
tar -tvf backup.tar
```

**Extract to a specific directory:**

```bash
tar -xvf backup.tar -C /tmp/myfolder
```

---

### 📝 With Compression

`tar` can work with compression tools like **gzip** and **bzip2** by adding extra flags:

| Compression | Flag | Extension           | Example                         |
| ----------- | ---- | ------------------- | ------------------------------- |
| gzip        | `-z` | `.tar.gz` or `.tgz` | `tar -czvf backup.tar.gz dir1`  |
| bzip2       | `-j` | `.tar.bz2`          | `tar -cjvf backup.tar.bz2 dir1` |
| xz          | `-J` | `.tar.xz`           | `tar -cJvf backup.tar.xz dir1`  |

**Extract compressed archives:**

```bash
tar -xzvf backup.tar.gz     # gzip
tar -xjvf backup.tar.bz2    # bzip2
tar -xJvf backup.tar.xz     # xz
```

---

## 🗜️ `gzip` and `gunzip`

**Purpose:**

* `gzip` compresses individual files.
* `gunzip` decompresses `.gz` files.
* It works on **single files only** (not directories).

---

### 🧩 Syntax

```bash
gzip [options] filename
gunzip [options] filename.gz
```

**Examples:**

```bash
gzip large.log           # creates large.log.gz
gunzip large.log.gz      # restores to large.log
```

**Notes:**

* `gzip` **removes the original file** after compression (unless `-k` is used).
* To keep original: `gzip -k large.log`

**Useful options:**

* `-v` → verbose output
* `-d` → decompress (same as gunzip)

---

## 🧳 `zip` and `unzip`

**Purpose:**

* `zip` compresses **multiple files and directories** into a `.zip` archive.
* `unzip` extracts them.
* Unlike `gzip`, `zip` can handle multiple files directly.

---

### 🧩 Syntax

```bash
zip [options] archive.zip file1 file2 ...
unzip [options] archive.zip
```

**Examples:**

```bash
zip backup.zip file1.txt file2.txt
zip -r project.zip project_dir    # recursively zip directory

unzip backup.zip
unzip backup.zip -d /tmp/extract_here
```

**Useful options:**

* `-r` → recursive (include all subdirectories)
* `-v` → verbose
* `-d <dir>` → extract to given directory

---

## ⚡ Tar vs Gzip vs Zip — At a Glance

| Tool         | Combines multiple files | Compresses                        | Typical extension | Notes                                   |
| ------------ | ----------------------- | --------------------------------- | ----------------- | --------------------------------------- |
| `tar`        | ✅ Yes                   | ❌ No (unless with `-z`/`-j`/`-J`) | `.tar`            | Just packaging                          |
| `gzip`       | ❌ No                    | ✅ Yes                             | `.gz`             | Compresses single files                 |
| `tar + gzip` | ✅ Yes                   | ✅ Yes                             | `.tar.gz`         | Common combo                            |
| `zip`        | ✅ Yes                   | ✅ Yes                             | `.zip`            | Handles multi-file compression natively |

---

## 📌 Practical Tips

* Use `tar -czvf` for backups of whole directories (most common).
* Use `gzip` for compressing **single large log files**.
* Use `zip` if sharing files cross-platform (Windows, macOS, Linux).
* Always use `-v` for verbose to see what’s happening.
* Use `du -h` to check sizes before and after compression.

---

## 10. 📑 Miscellaneous Helpful Commands

| Command    | Purpose                 | Example                |
| ---------- | ----------------------- | ---------------------- |
| `wc`       | Count lines/words       | `wc -l file.txt`       |
| `du`       | Show disk usage         | `du -sh /var/log`      |
| `df`       | Show free space         | `df -h`                |
| `basename` | Extract filename        | `basename /etc/passwd` |
| `dirname`  | Extract directory path  | `dirname /etc/passwd`  |
| `stat`     | Show file metadata      | `stat file.txt`        |
| `file`     | Show file type          | `file /bin/bash`       |

---
