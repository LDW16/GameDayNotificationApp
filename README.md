# NBA Game Day Notifications

## Project Guidelines
- [Project Guidelines by REXTECH](https://www.youtube.com/watch?v=09WfkKc0x_Q)

## Project Overview
This project is a sports alert system that sends real-time NBA game day score notifications to subscribed users via SMS or Email. It leverages Amazon Web Services (SNS, Lambda, EventBridge) and external NBA APIs to provide sports fans with timely game information. The project demonstrates efficient notification mechanisms and cloud computing best practices.

## Features
- Fetches live NBA game scores using an external API.
- Sends formatted score updates to subscribers via SMS/Email using Amazon SNS.
- Automated scheduling for periodic updates using Amazon EventBridge.
- Designed with security in mind by following the principle of least privilege for IAM roles.

## Prerequisites
- Free account and API key at [SportsData.io](https://sportsdata.io).
- Personal AWS account with basic understanding of AWS services and Python.

## Technologies
- **Cloud Provider:** AWS
- **Core Services:** SNS, Lambda, EventBridge
- **External API:** NBA Game API (SportsData.io)
- **Programming Language:** Python 3.x
- **Security:** Least privilege IAM roles for Lambda, SNS, and EventBridge.

## Project Structure
```
game-day-notifications/
├── src/
│   ├── gd_notifications.py          # Main Lambda function code
├── policies/
│   ├── gd_sns_policy.json           # SNS publishing permissions
│   ├── gd_eventbridge_policy.json   # EventBridge to Lambda permissions
│   └── gd_lambda_policy.json        # Lambda execution role permissions
├── .gitignore
└── README.md                        # Project documentation
```

## Setup Instructions

### 1. Clone the Repository (optional)
```bash
git clone https://github.com/ifeanyiro9/game-day-notifications.git
cd game-day-notifications
```
   ###Note: You may choose to create the information yourself.

### 2. Create an SNS Topic
1. Open the AWS Management Console and navigate to the SNS service.
2. Click **Create Topic** and select **Standard** as the topic type.
3. Name the topic (e.g., `gd_topic`) and note the ARN.
4. Click **Create Topic**.

### 3. Add Subscriptions to the SNS Topic
1. Click the topic name from the list.
2. Navigate to the **Subscriptions** tab and click **Create subscription**.
3. Select a protocol:
   - **Email:** Enter a valid email address.
   - **SMS:** Enter a valid phone number in international format (e.g., `+1234567890`).
4. Click **Create Subscription**.
5. Confirm email subscriptions by clicking the confirmation link sent to your inbox.

### 4. Create the SNS Publish Policy
1. Open the **IAM** service in the AWS Management Console.
2. Navigate to **Policies** → **Create Policy**.
3. Click **JSON** and paste the JSON policy from `gd_sns_policy.json`.
4. Replace `REGION` and `ACCOUNT_ID` with your AWS region and account ID.
5. Click **Next: Review** and enter a name for the policy (e.g., `gd_sns_policy`).
6. Click **Create Policy**.

### 5. Create an IAM Role for Lambda
1. Open the **IAM** service in the AWS Management Console.
2. Click **Roles** → **Create Role**.
3. Select **AWS Service** and choose **Lambda**.
4. Attach the following policies:
   - SNS Publish Policy (`gd_sns_policy` created earlier).
   - Lambda Basic Execution Role (`AWSLambdaBasicExecutionRole`).
5. Enter a name for the role (e.g., `gd_role`).
6. Click **Create Role** and copy the ARN.

### 6. Deploy the Lambda Function
1. Navigate to the **Lambda** service in AWS.
2. Click **Create Function** and select **Author from Scratch**.
3. Enter a function name (e.g., `gd_notifications`).
4. Choose **Python 3.x** as the runtime.
5. Assign the IAM role created earlier.
6. Under **Function Code**, copy and paste the content from `src/gd_notifications.py`.
7. Add the following environment variables:
   - `NBA_API_KEY`: Your NBA API key.
   - `SNS_TOPIC_ARN`: The ARN of the SNS topic.
8. Click **Create Function**.

### 7. Set Up Automation with EventBridge
1. Navigate to the **EventBridge** service.
2. Go to **Rules** → **Create Rule**.
3. Select **Event Source: Schedule** and set the cron schedule expression:
   ```cron
   0 9-23/4 * * ? *
   ```
   This schedule runs every 4 hours from 9 AM to 12 AM (9 AM, 1 PM, 5 PM, 9 PM).
4. Under **Targets**, select the Lambda function and save the rule.

## Sample Data Output
![Sample Notification Output](notificationapp_sampleoutput.png)

## Common Issues

### 1. Function Timeout Error
#### Resolution
1. Navigate to the Lambda function in the AWS Lambda console.
2. Select the **Configuration** tab.
3. In the **General configuration** section, click **Edit**.
4. Increase the **Timeout** value to at least 10 seconds or more, depending on the expected execution time.
5. Click **Save**.

### 2. EventBridge Scheduling
#### Cron Job Expression Explanation
- **Expression:** `0 9-23/4 * * ? *`
  - `0`: The minute at which the job runs.
  - `9-23/4`: Every 4 hours from 9 AM to 11 PM.
  - `*`: Any day of the month.
  - `*`: Any month.
  - `?`: Any day of the week.
  - `*`: Any year.

Schedule Details:
- The job runs at 9 AM, 1 PM, 5 PM, and 9 PM daily.

## What We Learned
- Designing a notification system with AWS SNS and Lambda.
- Securing AWS services using least privilege IAM policies.
- Automating workflows with EventBridge.
- Integrating external APIs into cloud-based workflows.

