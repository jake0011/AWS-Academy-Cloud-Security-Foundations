# Module 6: Logging and Monitoring
__

## Introduction

At the end of this module, you should be able to do the following:  
• Log and monitor access and control to help identify security threats.  
• Read and interpret log reports to identify security threats.  
• Monitor and report on your Amazon Web Services (AWS) resources and applications.  
• Recognize when to use Amazon CloudWatch and when to use AWS CloudTrail.

---

### Module Sections

This module includes the following sections:  
• Importance of logging and monitoring  
• Capture and collect  
• AWS services with built-in logs  
• Monitor and report  
• Best practices for logging and monitoring  
• Additional AWS services for logging and monitoring

This module also includes the following:  
• A demonstration to introduce you to AWS Security Hub  
• An activity that walks you through an AWS CloudTrail log file  
• A lab where you will configure monitoring and alerting with AWS CloudTrail and Amazon CloudWatch  
• A knowledge check that will test your understanding of key concepts covered in this module

---

### Scenario Context

Let’s discuss how the concepts in this module are applicable to the bank business scenario.

John’s story of an insider threat at his previous employer gave María an idea.  
At their next meeting, María plans to discuss how to use the logging and monitoring capabilities that AWS provides.

These capabilities can help an organization to identify, mitigate, and remediate security threats.  
María creates a list of AWS logging and monitoring services.

She will discuss these AWS offerings with John and explain the potential benefits and, if necessary, the potential costs.  
María wants to align each service with the threat it could help mitigate so she’s prepared for questions or scenarios that John might bring up.

Now that we've secured all of the tiers of a cloud application, let's look at the tools that AWS provides that help you log and monitor access to your resources to help you identify and react to anomalous behavior.

---

## Section 2:  Logging and Monitoring Concepts

### Logging

Logging is the collection and recording of activity and event data. The service itself can provide logging capabilities, as with Amazon Virtual Private Cloud (Amazon VPC) Flow Logs and Amazon Simple Storage Service (Amazon S3) server access logs. Or a secondary service, such as AWS CloudTrail, might provide logging.

The type of information logged will vary based on the service that is conducting the logging, but some common elements are logged:  
• Date and time of event  
• Originating location of request  
• Identity of resources accessed

A comprehensive logging methodology can help you in every phase of your incident response strategy. Log files are also a primary focus point in incident response. This is because of their ability to provide detailed, time-stamped records to investigators and incident responders.

Logging provides a record of events captured at a specific point in time. Common log files include access logs, application logs, system logs, event logs, API call logs, and database logs.

Log files can assist in troubleshooting performance issues within your AWS Cloud and on-premises environment. Logs are also necessary to perform security audits and adhere to recordkeeping requirements to demonstrate compliance with a number of regulatory measures.

Examples of regulatory measures include the Health Insurance Portability and Accountability Act (HIPAA) in the United States, the European Union General Data Protection Regulation (GDPR), and Brazil's General Data Protection Law (LGPD).

Finally, log files can assist you to remediate issues that arise organically or through audits. Logs provide you with insights on the point in time when an issue arose.

---

### Monitoring

Monitoring is the practice of continuously verifying the security and performance of your resources and data. AWS provides several services that you can use to monitor your resources, but this module will focus on two: AWS CloudTrail and Amazon CloudWatch.

CloudTrail provides a record of actions taken within your environment. CloudTrail logs include information such as the action type, identity of the user, and time and date of the action. With this information, you can monitor who is doing what and when they are doing it.

With CloudWatch, you can monitor your resources and applications in real time. CloudWatch provides you with system-wide visibility into resource utilization, application performance, and operational health.

Monitoring services and tools, along with a solid incident response plan, can help you mitigate the effects of a system outage or malicious actor. In many cases, monitoring can help you to spot an issue before it has operational impact.

---

### Key Takeaways

• Logging is the collection and recording of activity and event data.  
• Monitoring is the continuous verification of the security and performance of your resources, applications, and data.  
• AWS provides several services that you can use to log and monitor your resources.

---

## Section 3: Capturing and Collecting Logs

### AWS CloudTrail Overview

AWS CloudTrail is a service that enables governance, compliance, operational auditing, and risk auditing of your AWS account. It records actions taken by users, roles, or AWS services as events. These events include actions performed via:

