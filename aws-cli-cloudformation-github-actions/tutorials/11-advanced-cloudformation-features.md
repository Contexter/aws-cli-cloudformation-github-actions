### **Tutorial 11: Advanced CloudFormation Features**

In this tutorial, you will explore advanced AWS CloudFormation features such as nested stacks, cross-stack references, and custom resources, which will allow you to manage complex infrastructure setups more efficiently.

---

# Advanced CloudFormation Features

## Overview

AWS CloudFormation offers several advanced features that can help you manage more complex infrastructure setups. These features include nested stacks, cross-stack references, and custom resources, which provide greater flexibility and modularity in your infrastructure management.

In this tutorial, you will:

1. **Learn About Nested Stacks**: Break down large templates into smaller, reusable components using nested stacks.
2. **Use Cross-Stack References**: Share resources between stacks using cross-stack references.
3. **Create Custom Resources**: Extend CloudFormation’s capabilities by creating custom resources.

## 1. Using Nested Stacks

### Step 1: Understand Nested Stacks

Nested stacks allow you to divide your CloudFormation templates into smaller, reusable pieces. A nested stack is a CloudFormation stack that is created as part of another stack. This approach helps manage complex infrastructure by breaking it down into modular components.

### Step 2: Create a Parent Template

Start by creating a parent template that references child templates (nested stacks). Here’s an example of a parent template that creates an S3 bucket and an EC2 instance using nested stacks:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: A parent template that creates nested stacks

Resources:
  S3BucketStack:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.amazonaws.com/my-bucket/s3-bucket-template.yaml
      Parameters:
        BucketName: my-nested-bucket

  EC2InstanceStack:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.amazonaws.com/my-bucket/ec2-instance-template.yaml
      Parameters:
        InstanceType: t2.micro
        KeyName: MyKeyPair
```

### Step 3: Create Child Templates

Next, create the child templates that define the resources. For example, the `s3-bucket-template.yaml` might look like this:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: A nested stack that creates an S3 bucket

Parameters:
  BucketName:
    Type: String
    Description: The name of the S3 bucket

Resources:
  MyS3Bucket:
    Type: 'AWS::S3::Bucket'
    Properties:
      BucketName: !Ref BucketName

Outputs:
  BucketName:
    Value: !Ref MyS3Bucket
```

And the `ec2-instance-template.yaml` might look like this:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: A nested stack that creates an EC2 instance

Parameters:
  InstanceType:
    Type: String
    Description: The EC2 instance type
  KeyName:
    Type: String
    Description: The name of an existing EC2 KeyPair

Resources:
  MyEC2Instance:
    Type: 'AWS::EC2::Instance'
    Properties:
      InstanceType: !Ref InstanceType
      KeyName: !Ref KeyName
      ImageId: ami-0123456789abcdef0

Outputs:
  InstanceId:
    Value: !Ref MyEC2Instance
```

### Step 4: Deploy the Parent Template

Upload the child templates to an S3 bucket and deploy the parent stack:

```bash
aws cloudformation create-stack --stack-name my-parent-stack --template-body file://parent-template.yaml
```

This command will create the nested stacks and the resources defined in them.

## 2. Using Cross-Stack References

### Step 1: Export Resources in One Stack

To share resources between stacks, you can use cross-stack references. First, you need to export the resource you want to share from one stack. For example, in `s3-bucket-template.yaml`, add the following export to the outputs section:

```yaml
Outputs:
  BucketName:
    Value: !Ref MyS3Bucket
    Export:
      Name: MyBucketName
```

### Step 2: Import the Resource in Another Stack

In another stack, you can import the exported resource using the `Fn::ImportValue` function. For example:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: A stack that imports a resource from another stack

Resources:
  MyBucketPolicy:
    Type: 'AWS::S3::BucketPolicy'
    Properties:
      Bucket: !ImportValue MyBucketName
      PolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal: '*'
            Action: 's3:GetObject'
            Resource: !Sub 'arn:aws:s3:::${MyBucketName}/*'
```

This stack creates a bucket policy for the S3 bucket exported by the first stack.

### Step 3: Deploy the Stacks

Deploy the stack that imports the value:

```bash
aws cloudformation create-stack --stack-name my-importing-stack --template-body file://import-template.yaml
```

This will create the resources using the values exported by another stack.

## 3. Creating Custom Resources

### Step 1: Understand Custom Resources

Custom resources in CloudFormation allow you to extend its functionality by writing custom logic, usually in the form of AWS Lambda functions. When a stack with a custom resource is created, updated, or deleted, CloudFormation sends a request to your Lambda function, allowing you to perform any operation not natively supported by CloudFormation.

### Step 2: Create a Lambda Function

First, create a Lambda function that will handle the custom resource requests:

```python
import json
import boto3

def lambda_handler(event, context):
    response = {
        'Status': 'SUCCESS',
        'PhysicalResourceId': 'MyCustomResource',
        'StackId': event['StackId'],
        'RequestId': event['RequestId'],
        'LogicalResourceId': event['LogicalResourceId'],
        'Data': {
            'OutputKey': 'OutputValue'
        }
    }
    print(response)
    return response
```

### Step 3: Create a Custom Resource in CloudFormation

In your CloudFormation template, define the custom resource and link it to the Lambda function:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: A template with a custom resource

Resources:
  MyCustomResource:
    Type: 'Custom::MyCustomResourceType'
    Properties:
      ServiceToken: arn:aws:lambda:us-west-2:123456789012:function:MyLambdaFunction

Outputs:
  CustomResourceOutput:
    Value: !GetAtt MyCustomResource.OutputKey
```

### Step 4: Deploy the Stack with the Custom Resource

Deploy the stack:

```bash
aws cloudformation create-stack --stack-name my-custom-resource-stack --template-body file://custom-resource-template.yaml
```

CloudFormation will invoke the Lambda function as part of the stack creation process.

## Conclusion

You’ve now learned how to use advanced CloudFormation features such as nested stacks, cross-stack references, and custom resources. These features allow you to build more modular, reusable, and extensible infrastructure-as-code templates. In the next tutorial, you’ll learn how to secure your CI/CD pipeline to protect your deployment processes and resources.
