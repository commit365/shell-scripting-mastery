# Debugging and Error Handling in Shell Scripting

Effective debugging and error handling are crucial skills for writing robust and maintainable shell scripts. This lesson covers debugging techniques, error handling strategies, exit codes, and logging practices.

## 1. Debugging Techniques

### 1.1 Using set -x

The `set -x` command enables debug mode, printing each command before it's executed:

```bash
#!/bin/bash
set -x
# Your script here
```

You can also enable it for a specific section:

```bash
set -x
# Debugging section
set +x
```

### 1.2 Using set -e

The `set -e` command causes the script to exit immediately if any command exits with a non-zero status:

```bash
#!/bin/bash
set -e
# Your script here
```

### 1.3 Using set -u

The `set -u` command treats unset variables as an error:

```bash
#!/bin/bash
set -u
# Your script here
```

### 1.4 Combining Options

You can combine these options:

```bash
#!/bin/bash
set -euxo pipefail
# Your script here
```

- `-e`: Exit on error
- `-u`: Treat unset variables as errors
- `-x`: Print commands before execution
- `-o pipefail`: Return value of a pipeline is the status of the last command to exit with a non-zero status

### 1.5 Debugging Functions

To debug a specific function:

```bash
function_name() {
    local old_opts=$-
    set -x
    # Function body
    set +x
    local exit_code=$?
    set -$old_opts
    return $exit_code
}
```

### 1.6 Using bashdb

bashdb is a bash debugger that provides features like breakpoints and step-by-step execution:

```bash
bashdb script.sh
```

### 1.7 Using PS4

Customize the debug output prefix:

```bash
export PS4='+(${BASH_SOURCE}:${LINENO}): ${FUNCNAME:+${FUNCNAME}(): }'
set -x
```

## 2. Error Handling and Exit Codes

### 2.1 Understanding Exit Codes

- Exit codes range from 0 to 255
- 0 typically means success
- Non-zero values indicate various error conditions

### 2.2 Setting Exit Codes

```bash
exit 0  # Success
exit 1  # General error
```

### 2.3 Checking Exit Codes

```bash
command
if [ $? -ne 0 ]; then
    echo "Command failed"
    exit 1
fi
```

### 2.4 Using trap for Error Handling

```bash
trap 'echo "Error on line $LINENO"; exit 1' ERR
```

### 2.5 Creating an Error Function

```bash
error() {
    local parent_lineno="$1"
    local message="$2"
    local code="${3:-1}"
    if [[ -n "$message" ]] ; then
        echo "Error on or near line ${parent_lineno}: ${message}; exiting with status ${code}"
    else
        echo "Error on or near line ${parent_lineno}; exiting with status ${code}"
    fi
    exit "${code}"
}
trap 'error ${LINENO}' ERR
```

### 2.6 Handling Errors in Pipes

Use `set -o pipefail` to catch errors in pipes:

```bash
set -o pipefail
command1 | command2 | command3
if [ $? -ne 0 ]; then
    echo "Pipeline failed"
fi
```

## 3. Logging and Verbose Mode

### 3.1 Basic Logging

```bash
log() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') $*" >> script.log
}

log "Starting script"
# Your script here
log "Ending script"
```

### 3.2 Logging Levels

```bash
LOG_LEVEL=INFO

log() {
    local level="$1"
    shift
    if [[ "$LOG_LEVEL" == "DEBUG" || "$level" != "DEBUG" ]]; then
        echo "$(date '+%Y-%m-%d %H:%M:%S') [$level] $*" >> script.log
    fi
}

log INFO "Starting script"
log DEBUG "Debug information"
log ERROR "An error occurred"
```

### 3.3 Verbose Mode

Implement a verbose mode flag:

```bash
verbose=0

while getopts "v" opt; do
    case $opt in
        v) verbose=1 ;;
    esac
done

log() {
    if [ $verbose -eq 1 ]; then
        echo "$@"
    fi
}

log "This will only print in verbose mode"
```

### 3.4 Redirecting Output

Redirect stdout and stderr to files:

```bash
exec > >(tee -a stdout.log) 2> >(tee -a stderr.log >&2)
```

### 3.5 Rotating Logs

Implement log rotation:

```bash
MAX_LOG_SIZE=1048576  # 1MB

check_log_size() {
    if [ -f "$1" ] && [ $(stat -c%s "$1") -gt $MAX_LOG_SIZE ]; then
        mv "$1" "$1.$(date +%Y%m%d-%H%M%S)"
    fi
}

log() {
    check_log_size "script.log"
    echo "$(date '+%Y-%m-%d %H:%M:%S') $*" >> script.log
}
```

## 4. Advanced Debugging Techniques

### 4.1 Using shellcheck

shellcheck is a static analysis tool for shell scripts:

```bash
shellcheck script.sh
```

### 4.2 Profiling with PS4

Profile your script by setting PS4:

```bash
PS4='+ $EPOCHREALTIME\011 '
exec 3>&2 2>/tmp/scriptprofile.$$$
set -x
# Your script here
set +x
exec 2>&3 3>&-
sort -n /tmp/scriptprofile.$$$
```

### 4.3 Using strace

Trace system calls and signals:

```bash
strace -f ./script.sh
```

### 4.4 Using declare -p

Print all variables and their values:

```bash
declare -p
```

### 4.5 Debugging Subshells

To debug subshells, use `set -x` before the subshell:

```bash
(
    set -x
    # Subshell commands
)
```

## Exercises

1. Write a script that demonstrates the use of `set -euxo pipefail` and explain how each option affects the script's behavior.

2. Create a script that implements a logging system with different log levels (DEBUG, INFO, WARNING, ERROR) and the ability to set the current log level.

3. Write a script that uses trap to handle various signals (SIGINT, SIGTERM, ERR) and performs appropriate cleanup actions.

4. Implement a debug function that prints the current line number, function name, and a custom message when called.

5. Create a script that demonstrates error handling in a pipeline, ensuring that errors in any part of the pipeline are caught and handled.

6. Write a script that includes verbose mode, activated by a command-line flag, which provides additional output for debugging purposes.

7. Implement a function that checks for the existence and permissions of required files/directories, logging appropriate error messages and exiting with proper exit codes if requirements are not met.

## Review Questions

1. Explain the difference between `set -e` and `set -o pipefail`. How do they complement each other in error handling?

2. What is the purpose of the `PS4` variable in bash, and how can it be used to enhance debugging output?

3. Describe the advantages and potential drawbacks of using `set -u` in your scripts.

4. How can you use `trap` to implement a cleanup function that runs regardless of how the script exits?

5. Explain the concept of exit codes in shell scripting. How are they typically used, and what are some common conventions?

6. What are the benefits of implementing different log levels in your scripts? How would you design a logging system to be both informative and manageable?

7. How does shellcheck differ from bash's built-in debugging tools, and what types of issues can it help identify?

8. Describe a scenario where you would use strace to debug a shell script. What kind of information does strace provide?

9. How can you effectively debug subshells and command substitutions in your scripts?

10. Explain the concept of log rotation. Why is it important, and how would you implement it in a long-running script?

This lesson covers fundamental and advanced concepts of debugging and error handling in shell scripting. The exercises and review questions are designed to reinforce these concepts and encourage practical application of the knowledge gained.

[Next: 12. Advanced Topics](./12_advanced_topics.md)