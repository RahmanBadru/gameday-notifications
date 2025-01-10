# NBA GAMEDAY NOTIFICATION SYSTEM

This project implements a event driven notification system by using AWS Services like Amazon Event Bridge to trigger a lambda function to pull live NBA game data using Sports DATA IO API and send to your email via Amazon SNS (Simple Notification System)


## PROJECT SETUP

### Create AWS SNS TOPIC

1. Open the AWS Management Console
2. GO to SNS
3. Click on topics, click create topic
4. Select Standard as topic type
5. Name your topic (e.g gameday-topic) and click create topic

### Create a Subscription for the Topic
1. Click on your topic
2. Click create subscription
3. Set Protocol to email
4. Put your mail
5. Click create Subscription
6. Confirm the subscription from your mail

### Create an SNS Publish Policy
1. Open the IAM service in the AWS Management Console.
2. Navigate to Policies → Create Policy.
3. Click JSON and paste the JSON policy from gd_sns_policy.json file
4. Replace REGION and ACCOUNT_ID with your AWS region and account ID.
5. Click Next: Tags (you can skip adding tags).
6. Click Next: Review.
7. Enter a name for the policy (e.g., gameday-policy).
8. Review and click Create Policy.

### Create IAM Role for Lambda
1. Open the IAM service in the AWS Management Console.
2. Navigate to Roles -> Create Role
3. Click on use case drop down and click Lambda
4. Click Next and then attach two policies:
 - The policy create earlier (gameday-policy)
 - The Lambda Basic Execution Role (AWSLambdaBasicExecutionRole) (an AWS managed policy).
5. CLick Next, skip tags and create the role

### Create Lambda Function
1. Open the AWS Management Console and navigate to the Lambda service.
2. Click Create Function.
3. Select Author from Scratch.
4. Enter a function name (e.g., gameday-function).
5. Choose Python 3.x as the runtime.
6. Assign the IAM role created earlier (gameday-role) to the function.
7. Under the Function Code section:
 - Copy the content of the src/gd_notifications.py file from the repository.
 - Paste it into the inline code editor.
8. Under the Environment Variables section, add the following:
 - NBA_API_KEY: your NBA API key.
 - SNS_TOPIC_ARN: the ARN of the SNS topic created earlier.
9. Click Create Function

### Set up EventBridge Schedule to trigger Lambda
1. Navigate to the Eventbridge service in the AWS Management Console.
2. Go to Rules → Create Rule.
3. Select Event Source: Schedule.
4. Set the cron schedule for when you want updates (e.g., hourly).
5. Under Targets, select the Lambda function (gameday-function) and save the rule.

### Test the System
1. Open Lambda function and click on the function you created
2. click on test and name the event
3. Test the function
4. Verify you get a mail confirming it was successful