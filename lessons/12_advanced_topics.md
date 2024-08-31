# Advanced Topics in Shell Scripting

This lesson covers advanced shell scripting techniques, including subshells and command grouping, brace expansion and parameter expansion, and command-line option parsing. These topics will help you write more sophisticated and efficient shell scripts.

## 1. Subshells and Command Grouping

### 1.1 Subshells

A subshell is a child process launched by a shell script or command line.

#### 1.1.1 Creating Subshells

Subshells are created by enclosing commands in parentheses:

```bash
(command1; command2; command3)
```

#### 1.1.2 Subshell Characteristics

- Subshells inherit environment variables but cannot modify the parent's environment.
- Changes to the current working directory in a subshell do not affect the parent shell.

Example:

```bash
echo "Current directory: $PWD"
(cd /tmp; echo "Subshell directory: $PWD")
echo "Back in: $PWD"
```

#### 1.1.3 Capturing Subshell Output

Use command substitution to capture subshell output:

```bash
result=$(command1; command2)
```

### 1.2 Command Grouping

Command grouping executes a set of commands in the current shell environment.

#### 1.2.1 Using Curly Braces

Group commands using curly braces:

```bash
{ command1; command2; command3; }
```

Note: The spaces and semicolon are required.

#### 1.2.2 Differences from Subshells

- Grouped commands execute in the current shell, affecting variables and the working directory.
- Grouped commands are slightly more efficient than subshells.

Example:

```bash
echo "Before grouping: $var"
{ var="new value"; echo "Inside grouping: $var"; }
echo "After grouping: $var"
```

### 1.3 Process Substitution

Process substitution allows the output of a process to be treated as a file:

```bash
command <(command1) <(command2)
```

Example:

```bash
diff <(ls dir1) <(ls dir2)
```

## 2. Brace Expansion and Parameter Expansion

### 2.1 Brace Expansion

Brace expansion generates arbitrary strings.

#### 2.1.1 Range Expansion

```bash
echo {1..5}      # Output: 1 2 3 4 5
echo {a..e}      # Output: a b c d e
echo {1..10..2}  # Output: 1 3 5 7 9
```

#### 2.1.2 String Expansion

```bash
echo {apple,banana,cherry}      # Output: apple banana cherry
echo {A..C}{1..3}               # Output: A1 A2 A3 B1 B2 B3 C1 C2 C3
```

#### 2.1.3 Nested Brace Expansion

```bash
echo {A,B{1,2},C}  # Output: A B1 B2 C
```

### 2.2 Parameter Expansion

Parameter expansion allows manipulation of variable values.

#### 2.2.1 Basic Expansion

```bash
var="Hello"
echo ${var}         # Output: Hello
echo ${var}world    # Output: Helloworld
```

#### 2.2.2 Default Values

```bash
echo ${var:-default}   # Use default if var is unset or null
echo ${var:=default}   # Assign default if var is unset or null
echo ${var:+alternate} # Use alternate if var is set and not null
echo ${var:?error}     # Display error if var is unset or null
```

#### 2.2.3 Substring Extraction

```bash
var="Hello, World!"
echo ${var:7}      # Output: World!
echo ${var:7:5}    # Output: World
```

#### 2.2.4 String Manipulation

```bash
var="Hello, World!"
echo ${var#Hello, }   # Remove shortest match from start
echo ${var##*o}       # Remove longest match from start
echo ${var%World!}    # Remove shortest match from end
echo ${var%%o*}       # Remove longest match from end
```

#### 2.2.5 Search and Replace

```bash
var="Hello, World!"
echo ${var/o/O}       # Replace first occurrence
echo ${var//o/O}      # Replace all occurrences
echo ${var/#Hello/Hi} # Replace at beginning
echo ${var/%World/Everyone} # Replace at end
```

#### 2.2.6 Length of Variable

```bash
var="Hello"
echo ${#var}  # Output: 5
```

## 3. Command-line Option Parsing

