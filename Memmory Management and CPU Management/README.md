### 🧠 Memory Management

Memory management is the process of managing the computer's memory. The primary tool for memory management in Linux is the **kernel's virtual memory subsystem**, which uses **swap space** to extend RAM. Here are the key commands to monitor and manage memory.

***

#### 1. `free`
The `free` command is the most straightforward way to see how much memory is available. It displays the total amount of free and used physical memory and swap memory.
* **Syntax:** `free [options]`
* **Example:** `free -h`
    * The `-h` option provides a **human-readable** output (e.g., `G` for gigabytes, `M` for megabytes).
    * The output shows `total`, `used`, and `free` memory, as well as `shared`, `buff/cache`, and `available` memory. The **`available`** memory is a key metric, as it's an estimate of how much memory is usable by new applications without swapping.

#### 2. `swapon` and `swapoff`
These commands are used to enable and disable a **swap device** or **swap file**.
* **Syntax:** `swapon [options] <device_name>` or `swapoff [options] <device_name>`
* **Example:** `sudo swapon /dev/sdb2`
    * This command enables the partition `/dev/sdb2` as swap space.
    * You need **root privileges** (`sudo`) to use these commands.
* **To check active swap:** `swapon --show` or `cat /proc/swaps`

#### 3. `vmstat`
The `vmstat` (virtual memory statistics) command provides detailed, real-time reports on the system's virtual memory, processes, CPU, and I/O.
* **Syntax:** `vmstat [delay] [count]`
* **Example:** `vmstat 1`
    * This command prints a report every second until you stop it with `Ctrl+C`.
    * **Key columns** to watch for are:
        * **`si` (swap in):** Amount of memory swapped in from disk.
        * **`so` (swap out):** Amount of memory swapped out to disk.
    * High values in the `si` and `so` columns indicate that the system is heavily swapping, which is a sign of memory pressure and can degrade performance.

#### 4. `/proc/meminfo`
This is not a command but a **virtual file** that provides extensive, low-level details about the kernel's memory management.
* **Example:** `cat /proc/meminfo`
    * This provides a wealth of information, including `MemTotal`, `MemFree`, `Buffers`, `Cached`, and `SwapTotal`, giving a more granular view than `free`.

***

### 💻 CPU Management Commands

CPU management is handled by the Linux kernel's **scheduler**, which decides which processes get CPU time. You can influence the scheduler's decisions using the following commands.

#### 1. `top` or `htop`
These commands are the primary tools for monitoring CPU usage in real time.
* **Syntax:** `top` or `htop`
* **Usage:**
    * They show a dynamic list of processes sorted by their CPU usage.
    * **`htop`** is an enhanced version of `top` with a more user-friendly interface that allows you to easily sort, filter, and kill processes.
    * The first line of the output displays the **load average** over the last 1, 5, and 15 minutes, which is a key indicator of system performance. 

#### 2. `nice` and `renice`
These commands are used to manage a process's **scheduling priority** by setting its **niceness value**. A lower niceness value corresponds to a higher priority for CPU time.
* **Syntax:**
    * `nice -n <value> <command>`
    * `renice <value> -p <PID>`
* **Example:**
    * `nice -n 10 gzip big_file.tar`
        * This runs the `gzip` command with a niceness of 10, meaning it will have a lower CPU priority and won't interfere with other tasks.
    * `renice -5 -p 1234`
        * This changes the niceness of a running process with PID 1234 to -5, giving it a higher CPU priority.

#### 3. `taskset`
The `taskset` command is used to set or retrieve a process's **CPU affinity**, which is the set of CPU cores on which the process is allowed to run. This is useful for optimizing the performance of multithreaded applications.
* **Syntax:** `taskset [options] <mask> [command]`
* **Example:** `taskset -c 0,1,2 my_app`
    * This command runs `my_app` and restricts its execution to **CPU cores 0, 1, and 2**. This can prevent cache misses and other overhead associated with a process jumping between cores.

#### 4. `uptime`
This simple command provides a quick summary of how long the system has been running and displays the **load average**.
* **Syntax:** `uptime`
* **Example:** `10:16:57 up 5 days, 4:21, 2 users, load average: 0.15, 0.20, 0.25`
    * The load average values (0.15, 0.20, 0.25) correspond to the average number of jobs in the run queue over the past 1, 5, and 15 minutes, respectively.### 🧠 Memory Management Commands

