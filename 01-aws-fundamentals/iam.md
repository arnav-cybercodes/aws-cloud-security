# AWS Identity and Access Management (IAM)

## 1. Introduction

AWS Identity and Access Management (IAM) is a service that controls who can access AWS resources and what actions they are allowed to perform.

In simple terms, IAM answers two important questions:

1. Who are you?
2. What are you allowed to do?

For example, a developer might be allowed to upload files to an S3 bucket but not delete EC2 instances.

IAM helps enforce these permissions.

**Why is IAM important?**

A compromised IAM identity can allow an attacker to access sensitive data, modify cloud infrastructure, create additional users or escalate privileges.

---

## 2. Core IAM Components

AWS IAM has four important identity types.

### A. IAM Users

An IAM user represents an individual person or application that needs access to an AWS account.

An IAM user can have:
- Console password for AWS Management Console access.
- Access keys for programmatic access through AWS CLI or APIs.
- Permissions through attached IAM policies.

Example:

A company has three employees:
- Alice – Developer
- Bob – Security Analyst
- Charlie – Administrator

Each could have an individual IAM user with different permissions.

### B. IAM Groups

An IAM group is a collection of IAM users.

Instead of assigning the same permissions individually to every employee, you can assign permissions to a group.

Example:

**Developers group**
- Alice
- Bob

The group can have permissions to access development resources.

Important: IAM groups contain users, not roles or other groups.

### C. IAM Roles

An IAM role is an identity that provides temporary permissions to a person, application or AWS service when it assumes that role.

Unlike a typical IAM user, a role does not have its own permanent username and password or permanent access keys.

Example:

An EC2 instance needs to upload reports to an S3 bucket.

Instead of storing permanent AWS credentials inside the EC2 instance, we attach an IAM role that grants the required S3 permissions.

AWS provides temporary credentials when the instance uses that role.

This is generally safer than storing permanent access keys in application code.

### D. Root User

The root user is the original identity created when an AWS account is registered.

It has complete access to the AWS account.

Best practices:
- Enable MFA.
- Do not use the root user for everyday activities.
- Do not create root access keys.
- Use a separate administrative identity for routine administration.

---

## 3. Authentication vs Authorization

These two concepts are important in IAM.

### Authentication

Authentication verifies your identity.

Example:
You enter your AWS username and password and complete MFA verification.

AWS verifies that you are the person you claim to be.

### Authorization

Authorization determines what you are allowed to do.

Example:
You successfully log in to AWS, but your IAM permissions only allow you to view S3 buckets.

You cannot delete an EC2 instance because you do not have the required permission.

**Remember:**
- Authentication = Who are you?
- Authorization = What can you do?

---

## 4. IAM Policies

IAM policies are JSON documents that define permissions.

They specify:
- What actions are allowed or denied.
- Which AWS resources those actions apply to.
- Under which conditions the permissions apply.

