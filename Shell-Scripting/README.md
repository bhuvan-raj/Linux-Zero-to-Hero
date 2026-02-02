# Shell & Shell Scripting
<img src="https://github.com/bhuvan-raj/Linux-Zero-to-Hero/blob/main/assets/shell.webp" alt="Banner" />


## 1. What is a Shell?

* A **Shell** is a program that acts as an **interface between the user and the kernel (OS core)**.
* It takes **commands** from the user, passes them to the operating system, and shows the output.
* It can be **command-line based (CLI)** or **graphical (GUI shells)**, but most commonly refers to CLI shells in Linux/Unix.

### Popular Types of Shells:

* **sh** → Bourne Shell (original Unix shell)
* **bash** → Bourne Again Shell (most common in Linux)
* **zsh** → Z Shell (improved bash)
* **fish** → Friendly Interactive Shell (user-friendly)
* **csh/tcsh** → C Shell

---

## 2. What is Shell Scripting?

* A **Shell Script** is a text file containing a sequence of shell commands.
* Instead of typing commands one by one, you **write them in a file** and execute the file.
* Used for automation of repetitive tasks like backups, deployments, monitoring, etc.

**Example:**

```bash
#!/bin/bash
echo "Hello, Bubu!"
date
```

---

## 3. Shell Scripting Syntax

### 3.1 Shebang (#!)

* The **first line** of every script is usually the interpreter directive:

```bash
#!/bin/bash
```

This tells the system which shell to use for executing the script.

### 3.2 Comments

* Add `#` before a line to make it a comment.

```bash
# This is a comment
```

### 3.3 Variables

```bash
name="akash"
echo "Hello $name"
```

### 3.4 Input & Output

```bash
echo "Enter your name:"
read user_name
echo "Welcome $user_name"
```

---

## 🔹 Conditional Statements in Shell Scripting

Conditional statements allow a shell script to **make decisions** based on conditions such as command success, variable values, file properties, or user input. They control the **flow of execution** of a script.

In Linux shell scripting, conditions are usually evaluated using:

* `if`, `else`, `elif`
* `test` command or `[ ]`
* `[[ ]]` (extended test)
* `case` statement

---

## 1️⃣ `if` Statement

### Basic Syntax

```bash
if condition
then
    commands
fi
```

### Example

```bash
if [ $age -ge 18 ]
then
    echo "Eligible to vote"
fi
```

🔹 The condition is evaluated first
🔹 If **true**, commands inside `then` are executed
🔹 `fi` marks the end of the `if` block

---

## 2️⃣ `if–else` Statement

Used when you want to execute **one block if true and another if false**.

### Syntax

```bash
if condition
then
    commands_if_true
else
    commands_if_false
fi
```

### Example

```bash
if [ $marks -ge 50 ]
then
    echo "Pass"
else
    echo "Fail"
fi
```

---

## 3️⃣ `if–elif–else` Statement

Used for **multiple conditions**.

### Syntax

```bash
if condition1
then
    commands
elif condition2
then
    commands
else
    commands
fi
```

### Example

```bash
if [ $score -ge 90 ]
then
    echo "Grade A"
elif [ $score -ge 70 ]
then
    echo "Grade B"
else
    echo "Grade C"
fi
```

---

## 4️⃣ Condition Testing Methods

### 🔸 `test` Command

```bash
if test $a -eq $b
then
    echo "Equal"
fi
```

### 🔸 Single Brackets `[ ]`

Most commonly used.

```bash
if [ $a -eq $b ]
then
    echo "Equal"
fi
```

⚠️ Spaces are **mandatory**:

```bash
[ $a -eq $b ]   # correct
[$a -eq $b]    # wrong
```

---

### 🔸 Double Brackets `[[ ]]` (Recommended)

Supports advanced features like regex and logical operators.

```bash
if [[ $name == "Bubu" ]]
then
    echo "Welcome"
fi
```

✅ Safer
✅ No word splitting issues
✅ Supports pattern matching

---

## 5️⃣ Numeric Comparison Operators

| Operator | Meaning               |
| -------- | --------------------- |
| `-eq`    | equal                 |
| `-ne`    | not equal             |
| `-gt`    | greater than          |
| `-lt`    | less than             |
| `-ge`    | greater than or equal |
| `-le`    | less than or equal    |

### Example

```bash
if [ $a -gt $b ]
then
    echo "$a is greater"
fi
```

---

## 6️⃣ String Comparison Operators

| Operator    | Meaning             |
| ----------- | ------------------- |
| `=` or `==` | equal               |
| `!=`        | not equal           |
| `-z`        | string is empty     |
| `-n`        | string is not empty |

### Example

```bash
if [ -z "$username" ]
then
    echo "Username is empty"
fi
```

---

## 7️⃣ File Test Conditions

| Test      | Meaning                    |
| --------- | -------------------------- |
| `-f file` | file exists and is regular |
| `-d file` | directory exists           |
| `-e file` | file exists                |
| `-r file` | readable                   |
| `-w file` | writable                   |
| `-x file` | executable                 |

### Example

```bash
if [ -f "/etc/passwd" ]
then
    echo "File exists"
fi
```

---

## 8️⃣ Logical Operators

### AND (`&&`)

```bash
if [ $age -ge 18 ] && [ $age -le 60 ]
then
    echo "Eligible"
fi
```

### OR (`||`)

```bash
if [ $role == "admin" ] || [ $role == "root" ]
then
    echo "Access granted"
fi
```

### NOT (`!`)

```bash
if [ ! -f "data.txt" ]
then
    echo "File not found"
fi
```

---

## 9️⃣ Using Commands as Conditions

In shell scripting, **command exit status** matters:

* `0` → success (true)
* Non-zero → failure (false)

### Example

```bash
if ping -c 1 google.com > /dev/null
then
    echo "Internet is up"
else
    echo "Internet is down"
fi
```

---

## 🔟 `case` Statement (Alternative to if-elif)

Best for **menu-driven scripts**.

### Syntax

```bash
case variable in
    pattern1)
        commands ;;
    pattern2)
        commands ;;
    *)
        default_commands ;;
esac
```

### Example

```bash
case $choice in
    start)
        echo "Starting service" ;;
    stop)
        echo "Stopping service" ;;
    restart)
        echo "Restarting service" ;;
    *)
        echo "Invalid option" ;;
esac
```

---

## 🔹 Best Practices

✔ Always quote variables: `"$var"`
✔ Prefer `[[ ]]` over `[ ]`
✔ Indent code for readability
✔ Use comments to explain logic

---

## 🔹 Real-World DevOps Example

```bash
if systemctl is-active nginx > /dev/null
then
    echo "Nginx is running"
else
    echo "Nginx is not running"
    systemctl start nginx
fi
```
