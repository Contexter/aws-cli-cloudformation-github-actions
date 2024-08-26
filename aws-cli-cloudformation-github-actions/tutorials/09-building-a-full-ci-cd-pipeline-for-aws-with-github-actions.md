### **Tutorial 9: Building a Full CI/CD Pipeline for AWS with GitHub Actions**

In this tutorial, you will learn how to build a complete CI/CD (Continuous Integration/Continuous Deployment) pipeline that integrates GitHub Actions with AWS. This will automate the process of deploying infrastructure and applications to AWS.

---

# Building a Full CI/CD Pipeline for AWS with GitHub Actions

## Overview

A Continuous Integration/Continuous Deployment (CI/CD) pipeline automates the process of building, testing, and deploying your applications and infrastructure. By integrating GitHub Actions with AWS, you can automate the deployment of your AWS infrastructure using CloudFormation templates, as well as deploy your applications to AWS services like EC2, S3, or Lambda.

In this tutorial, you will:

1. **Set Up AWS Credentials in GitHub Actions**: Configure GitHub Actions to securely interact with your AWS account.
2. **Create a Deployment Workflow**: Build a workflow that deploys a CloudFormation stack whenever changes are pushed to your repository.
3. **Automate Application Deployment**: Extend the workflow to deploy an application to an AWS service.

## 1. Setting Up AWS Credentials in GitHub Actions

### Step 1: Generate AWS Access Keys

To allow GitHub Actions to deploy resources to AWS, you need to generate AWS access keys. These keys consist of an Access Key ID and a Secret Access Key.

1. Log in to the AWS Management Console.
2. Navigate to the **IAM** (Identity and Access Management) service.
3. Create a new IAM user with programmatic access and attach the necessary permissions (e.g., AdministratorAccess or a custom policy with specific permissions).
4. Download the Access Key ID and Secret Access Key.

### Step 2: Add AWS Credentials to GitHub Secrets

Now, add your AWS credentials to your GitHub repository as secrets:

1. Go to your GitHub repository.
2. Click on **Settings** > **Secrets and variables** > **Actions** > **New repository secret**.
3. Add two secrets:
   - `AWS_ACCESS_KEY_ID` with the value of your Access Key ID.
   - `AWS_SECRET_ACCESS_KEY` with the value of your Secret Access Key.

These secrets will be used by GitHub Actions to authenticate to your AWS account.

## 2. Creating a Deployment Workflow

### Step 1: Define the Workflow

Create a new workflow file named `deploy.yml` in the `.github/workflows` directory:

```bash
mkdir -p .github/workflows
touch .github/workflows/deploy.yml
```

### Step 2: Write the Workflow to Deploy a CloudFormation Stack

Open `deploy.yml` and define the following workflow:

```yaml
name: Deploy to AWS

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-west-2

    - name: Deploy CloudFormation stack
      run: |
        aws cloudformation deploy \
          --template-file cloudformation_template.yaml \
          --stack-name my-cloudformation-stack \
          --capabilities CAPABILITY_IAM
```

This workflow:

- **Triggers** on pushes to the `main` branch.
- **Configures** AWS credentials using the secrets you added.
- **Deploys** a CloudFormation stack using the `aws cloudformation deploy` command.

### Step 3: Add a CloudFormation Template

Create the CloudFormation template `cloudformation_template.yaml` in the root of your repository:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Deploying resources via GitHub Actions

Resources:
  MyS3Bucket:
    Type: 'AWS::S3::Bucket'
    Properties:
      BucketName: my-deployment-bucket

Outputs:
  BucketName:
    Value: !Ref MyS3Bucket
    Description: The name of the S3 bucket created
```

This template creates an S3 bucket named `my-deployment-bucket`.

### Step 4: Commit and Push the Workflow

Commit and push your new workflow and CloudFormation template to the repository:

```bash
git add .github/workflows/deploy.yml cloudformation_template.yaml
git commit -m "Add CI/CD pipeline for deploying CloudFormation stack"
git push origin main
```

When you push to the `main` branch, GitHub Actions will automatically deploy the CloudFormation stack to AWS.

## 3. Automating Application Deployment

### Step 1: Extend the Workflow for Application Deployment

To deploy an application, such as a website to S3, modify your `deploy.yml` to include the following steps:

```yaml
- name: Sync files to S3
  run: |
    aws s3 sync ./my-app s3://my-deployment-bucket --delete
```

This step synchronizes the files from the `./my-app` directory in your repository to the S3 bucket.

### Step 2: Add Application Files

Add your application files (e.g., an HTML file) to a directory named `my-app`:

```bash
mkdir my-app
echo "<h1>Hello from my deployed app!</h1>" > my-app/index.html
```

### Step 3: Commit and Push the Changes

Commit and push your application files along with the updated workflow:

```bash
git add my-app
git commit -m "Add application files for deployment"
git push origin main
```

After pushing, GitHub Actions will deploy your application to the S3 bucket.

## Conclusion

You’ve now set up a complete CI/CD pipeline using GitHub Actions to deploy both infrastructure and an application to AWS. This automation ensures that your infrastructure and applications are consistently deployed whenever changes are made to your repository. In the next tutorial, you'll learn how to monitor and troubleshoot your AWS resources using the AWS CLI and GitHub Actions.
