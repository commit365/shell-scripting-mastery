# File Operations and Text Processing in Shell Scripting

File operations and text processing are fundamental skills in shell scripting, enabling you to manipulate files, directories, and their contents effectively. This lesson covers reading and writing files, file testing and comparisons, and advanced text processing techniques.

## 1. Reading and Writing Files

### 1.1 Reading Files

#### 1.1.1 Using cat

The `cat` command is used to display the entire contents of a file:

```bash
cat file.txt
```

#### 1.1.2 Using less

For large files, `less` allows scrolling:

```bash
less file.txt
```

#### 1.1.3 Using head and tail

To view the beginning or end of a file:

```bash
head -n 10 file.txt  # First 10 lines
tail -n 10 file.txt  # Last 10 lines
```

#### 1.1.4 Reading Line by Line

To process a file line by line in a script:

```bash
while IFS= read -r line; do
    echo "Line: $line"
done < file.txt
```

#### 1.1.5 Reading into an Array

To read file contents into an array:

```bash
mapfile -t lines < file.txt
# or
IFS=$'\n' read -d '' -ra lines < file.txt
```

### 1.2 Writing Files

#### 1.2.1 Using Redirection

To write to a file (overwriting existing content):

```bash
echo "Hello, World!" > file.txt
```

To append to a file:

```bash
echo "New line" >> file.txt
```

#### 1.2.2 Using tee

To write to a file and display output simultaneously:

```bash
echo "Hello, World!" | tee file.txt
echo "Appended line" | tee -a file.txt
```

#### 1.2.3 Here Documents

For writing multi-line content:

```bash
cat << EOF > file.txt
Line 1
Line 2
Line 3
EOF
```

### 1.3 File Descriptors

Bash uses file descriptors for I/O operations:
- 0: Standard Input (stdin)
- 1: Standard Output (stdout)
- 2: Standard Error (stderr)

Example of redirecting stderr to a file:

```bash
command 2> error.log
```

## 2. File Testing and Comparisons

### 2.1 File Test Operators

Bash provides several operators for file testing:

- `-e file`: True if file exists
- `-f file`: True if file exists and is a regular file
- `-d file`: True if file exists and is a directory
- `-r file`: True if file is readable
- `-w file`: True if file is writable
- `-x file`: True if file is executable
- `-s file`: True if file exists and is not empty

Example:

```bash
if [ -f "file.txt" ]; then
    echo "file.txt exists and is a regular file"
fi
```

### 2.2 File Comparisons

To compare files:

- `-nt`: Newer than
- `-ot`: Older than
- `-ef`: Same file (hard links)

Example:

```bash
if [ "file1.txt" -nt "file2.txt" ]; then
    echo "file1.txt is newer than file2.txt"
fi
```

### 2.3 File Permissions

To check file permissions:

```bash
if [ -r "file.txt" ] && [ -w "file.txt" ]; then
    echo "file.txt is readable and writable"
fi
```

### 2.4 File Size

To get file size:

```bash
size=$(stat -c %s "file.txt")
echo "File size: $size bytes"
```

## 3. Advanced Text Processing Techniques

### 3.1 Using grep for Pattern Matching

grep is powerful for searching text patterns:

```bash
grep "pattern" file.txt          # Basic search
grep -i "pattern" file.txt       # Case-insensitive search
grep -r "pattern" directory      # Recursive search
grep -v "pattern" file.txt       # Invert match
grep -c "pattern" file.txt       # Count matches
grep -n "pattern" file.txt       # Show line numbers
```

### 3.2 Using sed for Text Transformation

sed is a stream editor for filtering and transforming text:

```bash
sed 's/old/new/' file.txt        # Replace first occurrence on each line
sed 's/old/new/g' file.txt       # Replace all occurrences
sed '1,5d' file.txt              # Delete lines 1-5
sed '/pattern/d' file.txt        # Delete lines matching pattern
sed 'G' file.txt                 # Double-space a file
```

### 3.3 Using awk for Advanced Text Processing

awk is powerful for processing structured data:

```bash
awk '{print $1}' file.txt        # Print first field of each line
awk -F: '{print $1}' /etc/passwd # Use custom field separator
awk '{sum += $1} END {print sum}' file.txt  # Sum first column
```

### 3.4 Using cut for Extracting Fields

cut is useful for extracting specific columns:

```bash
cut -d: -f1 /etc/passwd          # Extract usernames
cut -c1-10 file.txt              # Extract first 10 characters of each line
```

### 3.5 Using sort and uniq for Data Manipulation

sort and uniq are often used together for data processing:

```bash
sort file.txt                    # Sort lines alphabetically
sort -n file.txt                 # Sort numerically
sort -u file.txt                 # Sort and remove duplicates
sort file.txt | uniq             # Remove adjacent duplicates
sort file.txt | uniq -c          # Count occurrences of lines
```

### 3.6 Using tr for Character Translation

tr translates or deletes characters:

```bash
echo "HELLO" | tr '[:upper:]' '[:lower:]'  # Convert to lowercase
cat file.txt | tr -d '\n'                  # Remove newlines
```

### 3.7 Using diff for File Comparison

diff compares files line by line:

```bash
diff file1.txt file2.txt         # Show differences
diff -u file1.txt file2.txt      # Unified format diff
```

### 3.8 Using wc for Counting

wc counts lines, words, and characters:

```bash
wc file.txt                      # Lines, words, characters
wc -l file.txt                   # Count lines only
```

## Exercises

1. Write a script that reads a file, counts the occurrences of each word, and outputs the results sorted by frequency.

2. Create a script that takes two file names as arguments, compares them, and outputs whether they are identical, and if not, how they differ.

3. Write a script that processes a log file, extracting all IP addresses and counting their occurrences.

4. Create a script that takes a CSV file as input and converts it to a formatted HTML table.

5. Write a script that finds all files larger than 1MB in a directory tree and outputs their names and sizes.

6. Create a script that reads a text file and outputs the average word length.

7. Write a script that takes a file name as an argument and creates a backup copy with a timestamp in the filename.

## Review Questions

1. How would you read a file line by line in a bash script, preserving leading/trailing whitespace?

2. Explain the difference between `>` and `>>` when writing to files.

3. What is the purpose of file descriptors in bash, and how can you use them for advanced I/O operations?

4. How would you use `sed` to replace all occurrences of a pattern, but only in lines that match another pattern?

5. Describe a situation where you would use `awk` instead of `sed` for text processing.

6. How can you use `sort` and `uniq` together to find the most frequent lines in a file?

7. Explain the difference between `-e`, `-f`, and `-d` file test operators in bash.

8. How would you use `grep` to search for lines that do not match a pattern?

9. What is the purpose of the `IFS` variable when reading files, and how can it be modified?

10. Describe how you would use `diff` to create a patch file, and how you would apply that patch.

This lesson covers fundamental and advanced concepts of file operations and text processing in shell scripting. The exercises and review questions are designed to reinforce these concepts and encourage practical application of the knowledge gained.

[Next: Process Management](./10_process_management.md)