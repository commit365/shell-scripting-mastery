# Building a Complex Project in Shell Scripting

This lesson focuses on developing a larger, more complex shell scripting project. We'll cover planning and structuring a larger script, combining multiple scripts and functions, and creating a command-line tool.

## 1. Planning and Structuring a Larger Script

### 1.1 Project Planning

Before writing any code, it's crucial to plan your project:

1. Define the project's purpose and goals
2. Identify the main features and functionalities
3. Break down the project into smaller, manageable tasks
4. Determine the project's structure and organization
5. Choose appropriate naming conventions
6. Plan for error handling and logging
7. Consider performance and scalability

### 1.2 Project Structure

A well-organized project structure might look like this:

```
project_root/
├── bin/
│   └── main_script.sh
├── lib/
│   ├── module1.sh
│   ├── module2.sh
│   └── module3.sh
├── config/
│   └── config.ini
├── data/
│   └── sample_data.txt
├── logs/
│   └── .gitkeep
├── tests/
│   ├── test_module1.sh
│   ├── test_module2.sh
│   └── test_module3.sh
├── docs/
│   └── README.md
├── .gitignore
└── LICENSE
```

### 1.3 Main Script Structure

The main script (`bin/main_script.sh`) should have a clear structure:

```bash
#!/bin/bash

# Script metadata
VERSION="1.0.0"
AUTHOR="Your Name"

# Source libraries and modules
source "$(dirname "$0")/../lib/module1.sh"
source "$(dirname "$0")/../lib/module2.sh"
source "$(dirname "$0")/../lib/module3.sh"

# Global variables
CONFIG_FILE="$(dirname "$0")/../config/config.ini"
LOG_FILE="$(dirname "$0")/../logs/script.log"

# Function definitions
usage() {
    echo "Usage: $0 [options] <argument>"
    # ... more usage information
}

parse_arguments() {
    # Parse command-line arguments
}

main() {
    # Main logic of the script
}

# Script execution
if [[ "${BASH_SOURCE}" == "${0}" ]]; then
    parse_arguments "$@"
    main
fi
```

### 1.4 Configuration Management

Use a configuration file (`config/config.ini`) for settings:

```ini
[General]
debug_mode=false
max_threads=4

[Database]
host=localhost
port=5432
username=user
password=pass
```

Read the configuration in your script:

```bash
parse_config() {
    while IFS='=' read -r key value; do
        if [[ $key == $$*] ]]; then
            section="${key:1:-1}"
        elif [[ $value ]]; then
            declare -g "${section}_${key}=$value"
        fi
    done < "$CONFIG_FILE"
}
```

### 1.5 Logging

Implement a robust logging system:

```bash
log() {
    local level="$1"
    shift
    echo "$(date '+%Y-%m-%d %H:%M:%S') [$level] $*" >> "$LOG_FILE"
}

log_info() { log "INFO" "$@"; }
log_warn() { log "WARN" "$@"; }
log_error() { log "ERROR" "$@"; }
```

## 2. Combining Multiple Scripts and Functions

### 2.1 Modularization

Break your project into logical modules. Each module (`lib/module1.sh`, `lib/module2.sh`, etc.) should contain related functions:

```bash
#!/bin/bash
# lib/module1.sh

module1_function1() {
    # Function implementation
}

module1_function2() {
    # Function implementation
}
```

### 2.2 Sourcing Modules

Source your modules in the main script:

```bash
source "$(dirname "$0")/../lib/module1.sh"
source "$(dirname "$0")/../lib/module2.sh"
```

### 2.3 Namespace Management

To avoid naming conflicts, use prefixes for your functions:

```bash
module1_process_data() {
    # Implementation
}

module2_process_data() {
    # Different implementation
}
```

### 2.4 Dependency Management

Manage dependencies between modules:

```bash
#!/bin/bash
# lib/module2.sh

source "$(dirname "$0")/module1.sh"

module2_function() {
    module1_function1
    # Additional logic
}
```

### 2.5 Shared Utilities

Create a shared utilities module for common functions:

```bash
#!/bin/bash
# lib/utils.sh

is_number() {
    [[ "$1" =~ ^[0-9]+$ ]]
}

trim() {
    local var="$*"
    var="${var#"${var%%[![:space:]]*}"}"
    var="${var%"${var##*[![:space:]]}"}"
    printf '%s' "$var"
}
```

## 3. Creating a Command-Line Tool

### 3.1 Argument Parsing

Use `getopts` for parsing command-line arguments:

```bash
parse_arguments() {
    while getopts ":hv" opt; do
        case ${opt} in
            h )
                usage
                exit 0
                ;;
            v )
                echo "Version: $VERSION"
                exit 0
                ;;
            \? )
                echo "Invalid Option: -$OPTARG" 1>&2
                usage
                exit 1
                ;;
        esac
    done
    shift $((OPTIND -1))
}
```

