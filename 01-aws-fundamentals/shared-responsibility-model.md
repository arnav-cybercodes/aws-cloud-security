# AWS Shared Responsibility Model

## 1. Introduction

The AWS Shared Responsibility Model defines the security responsibilities of AWS and its customers.

- **Security OF the Cloud:** AWS is responsible for securing the underlying cloud infrastructure.
- **Security IN the Cloud:** Customers are responsible for securing their data, identities, applications and service configurations.

## 2. Responsibilities by Service

### EC2
AWS is responsible for:
- Physical servers and data centres.
- Hypervisor and underlying infrastructure.

Customer is responsible for:
- Operating system updates and patches.
- Security Groups and firewall configuration.
- IAM roles and application security.

### RDS
AWS is responsible for:
- Underlying infrastructure.
- Managed database engine maintenance in supported configurations.

Customer is responsible for:
- Database access permissions.
- Encryption configuration.
- Network access and public exposure.

### Lambda
AWS is responsible for:
- Underlying infrastructure.
- Runtime execution environment management.

Customer is responsible for:
- Secure application code.
- IAM permissions.
- Secrets and logging.

### S3
AWS is responsible for:
- Physical storage infrastructure.
- Underlying hardware security.

Customer is responsible for:
- Bucket policies and access controls.
- Encryption settings.
- Public access configuration.
- Data lifecycle management.

## 3. Common Security Risks

- Overprivileged IAM roles.
- Publicly exposed S3 buckets.
- Insecure Security Group rules.
- Weak authentication.
- Misconfigured cloud services.

## 4. Example Attack Scenario

An attacker exploits an SSRF vulnerability in an internet-facing application hosted on EC2.

1. The attacker tricks the application into requesting EC2 instance metadata.
2. The metadata service exposes temporary IAM role credentials.
3. The attacker obtains those credentials.
4. The attacker uses them to make AWS API requests.
5. The attacker accesses S3 data permitted by the compromised role.

This demonstrates how insecure customer configurations can create security incidents even when AWS infrastructure is operating correctly.

## 5. Security Best Practices

- Apply the principle of least privilege.
- Use IAM roles instead of permanent access keys where possible.
- Enforce IMDSv2 on EC2 instances.
- Restrict public access to S3 buckets.
- Enable appropriate CloudTrail logging.
- Monitor suspicious API activity.
- Apply preventive security policies and configuration guardrails.

## 6. Key Takeaway

AWS secures the underlying cloud infrastructure, while customers must properly configure and secure the services they use.

Understanding this division of responsibility is essential for AWS administration and cloud security.
