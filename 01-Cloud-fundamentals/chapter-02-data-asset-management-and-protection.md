# Chapter 2: Data Asset Management and Protection

## 1. Introduction to Data Asset Management

Before protecting data in a cloud environment, an organization must understand:

- What data it owns.
- Where the data is stored.
- How sensitive the data is.
- Who can access it.
- How it should be protected.

Asset management can be divided into two categories:

### Data Assets

Data assets are the actual information an organization stores or processes.

Examples:
- Customer names and addresses.
- Credit card information.
- Bank account details.
- Employee records.
- Passwords and API keys.
- Business documents.
- Medical records.

### Cloud Assets

Cloud assets are the computing and storage resources used to store and process data.

Examples:
- EC2 instances.
- S3 buckets.
- EBS volumes.
- Databases.
- Containers.
- Message queues.

**Difference:** Data assets represent the information, while cloud assets represent the resources that store, process and manage that information.

---

## 2. Data Identification and Classification

Data identification involves discovering what information an organization possesses and where it is stored.

Data classification involves assigning sensitivity levels to information based on its value and the potential damage caused by unauthorized disclosure.

Before classifying data, organizations should identify:
- Customer information.
- Employee records.
- Financial records.
- Credentials and API keys.
- Business documents.
- Sensitive application logs.

---

## 3. CIA Triad

The CIA Triad is one of the fundamental information security models.

It consists of:

### A. Confidentiality

Ensures that information is accessible only to authorized users.

Example: Customer bank details should not be accessible to other customers.

Security controls:
- Encryption.
- Access control.
- MFA.
- IAM policies.

### B. Integrity

Ensures that information remains accurate and is not modified without authorization.

Example: An attacker changes a customer's bank balance.

Security controls:
- Hashing.
- Digital signatures.
- File Integrity Monitoring.
- Audit logging.

### C. Availability

Ensures that information and services remain accessible to authorized users when required.

Example: Ransomware encrypts company files, preventing employees from accessing them.

Security controls:
- Backups.
- Redundancy.
- Disaster recovery.
- DDoS protection.

**Important:** Ransomware can affect both availability and integrity because it makes data inaccessible through unauthorized modification.

---

## 4. Data Classification Levels

A simple data classification system can use three levels.

### A. Low or Public

Information whose public disclosure would cause little or no damage.

Examples:
- Public IP addresses.
- Public website information.
- Application logs without sensitive information.
- Public software installation files.

Protection: Standard security controls.

### B. Moderate or Private

Information that should not be publicly disclosed and should generally be accessible only to authorized personnel.

Examples:
- Internal system architecture.
- Employee information.
- Internal financial records.
- Purchase orders.
- Travel reimbursement records.

Protection:
- Access restrictions.
- Encryption where appropriate.
- Role-based access control.
- Monitoring.

### C. High or Confidential

Highly sensitive information whose disclosure could cause significant financial, legal, operational or reputational damage.

Examples:
- AWS administrator credentials.
- Customer financial information.
- Trade secrets.
- Confidential business strategies.
- Sensitive intellectual property.

Protection:
- Strict least privilege.
- Strong encryption.
- MFA.
- Secrets management.
- Detailed auditing.
- Continuous monitoring.

The most valuable and sensitive information is often referred to as an organization's **crown jewels**.

Classification should depend on the actual context, potential impact and applicable regulations.

---

## 5. Regulatory and Industry Requirements

Security design may be influenced by legal, regulatory and industry requirements.

### GDPR

General Data Protection Regulation.

An EU data protection regulation governing the processing of personal data within its scope.

Important concepts:
- Personal data identification.
- Data protection.
- Access control.
- Accountability.
- Appropriate security measures.

### FISMA

Federal Information Security Modernization Act.

Requires US federal agencies to establish and maintain information security programs.

### FedRAMP

Federal Risk and Authorization Management Program.

