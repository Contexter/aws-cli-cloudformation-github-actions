### **Tutorial 6: Managing AWS Resources with AWS CLI**

In this tutorial, you will explore more advanced AWS CLI commands to manage various AWS resources such as EC2 instances, S3 buckets, and IAM roles. You will also learn how to use scripts to automate these tasks.

---

# Managing AWS Resources with AWS CLI

## Overview

The AWS CLI is a versatile tool that allows you to manage a wide range of AWS resources from the command line. In this tutorial, you will build on your basic knowledge of AWS CLI to manage more complex tasks involving EC2 instances, S3 buckets, and IAM roles. You will also learn how to create reusable scripts to automate these tasks.

In this tutorial, you will:

1. **Manage EC2 Instances**: Start, stop, and terminate EC2 instances using the AWS CLI.
2. **Work with S3 Buckets**: Create, list, and delete S3 buckets, and manage the objects within them.
3. **Manage IAM Roles and Policies**: Create and manage IAM roles and policies.
4. **Automate AWS Resource Management**: Write scripts to automate common AWS management tasks.

## 1. Managing EC2 Instances

### Step 1: Launch an EC2 Instance

To launch a new EC2 instance, you can use the following command:

```bash
aws ec2 run-instances \
    --image-id ami-0123456789abcdef0 \
    --count 1 \
    --instance-type t2.micro \
    --key-name MyKeyPair \
    --security-group-ids sg-0123456789abcdef0 \
    --subnet-id subnet-0123456789abcdef0
```

Make sure to replace the placeholders with your actual values.

### Step 2: Start and Stop EC2 Instances

To start an EC2 instance that you’ve previously stopped:

```bash
aws ec2 start-instances --instance-ids i-0123456789abcdef0
```

To stop a running EC2 instance:

```bash
aws ec2 stop-instances --instance-ids i-0123456789abcdef0
```

### Step 3: Terminate an EC2 Instance

To terminate an EC2 instance permanently:

```bash
aws ec2 terminate-instances --instance-ids i-0123456789abcdef0
```

### Step 4: Describe EC2 Instances

To retrieve information about your EC2 instances:

```bash
aws ec2 describe-instances
```

This command returns detailed information about all EC2 instances in your account.

## 2. Working with S3 Buckets

### Step 1: Create an S3 Bucket

To create a new S3 bucket:

```bash
aws s3 mb s3://your-bucket-name
```

### Step 2: List S3 Buckets

To list all S3 buckets in your AWS account:

```bash
aws s3 ls
```

### Step 3: Upload Files to S3

To upload a file to your S3 bucket:

```bash
aws s3 cp /path/to/local/file s3://your-bucket-name/
```

### Step 4: Download Files from S3

To download a file from your S3 bucket:

```bash
aws s3 cp s3://your-bucket-name/your-file.txt /path/to/local/directory
```

### Step 5: Delete an S3 Bucket

To delete an S3 bucket and all its contents:

```bash
aws s3 rb s3://your-bucket-name --force
```

## 3. Managing IAM Roles and Policies

### Step 1: Create an IAM Role

To create a new IAM role:

```bash
aws iam create-role --role-name MyNewRole --assume-role-policy-document file://trust-policy.json
```

The `trust-policy.json` file should define the trust relationship for the role.

### Step 2: Attach a Policy to an IAM Role

To attach a policy to your IAM role:

```bash
aws iam attach-role-policy --role-name MyNewRole --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
```

### Step 3: List IAM Roles

To list all IAM roles in your account:

```bash
aws iam list-roles
```

### Step 4: Delete an IAM Role

To delete an IAM role:

```bash
aws iam delete-role --role-name MyNewRole
```

## 4. Automating AWS Resource Management

### Step 1: Script to Manage EC2 Instances

You can create a script to automate starting, stopping, and terminating EC2 instances. Here’s an example:

```bash
#!/bin/bash

# Start EC2 instance
aws ec2 start-instances --instance-ids i-0123456789abcdef0

# Stop EC2 instance
aws ec2 stop-instances --instance-ids i-0123456789abcdef0

# Terminate EC2 instance
aws ec2 terminate-instances --instance-ids i-0123456789abcdef0
```

Save this script as `manage_ec2.sh`, make it executable, and run it to manage your EC2 instances.

### Step 2: Script to Sync Files with S3

Here’s a script to automate syncing files between your local system and an S3 bucket:

```bash
#!/bin/bash

# Sync local directory to S3
aws s3 sync /path/to/local/directory s3://your-bucket-name

# Sync S3 bucket to local directory
aws s3 sync s3://your-bucket-name /path/to/local/directory
```

Save this script as `s3_sync.sh`, make it executable, and run it to keep your local files and S3 bucket in sync.

## Conclusion

You’ve now learned how to manage various AWS resources using the AWS CLI, from EC2 instances to S3 buckets and IAM roles. By creating scripts to automate these tasks, you can streamline your AWS resource management and integrate it into your workflows. In the next tutorial, we will introduce you to AWS CloudFormation and how to use it for Infrastructure as Code (IaC).