Memory management is the process of managing the computer's memory. The primary tool for memory management in Linux is the **kernel's virtual memory subsystem**, which uses **swap space** to extend RAM. Here are the key commands to monitor and manage memory.

***

#### 1. `free`
The `free` command is the most straightforward way to see how much memory is available. It displays the total amount of free and used physical memory and swap memory.
* **Syntax:** `free [options]`
* **Example:** `free -h`
    * The `-h` option provides a **human-readable** output (e.g., `G` for gigabytes, `M` for megabytes).
    * The output shows `total`, `used`, and `free` memory, as well as `shared`, `buff/cache`, and `available` memory. The **`available`** memory is a key metric, as it's an estimate of how much memory is usable by new applications without swapping.

#### 2. `swapon` and `swapoff`
These commands are used to enable and disable a **swap device** or **swap file**.
* **Syntax:** `swapon [options] <device_name>` or `swapoff [options] <device_name>`
* **Example:** `sudo swapon /dev/sdb2`
    * This command enables the partition `/dev/sdb2` as swap space.
    * You need **root privileges** (`sudo`) to use these commands.
* **To check active swap:** `swapon --show` or `cat /proc/swaps`

#### 3. `vmstat`
The `vmstat` (virtual memory statistics) command provides detailed, real-time reports on the system's virtual memory, processes, CPU, and I/O.
* **Syntax:** `vmstat [delay] [count]`
* **Example:** `vmstat 1`
    * This command prints a report every second until you stop it with `Ctrl+C`.
    * **Key columns** to watch for are:
        * **`si` (swap in):** Amount of memory swapped in from disk.
        * **`so` (swap out):** Amount of memory swapped out to disk.
    * High values in the `si` and `so` columns indicate that the system is heavily swapping, which is a sign of memory pressure and can degrade performance.

#### 4. `/proc/meminfo`
This is not a command but a **virtual file** that provides extensive, low-level details about the kernel's memory management.
* **Example:** `cat /proc/meminfo`
    * This provides a wealth of information, including `MemTotal`, `MemFree`, `Buffers`, `Cached`, and `SwapTotal`, giving a more granular view than `free`.

***

### 💻 CPU Management Commands

CPU management is handled by the Linux kernel's **scheduler**, which decides which processes get CPU time. You can influence the scheduler's decisions using the following commands.

#### 1. `top` or `htop`
These commands are the primary tools for monitoring CPU usage in real time.
* **Syntax:** `top` or `htop`
* **Usage:**
    * They show a dynamic list of processes sorted by their CPU usage.
    * **`htop`** is an enhanced version of `top` with a more user-friendly interface that allows you to easily sort, filter, and kill processes.
    * The first line of the output displays the **load average** over the last 1, 5, and 15 minutes, which is a key indicator of system performance. 

#### 2. `nice` and `renice`
These commands are used to manage a process's **scheduling priority** by setting its **niceness value**. A lower niceness value corresponds to a higher priority for CPU time.
* **Syntax:**
    * `nice -n <value> <command>`
    * `renice <value> -p <PID>`
* **Example:**
    * `nice -n 10 gzip big_file.tar`
        * This runs the `gzip` command with a niceness of 10, meaning it will have a lower CPU priority and won't interfere with other tasks.
    * `renice -5 -p 1234`
        * This changes the niceness of a running process with PID 1234 to -5, giving it a higher CPU priority.

#### 3. `taskset`
The `taskset` command is used to set or retrieve a process's **CPU affinity**, which is the set of CPU cores on which the process is allowed to run. This is useful for optimizing the performance of multithreaded applications.
* **Syntax:** `taskset [options] <mask> [command]`
* **Example:** `taskset -c 0,1,2 my_app`
    * This command runs `my_app` and restricts its execution to **CPU cores 0, 1, and 2**. This can prevent cache misses and other overhead associated with a process jumping between cores.

#### 4. `uptime`
This simple command provides a quick summary of how long the system has been running and displays the **load average**.
* **Syntax:** `uptime`
* **Example:** `10:16:57 up 5 days, 4:21, 2 users, load average: 0.15, 0.20, 0.25`
    * The load average values (0.15, 0.20, 0.25) correspond to the average number of jobs in the run queue over the past 1, 5, and 15 minutes, respectively.