Provides standardized security assessment and authorization requirements for cloud services used by US federal agencies.

### FIPS 199

Provides a framework for categorizing information and information systems according to potential security impact:

- Low.
- Moderate.
- High.

### ITAR

International Traffic in Arms Regulations.

Controls certain defense-related articles, technical data and services.

Organizations handling ITAR-controlled information must consider applicable restrictions when choosing cloud providers and managing access.

### PCI DSS

Payment Card Industry Data Security Standard.

Applies to environments that store, process or transmit payment card information within its scope.

Focuses on protecting cardholder data through access controls, encryption, monitoring and other security requirements.

### HIPAA

Health Insurance Portability and Accountability Act.

US legislation that includes requirements for protecting certain Protected Health Information (PHI).

Examples:
- Patient records.
- Medical histories.
- Health information.
- Insurance-related health information.

**Important:** Security and compliance are different. Security protects systems and data, while compliance involves meeting and demonstrating applicable legal, regulatory or contractual requirements.

---

## 6. Data Asset Management in the Cloud

Cloud providers offer standardized services for storing and processing information.

Examples in AWS:

| Service | Purpose |
|---|---|
| Amazon S3 | Object storage |
| Amazon EBS | Block storage |
| Amazon RDS | Managed relational databases |
| Amazon EFS | Managed file storage |
| Amazon SQS | Message queuing |

Organizations should maintain an inventory of these resources and understand what information each resource contains.

Important questions:

- Which S3 buckets contain sensitive data?
- Which databases contain customer information?
- Which EC2 instances can access those databases?
- Where are backups stored?
- Which IAM identities can access the resources?

### Sensitive Data Discovery

Amazon Macie helps discover and identify sensitive data in Amazon S3.

Other examples:
- Google Cloud Sensitive Data Protection.
- Microsoft Purview.

These services help organizations discover, classify and protect sensitive information.

---

## 7. Hidden Data Assets and Secrets

Not all important data assets are obvious.

Apart from customer databases, organizations must protect:

### Web Server Logs

Logs may contain:
- IP addresses.
- Request URLs.
- Timestamps.
- User agents.
- Session identifiers, depending on logging configuration.

These logs may reveal user behaviour and application activity.

They are also useful during incident investigations.

### TLS Private Keys

Private keys associated with TLS certificates help establish secure communication.

If compromised, they may enable impersonation or other attacks depending on the protocol and configuration.

They must be stored securely.

### Password Hashes

Applications should store password hashes rather than plaintext passwords.

However, stolen password hashes can be attacked offline using tools such as Hashcat and John the Ripper.

Password storage should use dedicated algorithms such as:
- Argon2id.
- bcrypt.
- scrypt.

These should be configured with unique salts and appropriate parameters.

### Database Credentials

Applications commonly require credentials to connect to databases.

Examples:
- Database usernames.
- Passwords.
- API keys.
- Connection strings.

If an attacker compromises the application server, these secrets may also be exposed.

Use secrets management and least privilege to reduce the impact.

---

## 8. Tagging Cloud Resources

A tag is metadata attached to a cloud resource, generally consisting of a key and value.

Example:

| Key | Value |
|---|---|
| Environment | Production |
| Owner | Security-Team |
| DataClass | High |
| Department | Finance |
| Regulation | GDPR |

### Uses of Tags

1. Resource inventory.
2. Data classification.
3. Access-control conditions.
4. Security monitoring.
5. Compliance management.
6. Automated resource discovery.

### Tag Standardization

Organizations should maintain consistent tag names and values.

Example:

- `DataClass: Low`
- `DataClass: Moderate`
- `DataClass: High`
- `Environment: Production`
- `Environment: Development`

Automated tools can identify resources with missing or incorrect tags.

### Cloud Provider Terminology

| Provider | Terminology |
|---|---|
| AWS | Tags |
| Microsoft Azure | Tags |
| Google Cloud | Labels and network tags |
| IBM Cloud | Tags |
| Kubernetes | Labels |

