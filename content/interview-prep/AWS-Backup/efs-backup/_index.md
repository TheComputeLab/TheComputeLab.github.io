---
title: " 📁 EFS Backup"
description: "Interview-focused Amazon EFS backup and recovery covering AWS Backup, EFS recovery points, lifecycle, replication, encryption, cross-Region recovery, restore workflows, RPO/RTO and troubleshooting."
weight: 90
toc: true
---

Amazon Elastic File System (EFS) is a managed elastic file system designed for shared file storage. A strong EFS protection strategy combines **AWS Backup, recovery points, lifecycle management, replication where appropriate, encryption, access controls and tested recovery procedures**.
## 01 — EFS Backup Mental Model
Think about EFS protection as:
```text
EFS FILE SYSTEM
      │
      ↓
AWS BACKUP
      │
      ↓
RECOVERY POINT
      │
      ├──────────────┐
      ↓              ↓
BACKUP VAULT    CROSS-REGION COPY
      │              │
      ↓              ↓
RETENTION        DR RECOVERY
      │
      ↓
RESTORE
      │
      ↓
NEW / RECOVERED EFS FILE SYSTEM
```
### Core principle
> **EFS backup protects file-system data, while recovery also requires the network, IAM, security groups, mount configuration and application dependencies needed to access that data.**
## 02 — Does EFS Need Backup?
EFS provides highly durable managed storage, but durability is not the same as protection from:
- Accidental deletion
- Application-level deletion
- File corruption
- Malicious activity
- Compliance retention requirements
- Regional disaster scenarios
### Interview answer
> "EFS is highly durable, but I still use backup and recovery controls when the business needs historical recovery, protection from logical mistakes or disaster recovery."
## 03 — AWS Backup for EFS
AWS Backup provides centralized protection for EFS.
```text
EFS
 ↓
AWS BACKUP
 ↓
BACKUP PLAN
 ↓
BACKUP RULE
 ↓
RECOVERY POINT
 ↓
BACKUP VAULT
```
### Benefits
- Centralized policy
- Scheduling
- Retention
- Backup vaults
- Cross-Region copies
- Cross-account governance where supported
- Centralized monitoring
Reference: https://docs.aws.amazon.com/aws-backup/latest/devguide/working-with-supported-services.html
## 04 — EFS Recovery Point
A recovery point represents a protected state of the EFS file system.
```text
EFS
 ↓
BACKUP
 ↓
RECOVERY POINT
 ↓
RESTORE
 ↓
RECOVERED FILE SYSTEM
```
### Interview phrase
> **"I select the recovery point based on the incident time and the required RPO."**
## 05 — EFS Backup Frequency
Backup frequency should be derived from the business RPO.
```text
BUSINESS RPO
     ↓
BACKUP FREQUENCY
     ↓
RECOVERY POINT
```
### Example
```text
RPO = 24 HOURS
→ DAILY BACKUP MAY BE ACCEPTABLE

RPO = 4 HOURS
→ MORE FREQUENT PROTECTION MAY BE REQUIRED
```
### Interview answer
> "I start with the RPO and then design the backup schedule around it."
## 06 — EFS RPO
RPO defines how much data the business can afford to lose.
For EFS:
```text
RPO
 ↓
BACKUP FREQUENCY
 ↓
AVAILABLE RECOVERY POINT
```
### Important
Do not confuse EFS backup frequency with replication or application-level synchronization.
## 07 — EFS RTO
RTO defines how quickly the file system must be available again.
```text
INCIDENT
   ↓
SELECT RECOVERY POINT
   ↓
RESTORE
   ↓
CONFIGURE ACCESS
   ↓
MOUNT
   ↓
APPLICATION VALIDATION
   ↓
SERVICE RESTORED
```
### Interview point
> **"EFS restore time is only part of the application RTO; mounting, permissions and application validation also matter."**
## 08 — EFS Restore
An EFS recovery point can be restored to an EFS file system.
Typical flow:
```text
RECOVERY POINT
      ↓
RESTORE
      ↓
TARGET EFS
      ↓
MOUNT TARGET
      ↓
VALIDATE FILES
      ↓
APPLICATION RECOVERY
```
### Important
Plan the target network and security configuration before an incident.
Reference: https://docs.aws.amazon.com/aws-backup/latest/devguide/restoring-efs.html
## 09 — Restore to a New File System
A common recovery approach is to restore into a separate EFS file system.
```text
PROTECTED EFS
      ↓
BACKUP
      ↓
RECOVERY POINT
      ↓
RESTORE
      ↓
NEW EFS FILE SYSTEM
```
### Why?
- Safer validation
- Avoids immediately modifying production
- Allows file-level comparison
- Supports controlled application cutover
### Interview answer
> "I prefer a separate restore target when I need to validate recovered data before reconnecting production workloads."
## 10 — EFS Restore Validation
After restore:
```text
RESTORE
 ↓
MOUNT
 ↓
CHECK FILES
 ↓
CHECK OWNERSHIP
 ↓
CHECK PERMISSIONS
 ↓
CHECK APPLICATION DATA
 ↓
APPLICATION TEST
```
### Validate
- File count where relevant
- Directory structure
- Permissions
- Ownership
- Application access
- Expected timestamps / versions where applicable
## 11 — EFS Mount Architecture
An application normally accesses EFS through mount targets.
```text
EC2 / CONTAINER
       ↓
NETWORK
       ↓
EFS MOUNT TARGET
       ↓
EFS FILE SYSTEM
```
### Recovery implication
Restoring the data does not automatically solve every networking or application configuration requirement.
## 12 — EFS Mount Targets
EFS mount targets are associated with Availability Zones and VPC networking.
For recovery, validate:
```text
VPC
 ↓
SUBNET
 ↓
MOUNT TARGET
 ↓
SECURITY GROUP
 ↓
NFS ACCESS
 ↓
APPLICATION
```
### Interview answer
> "I treat mount targets and network access as part of the recovery design."
Reference: https://docs.aws.amazon.com/efs/latest/ug/accessing-fs.html
## 13 — EFS Security Groups
EFS commonly uses network security controls for NFS access.
```text
APPLICATION
     ↓
TCP 2049
     ↓
EFS MOUNT TARGET
```
### Troubleshooting
If the restored file system cannot be mounted, check:
- Security group rules
- Subnet
- VPC
- Routing
- DNS
- NFS access
### Interview point
> "A successful data restore is useless if the workload cannot mount the file system."
## 14 — IAM and EFS Recovery
Where IAM authorization is used for EFS access:
```text
APPLICATION
     ↓
IAM IDENTITY
     ↓
EFS AUTHORIZATION
     ↓
FILE SYSTEM
```
### Check
- IAM role
- EFS file-system policy
- Client authorization
- Access point permissions
### Senior-level point
Security configuration must be included in recovery testing.
## 15 — EFS Access Points
EFS Access Points provide application-specific entry points into a file system.
```text
APPLICATION A
      ↓
ACCESS POINT A
      ↓
EFS
```
```text
APPLICATION B
      ↓
ACCESS POINT B
      ↓
EFS
```
### Recovery consideration
If the application depends on a particular access point, recreate or configure the required access path during recovery.
Reference: https://docs.aws.amazon.com/efs/latest/ug/efs-access-points.html
## 16 — EFS Encryption
EFS supports encryption at rest and encryption in transit.
```text
APPLICATION
     ↓
ENCRYPTED CONNECTION
     ↓
EFS
     ↓
KMS / ENCRYPTION
```
### Interview answer
> "For encrypted EFS recovery, I validate the encryption configuration and the permissions required by the recovery workload."
Reference: https://docs.aws.amazon.com/efs/latest/ug/encryption.html
## 17 — KMS and EFS Recovery
If EFS uses a customer managed KMS key, recovery planning should include:
```text
EFS
 ↓
KMS KEY
 ↓
IAM / KEY POLICY
 ↓
RECOVERY ACCESS
```
### Troubleshooting
- Is the key enabled?
- Does the recovery identity have permission?
- Is the key available in the required Region?
- Are key policies correct?
### Interview phrase
> **"KMS is part of the recovery dependency chain, not just an encryption setting."**
## 18 — EFS Backup Retention
Retention should reflect:
- RPO
- Compliance
- Recovery history
- Cost
- Incident detection time
```text
BACKUP
 ↓
RETENTION WINDOW
 ↓
RECOVERY HISTORY
```
### Important
If an incident is discovered weeks after it occurred, a short retention period may make recovery impossible.
## 19 — EFS Lifecycle vs Backup Retention
These are different concepts.
```text
EFS LIFECYCLE
→ STORAGE CLASS / COST OPTIMIZATION
```
```text
BACKUP RETENTION
→ RECOVERY HISTORY
```
### Interview trap
> "Moving files to an EFS storage class is the same as backing them up."
**Incorrect.**
Lifecycle optimizes storage placement; backup creates recovery points.
Reference: https://docs.aws.amazon.com/efs/latest/ug/lifecycle-management-efs.html
## 20 — EFS Storage Classes
EFS supports storage classes designed for different access patterns.
A typical concept is:
```text
FREQUENTLY ACCESSED
       ↓
INFREQUENT ACCESS
       ↓
ARCHIVE, WHERE APPLICABLE
```
### Interview point
Storage-class lifecycle policies affect how data is stored and accessed; they are not a substitute for backup.
## 21 — EFS Replication
EFS supports replication between file systems in different AWS Regions.
```text
PRIMARY EFS
    │
    ↓
EFS REPLICATION
    │
    ↓
SECONDARY EFS
```
### Why?
- Regional disaster recovery
- Geographic resilience
- Faster recovery than rebuilding from a cold backup in some designs
### Important
Replication and backup solve different problems.
Reference: https://docs.aws.amazon.com/efs/latest/ug/replication.html
## 22 — EFS Replication vs Backup
```text
REPLICATION
→ CURRENT / NEAR-CURRENT COPY
→ REGIONAL RESILIENCE
→ FAST DR OPTIONS
```
```text
BACKUP
→ HISTORICAL RECOVERY POINTS
→ LONGER RETENTION
→ LOGICAL RECOVERY
```
### Strong architecture
```text
REPLICATION
+
BACKUP
+
ISOLATED RECOVERY
```
### Interview answer
> "I use replication for regional resilience and backups for historical recovery; I don't treat one as a replacement for the other."
## 23 — EFS Cross-Region DR
A DR architecture can look like:
```text
PRIMARY REGION
      │
   PRIMARY EFS
      │
      ├──────────────→ BACKUP
      │
      ↓
 EFS REPLICATION
      ↓
SECONDARY REGION
      ↓
SECONDARY EFS
      ↓
APPLICATION RECOVERY
```
### Design questions
- Is the DR EFS already available?
- Are EC2 / containers available?
- Are mount targets configured?
- Are security groups ready?
- Are IAM roles ready?
- Is DNS / application configuration prepared?
## 24 — EFS Backup Cross-Region Copy
AWS Backup can support copying recovery points across Regions for supported resources and configurations.
```text
PRIMARY REGION
      ↓
EFS BACKUP
      ↓
CROSS-REGION COPY
      ↓
BACKUP VAULT
      ↓
DR REGION
```
### Interview point
Cross-Region backup copy provides an independent recovery point, while EFS replication can provide a continuously maintained secondary file system.
Reference: https://docs.aws.amazon.com/aws-backup/latest/devguide/cross-region-backup.html
## 25 — Cross-Account EFS Backup
For stronger isolation:
```text
PRODUCTION ACCOUNT
      ↓
EFS
      ↓
AWS BACKUP
      ↓
BACKUP COPY
      ↓
BACKUP ACCOUNT
      ↓
PROTECTED VAULT
```
### Why?
- Administrative separation
- Ransomware resilience
- Reduced blast radius
- Central governance
### Interview answer
> "For critical EFS data, I consider a dedicated backup account so production compromise does not automatically compromise every recovery copy."
## 26 — Backup Vault Protection
Where supported by the recovery architecture, AWS Backup Vault Lock can provide stronger protection for recovery points.
```text
EFS
 ↓
AWS BACKUP
 ↓
RECOVERY POINT
 ↓
BACKUP VAULT
 ↓
VAULT LOCK
 ↓
PROTECTED RETENTION
```
### Interview answer
> "For immutable backup requirements, I evaluate Vault Lock and carefully validate the retention policy before applying irreversible controls."
Reference: https://docs.aws.amazon.com/aws-backup/latest/devguide/vault-lock.html
## 27 — Ransomware Protection for EFS
A layered design:
```text
                    PRODUCTION
                        │
                       EFS
                        │
                 ┌──────┴──────┐
                 ↓             ↓
              BACKUP       REPLICATION
                 │             │
                 ↓             ↓
          BACKUP ACCOUNT    DR REGION
                 │
            VAULT LOCK
                 │
                 ↓
             RECOVERY
```
### Additional controls
- Least-privilege IAM
- Separate backup administration
- KMS protection
- Monitoring
- Restore testing
### Principle
> **An isolated recovery copy is valuable because a compromised production identity should not control every recovery path.**
## 28 — Scenario: User Deletes Files
### Recovery approach
```text
FILE DELETED
    ↓
IDENTIFY INCIDENT
    ↓
SELECT LAST GOOD BACKUP
    ↓
RESTORE
    ↓
VALIDATE
    ↓
RECOVER FILES
```
### Important
Backup recovery is different from file-system replication.
If the replicated copy has already received the deletion, replication alone may not provide the historical state you need.
## 29 — Scenario: Application Corrupts Files
```text
CORRUPTION
    ↓
IDENTIFY LAST GOOD TIME
    ↓
SELECT RECOVERY POINT
    ↓
RESTORE SEPARATELY
    ↓
COMPARE DATA
    ↓
RECOVER REQUIRED FILES
```
### Interview answer
> "For logical corruption, I use a historical recovery point rather than assuming the current replicated copy is clean."
## 30 — Scenario: EFS Region Fails
### Replication-based recovery
```text
PRIMARY REGION
      X
      │
      ↓
SECONDARY REGION
      ↓
SECONDARY EFS
      ↓
RECOVERY COMPUTE
      ↓
MOUNT
      ↓
APPLICATION
```
### Backup-based recovery
```text
PRIMARY REGION
      X
      │
      ↓
BACKUP COPY
      ↓
DR REGION
      ↓
RESTORE EFS
      ↓
MOUNT
      ↓
APPLICATION
```
### Interview point
Replication may reduce recovery time, while backup provides historical recovery.
## 31 — Scenario: Restore Succeeds but Application Cannot Mount
### Troubleshooting
```text
RESTORE SUCCESS
      ↓
MOUNT FAILURE
      ↓
VPC
      ↓
MOUNT TARGET
      ↓
SECURITY GROUP
      ↓
DNS
      ↓
IAM / ACCESS POINT
      ↓
APPLICATION
```
### Interview answer
> "I troubleshoot the entire access path instead of treating the restore job as the end of recovery."
## 32 — Scenario: EFS Backup Job Fails
### Troubleshooting flow
```text
BACKUP FAILURE
      ↓
RESOURCE STATUS
      ↓
AWS BACKUP CONFIG
      ↓
IAM
      ↓
REGION
      ↓
BACKUP PLAN
      ↓
VAULT
      ↓
EVENT / LOG
```
### Questions
- Is the file system supported?
- Is the resource assigned to the backup plan?
- Is the backup vault available?
- Are required permissions present?
- Did the backup job actually start?
## 33 — Scenario: New EFS Is Not Protected
### Troubleshooting
```text
NEW EFS
  ↓
RESOURCE SELECTION
  ↓
TAGS / ASSIGNMENT
  ↓
BACKUP PLAN
  ↓
SCHEDULE
  ↓
BACKUP JOB
```
### Senior-level point
Automated backup policy should account for new resources so that protection does not depend on manual enrollment.
## 34 — Scenario: RPO Is 1 Hour
### Design
```text
RPO = 1 HOUR
       ↓
BACKUP / RECOVERY STRATEGY
       ↓
RECOVERY POINT AVAILABILITY
```
Evaluate:
- Backup frequency
- Backup completion time
- EFS replication
- Application data-change rate
- Recovery validation
### Interview answer
> "I would validate whether backup frequency alone meets the one-hour RPO and evaluate EFS replication if regional recovery requires a more current copy."
## 35 — Scenario: RTO Is 15 Minutes
A backup-only restore may not meet a strict RTO for a large file system.
### Evaluate
```text
PRE-STAGED DR
      +
EFS REPLICATION
      +
READY COMPUTE
      +
READY NETWORK
      +
READY IAM
```
### Interview answer
> "For a very aggressive RTO, I would evaluate a pre-staged DR architecture instead of relying only on an on-demand backup restore."
## 36 — Scenario: Long-Term Retention
### Requirement
Keep recovery points for years.
### Design
```text
EFS
 ↓
AWS BACKUP
 ↓
LONG-TERM RETENTION
 ↓
BACKUP VAULT
 ↓
OPTIONAL CROSS-ACCOUNT COPY
```
### Consider
- Compliance
- Cost
- Recovery frequency
- Vault protection
- Retention policy
## 37 — Scenario: Production Account Is Compromised
### Recovery design
```text
COMPROMISED PRODUCTION
          ↓
ISOLATED BACKUP ACCOUNT
          ↓
PROTECTED RECOVERY POINT
          ↓
DR REGION
          ↓
RESTORE
          ↓
RECOVERY
```
### Interview answer
> "The recovery account should have independent administrative controls and restricted access from production."
## 38 — EFS Backup Monitoring
Monitor:
```text
BACKUP JOBS
COPY JOBS
RESTORE JOBS
REPLICATION
RESOURCE COVERAGE
```
Useful AWS services:
```text
AWS BACKUP
EVENTBRIDGE
CLOUDWATCH
CLOUDTRAIL
SNS
```
### Interview point
> "I monitor protection coverage and restore operations, not only successful backup jobs."
## 39 — CloudTrail for EFS Investigation
CloudTrail can help investigate AWS API activity.
```text
ADMIN ACTION
     ↓
CLOUDTRAIL
     ↓
AUDIT TRAIL
     ↓
INVESTIGATION
```
Questions:
- Who changed the EFS configuration?
- Who changed access policy?
- Who changed backup settings?
- Who performed administrative operations?
## 40 — EFS Backup Cost Considerations
Cost considerations include:
```text
BACKUP STORAGE
RETENTION
CROSS-REGION COPIES
REPLICATION
DATA TRANSFER
RECOVERY OPERATIONS
```
### Design principle
> "I optimize retention and replication only after confirming the required recovery objectives."
## 41 — EFS Backup vs Replication
| Capability | AWS Backup | EFS Replication |
|---|---|---|
| Historical recovery | Yes | Limited |
| Scheduled recovery points | Yes | No |
| Regional resilience | Copy supported | Yes |
| Fast DR copy | Restore required | Secondary file system |
| Long-term retention | Yes | Not its primary purpose |
| Logical corruption recovery | Stronger with historical points | Current state may also replicate |
| Centralized governance | Yes | No |
### Interview memory
> **Backup = history**
> **Replication = resilience**
## 42 — EFS Backup Architecture Checklist
### Protection
- [ ] EFS inventory
- [ ] AWS Backup plan
- [ ] Backup selection validated
- [ ] Backup frequency defined
### Recovery
- [ ] RPO defined
- [ ] RTO defined
- [ ] Recovery points available
- [ ] Restore runbook
- [ ] Application dependencies documented
### Security
- [ ] IAM least privilege
- [ ] EFS encryption
- [ ] KMS permissions
- [ ] File-system policy reviewed
- [ ] Backup account isolation
### DR
- [ ] Cross-Region backup copy
- [ ] EFS replication evaluated
- [ ] DR compute
- [ ] VPC
- [ ] Mount targets
- [ ] Security groups
### Operations
- [ ] Backup monitoring
- [ ] Restore monitoring
- [ ] Replication monitoring
- [ ] CloudTrail
- [ ] Alerting
### Testing
- [ ] File recovery test
- [ ] Full EFS restore test
- [ ] Mount test
- [ ] Application recovery test
- [ ] RTO measurement
## 43 — EFS Recovery Runbook
```text
1. IDENTIFY INCIDENT
        ↓
2. DETERMINE FAILURE TYPE
        ↓
3. SELECT RECOVERY POINT / DR COPY
        ↓
4. RESTORE OR FAIL OVER
        ↓
5. CONFIGURE MOUNT TARGET
        ↓
6. VALIDATE IAM / SECURITY
        ↓
7. MOUNT EFS
        ↓
8. VALIDATE FILES
        ↓
9. VALIDATE APPLICATION
        ↓
10. MEASURE RTO
```
### Senior-level principle
> **Recovery is not complete until the application can use the recovered file system.**
## 44 — EFS Restore Testing
Test:
- Deleted file recovery
- Corrupted data recovery
- Full file-system restore
- Cross-Region recovery
- Encrypted recovery
- IAM-authorized mount
- Access Point recovery
- Application recovery
### Test model
```text
RECOVERY POINT
      ↓
RESTORE
      ↓
MOUNT
      ↓
VALIDATE
      ↓
APPLICATION TEST
      ↓
MEASURE RTO
```
## 45 — Senior-Level EFS Recovery Design
A mature architecture separates:
```text
FILE DATA
+
BACKUP HISTORY
+
REGIONAL DR
+
ACCOUNT ISOLATION
+
NETWORK ACCESS
+
SECURITY
+
APPLICATION RECOVERY
```
### Reference architecture
```text
                     PRODUCTION
                         │
                        EFS
                         │
                ┌────────┴────────┐
                ↓                 ↓
             BACKUP           REPLICATION
                │                 │
                ↓                 ↓
        BACKUP ACCOUNT       DR REGION
                │                 │
          VAULT PROTECTION     DR EFS
                │                 │
                └────────┬────────┘
                         ↓
                      RECOVERY
                         ↓
                    APPLICATION
```
## 46 — EFS Backup vs EBS Snapshot
A common interview comparison:
```text
EBS
→ BLOCK STORAGE
→ EBS SNAPSHOTS
→ VOLUME-LEVEL RECOVERY
```
```text
EFS
→ MANAGED FILE STORAGE
→ AWS BACKUP
→ FILE-SYSTEM RECOVERY
```
### Interview point
Do not automatically apply EBS snapshot concepts to EFS.
## 47 — EFS Backup vs S3 Backup
```text
EFS
→ SHARED FILE SYSTEM
→ NFS ACCESS
→ APPLICATION MOUNTS
```
```text
S3
→ OBJECT STORAGE
→ OBJECT VERSIONING
→ OBJECT-LEVEL RECOVERY
```
### Interview answer
> "The recovery architecture depends on the storage semantics. EFS is a file system accessed by workloads, while S3 is object storage with different recovery mechanisms."
## 48 — 60-Second EFS Backup Interview Answer
> "For EFS, I use AWS Backup to create centralized, policy-driven recovery points with appropriate retention. I define the backup schedule from the workload's RPO and validate the entire restore process against the RTO. For regional resilience, I evaluate EFS replication and cross-Region backup copies because they solve different problems: replication provides a current secondary file system, while backups provide historical recovery points. I also plan encryption, KMS, IAM, access points, mount targets and security groups as part of the recovery path. For critical workloads, I consider cross-account isolation and protected backup vaults, and I regularly test file recovery and full application recovery."
## 49 — EFS Interview Traps
### Trap 1
> "EFS is durable, so no backup is required."
**Better:** Durability does not protect against every logical or operational failure.
### Trap 2
> "EFS replication is a backup."
**Better:** Replication provides regional resilience; backup provides historical recovery.
### Trap 3
> "Restoring EFS means the application is recovered."
**Better:** Mount targets, networking, IAM and application configuration must also work.
### Trap 4
> "EFS lifecycle is backup retention."
**Better:** Lifecycle manages storage placement; backup retention manages recovery points.
### Trap 5
> "Backup success means recovery is guaranteed."
**Better:** Test restore and application access.
### Trap 6
> "Encryption cannot affect recovery."
**Better:** KMS and IAM can be critical recovery dependencies.
### Trap 7
> "A replicated EFS copy protects against every deletion."
**Better:** Logical changes may propagate; retain historical backup recovery points.
## 50 — Final EFS Mental Model
Memorize:
```text
EFS
│
├── AWS BACKUP
│     ↓
│   RECOVERY HISTORY
│
├── RETENTION
│     ↓
│   LONGER RECOVERY WINDOW
│
├── REPLICATION
│     ↓
│   REGIONAL DR
│
├── CROSS-ACCOUNT
│     ↓
│   ADMINISTRATIVE ISOLATION
│
├── KMS / IAM
│     ↓
│   SECURE ACCESS
│
├── MOUNT TARGETS
│     ↓
│   NETWORK ACCESS
│
└── RESTORE TEST
      ↓
   PROVEN RECOVERY
```
### Final principle
> **EFS protection is not just about restoring files; it is about restoring a usable file system and reconnecting the application within the required RPO and RTO.**
## Official AWS References
- [Amazon EFS User Guide](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html)
- [AWS Backup Supported Services](https://docs.aws.amazon.com/aws-backup/latest/devguide/working-with-supported-services.html)
- [AWS Backup Restore EFS](https://docs.aws.amazon.com/aws-backup/latest/devguide/restoring-efs.html)
- [AWS Backup Cross-Region Backup](https://docs.aws.amazon.com/aws-backup/latest/devguide/cross-region-backup.html)
- [AWS Backup Vault Lock](https://docs.aws.amazon.com/aws-backup/latest/devguide/vault-lock.html)
- [EFS Accessing File Systems](https://docs.aws.amazon.com/efs/latest/ug/accessing-fs.html)
- [EFS Access Points](https://docs.aws.amazon.com/efs/latest/ug/efs-access-points.html)
- [EFS Encryption](https://docs.aws.amazon.com/efs/latest/ug/encryption.html)
- [EFS Replication](https://docs.aws.amazon.com/efs/latest/ug/replication.html)
- [EFS Lifecycle Management](https://docs.aws.amazon.com/efs/latest/ug/lifecycle-management-efs.html)
