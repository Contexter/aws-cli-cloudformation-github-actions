### **Tutorial 10: Monitoring and Troubleshooting AWS with AWS CLI and GitHub Actions**

In this tutorial, you will learn how to monitor and troubleshoot your AWS resources using the AWS CLI and GitHub Actions.

---

# Monitoring and Troubleshooting AWS with AWS CLI and GitHub Actions

## Overview

Monitoring your AWS infrastructure and applications is crucial for maintaining performance, reliability, and security. AWS provides various tools and services, such as CloudWatch, to monitor resources. You can integrate these tools with the AWS CLI and GitHub Actions to automate monitoring and troubleshooting tasks.

In this tutorial, you will:

1. **Use AWS CloudWatch with AWS CLI**: Set up CloudWatch alarms and monitor logs using the AWS CLI.
2. **Automate Monitoring with GitHub Actions**: Create a GitHub Actions workflow to automate the monitoring of your AWS resources.
3. **Troubleshoot Common Issues**: Learn how to troubleshoot common AWS issues using AWS CLI and CloudWatch logs.

## 1. Using AWS CloudWatch with AWS CLI

### Step 1: Create a CloudWatch Alarm

CloudWatch Alarms allow you to monitor specific metrics and trigger actions when thresholds are breached.

To create a CloudWatch Alarm that monitors CPU utilization on an EC2 instance:

```bash
aws cloudwatch put-metric-alarm \
    --alarm-name HighCPUUtilization \
    --metric-name CPUUtilization \
    --namespace AWS/EC2 \
    --statistic Average \
    --period 300 \
    --threshold 80 \
    --comparison-operator GreaterThanOrEqualToThreshold \
    --dimensions Name=InstanceId,Value=i-0123456789abcdef0 \
    --evaluation-periods 2 \
    --alarm-actions arn:aws:sns:us-west-2:123456789012:MySNSTopic \
    --alarm-description "Alarm when CPU exceeds 80%" \
    --unit Percent
```

This command creates an alarm that triggers when the average CPU utilization exceeds 80% for two consecutive 5-minute periods.

### Step 2: Monitor CloudWatch Logs

You can monitor CloudWatch Logs directly from the CLI. For example, to view the logs of a specific log group:

```bash
aws logs get-log-events \
    --log-group-name /aws/lambda/my-function \
    --log-stream-name 2023/01/01/[$LATEST]abcdefgh1234567890abcdef
```

This command retrieves the log events for a specific Lambda function.

### Step 3: Set Up CloudWatch Dashboards

CloudWatch Dashboards allow you to visualize and monitor your AWS resources in real-time. To create a basic dashboard:

```bash
aws cloudwatch put-dashboard \
    --dashboard-name MyDashboard \
    --dashboard-body '{
        "widgets": [
            {
                "type": "metric",
                "x": 0,
                "y": 0,
                "width": 6,
                "height": 6,
                "properties": {
                    "metrics": [
                        [ "AWS/EC2", "CPUUtilization", "InstanceId", "i-0123456789abcdef0" ]
                    ],
                    "period": 300,
                    "stat": "Average",
                    "region": "us-west-2",
                    "title": "EC2 Instance CPU"
                }
            }
        ]
    }'
```

This command creates a dashboard that shows the average CPU utilization for an EC2 instance.

## 2. Automating Monitoring with GitHub Actions

### Step 1: Create a Monitoring Workflow

You can automate the monitoring of AWS resources using GitHub Actions by creating a workflow that checks CloudWatch metrics and logs. Create a workflow file named `monitor.yml` in the `.github/workflows` directory:

```yaml
name: Monitor AWS Resources

on:
  schedule:
    - cron: '0 * * * *' # Runs every hour

jobs:
  monitor:
    runs-on: ubuntu-latest

    steps:
    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-west-2

    - name: Check CloudWatch Alarm State
      run: |
        aws cloudwatch describe-alarms \
          --alarm-names "HighCPUUtilization" \
          --query "MetricAlarms[0].StateValue"

    - name: Fetch CloudWatch Logs
      run: |
        aws logs get-log-events \
          --log-group-name /aws/lambda/my-function \
          --log-stream-name 2023/01/01/[$LATEST]abcdefgh1234567890abcdef
```

This workflow runs every hour and checks the state of a CloudWatch alarm and fetches logs from a CloudWatch log group.

### Step 2: Add Notifications

You can integrate notifications into your workflow by using Amazon SNS or other services. For example, to send a notification if the alarm state is `ALARM`:

```yaml
- name: Check and Notify if Alarm is Triggered
  run: |
    STATE=$(aws cloudwatch describe-alarms --alarm-names "HighCPUUtilization" --query "MetricAlarms[0].StateValue" --output text)
    if [ "$STATE" == "ALARM" ]; then
      aws sns publish --topic-arn arn:aws:sns:us-west-2:123456789012:MySNSTopic --message "High CPU utilization detected!"
    fi
```

This step checks the alarm state and sends an SNS notification if the alarm is triggered.

## 3. Troubleshooting Common Issues

### Step 1: Investigate Failed Deployments

If a deployment fails, you can use CloudWatch Logs and the AWS CLI to investigate:

```bash
aws cloudformation describe-stack-events --stack-name my-stack
```

This command lists all events related to a CloudFormation stack, helping you identify the point of failure.

### Step 2: Analyze Performance Issues

Use CloudWatch metrics to analyze performance issues, such as high CPU usage or memory leaks:

```bash
aws cloudwatch get-metric-statistics \
    --namespace AWS/EC2 \
    --metric-name CPUUtilization \
    --start-time 2023-01-01T00:00:00Z \
    --end-time 2023-01-01T23:59:59Z \
    --period 300 \
    --statistics Average \
    --dimensions Name=InstanceId,Value=i-0123456789abcdef0
```

This command retrieves the average CPU utilization for an EC2 instance over a specific time period.

### Step 3: Use AWS CLI to Fix Issues

If you identify an issue, such as an instance with high CPU usage, you can take action using the AWS CLI. For example, you can stop the instance:

```bash
aws ec2 stop-instances --instance-ids i-0123456789abcdef0
```

## Conclusion

You’ve now learned how to monitor and troubleshoot your AWS resources using AWS CloudWatch, AWS CLI, and GitHub Actions. By integrating monitoring into your CI/CD pipeline, you can proactively manage the health and performance of your infrastructure. In the next tutorial, you’ll explore advanced CloudFormation features to further enhance your infrastructure automation.
