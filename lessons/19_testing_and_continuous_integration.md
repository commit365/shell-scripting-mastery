# Testing and Continuous Integration for Shell Scripts

Testing and continuous integration (CI) are essential practices in software development, including shell scripting. This lesson covers how to write tests for shell scripts and implement continuous integration for shell projects.

## 1. Writing Tests for Shell Scripts

### 1.1 Importance of Testing

Testing ensures that your scripts work as intended and helps catch bugs before deployment. It also makes it easier to refactor code and add new features with confidence.

### 1.2 Types of Tests

1. **Unit Tests**: Test individual functions or components in isolation.
2. **Integration Tests**: Test how different parts of the system work together.
3. **End-to-End Tests**: Test the entire script or application from start to finish.

### 1.3 Writing Unit Tests

#### 1.3.1 Using Bats (Bash Automated Testing System)

Bats is a popular testing framework for Bash scripts. To install Bats:

```bash
# Clone the Bats repository
git clone https://github.com/bats-core/bats-core.git /path/to/bats-core

# Add Bats to your PATH
export PATH="$PATH:/path/to/bats-core/bin"
```

#### 1.3.2 Creating a Test File

Create a test file with a `.bats` extension:

```bash
# test_example.bats
#!/usr/bin/env bats

setup() {
    # Code to run before each test
    MY_VAR="Hello, World!"
}

@test "check MY_VAR value" {
    [ "$MY_VAR" = "Hello, World!" ]
}

@test "check string length" {
    [ "${#MY_VAR}" -eq 13 ]
}
```

#### 1.3.3 Running Tests

Run the tests using Bats:

```bash
bats test_example.bats
```

### 1.4 Writing Integration Tests

Integration tests can be written similarly, but they may involve multiple components or external dependencies. Use Bats or a simple Bash script to execute integration tests.

Example:

```bash
# integration_test.bash
#!/bin/bash

# Test the interaction between two scripts
./script1.sh > output.txt
./script2.sh < output.txt

if grep -q "Expected Output" output.txt; then
    echo "Integration test passed"
else
    echo "Integration test failed"
    exit 1
fi
```

### 1.5 Using ShellCheck for Static Analysis

ShellCheck is a static analysis tool for shell scripts that helps identify common issues and potential bugs:

```bash
# Install ShellCheck
sudo apt-get install shellcheck

# Run ShellCheck on your script
shellcheck your_script.sh
```

### 1.6 Test Coverage

While traditional test coverage tools may not be directly applicable to shell scripts, you can track which functions have been tested by maintaining a coverage report manually or using a testing framework that supports coverage reporting.

## 2. Continuous Integration for Shell Projects

### 2.1 What is Continuous Integration?

Continuous Integration (CI) is the practice of automatically testing and building code changes to ensure that they work together. It helps catch issues early in the development process.

### 2.2 Setting Up CI with GitHub Actions

GitHub Actions is a powerful CI/CD tool integrated with GitHub. Here's how to set it up for a shell script project.

#### 2.2.1 Creating a Workflow

1. Create a directory for workflows:

```bash
mkdir -p .github/workflows
```

2. Create a workflow file (e.g., `ci.yml`):

```yaml
name: CI

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Install Bats
      run: |
        git clone https://github.com/bats-core/bats-core.git /tmp/bats-core
        export PATH="$PATH:/tmp/bats-core/bin"

    - name: Run tests
      run: bats test/*.bats
```

#### 2.2.2 Running the Workflow

Once the workflow file is created, GitHub Actions will automatically run the defined tests on every push and pull request to the main branch.

### 2.3 Using Travis CI

Travis CI is another popular CI service that integrates with GitHub. To set it up:

1. Create a `.travis.yml` file in your repository:

```yaml
language: bash

script:
  - bats test/*.bats
```

2. Enable Travis CI for your repository on the Travis CI website.

### 2.4 Using CircleCI

CircleCI is another CI/CD tool that can be used for shell scripts. To set it up:

1. Create a `.circleci/config.yml` file:

```yaml
version: 2.1

jobs:
  test:
    docker:
      - image: circleci/bash:latest
    steps:
      - checkout
      - run:
          name: Install Bats
          command: |
            git clone https://github.com/bats-core/bats-core.git /tmp/bats-core
            export PATH="$PATH:/tmp/bats-core/bin"
      - run:
          name: Run tests
          command: bats test/*.bats

workflows:
  version: 2
  test:
    jobs:
      - test
```

2. Push the changes to your repository, and CircleCI will run the tests automatically.

### 2.5 Notifications

Set up notifications for your CI/CD pipeline to alert you of build failures. Most CI tools allow you to configure notifications via email, Slack, or other communication platforms.

## Exercises

1. Write unit tests for a shell script that performs basic arithmetic operations (addition, subtraction, multiplication, and division).

2. Create a CI workflow using GitHub Actions that runs your tests every time you push changes to the repository.

3. Implement a logging mechanism in your scripts and write tests to verify that logs are generated correctly.

4. Write integration tests for a script that interacts with a database, ensuring that the database is in the expected state before and after running the script.

5. Set up Travis CI or CircleCI for a shell script project and ensure that tests are run automatically on each commit.

## Review Questions

1. Why is testing important in shell scripting, and what are the different types of tests you can implement?

2. How does Bats facilitate testing for shell scripts? What are its advantages over writing tests manually?

3. Describe the process of setting up continuous integration using GitHub Actions for a shell script project.

4. What are some common pitfalls to avoid when writing tests for shell scripts?

5. How can you ensure that your CI/CD pipeline is efficient and provides meaningful feedback to developers?

6. Explain the role of static analysis tools like ShellCheck in improving the quality of shell scripts.

7. Discuss the importance of logging in scripts and how it can aid in debugging and monitoring.

8. How would you handle testing for scripts that require external dependencies (e.g., databases, APIs)?

9. What strategies can you use to manage test data and environment configurations in a CI/CD pipeline?

10. Describe how you would implement a rollback mechanism in a CI/CD pipeline for a shell script that modifies system settings or files.

This lesson covers the essential practices of testing and continuous integration for shell scripts, providing detailed explanations, examples, and practical exercises. The exercises and review questions are designed to reinforce the learned concepts and encourage practical application of testing and CI techniques in shell scripting projects.

[Next: 20. Advanced Scripting Techniques](./20_advanced_scripting_techniques.md)