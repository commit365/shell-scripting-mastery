# Basic Command Line Usage

## 1. Navigating the File System

### 1.1 Understanding the File System Hierarchy

The Unix-like file system is organized in a hierarchical tree structure, starting from the root directory (/). Key directories include:

- /home: User home directories
- /etc: System configuration files
- /var: Variable data (logs, temporary files)
- /usr: User binaries and read-only data
- /bin: Essential command binaries
- /sbin: System binaries
- /tmp: Temporary files

### 1.2 Basic Navigation Commands

#### 1.2.1 pwd (Print Working Directory)
Displays the current working directory.

```bash
pwd
```

#### 1.2.2 cd (Change Directory)
Changes the current working directory.

```bash
cd /path/to/directory  # Absolute path
cd subdirectory        # Relative path
cd ..                  # Move up one level
cd ~                   # Go to home directory
cd -                   # Go to previous directory
```

#### 1.2.3 ls (List)
Lists directory contents.

```bash
ls                 # List current directory
ls -l              # Long format listing
ls -a              # Show hidden files
ls -lh             # Human-readable file sizes
ls -R              # Recursive listing
ls /path/to/dir    # List specific directory
```

### 1.3 Understanding File Paths

- Absolute paths: Start from the root directory (e.g., /home/user/documents)
- Relative paths: Relative to the current directory (e.g., ./documents or documents)
- Home directory shortcut: ~ (e.g., ~/documents)

### 1.4 Tab Completion

Use the Tab key for auto-completion of file and directory names.

### 1.5 Wildcards

- `*`: Matches any number of characters
- `?`: Matches any single character
- `[...]`: Matches any one of the enclosed characters

Example:
```bash
ls *.txt       # List all .txt files
ls doc?ment*   # Match document, documents, documentation, etc.
ls file[1-3]   # Match file1, file2, file3
```

## 2. File and Directory Operations

### 2.1 Creating Directories

#### 2.1.1 mkdir (Make Directory)
Creates a new directory.

```bash
mkdir new_directory
mkdir -p parent/child/grandchild  # Create parent directories as needed
```

### 2.2 Creating Files

#### 2.2.1 touch
Creates an empty file or updates the timestamp of an existing file.

```bash
touch newfile.txt
```

### 2.3 Copying Files and Directories

#### 2.3.1 cp (Copy)
Copies files or directories.

```bash
cp source.txt destination.txt
cp -r source_dir destination_dir  # Recursive copy for directories
cp -i file1 file2                 # Interactive mode (prompt before overwrite)
```

### 2.4 Moving and Renaming

#### 2.4.1 mv (Move)
Moves or renames files and directories.

```bash
mv oldname.txt newname.txt        # Rename a file
mv file.txt /path/to/destination  # Move a file
mv -i source dest                 # Interactive mode
```

### 2.5 Removing Files and Directories

#### 2.5.1 rm (Remove)
Removes files and directories.

```bash
rm file.txt
rm -r directory     # Recursive removal (for directories)
rm -f file.txt      # Force removal without prompting
rm -i file.txt      # Interactive mode
```

#### 2.5.2 rmdir (Remove Directory)
Removes empty directories.

```bash
rmdir empty_directory
```

### 2.6 Linking Files

#### 2.6.1 ln (Link)
Creates hard or symbolic links.

```bash
ln file.txt hardlink       # Create a hard link
ln -s file.txt symlink     # Create a symbolic link
```

### 2.7 File Permissions

#### 2.7.1 chmod (Change Mode)
Changes file permissions.

```bash
chmod 755 file.txt         # Octal notation
chmod u+x file.txt         # Symbolic notation (add execute permission for user)
```

#### 2.7.2 chown (Change Owner)
Changes file owner and group.

```bash
chown user:group file.txt
```

## 3. Viewing and Editing Files

### 3.1 Viewing File Contents

#### 3.1.1 cat (Concatenate)
Displays the entire contents of a file.

```bash
cat file.txt
```

#### 3.1.2 less
Allows scrolling through large files.

```bash
less largefile.txt
```

Key commands in less:
- Space: Next page
- b: Previous page
- /pattern: Search forward for pattern
- ?pattern: Search backward for pattern
- q: Quit

#### 3.1.3 head
Displays the first few lines of a file.

```bash
head file.txt       # Default: first 10 lines
head -n 20 file.txt # First 20 lines
```

#### 3.1.4 tail
Displays the last few lines of a file.

```bash
tail file.txt       # Default: last 10 lines
tail -n 20 file.txt # Last 20 lines
tail -f log.txt     # Follow mode (useful for log files)
```

### 3.2 Editing Files

#### 3.2.1 nano
A simple, beginner-friendly text editor.

```bash
nano file.txt
```

Key commands in nano:
- Ctrl+O: Save
- Ctrl+X: Exit
- Ctrl+W: Search

#### 3.2.2 vim
A powerful, modal text editor.

```bash
vim file.txt
```

Basic vim commands:
- i: Enter insert mode
- Esc: Exit insert mode
- :w: Save
- :q: Quit
- :wq: Save and quit

#### 3.2.3 emacs
An extensible, customizable text editor.

```bash
emacs file.txt
```

Basic emacs commands:
- Ctrl+x Ctrl+s: Save
- Ctrl+x Ctrl+c: Exit

### 3.3 Searching File Contents

#### 3.3.1 grep (Global Regular Expression Print)
Searches for patterns in files.

```bash
grep "pattern" file.txt
grep -i "case insensitive" file.txt
grep -r "recursive search" directory
```

#### 3.3.2 find
Searches for files and directories.

```bash
find /path -name "*.txt"
find . -type d        # Find directories
find . -type f -size +1M  # Find files larger than 1 MB
```

## Exercises

1. Create a directory structure for a project with at least three levels of nested directories.
2. Create several text files in different directories, then use `find` to locate all `.txt` files.
3. Use `cp` to copy a directory and its contents to a new location, then use `diff` to verify the copy.
4. Practice using `vim` or `emacs` to create and edit a simple shell script.
5. Use `grep` to search for a specific pattern across multiple files in a directory.
6. Experiment with file permissions using `chmod`. Create a file that can be executed by the owner but only read by others.
7. Use `head` and `tail` together with pipes to display lines 50-60 of a large text file.

## Review Questions

1. What is the difference between absolute and relative paths?
2. How do you create a directory and all necessary parent directories in one command?
3. Explain the difference between a hard link and a symbolic link.
4. What is the purpose of the `-r` option in the `cp` and `rm` commands?
5. How would you change the ownership of a file to a different user and group?
6. Describe the difference between `cat`, `less`, `head`, and `tail` for viewing file contents.
7. What are the basic modes in vim, and how do you switch between them?
8. How can you use `grep` to search for a case-insensitive pattern in all text files within a directory and its subdirectories?
9. Explain the octal notation for file permissions (e.g., 755, 644).
10. How would you find all files modified in the last 24 hours in your home directory and its subdirectories?

This lesson provides a thorough introduction to basic command line usage, covering file system navigation, file and directory operations, and methods for viewing and editing files. The exercises and review questions will help reinforce these fundamental skills, which are essential for effective shell scripting and system administration.

[Next: 03. Shell Scripting Fundamentals](./03_shell_scripting_fundamentals.md)