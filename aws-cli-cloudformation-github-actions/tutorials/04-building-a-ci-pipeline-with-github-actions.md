### **Tutorial 4: Building a CI Pipeline with GitHub Actions**

In this tutorial, you will learn how to build a Continuous Integration (CI) pipeline using GitHub Actions to automate the process of testing and validating your code every time changes are made.

---

# Building a CI Pipeline with GitHub Actions

## Overview

A Continuous Integration (CI) pipeline is an automated process that builds and tests your code every time you make a change. This ensures that your code is always in a deployable state, catching issues early in the development cycle. GitHub Actions is a powerful tool that allows you to easily set up a CI pipeline directly in your GitHub repository.

In this tutorial, you will:

1. **Set Up a Basic CI Pipeline**: Create a workflow that automatically runs tests every time you push code to your repository.
2. **Run Tests Across Multiple Environments**: Learn how to configure your pipeline to test your code in different environments.
3. **Use Build Artifacts**: Save and upload build artifacts for later use.

## 1. Setting Up a Basic CI Pipeline

### Step 1: Create a New Workflow

Create a new workflow file named `ci.yml` in the `.github/workflows` directory:

```bash
mkdir -p .github/workflows
touch .github/workflows/ci.yml
```

### Step 2: Define the Workflow

Open the `ci.yml` file in a text editor and define the basic structure of your CI pipeline:

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
  test:
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

This workflow does the following:

- **Triggers** on pushes and pull requests to the `main` branch.
- **Runs** on the latest Ubuntu environment.
- **Checks out** the code from the repository.
- **Sets up** a Node.js environment with version 14.
- **Installs dependencies** using `npm install`.
- **Runs tests** with `npm test`.

### Step 3: Commit and Push the Workflow

Commit and push your new CI workflow to the repository:

```bash
git add .github/workflows/ci.yml
git commit -m "Add basic CI pipeline"
git push origin main
```

Once pushed, GitHub will automatically execute the workflow every time the conditions defined in `on` are met.

## 2. Running Tests Across Multiple Environments

You might want to ensure that your code works in different environments, such as various Node.js versions. You can configure your workflow to run tests across multiple environments.

### Step 1: Modify the Workflow to Include a Matrix Strategy

Open the `ci.yml` file and modify it to include a matrix strategy:

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
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [12, 14, 16]

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: ${{ matrix.node-version }}

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test
```

This configuration runs the tests on Node.js versions 12, 14, and 16.

### Step 2: Commit and Push

Commit and push your changes:

```bash
git add .github/workflows/ci.yml
git commit -m "Run CI pipeline on multiple Node.js versions"
git push origin main
```

With this configuration, GitHub Actions will run your tests on all specified Node.js versions, ensuring your code is compatible with different environments.

## 3. Using Build Artifacts

Build artifacts are files generated during the CI process that you might want to store or share, such as test reports or compiled binaries.

### Step 1: Modify the Workflow to Save Artifacts

You can save artifacts in your workflow by adding an `upload-artifact` step:

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
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [12, 14, 16]

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: ${{ matrix.node-version }}

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test

    - name: Upload test results
      if: always()
      uses: actions/upload-artifact@v2
      with:
        name: test-results
        path: test-results/
```

This workflow assumes that your tests generate a report in the `test-results/` directory. The `upload-artifact` step will save these files and make them accessible via the GitHub Actions interface.

### Step 2: Commit and Push

After modifying the workflow, commit and push your changes:

```bash
git add .github/workflows/ci.yml
git commit -m "Add artifact upload step to CI pipeline"
git push origin main
```

## Conclusion

You’ve now built a basic CI pipeline using GitHub Actions, expanded it to run across multiple environments, and learned how to save build artifacts. This setup ensures that your code is tested thoroughly every time you make changes, providing confidence that your project remains in a deployable state. In the next tutorial, we will dive into using AWS CLI for managing AWS resources.
