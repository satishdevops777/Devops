## Cloud automation means:
- Using code (Python) to automatically detect, react, fix, optimize, or provision cloud infrastructure — without manual intervention.
- Two Main models
  
| Type                      | Example                                  |
| ------------------------- | ---------------------------------------- |
| 🔄 Scheduled Automation   | Cleanup every night                      |
| ⚡ Event-Driven Automation | React immediately when something happens |

 ### PROJECT 1: Auto Stop Idle EC2 (Event Driven)
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

### Professional
```py
import boto3
import logging
from datetime import datetime, timedelta, timezone
import config

# Setup logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger()

ec2 = boto3.client("ec2")
cloudwatch = boto3.client("cloudwatch")


def get_avg_cpu(instance_id):
    try:
        response = cloudwatch.get_metric_statistics(
            Namespace="AWS/EC2",
            MetricName="CPUUtilization",
            Dimensions=[{"Name": "InstanceId", "Value": instance_id}],
            StartTime=datetime.now(timezone.utc) - timedelta(days=config.DAYS),
            EndTime=datetime.now(timezone.utc),
            Period=86400,
            Statistics=["Average"]
        )

        datapoints = response["Datapoints"]

        if not datapoints:
            return None

        avg = sum(dp["Average"] for dp in datapoints) / len(datapoints)
        return avg

    except Exception as e:
        logger.error(f"Error fetching metrics for {instance_id}: {e}")
        return None


def stop_instance(instance_id):
    if config.DRY_RUN:
        logger.info(f"[DRY RUN] Would stop {instance_id}")
        return

    try:
        ec2.stop_instances(InstanceIds=[instance_id])
        logger.info(f"Stopped instance {instance_id}")
    except Exception as e:
        logger.error(f"Error stopping {instance_id}: {e}")


def process_instances():
    response = ec2.describe_instances(
        Filters=[
            {"Name": "instance-state-name", "Values": ["running"]},
            {"Name": "tag:Environment", "Values": [config.ENVIRONMENT_FILTER]}
        ]
    )

    for reservation in response["Reservations"]:
        for instance in reservation["Instances"]:
            instance_id = instance["InstanceId"]

            avg_cpu = get_avg_cpu(instance_id)

            if avg_cpu is None:
                continue

            logger.info(f"{instance_id} → Avg CPU: {avg_cpu:.2f}%")

            if avg_cpu < config.CPU_THRESHOLD:
                stop_instance(instance_id)


def lambda_handler(event, context):
    logger.info("Automation started")
    process_instances()
    logger.info("Automation completed")
```
---
### Unused Volumes Cleanup

```py
import boto3
from datetime import datetime, timezone

ec2 = boto3.client("ec2")

volumes = ec2.describe_volumes(
    Filters=[{"Name": "status", "Values": ["available"]}]
)

for vol in volumes["Volumes"]:
    vol_id = vol["VolumeId"]
    create_time = vol["CreateTime"]

    age_days = (datetime.now(timezone.utc) - create_time).days

    if age_days > 7:
        print(f"Deleting unused volume {vol_id}")
        ec2.delete_volume(VolumeId=vol_id)
```
---
### KMS Keys Deletion
```python
import boto3

kms = boto3.client("kms")

keys = kms.list_keys()

for key in keys["Keys"]:
    key_id = key["KeyId"]

    desc = kms.describe_key(KeyId=key_id)
    state = desc["KeyMetadata"]["KeyState"]

    if state == "Enabled":
        print(f"Scheduling deletion for {key_id}")
        kms.schedule_key_deletion(
            KeyId=key_id,
            PendingWindowInDays=7
        )
```
---
### S3 Empty Buckets Deletion
```python
import boto3

s3 = boto3.client("s3")

DRY_RUN = True

buckets = s3.list_buckets()

for bucket in buckets["Buckets"]:
    bucket_name = bucket["Name"]

    try:
        # Check normal objects
        objects = s3.list_objects_v2(Bucket=bucket_name)

        # Check versioned objects
        versions = s3.list_object_versions(Bucket=bucket_name)

        has_objects = "Contents" in objects
        has_versions = "Versions" in versions

        if not has_objects and not has_versions:
            print(f"Empty bucket: {bucket_name}")

            if not DRY_RUN:
                s3.delete_bucket(Bucket=bucket_name)
                print(f"Deleted bucket: {bucket_name}")

    except Exception as e:
        print(f"Error processing {bucket_name}: {e}")
```
---

### Sanpshots older than 15 days
```python
import boto3
from datetime import datetime, timezone, timedelta

ec2 = boto3.client("ec2")

DAYS = 15
DRY_RUN = True

def is_snapshot_used_by_ami(snapshot_id):
    images = ec2.describe_images(Owners=["self"])["Images"]
    for image in images:
        for block in image.get("BlockDeviceMappings", []):
            if "Ebs" in block:
                if block["Ebs"]["SnapshotId"] == snapshot_id:
                    return True
    return False

def delete_old_snapshots():
    snapshots = ec2.describe_snapshots(OwnerIds=["self"])

    for snapshot in snapshots["Snapshots"]:
        snapshot_id = snapshot["SnapshotId"]
        start_time = snapshot["StartTime"]

        age_days = (datetime.now(timezone.utc) - start_time).days

        if age_days > DAYS:
            if is_snapshot_used_by_ami(snapshot_id):
                print(f"Skipping {snapshot_id} (used by AMI)")
                continue

            print(f"Old snapshot found: {snapshot_id} ({age_days} days old)")

            if not DRY_RUN:
                ec2.delete_snapshot(SnapshotId=snapshot_id)
                print(f"Deleted snapshot: {snapshot_id}")

if __name__ == "__main__":
    delete_old_snapshots()


With Lambda

import boto3
from datetime import datetime, timezone

ec2 = boto3.client("ec2")

DAYS = 15
DRY_RUN = True


def is_snapshot_used_by_ami(snapshot_id):
    images = ec2.describe_images(Owners=["self"])["Images"]
    for image in images:
        for block in image.get("BlockDeviceMappings", []):
            if "Ebs" in block:
                if block["Ebs"]["SnapshotId"] == snapshot_id:
                    return True
    return False


def delete_old_snapshots():
    snapshots = ec2.describe_snapshots(OwnerIds=["self"])

    for snapshot in snapshots["Snapshots"]:
        snapshot_id = snapshot["SnapshotId"]
        start_time = snapshot["StartTime"]

        age_days = (datetime.now(timezone.utc) - start_time).days

        if age_days > DAYS:
            if is_snapshot_used_by_ami(snapshot_id):
                print(f"Skipping {snapshot_id} (used by AMI)")
                continue

            print(f"Old snapshot found: {snapshot_id} ({age_days} days old)")

            if not DRY_RUN:
                ec2.delete_snapshot(SnapshotId=snapshot_id)
                print(f"Deleted snapshot: {snapshot_id}")


def lambda_handler(event, context):
    print("Snapshot cleanup started")
    delete_old_snapshots()
    print("Snapshot cleanup finished")
    return {"status": "completed"}
```
