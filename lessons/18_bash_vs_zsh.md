# Bash vs. Zsh

Bash (Bourne Again SHell) and Zsh (Z Shell) are two of the most popular Unix shell environments. While they share many similarities, they also have significant differences that can impact usability, scripting, and functionality. This lesson explores these differences, highlights Zsh-specific features, and provides guidance for migrating scripts from Bash to Zsh.

## 1. Key Differences Between Bash and Zsh

### 1.1 Syntax and Compatibility

- **Bash**: Developed as a replacement for the original Bourne shell (`sh`). It is POSIX-compliant, which means that scripts written for `sh` will generally work in Bash.
- **Zsh**: While Zsh is largely compatible with Bash, it includes additional features and enhancements that may not be present in Bash scripts.

### 1.2 Interactive Features

- **Command Line Editing**:
  - **Bash**: Supports basic command line editing with Emacs and Vi modes.
  - **Zsh**: Offers enhanced command line editing capabilities, including better completion and history management.

- **Autocompletion**:
  - **Bash**: Basic tab completion.
  - **Zsh**: Advanced completion system with context-aware suggestions, customizable completions, and the ability to complete commands, options, and arguments.

### 1.3 Prompt Customization

- **Bash**: Supports basic prompt customization using the `PS1` variable.
- **Zsh**: Allows for extensive prompt customization with themes and plugins, including the use of colors, icons, and dynamic content (e.g., git branch).

### 1.4 History Management

- **Bash**: Maintains a history file (`~/.bash_history`) and supports basic history manipulation.
- **Zsh**: Offers more robust history management, including shared history across sessions, timestamped entries, and the ability to search history interactively.

### 1.5 Array Handling

- **Bash**: Supports indexed arrays but does not support associative arrays until version 4.
- **Zsh**: Supports both indexed and associative arrays natively, with more powerful array manipulation features.

### 1.6 Scripting Features

- **Bash**: Standard scripting features including functions, loops, and conditionals.
- **Zsh**: Adds additional features such as floating-point arithmetic, advanced globbing, and more powerful parameter expansion.

### 1.7 Performance

- **Bash**: Generally performs well for basic scripting tasks.
- **Zsh**: May have slightly slower startup times due to its extensive features, but it can be optimized with configuration.

## 2. Zsh-Specific Features and Enhancements

### 2.1 Advanced Completion

Zsh provides advanced completion options that allow for:
- Context-aware completions based on the command being typed.
- Customizable completion functions for specific commands.

Example of enabling completion:

```bash
autoload -Uz compinit
compinit
```

### 2.2 Extended Globbing

Zsh supports extended globbing, allowing for more powerful pattern matching:

```bash
# Match all .txt files except those starting with 'temp'
ls **/*.txt~temp*
```

### 2.3 Improved Prompt Customization

Zsh allows for extensive prompt customization using themes. You can use the `oh-my-zsh` framework for easy management of themes and plugins.

Example of a simple prompt customization:

```bash
PROMPT='%F{green}%n@%m %F{blue}%~ %F{yellow}%% %f'
```

### 2.4 Floating-Point Arithmetic

Zsh supports floating-point arithmetic natively:

```bash
result=$(echo "scale=2; 10 / 3" | bc)
echo $result  # Output: 3.33
```

### 2.5 Command Correction

Zsh can automatically correct minor typos in commands:

```bash
setopt correct
```

### 2.6 Directory Navigation

Zsh offers advanced directory navigation features, such as:
- `cd -` to switch to the previous directory.
- `cd ~-` to navigate to the last directory.

### 2.7 Plugin Support

Zsh has a rich ecosystem of plugins and frameworks, such as:
- **Oh-My-Zsh**: A community-driven framework for managing Zsh configuration.
- **zsh-syntax-highlighting**: Provides syntax highlighting for commands in the command line.

## 3. Migrating Scripts from Bash to Zsh

### 3.1 Compatibility Considerations

- Most basic Bash scripts will run in Zsh without modification.
- Review any Bash-specific syntax or features that may not be supported in Zsh.

### 3.2 Testing Your Scripts

Before fully migrating, test your scripts in Zsh:

```bash
zsh -n your_script.sh  # Check for syntax errors
```

### 3.3 Modifying Shebang

Change the shebang line to use Zsh:

```bash
#!/bin/zsh
```

### 3.4 Updating Syntax

If your Bash script uses features not supported in Zsh, update the syntax accordingly. For example, if you use associative arrays in Bash, ensure you adapt them to Zsh’s syntax.

### 3.5 Utilizing Zsh Features

Take advantage of Zsh-specific features to improve your scripts, such as:
- Using advanced completion and globbing.
- Implementing floating-point arithmetic where needed.
- Utilizing improved prompt customization for user interaction.

### 3.6 Testing and Validation

After migrating, thoroughly test your scripts to ensure they work as expected in Zsh. Validate functionality and performance.

## Exercises

1. Write a Bash script that utilizes arrays and then convert it to Zsh, taking advantage of Zsh’s associative arrays.

2. Create a Zsh script that demonstrates advanced completion features by implementing a command that accepts various options and arguments.

3. Implement a Zsh script that uses floating-point arithmetic to calculate the average of a list of numbers.

4. Write a script that utilizes Zsh’s extended globbing to find and list files with specific patterns in their names.

5. Create a simple command-line tool in Zsh that accepts user input and provides context-aware suggestions based on the input.

## Review Questions

1. What are the main differences between Bash and Zsh in terms of features and usability?

2. Describe how Zsh’s advanced completion system can improve the command-line experience.

3. Explain the significance of the `setopt` command in Zsh and how it can be used to enable specific features.

4. What steps would you take to migrate a Bash script to Zsh? What specific features might you want to leverage in Zsh?

5. How does Zsh handle arrays differently from Bash? Provide examples of both indexed and associative arrays in Zsh.

6. Discuss the benefits of using a framework like Oh-My-Zsh for managing Zsh configurations.

7. How can you implement command correction in Zsh, and what are the potential pitfalls?

8. Explain the purpose of the `autoload` command in Zsh and how it relates to function loading.

9. How would you test a script for compatibility between Bash and Zsh?

10. Describe a scenario where using Zsh’s floating-point arithmetic would be advantageous over Bash’s integer-only arithmetic.

This lesson covers the key differences between Bash and Zsh, highlights Zsh-specific features, and provides guidance for migrating scripts from Bash to Zsh. The exercises and review questions are designed to reinforce these concepts and encourage practical application of the knowledge gained.

[Next: 19. Testing and Continuous Integration](./19_testing_and_continuous_integration.md)