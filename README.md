# Automated RDS Backup and Retention Management

## Introduction

This project automates the backup process for an AWS RDS instance, creates snapshots, and implements a retention policy to manage the deletion of old snapshots. It also integrates AWS Lambda, SNS (Simple Notification Service), and EventBridge to schedule the backup process and notify users of successful backups and deletions.

## Features

- **Automated RDS Snapshot Creation**: Automatically creates a snapshot of an RDS instance at regular intervals.
- **Snapshot Retention Policy**: Retains snapshots for a configurable number of days (7 days in this case), deleting older snapshots.
- **SNS Notifications**: Sends notifications about the backup process and snapshot deletions.
- **EventBridge Scheduling**: Triggers Lambda function execution on a fixed schedule (every 5 minutes).

## Tools and Technologies

- **AWS RDS (Relational Database Service)**: Used to host the MySQL database.
- **AWS Lambda**: Executes the Python function to create snapshots and handle retention.
- **AWS SNS (Simple Notification Service)**: Sends notifications to a specified email address.
- **AWS EventBridge**: Triggers the Lambda function at regular intervals.
- **Python 3.11**: The runtime for the Lambda function, where the logic is implemented.

## Steps to Deploy

### 1) Setting Up the RDS Instance

To kick things off, we need an AWS RDS instance to serve as the database. We'll start by creating an RDS instance running MySQL:

- **Choose Your Engine**: MySQL is selected as the database engine for our instance.
- **Set Up the Instance**: During setup, give your instance a unique name like `database-1`, and configure the instance type to `db.t3.micro`. 
- **Credentials**: Set the master username to `admin`, and choose a secure password (e.g., `17Saythename` in this case).
- **Security Group**: The default security group is sufficient for now.
  
Once the RDS instance is ready, you’ll have a fully functional MySQL database, accessible for all your backup operations

### 2) Creating the SNS Topic for Notifications

Next, we set up a notification system to keep track of our backups. AWS SNS (Simple Notification Service) will help us send email notifications about the status of our backups.

- **Create the SNS Topic**: Head to SNS in the AWS Management Console and create a topic called `RDS_Backup`. Choose a standard topic type.
- **Set Up the Subscription**: Now, you’ll need to subscribe to this topic. The protocol is set to "Email," and the endpoint will be your email address. For example, `gopikagop09@gmail.com`. After this, you’ll get an email asking you to confirm the subscription. Click the confirmation link, and you’re all set!

Now, you’ll be able to receive real-time notifications whenever a backup occurs, or if an old snapshot is deleted according to the retention policy.

### 3) Creating the Lambda Function to Automate Backups

Now comes the heart of the automation—the Lambda function that will manage the backups.

- **Create the Lambda Function**: Open the AWS Lambda service, and create a new function named `myfunction`. Set the runtime to Python 3.11.
- **Add Your Code**: Inside the function editor, you’ll paste the Python code that handles the snapshot creation and retention logic. This code will automatically trigger the RDS snapshot creation and delete old snapshots based on the retention policy (7 days in this case).
- **SNS and RDS Configuration**: Be sure to update the Lambda function with the correct ARN for your SNS topic and the RDS instance identifier (`database-1`) so that the notifications and backups link to the right resources.

With the Lambda function in place, you’re ready to automate the entire backup process.

### 4) Scheduling with EventBridge

The last step is to set up an automatic schedule for Lambda to run regularly. AWS EventBridge is the tool that makes this happen.

- **Create a Rule**: In the EventBridge console, create a new rule called `my_rule`. Set the rule type to "Schedule" and choose a rate-based schedule to trigger every 5 minutes. This will ensure that every 5 minutes, a snapshot is created for your RDS instance.
- **Set the Target**: The target for this rule is your Lambda function (`myfunction`). With this in place, EventBridge will invoke your Lambda function every 5 minutes, ensuring that your RDS instance is backed up regularly.

### The Result: Continuous Backups with Notifications

With everything set up, here’s how it all works together:

- Every 5 minutes, EventBridge triggers your Lambda function to create a snapshot of your RDS instance.
- The Lambda function checks the retention policy (keeping snapshots for 7 days) and deletes old snapshots automatically.
- Once a snapshot is created, or an old one is deleted, SNS sends a notification to your email to keep you informed about the process.

## Conclusion

This solution not only automates your RDS instance backups but also ensures that you stay up-to-date on the status of those backups. The combination of RDS, Lambda, SNS, and EventBridge makes it a robust and easy-to-manage backup system. You can easily adjust the retention period, schedule, and configuration to suit your needs.
