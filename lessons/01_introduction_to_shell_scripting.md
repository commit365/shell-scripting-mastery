# Introduction to Shell Scripting

## 1. What is a Shell and Types of Shells

### 1.1 Definition of a Shell

A shell is a command-line interpreter that provides a user interface for interacting with an operating system's services and utilities. It acts as an intermediary between the user and the kernel, translating user commands into kernel-understandable instructions.

### 1.2 Functions of a Shell

- Command interpretation
- Script execution
- Environment variable management
- Input/output redirection
- Pipeline creation
- Wildcard expansion
- Command history management

### 1.3 Types of Shells

#### 1.3.1 Bourne Shell (sh)
- The original Unix shell, developed by Stephen Bourne
- Provides basic features like variables, control structures, and functions
- Still used as a default shell in many Unix-like systems

#### 1.3.2 Bourne Again Shell (bash)
- An enhanced version of the Bourne Shell
- Developed as part of the GNU Project
- Includes command-line editing, command history, and command completion
- Default shell for most Linux distributions and macOS (up to Catalina)

#### 1.3.3 Z Shell (zsh)
- Extended Bourne shell with improvements and features from bash, ksh, and tcsh
- Offers better customization, theming, and plugin support
- Default shell in macOS Catalina and later

#### 1.3.4 C Shell (csh) and TENEX C Shell (tcsh)
- Syntax similar to the C programming language
- Offers command-line editing and command history
- Less commonly used for scripting due to limitations

#### 1.3.5 Korn Shell (ksh)
- Combines features from the Bourne shell and the C shell
- Offers improved performance and additional programming features

### 1.4 Comparing Shells

| Feature | sh | bash | zsh | csh/tcsh | ksh |
|---------|----|----|-----|----------|-----|
| Script Compatibility | High | High | High | Low | High |
| Command-line Editing | No | Yes | Yes | Yes | Yes |
| Command History | No | Yes | Yes | Yes | Yes |
| Job Control | Limited | Yes | Yes | Yes | Yes |
| Array Support | No | Yes | Yes | Yes | Yes |
| Floating-point Arithmetic | No | No | Yes | No | Yes |
| Customization | Limited | Moderate | High | Moderate | Moderate |

## 2. Importance of Shell Scripting

### 2.1 Automation
- Automate repetitive tasks
- Batch processing of files and data
- Scheduled execution of maintenance tasks

### 2.2 System Administration
- User management
- System monitoring and reporting
- Backup and restore operations
- Software installation and updates

### 2.3 Development and Testing
- Build automation
- Continuous Integration/Continuous Deployment (CI/CD) pipelines
- Environment setup and teardown for testing

### 2.4 Data Processing
- Log file analysis
- Text manipulation and extraction
- Data transformation and formatting

### 2.5 Prototyping
- Quickly test ideas and concepts
- Iterate on solutions before implementing in other languages

### 2.6 Cross-platform Compatibility
- Scripts can run on various Unix-like systems with minimal modification
- Portable automation across different environments

### 2.7 Integration
- Glue different tools and utilities together
- Create complex workflows by combining simple commands

### 2.8 Learning and Skill Development
- Understanding of operating system concepts
- Improved command-line proficiency
- Foundation for learning other scripting languages

## 3. Setting Up Your Environment

### 3.1 Choosing a Shell

For this course, we'll focus on bash and zsh due to their widespread use and feature sets.

To check your current shell:
```bash
echo $SHELL
```

To change your default shell (example for zsh):
```bash
chsh -s $(which zsh)
```

### 3.2 Text Editors

Choose a text editor for writing scripts. Some popular options:

- Vim: Powerful, modal text editor available on most Unix-like systems
- Emacs: Extensible, customizable text editor
- Visual Studio Code: Modern, feature-rich editor with excellent shell script support
- Nano: Simple, easy-to-use editor for beginners

### 3.3 Setting Up Your Development Directory

Create a directory for your shell scripts:

```bash
mkdir ~/shellscripts
cd ~/shellscripts
```

### 3.4 Configuring Your Shell

#### 3.4.1 Bash Configuration

Edit `~/.bashrc` (for login shells, use `~/.bash_profile`):

```bash
# Add shellscripts directory to PATH
export PATH="$HOME/shellscripts:$PATH"

# Set a custom prompt
export PS1="\u@\h:\w\$ "

# Enable command-line editing
set -o emacs  # or 'set -o vi' for vi-style editing
```

#### 3.4.2 Zsh Configuration

Edit `~/.zshrc`:

```zsh
# Add shellscripts directory to PATH
export PATH="$HOME/shellscripts:$PATH"

# Set a custom prompt
PROMPT='%n@%m:%~%# '

# Enable command-line editing (usually enabled by default in zsh)
bindkey -e  # or 'bindkey -v' for vi-style editing

# Enable useful zsh features
autoload -Uz compinit && compinit  # Enhanced tab completion
setopt EXTENDED_HISTORY            # Save timestamp in history
setopt HIST_EXPIRE_DUPS_FIRST      # Expire duplicate entries first when trimming history
setopt HIST_IGNORE_DUPS            # Don't record an entry that was just recorded again
setopt HIST_IGNORE_ALL_DUPS        # Delete old recorded entry if new entry is a duplicate
```

### 3.5 Creating Your First Script

Create a new file called `hello.sh`:

```bash
#!/bin/bash
# This is a comment
echo "Hello, World!"
```

Make the script executable:

```bash
chmod +x hello.sh
```

Run the script:

```bash
./hello.sh
```

### 3.6 Version Control

Initialize a Git repository for your scripts:

```bash
git init
git add hello.sh
git commit -m "Add first shell script"
```

## Exercises

1. Determine which shell you're currently using and list its version.
2. Create a simple script that prints your current working directory and lists its contents.
3. Modify your shell's configuration file to add a custom alias for a frequently used command.
4. Write a script that accepts a command-line argument and echoes it back to the user.
5. Use `man` pages to explore three new shell built-in commands and experiment with them in your terminal.

## Review Questions

1. What is the primary function of a shell in an operating system?
2. Name three major differences between bash and zsh.
3. Why is shell scripting important for system administrators?
4. How can you make a shell script executable?
5. What is the purpose of the shebang (`#!/bin/bash`) at the beginning of a script?
6. How do you add a directory to your PATH in bash and zsh?
7. What are environment variables, and how are they useful in shell scripting?
8. Explain the difference between `.bashrc` and `.bash_profile`.
9. How does version control benefit shell script development?
10. What are some best practices for organizing and maintaining shell scripts?

This introduction provides a solid foundation for understanding shells, their importance, and how to set up a productive environment for shell scripting. The exercises and review questions will help reinforce the concepts and encourage hands-on practice.