### 3.1 Using getopts

The `getopts` builtin command is used for parsing command-line options.

#### 3.1.1 Basic Usage

```bash
while getopts ":a:bc" opt; do
  case $opt in
    a) echo "Option -a with value $OPTARG" ;;
    b) echo "Option -b" ;;
    c) echo "Option -c" ;;
    \?) echo "Invalid option: -$OPTARG" ;;
    :) echo "Option -$OPTARG requires an argument" ;;
  esac
done
```

#### 3.1.2 Shifting Processed Options

After processing options, shift them off:

```bash
shift $((OPTIND-1))
```

### 3.2 Manual Option Parsing

For more complex option parsing, you can use a manual approach:

```bash
while [[ $# -gt 0 ]]; do
  case $1 in
    --option1)
      OPTION1="$2"
      shift 2
      ;;
    --option2=*)
      OPTION2="${1#*=}"
      shift
      ;;
    --flag)
      FLAG=true
      shift
      ;;
    *)
      echo "Unknown option: $1"
      exit 1
      ;;
  esac
done
```

### 3.3 Using getopt

For more advanced option parsing, especially for long options, use the `getopt` command:

```bash
TEMP=$(getopt -o 'ab:c::' --long 'alpha,beta:,gamma::' -n 'example.sh' -- "$@")

if [ $? -ne 0 ]; then
    echo 'Terminating...' >&2
    exit 1
fi

eval set -- "$TEMP"
unset TEMP

while true; do
    case "$1" in
        '-a'|'--alpha')
            echo 'Option a'
            shift
            continue
            ;;
        '-b'|'--beta')
            echo "Option b, argument '$2'"
            shift 2
            continue
            ;;
        '-c'|'--gamma')
            case "$2" in
                '')
                    echo 'Option c, no argument'
                    shift 2
                    continue
                    ;;
                *)
                    echo "Option c, argument '$2'"
                    shift 2
                    continue
                    ;;
            esac
            ;;
        '--')
            shift
            break
            ;;
        *)
            echo 'Internal error!' >&2
            exit 1
            ;;
    esac
done
```

## Exercises

1. Write a script that uses subshells to perform parallel processing of a list of files, applying a specific operation to each file simultaneously.

2. Create a script that demonstrates the difference between subshells and command grouping by modifying variables and changing directories.

3. Write a script that uses brace expansion to generate a series of backup file names with timestamps.

4. Implement a function that uses parameter expansion to sanitize and validate user input, providing default values and error messages as appropriate.

5. Create a script that uses `getopts` to parse command-line options, including both short and long options, with some options requiring arguments.

6. Write a script that uses manual option parsing to handle a mix of short options, long options, and positional arguments.

7. Implement a script that uses process substitution to compare the output of two different commands or scripts.

## Review Questions

1. Explain the key differences between subshells and command grouping. In what scenarios would you prefer one over the other?

2. How does brace expansion differ from parameter expansion? Provide examples of when each would be most useful.

3. What are the advantages and limitations of using `getopts` for command-line option parsing?

4. Describe the process of implementing manual option parsing. What are its advantages over `getopts`?

5. How can you use parameter expansion to provide default values for variables? Give examples of different methods.

6. Explain the concept of process substitution and provide a real-world scenario where it would be particularly useful.

7. How would you use brace expansion to generate a sequence of file names or directory structures efficiently?

8. Describe the differences between `${var#pattern}`, `${var##pattern}`, `${var%pattern}`, and `${var%%pattern}` in parameter expansion.

9. How can you use subshells to run multiple background processes simultaneously while capturing their outputs?

10. Explain the purpose and usage of the `shift` command in the context of command-line option parsing.

This lesson covers advanced topics in shell scripting, including subshells and command grouping, brace expansion and parameter expansion, and command-line option parsing. The exercises and review questions are designed to reinforce these concepts and encourage practical application of the knowledge gained.

[Next: 13. Best Practices and Optimization](./13_best_practices_and_optimization.md)