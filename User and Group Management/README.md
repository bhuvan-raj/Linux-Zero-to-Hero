# 🧑‍💻 Linux User Management – Core Commands

<img src="https://github.com/bhuvan-raj/Linux-Zero-to-Hero/blob/main/assets/um.png" alt="Banner" width="800" height="400" />


Managing users is a key part of system administration.
These commands help you **view user info, create users, and manage their credentials**.

---

## 1. 🧠 `whoami`

**Purpose:** Shows the **currently logged-in username**.
**Usage:**

```bash
whoami
```

**Example Output:**

```
bubu
```

✅ Useful in scripts to know which user is running them.

---

## 2. 🪪 `id`

**Purpose:** Displays the **user ID (UID), group ID (GID), and group memberships** of a user.

**Usage:**

```bash
id          # current user
id username # specific user
```

**Example Output:**

```
uid=1000(bubu) gid=1000(bubu) groups=1000(bubu),27(sudo)
```

---

## 3. 👥 `who`

**Purpose:** Shows **who is logged in to the system** currently.

**Usage:**

```bash
who
```

**Example Output:**

```
bubu     tty1         2025-09-11 09:12
admin    pts/0        2025-09-11 10:01 (192.168.1.5)
```

---

## 4. 👤 `users`

**Purpose:** Displays a **short list of all currently logged-in users (only usernames)**.

**Usage:**

```bash
users
```

**Example Output:**

```
bubu admin root
```

---

## 5. ➕ `adduser`

**Purpose:** Interactively **create a new user** (recommended, user-friendly).
It also creates the user’s home directory.

**Usage:**

```bash
sudo adduser john
```

**Steps shown during execution:**

* Creates user
* Asks for password
* Sets user info (name, room, etc.)

---

## 6. ➕ `useradd`

**Purpose:** **Create a user (low-level)** — faster but needs options.
**Usage:**

```bash
sudo useradd -m john
sudo passwd john
```

* `-m` → creates home directory
* Needs `passwd` to set a password after creation

**Difference from `adduser`:**
`adduser` is more user-friendly, `useradd` is more basic and script-friendly.

---

## 7. 🔐 `passwd`

**Purpose:** Sets or changes a user’s password.

**Usage:**

```bash
passwd            # change own password
sudo passwd john  # change password of another user
```

**Extra:** Also can **lock or unlock** an account:

```bash
sudo passwd -l john   # lock
sudo passwd -u john   # unlock
```

---

## 8. 📇 `getent passwd`

**Purpose:** Retrieves **user account info from system databases (like `/etc/passwd` or LDAP)**.

**Usage:**

```bash
getent passwd john
```

**Example Output:**

```
john:x:1001:1001:John Doe,,,:/home/john:/bin/bash
```

This is better than `cat /etc/passwd` because it also works with network user directories (LDAP, NIS, etc.).

---

## 9. 📁 `cat /etc/passwd`

**Purpose:** Shows **all local user accounts** on the system.

**Usage:**

```bash
cat /etc/passwd
```

**Example Line:**

```
bubu:x:1000:1000:Bubu,,,:/home/bubu:/bin/bash
```

**Fields (colon-separated):**

```
username : x : UID : GID : comment : home directory : login shell
```

---

## 10. ⏳ `chage`

**Purpose:** View or change **password aging policies** for a user
(password expiry, last changed date, account expiry).

**Usage:**

```bash
sudo chage -l john       # list password aging info
sudo chage -M 90 john     # set password to expire every 90 days
sudo chage -E 2025-12-31 john  # set account expiry date
```

**Example Output:**

```
Last password change                                    : Sep 10, 2025
Password expires                                        : Dec 09, 2025
Account expires                                         : never
```

## ⏳ What Happens When a User’s Password Expires

When you use `chage` or system policies to set a **password expiry date**, Linux tracks this in the **`/etc/shadow`** file.

Each user account has:

* **Last password change date**
* **Maximum password age**
* **Password expiration date**

---

### 📌 When the Password Expires

* The user can **still log in**, **but is immediately forced to change the password** before continuing.
* After logging in, the system will show:

  ```
  Your password has expired.
  You must change your password now and login again!
  ```
* The user must enter their **old password** and then set a **new password**.
* If they do not change it, they cannot proceed to a shell session.

✅ This is called **graceful expiration** — the account is **not locked**, only the **password is considered old**.

---

### ⛔ If Password Is Expired + Account Is Locked

If the user doesn’t change it within the allowed **grace period** (if configured), the account can be **disabled**, and login will fail.

In that case they see:

```
Authentication failure
```

or

```
Account expired
```

Then an **administrator must reset their password** using:

