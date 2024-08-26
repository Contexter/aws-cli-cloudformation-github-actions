### **Tutorial 2: Setting Up GitHub Actions**

This tutorial will guide you through setting up GitHub Actions to automate tasks in your repository.

---

# Setting Up GitHub Actions

## Overview

GitHub Actions is a powerful automation tool integrated into GitHub that allows you to automate your software development workflows directly in your repository. Whether it's running tests, building your code, or deploying applications, GitHub Actions can automate these tasks.

In this tutorial, you will:

1. **Understand GitHub Actions**: Learn the basics of what GitHub Actions are and how they can be used.
2. **Set Up Your First Workflow**: Create and configure your first GitHub Action workflow.
3. **Use Pre-Built Actions**: Leverage actions from the GitHub Marketplace to speed up your automation setup.

## 1. Understanding GitHub Actions

GitHub Actions allows you to automate tasks in your development lifecycle, including continuous integration (CI) and continuous deployment (CD). Workflows are the core of GitHub Actions, and they consist of one or more jobs that can be triggered by events like pushing code or opening a pull request.

### Key Concepts:
- **Workflows**: YAML files that define automated processes.
- **Events**: Triggers that start workflows, such as code pushes or pull requests.
- **Jobs**: A sequence of steps executed on a runner (an environment where your code is executed).
- **Actions**: Individual tasks that are executed as part of a job.

## 2. Setting Up Your First Workflow

### Step 1: Create a `.github` Directory

Start by creating a directory in your repository where GitHub Actions will look for workflow files.

```bash
mkdir -p .github/workflows
```

### Step 2: Create a Workflow File

Create a new YAML file for your workflow inside the `.github/workflows` directory. Let's name it `main.yml`.

```bash
touch .github/workflows/main.yml
```

### Step 3: Define Your Workflow

Open the `main.yml` file in a text editor and add the following basic workflow:

```yaml
name: CI Pipeline

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Run a one-liner script
      run: echo "Hello, GitHub Actions!"
```

This workflow does the following:
- **Triggers** on pushes and pull requests to the `main` branch.
- **Runs** on the latest version of Ubuntu.
- **Checks out** the code from the repository.
- **Executes** a simple echo command.

### Step 4: Commit and Push

Commit your changes and push them to your repository:

```bash
git add .github/workflows/main.yml
git commit -m "Add initial GitHub Actions workflow"
git push origin main
```

Once pushed, GitHub will automatically detect the workflow and execute it whenever the specified events occur.

## 3. Using Pre-Built Actions

GitHub Marketplace provides a wide array of pre-built actions that you can include in your workflows. For example, you can use an action to set up a Node.js environment, run tests, or deploy code.

### Example: Using a Node.js Setup Action

To set up a Node.js environment in your workflow, modify the `main.yml` as follows:

```yaml
name: CI Pipeline

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '14'

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test
```

This updated workflow:
- **Sets up** a Node.js environment.
- **Installs** the necessary dependencies.
- **Runs** your tests.

## Conclusion

You've now set up your first GitHub Actions workflow and integrated a pre-built action to automate your CI process. In the next tutorial, you'll dive deeper into creating custom actions to further customize your workflows.
