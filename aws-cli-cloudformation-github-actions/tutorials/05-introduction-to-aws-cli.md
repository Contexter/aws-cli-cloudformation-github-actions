### **Tutorial 5: Introduction to AWS CLI**

In this tutorial, you will learn how to use the AWS Command Line Interface (CLI) to manage AWS resources directly from your terminal.

---

# Introduction to AWS CLI

## Overview

The AWS Command Line Interface (CLI) is a powerful tool that allows you to interact with AWS services directly from your terminal. Whether you want to automate tasks, manage resources, or integrate AWS services into your workflows, the AWS CLI provides a flexible and efficient way to do so.

In this tutorial, you will:

1. **Install and Configure AWS CLI**: Set up the AWS CLI on your local machine.
2. **Learn Basic AWS CLI Commands**: Get familiar with the most commonly used AWS CLI commands.
3. **Automate AWS Tasks**: Use the AWS CLI to automate common AWS management tasks.

## 1. Installing and Configuring AWS CLI

### Step 1: Install AWS CLI

AWS CLI can be installed on various operating systems. Here’s how to install it on your system:

#### macOS and Linux

Install using `brew` (for macOS) or `curl` (for Linux):

```bash
# macOS using Homebrew
brew install awscli

# Linux using curl
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

#### Windows

Download the installer from the official AWS CLI page and run it.

### Step 2: Verify Installation

After installing, verify the installation by checking the version:

```bash
aws --version
```

You should see output indicating the AWS CLI version installed on your system.

### Step 3: Configure AWS CLI

Before using AWS CLI, you need to configure it with your AWS credentials. Run the following command:

```bash
aws configure
```

You will be prompted to enter:

- **AWS Access Key ID**: Your AWS account’s access key.
- **AWS Secret Access Key**: The corresponding secret key.
- **Default region name**: The AWS region you want to work in (e.g., `us-west-2`).
- **Default output format**: The format for CLI output (e.g., `json`).

This will set up your CLI to interact with AWS using your credentials.

## 2. Learning Basic AWS CLI Commands

### Step 1: List S3 Buckets

Let’s start with a simple command to list all S3 buckets in your account:

```bash
aws s3 ls
```

This command will display a list of all S3 buckets in the region you’ve configured.

### Step 2: Launch an EC2 Instance

You can launch an EC2 instance using the following command:

```bash
aws ec2 run-instances \
    --image-id ami-0123456789abcdef0 \
    --count 1 \
    --instance-type t2.micro \
    --key-name MyKeyPair \
    --security-group-ids sg-0123456789abcdef0 \
    --subnet-id subnet-0123456789abcdef0
```

Replace the placeholders (like `ami-0123456789abcdef0`) with your actual values.

### Step 3: Describe EC2 Instances

To see the details of your running EC2 instances, use:

```bash
aws ec2 describe-instances
```

This will return detailed information about all EC2 instances in your account.

### Step 4: Manage IAM Users

Create a new IAM user:

```bash
aws iam create-user --user-name NewUser
```

To list all IAM users in your account:

```bash
aws iam list-users
```

## 3. Automating AWS Tasks with AWS CLI

### Step 1: Automate S3 Backups

Create a script to automate S3 backups by syncing a local directory to an S3 bucket:

```bash
#!/bin/bash
aws s3 sync /path/to/local/directory s3://your-s3-bucket-name
```

Save this script as `s3_backup.sh`, make it executable, and schedule it using cron (on Linux/macOS) or Task Scheduler (on Windows).

### Step 2: Automate EC2 Instance Management

Create a script to start and stop an EC2 instance:

```bash
#!/bin/bash

# Start EC2 instance
aws ec2 start-instances --instance-ids i-0123456789abcdef0

# Stop EC2 instance
aws ec2 stop-instances --instance-ids i-0123456789abcdef0
```

Save this script as `manage_ec2.sh` and run it as needed to control your EC2 instances.

## Conclusion

You’ve now learned how to install, configure, and use the AWS CLI to manage AWS resources. The AWS CLI is a powerful tool for automating cloud management tasks and integrating AWS into your development workflows. In the next tutorial, you’ll dive deeper into managing AWS resources with more advanced AWS CLI commands and scripts.