Tags are metadata, not security controls by themselves. They become useful for enforcement when connected to policies, automation and monitoring.

---

## 9. Tokenization

Tokenization replaces sensitive information with a substitute value called a token.

Example:

Original credit card number:
`4532 1234 5678 9012`

Replacement token:
`TKN-8392-ABCD`

The tokenization service maintains the mapping between the token and original sensitive information.

Only an authorized system can retrieve the original value.

### Benefits

- Reduces exposure of sensitive information.
- Limits the number of applications handling actual sensitive data.
- Helps protect payment information.
- Can reduce the impact of certain data breaches.

### Tokenization vs Encryption

| Tokenization | Encryption |
|---|---|
| Replaces sensitive data with a token | Transforms data into ciphertext |
| Uses a secure mapping or token service | Uses cryptographic keys |
| Original data is retrieved through the token service | Original data is recovered through decryption |

---

## 10. Encryption

Encryption transforms readable information (plaintext) into an unreadable representation (ciphertext) using cryptographic algorithms and keys.

There are three important states of data.

### A. Data in Transit

Data travelling across a network.

Example: A user submits login credentials to a website over HTTPS.

Protection: TLS encryption.

### B. Data at Rest

Data stored on persistent storage.

Examples:
- S3 objects.
- EBS volumes.
- RDS databases.
- Backups.

Protection: Storage encryption.

### C. Data in Use

Data currently being processed by a computer.

Example: An application loads decrypted customer records into RAM for processing.

Protection: Confidential computing and hardware-backed memory isolation.

---

## 11. Confidential Computing

Confidential computing aims to protect sensitive data while it is being processed.

It uses hardware-backed Trusted Execution Environments (TEEs).

Examples:
- Intel SGX.
- Intel TDX.
- AMD SEV.
- IBM Z Pervasive Encryption.

These technologies can protect workload memory from certain privileged software and infrastructure-level threats.

They do not eliminate all risks, particularly if the application itself is compromised.

---

## 12. Encryption at Rest and Key Management

Encryption at rest protects stored information.

However, encryption is only as strong as the protection given to its encryption keys.

An attacker who obtains both encrypted data and the necessary decryption key may be able to recover the original information.

This introduces the need for secure key management.

### HSM – Hardware Security Module

An HSM is specialized hardware used to generate, store and perform cryptographic operations with encryption keys.

Features:
- Secure key generation.
- Protected key storage.
- Cryptographic operations.
- Access restrictions.
- Tamper detection and response.

### KMS – Key Management Service

A Key Management Service helps create, manage, control and audit cryptographic keys.

Examples:

| Provider | KMS |
|---|---|
| AWS | AWS Key Management Service |
| Azure | Azure Key Vault |
| Google Cloud | Cloud KMS |
| IBM | Key Protect |

A cloud KMS can use HSMs internally to protect cryptographic keys.

### HSM vs KMS

| HSM | KMS |
|---|---|
| Specialized cryptographic hardware | Key management service |
| Protects cryptographic key material | Manages keys and their lifecycle |
| Can perform cryptographic operations | Controls key usage and access |
| May be dedicated to a customer | Commonly provided as a managed service |

---

## 13. Envelope Encryption

Envelope encryption uses two levels of encryption keys.

### DEK – Data Encryption Key

Encrypts the actual data.

### KEK – Key Encryption Key

Encrypts or wraps the DEK.

### How It Works

1. Generate a DEK.
2. Use the DEK to encrypt the actual data.
3. Use the KEK to encrypt the DEK.
4. Store the encrypted data and wrapped DEK.
5. When decryption is required, use the KEK to unwrap the DEK.
6. Use the DEK to decrypt the original data.

Key hierarchy:

KEK → Protects DEK → Protects actual data.

The KEK is generally protected within the KMS/HSM environment.

