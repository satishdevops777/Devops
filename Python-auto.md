## Cloud automation means:
- Using code (Python) to automatically detect, react, fix, optimize, or provision cloud infrastructure — without manual intervention.
- Two Main models
| Type                      | Example                                  |
| ------------------------- | ---------------------------------------- |
| 🔄 Scheduled Automation   | Cleanup every night                      |
| ⚡ Event-Driven Automation | React immediately when something happens |

 ### PROJECT 1: Auto Stop Idle EC2 (Event Driven)
 ---
 ### 🎯 Goal: If CPU < 5% for 15 days → stop instance automatically.
 - Lambda → Create Function → Python 3.10
 ```
import boto3

ec2 = boto3.client('ec2')

def lambda_handler(event, context):
    instance_id = event['detail']['instance-id']
    
    print(f"Stopping instance {instance_id}")
    
    ec2.stop_instances(InstanceIds=[instance_id])
    
    return {
        'status': 'Instance stopped'
    }
```