Example IAM policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::company-reports/*"
    }
  ]
}
```

### Understanding the policy

**Version:** Specifies the policy language version.

**Effect:** Defines whether the action is allowed or denied.

**Action:** Specifies the AWS operation.

**Resource:** Specifies which resource the policy applies to.

**Condition:** An optional field that specifies additional requirements for permission.

### Common IAM policy actions

| Action | Meaning |
|---|---|
| s3:GetObject | Read an S3 object |
| s3:PutObject | Upload an S3 object |
| s3:DeleteObject | Delete an S3 object |
| ec2:StartInstances | Start an EC2 instance |
| ec2:StopInstances | Stop an EC2 instance |
| iam:CreateUser | Create an IAM user |
| iam:AttachUserPolicy | Attach a policy to a user |

### Allow vs Deny

- Allow grants permission when applicable.
- Deny explicitly blocks permission.

An explicit Deny overrides an Allow.

If no applicable policy allows an action, it is generally denied by default. This is called **implicit deny**.

---

## 5. Types of IAM Policies

### AWS Managed Policies

Policies created and maintained by AWS.

Example:
`AdministratorAccess`

### Customer Managed Policies

Policies created and maintained by the customer.

They can be reused across multiple IAM identities.

### Inline Policies

Policies directly embedded into an individual IAM identity or resource.

They have a one-to-one relationship with the identity or resource they are attached to.

### Identity-Based vs Resource-Based Policies

**Identity-based policy:** Attached to an identity, such as an IAM user or role.

**Resource-based policy:** Attached to a resource, such as an S3 bucket, and specifies which principals can access it.

Example:
An S3 bucket policy can allow a particular IAM role to read objects from the bucket.

---

## 6. IAM Role Trust Policy

A trust policy defines who is allowed to assume an IAM role.

It is different from a permissions policy.

Example:

An EC2 instance needs to assume an IAM role.

The trust policy establishes that the EC2 service is allowed to assume that role.

Think of it this way:

- Trust policy = Who can use the role?
- Permissions policy = What can the role do?

Both are important for securing IAM roles.

---

## 7. Temporary Credentials and AWS STS

AWS Security Token Service (STS) provides temporary security credentials.

These credentials generally contain:

- Access Key ID
- Secret Access Key
- Session Token

Example:

An EC2 instance assumes an IAM role and receives temporary credentials.

The application uses these credentials to access S3.

Unlike permanent IAM user access keys, temporary credentials expire.

**Why is this useful?**

If temporary credentials are exposed, they do not remain valid forever. However, an attacker may still misuse them before they expire.

---

## 8. Principle of Least Privilege

The principle of least privilege means giving an identity only the permissions it needs to perform its job.

Example:

An EC2 application needs to read files from one S3 bucket.

Bad configuration:
- S3 Full Access.
- Access to every S3 bucket.

Better configuration:
- Only s3:GetObject.
- Only the required bucket and objects.

Why is this important?

If an attacker compromises the application and steals its credentials, the attacker is restricted by the permissions of that identity.

Least privilege limits the potential damage.

---

## 9. Common IAM Security Misconfigurations

### 1. Overprivileged Users

Giving a normal developer AdministratorAccess when they only need access to a few services.

### 2. Exposed Access Keys

Storing AWS access keys in public GitHub repositories, application code or unsecured configuration files.

### 3. Weak Authentication

Not enabling MFA for important accounts and identities.

### 4. Insecure Trust Policies

Allowing unintended external accounts or principals to assume sensitive IAM roles.

### 5. Excessive IAM Role Permissions

Giving an EC2 instance full S3 access when it only needs to read from one bucket.

### 6. Unused Credentials

Leaving unused access keys active for long periods.

---

## 10. How Attackers Abuse IAM

IAM is an important target during cloud attacks.

Common attack scenarios include:

**Credential Theft**

An attacker obtains an IAM user's access keys and uses them to make AWS API requests.

**Privilege Escalation**

An attacker with limited permissions discovers an IAM configuration weakness that allows them to gain more privileges.

**Role Abuse**

An attacker finds an overly permissive IAM role and uses it to access resources they should not normally access.

**Persistence**

An attacker creates a new IAM user, access key or other identity-based access mechanism to maintain access to a compromised account.

**Data Exfiltration**

An attacker uses compromised permissions to read sensitive S3 objects and transfer the data outside the environment.

---

## 11. IAM Security Best Practices

1. Apply least privilege to users, groups and roles.
2. Enable MFA for privileged identities.
3. Prefer IAM roles and temporary credentials over long-term access keys.
4. Never upload access keys or secret keys to GitHub.
5. Review IAM policies regularly.
6. Remove unused credentials and identities.
7. Restrict cross-account role access through trust policies.
8. Monitor suspicious IAM API activity using CloudTrail.
9. Use permission boundaries and AWS Organizations SCPs where appropriate.
10. Protect the root user.

---

## 12. Practical Investigation: What Should a Security Analyst Look For?

Suppose an attacker compromises an IAM user's credentials.

A security analyst should investigate:

| Evidence | What to look for |
|---|---|
| CloudTrail | Unusual AWS API calls |
| IAM users | Unexpected new users |
| Access keys | Unexpected key creation |
| IAM policies | Unexpected policy attachments or permission changes |
| STS | Unusual AssumeRole activity |
| Source IP | Unexpected login or API request locations |

Some important CloudTrail event names include:

- CreateUser
- CreateAccessKey
- AttachUserPolicy
- PutUserPolicy
- CreatePolicyVersion
- AssumeRole

These events can help identify suspicious IAM activity.

Note: Not every event is malicious. Investigate the identity, timing, source, context and expected business activity before drawing conclusions.

---

## 13. Key Takeaways

- IAM controls access to AWS resources.
- Users represent individual identities.
- Groups help manage permissions for multiple users.
- Roles provide temporary permissions to trusted identities and services.
- Policies define permissions.
- Trust policies determine who can assume a role.
- STS provides temporary credentials.
- Least privilege reduces the impact of compromised identities.
- CloudTrail helps investigate IAM activity.

**Final takeaway:**

IAM is one of the most important parts of AWS security. A poorly configured identity can become an attacker's entry point into an entire cloud environment.
