### Understanding File and Directory Permissions

Permission management in Linux is a fundamental security mechanism that controls who can access and what they can do with files and directories. Every file and directory on a Linux system is associated with a set of permissions that are categorized into three distinct entities:

  * **Owner (u):** The user who owns the file or directory. By default, this is the user who created it.
  * **Group (g):** The group that has been assigned ownership of the file or directory. All users in this group share the same access rights.
  * **Others (o):** All other users on the system who are not the owner and not part of the file's group.

-----

### The Three Permission Types

For each of the three entities (owner, group, and others), there are three primary types of permissions:

1.  **Read (r):**

      * **Files:** Allows viewing the contents of the file.
      * **Directories:** Allows listing the contents of the directory (i.e., seeing what files and subdirectories are inside).

2.  **Write (w):**

      * **Files:** Allows modifying, saving, or deleting the file.
      * **Directories:** Allows creating, renaming, or deleting files and subdirectories within that directory.

3.  **Execute (x):**

      * **Files:** Allows running the file as a program or script.
      * **Directories:** Allows entering the directory (i.e., navigating into it with `cd`).

These permissions are often represented in a simple format, such as `rwx` for read, write, and execute.

-----

### Viewing Permissions

You can view a file or directory's permissions using the `ls -l` command.

```bash
ls -l my_file.txt
-rw-r--r-- 1 bubu bubu 0 Sep 18 09:42 my_file.txt
```

The first part of the output, `-rw-r--r--`, is the permissions string. Let's break it down:

  * **`d` or `-`:** The very first character indicates the file type. A hyphen (`-`) means it's a regular file, while a `d` indicates a directory.
  * **`rw-`:** These are the permissions for the **owner**. In this example, the owner has read and write permissions but not execute.
  * **`r--`:** These are the permissions for the **group**. The group has only read permission.
  * **`r--`:** These are the permissions for **others**. Others also have only read permission.

-----

### Changing Permissions: `chmod`

The `chmod` (change mode) command is used to change a file's permissions. It can be used in two common ways: symbolic mode and octal mode.

#### 1\. Symbolic Mode

This method uses letters to represent the entities and permissions.

  * **Entities:** `u` (user/owner), `g` (group), `o` (others), `a` (all).
  * **Operators:** `+` (add permission), `-` (remove permission), `=` (set exact permissions).
  * **Permissions:** `r`, `w`, `x`.

**Examples:**

  * `chmod u+x my_script.sh`: Adds execute permission for the owner.
  * `chmod go-w my_file.txt`: Removes write permission for the group and others.
  * `chmod a=r my_file.txt`: Sets read-only permission for all.

#### 2\. Octal Mode

This is a more powerful and compact way to set permissions using a three-digit number. Each permission type is assigned a value:

  * **r** = 4
  * **w** = 2
  * **x** = 1
  * **-** = 0 (no permission)

You sum these values for each of the three entities (owner, group, and others) to get a three-digit number.

**Examples:**

  * `chmod 755 my_script.sh`:
      * **7** (4+2+1) = read, write, and execute for the owner.
      * **5** (4+0+1) = read and execute for the group.
      * **5** (4+0+1) = read and execute for others.
  * `chmod 644 my_file.txt`:
      * **6** (4+2+0) = read and write for the owner.
      * **4** (4+0+0) = read-only for the group.
      * **4** (4+0+0) = read-only for others.

-----

### Changing Ownership: `chown` and `chgrp`

  * **`chown` (change owner):** This command changes the user and/or group ownership of a file or directory.

      * `sudo chown <new_owner> my_file.txt`: Changes the owner of `my_file.txt`.
      * `sudo chown <new_owner>:<new_group> my_file.txt`: Changes both the owner and the group.

  * **`chgrp` (change group):** This command is specifically for changing the group ownership.

      * `sudo chgrp <new_group> my_file.txt`: Changes the group of `my_file.txt`.
