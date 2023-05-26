
# Project-Secure EC2 Access for_Humanitarian Operations

## Description

Your company has a web application that is hosted on AWS using EC2 instances behind a load balancer. 

The company has asked you to improve the application's security by implementing secure access to the EC2 instances.


AWS DevOps Assessment  / Secure EC2 Access

Challenge: 


11. Implement secure access to the EC2 instances: 
a. Use AWS Systems Manager Session Manager to enable secure remote access to the EC2 instances without opening inbound ports in the security group. 
b. Configure IAM policies to ensure that only authorized users have access to the EC2 instances using Systems Manager Session Manager.
c. Test the secure access to the EC2 instances to ensure that it's working as expected.

12. Ensure security and compliance:
 a. Implement AWS Identity and Access Management (IAM) to ensure that only authorized users have access to the application and its resources.
 b. Implement AWS Config to audit and monitor any changes to the environment and ensure compliance with industry regulations. 
 c. Implement AWS Shield to protect the application against DDoS attacks and other security threats.

13. Improve monitoring and visibility:
 a. Use Amazon CloudWatch to monitor the EC2 instances' CPU, memory, and disk usage to identify any performance issues.
 b. Use CloudWatch to create alarms to notify you of any unusual behavior or potential issues. 
 c. Use AWS CloudTrail to log and monitor any API activity and changes to the environment.



Deliverables:

14. A detailed report of the implementation of secure access to the EC2 instances, including a description of the IAM policies and test results.

15. Documentation of the security and compliance measures implemented.

16. A monitoring plan outlining the monitoring and visibility improvements made the environment.



Solutions : 

Here's a guide to help us improve the security of our web application hosted on AWS using EC2 instances behind a load balancer, focusing on improving monitoring and visibility using Amazon CloudWatch and AWS CloudTrail.
Improving Web Application Security on AWS


Task: Implementing Secure Access to EC2 Instances, Ensuring Security and Compliance, and Improving Monitoring and Visibility

In this hands-on, we will go through the steps to enhance the security of a web application hosted on AWS.

 We will implement secure access to EC2 instances using AWS Systems Manager Session Manager, configure IAM policies for access control, set up security and compliance measures using IAM and AWS Config, and improve monitoring and visibility using Amazon CloudWatch and AWS CloudTrail.





11/ Step 1: Implement Secure Access to EC2 Instances

a. Use AWS Systems Manager Session Manager to enable secure remote access to the EC2 instances without opening inbound ports in the security group.

As a first step go to the AWS Management Console and navigate to the EC2 service.

Prerequisites:
- Launch an Amazon EC2 instance(linux) with name of "First EC2"
Make sure you have the necessary permissions to perform the following steps.
Ensure that the EC2 instances are running and have the SSM Agent installed.
Verify that the IAM role associated with the EC2 instances has the required permissions
for Systems Manager.

 install SSM Agent on EC2
To install the AWS Systems Manager (SSM) Agent on an EC2 instance, you can follow these steps:

1.1 Launch an EC2 instance: Start by launching an EC2 instance in your AWS account 
if you haven't done so already.

1.2. Configure the IAM roles: Ensure that your EC2 instance has an IAM role associated with it that 
has the necessary permissions to access Systems Manager. Create or update an IAM role with 
the required permissions to access EC2 instances using Session Manager. The role should have 
the Amazon SSMManagedInstanceCore managed policy attached.

You need to adjust the IAM policy associated with the IAM user or role to include 
the required permissions for starting an SSM session.

Here's an example of a policy that grants the necessary permissions for starting an 
SSM session on EC2 instances:

json
Copy code
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "SSMStartSessionAccess",
      "Effect": "Allow",
      "Action": "ssm:StartSession",
      "Resource": "arn:aws:ec2:us-east-1:626840944227:instance/i-0094dbcb59eceece0"
    }
  ]
}
You can attach this policy to the IAM user or role associated with the provided ARN. 
Alternatively, you can modify an existing policy attached to the user or role by adding
the necessary ssm:StartSession permission.

