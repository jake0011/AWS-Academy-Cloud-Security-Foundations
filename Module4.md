# Module 4: Securing Your Infrastructure

# Section 1: Introduction

Welcome to the Securing Your Infrastructure module. This first section provides an introduction to the module.

## Module Objectives

At the end of this module, you should be able to do the following:
*   Define the components of a virtual private cloud (VPC).
*   Recognize account boundaries.
*   Describe Amazon Web Services (AWS) services that are available to protect your network and resources.

## Module Overview

### Sections
This module includes the following sections:
*   Structure of a three-tier web application
*   Using a VPC
*   Setting up public and private subnets and internet protocols
*   Using AWS security groups
*   Using AWS network ACLs
*   Using AWS load balancers
*   Pulling it all together
*   Protecting your compute resources

### Lab
This module also includes a lab where you will secure VPC resources by using security groups.

### Knowledge Check
Finally, you will be asked to complete a knowledge check that will test your understanding of key concepts covered in this module.

---

## Bank Business Scenario

Let’s discuss how the concepts in this module are applicable to the bank business scenario.

After María’s last meeting with John, she realized that John’s security concerns go beyond user access and management. To make her case for AWS migration, María will need to assuage John’s concerns by helping him understand how the bank’s infrastructure can be properly secured in AWS.

John asked María how they would ensure the security of the infrastructure that AnyBank could potentially migrate into AWS. For the next meeting, María decides to focus on using a virtual private cloud (VPC) and applying multiple layers of security to support the network infrastructure.

---

## Shared Responsibility Model

This module focuses on the **operating system, network, and firewall configuration** portion, and the **network traffic protection** portion of the shared responsibility model, which the customer is responsible to secure.

> **Accessibility Description:**
> Shared responsibility model listing customer and AWS responsibilities.
>
> *   **Customer is responsible for security *in* the cloud.** This includes:
>     *   Customer data
>     *   Platform, applications, identity and access management
>     *   Operating system, network, and firewall configuration
>     *   Client-side data encryption and data integrity, authentication
>     *   Server-side encryption of file system and data
>     *   Networking traffic protection, to include encryption, integrity, and identity
>
> *   **AWS is responsible for security *of* the cloud.** This includes:
>     *   The AWS foundation services for compute, storage, databases, and networking
>     *   The AWS Global Infrastructure, to include Regions, Availability Zones, and Edge Locations
>
> *End of accessibility description.*


## Section 1.1 Structure of a Three-Tier Web Application

This section describes the structure of a three-tier web application.

## Three-Tier Architecture

> **Accessibility Description:**
> Diagram of a three-tier web application that includes a presentation layer, business logic layer, and data storage layer. A VPC has subnets in two Availability Zones. The presentation layer includes two public subnets, one in each Availability Zone. One subnet contains a security group with a host instance, and the other subnet has a NAT gateway. An Application Load Balancer in this layer manages traffic between the internet and a frontend Auto Scaling group in the business logic layer. This Auto Scaling group handles scaling for frontend instances across the two Availability Zones. A Network Load Balancer in the business logic layer manages traffic between the frontend Auto Scaling group and a backend Auto Scaling group. The backend Auto Scaling group handles scaling for backend instances across the two Availability Zones. The data storage layer contains a primary database in one Availability Zone, with a read-only database in the second Availability Zone. The data storage layer communicates with the backend Auto Scaling group in the business logic layer.
> *End of accessibility description.*

A three-tier architecture is a software architecture pattern where the application has three logical tiers: the presentation layer, the application layer, and the data storage layer. This architecture is used in a client-server application such as a web application that has a frontend, backend, and database. Each layer or tier performs a specific task and can be managed independently of each other. This represents a shift from the monolithic way of building an application where the frontend, backend, and database are all in one place.

---

# Section 2: Using a VPC

This section provides information about using a VPC as part of securing your infrastructure.

## Amazon Virtual Private Cloud (Amazon VPC)

*   Provision a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define.
*   Control your virtual networking resources, including the following:
    *   Select the IP address range.
    *   Create subnets.
    *   Configure route tables and network gateways.
*   Customize the network configuration for your VPC.
*   Use multiple layers of security.

Use the Amazon Virtual Private Cloud (Amazon VPC) service to provision a logically isolated section of the AWS Cloud where you can launch your AWS resources. This isolated section is called a virtual private cloud, or VPC.

