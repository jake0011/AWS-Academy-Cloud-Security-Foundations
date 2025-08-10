# Module Title: Introduction to Security on AWS.

## Incident Response and Mitigation

Even with mature preventive and detective controls, you should put processes in place to respond to and mitigate the potential impact of security incidents. The architecture of your workload strongly affects your ability to operate effectively during an incident, isolate or contain systems, and restore operations to a known good state. Put tools and access in place ahead of a security incident. Then, routinely practice incident response through game days. This will help you ensure that your architecture can accommodate timely investigation and recovery. Another module in this course will describe a variety of approaches to incident response.

In AWS, the following practices facilitate effective incident response:

- **Detailed logging is available.** Logs contain important content such as file access and changes.
- **Events can be automatically processed** and invoke tools that automate responses through the use of AWS APIs.
- **You can pre-provision tooling and a “clean room”** by using AWS CloudFormation. This provides the ability to carry out forensics in a safe, isolated environment.

---

## Reducing the Attack Surface

Generally, a cyberattack shuts down due to two reasons: either the attackers exhaust themselves and give up, or the attackers achieve their goal. Reduce your exposure to unintended access by hardening operating systems and minimizing the components, libraries, and externally consumable services in use. Start by reducing unused components, such as operating system packages and applications. Configure security groups and network access control lists (ACLs) in Amazon Virtual Private Cloud (Amazon VPC) to help reduce the attack surface of your applications.

Certain AWS services, such as AWS Auto Scaling and Amazon CloudFront, help applications to scale to absorb common infrastructure layer attacks such as UDP reflection attacks and SYN floods.

- A **UDP reflection attack** takes place when the attacker asks the target computer for information by using a forged source address.
- A **SYN flood** is a type of distributed denial of service (DDoS) attack that aims to make a server unavailable to legitimate traffic by consuming all available server resources.

By using techniques such as automatic scaling, you can absorb larger volumes of application layer attacks.

> For more information, see the [Security Pillar: AWS Well-Architected Framework whitepaper](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/welcome.html).

---

## Core Design Principles for Security

The key takeaways from this section of the module are the design principles for security in the cloud:

- Apply the principle of **least privilege**.
- Enable **traceability**.
- **Secure all layers**.
- **Automate security**.
- **Protect data in transit and data at rest**.
- **Prepare for security events**.
- **Minimize the attack surface**.

---

## The Shared Responsibility Model

This section covers the shared responsibility model.

> **Accessibility Description:**  
> Shared responsibility model listing customer and AWS responsibilities. Customer is responsible for security in the cloud. This includes customer data. Platform, applications, identity and access management. Operating system, network, and firewall configuration. Client-side data encryption and data integrity, authentication. Server-side encryption of file system and data. Networking traffic protection, to include encryption, integrity, and identity. AWS is responsible for security of the cloud. This includes the AWS foundation services for compute, storage, databases, and networking. And the AWS Global Infrastructure, to include Regions, Availability Zones, and Edge Locations. End of accessibility description.

Security and compliance are shared responsibilities between AWS and customers.

### AWS Responsibility: Security *OF* the Cloud
This responsibility includes securing components, from the host operating system and virtualization layer down to the physical security of the facilities where the service operates. AWS is responsible for protecting the global infrastructure that runs all the services that are offered in the AWS Cloud. This infrastructure is composed of the hardware, software, networking, and facilities that run AWS Cloud services.

### Customer Responsibility: Security *IN* the Cloud
You assume responsibility and management in the cloud. The security steps that you must take depend on the services that you use and the complexity of your system. Customer responsibilities include selecting and securing operating systems that run on EC2 instances, and securing the applications that are launched on AWS resources. Customers must also select and handle security group configurations, firewall configurations, network configurations, and secure account management. Customers are also responsible for managing their data, including encryption options.

To reiterate, AWS secures the hardware, software, facilities, and networks that run all AWS products and services. You are responsible for what you implement by using AWS products and services, and for the applications that you connect to AWS. The security steps that you must take depend on the services that you use and the complexity of your system.

### Example Scenario: Shared Responsibility in Practice

Consider an example where your company uses Amazon S3 to store data. Your AWS environment also includes EC2 instances and an Amazon Relational Database Service (Amazon RDS) instance. These resources run a MySQL database, which is deployed inside a virtual private cloud (VPC). One EC2 instance hosts a web server, and the web application that runs on it uses the database to store application data.

