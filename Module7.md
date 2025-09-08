# Module 7: Responding to and Managing an Incident

---

## Introduction

Welcome to the **Responding to and Managing an Incident** module. This section introduces the key concepts and structure of the module.

### Module Objectives

By the end of this module, you should be able to:

- Identify an incident  
- Describe AWS services used for incident recognition and remediation  
- Identify best practices for incident response  

---

### Module Overview

This module includes the following sections:

- Identifying an incident  
- AWS services that support the discovery and recognition phase  
- AWS services that support the resolution and recovery phase  
- Best practices for handling an incident  

**Lab Activity:**  
- Remediating an incident using AWS Config and AWS Lambda  

**Knowledge Check:**  
- A quiz to test your understanding of key concepts covered in the module  

---

## Bank Business Scenario

### Scenario (1 of 3)

This module applies to a bank business scenario. It demonstrates how incident response concepts are relevant in a real-world context.

---

### Scenario (2 of 3)

After discussing AWS services for threat mitigation, John gives María full support to begin preparing for migration to the AWS Cloud. María is pleased but understands that securing cloud resources is an ongoing responsibility.

---

### Scenario (3 of 3)

Due to the migration, the bank’s incident response plan must be reviewed and updated. The new plan should include updated tools and methods for the security team. This becomes María’s next project.

---

## Shared Responsibility Model

### Overview

The **Shared Responsibility Model** outlines the division of security responsibilities between AWS and the customer.

**Customer Responsibilities (Security *in* the cloud):**

- Customer data  
- Platform, applications, identity and access management  
- Operating system, network, and firewall configuration  
- Client-side data encryption and integrity, authentication  
- Server-side encryption of file system and data  
- Network traffic protection (encryption, integrity, identity)  

**AWS Responsibilities (Security *of* the cloud):**

- AWS foundational services (compute, storage, databases, networking)  
- AWS Global Infrastructure (Regions, Availability Zones, Edge Locations)  

---

Now that we’ve covered the tools AWS provides for logging and monitoring access, let’s explore how to respond to a security incident.


---

## Section 1: Identifying an Incident

### Incident Recognition and Response

**Incident response** is a set of information security policies and procedures used to:

- Identify, contain, and eliminate cyberattacks  
- Quickly detect and halt attacks  
- Minimize damage and prevent future attacks  

---

### Recognizing Incidents

Not all events are incidents requiring immediate action. Examples of non-critical events include:

- **Logging in from a remote location**: May be legitimate if the employee is traveling or using a VPN  
- **Failing hard drive that is still operational**: Allows for scheduled replacement without emergency action  
- **Unauthorized access attempts**: If access is denied, it may not be a breach but still worth monitoring  

Organizations must differentiate between abnormal events and true incidents that require analysis and remediation.

---

## Incident Response Phases

### Phase 1: Discovery and Recognition

This phase includes:

- **Incident identification, logging, and categorization**  
- **Incident notification and escalation**  
- **Investigation and diagnosis**  

**Notifications** are triggered by alerts and can be sent via email, SMS, or push notifications.  
**Escalation** occurs when an employee cannot resolve the issue and passes it to a specialist.  
**Investigation** involves gathering data and developing strategies to resolve threats.

AWS provides services to help discover and identify events that may lead to or constitute an incident.

---

### Phase 2: Resolution and Recovery

This phase includes:

- **Forensic isolation**: Isolate the issue and perform a deep dive. Capture disk images or system configurations. Reproduce bugs if applicable.  
- **Stage a fix**: Apply and test the fix in a controlled environment  
- **Deploy the fix**: Push updates to production  
- **Incident closure**: Finalize resolution and document the incident  

Automate these processes where possible and conduct **game days** in staging environments to refine incident response procedures.

AWS offers services to support incident remediation.

---

## Key Takeaways: Identifying an Incident

- Incident response involves identifying, containing, and eliminating cyberattacks  
- Not all events are incidents requiring immediate remedy  
- The first phase is **discovery and recognition**  
- The second phase is **resolution and recovery**


## Section 2: Discovery and Recognition Phase

AWS offers several services that support the **discovery and recognition** phase of incident response. These services help organizations identify potential attacks and security threats.

### Services Covered in This Section:

- AWS Trusted Advisor  
- Amazon CloudWatch  
- Amazon Inspector  
- Amazon GuardDuty  
- AWS Shield  
- AWS Config  

---

### AWS Trusted Advisor

**AWS Trusted Advisor** helps improve performance and close security gaps by providing recommendations based on best practices learned from serving hundreds of thousands of AWS customers.

It inspects your AWS environment and suggests improvements in areas such as:

- Performance  
- Security  
- Cost optimization  
- Fault tolerance  
- Service quotas  

**Access Based on Support Plan:**