Amazon VPC provides control over your virtual networking resources, such as selecting your own IP address range, creating subnets, and configuring route tables and network gateways. You can use both IPv4 and IPv6 in your VPC for secure access to resources and applications.

You can also customize the network configuration for your VPC. For example, you can create a public subnet for your web servers that can access the public internet. You can place your backend systems, such as databases or application servers, in a private subnet without public internet access.

Finally, you can use multiple layers of security in a VPC to help control access to Amazon Elastic Compute Cloud (Amazon EC2) instances in the VPC's subnets. Security mechanisms include security groups and network access control lists (ACLs).

> For more information, see [What Is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) in the Amazon VPC User Guide.

## VPCs and Subnets

*   **VPC**
    *   Is logically isolated from other VPCs.
    *   Is dedicated to your AWS account.
    *   Belongs to a single AWS Region and can span multiple Availability Zones.
*   **Subnet**
    *   Is a range of IP addresses that divide a VPC.
    *   Belongs to a single Availability Zone.
    *   Is classified as public or private.

> **Accessibility Description:**
> Diagram of subnets in a VPC. A Region within the AWS Cloud has one VPC, which spreads across two Availability Zones. The VPC has one subnet in each Availability Zone.
> *End of accessibility description.*

A **VPC** is a virtual network that is logically isolated from other virtual networks in the AWS Cloud. A VPC is dedicated to your account, belongs to a single AWS Region, and can span multiple Availability Zones.

After you create a VPC, you can divide it into one or more subnets. A **subnet** is a range of IP addresses that divide a VPC. Subnets belong to a single Availability Zone, but you can create subnets in different Availability Zones for high availability. Subnets are generally classified as public or private.

# Section 3: Setting Up Public and Private Subnets and Internet Protocols

This section describes how to set up public and private subnets and internet protocols.

## Internet Gateway

An internet gateway serves two purposes:
*   Provides a target in your VPC route tables for internet-routable traffic.
*   Performs network address translation (NAT) for instances that have been assigned public IPv4 addresses.

An internet gateway supports IPv4 and IPv6 traffic, and it doesn't cause availability risks or bandwidth constraints on your network traffic. There's no additional charge to have an internet gateway in your account.

To enable access to or from the internet for instances in a subnet in a VPC, you must do the following:
1.  Create an internet gateway, and attach it to your VPC.
2.  Add a route to your subnet's route table that directs internet-bound traffic to the internet gateway.
3.  Confirm that each instance in your subnet has a globally unique IP address (public IPv4 address, Elastic IP address, or IPv6 address).
4.  Confirm that your network ACL and security group rules allow the relevant traffic to flow to and from your instance.

> For more information, see [Connect to the Internet Using an Internet Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html) in the Amazon VPC User Guide.

## NAT Gateway

A network address translation (NAT) gateway:
*   Supports instances in a private subnet to connect to the internet or other AWS services.
*   Prevents the internet from initiating a connection to those instances.
*   Requires that you specify the following at creation:
    *   The public subnet in which the NAT gateway should reside.
    *   An Elastic IP address to associate with the NAT gateway.
*   After creation, requires that you update the route table for one or more of your private subnets to direct internet-bound traffic to the NAT gateway.

You can also use a NAT instance in a public subnet in your VPC instead of a NAT gateway. However, a NAT gateway is a managed NAT service that provides better availability, higher bandwidth, and less administrative effort. For common use cases, AWS recommends that you use a NAT gateway instead of a NAT instance.

> **For more information, see the following topics in the Amazon VPC User Guide:**
> *   [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)
> *   [NAT Instances](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_NAT_Instance.html)
> *   [Compare NAT Gateways and NAT Instances](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-comparison.html)

## Private Subnet

*   All subnets consist of a contiguous range of IP addresses.
*   Interfaces that are attached to instances in private subnets cannot be reached from outside the parent VPC.
*   Private subnets are often used to host database instances that don't need to be accessed through the public internet.

> **Accessibility Description:** Diagram of a VPC with a public subnet and a private subnet. An EC2 instance in the private subnet routes traffic to a private route table, and then to a NAT gateway in the public subnet. From there, the traffic is routed to a public route table and then to an internet gateway.
> *End of accessibility description.*