- AWS Management Console  
- AWS Command Line Interface (AWS CLI)  
- AWS Software Development Kits (SDKs) and APIs  

You can view, search, download, archive, analyze, and respond to these recorded events in the CloudTrail console.

CloudTrail can be integrated into applications using its API. You can automate trail creation for your organization, check trail statuses, and control user access to CloudTrail events.

For more information, visit [AWS CloudTrail]

CloudTrail records key information about each API call, including:

- API name  
- Identity of the caller  
- Time of the API call (in UTC)  
- Origin location of the call  
- Request parameters  
- Response elements returned by the AWS service  

This data helps you:

- Track changes to AWS resources  
- Troubleshoot operational issues  
- Ensure compliance with internal policies and regulatory standards  

---

## Section 4: AWS Services with Built-in Logging

### Amazon S3: Server Access Logging

Server access logging is a built-in feature of **Amazon S3**. You can enable this feature to generate detailed records of requests made to an S3 bucket. These logs are useful for:

- Security and access audits  
- Gaining insights into your customer base  
- Understanding storage usage and billing  

For more information, see Logging Requests Using Server Access Logging in the Amazon S3 User Guide.

---

### Amazon VPC: VPC Flow Logs

**VPC Flow Logs** is a built-in feature of **Amazon VPC**. It captures information about inbound and outbound IP traffic from your VPC network interfaces. Flow logs can be created at:

- VPC level  
- Subnet level  
- Individual network interface level  

You can publish flow log data to:

- Amazon CloudWatch Logs  
- Amazon S3  

Flow logs help with:

- Troubleshooting  
- Monitoring  
- Analyzing IP traffic flow within your VPC  

Because flow log data is collected outside the path of network traffic, it does not affect throughput, latency, or performance.

For more information, see [Logging IP Traffic with VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest) Elastic Load Balancing (ELB): Access Logs

**ELB access logs** capture detailed information about requests sent to your load balancer. These logs include:

- Time of request receipt  
- Client IP address  
- Latencies  
- Request paths  
- Server responses  

This information is useful for:

- Troubleshooting issues  
- Analyzing network traffic  

For more information, see [Access Logs for Your Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-access-logs.html)

---

## Section 5: Monitting and reporting

This section covers monitoring and reporting.

**Amazon CloudWatch** is a monitoring and observability service designed for:

- DevOps engineers  
- Developers  
- Site Reliability Engineers (SREs)  
- IT managers  
- Product owners  

CloudWatch collects monitoring and operational data as:

- Logs  
- Metrics  
- Events  

This data provides a unified view of AWS resources, applications, and services running both on AWS and on-premises servers.

You can use CloudWatch to:

- Detect anomalous behavior  
- Set alarms  
- Visualize logs and metrics side by side  
- Take automated actions  
- Troubleshoot issues  
- Discover insights to keep applications running smoothly  

CloudWatch helps break down data silos, enabling system-wide visibility and faster issue resolution.

---

### CloudWatch Events and EventBridge

**CloudWatch Events** delivers a near-real-time stream of system events that describe changes in AWS resources. It:

- Detects operational changes as they occur  
- Responds by sending messages, activating functions, making changes, and capturing state information  

CloudWatch Events is currently being replaced by **Amazon EventBridge**.

---

### CloudWatch Logs

With **CloudWatch Logs**, you can:

- Monitor, store, and access log files from EC2 instances, CloudTrail, Amazon Route 53, and other sources  
- Centralize logs from all systems, applications, and AWS services  
- View logs as a consistent flow of time-ordered events  
- Search for error codes or patterns  
- Filter logs by specific fields  
- Archive logs securely for future analysis  
- Query logs using a powerful query language  
- Visualize log data in dashboards  

For more information, visit Amazon CloudWatch.

---

## CloudTrail and CloudWatch Integration

**AWS CloudTrail** continuously monitors and logs activity of users, groups, and roles in your AWS environment. These logs are useful for:

- Compliance auditing  
- Security analysis  
- Troubleshooting  

Example: If an EC2 instance is deleted without permission, CloudTrail can identify the user, group, or role responsible.

**Amazon CloudWatch** continuously monitors the performance of AWS resources and applications. It can detect and notify you of anomalous behavior, such as unauthorized EC2 instance deletion.

