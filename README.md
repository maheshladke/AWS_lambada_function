AWS Cloud Cost Optimization - Identifying Stale Resources

This script:
- Lists all snapshots owned by your AWS account.
- Checks if each snapshot has an associated volume.
- Deletes the snapshot if no volume is found.


Step-by-Step Procedure to Configure AWS Lambda
- Create IAM Role
- Go to IAM → Roles → Create Role.
- Choose Lambda as the trusted entity.
- Attach policies:
- AmazonEC2FullAccess (or a custom policy with DescribeSnapshots and DeleteSnapshot).
- Save the role.
- Create Lambda Function

  - Create Lambda Function
- Go to AWS Lambda → Create Function.
- Choose Author from scratch.
- Runtime: Python 3.x.
- Assign the IAM role you created.
- Add Python Code
- In the Lambda console, paste the script above into the code editor.
- Click Deploy.
- Test the Function
- Create a test event (dummy JSON).
- Run the function.
- Check CloudWatch logs to confirm snapshots are deleted.
- Automate with CloudWatch Events (EventBridge)
- Go to EventBridge → Create Rule.
- Schedule expression (e.g., rate(1 day) to run daily).
- Target: your Lambda function.
- This ensures stale snapshots are cleaned up automatically.
- Monitor & Audit
- Use CloudWatch Logs to track deletions.
- Optionally, add SNS notifications for snapshot deletions.