All subnets consist of a contiguous range of IP addresses. These addresses should not overlap with other subnets in your VPC. Interfaces that are attached to EC2 instances in private subnets are not reachable from outside the parent VPC. However, by using an AWS managed NAT gateway, EC2 instances can make outbound requests (such as for patching), and the response from the external resource will be allowed back in. Private subnets are often used to host database (DB) instances that don't need to be accessed through the public internet. A NAT gateway must have an Elastic IP address assigned to it.

## Public Subnet

When external traffic needs to reach an interface, such as an Amazon Elastic Compute Cloud (Amazon EC2) instance, the following is required:
*   A public IP address must be assigned to the EC2 instance.
*   The subnet's route table must include an entry that directs traffic to an internet gateway.
*   With these two factors in place, the subnet is considered to be a public subnet.

> **Accessibility Description:** Diagram of a VPC with a public subnet. Traffic from an EC2 instance in the subnet goes to a public route table and then to an internet gateway.
> *End of accessibility description.*

Often, companies will place web servers inside a public subnet. However, AWS recommends using a load balancer in the public subnet and having the load balancer relay traffic to web servers that are hosted in private subnets.

## IP Addressing

*   When you create a VPC, you assign a CIDR range (a range of private addresses).
*   You cannot change the range in a VPC or subnet, but you can add more CIDR ranges to your VPC.
*   The largest CIDR block size is /16, and the smallest is /28.
*   The CIDR ranges of subnets shouldn't overlap.

When you create a VPC, you assign a Classless Inter-Domain Routing (CIDR) range to it, which is a range of private addresses. The CIDR block might be as large as /16 (65,536 addresses) or as small as /28 (16 addresses). The CIDR block of a subnet can be the same as the block for the VPC (meaning the VPC has a single subnet), or it can be a subset of the VPC's CIDR block to support multiple subnets. If you create more than one subnet in a VPC, their CIDR ranges should not overlap.

