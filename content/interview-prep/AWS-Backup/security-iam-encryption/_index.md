---
title: " 🔐 Security, IAM & Encryption"
description: "Interview-focused AWS Backup security covering IAM, KMS encryption, backup vaults, Vault Lock, cross-account isolation, least privilege, audit logging, ransomware resilience and recovery troubleshooting."
weight: 130
toc: true
---

AWS Backup security is not only about encrypting recovery points. A production-grade design must control **who can create, copy, delete and restore backups**, protect encryption keys, isolate recovery accounts and provide audit evidence.
## 01 — Security Mental Model
```text
BACKUP SECURITY
      │
 ┌────┼───────────┐
 ↓    ↓           ↓
IAM  ENCRYPTION  ISOLATION
 ↓    ↓           ↓
WHO  KMS        ACCOUNT / VAULT
 │    │           │
 └────┼───────────┘
      ↓
    AUDIT
      ↓
  RECOVERY
```
### Core principle
> **Protect the recovery data, the encryption keys and the administrative paths to both.**
## 02 — AWS Backup Security Layers
A strong architecture uses multiple layers:
```text
IDENTITY
   ↓
IAM
   ↓
AWS BACKUP
   ↓
BACKUP VAULT
   ↓
KMS
   ↓
VAULT LOCK
   ↓
CROSS-ACCOUNT
   ↓
CROSS-REGION
   ↓
AUDIT / MONITORING
```
### Interview answer
> "I design backup security as defense in depth rather than relying on encryption alone."
## 03 — IAM and AWS Backup
IAM controls access to AWS Backup operations.
Typical permissions may cover:
- Creating backup plans
- Assigning resources
- Starting backup jobs
- Copying recovery points
- Restoring resources
- Deleting recovery points
- Managing vaults
### Principle
> **Not every backup administrator needs permission to delete recovery points or manage KMS keys.**
Reference: https://docs.aws.amazon.com/aws-backup/latest/devguide/security-iam-awsmanpol.html
## 04 — Least Privilege
Avoid:
```text
BACKUP ADMIN
      ↓
AdministratorAccess
```
Prefer:
```text
BACKUP OPERATOR
      ↓
ONLY REQUIRED BACKUP ACTIONS
```
Separate highly privileged roles where practical:
```text
BACKUP OPERATOR
BACKUP AUDITOR
VAULT ADMIN
KMS ADMIN
RESTORE OPERATOR
SECURITY ADMIN
```
### Interview answer
> "I separate operational backup permissions from destructive and key-management permissions."
## 05 — Backup Administrator vs KMS Administrator
These are different responsibilities.
```text
BACKUP ADMIN
→ BACKUP PLAN
→ BACKUP JOB
→ VAULT OPERATIONS
```
```text
KMS ADMIN
→ KEY LIFECYCLE
→ KEY POLICY
→ KEY CONFIGURATION
```
### Senior-level point
Separating duties reduces the blast radius of a compromised administrative identity.
## 06 — Restore Permissions
Restore access can be sensitive because it can create infrastructure or expose recovered data.
```text
RESTORE REQUEST
      ↓
IAM
      ↓
AWS BACKUP
      ↓
RESOURCE
```
### Control
Use a dedicated restore role where appropriate.
### Interview question
> "Should every backup operator be allowed to restore production data?"
**Not necessarily.**
Access should follow operational responsibility and least privilege.
## 07 — Delete Permissions
Deletion is especially sensitive.
```text
BACKUP
  ↓
DELETE
  ↓
RECOVERY POINT LOST
```
### Principle
Restrict deletion permissions and use backup-vault protection where appropriate.
### Interview answer
> "I treat backup deletion as a high-risk operation because deleting the recovery point can remove the organization's last recovery option."
## 08 — AWS Backup Vault
A backup vault is a logical container for recovery points.
```text
RECOVERY POINT
      ↓
BACKUP VAULT
      ↓
RETENTION
      ↓
RECOVERY
```
### Security controls
- Access policies
- Encryption
- Vault Lock
- Monitoring
- Cross-account architecture
Reference: https://docs.aws.amazon.com/aws-backup/latest/devguide/vaults.html
## 09 — Vault Access Policy
A vault access policy can control which principals or accounts can perform actions against the vault.
```text
PRINCIPAL
   ↓
VAULT POLICY
   ↓
ALLOW / DENY
   ↓
RECOVERY POINT
```
### Interview point
IAM policies and resource-based vault policies can work together.
> **The effective permission is determined by the complete authorization model.**
## 10 — AWS Backup Vault Lock
Vault Lock provides additional protection for recovery points against deletion or changes according to its configuration.
```text
RECOVERY POINT
      ↓
VAULT
      ↓
VAULT LOCK
      ↓
PROTECTED RETENTION
```
### Use cases
- Compliance
- Ransomware resilience
- Long-term retention
- Protection against malicious deletion
Reference: https://docs.aws.amazon.com/aws-backup/latest/devguide/vault-lock.html
## 11 — Governance Mode
Governance mode provides protection while retaining an administrative mechanism for authorized changes during the applicable grace period.
```text
VAULT
 ↓
GOVERNANCE LOCK
 ↓
PROTECTED RECOVERY
```
### Interview point
Useful where an organization needs stronger protection but still requires controlled administrative flexibility.
## 12 — Compliance Mode
Compliance mode is designed for stronger immutability.
```text
VAULT
 ↓
COMPLIANCE LOCK
 ↓
GRACE PERIOD
 ↓
STRONG RETENTION PROTECTION
```
### Important
Configuration must be reviewed carefully before enabling controls that can become effectively irreversible.
### Interview answer
> "I validate retention, recovery and compliance requirements before enabling irreversible vault controls."
## 13 — Encryption at Rest
AWS Backup encrypts backup data using AWS KMS.
```text
RESOURCE
   ↓
BACKUP
   ↓
ENCRYPTED RECOVERY POINT
   ↓
KMS
```
### Key considerations
- AWS managed / service-managed encryption options
- Customer managed KMS keys where supported and required
- Key policy
- IAM permissions
- Key lifecycle
Reference: https://docs.aws.amazon.com/aws-backup/latest/devguide/encryption.html
## 14 — KMS Customer Managed Keys
A customer managed key provides additional control over:
- Key policy
- Key administrators
- Key rotation settings
- Key lifecycle
- Auditability
### Architecture
```text
AWS BACKUP
     ↓
CUSTOMER KMS KEY
     ↓
ENCRYPTED RECOVERY POINT
```
### Interview point
Customer-managed keys increase control but also increase operational responsibility.
## 15 — KMS Key Policy
IAM permissions alone may not be enough for KMS operations.
```text
REQUEST
  ↓
IAM
  ↓
KMS KEY POLICY
  ↓
KMS
```
### Troubleshooting questions
- Is the key enabled?
- Does the key policy allow the required principal?
- Does IAM allow the action?
- Is the key in the correct Region?
- Is the account relationship configured correctly?
## 16 — KMS Key Disabled
A disabled key can prevent encrypted recovery operations.
```text
RESTORE
  ↓
KMS KEY
  ↓
DISABLED
  ↓
FAILURE
```
### Recovery
Check:
```text
KEY STATE
↓
KEY POLICY
↓
IAM
↓
BACKUP CONFIGURATION
```
### Senior-level point
> **KMS key lifecycle is part of disaster recovery.**
## 17 — KMS Key Deletion Risk
Deleting a customer-managed KMS key can make encrypted data unrecoverable depending on the service and key usage.
```text
BACKUP
 ↓
KMS KEY
 ↓
KEY DELETED
 ↓
RECOVERY RISK
```
### Control
- Restrict key deletion
- Use separate key administrators
- Monitor key lifecycle operations
- Include KMS in DR runbooks
### Interview answer
> "I protect KMS keys as carefully as the backups because losing the encryption key can undermine the recovery strategy."
## 18 — Cross-Account Backup Security
```text
PRODUCTION ACCOUNT
       ↓
BACKUP COPY
       ↓
BACKUP ACCOUNT
       ↓
PROTECTED VAULT
```
### Security benefit
A production compromise has a harder path to the recovery copy.
### Additional controls
- Separate administrators
- Least privilege
- Vault policies
- Vault Lock
- KMS
- CloudTrail
## 19 — Cross-Region Security
```text
PRIMARY REGION
      ↓
BACKUP COPY
      ↓
DR REGION
      ↓
DR VAULT
```
### Security questions
- Is the destination vault protected?
- Is destination encryption configured?
- Are IAM permissions correct?
- Is the destination KMS key available?
- Is the DR account isolated?
## 20 — Cross-Account + Cross-Region
A strong security architecture:
```text
                 PRODUCTION
                     │
                     ↓
                   BACKUP
                     │
             ┌───────┴────────┐
             ↓                ↓
       DR REGION        BACKUP ACCOUNT
             │                │
          VAULT              VAULT
             │                │
             └───────┬────────┘
                     ↓
                PROTECTED DATA
```
### Goal
Separate:
- Region failure
- Account compromise
- Backup deletion
- Administrative compromise
## 21 — Ransomware Protection
Ransomware resilience requires multiple controls.
```text
PRODUCTION
    ↓
BACKUP
    ↓
CROSS-ACCOUNT
    ↓
CROSS-REGION
    ↓
VAULT LOCK
    ↓
RECOVERY
```
### Security controls
- Least privilege
- MFA
- Separate backup account
- Vault Lock
- KMS
- CloudTrail
- Monitoring
- Restore testing
### Interview answer
> "I assume production credentials can eventually be compromised, so I design recovery copies outside the same administrative blast radius."
## 22 — Immutable Backup
Immutability means recovery points cannot be freely modified or deleted during protected retention.
```text
BACKUP
 ↓
VAULT
 ↓
LOCK
 ↓
IMMUTABLE RETENTION
```
### Important
Encryption alone does not provide immutability.
Account separation alone does not guarantee immutability.
### Interview memory
> **Encryption protects confidentiality.**
> **Immutability protects recovery history.**
## 23 — Confidentiality, Integrity, Availability
Backup security maps naturally to the CIA triad.
```text
CONFIDENTIALITY
→ ENCRYPTION + IAM
```
```text
INTEGRITY
→ VAULT LOCK + ACCESS CONTROL
```
```text
AVAILABILITY
→ CROSS-REGION + DR
```
### Interview answer
> "A complete backup security architecture addresses confidentiality, integrity and availability."
## 24 — Backup Data in Transit
When backup data is transferred between AWS services or Regions, AWS security controls and service-specific encryption mechanisms apply.
### Interview point
Do not confuse:
```text
AT REST
→ KMS
```
with:
```text
IN TRANSIT
→ TLS / SERVICE COMMUNICATION SECURITY
```
Security design should account for both.
## 25 — IAM Policy Conditions
Where supported, IAM policy conditions can further constrain access.
Examples:
```text
SOURCE ACCOUNT
SOURCE IP / NETWORK CONTEXT
RESOURCE ARN
REQUEST CONDITIONS
```
### Principle
Use conditions carefully so recovery operations are not accidentally blocked.
## 26 — Service Control Policies
AWS Organizations Service Control Policies can establish organization-level guardrails.
```text
ORGANIZATION
      ↓
SCP
      ↓
ACCOUNT
      ↓
IAM
      ↓
AWS BACKUP
```
### Use case
Prevent dangerous actions across accounts where organizational policy requires it.
### Interview answer
> "I use SCPs as preventive guardrails and IAM as workload-level access control."
## 27 — IAM Access Analyzer
IAM Access Analyzer can help identify unintended external access and validate resource policies.
```text
POLICY
 ↓
ANALYZER
 ↓
POTENTIAL EXTERNAL ACCESS
 ↓
REVIEW
```
### Interview point
Use analysis tools as part of continuous security review.
## 28 — CloudTrail
CloudTrail records AWS API activity for auditing and investigation.
```text
ADMIN ACTION
     ↓
CLOUDTRAIL
     ↓
EVENT
     ↓
INVESTIGATION
```
### Useful questions
- Who changed backup configuration?
- Who deleted a recovery point?
- Who changed a vault?
- Who modified IAM?
- Who changed KMS configuration?
Reference: https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html
## 29 — CloudWatch and EventBridge
Use monitoring and event-driven controls to detect backup security events.
```text
EVENT
 ↓
EVENTBRIDGE
 ↓
RULE
 ↓
SNS / LAMBDA / AUTOMATION
```
### Monitor
- Backup failures
- Copy failures
- Restore failures
- Configuration changes
- Security events
## 30 — Backup Security Monitoring
A mature dashboard should answer:
```text
WHO HAS BACKUP ACCESS?
WHO CAN DELETE?
WHO CAN RESTORE?
WHICH KEYS ARE USED?
WHICH VAULTS ARE LOCKED?
WHICH RESOURCES ARE PROTECTED?
WHICH COPIES FAILED?
```
### Senior-level point
Security monitoring should cover both **data protection** and **administrative protection**.
## 31 — Scenario: Backup Deletion Attempt
```text
ATTACKER
  ↓
DELETE RECOVERY POINT
  ↓
VAULT POLICY / LOCK
  ↓
DENY
```
### Response
- Investigate CloudTrail
- Identify principal
- Review IAM
- Verify vault controls
- Verify other recovery copies
### Interview answer
> "I want deletion to fail or be constrained by an independent recovery boundary, then investigate the attempted operation."
## 32 — Scenario: Production Credentials Compromised
```text
COMPROMISED IDENTITY
       ↓
PRODUCTION ACCOUNT
       ↓
DESTRUCTIVE ACTION
       X
       ↓
SEPARATE BACKUP ACCOUNT
       ↓
PROTECTED VAULT
```
### Design
- Separate account
- Separate administrative roles
- Vault Lock
- Restricted KMS access
- Audit logging
## 33 — Scenario: KMS AccessDenied During Restore
```text
RESTORE
  ↓
ACCESSDENIED
  ↓
KMS
  ↓
IAM + KEY POLICY
```
### Troubleshooting
1. Identify the principal
2. Check IAM permission
3. Check key policy
4. Check key state
5. Check Region
6. Check account relationship
### Interview answer
> "For KMS AccessDenied, I inspect both IAM and the key policy because both can affect authorization."
## 34 — Scenario: Recovery Point Exists but Cannot Be Read
Check:
```text
VAULT ACCESS
 ↓
IAM
 ↓
VAULT POLICY
 ↓
KMS
 ↓
KEY POLICY
 ↓
KEY STATE
```
### Lesson
A visible recovery point does not necessarily mean the current operator can restore it.
## 35 — Scenario: Backup Copy Fails Across Accounts
Troubleshooting:
```text
SOURCE
 ↓
BACKUP POLICY
 ↓
DESTINATION ACCOUNT
 ↓
DESTINATION VAULT
 ↓
VAULT POLICY
 ↓
KMS
 ↓
REGION
```
### Interview point
Cross-account backup requires end-to-end authorization, not only source-account permissions.
## 36 — Scenario: Backup Account Administrator Is Compromised
This is a high-impact event.
### Defense in depth
```text
BACKUP ACCOUNT
      ↓
LIMITED ADMINISTRATORS
      ↓
VAULT LOCK
      ↓
CROSS-REGION COPY
      ↓
INDEPENDENT RECOVERY OPTION
```
### Senior-level point
For very critical environments, consider multiple independent recovery boundaries rather than one central account becoming a single point of failure.
## 37 — Scenario: KMS Key Administrator Is Compromised
Potential impact:
```text
KMS ADMIN
 ↓
KEY POLICY / KEY LIFECYCLE
 ↓
RECOVERY
```
### Controls
- Separate duties
- Least privilege
- MFA
- Monitoring
- Key deletion controls
- Break-glass procedures
### Lesson
KMS administration is part of the recovery security boundary.
## 38 — Secrets and Credentials
A restored application may require:
```text
SECRETS
CERTIFICATES
IAM ROLES
API KEYS
DATABASE CREDENTIALS
```
### Interview point
Do not store sensitive credentials in backup runbooks as plain text.
Use appropriate AWS security services and controlled access mechanisms.
## 39 — Backup Security and Compliance
Common requirements:
- Encryption
- Retention
- Immutability
- Auditability
- Access control
- Geographic separation
- Separation of duties
### Architecture
```text
IAM
 ↓
KMS
 ↓
BACKUP
 ↓
CROSS-ACCOUNT
 ↓
CROSS-REGION
 ↓
VAULT LOCK
 ↓
CLOUDTRAIL
```
## 40 — Separation of Duties
A mature model:
```text
SECURITY ADMIN
     │
     ├── IAM
     │
KMS ADMIN
     │
     ├── KMS
     │
BACKUP ADMIN
     │
     ├── BACKUP
     │
RESTORE OPERATOR
     │
     └── RECOVERY
```
### Goal
No single routine operator should automatically control every security layer.
## 41 — Break-Glass Access
Emergency access may be required during a disaster.
```text
NORMAL ACCESS
   ↓
RESTRICTED
```
```text
DISASTER
   ↓
BREAK-GLASS ROLE
   ↓
RECOVERY
```
### Controls
- Strong authentication
- Limited duration
- Approval
- Logging
- Post-incident review
### Interview answer
> "Break-glass access should be exceptional, controlled and auditable."
## 42 — Backup Security Runbook
```text
1. IDENTIFY INCIDENT
        ↓
2. IDENTIFY ACCOUNT / REGION
        ↓
3. IDENTIFY RECOVERY POINT
        ↓
4. VERIFY IAM
        ↓
5. VERIFY VAULT ACCESS
        ↓
6. VERIFY KMS
        ↓
7. RESTORE
        ↓
8. VALIDATE
        ↓
9. AUDIT
```
## 43 — Security Validation Before Restore
Before restoring sensitive data:
```text
WHO
 ↓
WHAT
 ↓
WHERE
 ↓
WHICH RECOVERY POINT
 ↓
WHICH KMS KEY
 ↓
WHICH DESTINATION
```
### Questions
- Is the requester authorized?
- Is the recovery point correct?
- Is the destination secure?
- Is the data classified?
- Are required keys available?
## 44 — Security Validation After Restore
Validate:
```text
IAM
 ↓
KMS
 ↓
NETWORK
 ↓
SECURITY GROUPS
 ↓
DATA ACCESS
 ↓
APPLICATION
```
### Principle
> **A successful restore should not accidentally create a security regression.**
## 45 — Backup Security Testing
Test:
- Unauthorized backup deletion
- Unauthorized vault access
- Unauthorized restore
- KMS denial
- Cross-account recovery
- Cross-Region recovery
- Vault Lock behavior
- Break-glass access
- Audit logging
### Test model
```text
ATTEMPT
 ↓
CONTROL
 ↓
DENY / ALLOW
 ↓
AUDIT
 ↓
RECOVERY
```
## 46 — Security Architecture Checklist
### IAM
- [ ] Least privilege
- [ ] Separate backup roles
- [ ] Restore access controlled
- [ ] Delete access restricted
- [ ] MFA for privileged users
### KMS
- [ ] Encryption requirements defined
- [ ] Key policy reviewed
- [ ] Key state monitored
- [ ] Key administrators separated
- [ ] Key lifecycle documented
### Vault
- [ ] Vault access policy
- [ ] Vault Lock where required
- [ ] Retention validated
### Isolation
- [ ] Cross-account copy
- [ ] Cross-Region copy
- [ ] Separate administrators
### Audit
- [ ] CloudTrail
- [ ] Monitoring
- [ ] Alerts
### Recovery
- [ ] Restore tested
- [ ] KMS recovery tested
- [ ] Cross-account recovery tested
- [ ] Cross-Region recovery tested
## 47 — Senior-Level Security Architecture
```text
                         PRODUCTION
                             │
                           DATA
                             │
                           BACKUP
                             │
              ┌──────────────┴──────────────┐
              ↓                             ↓
        CROSS-REGION                  CROSS-ACCOUNT
              │                             │
          DR VAULT                    BACKUP VAULT
              │                             │
              └──────────────┬──────────────┘
                             ↓
                         VAULT LOCK
                             ↓
                            KMS
                             ↓
                        RECOVERY
```
### Administrative layers
```text
IAM
 ↓
SCP
 ↓
VAULT POLICY
 ↓
KMS POLICY
 ↓
CLOUDTRAIL
 ↓
MONITORING
```
## 48 — 60-Second Security Interview Answer
> "For AWS Backup security, I start with least-privilege IAM and separation of duties. I restrict who can create, restore and especially delete recovery points. I protect backup data with KMS encryption and treat KMS key policies and lifecycle as part of the recovery dependency. For critical workloads, I use cross-account and cross-Region copies to separate recovery from the production failure domain, and I evaluate Vault Lock for immutable retention. I use CloudTrail and monitoring to audit administrative actions and test unauthorized operations. Finally, I regularly test restore access, KMS access and cross-account recovery so the security controls do not prevent legitimate disaster recovery."
## 49 — Security Interview Traps
### Trap 1
> "Encryption makes backups secure."
**Better:** Encryption protects data confidentiality; IAM, vault controls, isolation and auditing are also required.
### Trap 2
> "Anyone with backup access can delete backups."
**Better:** Deletion should be tightly controlled and protected by independent recovery boundaries.
### Trap 3
> "IAM controls all KMS access."
**Better:** KMS authorization can involve both IAM and the KMS key policy.
### Trap 4
> "Vault Lock is just encryption."
**Better:** Vault Lock provides retention and immutability-oriented protection.
### Trap 5
> "Cross-Region backup protects against account compromise."
**Better:** Cross-Region addresses geographic failure; cross-account improves account-level isolation.
### Trap 6
> "The KMS key can be managed like any other application resource."
**Better:** KMS key lifecycle is a critical recovery dependency.
### Trap 7
> "Backup account should have administrator access for convenience."
**Better:** Use least privilege and separation of duties.
### Trap 8
> "Successful restore means security is correct."
**Better:** Validate IAM, KMS, network and application access after restore.
## 50 — Final Security Mental Model
Memorize:
```text
IAM
→ WHO CAN ACT
```
```text
KMS
→ WHO CAN USE THE KEY
→ DATA ENCRYPTION
```
```text
VAULT POLICY
→ WHO CAN ACCESS THE VAULT
```
```text
VAULT LOCK
→ PROTECT RECOVERY HISTORY
```
```text
CROSS-ACCOUNT
→ ADMINISTRATIVE ISOLATION
```
```text
CROSS-REGION
→ REGIONAL ISOLATION
```
```text
CLOUDTRAIL
→ AUDIT
```
```text
RESTORE TEST
→ PROVE SECURITY + RECOVERY
```
### Final principle
> **Secure backups by protecting identity, encryption keys, recovery points and administrative boundaries together.**
## Official AWS References
- [AWS Backup Security and IAM](https://docs.aws.amazon.com/aws-backup/latest/devguide/security-iam-awsmanpol.html)
- [AWS Backup Security](https://docs.aws.amazon.com/aws-backup/latest/devguide/security.html)
- [AWS Backup Vaults](https://docs.aws.amazon.com/aws-backup/latest/devguide/vaults.html)
- [AWS Backup Vault Lock](https://docs.aws.amazon.com/aws-backup/latest/devguide/vault-lock.html)
- [AWS Backup Encryption](https://docs.aws.amazon.com/aws-backup/latest/devguide/encryption.html)
- [AWS Backup Cross-Account Backup](https://docs.aws.amazon.com/aws-backup/latest/devguide/manage-cross-account-backup.html)
- [AWS Backup Cross-Region Backup](https://docs.aws.amazon.com/aws-backup/latest/devguide/cross-region-backup.html)
- [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
- [AWS Key Management Service Developer Guide](https://docs.aws.amazon.com/kms/latest/developerguide/)