When used together:

- You can create custom CloudWatch alarms and notifications for specific CloudTrail events  
- This integration enables quick responses to key operational issues

---

## Key Takeaways

- CloudWatch provides a unified view of the operational health of AWS resources, applications, and services  
- CloudWatch collects monitoring and operational data as logs, metrics, and events  
- CloudTrail monitors **actions**, while CloudWatch monitors **performance**  
- You can create custom dashboards, alarms, and notifications for key metrics

---

## Section 6: Best Practices for Logging and Monitoring

### Define Your Requirements

The first step in using AWS logging and monitoring capabilities is to **define your requirements**. This includes:

- Identifying the resources, applications, and services to maintain logs for  
- Understanding organizational, legal, and compliance requirements for your workloads  
- Evaluating AWS resources available to support your logging and monitoring needs  

---

### Collect Metrics and Define Baselines

By collecting metrics and defining baselines, you can:

- Gain insights into potential security threats  
- Establish normal behavior patterns  
- Detect anomalies and malicious activity  

---

### Alerting and Response

- Define who should receive alerts  
- Specify what actions should be taken when alerts are triggered  
- Ensure alerts are actionable and relevant to the responsible teams  

---

### Configure Logging Throughout the Workload

Logging should be configured across all layers, including:

- Application logs  
- AWS service logs  
- Resource logs  

---

### Centralize and Automate Log Collection

- Collect logs centrally for easier analysis and management  
- Use automation tools like **Amazon CloudWatch** to analyze logs  
- Detect anomalies or indicators of compromise efficiently  


---

## Section 7: Additional AWS Services for Logging and Monitoring

### AWS Trusted Advisor

**AWS Trusted Advisor** provides recommendations based on AWS best practices learned from serving hundreds of thousands of customers. It evaluates your account using checks across five categories:

- Cost optimization  
- Security  
- Fault tolerance  
- Performance  
- Service quotas  

Trusted Advisor helps you:

- Optimize AWS infrastructure  
- Improve security posture  
- Reduce costs  
- Monitor service limits  

As an administrator, Trusted Advisor can automate the process of identifying improvements, saving time and effort.

**Availability by Support Plan:**

- **Basic & Developer Support**: Access to core security checks and service quota checks  
- **Business & Enterprise Support**: Access to all checks  

For more information, visit the AWS Trusted Advisor User Guide.

---

### Amazon EventBridge

**Amazon EventBridge** is a serverless event bus service that enables event-driven application architecture. It connects applications using events—signals that indicate a change in system state.

Key features:

- No need to provision or manage servers  
- Automatic scaling based on event volume  
- Built-in fault tolerance  

**Use Cases for Monitoring and Auditing:**

- Monitor AWS environments in real-time  
- Respond to operational changes  
- Prevent infrastructure vulnerabilities  

**Example:**  
When resources are accessed by cross-accounts or public accounts, you can configure an **Amazon Access Analyzer** event to trigger an **AWS Lambda** function via EventBridge to remove unintended permissions.

EventBridge was formerly known as **Amazon CloudWatch Events**. Both services use the same API, but new features are added only to EventBridge.

For more information, visit the [Amazon EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html) security posture through automated, continuous security checks. It aggregates alerts from:

- AWS services  
- Third-party partner products  

Security Hub presents findings in a standardized format, making it easier to act on alerts. You can:

- Create automated response and remediation workflows  
- Integrate with EventBridge  
- View security scores per standard and across accounts  

**Example Use Case:**  
Security Hub helps prioritize response efforts by correlating findings across accounts and resources, aiding central security and DevSecOps teams.

For more information, visit AWS Security Hub.

---

### AWS Config

**AWS Config** enables you to assess, audit, and evaluate AWS resource configurations. It continuously monitors and records configuration changes, allowing you to:

- Automate evaluation against desired configurations  
- View IAM policies assigned to users, groups, or roles at any recorded time  
- Maintain auditing and compliance with historical data  

**Use Cases:**

- Compliance auditing  
- Security analysis  
- Change management  
- Operational troubleshooting  

**Example:**  
When resources are created, updated, or deleted, AWS Config streams changes to **Amazon SNS**, notifying you of all configuration changes. It also maps relationships between resources to assess impact.

For more information, visit AWS Config.
