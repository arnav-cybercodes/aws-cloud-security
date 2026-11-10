# Chapter 1: Cloud Security Fundamentals

## 1. Shared Responsibility Model

The Shared Responsibility Model defines the division of security responsibilities between a cloud service provider (CSP) and its customer.

The cloud provider is responsible for securing the underlying cloud infrastructure, while the customer is responsible for securing the resources, configurations, applications and data they use in the cloud.

### Cloud Provider Responsibilities

- Physical data centre security.
- Physical servers and networking infrastructure.
- Hypervisor and virtualization infrastructure.
- Infrastructure-level isolation between customers.
- Security of the underlying cloud platform.

### Customer Responsibilities

- Operating system security and patching (IaaS).
- Application security.
- Identity and Access Management (IAM).
- Data protection and encryption configuration.
- Network access configurations.
- Security monitoring and incident response.

### Example: AWS EC2

AWS manages the physical server and hypervisor.

The customer manages the guest operating system, installed applications, user accounts, security configurations and data.

**Important principle:** The cloud provider secures the cloud, while the customer secures what they put in the cloud.

---

## 2. Cloud Delivery Models

Cloud services are commonly categorized into three delivery models.

### IaaS – Infrastructure as a Service

Provides virtualized computing infrastructure, including servers, storage and networking.

Example: AWS EC2.

Customer responsibilities:
- Guest operating system.
- Applications.
- Data.
- IAM and security configurations.
- Virtual network configuration.

Provider responsibilities:
- Physical infrastructure.
- Hypervisor.
- Underlying networking and storage infrastructure.

### PaaS – Platform as a Service

Provides a managed platform for developing and deploying applications.

Example: AWS Elastic Beanstalk.

The provider manages more of the underlying infrastructure and platform, while the customer remains responsible for application code, data, identities and relevant configurations.

### SaaS – Software as a Service

Provides ready-to-use software over the internet.

Examples: Gmail and Microsoft 365.

The provider manages the application infrastructure and software. Customers remain responsible for appropriate account security, access permissions, data handling and user configurations.

**Key takeaway:** As we move from IaaS to PaaS to SaaS, the provider generally takes on more responsibility. Exact responsibilities depend on the service.

---

## 3. Network Security

Cloud network security involves protecting communication between systems, applications and users.

Responsibilities may be divided between the provider and customer.

Customer-managed network security controls can include:

- Virtual Private Clouds (VPCs).
- Public and private subnets.
- Security Groups.
- Network Access Control Lists (NACLs).
- Firewalls.
- Routing configurations.
- Transport encryption.

### Example

An EC2 instance has SSH (port 22) exposed to the entire internet.

This increases the attack surface and may allow unauthorized login attempts.

A better configuration restricts SSH access to trusted IP addresses or uses more secure administrative access mechanisms.

---

## 4. Operating System Security

In IaaS environments, customers are generally responsible for securing their guest operating systems.

Important activities include:

- Installing security patches.
- Removing unnecessary services.
- Configuring host firewalls.
- Managing user accounts.
- Applying least privilege.
- Installing endpoint protection.
- Monitoring system activity.

Example: If an EC2 Windows Server instance has an unpatched vulnerability, patching the guest operating system is generally the customer's responsibility.

---

## 5. Middleware Security

Middleware refers to software operating between the operating system and the application.

Examples:
- Database systems.
- Application servers.
- Message queues.
- Web servers.

Middleware security includes:
- Secure configuration.
- Access control.
- Software updates.
- Encryption.
- Authentication.
- Monitoring.

In PaaS and managed services, some middleware responsibilities may be shared with the provider.

---

## 6. Application Security

Application security focuses on protecting the software that users interact with.

Common vulnerabilities include:

- SQL Injection.
- Cross-Site Scripting (XSS).
- Broken authentication.
- Broken access control.
- Insecure input handling.

Even if the underlying cloud infrastructure is secure, vulnerable application code can expose sensitive information.

---

## 7. Data Access Security

Data access security ensures that only authorized users and applications can access information.

Important controls:
- IAM policies.
- Role-based access control (RBAC).
- Least privilege.
- Encryption.
- Access monitoring.
- Secure storage permissions.

Example: An S3 bucket containing customer records should not be publicly accessible unless public access is explicitly intended and appropriate.

---

## 8. Risk Management

Risk management is the process of identifying, assessing, prioritizing and treating risks.

Risk is commonly evaluated using two factors:

- **Likelihood:** Probability of an incident occurring.
- **Impact:** Potential damage caused by the incident.

