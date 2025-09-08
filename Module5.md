# Module 5: Protecting Data in Your Application


## Module Introduction:
Welcome to the **Protecting Data in Your Application** module of the AWS Academy Cloud Security Foundations course. This module introduces key concepts and practices for securing data in cloud-based applications.

---

### Objectives:
By the end of this module, you should be able to:

- 🔐 Describe how to **protect data at rest and in transit**.
- 🛡️ Identify **Amazon S3 protection features**.
- 🔒 Encrypt data in **Amazon S3**.
- 🔄 Differentiate between **client-side encryption (CSE)** and **server-side encryption (SSE)**.
- 🧰 Identify **AWS services** that help protect your data.

---

### Module Sections:
- **Protect data at rest**
- **Amazon S3 protection features**
- **Protection through encryption**
- **Protect data in transit**
- **Best practices to protect data in Amazon S3**
- **Additional data protection services**

> 💡 Includes a hands-on **lab**:  
> Practice implementing encryption of data at rest using **AWS KMS**.

> 🧠 Includes a **knowledge check** to test your understanding of key concepts.

---

### Scenario Context:
This module is framed around a **bank business scenario**:

- María presents a security plan to John, who is supportive but uncertain about AWS’s security capabilities.
- John shares a past experience where a **disgruntled employee** accessed unsecured user data, leading to financial losses.
- María aims to demonstrate how **AWS can secure user data** in the cloud to reassure John.

---

### Shared Responsibility Model (Focus Areas):
This module covers two key areas of the **Shared Responsibility Model**:

#### Customer Responsibilities:
- Security **in** the cloud:
  - Customer data
  - Platform, applications, IAM
  - OS, network, firewall configuration
  - Client-side encryption & data integrity
  - Authentication
  - Server-side encryption
  - Network traffic protection (encryption, integrity, identity)

#### AWS Responsibilities:
- Security **of** the cloud:
  - AWS foundational services (compute, storage, databases, networking)
  - AWS Global Infrastructure (Regions, Availability Zones, Edge Locations)


## Section 2: Protecting Data at Rest

### Overview:
This section explains the importance of protecting **data at rest** and outlines common threats and mitigation strategies using AWS services, particularly Amazon S3.

---

### Why Protect Data at Rest?
Encrypting data at rest ensures that even if unauthorized access occurs, the data remains secure. This is crucial for:

- Preventing **data breaches**
- Meeting **business** and **compliance** requirements
- Reducing the impact of **endpoint compromises**

---

### Common Threats & Mitigations:

| **Threat**                     | **Mitigation Strategy**                                                                 |
|-------------------------------|------------------------------------------------------------------------------------------|
| **Information Disclosure**     | - Limit user access using policies<br>- Encrypt confidential data                       |
| **Data Integrity Compromise**  | - Use resource permissions<br>- Implement digital signatures and encryption<br>- Restore from backup or previous S3 object version |
| **Accidental/Malicious Deletion** | - Apply least privilege principle<br>- Restore from backup or previous S3 object version |
| **System/Hardware/Software Failure** | - Restore data from replicas in case of failure or disaster                         |

---

### Amazon S3 Security Defaults:
- All **S3 resources** (buckets, objects, subresources) are **private by default**.
- Only the **resource owner** (creating AWS account) has access.
- Access can be granted using **access policies**.

> 🔐 **S3 Block Public Access**: An additional layer to prevent accidental data exposure.

---

### Access Control Mechanisms in Amazon S3:

#### 1. **Identity-Based Policies**
- Attached to **IAM users**
- Define what actions a user is allowed to perform

#### 2. **Resource-Based Policies**
- Attached directly to **resources** (e.g., S3 buckets, VPC endpoints, KMS keys)
- Define what actions specific users/accounts can perform on the resource

> 🧠 **Example**:  
> - Bob’s IAM policy allows `GET`, `PUT`, and `LIST` on Bucket X  
> - Bucket X’s resource policy allows only `GET` and `LIST`  
> - Result: Bob **cannot PUT** objects into Bucket X

#### 3. **Cross-Account Access**
- Use **resource-based policies** to grant access to other AWS accounts without using roles

---

### Access Control Lists (ACLs):
- ACLs are **independent** of IAM policies
- Mostly **deprecated** in modern use cases
- AWS recommends **disabling ACLs** unless object-level control is required
- With ACLs disabled, **bucket policies** become the sole access control mechanism

---

### Key Takeaways:
- 🔐 **Encrypting data at rest** enhances security against unauthorized access
- 🛡️ **Amazon S3 is private by default** and requires credentials for access
- 🔧 S3 supports two main access control types:
  - **Identity-based**
  - **Resource-based**