> For more information, see [VPC Sizing](https://docs.aws.amazon.com/vpc/latest/userguide/configure-your-vpc.html#vpc-sizing) in the Amazon VPC User Guide.

### Reserved IP Addresses
When you create a subnet, it requires its own CIDR block. For each CIDR block that you specify, AWS reserves five IP addresses within that block that you cannot use.

**Example:** A VPC with an IPv4 CIDR block of `10.0.0.0/16` has 65,536 total IP addresses. The VPC has a subnet with a `/24` CIDR block (`10.0.0.0/24`). Although this subnet has 256 IP addresses, only 251 are available for use because 5 are reserved.

| IP Address for CIDR Block 10.0.0.0/24 | Reserved for |
| :--- | :--- |
| `10.0.0.0` | Network address |
| `10.0.0.1` | VPC local router (internal communication) |
| `10.0.0.2` | Domain Name System (DNS) resolution |
| `10.0.0.3` | Future use |
| `10.0.0.255` | Network broadcast address |

### Public IP Address
*   A public IP address is an IP address that is used to access the internet.
*   A public IP address can be automatically assigned if you modify the subnet's auto-assign public IP address properties.
*   Public IP addresses are dynamic. If you stop and then start your instance, a new public IP address is assigned. For production projects, use an Elastic IP address.

> For more information, see [Public IPv4 Addresses](https://docs.aws.amazon.com/vpc/latest/userguide/how-it-works.html#vpc-public-ipv4-addresses) in the Amazon VPC User Guide.

### Elastic IP Address
*   Is associated with an AWS account.
*   Is static and doesn't change over time.
*   Comes from Amazon's pool of IPv4 addresses.
*   If you unassign an Elastic IP address, you are charged for it until you release it from your account.

An Elastic IP address is a static, public IP address that is designed for dynamic cloud computing. You can associate an Elastic IP address with any instance or network interface for any VPC in your account. By using an Elastic IP address, you can mask the failure of an instance or software by rapidly remapping the address to another instance in your account. If your instance doesn't have a public IPv4 address, you can associate an Elastic IP address with your instance to enable communication with the internet. Additional costs might apply when you use Elastic IP addresses, so it's important to release them when you no longer need them.

> For more information, see [Associate Elastic IP Addresses with Resources in Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-eips.html) in the Amazon VPC User Guide.

### Elastic Network Interface
An elastic network interface is a virtual network interface. You can do the following:
*   Attach it to an instance.
*   Detach it from the instance, and attach it to another instance to redirect network traffic.
*   Its attributes follow when it is reattached to a new instance.
*   Each instance in your VPC has a default network interface, which is assigned a private IPv4 address from your VPC's range.

You cannot detach a primary (default) network interface from an instance. You can create and attach additional network interfaces to any instance in your VPC. In certain circumstances, you can have two network interfaces on an EC2 instance, which is great for forensics. You can associate the network interface to a forensic instance and start tracking an exploit.

### Route Tables and Routes
*   A route table contains a set of rules (or routes), which you can configure to direct network traffic from your subnet.
*   Each route specifies a destination and a target.
*   By default, every route table contains a local route for communication within the VPC.
*   Each subnet should have its own route table.

> **Accessibility Description:** Diagram of a VPC with public and private route tables. The public subnet contains an EC2 instance and a NAT gateway. The NAT gateway directs traffic to a public route table and then to an internet gateway. The private subnet contains an EC2 instance whose traffic goes to a private route table. From there, traffic is directed to the NAT gateway in the public subnet.
> *End of accessibility description.*

A route table contains a set of rules (called routes) that direct network traffic from your subnet. By default, every route table you create contains a local route for communication within the VPC, which you cannot delete. Each subnet should have its own route table, or it will use the main route table of the parent VPC. The main route table controls routing for all subnets that are not explicitly associated with any other route table.

> For more information, see [Configure Route Tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html) in the Amazon VPC User Guide.

---

## Key Takeaways
*   Public subnets are used when external traffic needs to reach an interface, such as an EC2 instance.
*   Private subnets are often used to host database instances that don't need to be accessed through the public internet.
*   Route tables determine where traffic is routed in your VPC.

  # Using AWS Security Groups

This section provides information about using security groups as part of securing your infrastructure.

## What is a Security Group?

> **Accessibility Description:**
> Diagram of a VPC with a public subnet and a private subnet. The public subnet contains an EC2 instance that is protected by security group 1. The private subnet contains an Amazon Relational Database Service (Amazon RDS) database instance that is protected by security group 2. Communication is allowed between the two security groups, and the internet can communicate with security group 1.
> *End of accessibility description.*

A **security group** acts as a virtual firewall for an EC2 instance, and controls inbound and outbound traffic for the instance. Security groups act at the **instance level**, not the subnet level. Therefore, each instance in a subnet in your VPC can be assigned to a different set of security groups.

At the most basic level, a security group is a way to filter traffic to your instances.

## Security Group Rules and Behavior

*   Security groups have rules that control inbound and outbound instance traffic.
*   Default security groups deny all inbound traffic and allow all outbound traffic. This behavior is considered **stateful**.

When you create a security group, it has no inbound rules. Therefore, no inbound traffic that originates from another host to your instance is permitted until you add inbound rules.

By default, a security group includes an outbound rule that allows all outbound traffic. You can remove this rule and add more specific outbound rules. If your security group has no outbound rules, then no outbound traffic that originates from your instance is allowed.

### Stateful Nature
Security groups are **stateful**, which means that state information is kept even after a request is processed.
*   If you send a request *from* your instance, the response traffic for that request is allowed to flow *in*, regardless of inbound security group rules.
*   Responses to allowed inbound traffic are allowed to flow *out*, regardless of outbound rules.

### Rule Evaluation
**All rules** are evaluated before a decision is made to allow traffic.

### Default Security Group Rules Example

| Inbound | | | |
| :--- | :--- | :--- | :--- |
| **Source** | **Protocol** | **Port Range** | **Description** |
| sg-xxxxxxxx | All | All | Allow inbound traffic from network interfaces that are assigned to the same security group. |

<br>

| Outbound | | | |
| :--- | :--- | :--- | :--- |
| **Destination** | **Protocol** | **Port Range** | **Description** |
| 0.0.0.0/0 | All | All | Allow all outbound IPv4 traffic. |
| ::/0 | All | All | Allow all outbound IPv6 traffic. |

> For more information, see [Control Traffic to Resources Using Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the Amazon VPC User Guide.

---

## Key Takeaways
*   A security group acts as a virtual firewall for an instance to control inbound and outbound traffic.
*   Security groups are **stateful**, which means that state information is kept even after a request is processed.
*   All rules are evaluated before a decision is made to allow traffic.

# Using AWS Network ACLs

This section provides information about using network access control lists (ACLs) as part of securing your infrastructure.

## What are Network ACLs?

> **Accessibility Description:**
> Diagram showing how network ACLs work. A VPC contains a public subnet and a private subnet. Each subnet contains an EC2 instance. A network ACL in each subnet controls traffic to and from the instances.
> *End of accessibility description.*

A **network access control list (ACL)** is an optional layer of security for your VPC. A network ACL acts as a firewall to control traffic in and out of one or more subnets. To add another layer of security to your VPC, you can set up network ACLs with rules that are similar to your security group rules.

Each subnet in your VPC must be associated with a network ACL. If you don't explicitly associate a subnet with a network ACL, the subnet is automatically associated with the default network ACL. You can associate a network ACL with multiple subnets; however, a subnet can be associated with only one network ACL at a time. When you associate a new network ACL with a subnet, the previous association is removed.

## Network ACL Rules and Behavior

*   A network ACL has separate inbound and outbound rules, and each rule can either **allow or deny** traffic.
*   Network ACLs are **stateless**.
*   Each VPC automatically comes with a modifiable **default network ACL**. By default, it allows all inbound and outbound IPv4 traffic.
*   You can create a **custom network ACL** and associate it with a subnet. By default, each custom network ACL denies all inbound and outbound traffic until you add rules.

Network ACLs are **stateless**, which means that responses to inbound traffic are subject to the rules for outbound traffic (and vice versa).

Rules are evaluated in **number order** before a decision is made to allow traffic.

Each network ACL also includes a rule whose rule number is an asterisk (`*`). This rule ensures that if a packet doesn't match any of the other numbered rules, it's denied. You can't modify or remove this rule.

### Default Network ACL Rules Example (IPv4 Only)

| Rule # | Type | Protocol | Port Range | Source | Allow/Deny |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 100 | All IPv4 traffic | All | All | 0.0.0.0/0 | ALLOW |
| * | All IPv4 traffic | All | All | 0.0.0.0/0 | DENY |

> For more information, see [Control Traffic to Subnets Using Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html) in the Amazon VPC User Guide.

## Comparing Security Groups and Network ACLs

| Attribute | Security Groups | Network ACLs |
| :--- | :--- | :--- |
| **Scope** | Instance or interface level | Subnet level |
| **Supported Rules** | Allow rules only | Allow and deny rules |
| **State** | **Stateful** (return traffic is automatically allowed, regardless of rules) | **Stateless** (return traffic must be explicitly allowed by rules) |
| **Order of Rules** | All rules are evaluated before a decision is made | Rules are evaluated in number order before a decision is made |

## VPC Security Features

> **Accessibility Description:**
> EC2 instance surrounded by layers of security. The closest layer to the instance is a security group. The next layer is network ACLs. The outside, and farthest layer, is subnet routing.
> *End of accessibility description.*

VPC security features include the following layers:
*   **Security groups** act as virtual firewalls for your EC2 instances to control inbound and outbound traffic.
*   **Network ACLs** provide an optional layer of security for your VPC. They act as firewalls to control traffic in and out of one or more subnets.
*   **Subnets** make networks more efficient. Through subnetting, network traffic can travel a shorter distance without passing through unnecessary routers to reach its destination.
*   **Route tables** control where network traffic is directed.

With the **VPC Flow Logs** feature, you can capture information about the IP traffic going to and from network interfaces in your VPC. You can publish flow log data to Amazon CloudWatch Logs or Amazon Simple Storage Service (Amazon S3). You can create a flow log for a VPC, subnet, or network interface.

> For more information, see [Logging IP Traffic Using VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html) in the Amazon VPC User Guide.

---

## Key Takeaways
*   A network ACL is an optional layer of security for your VPC and acts as a firewall to control traffic at the **subnet level**.
*   Each subnet in your VPC must be associated with a network ACL.
*   Network ACLs are **stateless**, which means that responses to inbound traffic are subject to the rules for outbound traffic (and vice versa).
*   Rules are evaluated in **number order** before a decision is made to allow traffic.

# Using AWS Load Balancers

This section provides information about using load balancers as part of securing your infrastructure.

## Elastic Load Balancing (ELB)

*   Distributes incoming application traffic.
*   Supports high availability.
*   Performs health checks on instances.
*   Provides the following types:
    *   Application Load Balancer
    *   Network Load Balancer
    *   Classic Load Balancer

> **Accessibility Description:**
> Diagram of traffic from a user going to an application load balancer. Traffic is then split between two EC2 instances.
> *End of accessibility description.*

The **Elastic Load Balancing (ELB)** service automatically distributes incoming application traffic across multiple targets, such as EC2 instances, containers, and IP addresses. You configure your load balancer to accept incoming traffic by specifying one or more **listeners**, which are processes that check for connection requests.

ELB scales your load-balancing device as traffic changes, increasing the availability and fault tolerance of your applications. You can add and remove instances from your load balancer as your needs change without disrupting the overall flow of requests. ELB can also configure **health checks** to monitor the health of registered targets, sending requests only to healthy ones.

ELB is integrated with other popular AWS services such as Amazon EC2 Auto Scaling, Amazon Elastic Container Service (Amazon ECS), AWS CloudFormation, and AWS Certificate Manager (ACM).

### Types of Load Balancers

*   **Application Load Balancer:** Operates at the request level (Layer 7) and routes traffic to targets (EC2 instances, containers, IP addresses, and AWS Lambda functions) based on the content of the request (e.g., URL path). It is ideal for advanced load balancing of HTTP and HTTPS traffic and is well-suited for microservices and container-based applications. It simplifies security by ensuring that the latest SSL and TLS ciphers and protocols are used.
*   **Network Load Balancer:** Operates at the connection level (Layer 4) and routes connections to targets based on IP protocol data. It is ideal for load balancing both TCP and UDP traffic and is capable of handling millions of requests per second with ultra-low latencies. It is optimized for sudden and volatile traffic patterns.
*   **Classic Load Balancer:** This is a legacy load balancer that provides basic load balancing across multiple EC2 instances and operates at both the request and connection levels. It is intended for applications that were built within the EC2-Classic network.

> For more information, see [What Is Elastic Load Balancing?](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html) in the ELB User Guide.

## Data Protection in ELB

*   **Single point of contact:** A load balancer serves as the single point of contact for clients, distributing traffic across multiple targets in multiple Availability Zones. This increases your application's availability. An Application Load Balancer can also manage secure HTTPS communication and certificates.
*   **Encryption at rest:** If you enable server-side encryption for your S3 bucket where ELB access logs are stored, ELB automatically encrypts each access log file before storing it.
*   **Encryption in transit:** ELB simplifies building secure web applications by terminating HTTPS and TLS traffic from clients at the load balancer. The load balancer performs the work of encrypting and decrypting the traffic, instead of requiring each EC2 instance to handle TLS termination.

> For more information, see [Data Protection in Elastic Load Balancing](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/data-protection.html) in the ELB User Guide.

## Load Balancers in Action

This diagram shows how load balancers work in a multi-tier architecture.
*   A VPC has subnets in two Availability Zones.
*   Internet traffic goes from an internet gateway to load balancers in the public subnets.
*   These **first-layer load balancers** direct traffic to web servers in a private subnet.
*   Traffic from the web servers goes to a **second-layer load balancer**, which directs the traffic to application servers in another private subnet.
*   Traffic from the application servers goes to the primary database server, which can communicate with a standby database server in the second Availability Zone.

---

## Key Takeaways
*   ELB automatically distributes incoming application traffic across multiple targets, such as EC2 instances, containers, IP addresses, and Lambda functions.
*   ELB can handle the varying load of your application traffic in a single Availability Zone or across multiple Availability Zones.
*   You can add and remove instances from your load balancer as your needs change, without disrupting the overall flow of requests to your application.

# Pulling It All Together

This section examines how all of these security features work together.

## Workflow

The diagram on this slide shows how load balancing and VPC components work together.
*   A VPC has two subnets: a public subnet (Subnet A) and a private subnet (Subnet B).
*   **In Subnet A (Public):**
    *   EC2 instances send traffic through a load balancer.
    *   Traffic passes through a security group, then a network ACL.
    *   Finally, traffic routes through a route table to an internet gateway for public access.
*   **In Subnet B (Private):**
    *   EC2 instances send traffic through a load balancer.
    *   Traffic passes through a security group, then a network ACL.
    *   Finally, traffic routes through a route table to a NAT gateway located in Subnet A for outbound internet access.

## Best Practices to Protect Your Network

*   **Control traffic at all layers.** For a VPC, this includes using security groups, network ACLs, and subnets. Use subnets in multiple Availability Zones to separate layers of your application. Configure security groups and network ACLs to only allow the necessary inbound and outbound traffic.
*   **Inspect and filter your traffic at the application level.**
*   **Automate network protection.** Use threat intelligence and anomaly detection to automate protection mechanisms to provide a self-defending network.
*   **Limit exposure.** Limit the exposure of the workload to the internet and internal networks by only allowing the minimum required access.

---

# Protecting Your Compute Resources

This section describes best practices to protect your compute resources.

## Amazon Inspector

*   Run automated security assessments on EC2 instances and applications.
*   Identify application security issues.
*   Enforce security standards and best practices.
*   Generate assessment reports.

**Amazon Inspector** is an automated security assessment service that helps improve the security and compliance of applications deployed on AWS. The service helps you to identify security vulnerabilities and deviations from security best practices in applications, both before they are deployed and while they are running in a production environment. For example, the service can help you check for unintended network accessibility of your EC2 instances and for vulnerabilities on those instances.

Amazon Inspector provides you the opportunity to define standards and best practices for your applications, and validate adherence to these standards. After performing an assessment, Amazon Inspector produces a detailed list of security findings, prioritized by level of severity.

> For more information, see [Amazon Inspector](https://aws.amazon.com/inspector).

### Security Benefits of Amazon Inspector

*   **Automate tasks to help you respond to security issues:** When you use Amazon EventBridge events with Amazon Inspector, you can automate tasks to help you respond to security issues that Amazon Inspector findings reveal.
*   **Regularly monitor your resources:** Amazon Inspector helps to find security vulnerabilities in applications and departures from security best practices before your application is deployed and while it's running in production.
*   **Benefit from AWS security expertise:** Amazon Inspector includes a knowledge base of rules charted to common security best practices and vulnerability definitions. AWS constantly updates the security best practices and rules.
*   **Integrate security into DevOps:** Amazon Inspector is an API-bound service that analyzes network configurations and uses an optional agent for visibility into EC2 instances. This helps you to build security assessments into your existing DevOps process.

## AWS Systems Manager

Amazon Inspector uses the widely deployed **AWS Systems Manager Agent (SSM Agent)** to collect the software inventory and configurations from your EC2 instances. The collected application inventory and configurations are then used to assess workloads for vulnerabilities.

**AWS Systems Manager** gives you visibility and control of your infrastructure on AWS. It provides a unified user interface so you can view operational data from multiple AWS services and automate management tasks. You can collect system inventory, apply operating system patches, maintain up-to-date antivirus definitions, and configure operating systems and applications at scale.

---

## Key Takeaways
*   Amazon Inspector is an automated security assessment service that helps improve the security and compliance of applications deployed on AWS.
*   Systems Manager gives you visibility and control of your infrastructure on AWS.
*   Scan your compute resources regularly for vulnerabilities, and patch them accordingly. You can automate this task by using AWS services such as Lambda and Systems Manager.

# Module Review

It’s now time to review the module, and wrap up with a knowledge check and discussion of a practice certification exam question.

## Module Summary
In this module, you learned how to do the following:
*   Define the components of a VPC.
*   Recognize account boundaries.
*   Describe AWS services that are available to protect your network and resources.

## Complete the Knowledge Check
It is now time to complete the knowledge check for this module.

---

## Sample Exam Question

> A system administrator created a single EC2 instance, and set up network ACLs and the appropriate subnet routing. However, they want to provide an extra layer of security by applying a firewall to control access to and from the EC2 instance. Which action should the system administrator take?

*   **A.** Create a network ACL.
*   **B.** Configure a security group.
*   **C.** Update the subnet route tables.
*   **D.** Set up a load balancer.

### Sample Exam Question Answer

> A system administrator created a single EC2 instance, and set up network ACLs and the appropriate subnet routing. However, they want to provide an extra layer of security by applying a firewall to control access to and from the EC2 instance. Which action should the system administrator take?

The keywords in the question are **applying a firewall** and **control access to and from the EC2 instance**.

The correct answer is **B**. A security group acts as a virtual firewall for your EC2 instances to control inbound and outbound traffic.

**Incorrect answers:**
*   **A:** A network ACL acts as a firewall for associated subnets to control inbound and outbound traffic, but it operates at the **subnet level**, not the instance level.
*   **C:** A route table is used to control where network traffic is directed. It does not function as a firewall.
*   **D:** A load balancer automatically distributes incoming application traffic and scales resources to meet traffic demands. It does not function as a firewall.
