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

### Find the Unused EC2 instances
```py
import boto3
from datetime import datetime, timedelta

ec2 = boto3.client('ec2')
cloudwatch = boto3.client('cloudwatch')

instances = ec2.describe_instances()

for reservation in instances['Reservations']:
    for instance in reservation['Instances']:
        instance_id = instance['InstanceId']

        response = cloudwatch.get_metric_statistics(
            Namespace='AWS/EC2',
            MetricName='CPUUtilization',
            Dimensions=[{'Name': 'InstanceId', 'Value': instance_id}],
            StartTime=datetime.utcnow() - timedelta(days=14),
            EndTime=datetime.utcnow(),
            Period=1209600,
            Statistics=['Average']
        )

        datapoints = response['Datapoints']
        if datapoints:
            avg_cpu = datapoints[0]['Average']
            if avg_cpu < 5:
                print(f"{instance_id} is idle (CPU < 5%)")
```



### Remove Unused EBS Volumes (Tag Based)
- Criteria:
- status = available (not attached)
- Tag Environment = dev
- Optional: Older than X days

```py
import boto3
from datetime import datetime, timezone

ec2 = boto3.client("ec2")

ENVIRONMENT_FILTER = "dev"
DRY_RUN = True
AGE_DAYS = 7

def volume_is_old(volume):
    create_time = volume["CreateTime"]
    age = datetime.now(timezone.utc) - create_time
    return age.days >= AGE_DAYS

volumes = ec2.describe_volumes(
    Filters=[
        {"Name": "status", "Values": ["available"]},
        {"Name": "tag:Environment", "Values": [ENVIRONMENT_FILTER]},
    ]
)

for vol in volumes["Volumes"]:
    vol_id = vol["VolumeId"]

    if volume_is_old(vol):
        print(f"Candidate EBS: {vol_id}")

        if not DRY_RUN:
            ec2.delete_volume(VolumeId=vol_id)
            print(f"Deleted: {vol_id}")
```


### KMS Keys 
```
import boto3
from datetime import datetime, timedelta

kms = boto3.client("kms")

ENVIRONMENT_FILTER = "dev"
DRY_RUN = True

keys = kms.list_keys()

for key in keys["Keys"]:
    key_id = key["KeyId"]

    tags = kms.list_resource_tags(KeyId=key_id)["Tags"]

    tag_dict = {t["TagKey"]: t["TagValue"] for t in tags}

    if tag_dict.get("Environment") == ENVIRONMENT_FILTER:
        key_desc = kms.describe_key(KeyId=key_id)
        state = key_desc["KeyMetadata"]["KeyState"]

        if state == "Enabled":
            print(f"Scheduling deletion for KMS key: {key_id}")

            if not DRY_RUN:
                kms.schedule_key_deletion(
                    KeyId=key_id,
                    PendingWindowInDays=7
                )
```

### Empty S3 buckets
```
import boto3

s3 = boto3.client("s3")

ENVIRONMENT_FILTER = "dev"
DRY_RUN = True

buckets = s3.list_buckets()

for bucket in buckets["Buckets"]:
    bucket_name = bucket["Name"]

    try:
        tags = s3.get_bucket_tagging(Bucket=bucket_name)["TagSet"]
        tag_dict = {t["Key"]: t["Value"] for t in tags}

        if tag_dict.get("Environment") == ENVIRONMENT_FILTER:
            objects = s3.list_objects_v2(Bucket=bucket_name)

            if "Contents" not in objects:
                print(f"Empty bucket candidate: {bucket_name}")

                if not DRY_RUN:
                    s3.delete_bucket(Bucket=bucket_name)
                    print(f"Deleted bucket: {bucket_name}")

    except Exception:
        continue
```


Working
```python
import boto3
from datetime import datetime, timedelta, timezone

ec2 = boto3.client("ec2")
cloudwatch = boto3.client("cloudwatch")

CPU_THRESHOLD = 5
DAYS = 7

def get_avg_cpu(instance_id):
    response = cloudwatch.get_metric_statistics(
        Namespace="AWS/EC2",
        MetricName="CPUUtilization",
        Dimensions=[{"Name": "InstanceId", "Value": instance_id}],
        StartTime=datetime.now(timezone.utc) - timedelta(days=DAYS),
        EndTime=datetime.now(timezone.utc),
        Period=86400,
        Statistics=["Average"]
    )

    datapoints = response["Datapoints"]

    if not datapoints:
        return None

    avg_cpu = sum(dp["Average"] for dp in datapoints) / len(datapoints)
    return avg_cpu


def lambda_handler(event, context):
    print("Lambda started")

    instances = ec2.describe_instances(
        Filters=[
            {"Name": "instance-state-name", "Values": ["running"]}
        ]
    )

    for reservation in instances["Reservations"]:
        for instance in reservation["Instances"]:
            instance_id = instance["InstanceId"]

            avg_cpu = get_avg_cpu(instance_id)

            if avg_cpu is None:
                continue

            print(f"{instance_id} → Avg CPU: {avg_cpu:.2f}%")

            if avg_cpu < CPU_THRESHOLD:
                print(f"Stopping {instance_id}")
                ec2.stop_instances(InstanceIds=[instance_id])

    print("Lambda finished")
```
### Reusable Template
```py
def get_resources():
    pass

def evaluate(resource):
    pass

def remediate(resource):
    pass

def main():
    resources = get_resources()
    for r in resources:
        if evaluate(r):
            remediate(r)
```
