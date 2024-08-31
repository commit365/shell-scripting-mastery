# Shell Scripting Fundamentals

## 1. Creating and Running Your First Script

### 1.1 Script Structure

A shell script typically consists of the following elements:

1. Shebang (#!/bin/bash or #!/bin/zsh)
2. Comments
3. Commands
4. Variables
5. Control structures
6. Functions (optional)

### 1.2 Creating a Script

To create a script:

1. Open a text editor (e.g., nano, vim, or emacs)
2. Start with a shebang
3. Add comments and commands
4. Save the file with a .sh extension (optional, but recommended)

Example:

```bash
#!/bin/bash

# This is a comment
echo "Hello, World!"
```

### 1.3 Making a Script Executable

To make a script executable:

```bash
chmod +x script_name.sh
```

### 1.4 Running a Script

There are several ways to run a script:

1. Execute directly:
   ```bash
   ./script_name.sh
   ```

2. Use bash or zsh explicitly:
   ```bash
   bash script_name.sh
   zsh script_name.sh
   ```

3. Source the script:
   ```bash
   source script_name.sh
   # or
   . script_name.sh
   ```

### 1.5 Script Exit Status

Every command, including scripts, returns an exit status (0-255):
- 0 typically means success
- Non-zero values indicate various error conditions

To check the exit status of the last command:
```bash
echo $?
```

To set a custom exit status in your script:
```bash
exit 0  # Success
exit 1  # General error
```

## 2. Variables and Data Types

### 2.1 Variable Declaration and Assignment

In shell scripting, variables are typically untyped. To declare and assign a variable:

```bash
variable_name=value
```

Note: There should be no spaces around the equals sign.

Examples:
```bash
name="John Doe"
age=30
pi=3.14
```

### 2.2 Variable Usage

To use a variable, prefix it with a dollar sign ($):

```bash
echo "Hello, $name!"
echo "You are $age years old."
```

For more complex expressions, use curly braces:

```bash
echo "The file is ${filename}_backup.txt"
```

### 2.3 Variable Scope

By default, variables are global within a script. To limit a variable's scope to a function, use the `local` keyword:

```bash
function example_function() {
    local local_var="I'm local"
    echo $local_var
}
```

### 2.4 Environment Variables

Environment variables are inherited from the parent process. Some common ones:

- PATH: Directories searched for commands
- HOME: User's home directory
- USER: Current username

To set an environment variable:
```bash
export MY_VAR="some value"
```

### 2.5 Special Variables

Shell scripts have several special variables:

- $0: Script name
- $1, $2, ...: Positional parameters
- $#: Number of positional parameters
- $@: All positional parameters (preserving whitespace and quoting)
- $*: All positional parameters (as a single word)
- $$: Process ID of the current shell
- $?: Exit status of the last command

### 2.6 Arrays

Bash and Zsh support indexed and associative arrays.

Indexed arrays:
```bash
fruits=("apple" "banana" "cherry")
echo ${fruits}  # apple
echo ${fruits[@]}  # all elements
echo ${#fruits[@]}  # array length
```

Associative arrays (Bash 4+ and Zsh):
```bash
declare -A user
user[name]="John"
user[age]=30
echo ${user[name]}  # John
```

### 2.7 Arithmetic Operations

For arithmetic operations, use the `(( ))` construct:

```bash
a=5
b=3
((c = a + b))
echo $c  # 8

((a++))
echo $a  # 6
```

For floating-point arithmetic, use `bc`:

```bash
result=$(echo "scale=2; 5 / 3" | bc)
echo $result  # 1.66
```

## 3. Command Substitution and Quoting

### 3.1 Command Substitution

Command substitution allows you to use the output of a command as part of another command or assignment.

There are two syntaxes:
1. Backticks (legacy): `` `command` ``
2. Parentheses (preferred): `$(command)`

Examples:
```bash
current_date=$(date +%Y-%m-%d)
echo "Today is $current_date"

files_count=$(ls | wc -l)
echo "This directory contains $files_count files"
```

### 3.2 Quoting

Quoting is crucial in shell scripting to control word splitting and variable expansion.

#### 3.2.1 Single Quotes ('')

Single quotes preserve the literal value of each character within the quotes:

```bash
echo 'The variable $name has the value "$name"'
# Output: The variable $name has the value "$name"
```

#### 3.2.2 Double Quotes ("")

Double quotes preserve the literal value of all characters except $, `, \, and (in some cases) !:

```bash
name="John"
echo "Hello, $name!"
# Output: Hello, John!
```

#### 3.2.3 Escaping Characters

Use backslash (\) to escape special characters:

```bash
echo "The price is \$10"
# Output: The price is $10
```

### 3.3 Here Documents

Here documents allow you to pass multiple lines of input to a command:

```bash
cat << EOF > output.txt
This is line 1
This is line 2
EOF
```

### 3.4 Command Grouping

Group commands using parentheses or curly braces:

```bash
# Subshell (executes in a separate process)
(cd /tmp && echo "Current directory: $PWD")

# Current shell
{ echo "Start"; echo "End"; }
```

## Exercises

1. Write a script that accepts a name as a command-line argument and prints a personalized greeting.

2. Create a script that performs basic arithmetic operations (+, -, *, /) on two numbers provided as command-line arguments.

3. Write a script that creates a backup of a specified file, adding the current date to the filename.

4. Create an array of colors and write a script that randomly selects and prints a color.

5. Write a script that counts the number of files in a directory provided as an argument, using command substitution.

6. Create a script that uses a here document to generate a simple HTML file.

7. Write a script that demonstrates the difference between single and double quotes when dealing with variables and command substitution.

## Review Questions

1. What is the purpose of the shebang in a shell script?

2. How do you make a shell script executable, and why is this necessary?

3. Explain the difference between sourcing a script and executing it directly.

4. What is the significance of the exit status in shell scripting?

5. How do you declare and use local variables in a shell script?

6. What is the difference between $@ and $* when referring to script arguments?

7. Explain the difference between indexed and associative arrays in shell scripting.

8. How do you perform floating-point arithmetic in a shell script?

9. What are the advantages of using $() for command substitution over backticks?

10. Describe the differences between single quotes, double quotes, and escaping characters in shell scripting.

This lesson covers the fundamental concepts of shell scripting, including script creation and execution, variable usage, and command substitution. The exercises and review questions will help reinforce these concepts and encourage practical application of the knowledge gained.

[Next: 04. Input and Output](./04_input_and_output.md)