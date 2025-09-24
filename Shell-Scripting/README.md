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
name="Bubu"
echo "Hello $name"
```

### 3.4 Input & Output

```bash
echo "Enter your name:"
read user_name
echo "Welcome $user_name"
```

### 3.5 Conditional Statements

```bash
if [ $age -ge 18 ]
then
  echo "You are an adult"
else
  echo "You are a minor"
fi
```

### 3.6 Loops

**For Loop**

```bash
for i in 1 2 3 4 5
do
  echo "Number: $i"
done
```

**While Loop**

```bash
count=1
while [ $count -le 5 ]
do
  echo "Count = $count"
  count=$((count+1))
done
```

### 3.7 Functions

```bash
greet() {
  echo "Hello $1!"
}
greet "Bubu"
```

### 3.8 Exit Status

* Every command returns an **exit status code** (`0 = success, non-zero = error`).

```bash
ls /nonexistent
echo $?   # Prints exit status of last command
```

---

## 4. Fundamentals of Shell Scripting

✔ **Always start with shebang** (`#!/bin/bash`)
✔ **Make script executable**:

```bash
chmod +x script.sh
./script.sh
```

✔ **Indent code properly** for readability
✔ **Test exit codes** to handle errors properly
✔ **Use variables** instead of hardcoding values
✔ **Use quotes** around variables to avoid word splitting
✔ **Debug scripts** with `bash -x script.sh`

---

✅ **In summary**:

* **Shell** → interface between user and kernel.
* **Shell Script** → file with commands to automate tasks.
* **Syntax** → shebang, variables, conditionals, loops, functions, exit codes.
* **Fundamentals** → start with `#!/bin/bash`, make executable, handle errors, debug.
