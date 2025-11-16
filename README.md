# EC2 Auto Start & Stop using AWS Lambda

This repository contains two AWS Lambda functions that automatically **start** and **stop** EC2 instances based on specific tags.  
This helps reduce AWS costs by ensuring development or test environment instances run only when required.

---

## 🚀 Features

### ✅ **Auto-Start Lambda**
Starts all EC2 instances that have the following tags:
- `Environment = Development`
- `AutoStart = true`
- Instance state: **stopped**

### ✅ **Auto-Stop Lambda**
Stops all EC2 instances that have the following tags:
- `Environment = Development`
- `AutoStop = true`
- Instance state: **running**

---

## 📌 How It Works

1. AWS Lambda is triggered by a **CloudWatch Event Rule (Cron)**.
2. Lambda scans all EC2 instances using `describe_instances()`.
3. Instances with specific tag values are filtered.
4. Selected instances are automatically started or stopped.

---

## 🧩 Tag Requirements

| Tag Key       | Value      | Used For     |
|---------------|------------|--------------|
| `Environment` | Development | Both Start & Stop |
| `AutoStart`   | true        | Auto-Start Lambda |
| `AutoStop`    | true        | Auto-Stop Lambda |

Only instances with correct tag combinations will be started or stopped.

---

## 🔧 Auto-Start Lambda Code

```python
import boto3

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')
    
    response = ec2.describe_instances(
        Filters=[
            {'Name': 'tag:Environment', 'Values': ['Development']},
            {'Name': 'tag:AutoStart', 'Values': ['true']},
            {'Name': 'instance-state-name', 'Values': ['stopped']}
        ]
    )
    
    instances_to_start = []

    for reservation in response['Reservations']:
        for instance in reservation['Instances']:
            instances_to_start.append(instance['InstanceId'])
            
    print(instances_to_start)
    
    if instances_to_start:
        ec2.start_instances(InstanceIds=instances_to_start)
        print(f"Started instances: {instances_to_start}")
    else:
        print("No instances to start")
