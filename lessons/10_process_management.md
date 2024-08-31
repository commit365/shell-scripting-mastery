# Process Management in Shell Scripting

Process management is a crucial aspect of shell scripting and system administration. This lesson covers understanding processes, job control, background processes, and handling signals and traps.

## 1. Understanding Processes

### 1.1 What is a Process?

A process is an instance of a running program. Each process has:
- A unique Process ID (PID)
- A parent process (PPID)
- Associated resources (memory, open files, etc.)
- An execution state

### 1.2 Process States

Processes can be in various states:
- Running: Currently executing on the CPU
- Sleeping: Waiting for a resource or event
- Stopped: Suspended and not running
- Zombie: Terminated but not yet cleaned up by the parent

### 1.3 Viewing Processes

#### 1.3.1 ps Command

The `ps` command shows process information:

```bash
ps aux  # Show all processes for all users
ps -ef  # Full-format listing
ps -u username  # Show processes for a specific user
```

#### 1.3.2 top Command

`top` provides a real-time, dynamic view of processes:

```bash
top
```

### 1.4 Process Hierarchy

Processes form a hierarchy:
- Init (PID 1) is the root of the process tree
- Each process (except init) has a parent process
- Processes can spawn child processes

To view the process tree:

```bash
pstree
```

### 1.5 Process Priority

Process priority is managed by the nice value:
- Range: -20 (highest priority) to 19 (lowest priority)
- Default: 0

To change priority:

```bash
nice -n 10 command  # Start a command with lower priority
renice -n -5 -p PID  # Change priority of a running process
```

## 2. Job Control and Background Processes

### 2.1 Foreground and Background Jobs

- Foreground: Process that has control of the terminal
- Background: Process running without terminal control

### 2.2 Running Jobs in the Background

To start a job in the background, append `&`:

```bash
long_running_command &
```

### 2.3 Job Control Commands

- `jobs`: List background jobs
- `fg`: Bring a job to the foreground
- `bg`: Resume a stopped job in the background
- `Ctrl+Z`: Suspend the current foreground job

Example:

```bash
sleep 100 &  # Start a background job
jobs  # List jobs
fg %1  # Bring job 1 to foreground
Ctrl+Z  # Suspend the job
bg %1  # Resume the job in the background
```

### 2.4 Disowning Processes

To keep a process running after logging out:

```bash
nohup command &  # Immune to hangups
disown %1  # Disown job 1
```

### 2.5 Wait Command

Wait for background jobs to finish:

```bash
wait  # Wait for all background jobs
wait %1  # Wait for job 1
wait PID  # Wait for a specific PID
```

## 3. Signals and Traps

### 3.1 Understanding Signals

Signals are software interrupts sent to a process to indicate that an event has occurred.

Common signals:
- SIGTERM (15): Termination request
- SIGKILL (9): Forceful termination
- SIGINT (2): Interrupt (usually Ctrl+C)
- SIGHUP (1): Hangup
- SIGSTOP (19): Stop the process
- SIGCONT (18): Continue a stopped process

### 3.2 Sending Signals

Use the `kill` command to send signals:

```bash
kill PID  # Send SIGTERM
kill -9 PID  # Send SIGKILL
killall process_name  # Kill all processes with this name
```

### 3.3 Trapping Signals

Use the `trap` command to catch and handle signals:

```bash
trap 'command' SIGNAL
```

Example:

```bash
#!/bin/bash

cleanup() {
    echo "Cleaning up..."
    # Add cleanup code here
}

trap cleanup EXIT

# Rest of the script
```

### 3.4 Ignoring Signals

To ignore a signal:

```bash
trap '' SIGINT
```

### 3.5 Resetting Traps

To reset a trap to its default behavior:

```bash
trap - SIGINT
```

### 3.6 Trapping Multiple Signals

```bash
trap 'echo "Caught signal"; exit 1' SIGINT SIGTERM
```

### 3.7 Timeout Command

Use `timeout` to run a command with a time limit:

```bash
timeout 10s command  # Run for a maximum of 10 seconds
```

## 4. Advanced Process Management Techniques

### 4.1 Process Substitution

Use process substitution to treat command output as a file:

```bash
diff <(ls dir1) <(ls dir2)
```

### 4.2 Named Pipes (FIFOs)

Create and use named pipes for inter-process communication:

```bash
mkfifo mypipe
command1 > mypipe &
command2 < mypipe
```

### 4.3 /proc Filesystem

Explore the `/proc` filesystem for detailed process information:

```bash
cat /proc/PID/status
```

### 4.4 Cgroups

Use cgroups to limit and allocate resources to processes:

```bash
cgcreate -g cpu:/mygroup
cgexec -g cpu:mygroup command
```

### 4.5 Monitoring with pidstat

Use `pidstat` for detailed per-process statistics:

```bash
pidstat -p PID 1 5  # Monitor PID every 1 second for 5 iterations
```

## Exercises

1. Write a script that starts three background processes, waits for them to complete, and reports their exit statuses.

2. Create a script that uses `trap` to catch SIGINT and SIGTERM, performing cleanup operations before exiting.

3. Write a script that monitors the CPU usage of a specific process and terminates it if it exceeds a certain threshold.

4. Create a script that demonstrates the use of nice and renice to adjust process priorities.

5. Write a script that uses a named pipe to communicate between two separate shell scripts.

6. Create a script that spawns multiple background processes and uses job control commands to manage them.

7. Write a script that demonstrates the difference between SIGTERM and SIGKILL when attempting to terminate a process.

## Review Questions

1. Explain the difference between a foreground and a background process. How can you switch a process between these states?

2. What is the purpose of the `nohup` command, and how does it differ from using `&` to run a process in the background?

3. Describe the lifecycle of a typical process, from creation to termination.

4. How does the `nice` value affect process scheduling? What are the limitations of using `nice` for non-root users?

5. Explain the difference between SIGTERM and SIGKILL. Why might you choose one over the other?

6. What is a zombie process, and how can they be prevented or removed?

7. How can you use the `trap` command to make your scripts more robust and handle unexpected terminations?

8. Describe the purpose and functionality of the `/proc` filesystem in relation to process management.

9. How can you use process substitution to simplify complex command pipelines?

10. Explain how cgroups can be used to manage system resources for groups of processes.

This lesson covers fundamental and advanced concepts of process management in shell scripting and Unix-like systems. The exercises and review questions are designed to reinforce these concepts and encourage practical application of the knowledge gained.

[Next: 11. Debugging and Error Handling](./11_debugging_and_error_handling.md)