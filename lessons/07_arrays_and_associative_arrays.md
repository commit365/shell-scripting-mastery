# Arrays and Associative Arrays in Shell Scripting

Arrays are data structures that allow you to store multiple values under a single variable name. In shell scripting, we have two types of arrays: indexed arrays and associative arrays. This lesson covers both types, their operations, and functions.

## 1. Indexed Arrays

Indexed arrays use integer indices to access and manipulate elements.

### 1.1 Creating Indexed Arrays

There are several ways to create indexed arrays:

1. Explicit declaration:

```bash
declare -a my_array
```

2. Implicit declaration by assignment:

```bash
my_array=(apple banana cherry)
```

3. Individual element assignment:

```bash
my_array="apple"
my_array="banana"
my_array="cherry"
```

### 1.2 Accessing Array Elements

To access array elements, use the index in square brackets:

```bash
echo ${my_array}  # Outputs: apple
```

To access all elements:

```bash
echo ${my_array[@]}  # Outputs: apple banana cherry
```

### 1.3 Array Length

To get the number of elements in an array:

```bash
echo ${#my_array[@]}
```

### 1.4 Array Indices

To get all indices of an array:

```bash
echo ${!my_array[@]}
```

### 1.5 Slicing Arrays

You can extract a slice of an array using the `${array[@]:start:length}` syntax:

```bash
my_array=(apple banana cherry date elderberry)
echo ${my_array[@]:1:3}  # Outputs: banana cherry date
```

### 1.6 Adding Elements

To append elements to an array:

```bash
my_array+=(fig grape)
```

### 1.7 Removing Elements

To remove an element, use the `unset` command:

```bash
unset my_array
```

Note that this leaves a gap in the array indices.

### 1.8 Copying Arrays

To copy an array:

```bash
new_array=("${my_array[@]}")
```

### 1.9 Looping Through Arrays

You can iterate through array elements using a for loop:

```bash
for fruit in "${my_array[@]}"; do
    echo "$fruit"
done
```

To iterate with indices:

```bash
for i in "${!my_array[@]}"; do
    echo "Index $i: ${my_array[$i]}"
done
```

## 2. Array Operations and Functions

### 2.1 Sorting Arrays

To sort an array:

```bash
sorted_array=($(printf '%s\n' "${my_array[@]}" | sort))
```

### 2.2 Reversing Arrays

To reverse an array:

```bash
reversed_array=($(printf '%s\n' "${my_array[@]}" | tac))
```

### 2.3 Finding Unique Elements

To get unique elements:

```bash
unique_array=($(printf '%s\n' "${my_array[@]}" | sort -u))
```

### 2.4 Filtering Arrays

To filter array elements based on a condition:

```bash
filtered_array=($(printf '%s\n' "${my_array[@]}" | grep 'pattern'))
```

### 2.5 Joining Array Elements

To join array elements with a delimiter:

```bash
joined_string=$(IFS=", "; echo "${my_array[*]}")
```

### 2.6 Finding Element Index

To find the index of an element:

```bash
find_index() {
    local array=("$@")
    local element="${array[-1]}"
    unset 'array[-1]'
    for i in "${!array[@]}"; do
        if [[ "${array[$i]}" == "$element" ]]; then
            echo "$i"
            return 0
        fi
    done
    return 1
}

my_array=(apple banana cherry date)
index=$(find_index "${my_array[@]}" "cherry")
echo "Index of 'cherry': $index"
```

## 3. Associative Arrays (Bash 4+ and Zsh)

Associative arrays use string keys instead of integer indices.

### 3.1 Creating Associative Arrays

To create an associative array:

```bash
declare -A my_assoc_array
```

Or with initial values:

```bash
declare -A my_assoc_array=(
    [fruit]="apple"
    [vegetable]="carrot"
    [protein]="chicken"
)
```

### 3.2 Adding/Modifying Elements

To add or modify elements:

```bash
my_assoc_array[grain]="rice"
my_assoc_array[fruit]="banana"
```

### 3.3 Accessing Elements

To access an element:

```bash
echo ${my_assoc_array[fruit]}
```

### 3.4 Listing Keys and Values

To list all keys:

```bash
echo ${!my_assoc_array[@]}
```

To list all values:

```bash
echo ${my_assoc_array[@]}
```

### 3.5 Checking for Key Existence

To check if a key exists:

```bash
if [[ -v my_assoc_array[fruit] ]]; then
    echo "Fruit exists in the array"
fi
```

### 3.6 Removing Elements

To remove an element:

```bash
unset my_assoc_array[fruit]
```

### 3.7 Looping Through Associative Arrays

To iterate through an associative array:

```bash
for key in "${!my_assoc_array[@]}"; do
    echo "Key: $key, Value: ${my_assoc_array[$key]}"
done
```

### 3.8 Getting Array Length

To get the number of elements:

```bash
echo ${#my_assoc_array[@]}
```

## Exercises

1. Write a script that creates an array of numbers and calculates their sum and average.

2. Create a function that takes an array as input and returns a new array with only the unique elements.

3. Implement a simple phonebook using an associative array, allowing users to add, remove, and look up entries.

4. Write a script that reads a text file and creates an associative array where keys are words and values are their frequencies.

5. Create a function that merges two arrays, removing duplicates and sorting the result.

6. Implement a stack data structure using an array, with push and pop operations.

7. Write a script that uses an associative array to store and manipulate a simple key-value configuration file.

## Review Questions

1. What is the difference between indexed arrays and associative arrays?

2. How do you append multiple elements to an existing array in a single operation?

3. Explain the difference between `${array[*]}` and `${array[@]}` when expanding array elements.

4. How can you remove an element from the middle of an array without leaving a gap in the indices?

5. What is the purpose of the `declare -A` command when working with associative arrays?

6. How would you check if a specific value exists in an array?

7. Explain how to sort an array in reverse order.

8. What are some limitations of associative arrays compared to indexed arrays?

9. How can you convert an associative array to a series of key-value pairs suitable for export to a file?

10. Describe a real-world scenario where using an associative array would be more appropriate than an indexed array.

This lesson covers the fundamental concepts of indexed and associative arrays in shell scripting, including creation, manipulation, and common operations. The exercises and review questions will help reinforce these concepts and encourage practical application of the knowledge gained.