A simplified risk model:

Risk ≈ Likelihood × Impact

### Known and Unknown Risks

**Known risks:** Risks whose nature and potential impact are understood.

Example: An EC2 instance is running an outdated operating system with a known vulnerability.

**Unknown risks:** Risks where there is insufficient information to estimate likelihood or impact.

### Four Risk Treatment Strategies

#### 1. Avoid

Eliminate the activity that creates the risk.

Example: Remove an unnecessary vulnerable application.

#### 2. Mitigate

Reduce the likelihood or impact of the risk.

Example: Restrict SSH access, enable MFA and apply security patches.

#### 3. Transfer

Transfer some financial consequences or responsibilities to another party.

Examples:
- Cyber insurance.
- Using a cloud provider for physical infrastructure management.

Transferring risk does not eliminate all customer responsibilities.

#### 4. Accept

Consciously accept a risk after evaluating its likelihood, impact and potential consequences.

Risk decisions should be documented and approved by appropriate stakeholders.

---

## 9. Least Privilege

The Principle of Least Privilege means providing users, applications and systems with only the permissions required to perform their tasks.

Example:

An application only needs read access to one S3 bucket.

It should not receive full AWS administrator permissions.

Benefits:
- Limits unauthorized access.
- Reduces attack surface.
- Limits the impact of compromised accounts.
- Prevents unnecessary privilege escalation opportunities.

Least privilege applies to:
- IAM users and roles.
- Linux and Windows accounts.
- Applications.
- Database accounts.
- Automated scripts.

---

## 10. Defense in Depth

Defense in Depth is a security strategy that uses multiple complementary security controls.

The objective is to avoid relying on a single security mechanism.

Example cloud security layers:

1. IAM and MFA.
2. Network security.
3. Operating system hardening.
4. Endpoint Detection and Response (EDR).
5. Encryption.
6. SIEM and monitoring.
7. Backups and disaster recovery.

If one control fails, other controls may still prevent or detect an attack.

### Independent Security Controls

If two independent security controls are each 95% effective:

Probability of both failing:

0.05 × 0.05 = 0.0025

Combined effectiveness:

1 - 0.0025 = 99.75%

This is a simplified probability model. Real security controls may have overlapping weaknesses and may not be independent.

Adding more controls also has diminishing returns, so controls should be complementary rather than unnecessarily duplicated.

---

## 11. Threat Modeling

Threat modeling is the process of identifying potential attackers, understanding their objectives and identifying weaknesses they could exploit.

It helps organizations think about their systems from an attacker's perspective.

### Questions to Ask

1. Who might attack the system?
2. What information or resources might they target?
3. What attack paths could they use?
4. Which components are vulnerable?
5. What security controls can reduce the risks?

### Example

Consider an online application containing:

- Login page.
- Web server.
- Application server.
- Database.

| Component | Potential threat | Security control |
|---|---|---|
| Login page | Brute-force attacks | MFA and rate limiting |
| Application | SQL Injection | Parameterized queries |
| Database | Unauthorized access | Access restrictions |
| Administrator dashboard | Account compromise | MFA and least privilege |
| Network | Unauthorized connections | Firewalls and segmentation |

### Trust Boundaries

A trust boundary is a point where data or control passes between components with different levels of trust.

Examples:
- Internet to application.
- Public subnet to private subnet.
- Application server to database.
- Customer environment to cloud provider infrastructure.

Security controls should be implemented at important trust boundaries.

---

## 12. Important Security Principles

### Shared Responsibility
Understand which security responsibilities belong to the cloud provider and which belong to the customer.

### Least Privilege
Provide only the permissions necessary to perform a task.

### Defense in Depth
Use multiple complementary security controls.

### Threat Modeling
Identify attackers, attack paths and system weaknesses before incidents occur.

### Risk Management
Prioritize security risks based on likelihood and impact.

---

## 13. Practical Relevance to Cloud Security and DFIR

These concepts provide the foundation for cloud security operations and incident response.

During a cloud security investigation, an analyst may need to:

- Identify which party was responsible for the affected resource.
- Understand the cloud architecture.
- Identify trust boundaries.
- Determine the attack path.
- Assess the impact.
- Examine IAM permissions.
- Identify failed or missing security controls.
- Recommend mitigation strategies.

### Key Takeaway

Cloud security is not simply about securing virtual machines. It requires understanding infrastructure, identities, networks, applications, data, responsibilities and risks.

**Chapter 1 establishes the fundamental principles required to design, deploy and secure cloud environments.**
