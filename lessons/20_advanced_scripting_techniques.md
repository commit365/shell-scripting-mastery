# Advanced Scripting Techniques

This lesson covers advanced techniques in shell scripting, including coprocesses and named pipes, process substitution, and scripting with heredocs. These techniques enhance the capabilities of your scripts and improve their efficiency and readability.

## 1. Coprocesses and Named Pipes

### 1.1 What are Coprocesses?

Coprocesses allow you to run a command in the background and communicate with it through a pipe. This enables asynchronous processing and can improve the performance of scripts that require concurrent execution.

#### 1.1.1 Creating a Coprocess

To create a coprocess, use the `|&` operator, which combines stdout and stderr:

```bash
coproc my_coproc {
    # Commands to run in the coprocess
    while read line; do
        echo "Received: $line"
    done
}
```

#### 1.1.2 Interacting with a Coprocess

You can send data to the coprocess using its file descriptors:

```bash
echo "Hello, coprocess!" >&"${my_coproc}"
```

To read output from the coprocess:

```bash
read -u "${my_coproc}" output
echo "Coprocess output: $output"
```

#### 1.1.3 Example of Using a Coprocess

Here’s an example of a coprocess that counts lines and words from input:

```bash
#!/bin/bash

coproc count_lines {
    while read line; do
        echo "$line" | wc -l
    done
}

echo -e "Line 1\nLine 2\nLine 3" >&"${count_lines}"
wait "$count_lines_PID"  # Wait for the coprocess to finish
```

### 1.2 Named Pipes (FIFOs)

Named pipes allow for inter-process communication by creating a special file that acts as a pipe.

#### 1.2.1 Creating a Named Pipe

Use the `mkfifo` command to create a named pipe:

```bash
mkfifo my_pipe
```

#### 1.2.2 Writing to and Reading from a Named Pipe

You can write to the named pipe in one terminal and read from it in another:

**Writing:**

```bash
echo "Hello, World!" > my_pipe
```

**Reading:**

```bash
cat my_pipe
```

#### 1.2.3 Example of Using a Named Pipe

Here’s a complete example:

```bash
#!/bin/bash

mkfifo my_pipe

# Writer
{
    for i in {1..5}; do
        echo "Message $i" > my_pipe
        sleep 1
    done
} &

# Reader
{
    while read line; do
        echo "Received: $line"
    done < my_pipe
}

wait
rm my_pipe  # Clean up
```

## 2. Process Substitution

### 2.1 What is Process Substitution?

Process substitution allows you to use the output of a command as if it were a file. This is useful for commands that require file arguments but you want to use the output of another command.

### 2.2 Syntax

Process substitution is done using `<(...)` for input and `>(...)` for output:

- **Input**: `<(command)`
- **Output**: `>(command)`

### 2.3 Example of Input Process Substitution

Here’s an example that compares the output of two commands:

```bash
diff <(ls dir1) <(ls dir2)
```

This command compares the contents of `dir1` and `dir2` without creating temporary files.

### 2.4 Example of Output Process Substitution

You can also use process substitution to direct output:

```bash
cat > >(sort > sorted_output.txt)
```

This command sorts the output of `cat` and writes it to `sorted_output.txt`.

### 2.5 Combining with Other Commands

Process substitution can be combined with other commands for powerful results:

```bash
paste <(ls dir1) <(ls dir2) > combined.txt
```

This command combines the output of two `ls` commands into a single file.

## 3. Scripting with Heredocs

### 3.1 What are Heredocs?

Heredocs allow you to create multi-line strings directly in your scripts. They are particularly useful for passing large blocks of text to commands or scripts.

### 3.2 Basic Syntax

The basic syntax for a heredoc is:

```bash
command <<EOF
line 1
line 2
...
EOF
```

### 3.3 Example of Using Heredocs

Here’s an example of using heredocs to create a configuration file:

```bash
#!/bin/bash

cat <<EOF > config.ini
[General]
app_name = MyApp
version = 1.0

[Database]
host = localhost
user = root
password = secret
EOF
```

### 3.4 Using Heredocs with Commands

You can pass heredocs to commands that accept input:

```bash
#!/bin/bash

mail -s "Subject" recipient@example.com <<EOF
Hello,

This is a test email sent from a shell script.

Best,
Your Name
EOF
```

### 3.5 Customizing Heredocs

You can customize heredocs by using different delimiters or enabling variable expansion:

```bash
name="World"
cat <<EOF
Hello, $name!
EOF
```

### 3.6 Disabling Variable Expansion

To disable variable expansion, use a quoted delimiter:

```bash
cat <<'EOF'
Hello, $name!
EOF
```

## Exercises

1. Write a script that uses a coprocess to monitor system load and log the output to a file.

2. Create a script that uses a named pipe to transfer data between two processes, demonstrating both reading and writing.

3. Implement a script that uses process substitution to compare the output of two different commands.

4. Write a script that generates a configuration file using heredocs, including user-defined variables.

5. Create a script that sends a multi-line message via email using heredocs.

## Review Questions

1. What are the advantages of using coprocesses in shell scripting? Provide an example scenario where they would be beneficial.

2. Explain how named pipes work and provide a use case where they would be preferred over traditional pipes.

3. Describe the differences between process substitution and traditional input/output redirection.

4. How can heredocs improve the readability and maintainability of your scripts? Give an example of its usage.

5. What considerations should you keep in mind when using heredocs with variable expansion?

6. Discuss potential pitfalls when using coprocesses and how to avoid them.

7. How can you ensure that a named pipe is properly cleaned up after use?

8. Explain how you would use process substitution in a script that requires comparing the outputs of two commands.

9. What are some best practices for organizing and structuring scripts that utilize advanced techniques like coprocesses and heredocs?

10. Describe a scenario where combining multiple advanced scripting techniques (e.g., coprocesses, named pipes, and heredocs) could lead to a more efficient solution.

This lesson covers advanced scripting techniques, including coprocesses, named pipes, process substitution, and heredocs. The exercises and review questions are designed to reinforce these concepts and encourage practical application of advanced techniques in shell scripting.

[Next: 21. Shell Scripting in the Real World](./21_shell_scripting_in_the_real_world.md)