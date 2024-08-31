# String Manipulation in Shell Scripting

String manipulation is a crucial skill in shell scripting, allowing you to process and transform text data efficiently. This lesson covers string operations, comparisons, regular expressions, and the use of sed and awk for advanced text processing.

## 1. String Operations and Comparisons

### 1.1 String Concatenation

In bash, you can concatenate strings simply by placing them next to each other:

```bash
str1="Hello"
str2="World"
result="$str1 $str2"
echo $result  # Output: Hello World
```

### 1.2 String Length

To get the length of a string:

```bash
str="Hello, World!"
echo ${#str}  # Output: 13
```

### 1.3 Substring Extraction

To extract a substring:

```bash
str="Hello, World!"
echo ${str:7}     # Output: World!
echo ${str:7:5}   # Output: World
```

### 1.4 String Replacement

To replace the first occurrence of a substring:

```bash
str="Hello, World!"
echo ${str/World/Universe}  # Output: Hello, Universe!
```

To replace all occurrences:

```bash
str="Hello, Hello, Hello!"
echo ${str//Hello/Hi}  # Output: Hi, Hi, Hi!
```

### 1.5 String Trimming

To trim from the beginning:

```bash
str="   Hello, World!   "
echo ${str##*( )}  # Output: Hello, World!   
```

To trim from the end:

```bash
str="   Hello, World!   "
echo ${str%%*( )}  # Output:    Hello, World!
```

### 1.6 Case Conversion

To convert to uppercase:

```bash
str="Hello, World!"
echo ${str^^}  # Output: HELLO, WORLD!
```

To convert to lowercase:

```bash
str="Hello, World!"
echo ${str,,}  # Output: hello, world!
```

### 1.7 String Comparisons

For string comparisons, use the `=` operator inside square brackets:

```bash
if [ "$str1" = "$str2" ]; then
    echo "Strings are equal"
fi
```

For case-insensitive comparison:

```bash
if [[ "${str1,,}" = "${str2,,}" ]]; then
    echo "Strings are equal (case-insensitive)"
fi
```

Other comparison operators:
- `!=`: Not equal
- `<`: Less than (ASCII order)
- `>`: Greater than (ASCII order)
- `-z`: Zero length
- `-n`: Non-zero length

Example:

```bash
if [ -z "$str" ]; then
    echo "String is empty"
elif [ -n "$str" ]; then
    echo "String is not empty"
fi
```

## 2. Regular Expressions

Regular expressions (regex) are powerful tools for pattern matching and text processing.

### 2.1 Basic Regex Syntax

- `.`: Matches any single character
- `*`: Matches zero or more occurrences of the previous character
- `^`: Matches the start of a line
- `$`: Matches the end of a line
- `[]`: Matches any single character within the brackets
- `[^]`: Matches any single character not within the brackets

### 2.2 Extended Regex Syntax

- `+`: Matches one or more occurrences of the previous character
- `?`: Matches zero or one occurrence of the previous character
- `()`: Groups characters
- `|`: Alternation (OR)
- `{}`: Specifies the number of occurrences

### 2.3 Using Regex in Bash

Bash provides the `=~` operator for regex matching:

```bash
if [[ "$string" =~ ^[0-9]+$ ]]; then
    echo "String contains only numbers"
fi
```

### 2.4 grep with Regex

The `grep` command is often used with regex for searching:

```bash
grep -E '^[A-Z]{3}-[0-9]{4}$' file.txt
```

This searches for lines in `file.txt` that match the pattern of three uppercase letters, a hyphen, and four digits.

## 3. sed for Text Processing

sed (stream editor) is a powerful tool for text transformation.

### 3.1 Basic sed Syntax

```bash
sed 'command' filename
```

### 3.2 Common sed Commands

- Substitution:
  ```bash
  sed 's/old/new/' file.txt         # Replace first occurrence on each line
  sed 's/old/new/g' file.txt        # Replace all occurrences
  sed 's/old/new/2' file.txt        # Replace second occurrence on each line
  ```

- Deletion:
  ```bash
  sed '/pattern/d' file.txt         # Delete lines matching pattern
  sed '2d' file.txt                 # Delete second line
  sed '2,5d' file.txt               # Delete lines 2 through 5
  ```

- Insertion and Appending:
  ```bash
  sed '2i\New Line' file.txt        # Insert before line 2
  sed '2a\New Line' file.txt        # Append after line 2
  ```

- Multiple Commands:
  ```bash
  sed -e 's/old/new/' -e 's/foo/bar/' file.txt
  ```

### 3.3 sed with Regular Expressions

sed supports regular expressions in its patterns:

```bash
sed 's/^[0-9]\{3\}/(&)/' file.txt   # Add parentheses around first three digits
```

### 3.4 In-place Editing

Use the `-i` option for in-place editing:

```bash
sed -i 's/old/new/g' file.txt
```

## 4. awk for Text Processing

awk is a powerful text-processing tool that treats input as records and fields.

### 4.1 Basic awk Syntax

```bash
awk 'pattern { action }' filename
```

### 4.2 Field Processing

awk splits each line into fields:

```bash
echo "John Doe 25" | awk '{ print $2 }'  # Output: Doe
```

### 4.3 Built-in Variables

- `NR`: Current record number
- `NF`: Number of fields in the current record
- `$0`: Entire current record
- `$1`, `$2`, etc.: Individual fields

Example:

```bash
awk '{ print NR ":", $1 }' file.txt
```

### 4.4 Patterns and Actions

```bash
awk '$3 > 30 { print $1, $2 }' data.txt  # Print names where third field > 30
```

### 4.5 BEGIN and END Blocks

```bash
awk 'BEGIN { print "Start" } { print $0 } END { print "End" }' file.txt
```

### 4.6 Built-in Functions

awk provides many built-in functions:

```bash
awk '{ print toupper($1) }' file.txt  # Convert first field to uppercase
```

### 4.7 User-Defined Functions

You can define your own functions in awk:

```bash
awk '
function max(a, b) {
    return (a > b) ? a : b
}
{ print max($1, $2) }
' data.txt
```

## Exercises

1. Write a script that takes a string as input and reverses it without using any built-in reverse functions.

2. Create a function that validates an email address using regex.

3. Write a sed script that converts all occurrences of "color" to "colour" in a file, but only if they're not part of another word.

4. Use awk to process a CSV file, calculating the average of a specific column.

5. Write a script that uses sed to remove all HTML tags from a file.

6. Create an awk script that finds the longest line in a file and prints its length and content.

7. Write a sed command that swaps the positions of the first two words on each line of a file.

## Review Questions

1. What's the difference between `${var#pattern}` and `${var##pattern}` in string manipulation?

2. Explain the concept of greedy vs. non-greedy matching in regular expressions.

3. How does sed's `-i` option differ from its normal operation?

4. In awk, what's the difference between `$0` and `$1`?

5. How would you use sed to perform a multi-line replacement?

6. Explain the purpose of the `BEGIN` and `END` blocks in awk.

7. How can you use bash's built-in string manipulation to achieve case-insensitive string comparison?

8. What are the advantages of using awk over sed for certain text processing tasks?

9. How would you use regex with grep to find all lines in a file that don't start with a number?

10. Explain how you would use sed to delete every third line in a file.

This lesson covers the fundamental concepts of string manipulation in shell scripting, including basic operations, regular expressions, and the use of sed and awk for advanced text processing. The exercises and review questions will help reinforce these concepts and encourage practical application of the knowledge gained.

[Next: 09: File Operations and Text Processing](./09_file_operations_and_text_processing.md)