## Section 3: Amazon S3 Protection Features
__

### This section covers Amazon S3 protection features. 

Amazon S3 provides a number of security features to consider as you develop and implement your own security policies. Amazon S3 provides the Block Public Access feature to help you manage public access to S3 resources. By default, new buckets and objects don't allow public access, but users can modify bucket policies or object permissions to allow public access. S3 Block Public Access provides settings that override these policies and permissions so that you can limit public access to these resources.

This feature provides four settings:  
• BlockPublicAcls: Prevent any new operations to make buckets or objects public through bucket or object ACLs. Existing policies and ACLs for buckets and objects are not modified.  
• IgnorePublicAcls: Ignore all public ACLs on a bucket and any objects that it contains.  
• BlockPublicPolicy: Reject calls to PUT a bucket policy if the specified bucket policy allows public access. (Enabling this setting doesn't affect existing bucket policies.)  
• RestrictPublicBuckets: Restrict access to a bucket with a public policy to only AWS services and authorized users within the bucket owner's account.

These settings are independent and can be used in any combination. You can apply each setting to an access point, a bucket, or an entire AWS account. You cannot apply these settings on a per-object basis. If the block public access settings for the access point, bucket, or account differ, then Amazon S3 applies the most restrictive combination of the access point, bucket, and account settings.

When Amazon S3 receives a request to access a bucket or an object, the service determines whether the bucket or the bucket owner's account has a block public access setting applied. If an existing block public access setting prohibits the requested access, Amazon S3 rejects the request.

In addition to these settings, the Amazon S3 console highlights your publicly accessible buckets, indicates the source of public accessibility, and also warns you if changes to your bucket policies or bucket ACLs would make your bucket publicly accessible.

For more information, see Blocking Public Access to Your Amazon S3 Storage in the Amazon Simple Storage Service User Guide.
---

Another security feature to consider is Versioning. It is a method of keeping multiple variants of an object in the same bucket. With this feature, you can preserve, retrieve, and restore every version of every object stored in your buckets. By default, S3 Versioning is disabled on buckets, and you must explicitly enable it.

Versioning can help you to recover objects from accidental deletion or overwrite. For example, if you delete an object, instead of removing it permanently, Amazon S3 inserts a delete marker, which becomes the current object version. You can always restore the previous version. Overwriting an object results in a new object version in the bucket, but you can always restore the previous version.

With versioning enabled, the most recently written version will be retrieved by default. You can retrieve older versions of an object by specifying the version in your request.

To customize your data retention approach and control storage costs, use object versioning with S3 Lifecycle.

After you enable versioning for a bucket, you can't return it to an unversioned state. You can, however, suspend versioning on a bucket, which stops accruing new versions of the same object in a bucket. You might do this because you only want a single version of an object in a bucket. Or, you might not want to accrue charges for multiple versions. When you suspend versioning, existing objects in your bucket do not change.

When working with S3 Versioning in Amazon S3 buckets, you can optionally add another layer of security by enabling multi-factor authentication (MFA) delete. When you do this, the bucket owner must include two forms of authentication in any request to delete a version or change the versioning state of the bucket.

For more information, see Using Versioning in S3 Buckets in the Amazon Simple Storage Service User Guide.
---

Finally, with Object Lock, you can store objects by using a write-once-read-many (WORM) model. This feature provides protection for scenarios where it's imperative that data is not changed or deleted. The feature can provide protection for a fixed amount of time or indefinitely. You can use Object Lock to meet regulatory requirements that require WORM storage, or add an extra layer of protection against object changes and deletion.

Note that Object Lock works only in versioned buckets.

Object Lock provides two ways to manage object retention: retention periods and legal holds.  
- A **retention period** specifies a fixed period during which an object remains locked. During this period, your object is WORM-protected and can't be overwritten or deleted.  
- A **legal hold** provides the same protection as a retention period but has no expiration date. Instead, a legal hold remains in place until you explicitly remove it.

You can configure Object Lock in one of two modes:  
- **Governance mode**: Users can't overwrite or delete an object version or alter its lock settings unless they have special permissions.  
- **Compliance mode**: A protected object version can't be overwritten or deleted by any user, including the root user in your AWS account.

For more information, see How S3 Object Lock Works in the Amazon Simple Storage Service User Guide.

---

### Key takeaways from this section of the module include the following:  
• Block Public Access ensures that objects never have public access, now and in the future.  
• Versioning preserves, retrieves, and restores every version of every object stored in an S3 bucket.  
• Object Lock prevents an object version from being deleted or overwritten for a fixed amount of time or indefinitely.

---
