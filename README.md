# Automated RDS Backup Solution with AWS Lambda, CloudWatch, and SNS

This project automates backups for an Amazon RDS instance using AWS Lambda, triggered by scheduled CloudWatch Events. It also includes an SNS notification setup to inform the user of backup statuses and an automatic retention policy to delete outdated snapshots.

## Features

- **Automated Backups**: Scheduled RDS backups through AWS Lambda and CloudWatch Events.
- **Retention Policy**: Automatic deletion of snapshots older than a specified number of days to save storage costs.
- **Real-Time Notifications**: Immediate notification of backup status (success or failure) via Amazon SNS.
- **Cost-Optimized Solution**: Efficient use of AWS resources to manage and store only necessary backups.

## Tools and Technologies

- **AWS Lambda**: Executes the backup and retention policy functions.
- **Amazon RDS**: The primary database instance being backed up.
- **Amazon CloudWatch Events**: Triggers the Lambda function based on the backup schedule.
- **Amazon SNS**: Sends notifications about backup success or failure.
- **Python**: Scripting language used in the Lambda function.

## Steps to Deploy

### Step 1: Setup AWS Resources

1. **Create an RDS Instance**: Set up the Amazon RDS instance you want to back up, if you haven't already.
2. **Set Up an SNS Topic**: In the SNS Console, create a new topic and subscribe your email or phone number to receive notifications.

### Step 2: Configure the Lambda Function

1. **Create a Lambda Function**:
   - In the Lambda Console, create a new function using Python.
   - Paste the Python code that initiates the backup, implements the retention policy, and sends SNS notifications.

2. **Set Up Environment Variables**:
   - In the Lambda function’s configuration, add the following environment variables:
     - `RESOURCE_ID`: The RDS instance identifier.
     - `SNS_TOPIC_ARN`: The ARN of the SNS topic.
     - `RETENTION_DAYS`: The number of days to retain backups (e.g., `7`).

3. **Assign Necessary IAM Role**:
   - Attach policies to the Lambda execution role to allow it to access RDS, SNS, and CloudWatch Logs.

### Step 3: Schedule the Backup with CloudWatch

1. **Create a CloudWatch Rule**:
   - In the CloudWatch Console, create a new rule and set a schedule expression for the desired backup frequency (e.g., daily).
   - Set the target to trigger the Lambda function created in Step 2.

### Step 4: Test the Solution

1. **Initiate a Manual Test**: Trigger the Lambda function manually to ensure the backup process works correctly and that notifications are sent.
2. **Verify Backup Creation**: Check the RDS Console to confirm the new snapshot is created.
3. **Check for Retention Policy**: Verify that older snapshots beyond the specified retention period are deleted.

## Result

After deploying this solution, backups for your RDS instance are automatically created according to the schedule defined in CloudWatch Events. Each time a backup is created, a notification is sent to the configured SNS topic, alerting you of the status. Snapshots older than the defined retention period are automatically deleted, keeping storage usage efficient.

## Conclusion

This solution simplifies RDS backup management by automating the backup and cleanup process, enhancing data protection, and reducing manual intervention. With features like real-time notifications and automated retention, this project demonstrates effective use of AWS services to create a reliable, scalable backup solution. 
This project is an ideal fit for cloud environments where automated data protection and cost optimization are critical requirements.
