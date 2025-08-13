# Module 3: Securing Access to Cloud Resources


## Section 1: Introduction
Welcome to the Securing Access to Cloud Resources module. This first section provides an introduction to the module.

## Learning Objectives

At the end of this module, you should be able to do the following:
*   Authorize access to Amazon Web Services (AWS) services by using AWS Identity and Access Management (IAM) users, groups, and roles.
*   Differentiate between different types of security credentials in IAM.
*   Authorize access to AWS services by using identity-based and resource-based policies.
*   Identify other AWS services that provide authentication and access management services.
*   Centrally manage and enforce policies for multiple AWS accounts.

---

## Module Outline

This module includes the following sections:
*   IAM fundamentals
*   Authenticating with IAM
*   Authorizing with IAM
*   Examples of authorizing with IAM
*   Additional authentication and access management services
*   Using AWS Organizations

This module also includes the following:
*   Demonstration: Amazon S3 Cross-Account Resource-Based Policy
*   Lab: Using Resource-Based Policies to Secure an S3 Bucket

Finally, you will be asked to complete a knowledge check that will test your understanding of key concepts covered in this module.

Let’s discuss how the concepts in this module are applicable to the bank business scenario.

# Scenario: Securing a Bank's Cloud Migration

### The Initial Meeting
María’s initial meeting with John went well. John has a strong grasp of the fundamentals of cloud security, and they want to use the shared responsibility model as a focal point for their discussions. John agrees that migrating to the AWS Cloud could be a smart move to modernize the bank’s business operations.

However, he has reservations about how the bank could adequately secure online access for their members while also maintaining internal security protocols.

### John's Concerns & María's Plan
John is particularly focused on restricting internal access to critical resources as much as possible.

In preparation for their next meeting, María decides to focus on the AWS services for identity and access management, including services that could be used to roll out a planned mobile app. She also plans to discuss security best practices, such as the principle of least privilege, to address his concerns about access.

---

### Focus Area: The Shared Responsibility Model

This module focuses on the **platform, applications, and identity and access management** portion of the shared responsibility model, which the customer is responsible to secure.

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

# Section 2: IAM Fundamentals

This section covers fundamental information about the AWS Identity and Access Management (IAM) service.

## What is IAM?

IAM is a web service that helps you to securely control access to AWS resources. Use IAM to control who is **authenticated** (signed in) and **authorized** (has permissions) to use resources.

Key features of IAM include:
*   **Centralized Control:** IAM is integrated into most AWS services. You can define access controls from one place in the AWS Management Console, and they will take effect throughout your AWS environment.
*   **Federated Access:** You can use IAM to grant users, groups, and applications granular access by using existing identity systems. AWS supports federation from corporate systems such as Microsoft Active Directory and standards-based identity providers. This capability is beneficial when it comes to integrating new cloud deployments with existing on-premises infrastructure.
*   **Multi-Factor Authentication (MFA):** If MFA is activated and an IAM user attempts to log in, the user is prompted for an authentication code. The authentication code is delivered to a specially configured AWS MFA hardware device or to a software-based AWS MFA device, such as Google's Authenticator application.
*   **Audit Support:** By using AWS CloudTrail, IAM can also support information assurance by providing log records that include the identity information of users that request resources in your account.