The plaintext DEK is used temporarily and discarded when no longer needed.

### Why Envelope Encryption?

Imagine an organization has thousands of files.

Instead of asking KMS to directly encrypt every file, the application can encrypt files using their DEKs and use a centrally managed KEK to protect those DEKs.

Benefits:
- Scalability.
- Centralized key management.
- Reduced dependence on direct KMS encryption operations.
- Easier key rotation and access management.

---

## 14. Server-Side vs Client-Side Encryption

### Server-Side Encryption (SSE)

The cloud service performs encryption after receiving the data.

Example: Amazon S3 with SSE-KMS.

Benefits:
- Easy to configure.
- Less application complexity.
- Integration with cloud KMS.
- Provider-managed encryption operations.

The service can decrypt data for authorized requests.

### Client-Side Encryption

The application encrypts data before sending it to cloud storage.

Process:

1. Application receives plaintext.
2. Application encrypts data locally.
3. Application uploads ciphertext.
4. Cloud storage stores the encrypted data.

Benefits:
- Storage provider does not independently have access to plaintext.
- Greater control over encryption.
- Useful for highly sensitive information.

Drawbacks:
- More application complexity.
- Customer-managed encryption and recovery.
- More demanding key management.
- Limited server-side searching and processing.

### Comparison

| Feature | Server-side | Client-side |
|---|---|---|
| Encryption | Cloud service | Application |
| Complexity | Lower | Higher |
| Key management | Often provider-integrated | Customer-controlled or separately managed |
| Cloud storage access to plaintext | Available through authorized service operations | Not available if keys remain outside provider control |

Use well-tested cryptographic libraries. Never implement custom encryption algorithms.

---

## 15. Homomorphic Encryption

Homomorphic encryption allows certain computations to be performed directly on encrypted data without first decrypting it.

Example:

A service could potentially calculate statistics on encrypted customer information without accessing the plaintext values.

It has interesting privacy applications but can be computationally expensive.

---

## 16. Cryptographic Erasure

Cryptographic erasure makes encrypted data unrecoverable by securely destroying the necessary encryption key material.

Example:

An organization stores 100 GB of encrypted customer information.

If the required decryption keys are securely destroyed, the encrypted data becomes computationally infeasible to recover.

### Benefits

- Faster than overwriting large amounts of data.
- Useful for distributed storage.
- Can address copies of encrypted data.
- Useful for secure data disposal.

### Limitation

Cryptographic erasure does not automatically free storage space.

The encrypted files may still physically exist.

Therefore, distinguish between:

- Cryptographic erasure: Makes data inaccessible.
- Storage deletion: Removes stored objects.
- Storage reclamation: Frees storage capacity.

All usable copies of the relevant key material must be destroyed or made permanently inaccessible for cryptographic erasure to be effective.

---

## 17. Encryption at Different Application Layers

Encryption can be implemented at three major layers.

### A. Disk-Level Encryption

Encryption happens at the storage layer.

Example: AWS EBS encryption.

Benefits:
- Protects against unauthorized access to physical storage media.
- Usually transparent to applications.
- Often managed by the cloud provider.

Limitations:
- Does not necessarily protect data from an attacker with authorized access to the storage system.
- Does not protect against every application or database compromise.

### B. Platform-Level Encryption

Encryption happens at the database or managed platform layer.

Example: Database storage encryption.

Benefits:
- Protects underlying storage from direct access.
- Database operations generally continue normally.
- Usually integrated into managed services.

Limitations:
- Database administrators and authorized database operations may still access plaintext.
- Database-level encryption can affect compression and deduplication.

### C. Application-Level Encryption

Encryption happens inside the application before data is sent to the database.

Benefits:
- Can protect selected sensitive fields from database-level compromise.
- Provides an additional layer of confidentiality.
- Allows application-controlled encryption policies.

Limitations:
- More development complexity.
- Additional key management.
- May affect searching, sorting and database operations.
- Can introduce performance overhead.

