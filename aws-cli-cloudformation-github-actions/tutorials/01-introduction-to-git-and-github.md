### **Tutorial 1: Introduction to Git and GitHub**

---

# Introduction to Git and GitHub

Welcome to the first tutorial in the **Mastering AWS CLI, CloudFormation, Git, and GitHub Actions** series. In this tutorial, we will cover the fundamentals of Git, the most widely used version control system, and GitHub, a platform that enables collaboration and code sharing among developers.

## **Objectives**

By the end of this tutorial, you will:

1. Understand what Git is and why it's important for version control.
2. Learn how to install and configure Git on your local machine.
3. Familiarize yourself with essential Git commands.
4. Create a GitHub account and set up a repository.
5. Learn how to clone, commit, push, pull, and manage branches in Git and GitHub.

## **What is Git?**

Git is a distributed version control system that allows you to track changes in your code, collaborate with others, and manage different versions of your project. It’s an essential tool for any developer, providing a history of changes, the ability to revert to previous states, and a platform for collaboration.

### **Key Features of Git**

- **Version Control**: Keep track of changes over time, so you can revert to specific versions if needed.
- **Branching and Merging**: Work on different features or bug fixes in isolation and then merge them back into the main codebase.
- **Collaboration**: Multiple developers can work on the same project simultaneously without interfering with each other’s work.
- **Distributed System**: Every developer has a complete copy of the project’s history, allowing for offline work and redundancy.

## **Installing Git**

### **On macOS**

Git is typically pre-installed on macOS. You can check if Git is installed by opening Terminal and typing:

```bash
git --version
```

If Git is not installed, you can install it using Homebrew:

```bash
brew install git
```

### **On Windows**

Download and install Git from the official [Git for Windows](https://gitforwindows.org/) page. Once installed, you can use Git Bash, a command-line tool that provides a Unix-style shell.

### **On Linux**

Use the package manager for your Linux distribution to install Git:

```bash
# Ubuntu/Debian
sudo apt-get install git

# CentOS/Fedora
sudo yum install git
```

### **Configuring Git**

After installing Git, you need to configure your username and email address, which will be associated with your commits.

```bash
git config --global user.name "Your Name"
git config --global user.email "youremail@example.com"
```

These configurations will be stored in the global `.gitconfig` file.

## **Getting Started with Git**

### **Initializing a Git Repository**

To start tracking a project with Git, navigate to your project directory and run:

```bash
git init
```

This command initializes a new Git repository, creating a hidden `.git` directory that contains all the metadata and version history.

### **Basic Git Commands**

- **Checking the Status**:
  ```bash
  git status
  ```
  This command shows the current status of your repository, including staged, unstaged, and untracked files.

- **Adding Files**:
  ```bash
  git add filename
  ```
  Stage a file for the next commit. You can stage all files by using `git add .`.

- **Committing Changes**:
  ```bash
  git commit -m "Your commit message"
  ```
  Commit the staged changes with a descriptive message.

- **Viewing Commit History**:
  ```bash
  git log
  ```
  View the commit history for the repository.

## **Creating a GitHub Account**

To use GitHub, you need to create an account:

1. Visit [GitHub](https://github.com/) and sign up for a free account.
2. After signing up, you can create repositories, collaborate with others, and explore open-source projects.

## **Creating a GitHub Repository**

Once you have a GitHub account, you can create a new repository:

1. Log in to GitHub.
2. Click the `+` icon in the upper right corner and select "New repository."
3. Enter a name for your repository, add a description (optional), and choose whether the repository should be public or private.
4. Click "Create repository."

### **Cloning a GitHub Repository**

To clone a repository (copy it to your local machine), use the following command:

```bash
git clone https://github.com/username/repository-name.git
```

Replace `username` with your GitHub username and `repository-name` with the name of your repository.

### **Connecting Your Local Repository to GitHub**

If you have a local repository that you want to push to GitHub, you need to link it to a remote repository:

```bash
git remote add origin https://github.com/username/repository-name.git
```

To push your commits to GitHub:

```bash
git push -u origin main
```

This command pushes the local `main` branch to the remote repository.

## **Working with Branches**

Branches allow you to work on different features or bug fixes without affecting the main codebase. 

### **Creating a Branch**

```bash
git branch feature-branch
```

### **Switching to a Branch**

```bash
git checkout feature-branch
```

### **Merging a Branch**

After completing work on a branch, you can merge it back into the `main` branch:

1. Switch to the `main` branch:
   ```bash
   git checkout main
   ```

2. Merge the branch:
   ```bash
   git merge feature-branch
   ```

### **Deleting a Branch**

```bash
git branch -d feature-branch
```

## **Collaborating on GitHub**

### **Forking a Repository**

Forking creates a personal copy of someone else’s repository in your GitHub account. You can make changes to the forked repository and later propose these changes to the original repository via a pull request.

### **Creating a Pull Request**

A pull request allows you to notify the repository’s maintainers of changes you’ve pushed to a fork. They can review the changes and, if approved, merge them into the original repository.

1. Push your changes to a branch in your forked repository.
2. Go to the original repository on GitHub.
3. Click "New pull request."
4. Select the branch you want to merge from your fork and create the pull request.

## **Summary**

In this tutorial, you learned the basics of Git and GitHub, including how to install Git, create and manage repositories, and collaborate with others. These skills are foundational for the rest of this guide, where you’ll apply version control and collaboration techniques to manage your AWS infrastructure as code.

### **Next Steps**

Move on to **[Setting Up GitHub Actions](02-setting-up-github-actions.md)**, where you’ll learn how to automate tasks using GitHub Actions, setting the stage for more advanced CI/CD pipelines.

---

This content will provide a solid foundation in Git and GitHub, preparing you for the more advanced topics that follow. Feel free to copy this content into the corresponding `.md` file in your repository!