```bash
sudo passwd username
```

---

## 📁 Where Expiry Info is Stored

In `/etc/shadow`, each line has:

```
username:encrypted_pass:last_change:min:max:warn:inactive:expire
```

Example:

```
john:$6$.....:19623:0:90:7:14:
```

* `90` → max days before expiry
* `7` → warning before expiry
* `14` → inactive period (after expiry before lock)

---

## 📌 Summary

| State                        | Meaning                              | User Can Login?           | Action Needed         |
| ---------------------------- | ------------------------------------ | ------------------------- | --------------------- |
| Password valid               | Not expired                          | ✅ Yes                     | Nothing               |
| Password expired             | Must change password at next login   | ⚠️ Only to reset password | User resets password  |
| Account locked due to expiry | Password not changed in grace period | ❌ No                      | Admin resets password |



## 🛠 How to Unexpire an Expired User

### Option 1 — Using `chage`

```bash
sudo chage -E -1 username
```

* `-E` sets the account expiration date.
* `-1` means **“never expires”**.

---

### Option 2 — Using `usermod`

```bash
sudo usermod -e "" username
```

* `-e ""` clears the expiration date.

---

After this, the user will be able to log in normally again.
Their files, permissions, and UID stay the same — nothing is lost.
---

## 🧠 Does installing an application create a user automatically?

**Not always — but sometimes, yes.**
It depends on the **type of application** and **how it is designed to run**.


## ⚙️ Why some applications create their own users

Some services (like databases, web servers, mail servers, etc.) need to run **as a non-root user** for **security reasons**.

* Running everything as `root` is dangerous: if the app gets compromised, the attacker gets full system control.
* So, the package creates a **dedicated system user** to run the service in an isolated way.

### 📝 Example

* Installing `mysql` creates a user called `mysql`
* Installing `nginx` creates a user called `nginx`
* Installing `apache2` creates a user called `www-data`

You can see them in `/etc/passwd` like this:

```bash
grep mysql /etc/passwd
grep nginx /etc/passwd
```

They are usually **system users**:

* Have **no login shell** (`/usr/sbin/nologin` or `/bin/false`)
* Have **no password**
* Only used internally by the service

---

## 📌 Key Points

* ✅ Some packages create a dedicated user automatically.
* ✅ These are usually **system users** for running daemons safely.
---

## 👥 Users in Linux

Linux has **two types of users**:

| Type                     | UID Range                         | Purpose                                                                               |
| ------------------------ | --------------------------------- | ------------------------------------------------------------------------------------- |
| **System users**         | 0–999 (or 0–499 on older systems) | Created by the system or packages to run services (e.g. `mysql`, `nginx`, `www-data`) |
| **Normal (human) users** | 1000+                             | Created manually for real people                                                      |

> `root` always has UID `0`.

---

## 📋 How to List All Users

All local user accounts are stored in:

```
/etc/passwd
```

**Command:**

```bash
cat /etc/passwd
```

Example line:

```
bubu:x:1000:1000:Bubu,,,:/home/bubu:/bin/bash
```

**Fields (colon-separated):**

```
username : x : UID : GID : comment : home_dir : login_shell
```

---

## 🧮 Filter Based on UID

### 🔹 List all system users (UID < 1000)

```bash
awk -F: '$3 < 1000 {print $1, $3}' /etc/passwd
```

Example output:

```
root 0
daemon 1
mysql 113
nginx 116
```

---

### 🔹 List all normal (human) users (UID ≥ 1000)

```bash
awk -F: '$3 >= 1000 {print $1, $3}' /etc/passwd
```

Example output:

```
bubu 1000
john 1001
```

---

## 💡 Bonus: Using `getent`

If your system uses central authentication (LDAP, NIS), use `getent` instead:

```bash
getent passwd | awk -F: '$3>=1000 {print $1}'
```

This shows all normal users from both local and network databases.

---

## 📌 Summary

* `cat /etc/passwd` → view all users
* `UID < 1000` → system users
* `UID ≥ 1000` → normal human users
* `root` is always `UID 0`

---

## 📌 Summary Table

| Command           | Purpose                                |
| ----------------- | -------------------------------------- |
| `whoami`          | Shows current logged-in username       |
| `id`              | Shows UID, GID, groups of a user       |
| `who`             | Shows all logged-in users              |
| `users`           | Shows logged-in users (only usernames) |
| `adduser`         | Create a user (interactive)            |
| `useradd`         | Create a user (non-interactive)        |
| `passwd`          | Set/change/lock/unlock password        |
| `getent passwd`   | Get user info from system databases    |
| `cat /etc/passwd` | Shows all local user accounts          |
| `chage`           | Manage password/account expiry         |

