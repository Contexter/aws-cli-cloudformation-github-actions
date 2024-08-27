### **Tutorial 3: Writing Custom GitHub Actions**

In this tutorial, we will focus on creating and testing a **custom GitHub Action** using **Node.js** and **Docker**. Everything will be executed on **GitHub runners**, ensuring centralized logging and consistent environments without local machine dependencies.

---

## Writing Custom GitHub Actions

### 1. **Understanding the Structure of a Custom Action**

A custom GitHub Action typically includes:
- **Metadata File (`action.yml`)**: Defines inputs, outputs, and execution details.
- **Action Code**: Logic written in JavaScript (Node.js) or defined in a Dockerfile.

### 2. **Creating a Simple JavaScript Action (Node.js)**

**Node.js** is ideal for lightweight actions that integrate well with GitHub’s ecosystem.

**Step 1: Set Up Your Repository**

Create a directory for your action:

```bash
mkdir -p .github/actions/my-custom-action
cd .github/actions/my-custom-action
```

**Step 2: Create the Metadata File (`action.yml`)**

Define the action’s metadata in `action.yml`:

```bash
touch action.yml
```

```yaml
name: 'My Custom Node.js Action'
description: 'A Node.js action that echoes an input message'
inputs:
  message:
    description: 'The message to echo'
    required: true
runs:
  using: 'node12'
  main: 'index.js'
```

**Step 3: Write the Action Code (`index.js`)**

Create `index.js` and add the logic:

```bash
touch index.js
```

```javascript
const core = require('@actions/core');

try {
  const message = core.getInput('message');
  console.log(`Hello, ${message}!`);

  core.setOutput("echoedMessage", message);
} catch (error) {
  core.setFailed(`Action failed with error: ${error.message}`);
}
```

**Step 4: Running the Action on GitHub Runners**

When your action is triggered in a GitHub workflow, the runner automatically initializes the Node.js environment and installs dependencies. You don’t need to manually run `npm init` or `npm install` on your local machine—this is handled by the runner:

```yaml
name: Test Custom Action

on: [push]

jobs:
  test-action:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Run custom action
      uses: ./.github/actions/my-custom-action
      with:
        message: "Test message from workflow"
```

### 3. **Alternative: Docker-Based Action**

If your action requires a specific environment or more complex dependencies, you can create a **Docker-based** action.

**Step 1: Create a Dockerfile**

Define your environment in a `Dockerfile`:

```bash
touch Dockerfile
```

```Dockerfile
FROM node:12-alpine

WORKDIR /usr/src/app

COPY . .

RUN npm install

ENTRYPOINT ["node", "/usr/src/app/index.js"]
```

**Step 2: Modify `action.yml` for Docker**

Update `action.yml` to use Docker:

```yaml
runs:
  using: 'docker'
  image: 'Dockerfile'
```

### 4. **Conclusion: Centralized Execution and Logging**

Running your custom actions on **GitHub runners** centralizes execution and logging, making it easier to maintain and scale your workflows. Whether you use **Node.js** for simplicity or **Docker** for more control, testing and running actions directly in the cloud ensures consistency and reliability.
