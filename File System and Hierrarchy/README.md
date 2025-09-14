
# 🧠 Linux Filesystem — Internal Architecture

## 📌 What is a Filesystem?

A **filesystem** is the logic the operating system uses to store, organize, access, and manage data on storage media (like SSDs/HDDs).

**Key goals:**

* Locate and retrieve data efficiently
* Keep data organized and secure
* Track metadata (permissions, timestamps, ownership)
* Prevent corruption (journaling)

---

## 🧩 Components of a Linux Filesystem

**1. Superblock**

* Contains global metadata about the filesystem
* Located at a fixed position on disk
* Stores:

  * Filesystem type (ext4, xfs, btrfs…)
  * Total blocks, free blocks
  * Block size
  * Mount info
  * State (clean or needs fsck)

**2. Inodes (Index Nodes)**

* Every file/directory has an inode
* Stores metadata about the file **except the name**

  * File type (regular, directory, symlink…)
  * Permissions (rwx)
  * UID, GID
  * Size, timestamps (atime, mtime, ctime)
  * Pointers to data blocks

**3. Data Blocks**

* Store the **actual contents** of the file
* Inodes point to these blocks

**4. Directory Entries**

* Special files that map **filenames → inode numbers**

---

## 💽 Common Linux Filesystem Types

| Filesystem           | Description                                             |
| -------------------- | ------------------------------------------------------- |
| **ext2**             | Old, no journaling, simple                              |
| **ext3**             | ext2 + journaling                                       |
| **ext4**             | Default on many distros, large file support, journaling |
| **XFS**              | High-performance, great for large files                 |
| **Btrfs**            | Modern, snapshots, checksumming, pooling                |
| **F2FS**             | Optimized for flash storage                             |
| **vfat/exFAT**       | Windows-compatible, no permissions                      |
| **tmpfs**            | RAM-based, temporary                                    |
| **procfs (`/proc`)** | Virtual, exposes process/kernel info                    |
| **sysfs (`/sys`)**   | Virtual, exposes device/kernel data                     |

---

## ⚙️ Mounting Filesystems

* Linux uses a **single-rooted tree** structure.
* A **mount point** is a directory where another filesystem is attached.
* Mounting links physical devices into this tree.

```bash
mount /dev/sdb1 /mnt
umount /mnt
df -h      # check mounted filesystems
lsblk      # list block devices
```

* `/etc/fstab` defines what to mount automatically at boot.

---

## ⚡ Journaling

* Modern filesystems (ext4, xfs, btrfs) use **journaling**:

  * Changes are first written to a journal
  * After a crash, journal is replayed to recover
* Improves reliability and consistency

---

## 🧮 File Permissions and Ownership

* Each file has:

  * Owner (user), group, and permission bits
* Permissions: `rwx` for **user, group, others**
* Commands:

  * `chmod`, `chown`, `chgrp`
* Example:

```bash
-rw-r--r--  1 bubu dev 1024 Sep 14  notes.txt
```

---

# 🌳 Linux Filesystem Hierarchy — Directory Structure

Linux follows the **Filesystem Hierarchy Standard (FHS)** which organizes files in a **tree starting at `/` (root)**.

---

## 📁 Top-Level Directories and Their Purpose

| Directory        | Description                                              |
| ---------------- | -------------------------------------------------------- |
| `/`              | Root directory, starting point of all filesystems        |
| `/bin`           | Essential user binaries (ls, cp, mv, cat)                |
| `/sbin`          | Essential system binaries (fsck, reboot)                 |
| `/boot`          | Bootloader files, kernel (`vmlinuz`), initrd             |
| `/dev`           | Device files (disks, terminals, etc.)                    |
| `/etc`           | System configuration files (passwd, fstab, sshd\_config) |
| `/home`          | Home directories for normal users                        |
| `/root`          | Home directory of root user                              |
| `/lib`, `/lib64` | Shared libraries and kernel modules                      |
| `/media`         | Mount points for removable media (USB, CD)               |
| `/mnt`           | Temporary mount point for admins                         |
| `/opt`           | Optional third-party application software                |
| `/srv`           | Data served by services (like web or ftp)                |
| `/tmp`           | Temporary files, cleared on reboot                       |
| `/usr`           | User applications, binaries, libraries, documentation    |
| `/var`           | Variable data (logs, mail, spool, cache)                 |
| `/proc`          | Virtual FS with process/kernel info                      |
| `/sys`           | Virtual FS with hardware/kernel info                     |
| `/run`           | Runtime transient data (PID files, sockets)              |

---

## 🗂️ Directory Tree Example

```plaintext
/
├── bin
├── boot
├── dev
├── etc
├── home
│   ├── bubu
│   └── user2
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── srv
├── sys
├── tmp
├── usr
│   ├── bin
│   ├── lib
│   ├── sbin
│   └── share
└── var
    ├── log
    ├── mail
    └── cache
```

---

# 📌 Key Points to Remember

* Linux has a **single unified directory tree** starting at `/`.
* **Inodes** store metadata, **data blocks** store content.
* **Mounting** integrates storage devices into the tree.
* **Virtual filesystems** like `/proc` and `/sys` provide system info.
* **Permissions, journaling, and quotas** are part of filesystem management.
* **FHS** defines where files should logically live.

---

If you want, Bubu, I can next create a **clear diagram showing how superblock → inode → data blocks link up** alongside the **directory hierarchy tree**, to use in your teaching material.

Would you like me to make that diagram?