In this scenario:

- **AWS is responsible for:** protecting the global infrastructure, which contains the physical servers that host the virtual machines and storage hardware. These virtual machines and storage hardware host your S3 bucket, EC2 instances, and database instance. AWS is responsible for the security of the physical networking infrastructure that ensures that these components can be accessed. AWS is also responsible for the security of the hypervisor layer that hosts the EC2 instances. (The hypervisor is the host OS that runs the EC2 instances, which are virtual machines that run guest operating systems.)

- **You (the customer) are responsible for:** managing the guest OS that runs on the EC2 instances (including Microsoft Windows or Linux OS updates and security patches). You are also responsible for managing any application software or utilities that you install. Additionally, you are responsible for the configuration of the security groups that control network access to each EC2 instance and to the RDS database instance. You are also responsible for configuring security on the S3 bucket and the objects that you store in it. For example, you could use one or more of the security features that AWS provides, such as bucket policies, data encryption, and S3 Block Public Access.

> For more information, see [Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model).

### Customer Control Over Security
While AWS secures and maintains the cloud infrastructure, **you are responsible for securing everything that you put IN the cloud.**

Before you architect any workload, you need to put practices in place that influence security. You will want to control who can do what. In addition, you want to be able to identify security incidents, protect your systems and services, and maintain the confidentiality and integrity of data through data protection. You should have a well-defined and practiced process to respond to security incidents. These tools and techniques are important because they support objectives such as preventing financial loss or complying with regulatory obligations.

Because AWS physically secures the infrastructure that supports our cloud services, as an AWS customer you can focus on using services to accomplish your goals. The AWS Cloud also provides greater access to security data and an automated approach to respond to security events.

When using AWS services, you maintain complete control over your content and are responsible for managing critical security requirements, including the following:
- The content you choose to store on AWS
- The AWS services that are used with the content
- The country in which that content is stored
- The format and structure of that content, and whether it is masked, anonymized, or encrypted
- Who has access to that content, and how those access rights are granted, managed, and revoked

You retain control of what security you choose to implement to protect your own data, platform, applications, identity and access management, and operating system. This means that the shared responsibility model changes depending on the AWS services that you use.

### Activity: Analyzing a Scenario
> **Accessibility Description:**  
> The architectural diagram shows an AWS Cloud box which contains an AWS Global Infrastructure area, as well as an Amazon S3 bucket, and finally a VPC which contains an Amazon EC2 instance and an Oracle instance. End of accessibility description.

**Scenario:** A customer uses Amazon S3 to store data. The customer manages a VPC that contains an EC2 instance and an Amazon RDS for Oracle database instance. Who is responsible for maintaining the security of each component?

**Analysis:**

- **The customer controls the security *in* the cloud.** In the scenario above, this includes updating and patching the guest operating system and associated application software on the EC2 instance, and configuration of the AWS provided security group firewall. Because the customer's responsibility is determined by the AWS Cloud services that they select, the Oracle upgrades or patches will be a customer responsibility if they are running the Oracle database on an EC2 instance. The customer is also responsible for using IAM tools to apply the correct permissions at the platform level, such as for S3 buckets, and at the IAM user or group level.

- **AWS manages security *of* the cloud** by ensuring that the AWS infrastructure complies with global and regional regulatory requirements and best practices. AWS operates, manages, and controls the components from the host operating system and virtualization layer down to the physical security of the facilities that the service operates in. In this example, AWS is responsible for the physical security of the data center and the virtualization infrastructure. If the Oracle instance runs as an Amazon RDS instance, then AWS is responsible for the Oracle upgrades and patches. Amazon RDS automates common administrative tasks, such as performing backups and patching the software that powers your database.

---

## Security and Governance with a Managed Services Organization (MSO)

One approach to implement security and governance is to create a centralized team that is responsible for establishing repeatable processes and templates to deploy applications to AWS while maintaining organizational control over the deployments. Such a team could be either internal or external (third-party vendors), and is often referred to as a **provisioning team, center of excellence, or managed services organization (MSO)**. External vendors are commonly referred to as **managed service providers (MSPs)**. AWS validates AWS Partners under the AWS Managed Service Provider (MSP) Program.