1.3. Connect to the EC2 instance: Connect to your EC2 instance using SSH 

1.4. Download the SSM Agent: Once connected to the EC2 instance, download the SSM Agent 
installation package using the following command:

wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
sudo yum install -y amazon-ssm-agent.rpm
sudo systemctl start amazon-ssm-agent
sudo systemctl enable amazon-ssm-agent
sudo systemctl status amazon-ssm-agent

If the SSM Agent is active and running without any errors, you should see the status 
as "active (running)" in the output.

1.5. Download and install the Session Manager plugin RPM package.
https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html

sudo yum install -y https://s3.amazonaws.com/session-manager-downloads/plugin/latest/linux_64bit/session-manager-plugin.rpm

Select the EC2 instances hosting your web application.
Choose the "Actions" dropdown and click on "Connect".
In the Connect to your instance section, select "Session Manager" as the connection method.
Click on "Connect" to open a secure shell (SSH) session to your EC2 instance using Systems Manager Session Manager.
Verify that you can access the EC2 instance securely without requiring inbound ports to be open

b. As a second step, configure IAM policies to ensure that only authorized users have access to the EC2 instances using Systems Manager Session Manager.
Go to the IAM service in the AWS Management Console.
Create a new IAM policy that grants necessary permissions for EC2 instances accessed via Systems Manager Session Manager.

Define the policy with appropriate actions, such as ssm:StartSession, ssm:ListSessions, and ssm:TerminateSession.

Attach this policy to IAM roles or users that should have access to the EC2 instances.
Test the access by logging in as an authorized user and verifying the ability to start sessions using Systems Manager Session Manager.

c. Test the secure access to the EC2 instances to ensure that it's working as expected.
Log in as an authorized user who has been assigned the IAM policy granting access to EC2 instances via Systems Manager Session Manager.
Open the AWS Management Console and navigate to the EC2 service.
Select the appropriate EC2 instance for testing.
Choose the "Actions" dropdown and click on "Connect."
In the Connect to your instance section, select "Session Manager" as the connection method.
Click on "Connect" to start a secure shell (SSH) session to the EC2 instance.
Verify that you can access the instance successfully.
Repeat the test with multiple authorized users to ensure access is restricted to authorized individuals only.


12 / Step 2: Ensure Security and Compliance

 a. Implement AWS Identity and Access Management (IAM) to ensure that only authorized users have access to the application and its resources.

Go to the IAM service in the AWS Management Console.
Create IAM roles or users for different user groups with distinct permissions based on their responsibilities.


This role grants full access to manage EC2 resources, including creating, modifying, and terminating instances.

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:*"
            ],
            "Resource": "*"
        }
    ]
}

Define appropriate policies for each IAM role or user, granting access to necessary AWS resources.

IAM policy that grants necessary permissions for EC2 instances accessed via Systems Manager Session Manager:

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:StartSession",
                "ssm:ListSessions",
                "ssm:TerminateSession"
            ],
            "Resource": "*"
        }
    ]
}



Assign these roles or users to individuals based on their requirements and responsibilities.



 b. Implement AWS Config to audit and monitor any changes to the environment and ensure compliance with industry regulations.
Go to the AWS Management Console and navigate to the AWS Config service.
Set up AWS Config rules to monitor the compliance of resources and configurations against desired configurations.
Define rules to track changes made to EC2 instances, security groups, IAM policies, and other relevant resources.
Configure delivery channels to store and receive notifications about configuration changes.



 c. Implement AWS Shield to protect the application against DDoS attacks and other security threats. 

- To implement AWS Shield for protecting our application against DDoS attacks and other security threats, we can follow these steps:

Enable AWS Shield Advanced: 
AWS Shield Advanced provides comprehensive protection against DDoS attacks. We can enable it on the AWS Management Console or through the AWS CLI/API. Enabling Shield Advanced requires a subscription and incurs additional charges.