---

# 🧠 `usermod`

## 📌 What is `usermod`?

* `usermod` is used to **modify existing user accounts** in Linux.
* It can change a user’s **login name, UID, GID, home directory, shell, expiry date, group memberships**, and more.
* It edits these files under the hood:

  * `/etc/passwd` — user account info
  * `/etc/shadow` — password and expiry info
  * `/etc/group` and `/etc/gshadow` — group info

---

## ⚙️ General Syntax

```bash
usermod [options] username
```

* **Must be run as root or using `sudo`**.

---

## 🧩 Commonly Used Options (with examples)

| Option           | Description                                             | Example                               |
| ---------------- | ------------------------------------------------------- | ------------------------------------- |
| `-l newname`     | Change the user’s login name                            | `sudo usermod -l bob alice`           |
| `-d new_home`    | Change the user’s home directory (without moving files) | `sudo usermod -d /home/bob bob`       |
| `-d new_home -m` | Change the home directory **and move files** to it      | `sudo usermod -d /home/bob -m bob`    |
| `-u UID`         | Change the user’s UID                                   | `sudo usermod -u 1050 bob`            |
| `-g group`       | Change the user’s primary group                         | `sudo usermod -g developers bob`      |
| `-G g1,g2`       | Set **supplementary** groups (replaces current list)    | `sudo usermod -G docker,sudo bob`     |
| `-aG g1,g2`      | **Append** supplementary groups (recommended)           | `sudo usermod -aG docker bob`         |
| `-s /bin/bash`   | Change login shell                                      | `sudo usermod -s /bin/zsh bob`        |
| `-c "comment"`   | Set or change comment field (Full Name)                 | `sudo usermod -c "Bob Developer" bob` |
| `-L`             | Lock account (disables password login)                  | `sudo usermod -L bob`                 |
| `-U`             | Unlock account                                          | `sudo usermod -U bob`                 |
| `-e YYYY-MM-DD`  | Set account expiry date                                 | `sudo usermod -e 2025-12-31 bob`      |
| `-f days`        | Set password inactivity period after expiry             | `sudo usermod -f 10 bob`              |

---

## 📂 Behind the Scenes — What Happens

| Action          | File modified                               |
| --------------- | ------------------------------------------- |
| Change username | `/etc/passwd` and `/etc/shadow` login field |
| Change UID      | `/etc/passwd` UID field                     |
| Change groups   | `/etc/group` and `/etc/gshadow`             |
| Change home     | `/etc/passwd` home field                    |
| Change shell    | `/etc/passwd` shell field                   |
| Set expiry      | `/etc/shadow` last field                    |
---

## 📝 Practical Examples

**1. Add user to the docker group (supplementary):**

```bash
sudo usermod -aG docker alice
```

**2. Move home directory from `/home/alice` to `/home/alicia`:**

```bash
sudo usermod -d /home/alicia -m alice
```

**3. Change login shell to Zsh:**

```bash
sudo usermod -s /bin/zsh alice
```

**4. Lock and unlock a user account:**

```bash
sudo usermod -L alice    # lock (disables password login)
sudo usermod -U alice    # unlock
```

**5. Set account to expire on a specific date:**

```bash
sudo usermod -e 2025-12-31 alice
```


## 🆔 `id`

**Purpose:** Show UID, GID, and group membership of a user.

**Syntax:**

```bash
id [username]
```

**Examples:**

```bash
id           # current user
id alice     # specific user
```

**Output Example:**

```
uid=1000(alice) gid=1000(alice) groups=1000(alice),27(sudo),1001(docker)
```

✅ **Use it to:** verify who you are and what groups you belong to.

---

## 🗑️ `deluser`

**Purpose:** High-level Debian/Ubuntu command to remove users (wrapper script).

**Syntax:**

```bash
sudo deluser [--remove-home] username
```

**Examples:**

```bash
sudo deluser alice               # remove user (keeps home)
sudo deluser --remove-home alice # remove user and their home directory
```

**Notes:**

* Does distro-specific cleanup (mail spool, groups, etc.).
* Safer and more user-friendly than `userdel`.

---

## 🗑️ `userdel`

**Purpose:** Low-level universal Linux command to remove users.

**Syntax:**

```bash
sudo userdel [-r] username
```

**Examples:**

```bash
sudo userdel alice       # remove user only
sudo userdel -r alice    # remove user and their home + mail spool
```

**Notes:**

* Works on all distros.
* Does **not** remove files owned by the user outside their home directory.
* `-r` is **permanent** — use with caution.

---

📌 **Key difference**

* `deluser` = friendlier, high-level wrapper (Debian-based systems)
* `userdel` = universal low-level tool (works on all Linux systems)

