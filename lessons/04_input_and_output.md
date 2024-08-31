# Input and Output in Shell Scripting

## 1. Reading User Input and Command-line Arguments

### 1.1 Command-line Arguments

Command-line arguments are parameters passed to a script when it is executed.

#### 1.1.1 Accessing Command-line Arguments

- `$0`: Name of the script
- `$1`, `$2`, ...: Individual arguments
- `$#`: Number of arguments
- `$@`: All arguments as separate words
- `$*`: All arguments as a single word

Example:

```bash
#!/bin/bash

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "Number of arguments: $#"
echo "All arguments: $@"
```

#### 1.1.2 Shifting Arguments

The `shift` command moves the arguments to the left, discarding `$1`:

```bash
shift
# $2 becomes $1, $3 becomes $2, etc.
```

#### 1.1.3 Handling Optional Arguments

Use getopts for parsing short options:

```bash
while getopts ":a:b:" opt; do
  case $opt in
    a) a_arg="$OPTARG";;
    b) b_arg="$OPTARG";;
    \?) echo "Invalid option -$OPTARG" >&2;;
  esac
done
```

For long options, consider using a case statement:

```bash
for arg in "$@"; do
  case $arg in
    --option1=*)
    OPTION1="${arg#*=}"
    shift
    ;;
    --option2=*)
    OPTION2="${arg#*=}"
    shift
    ;;
  esac
done
```

### 1.2 Reading User Input

#### 1.2.1 Using read

The `read` command reads a line from standard input:

```bash
echo "Enter your name:"
read name
echo "Hello, $name!"
```

Options for `read`:
- `-p`: Specify a prompt
- `-s`: Silent mode (for passwords)
- `-t`: Specify a timeout
- `-n`: Limit the number of characters to read

Example:

```bash
read -p "Enter password: " -s -t 10 password
echo
echo "Password entered: $password"
```

#### 1.2.2 Reading Multiple Values

```bash
read -p "Enter first and last name: " first_name last_name
echo "First name: $first_name"
echo "Last name: $last_name"
```

#### 1.2.3 Reading from a File

```bash
while IFS= read -r line; do
  echo "Line: $line"
done < input.txt
```

## 2. Standard Input, Output, and Error

In Unix-like systems, there are three standard streams:

1. Standard Input (stdin) - File descriptor 0
2. Standard Output (stdout) - File descriptor 1
3. Standard Error (stderr) - File descriptor 2

### 2.1 Standard Input (stdin)

stdin is the default input stream. Many commands read from stdin if no file is specified.

Example using cat:

```bash
cat # Press Enter, then type. Press Ctrl+D to end input.
```

### 2.2 Standard Output (stdout)

stdout is the default output stream for a command's normal output.

Example:

```bash
echo "This goes to stdout"
```

### 2.3 Standard Error (stderr)

stderr is used for error messages and diagnostic output.

Example:

```bash
ls non_existent_file # This will print an error to stderr
```

## 3. Redirecting and Piping

### 3.1 Output Redirection

#### 3.1.1 Redirecting stdout

Use `>` to redirect stdout to a file (overwrite):

```bash
echo "Hello, World!" > output.txt
```

Use `>>` to append stdout to a file:

```bash
echo "Another line" >> output.txt
```

#### 3.1.2 Redirecting stderr

Use `2>` to redirect stderr to a file:

```bash
ls non_existent_file 2> error.log
```

#### 3.1.3 Redirecting both stdout and stderr

To the same file:

```bash
command > output.txt 2>&1
```

To different files:

```bash
command > output.txt 2> error.log
```

### 3.2 Input Redirection

Use `<` to redirect input from a file:

```bash
sort < unsorted.txt
```

### 3.3 Here Documents

A here document allows you to pass multiple lines of input to a command:

```bash
cat << EOF > output.txt
Line 1
Line 2
Line 3
EOF
```

### 3.4 Here Strings

A here string passes a single line as input to a command:

```bash
grep "pattern" <<< "This is the input string"
```

### 3.5 Piping

Piping (`|`) connects the stdout of one command to the stdin of another:

```bash
ls -l | grep "\.txt$"
```

#### 3.5.1 Named Pipes (FIFOs)

Create a named pipe:

```bash
mkfifo mypipe
```

Use the named pipe:

```bash
# Terminal 1
echo "Hello, Pipe!" > mypipe

# Terminal 2
cat < mypipe
```

### 3.6 Process Substitution

Process substitution allows you to use the output of a command as a file:

```bash
diff <(ls dir1) <(ls dir2)
```

### 3.7 tee Command

The `tee` command reads from stdin and writes to both stdout and files:

```bash
echo "Hello" | tee output.txt
```

## Exercises

1. Write a script that accepts a filename as a command-line argument and counts the number of lines, words, and characters in the file.

2. Create a script that prompts the user for their name and age, then writes this information to a file.

3. Write a script that reads a list of numbers from a file (one per line) and calculates their sum and average.

4. Create a script that takes command-line options for input and output files, reads the input file, converts all text to uppercase, and writes the result to the output file.

5. Write a script that uses a here document to generate a simple HTML file, prompting the user for a title and body content.

6. Create a pipeline that finds all `.log` files in the current directory and its subdirectories, sorts them by size, and displays the top 5 largest files.

7. Write a script that demonstrates the use of named pipes to communicate between two separate shell sessions.

## Review Questions

1. What is the difference between `$@` and `$*` when referring to command-line arguments?

2. How can you securely read a password from the user without displaying it on the screen?

3. Explain the difference between `>` and `>>` when redirecting output to a file.

4. How would you redirect both stdout and stderr to the same file?

5. What is the purpose of the `IFS` variable when reading input, and how can it be used?

6. Describe the difference between a here document and a here string.

7. How does process substitution differ from regular piping, and when might you use it?

8. What is the purpose of the `tee` command, and how can it be useful in scripts?

9. How can you use named pipes (FIFOs) for inter-process communication?

10. Explain how you would implement a simple logging system in a shell script, writing different types of messages to different output streams.

This lesson covers the fundamental concepts of input and output in shell scripting, including reading user input, handling command-line arguments, working with standard streams, and using various redirection and piping techniques. The exercises and review questions will help reinforce these concepts and encourage practical application of the knowledge gained.