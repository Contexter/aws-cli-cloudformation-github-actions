### **Tutorial 8: Parameterizing and Reusing CloudFormation Templates**

In this tutorial, you will learn how to create flexible and reusable CloudFormation templates by using parameters, mappings, and outputs.

---

# Parameterizing and Reusing CloudFormation Templates

## Overview

To make your AWS CloudFormation templates more flexible and reusable, you can parameterize them. Parameters allow you to pass in different values when you create or update a stack, making your templates adaptable to different environments or configurations. Additionally, you can use mappings and outputs to further customize and extract information from your templates.

In this tutorial, you will:

1. **Learn How to Use Parameters**: Add parameters to your CloudFormation templates to make them flexible and reusable.
2. **Use Mappings for Conditional Resource Creation**: Utilize mappings to create resources based on different conditions.
3. **Extract and Use Outputs**: Define outputs to extract and use key information from your stack.

## 1. Using Parameters in CloudFormation Templates

### Step 1: Add Parameters to Your Template

Parameters allow you to pass values into your CloudFormation template when creating or updating a stack. These can include things like instance types, bucket names, or environment configurations.

Here’s an example of how to add a parameter to a CloudFormation template:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: A CloudFormation template with parameters

Parameters:
  InstanceTypeParameter:
    Type: String
    Default: t2.micro
    Description: EC2 instance type

Resources:
  MyEC2Instance:
    Type: 'AWS::EC2::Instance'
    Properties:
      InstanceType: !Ref InstanceTypeParameter
      ImageId: ami-0123456789abcdef0
```

In this example, the `InstanceTypeParameter` parameter allows you to specify the EC2 instance type when creating the stack.

### Step 2: Create a Stack Using Parameters

When creating a stack with a parameterized template, you can specify parameter values using the `--parameters` option:

```bash
aws cloudformation create-stack --stack-name my-ec2-stack --template-body file://parameterized_template.yaml --parameters ParameterKey=InstanceTypeParameter,ParameterValue=t2.small
```

If you don’t specify a parameter value, the default value (in this case, `t2.micro`) will be used.

## 2. Using Mappings for Conditional Resource Creation

### Step 1: Add Mappings to Your Template

Mappings allow you to create a lookup table in your CloudFormation template. You can use mappings to set values based on the environment, region, or other conditions.

Here’s an example of using mappings to specify different AMI IDs based on the region:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: A CloudFormation template with mappings

Mappings:
  RegionMap:
    us-east-1:
      AMI: ami-0c55b159cbfafe1f0
    us-west-2:
      AMI: ami-0bdf93799014acdc4

Parameters:
  InstanceTypeParameter:
    Type: String
    Default: t2.micro
    Description: EC2 instance type

Resources:
  MyEC2Instance:
    Type: 'AWS::EC2::Instance'
    Properties:
      InstanceType: !Ref InstanceTypeParameter
      ImageId: !FindInMap [RegionMap, !Ref "AWS::Region", AMI]
```

This template uses the `RegionMap` mapping to select the correct AMI ID based on the region where the stack is deployed.

### Step 2: Create a Stack Using Mappings

When you create a stack using this template, CloudFormation automatically selects the correct AMI based on the region:

```bash
aws cloudformation create-stack --stack-name my-ec2-stack --template-body file://mapping_template.yaml
```

## 3. Extracting and Using Outputs

### Step 1: Add Outputs to Your Template

Outputs allow you to extract key information from your stack and use it in other contexts. For example, you can output the instance ID or the DNS name of an EC2 instance.

Here’s an example of adding outputs to your template:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: A CloudFormation template with outputs

Parameters:
  InstanceTypeParameter:
    Type: String
    Default: t2.micro
    Description: EC2 instance type

Resources:
  MyEC2Instance:
    Type: 'AWS::EC2::Instance'
    Properties:
      InstanceType: !Ref InstanceTypeParameter
      ImageId: ami-0123456789abcdef0

Outputs:
  InstanceId:
    Description: The Instance ID
    Value: !Ref MyEC2Instance
  InstancePublicIp:
    Description: The Public IP of the Instance
    Value: !GetAtt MyEC2Instance.PublicIp
```

### Step 2: Retrieve Output Values

After creating the stack, you can retrieve the output values using the following command:

```bash
aws cloudformation describe-stacks --stack-name my-ec2-stack --query 'Stacks[0].Outputs'
```

This command will return the instance ID and public IP address of the EC2 instance created by the stack.

## Conclusion

You’ve now learned how to make your CloudFormation templates more flexible and reusable by using parameters, mappings, and outputs. These features allow you to create templates that can be easily adapted to different environments and conditions. In the next tutorial, you’ll learn how to build a full CI/CD pipeline for AWS using GitHub Actions and CloudFormation.
