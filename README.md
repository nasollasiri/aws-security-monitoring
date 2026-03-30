<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Build a Security Monitoring System

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-monitoring)

**Author:** siri  
**Email:** nasollasiri@gmail.com

---

![Image](http://learn.nextwork.org/soothed_vermilion_swift_tarragon/uploads/aws-security-monitoring_reghtjy)

---

## Introducing Today's Project!

In this project, I demonstrated how to monitor sensitive activity in AWS by tracking access to secrets and setting up real-time alerts. I used AWS Secrets Manager, CloudTrail, and CloudWatch to build a security monitoring system. First, I created a secret and enabled CloudTrail to log API activity. Then, I set up a CloudWatch metric filter and alarm to detect access to the secret. Finally, I connected it with SNS to receive email alerts and tested the system. This project helped me understand how real-time cloud security monitoring and alerting works.

### Tools and concepts

In this project, I learned several key AWS services and concepts related to security monitoring. I worked with CloudTrail for logging and tracking API activities, CloudWatch for monitoring logs and creating metric filters and alarms, and SNS (Simple Notification Service) for sending real-time alerts. I also used Secrets Manager to store and retrieve sensitive data securely. Additionally, I understood important concepts like read vs write API activities, event tracking, metric filtering, and automated alerting, which together help in detecting and responding to security events effectively.

### Project reflection

it took me 90 minutes to complete this project

---

## Create a Secret

AWS Secrets Manager is a service used to securely store, manage, and retrieve sensitive data like passwords, API keys, and credentials. It helps protect secrets, control access, and automate tasks like password rotation without hardcoding them in applications.

The secret I created in AWS Secrets Manager was a key-value pair containing sensitive information, where the key was The secret is and the value was I need 3 coffees a day to function. This was used to simulate storing confidential data securely and to generate a CloudTrail event (GetSecretValue) when the secret was retrieved.

![Image](http://learn.nextwork.org/soothed_vermilion_swift_tarragon/uploads/aws-security-monitoring_o5p6q7r8)

---

## Set Up CloudTrail

AWS CloudTrail is a service provided by AWS that helps you monitor, record, and audit all activity in your AWS account. It logs actions taken by users, roles, and AWS services—such as API calls, console logins, and resource changes—so you can track who did what, when, and from where. This is especially useful for security analysis, compliance, troubleshooting, and governance, as it gives a complete history of activity across your cloud environment.

A trail in CloudTrail is the configuration that determines how these events are captured and delivered. It specifies which events to log (management events, data events, or insights), and where to store them, typically in an Amazon S3 bucket or send them to CloudWatch Logs for monitoring. Trails can be set up for a single region or across all regions, and they ensure that the recorded activity is continuously collected and available for review or automated analysis.


AWS CloudTrail records three main types of events based on the kind of activity happening in your AWS account. First, Management events capture control-plane operations such as creating, modifying, or deleting AWS resources. These include API activities like launching an EC2 instance, creating an S3 bucket, or retrieving secrets using the `GetSecretValue` API. Second, Data events (also called data-plane events) track operations performed on the actual data within resources, such as reading or writing objects in an S3 bucket or invoking a Lambda function. These are more detailed and usually disabled by default due to high volume. Third, Insights events help detect unusual or abnormal activity patterns, such as sudden spikes in API calls or irregular usage behavior, which may indicate a potential security issue. Overall, CloudTrail monitors all API activities including read operations (like describing resources), write operations (like cents include types like...

### Read vs Write Activity

In AWS CloudTrail, Read API activities only retrieve or view data without making
changes, while Write API activities create, modify, or delete resources and
change the system state.


---

## Verifying CloudTrail

I retrieved the secret in two ways: FirThe two ways to retrieve a secret in AWS Secrets Manager are:

1. Using AWS Management Console:
You manually go to Secrets Manager → select your secret → click “Retrieve secret value”. This is a visual, manual method used for testing and generates a CloudTrail event (GetSecretValue).

2. Using AWS API/CLI:
You retrieve the secret programmatically using API calls like GetSecretValue (via AWS CLI or SDK). This method is used in real applications where code securely fetches secrets automatically.st through... Second using...'

To analyze my CloudTrail events, I visited...When analyzing the CloudTrail events, I discovered that every action performed in AWS is recorded with detailed information such as the event name, time, user identity, and source IP address. Specifically, I observed the GetSecretValue event when retrieving a secret from Secrets Manager, which confirmed that even read operations are logged. The logs clearly showed whether the activity was a read or write operation, along with the exact service involved. This helped me understand how CloudTrail provides complete visibility into API activities, making it useful for auditing, monitoring, and detecting suspicious behavior. I found... This tells me...'

![Image](http://learn.nextwork.org/soothed_vermilion_swift_tarragon/uploads/aws-security-monitoring_s8t9u0v1)

---

## CloudWatch Metrics

Amazon CloudWatch Logs is a service that collects, stores, and monitors log data from your AWS resources, applications, and services in real time. It allows you to centralize logs from sources like EC2 instances, Lambda functions, and other AWS services, making it easier to view and analyze system activity in one place.

CloudWatch Logs is important for monitoring because it helps you track the health and performance of your systems, detect errors or unusual behavior, and troubleshoot issues quickly. You can set up filters, metrics, and alarms to automatically notify you when something goes wrong, such as spikes in errors or system failures. This proactive monitoring improves reliability, enhances security by identifying suspicious activity, and supports faster debugging and operational visibility across your infrastructure.


The key difference between AWS CloudTrail and Amazon CloudWatch Logs lies in what they track and how they are used. CloudTrail is focused on recording API activity and user actions in your AWS account—basically answering “who did what, when, and from where.” It is mainly used for auditing, security, and compliance, as it keeps a history of all management operations performed on AWS resources.

On the other hand, CloudWatch Logs is designed to collect and monitor application and system logs such as error messages, performance data, and runtime events. It helps you analyze, troubleshoot, and set up real-time monitoring and alerts for your applications and infrastructure. In short, CloudTrail is about account activity tracking, while CloudWatch Logs is about operational monitoring and debugging.

In Amazon CloudWatch, Metric value and Default value are used when creating filters (like metric filters on logs) to control how data is converted into measurable metrics.

The metric value is the actual number that gets assigned to a metric when a specific pattern or condition is matched in your logs. For example, if a log entry contains the word “ERROR,” you might assign a metric value of 1, so every occurrence increases the error count. This helps you quantify events and track trends over time.

The default value, on the other hand, is used when no matching log event is found during a given time period. Instead of leaving a gap (no data), CloudWatch can assign this default value (often 0) to ensure the metric remains continuous. This is important for accurate monitoring and alarms, as it prevents missing data from being misinterpreted as a lack of activity.

![Image](http://learn.nextwork.org/soothed_vermilion_swift_tarragon/uploads/aws-security-monitoring_a9b0c1d2)

---

## CloudWatch Alarm

The threshold setting in my CloudWatch alarm defines when the alarm should trigger based on the metric value. In this project, I set the threshold to greater than or equal to 1, which means the alarm is triggered whenever at least one matching event (such as GetSecretValue) occurs. This ensures that even a single sensitive action is immediately detected and an alert is sent through SNS.

An SNS (Simple Notification Service) topic is a communication channel in AWS used to send messages or alerts to multiple subscribers at once. It acts like a central hub where messages are published, and all subscribed endpoints—such as email, SMS, or applications—receive the notification automatically. SNS topics are commonly used for real-time alerts, such as sending security notifications from CloudWatch or GuardDuty. In short, an SNS topic enables one-to-many communication for instant message delivery.

AWS requires email confirmation for SNS subscriptions to ensure security and user consent. This process verifies that the email address provided is valid and that the owner agrees to receive notifications. Without confirmation, anyone could subscribe another person’s email and send unwanted or spam messages. By confirming the subscription, AWS prevents misuse, ensures reliable message delivery, and protects users from unauthorized notifications.

![Image](http://learn.nextwork.org/soothed_vermilion_swift_tarragon/uploads/aws-security-monitoring_fsdghstt)

---

## Troubleshooting Notification Errors

During the test of my monitoring system, I triggered an event by retrieving the secret from AWS Secrets Manager. This action was successfully recorded in CloudTrail, matched by the CloudWatch metric filter, and then triggered the CloudWatch alarm. As a result, an SNS notification was sent, and I received an alert email in my inbox. This confirmed that the entire monitoring pipeline—from event logging to alert notification—was working correctly.

To troubleshoot the error, I followed several steps. First, I checked the CloudFormation Events tab to identify the exact failure reason. Second, I verified that the correct AWS region (such as us-east-1) was selected. Third, I ensured that GuardDuty was enabled in the account before creating resources. Fourth, I reviewed and corrected the CloudFormation template (YAML) for any configuration issues. Finally, I confirmed that my AWS account setup and permissions were complete, as incomplete activation can prevent services from working properly.

👉

I did not receive an email earlier because the SNS subscription was not confirmed. In AWS SNS, email notifications are only sent after the recipient confirms the subscription through the verification link sent to their inbox. Until this confirmation is completed, messages are not delivered, which is why I did not receive any alerts initially.


---

## Success!

I validated that my monitoring system works by performing a test action, such as retrieving the secret from AWS Secrets Manager. This triggered a GetSecretValue event, which was recorded in CloudTrail, matched by the CloudWatch metric filter, and activated the CloudWatch alarm. As a result, an SNS notification was sent, and I received an email alert, confirming that the entire monitoring system was functioning correctly.

![Image](http://learn.nextwork.org/soothed_vermilion_swift_tarragon/uploads/aws-security-monitoring_ageraergearge)

---

## Comparing CloudWatch with CloudTrail Notifications

I updated my CloudTrail configurations to improve security monitoring and visibility of AWS activities. Specifically, I enabled both read and write events and integrated CloudTrail with CloudWatch Logs so that I could track sensitive actions like GetSecretValue in real time. This allowed me to create metric filters and alarms, ensuring that any critical activity triggers an immediate alert through SNS.

After enabling direct CloudTrail SNS notifications, I received an email in my inbox confirming the subscription to the SNS topic. Once confirmed, I started receiving alert emails whenever the specified CloudTrail event (such as GetSecretValue) was triggered. These emails contained details about the activity, helping me monitor actions in real time.

![Image](http://learn.nextwork.org/soothed_vermilion_swift_tarragon/uploads/aws-security-monitoring_d7e8f9g0)

---

---
