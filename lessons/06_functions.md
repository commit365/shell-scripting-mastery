# Functions in Shell Scripting

Functions are reusable blocks of code that perform specific tasks. They help organize code, improve readability, and reduce redundancy in shell scripts. This lesson covers function definition, calling, parameters, return values, and variable scope.

## 1. Defining and Calling Functions

### 1.1 Function Definition

In shell scripting, you can define functions using two syntaxes:

1. Using the `function` keyword:

```bash
function function_name {
    # function body
}
```

2. Without the `function` keyword (POSIX-compliant):

```bash
function_name() {
    # function body
}
```

Example:

```bash
#!/bin/bash

greet() {
    echo "Hello, World!"
}

# Call the function
greet
```

### 1.2 Function Naming Conventions

- Use descriptive names that indicate the function's purpose
- Use lowercase letters and underscores for readability
- Avoid using built-in command names or existing function names

### 1.3 Calling Functions

To call a function, simply use its name:

```bash
function_name
```

Functions must be defined before they are called. It's common practice to define functions at the beginning of a script.

### 1.4 Exit Status of Functions

Functions, like commands, return an exit status. The exit status is the status of the last command executed in the function, unless explicitly set using the `return` statement.

```bash
check_number() {
    if [ $1 -gt 0 ]; then
        return 0  # Success
    else
        return 1  # Failure
    fi
}

check_number 5
echo $?  # Prints 0 (success)

check_number -3
echo $?  # Prints 1 (failure)
```

## 2. Function Parameters and Return Values

### 2.1 Function Parameters

Functions can accept parameters, which are accessed within the function using positional parameters: `$1`, `$2`, `$3`, etc.

Example:

```bash
greet_person() {
    echo "Hello, $1!"
}

greet_person "Alice"  # Output: Hello, Alice!
greet_person "Bob"    # Output: Hello, Bob!
```

Special parameters in functions:
- `$#`: Number of parameters
- `$*`: All parameters as a single word
- `$@`: All parameters as separate words

Example:

```bash
print_params() {
    echo "Number of parameters: $#"
    echo "All parameters: $*"
    echo "Parameters as separate words:"
    for param in "$@"; do
        echo "$param"
    done
}

print_params apple banana cherry
```

### 2.2 Return Values

In shell scripting, functions can "return" values in several ways:

1. Using the `return` statement (limited to integer values 0-255):

```bash
is_even() {
    if [ $(($1 % 2)) -eq 0 ]; then
        return 0  # True (success)
    else
        return 1  # False (failure)
    fi
}

is_even 4
if [ $? -eq 0 ]; then
    echo "4 is even"
fi
```

2. Echoing the result and capturing it with command substitution:

```bash
get_square() {
    echo $(($1 * $1))
}

result=$(get_square 5)
echo "The square of 5 is $result"
```

3. Modifying a global variable:

```bash
RESULT=""

calculate_square() {
    RESULT=$(($1 * $1))
}

calculate_square 6
echo "The square of 6 is $RESULT"
```

### 2.3 Returning Multiple Values

To return multiple values, you can use one of these methods:

1. Echoing multiple values and parsing them:

```bash
get_dimensions() {
    echo "100 50"  # Width and height
}

read width height < <(get_dimensions)
echo "Width: $width, Height: $height"
```

2. Using global variables:

```bash
get_dimensions() {
    WIDTH=100
    HEIGHT=50
}

get_dimensions
echo "Width: $WIDTH, Height: $HEIGHT"
```

## 3. Variable Scope in Functions

### 3.1 Global Variables

By default, variables in shell scripts are global. They can be accessed and modified from anywhere in the script, including within functions.

```bash
#!/bin/bash

MY_GLOBAL_VAR="I am global"

print_global() {
    echo "$MY_GLOBAL_VAR"
}

print_global  # Output: I am global

MY_GLOBAL_VAR="I am modified"
print_global  # Output: I am modified
```

### 3.2 Local Variables

To create variables with local scope within a function, use the `local` keyword:

```bash
#!/bin/bash

my_function() {
    local local_var="I am local"
    echo "Inside function: $local_var"
}

my_function
echo "Outside function: $local_var"  # This will be empty
```

### 3.3 Shadowing Global Variables

Local variables can have the same name as global variables, shadowing them within the function:

```bash
#!/bin/bash

MY_VAR="global"

my_function() {
    local MY_VAR="local"
    echo "Inside function: $MY_VAR"
}

echo "Before function: $MY_VAR"
my_function
echo "After function: $MY_VAR"
```

### 3.4 Exporting Functions

You can export functions to make them available to child processes:

```bash
export -f function_name
```

This is useful when you want to use functions in subshells or child scripts.

## Exercises

1. Write a function that takes two numbers as parameters and returns their sum.

2. Create a function that calculates the factorial of a given number using recursion.

3. Write a script with a function that generates a random password of a specified length (passed as a parameter).

4. Implement a function that takes a string as input and returns the reverse of that string.

5. Create a function that checks if a given year is a leap year and returns an appropriate boolean value.

6. Write a script with a function that finds the largest number in an array passed as parameters.

7. Implement a function that simulates a simple calculator, taking an operation (+, -, *, /) and two numbers as parameters.

## Review Questions

1. What are the benefits of using functions in shell scripts?

2. Explain the difference between global and local variables in the context of shell functions.

3. How can a function return a value in shell scripting? What are the limitations of the `return` statement?

4. What is variable shadowing, and how can it be useful or problematic in functions?

5. How do you pass an array to a function, and how can you process all its elements inside the function?

6. Explain the purpose of the `local` keyword in shell functions. What happens if you omit it?

7. How can you make a function available to child processes or scripts?

8. What is the difference between `$*` and `$@` when used within a function to refer to its parameters?

9. How would you implement a function that needs to return multiple values?

10. Describe a scenario where recursion in shell functions might be useful, and explain how you would implement it.

This lesson covers the fundamental concepts of functions in shell scripting, including function definition, parameters, return values, and variable scope. The exercises and review questions will help reinforce these concepts and encourage practical application of the knowledge gained.

[Next: 07. Arrays and Associative Arrays](./07_arrays_and_associative_arrays.md)