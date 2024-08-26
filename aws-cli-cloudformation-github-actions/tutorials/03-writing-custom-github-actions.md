### **Tutorial 3: Writing Custom GitHub Actions**

This tutorial will guide you through the process of creating your own custom GitHub Actions to tailor your workflows specifically to your needs.

---

# Writing Custom GitHub Actions

## Overview

While GitHub offers a wide range of pre-built actions, there may be cases where you need something highly specific to your project. In such cases, creating a custom GitHub Action is the way to go. Custom actions can be created using JavaScript or by defining a Docker container.

In this tutorial, you will:

1. **Understand the Structure of a Custom Action**: Learn the basics of how custom GitHub Actions are structured.
2. **Create a Simple JavaScript Action**: Build a custom action using JavaScript.
3. **Test and Publish Your Action**: Ensure your action works as expected and make it available for others to use.

## 1. Understanding the Structure of a Custom Action

A custom GitHub Action typically consists of three main components:

- **Metadata File (`action.yml`)**: Defines the action's inputs, outputs, and main entry point.
- **Code**: The logic of your action, written in JavaScript or a Dockerfile.
- **README.md**: A documentation file explaining how to use your action.

### Example Metadata (`action.yml`)

Here's a basic `action.yml` file structure:

```yaml
name: 'My Custom Action'
description: 'An example custom action to demonstrate structure'
inputs:
  example_input:
    description: 'An example input value'
    required: true
    default: 'default value'
outputs:
  example_output:
    description: 'An example output value'
runs:
  using: 'node12'
  main: 'index.js'
```

This file describes an action that takes an `example_input` and produces an `example_output`, with the logic contained in `index.js`.

## 2. Creating a Simple JavaScript Action

### Step 1: Set Up Your Repository

Start by creating a new directory for your action within your GitHub repository:

```bash
mkdir -p .github/actions/my-custom-action
cd .github/actions/my-custom-action
```

### Step 2: Create the Metadata File (`action.yml`)

Inside the `my-custom-action` directory, create the `action.yml` file:

```bash
touch action.yml
```

Add the following content to `action.yml`:

```yaml
name: 'My Custom Action'
description: 'A custom action that echoes an input message'
inputs:
  message:
    description: 'The message to echo'
    required: true
runs:
  using: 'node12'
  main: 'index.js'
```

### Step 3: Write the Action Code (`index.js`)

Create an `index.js` file where you will write the JavaScript logic for your action:

```bash
touch index.js
```

Add the following simple code to `index.js`:

```javascript
const core = require('@actions/core');

try {
  // Get the input message
  const message = core.getInput('message');
  console.log(`Hello, ${message}!`);
  
  // Set an output variable
  core.setOutput("echoedMessage", message);
} catch (error) {
  core.setFailed(`Action failed with error: ${error.message}`);
}
```

### Step 4: Add Dependencies

Initialize the Node.js project and install the necessary dependencies:

```bash
npm init -y
npm install @actions/core
```

### Step 5: Test Your Action Locally

You can test your action locally using a simple Node.js script:

```bash
node index.js
```

Make sure to pass in the necessary environment variables for testing, or hard-code test values.

### Step 6: Commit and Push Your Action

Once you're satisfied with your action, commit and push it to your repository:

```bash
git add .
git commit -m "Add custom GitHub Action"
git push origin main
```

## 3. Using Your Custom Action in a Workflow

Now that your custom action is in your repository, you can use it in your workflows like this:

```yaml
name: Use Custom Action

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Run custom action
      uses: ./.github/actions/my-custom-action
      with:
        message: "This is a custom action!"
```

## Conclusion

You’ve now created and tested your first custom GitHub Action. Custom actions are powerful tools that let you tailor your workflows to fit your exact needs. In the next tutorial, you'll explore how to build a CI pipeline using GitHub Actions to automate your software development process.