Configure DDoS Protection: AWS Shield provides automatic protection against common and large-scale DDoS attacks. Shield uses various techniques to detect and mitigate DDoS attacks, such as rate limiting, anomaly detection, and traffic engineering. It helps to ensure that our application remains accessible during an attack.


Steps :

Go to the AWS Management Console :
1. Navigate to the AWS Shield service by typing "Shield" in the search bar or by selecting it from the list of available services.
2. In the AWS Shield console, you will find two options: AWS Shield Standard and AWS Shield Advanced. Shield Standard provides basic protection by     
   default for all AWS customers at no extra cost. Shield Advanced offers more advanced features and requires a subscription with additional charges.
3. Click on the "Get Started" button under either Shield Standard or Shield Advanced, depending on your requirements and budget.
4. If you choose Shield Standard, it will be automatically enabled for your account and associated with all your AWS resources.
5. If you opt for Shield Advanced, you will need to subscribe to the service and configure it according to your needs. This includes providing    
   information about your organization, contact details, and selecting resources to protect.
6. After enabling Shield Advanced, you can configure additional settings such as DDoS protection policies and rate-based rules. These settings allow you  
   to customize the level of protection for your resources and specify thresholds for triggering DDoS mitigation actions.
7. You can also integrate AWS Shield with AWS Web Application Firewall (WAF) for enhanced protection against web-based attacks. This can be done by  
   navigating to the AWS WAF service and following the instructions to set up rules and conditions.
8. Monitor the DDoS protection status and events by accessing the AWS Shield console. It provides real-time visibility into DDoS attacks and mitigation  
   efforts, allowing you to analyze traffic patterns and take necessary actions.
9. AWS Shield also provides integration with other AWS services, such as Amazon CloudFront, Amazon Route 53, and Elastic Load Balancing, to extend  
   protection to your applications and resources distributed across multiple regions.




Using Amazon CloudWatch and AWS CloudTrail.



13 a  / Step 1: Set up CloudWatch monitoring for EC2 instances :

Go to the AWS Management Console and navigate to the CloudWatch service.
Click on "Create dashboard" and give it a name, e.g., "EC2 Monitoring Dashboard."
In the dashboard, click on "Add widget" and select "Line" or "Number" to display metrics such as CPU, memory, and disk usage.
Choose the EC2 instances you want to monitor and select the appropriate metrics for each instance.
Customize the widget properties and choose the desired time range for the data.
Save the dashboard and regularly review the metrics to identify any performance issues.

b /Step 2: Create CloudWatch alarms for anomaly detection :

In the CloudWatch service, navigate to "Alarms" in the sidebar.
Click on "Create alarm" and select the metric you want to monitor, such as CPU utilization, memory usage and disk usuge.
Define the conditions for triggering the alarm, e.g., if CPU utilization exceeds a certain threshold for a specified period.
Specify the actions to take when the alarm is triggered, such as sending a notification to an SNS topic or executing an AWS Lambda function.
Set up additional actions as necessary, such as stopping or terminating instances if the situation requires it.
Repeat steps 2-5 for each metric and anomaly you want to monitor.
Regularly review and fine-tune the alarms to ensure they are effective in detecting potential issues.

 c / Step 3: Enable AWS CloudTrail for API activity logging :

Go to the AWS Management Console and navigate to the CloudTrail service.
Click on "Trails" in the sidebar and then "Create trail."
Give the trail a name, e.g., "Application API Activity."
Choose the S3 bucket where you want to store the CloudTrail logs.
Specify the management events and data events you want to log, such as EC2 instance-related events and changes to security groups.
Configure advanced settings if needed, such as enabling log file encryption.
Review the trail settings and create the trail.

Once the trail is created, we can use CloudTrail's insights to monitor and analyze API activity and changes to our environment.
Remember to regularly review and analyze the CloudWatch metrics and alarms to detect any performance issues or anomalies promptly. 
Additionally, leverage CloudTrail's logs to monitor API activity and changes for enhanced security.