# 🧠 Linux Group Management — In-Depth Study Note

<img src="https://github.com/bhuvan-raj/Linux-Zero-to-Hero/blob/main/assets/gm.jpeg" alt="Banner" />


---

## 📌 What are Groups?

* Groups are collections of users.
* They are used to **grant permissions collectively** (on files, directories, devices, etc.).
* Each user has:

  * **Primary group** — defined in `/etc/passwd`
  * **Supplementary groups** — listed in `/etc/group`

---

## 📂 Important Files

| File           | Purpose                             |
| -------------- | ----------------------------------- |
| `/etc/group`   | Group names, GIDs, member usernames |
| `/etc/gshadow` | Group passwords, administrators     |

---

## ⚙️ Core Group Management Commands

---

### 1) `groupadd`

**Purpose:** Create a new group.

**Syntax**

```bash
sudo groupadd [options] groupname
```

**Examples**

```bash
sudo groupadd developers             # create a new normal group
sudo groupadd -g 1500 developers     # specify a GID manually
sudo groupadd -r syslog              # create a system group
```

**Notes**

* `-r` creates a **system group** (typically uses GID < 1000).
* Use `getent group developers` to confirm creation.

---

### 2) `groupdel`

**Purpose:** Delete an existing group.

**Syntax**

```bash
sudo groupdel groupname
```

**Example**

```bash
sudo groupdel developers
```

**Notes**

* Only deletes the group entry; does **not** affect user accounts or file ownership.
* If any user has this group as **primary group**, you must change that user’s primary group first.

---

### 3) `groupmod`

**Purpose:** Modify existing groups (rename or change GID).

**Syntax**

```bash
sudo groupmod [options] groupname
```

**Examples**

```bash
sudo groupmod -n devteam developers   # rename 'developers' to 'devteam'
sudo groupmod -g 1600 developers      # change GID to 1600
```

**Notes**

* Be careful changing GID — existing files may need ownership updated.

---

### 4) `gpasswd`

**Purpose:** Manage group membership and set group admins or passwords.

**Syntax**

```bash
sudo gpasswd [options] groupname
```

**Common options & examples**

```bash
sudo gpasswd -a alice developers   # add alice to group
sudo gpasswd -d alice developers   # remove alice from group
sudo gpasswd -A bob developers     # set bob as group administrator
sudo gpasswd developers            # set a password for the group
```

**Notes**

* Group passwords are mostly obsolete; modern systems use `sudo` and group memberships instead.
* `-a` and `-d` edit `/etc/group` and `/etc/gshadow`.

---

### 5) `groups`

**Purpose:** Show groups that a user belongs to.

**Syntax**

```bash
groups [username]
```

**Example**

```bash
groups alice
```

**Output**

```
alice : alice sudo docker developers
```

---

### 6) `getent group`

**Purpose:** View all groups or a single group entry.

**Examples**

```bash
getent group                # list all groups
getent group developers     # show one group entry
```

**Output example**

```
developers:x:1001:alice,bob
```

---
## 👥 Adding/Removing Users from Groups

* **Add user to supplementary group:**

  ```bash
  sudo usermod -aG developers alice
  ```
* **Remove user from a group (manually):**

  ```bash
  sudo gpasswd -d alice developers
  ```
* **Change primary group:**

  ```bash
  sudo usermod -g developers alice
  ```

---

## ⚠️ Important Notes & Gotchas

* **Primary group** is stored in `/etc/passwd` (GID field).
* **Supplementary groups** are stored in `/etc/group`.
* Always use `-aG` (append) instead of `-G` alone (which overwrites all groups).
* Removing a group doesn’t remove files owned by that group — ownership stays but will display the GID if the group name no longer exists.
* Changing a group’s GID doesn’t automatically update file group ownership — you may need:

  ```bash
  sudo find / -group oldgroup -exec chgrp newgroup {} \;
  ```

---

## 📌 Quick Cheat Sheet

| Task                   | Command                            |
| ---------------------- | ---------------------------------- |
| Create group           | `sudo groupadd groupname`          |
| Delete group           | `sudo groupdel groupname`          |
| Rename group           | `sudo groupmod -n newname oldname` |
| Change group GID       | `sudo groupmod -g 1500 groupname`  |
| Add user to group      | `sudo gpasswd -a user group`       |
| Remove user from group | `sudo gpasswd -d user group`       |
| Show user groups       | `groups username`                  |
| Show all groups        | `getent group`                     |

---

## 📌 Summary

* **groupadd/groupdel/groupmod** — manage groups themselves.
* **gpasswd/usermod** — manage membership.
* **groups/getent** — check memberships.

---














