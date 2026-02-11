import boto3

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')

    # Get all snapshots owned by your account
    snapshots = ec2.describe_snapshots(OwnerIds=['self'])['Snapshots']

    # Get all existing volumes
    volumes = ec2.describe_volumes()['Volumes']
    existing_volume_ids = {v['VolumeId'] for v in volumes}

    for snapshot in snapshots:
        snapshot_id = snapshot['SnapshotId']
        volume_id = snapshot.get('VolumeId')

        # If snapshot's volume does not exist anymore, delete it
        if volume_id not in existing_volume_ids:
            try:
                ec2.delete_snapshot(SnapshotId=snapshot_id)
                print(f"Deleted snapshot {snapshot_id} (volume {volume_id} not found).")
            except Exception as e:
                print(f"Error deleting snapshot {snapshot_id}: {str(e)}")
        else:
            print(f"Snapshot {snapshot_id} is linked to active volume {volume_id}, skipping.")
