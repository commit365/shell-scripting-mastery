# Version Control with Git

Version control is an essential skill for any programmer, including shell script developers. This lesson covers basic Git operations and how to collaborate on scripts using GitHub.

## 1. Basic Git Operations

### 1.1 Installing Git

On most Unix-like systems, Git can be installed using the package manager:

```bash
# For Debian/Ubuntu
sudo apt-get update
sudo apt-get install git

# For macOS (using Homebrew)
brew install git
```

Verify the installation:

```bash
git --version
```

### 1.2 Configuring Git

Set up your identity:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

Set your default editor:

```bash
git config --global core.editor "vim"
```

### 1.3 Initializing a Repository

To start version-controlling your scripts:

```bash
cd /path/to/your/project
git init
```

### 1.4 Checking Repository Status

To see the current state of your repository:

```bash
git status
```

### 1.5 Staging Changes

To stage files for commit:

```bash
git add filename.sh
# Or to stage all changes:
git add .
```

### 1.6 Committing Changes

To commit staged changes:

```bash
git commit -m "Add initial version of script"
```

To commit all changes (staging and committing in one step):

```bash
git commit -am "Update error handling in script"
```

### 1.7 Viewing Commit History

To see the commit history:

```bash
git log
# For a more concise view:
git log --oneline
```

### 1.8 Creating and Switching Branches

To create a new branch:

```bash
git branch new-feature
```

To switch to a branch:

```bash
git checkout new-feature
```

To create and switch in one command:

```bash
git checkout -b new-feature
```

### 1.9 Merging Branches

To merge changes from another branch:

```bash
git checkout main
git merge new-feature
```

### 1.10 Handling Merge Conflicts

If there are conflicts, Git will mark them in the affected files. Edit the files to resolve conflicts, then:

```bash
git add .
git commit -m "Resolve merge conflicts"
```

### 1.11 Viewing Differences

To see changes in staged files:

```bash
git diff --staged
```

To see changes in unstaged files:

```bash
git diff
```

### 1.12 Undoing Changes

To unstage changes:

```bash
git reset HEAD filename.sh
```

To discard changes in working directory:

```bash
git checkout -- filename.sh
```

To undo the last commit:

```bash
git reset --soft HEAD^
```

### 1.13 Tagging

To create a lightweight tag:

```bash
git tag v1.0
```

To create an annotated tag:

```bash
git tag -a v1.0 -m "Version 1.0 release"
```

## 2. Collaborating on Scripts Using GitHub

### 2.1 Creating a GitHub Account

Visit [GitHub](https://github.com) and sign up for an account if you don't have one.

### 2.2 Creating a Repository on GitHub

1. Click the "+" icon in the top right corner
2. Select "New repository"
3. Fill in repository name, description, and other settings
4. Click "Create repository"

### 2.3 Connecting Local Repository to GitHub

To add a remote repository:

```bash
git remote add origin https://github.com/username/repo-name.git
```

To push your local repository to GitHub:

```bash
git push -u origin main
```

### 2.4 Cloning a Repository

To clone an existing repository:

```bash
git clone https://github.com/username/repo-name.git
```

### 2.5 Fetching and Pulling Changes

To fetch changes without merging:

```bash
git fetch origin
```

To fetch and merge changes:

```bash
git pull origin main
```

### 2.6 Creating Pull Requests

1. Create a new branch: `git checkout -b feature-branch`
2. Make changes and commit them
3. Push the branch to GitHub: `git push origin feature-branch`
4. Go to the GitHub repository page
5. Click "Compare & pull request"
6. Fill in the pull request details and create it

### 2.7 Reviewing and Merging Pull Requests

1. Go to the "Pull requests" tab on GitHub
2. Click on a pull request to review it
3. Add comments or request changes if needed
4. Click "Merge pull request" when ready

### 2.8 Forking a Repository

To contribute to a project you don't have write access to:

1. Click the "Fork" button on the GitHub repository page
2. Clone your forked repository
3. Make changes and push to your fork
4. Create a pull request from your fork to the original repository

### 2.9 Keeping a Fork Updated

Add the original repository as a remote:

```bash
git remote add upstream https://github.com/original-owner/original-repo.git
```

Fetch and merge changes:

```bash
git fetch upstream
git checkout main
git merge upstream/main
```

### 2.10 Using GitHub Issues

1. Go to the "Issues" tab on GitHub
2. Click "New issue"
3. Provide a title and description
4. Assign labels, milestones, and assignees as needed
5. Submit the issue

### 2.11 GitHub Actions for Continuous Integration

Create a `.github/workflows/ci.yml` file in your repository:

```yaml
name: CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Run tests
      run: |
        chmod +x ./test_script.sh
        ./test_script.sh
```

This will run `test_script.sh` on every push and pull request.

### 2.12 GitHub Pages for Documentation

To host documentation for your scripts:

1. Go to repository settings
2. Scroll to "GitHub Pages" section
3. Choose source (e.g., main branch, docs folder)
4. Your documentation will be available at `https://username.github.io/repo-name/`

## Exercises

1. Create a new Git repository for a shell script project, make several commits with different changes, and push it to GitHub.

2. Clone a public repository from GitHub, create a new branch, make some improvements to a script, and create a pull request.

3. Simulate a merge conflict by having two team members edit the same part of a script, then resolve the conflict.

4. Set up a GitHub Action that runs shellcheck on your scripts whenever changes are pushed.

5. Create a comprehensive `.gitignore` file for a shell scripting project, considering various operating systems and text editors.

6. Use Git hooks to automatically run tests before committing changes to your scripts.

7. Create a GitHub wiki for one of your script repositories, documenting its usage and configuration options.

## Review Questions

1. Explain the difference between `git fetch` and `git pull`. In what scenarios would you use one over the other?

2. What is the purpose of branching in Git? Describe a branching strategy that would work well for collaborative script development.

3. How does Git's staging area (index) work, and why is it useful in the commit process?

4. Describe the process of resolving a merge conflict in Git. What tools can assist in this process?

5. What are the advantages of using pull requests for collaboration, as opposed to direct commits to the main branch?

6. Explain the concept of rebasing in Git. When might you choose to rebase instead of merge?

7. How can you use Git tags, and why are they useful in script development?

8. Describe the process of reverting a commit in Git. How does this differ from resetting?

9. What are Git hooks, and how can they be used to improve the development workflow for shell scripts?

10. Explain how you would set up a GitHub Action to automatically test and deploy a shell script to a server whenever changes are pushed to the main branch.

This lesson covers fundamental and advanced concepts of version control with Git and collaboration using GitHub, specifically in the context of shell script development. The exercises and review questions are designed to reinforce these concepts and encourage practical application of version control in real-world scripting scenarios.

[Next: 17. Building a Complex Project](./17_building_a_complex_project.md)