### 3.2 Subcommands

Implement subcommands for complex tools:

```bash
main() {
    local subcommand="$1"
    shift

    case "$subcommand" in
        init)
            initialize_project "$@"
            ;;
        run)
            run_project "$@"
            ;;
        clean)
            clean_project "$@"
            ;;
        *)
            echo "Unknown subcommand: $subcommand" >&2
            usage
            exit 1
            ;;
    esac
}
```

### 3.3 Help and Documentation

Provide comprehensive help:

```bash
usage() {
    cat << EOF
Usage: $(basename "$0") [options] <command> [<args>]

Options:
  -h    Show this help message and exit
  -v    Show version information and exit

Commands:
  init   Initialize a new project
  run    Run the project
  clean  Clean up project files

Run '$(basename "$0") <command> -h' for more information on a command.
EOF
}
```

### 3.4 Input Validation

Validate user input thoroughly:

```bash
validate_input() {
    local input="$1"
    if [[ -z "$input" ]]; then
        log_error "Input cannot be empty"
        return 1
    fi
    if ! is_number "$input"; then
        log_error "Input must be a number"
        return 1
    fi
    return 0
}
```

### 3.5 Progress Indication

For long-running operations, provide progress indication:

```bash
show_progress() {
    local duration="$1"
    local sleep_interval=0.1
    local progress=0
    local bar_size=40

    while [ $progress -le 100 ]; do
        local filled=$(( progress * bar_size / 100 ))
        local empty=$(( bar_size - filled ))
        printf "\rProgress: [%s%s] %d%%" "$(printf "#%.0s" $(seq 1 $filled))" "$(printf " %.0s" $(seq 1 $empty))" "$progress"
        progress=$(( progress + 1 ))
        sleep "$sleep_interval"
    done
    echo
}
```

### 3.6 Error Handling

Implement robust error handling:

```bash
set -e
trap 'handle_error $? $LINENO' ERR

handle_error() {
    local exit_code="$1"
    local line_number="$2"
    log_error "An error occurred on line $line_number with exit code $exit_code"
    exit "$exit_code"
}
```

### 3.7 Testing

Create unit tests for your functions:

```bash
#!/bin/bash
# tests/test_module1.sh

source "$(dirname "$0")/../lib/module1.sh"

test_module1_function1() {
    local result=$(module1_function1 "test input")
    if [[ "$result" != "expected output" ]]; then
        echo "FAIL: module1_function1 did not return expected output"
        return 1
    fi
    echo "PASS: module1_function1"
}

run_tests() {
    test_module1_function1
}

run_tests
```

## Exercises

1. Design and implement a complex shell script project for managing a virtual server environment. Include features like creating, starting, stopping, and deleting virtual machines, as well as monitoring resource usage.

2. Create a modular backup system that can handle different types of backups (full, incremental, differential) for various data sources (files, databases). Implement a command-line interface for managing backups.

3. Develop a log analysis tool that can process logs from multiple sources, filter based on various criteria, and generate reports. Include options for real-time monitoring and alerting.

4. Build a comprehensive system health monitoring tool that checks various aspects of system performance, security, and integrity. Implement a plugin system to allow for easy extension of monitoring capabilities.

5. Create a deployment automation tool that can manage the deployment of a complex application across multiple servers. Include features like rollback, version management, and deployment scheduling.

## Review Questions

1. What are the key considerations when planning a large shell scripting project? How do these differ from smaller scripts?

2. Explain the benefits of modularizing a shell script project. What are some potential drawbacks, and how can they be mitigated?

3. Describe different strategies for managing configuration in a complex shell script project. What are the pros and cons of each approach?

4. How would you implement a plugin system in a shell script project? What are the challenges involved?

5. Discuss the importance of proper error handling and logging in a large project. How would you implement a robust error handling system?

6. Explain the concept of "separation of concerns" in the context of shell scripting. How can this principle be applied to improve project structure?

7. What strategies can be employed to ensure good performance in a large shell script project, especially when dealing with large datasets or complex operations?

8. How would you approach testing a complex shell script project? Discuss different types of tests that might be relevant and how they could be implemented.

9. Describe the process of creating a user-friendly command-line interface for a complex tool. What features and considerations are important?

10. How can version control systems like Git be effectively used in the development of a large shell scripting project? Discuss branching strategies and collaboration workflows.

This lesson covers advanced concepts in building complex shell scripting projects, including project planning and structure, modularization, and creating sophisticated command-line tools. The exercises and review questions are designed to challenge your understanding and encourage the application of these concepts in real-world scenarios.

[Next: 18. Bash vs Zsh](./18_bash_vs_zsh.md)