### Comparison

| Feature | Disk-Level | Platform-Level | Application-Level |
|---|---|---|---|
| Encryption layer | Storage | Database | Application |
| Protects physical storage theft | Yes | Yes | Yes |
| Protects against storage-layer access | Limited | Better | Better |
| Protects against database administrator access | Generally no | Generally no | Can protect encrypted fields |
| Complexity | Low | Low–moderate | High |

A practical approach is to use provider-managed encryption for ordinary workloads and consider application-level encryption for particularly sensitive information.

---

## 18. Quantum-Safe Cryptography

Quantum computers may eventually threaten certain widely used public-key cryptographic algorithms.

Examples:
- RSA.
- Elliptic Curve Cryptography (ECC).

Shor's algorithm could break these systems when sufficiently powerful fault-tolerant quantum computers become available.

### Harvest Now, Decrypt Later

An attacker collects encrypted information today and stores it.

In the future, sufficiently powerful quantum computers may make it possible to decrypt information protected by vulnerable algorithms.

This is particularly concerning for information requiring long-term confidentiality.

### Post-Quantum Cryptography (PQC)

PQC refers to cryptographic algorithms designed to resist attacks from both classical and known quantum computing approaches.

Examples of NIST-standardized algorithms:

- ML-KEM: Key encapsulation mechanism.
- ML-DSA: Digital signature algorithm.
- SLH-DSA: Stateless hash-based digital signature algorithm.

AES-256 is also considered a strong symmetric encryption choice against foreseeable quantum attacks, although Grover's algorithm reduces its effective brute-force security.

Organizations should plan for cryptographic migration rather than assuming all current encryption will remain secure indefinitely.

---

## 19. Encryption Strategy: Best Practices

When designing a cloud encryption strategy:

### Data Identification
- Identify all important data assets.
- Understand where sensitive information is stored.
- Classify data according to sensitivity.

### Encryption
- Use encryption at rest.
- Use TLS for data in transit.
- Consider confidential computing for sensitive workloads.
- Consider application-level encryption for highly sensitive data.

### Key Management
- Use managed KMS solutions where appropriate.
- Protect encryption keys.
- Apply least privilege to key access.
- Monitor key usage.
- Plan for key rotation and recovery.
- Establish emergency access procedures.

### Resource Management
- Standardize cloud resource tags.
- Automate identification of untagged resources.
- Maintain an inventory of sensitive storage locations.

### Data Deletion
- Establish secure deletion procedures.
- Consider cryptographic erasure.
- Account for backups, replicas and snapshots.
- Ensure key destruction does not violate retention or evidence-preservation requirements.

### Performance
- Evaluate encryption overhead.
- Test using realistic workloads.
- Prefer established and supported cryptographic implementations.

---

## 20. Practical Relevance to Cloud Security and DFIR

These concepts are particularly useful when investigating cloud security incidents.

An analyst may need to determine:

1. What sensitive data was exposed?
2. Where was the data stored?
3. Was the data encrypted?
4. Which encryption mechanism was used?
5. Who had access to the encryption keys?
6. Were KMS operations performed by unauthorized identities?
7. Was data copied, modified or deleted?
8. Were backups or replicas affected?
9. Did the incident affect confidentiality, integrity or availability?

### Useful AWS Investigation Sources

- AWS CloudTrail: API activity and KMS operations.
- Amazon S3 access logs: Object access activity, if enabled.
- S3 server access logging: Detailed request records.
- VPC Flow Logs: Network traffic metadata.
- IAM: Identity and permission investigation.
- AWS Config: Resource configuration history, where configured.

### Key Takeaway

Data protection is not limited to enabling encryption.

A secure cloud environment requires data identification, classification, access control, encryption, key management, monitoring and proper data disposal.

**Chapter 2 establishes how to identify valuable information and protect it throughout its lifecycle.**