- **Basic & Developer Support**: Access core security checks and service quota checks via the AWS Management Console  
- **Business, Enterprise On-Ramp, & Enterprise Support**: Access all checks via the Console, AWS Support API, and AWS CLI  

You can also use **Amazon EventBridge** to monitor the status of Trusted Advisor checks.

For more information, visit the AWS Trusted Advisor User Guide.

---

### Amazon CloudWatch

**Amazon CloudWatch** is a reliable, scalable, and flexible monitoring solution that can be used within minutes. It eliminates the need to set up and manage your own monitoring infrastructure.

Key features:

- Automatically displays metrics for every AWS service you use  
- Allows creation of custom dashboards for your applications  
- Enables alarms that monitor metrics and send notifications or trigger automated actions when thresholds are breached  
- Provides system-wide visibility into resource utilization, application performance, and operational health  

For more information, see the [Amazon Cloudatch User Guide.

---

### Amazon Inspector

**Amazon Inspector** is a vulnerability management service that continuously scans your AWS workloads for vulnerabilities.

Key capabilities:

- Automatically discovers and scans EC2 instances and container images in Amazon ECR  
- Identifies software vulnerabilities and unintended network exposure  
- Generates **findings** that describe the issue, affected resources, severity, and remediation guidance  

For more information, see the Amazon Inspector User Guide.

---

### Amazon GuardDuty

**Amazon GuardDuty** is a continuous security monitoring service that identifies unexpected and potentially unauthorized or malicious activity.

How it works:

- Uses threat intelligence feeds (e.g., malicious IPs and domains) and machine learning  
- Detects issues like privilege escalations, exposed credentials, and communication with malicious entities  
- Monitors AWS account access behavior for signs of compromise (e.g., unusual API calls or deployments in unused regions)  

**Example:** Detects compromised EC2 instances being used for malware distribution or cryptocurrency mining.

For more information, see the Amazon GuardDuty User Guide.

---

### AWS Shield

**AWS Shield** protects against Distributed Denial of Service (DDoS) attacks.

Key features:

- Automatically protects applications hosted on AWS from common DDoS attacks  
- **AWS Shield Advanced** offers enhanced threat detection, mitigation, and response capabilities  

**Analogy:** A DDoS attack is like a traffic jam that clogs up a highway, preventing regular traffic from reaching its destination.

For more information, see the AWS Shield Developer Guide.

---

### AWS Config

**AWS Config** is a continuous monitoring and assessment service that provides:

- An inventory of your AWS resources  
- A record of configuration changes over time  
- The ability to view current and historical configurations  
- Notifications when changes occur  
- Integration with other AWS services for remediation  

You can use AWS Config to troubleshoot outages, analyze security incidents, and restore resources to a known good state.

For more information, see the [AWS Config UserGuide.

---

### Evaluating Rules with AWS Config

As configuration changes occur, AWS Config:

1. **Records and normalizes** the changes  
2. **Evaluates** them against your defined rules  
3. **Displays** compliance results in the console or via API  

You can configure:

- **AWS Systems Manager** or **Amazon SNS** to trigger automatic remediation or alerts  
- **Amazon S3** to store change history and snapshots for analysis  

---

## Key Takeaways: AWS Services for Discovery and Recognition

AWS provides several services to support the **discovery and recognition** phase of incident response:

- **AWS Trusted Advisor**: Inspects your environment and recommends improvements for cost, performance, and security  
- **Amazon CloudWatch**: Offers scalable monitoring and alerting for AWS services and custom applications  
- **AWS Config**: Continuously monitors and records configuration changes, enabling compliance and troubleshooting  
- **Amazon Inspector**: Scans workloads for vulnerabilities and network exposure  
- **AWS Shield**: Protects against DDoS attacks  
- **Amazon GuardDuty**: Continuously monitors for unauthorized or malicious activity using threat intelligence and machine learning

---
## Section 3: Resolution and Recovery Phase

### AWS Services That Support the Resolution and Recovery Phase

AWS provides several services to support the **resolution and recovery** phase of incident response. These services help organizations respond to incidents, restore operations, and automate recovery workflows.

### Services Covered in This Section:

- AWS Systems Manager  
- AWS CloudFormation  
- Amazon Simple Notification Service (Amazon SNS)  
- AWS Step Functions  
- AWS Lambda  

> **Note:** Lambda, Amazon SNS, and Step Functions are all **event-driven response services**, meaning they respond to actions generated by users or systems.

---

### AWS Systems Manager

**AWS Systems Manager** provides visibility and control over your AWS infrastructure.

Key features:

- Unified user interface to view operational data from multiple AWS services  
- Group resources by application for monitoring and troubleshooting  
- Perform on-demand changes (e.g., update applications, run shell scripts)  
- Automate operational tasks like patching and configuration enforcement  

For more information, see the AWS Systems Manager User Guide.

---

### AWS CloudFormation

**AWS CloudFormation** helps you model and set up AWS resources using infrastructure as code.

Key capabilities:

- Create templates that describe all required AWS resources  
- Automatically provision and configure resources  
- Recreate staging environments inside isolated forensic VPCs for deep investigation  
- Apply and test fixes before pushing to production  

For more information, see the AWS CloudFormation User Guide.

---

### Amazon Simple Notification Service (Amazon SNS)

**Amazon SNS** is an event-driven web service that enables applications, users, and devices to send and receive notifications instantly.

Key features:

- Integrates with AWS Lambda to trigger functions based on published messages  
- Enables automated workflows and alerts for potential exploits  
- Supports message routing to other SNS topics or AWS services  

For more information, see the Amazon SNS Developer Guide.

--
### AWS Step Functions

**AWS Step Functions** is a low-code, visual workflow service used to:

- Build distributed applications  
- Automate IT and business processes  
- Create data and machine learning pipelines  

Key features:

- Event-driven workflows  
- Built-in support for failure handling, retries, parallelization, service integrations, and observability  
- Enables developers to focus on business logic rather than infrastructure  

Use case: When abnormalities occur, Step Functions can orchestrate complex workflows that connect services, systems, or people within minutes.

For more information, see the [AWS Step Functions Developer Guide](https### AWS Lambda

**AWS Lambda** is a serverless, event-driven compute service that allows you to run code on demand without provisioning or managing servers.

Key features:

- Stateless functions that scale automatically  
- Pay only for compute time used  
- Can be triggered by AWS services or custom applications  
- Supports integration with services like Amazon S3, SNS, and EventBridge  
- Can use AWS Key Management Service (KMS) to securely store and retrieve secrets  

**Invocation Models:**

- **Push model**: Event sources directly invoke Lambda  
- **Pull model**: Lambda polls event sources  
- **Request-response model**: Lambda runs synchronously and returns a response  

For more information, see the AWS Lambda Developer Guide.

---

### Lambda for Incident Response

Lambda can be used in **event-driven architectures** to automate incident response.

**Example Workflow:**

1. A user issues a `StopLogging` request to CloudTrail.  
2. CloudTrail and EventBridge detect the event and invoke a Lambda function.  
3. The Lambda function collects event details (e.g., who disabled logging, when, and what resource).  
4. The function sends a notification via Amazon SNS to a response analyst.  
5. Another Lambda function is invoked to automatically restart CloudTrail logging.

This setup reduces the time between detection and response, enabling faster remediation.

---

### Working Together for Incident Response

You can combine **Step Functions**, **Lambda**, **CloudFormation**, and **Amazon SNS** to remediate a compromised instance.

**Example Workflow:**

1. A script or tool pushes an instance ID to an SNS topic.  
2. A Lambda function verifies the ID and initiates a Step Functions workflow if compromised.  
3. The workflow:
   - Removes the instance from its Auto Scaling group  
   - Creates snapshots of attached EBS volumes  
   - Isolates the instance by removing security groups and assigning a forensics group  
   - Uses CloudFormation to create a new VPC and forensics instance  
   - Performs a basic investigation on the volumes  
   - Sends a report via SNS to the security team

## Key takeaways:
A key takeaway from this section of the module is that AWS offers several services that support the resolution and recovery phase, including the following:
 - Systems Manager gives you visibility and control of your infrastructure on AWS.
 - With CloudFormation, you can create and provision AWS infrastructure deployments predictably and repeatedly.
 - Lambda is an event-driven response service that provides the ability to run code without provisioning or managing servers.
 - Amazon SNSis an event-driven response web service that coordinates and manages delivering or sending messages to subscribing endpoints or clients.
 - Step Functions is an event-driven response service that makes it easy to coordinate the components of distributed applications as a series of steps in a visual workflow.
 - AWS Training and Certification Responding to and Managing an Incident 

## Section 6: Best Practices in Responding to and Managing an Incident

To improve your incident response capabilities, consider the following industry best practices:

### 1. Identify Key Personnel, External Resources, and Tooling

- Maintain a contact list of internal personnel involved in incident response  
- Engage with external partners when necessary  
- Research and test tools that support incident response and recovery  

---

### 2. Automate Containment Capabilities

- Use automation to reduce response times and minimize organizational impact  
- Implement event-driven mechanisms to trigger containment actions automatically  

---

### 3. Develop Incident Response Plans

- Create clear, easy-to-follow runbooks for responding to incidents  
- Include escalation and communication plans for internal and external stakeholders  
- Document root causes and mitigation strategies to prevent recurrence  

---

### 4. Pre-Provision Access and Tools

- Ensure security personnel have the correct access pre-provisioned in AWS  
- Deploy necessary tools in advance to enable rapid response  

---

### 5. Run Incident Response Game Days

- Conduct simulated incident response events involving key staff and management  
- Use game days to test procedures for different threat scenarios  
- Apply lessons learned to improve your incident response processes  
