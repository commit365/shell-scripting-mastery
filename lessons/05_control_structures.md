# Control Structures in Shell Scripting

Control structures are fundamental components of shell scripting that allow you to control the flow of execution in your scripts. They enable you to make decisions, repeat actions, and select specific code blocks based on different conditions.

## 1. Conditional Statements

Conditional statements allow your script to make decisions and execute different code blocks based on specific conditions.

### 1.1 if Statement

The basic syntax of an if statement is:

```bash
if [ condition ]; then
    # commands to execute if condition is true
fi
```

Example:

```bash
#!/bin/bash

age=18

if [ $age -ge 18 ]; then
    echo "You are an adult."
fi
```

### 1.2 if-else Statement

The if-else statement allows you to specify an alternative action when the condition is false:

```bash
if [ condition ]; then
    # commands to execute if condition is true
else
    # commands to execute if condition is false
fi
```

Example:

```bash
#!/bin/bash

age=16

if [ $age -ge 18 ]; then
    echo "You are an adult."
else
    echo "You are a minor."
fi
```

### 1.3 if-elif-else Statement

For multiple conditions, use the if-elif-else structure:

```bash
if [ condition1 ]; then
    # commands to execute if condition1 is true
elif [ condition2 ]; then
    # commands to execute if condition2 is true
else
    # commands to execute if all conditions are false
fi
```

Example:

```bash
#!/bin/bash

score=75

if [ $score -ge 90 ]; then
    echo "Grade: A"
elif [ $score -ge 80 ]; then
    echo "Grade: B"
elif [ $score -ge 70 ]; then
    echo "Grade: C"
else
    echo "Grade: F"
fi
```

### 1.4 Nested if Statements

You can nest if statements within each other:

```bash
if [ condition1 ]; then
    if [ condition2 ]; then
        # commands
    fi
fi
```

### 1.5 Test Operators

Common test operators for conditions:

- Numeric comparisons: `-eq`, `-ne`, `-lt`, `-le`, `-gt`, `-ge`
- String comparisons: `=`, `!=`, `<`, `>`
- File tests: `-e` (exists), `-f` (regular file), `-d` (directory), `-r` (readable), `-w` (writable), `-x` (executable)

Example:

```bash
if [ -f "$file" ] && [ -r "$file" ]; then
    echo "File exists and is readable"
fi
```

### 1.6 Advanced Condition Syntax

For more complex conditions, use double square brackets `[[ ]]`:

```bash
if [[ $string == *"substring"* ]]; then
    echo "String contains substring"
fi
```

## 2. Loops

Loops allow you to execute a block of code repeatedly.

### 2.1 for Loop

The for loop iterates over a list of items:

```bash
for variable in list
do
    # commands
done
```

Examples:

```bash
# Iterate over a list of words
for color in red green blue
do
    echo "Color: $color"
done

# Iterate over a range of numbers
for i in {1..5}
do
    echo "Number: $i"
done

# C-style for loop
for ((i=0; i<5; i++))
do
    echo "Index: $i"
done
```

### 2.2 while Loop

The while loop executes as long as a condition is true:

```bash
while [ condition ]
do
    # commands
done
```

Example:

```bash
#!/bin/bash

count=1

while [ $count -le 5 ]
do
    echo "Count: $count"
    ((count++))
done
```

### 2.3 until Loop

The until loop executes until a condition becomes true:

```bash
until [ condition ]
do
    # commands
done
```

Example:

```bash
#!/bin/bash

count=1

until [ $count -gt 5 ]
do
    echo "Count: $count"
    ((count++))
done
```

### 2.4 Loop Control Statements

- `break`: Exit the loop immediately
- `continue`: Skip the rest of the current iteration and continue with the next

Example:

```bash
for i in {1..10}
do
    if [ $i -eq 5 ]; then
        continue
    fi
    if [ $i -eq 8 ]; then
        break
    fi
    echo "Number: $i"
done
```

### 2.5 Infinite Loops

Create an infinite loop using `while true` or `for (( ; ; ))`:

```bash
while true
do
    echo "This will run forever (Press Ctrl+C to stop)"
    sleep 1
done
```

## 3. Case Statements

Case statements provide a clean way to handle multiple conditions:

```bash
case $variable in
    pattern1)
        # commands for pattern1
        ;;
    pattern2)
        # commands for pattern2
        ;;
    *)
        # default commands
        ;;
esac
```

Example:

```bash
#!/bin/bash

echo "Enter a fruit name:"
read fruit

case $fruit in
    "apple")
        echo "It's a red fruit."
        ;;
    "banana")
        echo "It's a yellow fruit."
        ;;
    "orange")
        echo "It's an orange fruit."
        ;;
    *)
        echo "Unknown fruit."
        ;;
esac
```

### 3.1 Multiple Patterns

You can match multiple patterns for a single case:

```bash
case $variable in
    pattern1|pattern2|pattern3)
        # commands
        ;;
esac
```

### 3.2 Pattern Matching

Case statements support pattern matching:

- `*`: Matches any string
- `?`: Matches any single character
- `[...]`: Matches any one of the enclosed characters

Example:

```bash
case $filename in
    *.jpg|*.jpeg)
        echo "JPEG image"
        ;;
    *.png)
        echo "PNG image"
        ;;
    *.txt)
        echo "Text file"
        ;;
    *)
        echo "Unknown file type"
        ;;
esac
```

## Exercises

1. Write a script that prompts the user for a number and determines if it's positive, negative, or zero using if-elif-else statements.

2. Create a script that uses a for loop to print the multiplication table for a given number.

3. Write a script using a while loop that generates random numbers between 1 and 10 until it generates a 7.

4. Create a script that uses an until loop to implement a simple countdown timer.

5. Write a script that uses a case statement to convert a numeric month (1-12) to its corresponding name (January-December).

6. Create a script that uses nested if statements to determine if a year is a leap year.

7. Write a script that uses a for loop with the `continue` and `break` statements to print all odd numbers between 1 and 20, but stops if it encounters a number divisible by 13.

## Review Questions

1. What is the difference between `if` and `case` statements? When would you prefer one over the other?

2. Explain the difference between `while` and `until` loops. Can any `while` loop be rewritten as an `until` loop?

3. How can you create an infinite loop, and in what situations might this be useful?

4. What is the purpose of the `break` and `continue` statements in loops?

5. How do you perform pattern matching in a case statement?

6. What are the advantages of using `[[` instead of `[` in conditional statements?

7. How would you implement a menu-driven script using a while loop and a case statement?

8. Explain how you would use a for loop to process all files with a specific extension in a directory.

9. What is the difference between `$@` and `$*` when used in a for loop to iterate over script arguments?

10. How can you use control structures to implement error handling and input validation in your scripts?

This lesson covers the fundamental concepts of control structures in shell scripting, including conditional statements, loops, and case statements. The exercises and review questions will help reinforce these concepts and encourage practical application of the knowledge gained.

[Next: 06. Functions](./06_functions.md)