> **For more information:**
> *   See [What Is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the AWS Identity and Access Management User Guide.
> *   See [AWS Services that Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the AWS Identity and Access Management User Guide.

## Authentication vs. Authorization

With IAM, you can use the identity of requesters to control who can use your AWS resources (**authentication**) and also use access management to control what resources they can use and in what ways (**authorization**).

To understand the fundamental aspects of authentication and authorization, consider a banking analogy:
1.  **Authentication (Are you who you say you are?):** Before a bank allows you to access your checking account online, it must ensure you are who you claim to be. They typically accomplish this by requiring you to enter your username and a password. For extra security, they might use two-factor authentication, requiring a code from your phone.
2.  **Authorization (What are you allowed to do?):** Once you are successfully logged in (authenticated), you are authorized to access the money *in your account* and pay bills with it. However, you are *not* authorized to access the money in another customer's account.

How does this relate to IAM? Similar to how a bank must secure its resources (your money), you—as an AWS account owner—must secure access to your AWS account. You want your data to be secure from others, and you want your application logic to be safe from modification. IAM provides the features to accomplish these security objectives.

## IAM Building Blocks: Users, Groups, Roles, and Policies

You can use IAM to control access to your AWS resources by creating users, groups, and roles, and attaching policies to them.

*   **IAM User:** An entity you create in AWS to represent a person or application that interacts with AWS. A user is given a permanent set of credentials. The first user in a new account is the root user.
*   **IAM Group:** A collection of IAM users. You can use groups to grant the same set of permissions to multiple users efficiently.
*   **IAM Role:** An AWS identity with specific permissions that does not have long-term credentials. When a person or application assumes a role, they are provided with temporary security credentials for that session.
*   **IAM Policy:** A document that explicitly lists permissions. A policy can be attached to a user, group, role, or a combination of these.

> **Best Practice:** To configure long-term access, a best practice is to attach IAM policies to IAM groups, and then assign IAM users to these IAM groups. An IAM user in a group inherits the permissions of that group. You can also attach policies directly to a user for more specific permissions.

> For more information, see [Security Best Practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the AWS Identity and Access Management User Guide.

## Key IAM Terminology

*   **IAM Entities:** The IAM resource objects that AWS uses for **authentication**. These include IAM users and roles.
*   **IAM Identities:** The IAM resource objects that are used to **identify and group**, and to which you can attach a policy. These include users, groups, and roles.
*   **IAM Resources:** The users, groups, roles, policies, and identity provider objects that are stored in IAM.
*   **Principal:** A person or application that uses the AWS account root user, an IAM user, or an IAM role to sign in and make requests to AWS. This includes federated users and assumed roles.

## The Request Context

A **request** is made any time a principal attempts to use the console, API, or AWS CLI. The request contains the following information:
*   **Actions or operations:** What the principal wants to perform.
*   **Resources:** The object or objects upon which the actions are performed.
*   **Principal:** The person or application (using a user or role) sending the request.
*   **Environment data:** IP address, user agent, SSL status, time of day, etc.
*   **Resource data:** Data related to the resource being requested.

AWS gathers all this information into a **request context**, which is then used to evaluate and authorize (or deny) the request.

## Service Endpoints and Policies

To connect to an AWS service, you must use its entry point URL, known as an **endpoint**. An example is `ec2.us-east-1.amazonaws.com`.

You can create **endpoint policies** and attach them to endpoints to control access.
*   Endpoint policies **do not** override or replace IAM user policies or service-specific policies.
*   An endpoint policy is a separate policy to *only* control access *from* the endpoint to the specified service.
*   You can only attach one policy to an endpoint, but you can modify it at any time.
*   These policies are similar to IAM policies but must contain a `Principal` element and cannot exceed 20,480 characters.

---

## Key Takeaways
*   IAM is a web service that helps you securely control access to AWS resources.
*   **Authentication** deals with *who* is requesting access. **Authorization** determines *what* they have access to.
*   IAM uses **users, groups, roles, and policies** to provide authentication and authorization to AWS resources.```


# Section 3: Authenticating with IAM

This section describes using IAM for authentication.

## IAM Roles

An **IAM role** is an IAM identity that provides the ability to define a specific set of permissions to access the resources that a user or service needs. An IAM role is similar to an IAM user in that you can attach permissions policies to it. However, you don't attach the permissions to a specific IAM user or group. Instead, you attach the permissions to a role, and the user or the service **assumes** the role as needed.

When a user assumes a role, the user's prior permissions are temporarily forgotten. AWS returns **temporary security credentials** that the user or application can then use to make programmatic requests to AWS for the duration of the session. The **AWS Security Token Service (AWS STS)** issues the temporary security credentials.

When you use IAM roles, you don’t need to grant long-term security credentials to each entity that requires access to a resource.

For a service like Amazon EC2, applications or AWS services can programmatically assume a role at runtime. The principal that assumes the role might be an IAM user, group, or role from another AWS account, including accounts that you do not own.

### Example: EC2 Instance Assuming a Role
The following example illustrates a situation when you might need to use temporary security credentials to provide a role for an EC2 instance:
1.  An administrator creates the `Get-pics` role in IAM, defines a policy that grants access to an Amazon Simple Storage Service (Amazon S3) bucket named `photos`, and attaches the policy to the role.
2.  An EC2 instance assumes the `Get-pics` role.
3.  The EC2 instance is granted temporary permissions to access the S3 bucket named `photos`.

## Types of Security Credentials

When you interact with AWS by using the console, AWS CLI, or AWS SDKs, you must provide AWS credentials.

Two primary types of credentials are used for authentication. Which credential type you use depends on how you access AWS:
*   **To authenticate from the console,** you must sign in with your user name and password.
*   **To authenticate programmatically** through the AWS CLI, SDKs, and APIs, you must provide an AWS access key. The AWS access key is the combination of an access key ID and a secret access key.

The situation will dictate the authentication method. In most circumstances, a human user will use the AWS CLI method, whereas an automated program will use the SDK or API method. You might also be required to provide additional security information. For example, AWS recommends that you use MFA to increase the security of your account.

> For more information, see [AWS Security Credentials](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html) in the AWS General Reference Guide.

## Multi-Factor Authentication (MFA)

Multi-factor authentication (MFA) is a best practice that adds an extra layer of protection on top of your user name and password. MFA requires two or more factors to achieve authentication. Factors include the following:
*   **Something you know,** such as a password
*   **Something you have,** such as a cryptographic token
*   **Something you are,** such as your fingerprint

By default, MFA is not activated. When MFA is enabled and a user signs in to the AWS Management Console, they are prompted for their user name and password (the first factor—what they know). They are then prompted for an authentication response from their AWS MFA device (the second factor—what they have).

Examples of MFA devices include security key devices, such as YubiKey and Gemalto devices, and virtual devices, such as Google Authenticator or Authy.

## Scenario: Cross-Account Role Access

In this scenario, your organization has created multiple AWS accounts to isolate your production environment from your development environment. After testing an update within the development environment, you need to grant members of the `Developers` group temporary access to update the `productionapp` S3 bucket.

You take the following steps to grant this temporary access:
1.  An administrator within the **production account** creates a role (`UpdateApp`) that grants the development account read/write access to the production resources. To do this, you need to define a **trust policy** that specifies the development account as a principal. This will authorize users from the development account to assume the role.
2.  Within the **development account**, an administrator grants members of the `Developers` group permissions to assume the `UpdateApp` role. Because this is a role, the Developers will be granted permissions to call AWS STS to provide a temporary security token.
3.  The user requests to assume the `UpdateApp` role through the console, API, or AWS CLI.
4.  Upon receiving the role request, AWS STS returns temporary credentials to the requestor after verification is complete.
5.  By using the temporary credentials that AWS STS provided, the user from the `Developers` group is permitted to update the `productionapp` bucket in the production account.

---

## Key Takeaways
*   A best practice is to attach IAM policies to IAM groups, and then assign IAM users to these IAM groups.
*   IAM roles use temporary security credentials.
*   AWS STS provides temporary credentials for roles.
*   MFA adds an extra layer of protection on top of your user name and password.


# Section 4: Authorizing with IAM

This section describes using IAM for authorization.

## The Principle of Least Privilege

When you grant permissions to users, groups, roles, and resources, follow the standard security advice of strong authentication and the **principle of least privilege**. This practice means that you grant only the permissions that are needed to perform a task.

It’s more secure to start with a minimum set of permissions and grant additional permissions as needed. This provides better security than starting with permissions that are too permissive and then trying to restrict them later. To define the correct set of permissions, you must do some research to determine what access is needed to accomplish a specific task.

When you create IAM policies, determine what your users need to do, and then craft policies that allow them to perform *only* those tasks. Similarly, create policies for individual resources that identify precisely who is allowed to access the resource, and allow only the minimal permissions for those users. For example, perhaps developers should be allowed to create EC2 instances in production environments but not to stop or terminate the instances.

## How IAM Policies Work

You can control access to your AWS resources by using IAM policies that grant or deny permissions.

> **Example:**
> In the diagram, users in an IAM group have different permissions for two S3 buckets based on the policies applied.
> *   They are assigned **full access** to S3 Bucket 1 (read, write, delete).
> *   They are assigned **read-only access** to S3 Bucket 2.

By default, an authenticated IAM user, group, or role can't access anything in your account until you grant them permissions. You grant permissions by creating a **policy**. Policies are objects that define permissions for the identity or resource they're associated with. Most policies are defined and stored in a JavaScript Object Notation (JSON) document.

Policies define the `effect`, `actions`, `resources`, and optional `conditions` under which an entity can invoke API operations. By default, any actions or resources that are not explicitly allowed are denied. When an IAM principal (user or role) makes a request, AWS evaluates that request based on the permissions in these policies to determine whether access will be allowed or denied.

### Types of Policies
AWS currently supports six types of policies:
*   **Identity-based policies:** These policies are attached to IAM identities (users, groups, or roles) and grant permissions to the identity.
*   **Resource-based policies:** These are attached to resources (like an S3 bucket) and grant permissions to the principal specified in the policy.
*   **Permissions boundaries:** These define the *maximum* permissions that the identity-based policies can grant to an entity. Permissions boundaries do not grant permissions.
*   **AWS Organizations service control policy (SCP):** This defines the *maximum* permissions for account members of an organization or organizational unit (OU). SCPs limit permissions but cannot grant them.
*   **Access control lists (ACLs):** You can use an ACL to control which principals in *other accounts* can access the resource to which the ACL is attached. ACLs are the only policy type that doesn't use the JSON document format.
*   **Session policies:** These are used with the AWS CLI or AWS API to create a temporary session for a role or federated user. Session policies limit the permissions for a session but cannot grant them.

> For more information, see [Policies and Permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the AWS Identity and Access Management User Guide.

### Identity-Based vs. Resource-Based Policies
The two types of IAM policies that can grant permissions are identity-based policies and resource-based policies. The difference between them is where they are applied.

*   **Identity-based policies** are attached to an IAM user, group, or role. They indicate what that identity can do. For example, you could grant a user the ability to access an Amazon DynamoDB table.
*   **Resource-based policies** are attached to a resource. They indicate what a specified user (or group of users) is permitted to do with that resource. For example, you can use resource-based policies to grant access to an S3 bucket or to grant cross-account access between two trusted AWS accounts.

> For more information, see [Identity-Based Policies and Resource-Based Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_identity-vs-resource.html) in the AWS Identity and Access Management User Guide.

### Managed vs. Inline Policies
IAM policies can be categorized as either managed or inline.
*   **Managed Policies** are standalone, identity-based policies that you can attach to multiple users, groups, and roles.
    *   **AWS managed policies** are created and managed by AWS.
    *   **Customer managed policies** are created and managed by you.
    *   **Features:** Reusability, central change management, versioning and rollback, and the ability to delegate permissions management. Managed policies also enable the use of permissions boundaries.

*   **Inline Policies** are embedded directly into a single principal entity (a user, group, or role). The policy is an inherent part of that entity.
    *   Each entity has its own copy of the policy, which makes central management impossible.
    *   Inline policies are useful when you want to maintain a strict one-to-one relationship between a policy and the principal. This is because inline policies cannot be inadvertently attached to the wrong entity.

In most cases, **AWS recommends the use of managed policies instead of inline policies.**

> For more information, see [Managed Policies and Inline Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html) in the AWS Identity and Access Management User Guide.

## Policy Evaluation Logic
This diagram shows the logic that AWS uses when evaluating IAM policies. AWS evaluates all applicable policies and goes through this evaluation logic:

1.  **By default, all requests are implicitly denied.**
2.  AWS checks for an **explicit allow** in any applicable policy. If one exists, it overrides the default deny.
3.  AWS checks for an **explicit deny** in any applicable policy. If an explicit deny exists, it overrides any explicit allow.

> **The most restrictive policy wins.** An explicit deny always overrides any explicit allow.

The order that the policies are evaluated in has no effect on the outcome of the evaluation. All policies are evaluated, and the result is ​always​ that the request is either allowed or denied.

> For additional information, see [Testing IAM Policies with the IAM Policy Simulator](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html) in the AWS Identity and Access Management User Guide.

---

## Key Takeaways
*   Permissions to access AWS account services and resources are defined in IAM policy documents.
*   Attach IAM policies to IAM users, IAM groups, or IAM roles.
*   Follow the principle of least privilege when you grant account access.
*   When IAM determines permissions, an **explicit deny** will always override any **allow** statement.```


# Examples of Authorizing with IAM

### IAM Policy Example: Allow and Deny Statements

To understand how IAM policy logic works, consider this example policy which is broken into two parts.

#### Part 1: The `Allow` Statement

> **Accessibility Description:** Code snippet of an `Allow` statement portion of an IAM policy. The highlighted portion shows a configuration that allows an entity to perform any DynamoDB or S3 action on a specific DynamoDB table and two S3 buckets.

The first half of this policy gives users access to *only* the following resources:
*   DynamoDB table named `coursenotes`
*   S3 bucket named `course-notes-web`
*   S3 bucket named `course-notes-mp3` and all the objects that it contains

#### Part 2: The `Deny` Statement

> **Accessibility Description:** Code snippet of a `Deny` statement portion of an IAM policy. The highlighted portion shows a configuration that denies an entity to perform any DynamoDB or S3 action on any resource other than the specific DynamoDB table and S3 buckets specified in the allow section.

The second half of this policy is an **explicit deny**. This explicit deny ensures that the entity can’t perform any action on any other DynamoDB table or S3 bucket, *except for* the specific ones listed in the `Allow` statement.

Even if permissions are granted in another policy, the explicit deny overrides those permissions. **An explicit deny statement takes precedence over an allow statement.** In its totality, this IAM policy allows access to specific DynamoDB and Amazon S3 resources and then explicitly denies access to all other DynamoDB or S3 resources in the account.

### Permissions Boundary Example

A **permissions boundary** is an advanced feature for using a managed policy to set the *maximum* permissions that an identity-based policy can grant to an IAM entity. An entity's permissions boundary allows it to perform only the actions that are allowed by *both* its identity-based policies *and* its permissions boundaries.

> **Accessibility Description:** Code snippets of a permissions boundary. The first snippet allows three types of actions to be completed, on in three specific areas S3, CloudWatch, and EC2. The second snippet allows only the `IAM:CreateUser` action on any resource.

Consider this scenario:
1.  **Policy 1 (The Permissions Boundary)** is attached to `ExampleUser`. This policy restricts the user to managing only Amazon S3, Amazon CloudWatch, and Amazon EC2 resources. It denies access to all other services, including IAM.
2.  **Policy 2 (Identity-Based Policy)** is then attached to `ExampleUser`. This policy allows the `iam:CreateUser` action.

If `ExampleUser` attempts to perform the `iam:CreateUser` operation, the operation **fails**. This is because the permissions boundary (Policy 1) does not allow any IAM actions. To make it succeed, the boundary policy would need to be modified to allow management of the IAM service.

> For more information, see [Permissions Boundaries for IAM Entities](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html) in the AWS Identity and Access Management User Guide.

---

# Section 5: Additional Authentication and Access Management Services

This section covers additional authentication and access management services.

### Identity Federation
**Identity federation** is a system of trust between two parties to authenticate users and convey information needed to authorize access to resources.
*   **Identity providers (IdPs)** are responsible for user authentication.
*   **Service providers (SPs)**, such as services or applications, are responsible for controlling access to resources.

Through administrative agreement and configuration, the SP trusts the IdP to authenticate users and grants them access.

Two AWS services are available to provide federation:
*   **AWS Single Sign-On (AWS SSO):** A great option if you are using a single centralized directory.
*   **IAM:** Consider using IAM if you are using multiple directories within your organization, or if you wish to use attribute-based permissions.

> For more information, see [Identity Federation in AWS](https://aws.amazon.com/identity/federation).

### AWS Single Sign-On (AWS SSO)
With AWS Single Sign-On (AWS SSO), you can create or connect identities once in AWS and centrally manage access across your AWS accounts. AWS SSO provides a unified administration experience to define, customize, and assign fine-grained permissions based on common job functions.

Users in your AWS SSO environment can use their directory credentials to access their user portal. From there, they can access all their assigned AWS accounts or cloud applications. AWS SSO supports commonly used cloud applications such as Microsoft 365 and Salesforce.

> For more information, see [What is AWS Single Sign-On?](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html) in the AWS Single Sign-On User Guide.

### AWS Directory Service for Microsoft Active Directory
Also commonly referred to as **AWS Managed Microsoft AD**, this service makes it possible for directory-aware workloads and AWS resources to use managed Active Directory in the AWS Cloud. The service is built on Microsoft Active Directory, which eliminates the requirement to synchronize or replicate your existing Active Directory data to the cloud. With this service, you can use your existing on-premises user credentials to access cloud resources, which simplifies the process to extend your existing Active Directory to AWS.

> For more information, see [AWS Directory Service: Managed Microsoft Active Directory in AWS](https://aws.amazon.com/directoryservice).

### Amazon Cognito
Amazon Cognito is a service that provides **authentication, authorization, and user management** (sign-up, sign-in, and access control) for your mobile and web applications. The service provides a secure identity store, which can help you to scale effectively for millions of users.

Amazon Cognito supports direct sign-in through user name and password, but it also supports third-party authentication through social identity providers such as Apple, Google, Facebook, and Amazon.

Amazon Cognito relies on two main components:
*   **User pools** are directories that provide sign-up and sign-in options for your application users.
*   **Identity pools** enable you to grant your users access to other AWS services through temporary, limited-privilege AWS credentials.

> For more information, see [What Is Amazon Cognito?](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html) in the Amazon Cognito Developer Guide.
>

# Section 6: Using AWS Organizations

This section describes the AWS Organizations service.

## What is AWS Organizations?

**AWS Organizations** is an account management service that you can use to create an organization where you can consolidate multiple accounts and centrally manage them.

Key features of AWS Organizations include:
*   **Centralized Management:** Provides centralized account creation and management, as well as consolidated billing capabilities. With these features, you can manage your security, compliance, and budgetary needs more efficiently.
*   **Hierarchical Grouping:** Provides the ability to hierarchically group your accounts in **organizational units (OUs)** and attach different access policies to each. You can nest OUs within other OUs up to a depth of five levels.
*   **Service Control Policies (SCPs):** A key feature of Organizations is the use of SCPs to specify the *maximum permissions* for member accounts in your organization. This helps you ensure that your accounts stay within your organization's access control guidelines.

SCPs **cannot be used to grant any permissions.** They can only restrict access to a service, resource, or API action for any member account, user, or role. Any restrictions put in place by an SCP will remain in place even if the restricted actions are implicitly allowed elsewhere.

Organizations builds upon IAM by expanding the granular control that IAM provides to the account level. It does this by giving you control over what users and roles in an account or a group of accounts can do. This additional layer of control ensures that users can access only what *both* Organizations policies *and* IAM policies allow. If either service blocks an operation, the user will not be able to access that operation.

> For more information, see [What is AWS Organizations?](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html) in the AWS Organizations User Guide.

### Service Control Policy (SCP) Example

This SCP example prevents member accounts from leaving the organization. The `Effect` of the policy statement is to explicitly `Deny` the `organizations:LeaveOrganization` action, which prevents member accounts from leaving.

> For more information, see [Service Control Policies (SCPs)](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html) in the AWS Organizations User Guide.

---

# Lab: Using Resource-Based Policies to Secure an S3 Bucket

You will now complete the "Using Resource-Based Policies to Secure an S3 Bucket" lab.

## Lab Tasks
In this lab, you will complete the following tasks:
1.  Accessing the console as an IAM user
2.  Attempting read-level access to AWS services
3.  Analyzing the identity-based policy applied to the IAM user
4.  Attempting write-level access to AWS services
5.  Assuming an IAM role and reviewing a resource-based policy
6.  Understanding resource-based policies

After you complete the lab, your educator might choose to lead a conversation about the key takeaways from the lab.

---

# Module Review

It’s now time to review the module, and wrap up with a knowledge check and discussion of a practice certification exam question.

## Learning Objectives Summary
In this module you learned how to do the following:
*   Authorize access to AWS services by using IAM users, groups, and roles.
*   Differentiate between different types of security credentials in IAM.
*   Authorize access to AWS services by using identity-based and resource-based policies.
*   Identify other AWS services that provide authentication and access management services.
*   Centrally manage and enforce policies for multiple AWS accounts.