MSOs or MSPs are typically responsible for provisioning accounts; establishing repeatable processes for deployment; auditing the deployments of workload owners; and hosting shared services for security, continuous monitoring, connectivity, and authentication. MSOs essentially create the guardrails for security, data protection, and disaster recovery in the company.

> For more information, see [AWS Managed Service Provider (MSP) Program](https://aws.amazon.com/partners/programs/msp).

### The MSO Model and Workload Owners
In the MSO model, **workload owners** handle the actual deployment, development, and maintenance of applications. Workload owners typically include system administrators, developers, and others who are directly responsible for one or more applications. Adding an MSO helps ensure that applications are deployed in a secure and compliant fashion through the automated implementation of organizational security requirements. Having an MSO also means that the workload owner can scope down their authorization documentation to only the configuration and installation of software that is specific to a particular application. This is because the workload owner inherits a significant portion of the security control implementation from the MSO.

### Common MSO Activities
AWS customer MSOs often perform the following activities:
- **Account provisioning:** After reviewing the workload owner’s use case, the MSO establishes the initial account, connects it to the appropriate account for consolidated billing, and configures basic security functionality before granting access to the workload owner.
- **Security oversight:** With centralized account provisioning, the MSO can implement features that enable security personnel to monitor the application as it is deployed and managed. The MSO might perform activities such as establishing an auditor group with cross-account access and linking the application VPC to a shared services VPC that the MSO controls.
- **Amazon VPC configuration:** An Amazon VPC configuration includes the VPC and its subnets, security group configurations, and network access control lists (ACLs). To maintain tighter control over the application VPCs, the MSO might retain control of VPC configuration and require the workload owner to request wanted changes to network security.
- **IAM configuration:** The MSO can create user groups and assign permissions. This can include creating groups for internal auditors, an IAM superuser, and application administrative groups that are segregated by functionality (for example, database and Unix administrators).
- **Development and approval of templates:** The MSO can create preapproved CloudFormation templates for common use cases. By using templates, workload owners inherit the security implementation of the approved template, which limits their authorization documentation to the features that are unique to their application. Templates can be reused to shorten the time required to approve and deploy new applications.
- **AMI creation and management:** The MSO can create a library of common, approved AMIs for the organization, thus providing centralized management and updating of machine images. By creating common templates, the MSO can enforce the use of approved AMIs.
- **Development of a shared services VPC:** With a shared service VPC, the MSO can receive nearly continuous monitoring feeds from the organization’s application VPC and common, shared services that are required for their organization. This often includes a shared access management platform, logging endpoints, and aggregation of configuration information.

---

## Module Review

### Key Takeaways
- The AWS shared responsibility model helps organizations that adopt the cloud to achieve their security and compliance goals.
- Customers are responsible for securing everything they put **IN** the cloud.
- An MSO essentially creates the guardrails for security, data protection, and disaster recovery.

### Learning Objectives Summary
In this module, you learned how to do the following:
- Identify security features and benefits of cloud computing.
- Identify the security principles that the AWS Cloud is structured around.
- Identify which part of an application the user is responsible to secure in the cloud.

### Knowledge Check: Practice Question
Sample exam question:
According to the shared responsibility model, who is responsible for configuring security group rules to determine which ports are open to an EC2 Linux instance?

Choice Response

  A. AWS is responsible for configuring security group rules.
  
  B. The customer is responsible for configuring security group rules.
  
  C. Security group rules are not needed.
  
  D. AWS is responsible for configuring security group rules, and the customer must open ports to the instance.

*Look at the answer choices and rule them out based on the keywords.*

**Keywords to recognize:** configuring security group rules.

**The correct answer is B.**

The customer is responsible for controlling network access to EC2 instances.

**Incorrect answers:**
- **Answer A:** AWS maintains the configuration of its infrastructure devices, but the customer is responsible for configuring their own guest operating systems (including networking traffic protection), databases, and applications.
- **Answer C:** Security group rules filter traffic based on protocols and port numbers, and the customer is responsible for configuring the networking traffic protection.
- **Answer D:** AWS maintains the configuration of its infrastructure devices, but the customer is responsible for configuring their own guest operating systems (including networking traffic protection), databases, and applications.
