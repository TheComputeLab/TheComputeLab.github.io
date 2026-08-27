---
title: " 🖥️ EC2 Backup & Recovery"
description: "Interview-focused Amazon EC2 backup and recovery covering EBS snapshots, AMIs, AWS Backup, lifecycle, encryption, cross-Region recovery, cross-account protection, restore workflows, DR and troubleshooting."
weight: 80
toc: true
---

Amazon EC2 recovery is primarily about protecting the **data stored on attached EBS volumes**, preserving the configuration needed to recreate the instance, and designing a recovery process that satisfies the workload's RPO and RTO.
## 01 — EC2 Backup Mental Model
Think about EC2 protection as multiple layers:
```text
EC2 INSTANCE
     │
     ├── INSTANCE CONFIGURATION
     │
     └── EBS VOLUMES
             │
             ↓
       SNAPSHOT / BACKUP
             │
       ┌─────┴─────┐
       ↓           ↓
    PRIMARY       COPY
    REGION       REGION / ACCOUNT
       │           │
       └─────┬─────┘
             ↓
          RESTORE
             ↓
        NEW EC2 INSTANCE
```
### Core principle
> **Protecting an EC2 workload means protecting both its data and the configuration required to rebuild the service.**
## 02 — What Should Be Backed Up?
An EC2 workload can contain:
```text
EC2
│
├── EBS ROOT VOLUME
├── EBS DATA VOLUMES
├── INSTANCE CONFIGURATION
├── SECURITY GROUPS
├── IAM ROLE
├── NETWORK CONFIGURATION
├── APPLICATION CONFIGURATION
└── EXTERNAL DEPENDENCIES
```
### Important
An EBS snapshot primarily protects the contents of the EBS volume. It does not automatically recreate every surrounding application dependency.
### Interview answer
> "I identify both the storage data and the infrastructure dependencies needed to rebuild the application."
## 03 — EBS Snapshots
An EBS snapshot is a point-in-time backup of an EBS volume.
```text
EBS VOLUME
    ↓
SNAPSHOT
    ↓
RECOVERY POINT
    ↓
NEW EBS VOLUME
```
Snapshots are stored in Amazon S3-managed infrastructure, although users do not directly manage the underlying S3 objects.
### Interview phrase
> **"EBS snapshots give me point-in-time recovery for EBS volume data."**
Reference: https://docs.aws.amazon.com/ebs/latest/userguide/ebs-snapshots.html
## 04 — Incremental Snapshot Concept
EBS snapshots after the first snapshot are incremental at the storage level.
```text
SNAPSHOT 1
FULL INITIAL BASELINE
        ↓
SNAPSHOT 2
CHANGED BLOCKS
        ↓
SNAPSHOT 3
NEW CHANGED BLOCKS
```
### Important
Even though snapshots are incremental internally, each snapshot represents the state of the volume at a particular point in time.
### Interview answer
> "EBS snapshots are incremental after the initial snapshot, which can make repeated protection efficient."
## 05 — Crash-Consistent vs Application-Consistent
A snapshot captures storage at a point in time, but application consistency depends on how the workload is handled.
### Crash-consistent
```text
APPLICATION RUNNING
       ↓
SNAPSHOT
       ↓
STORAGE STATE
```
This is similar to what happens when a system loses power unexpectedly.
### Application-consistent
```text
QUIESCE / FLUSH APPLICATION
          ↓
       SNAPSHOT
          ↓
CONSISTENT RECOVERY POINT
```
### Interview answer
> "For databases and transactional applications, I determine whether crash consistency is sufficient or whether application-aware quiescing is required."
## 06 — AWS Backup for EC2
AWS Backup can provide centralized policy-driven backup management for supported EC2 resources.
```text
EC2
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
- Centralized policies
- Scheduling
- Retention
- Vault management
- Copy operations
- Monitoring
### Interview answer
> "I use AWS Backup when centralized governance and policy management are more important than managing individual EC2 snapshots manually."
Reference: https://docs.aws.amazon.com/aws-backup/latest/devguide/working-with-supported-services.html
## 07 — AWS Backup vs Manual EBS Snapshots
| Approach | AWS Backup | Manual / Native EBS Snapshots |
|---|---|---|
| Centralized policy | Strong | Requires custom management |
| Scheduling | Built-in policy model | Automation required |
| Vault governance | Yes | EBS snapshot management |
| Cross-account governance | Supported capabilities | Requires design |
| Cross-Region copies | Supported capabilities | Supported with snapshot copy |
| Monitoring | Centralized | Requires additional tooling |
### Interview answer
> "I choose based on governance requirements, resource scope and operational complexity rather than assuming one mechanism is always better."
## 08 — AMI vs EBS Snapshot
This is a common interview question.
### EBS snapshot
Protects:
```text
EBS VOLUME DATA
```
### AMI
Represents:
```text
IMAGE / INSTANCE LAUNCH CONFIGURATION
        +
EBS SNAPSHOTS FOR MAPPED EBS VOLUMES
```
### Simple distinction
```text
SNAPSHOT
→ VOLUME DATA

AMI
→ IMAGE / LAUNCHABLE INSTANCE TEMPLATE
```
### Interview answer
> "I use snapshots when the primary requirement is volume-data recovery, while an AMI can be useful when I need a reusable image for launching an EC2 instance."
Reference: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html
## 09 — AMI Is Not a Complete Application Backup
An AMI does not automatically preserve every external dependency.
```text
AMI
├── OS / IMAGE
├── MAPPED EBS VOLUME SNAPSHOTS
└── INSTANCE LAUNCH INFORMATION
```
It does not magically recreate:
- External databases
- DNS
- Secrets
- External load balancers
- Network dependencies
- Application data stored elsewhere
### Interview point
> **"An AMI is an infrastructure recovery artifact, not automatically a complete application disaster-recovery solution."**
## 10 — EC2 Backup Architecture
A resilient architecture:
```text
                  PRODUCTION EC2
                       │
                ┌──────┴──────┐
                ↓             ↓
             EBS ROOT      EBS DATA
                │             │
                └──────┬──────┘
                       ↓
                  AWS BACKUP
                       ↓
                  BACKUP PLAN
                       ↓
                  RECOVERY POINT
                       ↓
                  PRIMARY VAULT
                       │
              ┌────────┴────────┐
              ↓                 ↓
       CROSS-REGION COPY   CROSS-ACCOUNT COPY
              ↓                 ↓
          DR REGION        BACKUP ACCOUNT
```
### Goal
Protect both data and the recovery boundary.
## 11 — Backup Vault Strategy for EC2
A vault strategy can separate:
```text
PRODUCTION BACKUPS
       ↓
PROD VAULT
```
```text
LONG-TERM / COMPLIANCE
       ↓
COMPLIANCE VAULT
```
```text
ISOLATED COPY
       ↓
BACKUP ACCOUNT VAULT
```
### Design principle
Use separation when there is a meaningful security, compliance or operational reason.
## 12 — Cross-Region EC2 Recovery
A snapshot or backup copy can be used in another Region where supported.
```text
PRIMARY REGION
      ↓
EBS BACKUP
      ↓
COPY
      ↓
DR REGION
      ↓
RESTORE EBS
      ↓
LAUNCH EC2
```
### Important
The target Region must have all required recovery dependencies available.
### Interview answer
> "Cross-Region data protection is only one part of EC2 DR; I also need the networking, IAM, security groups, images and application dependencies required to launch the service."
Reference: https://docs.aws.amazon.com/ebs/latest/userguide/ebs-copy-snapshot.html
## 13 — Cross-Account EC2 Backup
For stronger isolation:
```text
PRODUCTION ACCOUNT
        ↓
EC2
        ↓
BACKUP
        ↓
COPY
        ↓
BACKUP ACCOUNT
        ↓
PROTECTED BACKUP
```
### Why?
A compromised production account should not automatically provide unrestricted control over every recovery copy.
### Interview answer
> "For critical workloads I consider a dedicated backup account with restricted administrative access."
## 14 — Snapshot Encryption
EBS snapshots can be encrypted.
```text
EBS VOLUME
    ↓
ENCRYPTED SNAPSHOT
    ↓
KMS
    ↓
PROTECTED DATA
```
### Important
KMS permissions can become part of the recovery path.
### Interview questions
- Which KMS key protects the data?
- Does the recovery identity have access?
- Is the key enabled?
- Can the snapshot be copied or restored in the target account/Region?
Reference: https://docs.aws.amazon.com/ebs/latest/userguide/ebs-encryption.html
## 15 — Encrypted Snapshot Copy
When copying encrypted EBS snapshots across Regions or accounts, encryption and KMS permissions must be planned carefully.
```text
SOURCE SNAPSHOT
      ↓
COPY
      ↓
TARGET KMS KEY
      ↓
TARGET SNAPSHOT
```
### Interview answer
> "For encrypted cross-account or cross-Region recovery, I validate both snapshot permissions and KMS key permissions before the incident."
## 16 — EBS Snapshot Lifecycle
EBS snapshots can be managed with lifecycle automation.
```text
SNAPSHOT
   ↓
RETENTION POLICY
   ↓
LIFECYCLE
   ↓
EXPIRATION
```
Amazon Data Lifecycle Manager can automate lifecycle management for supported EBS snapshots.
### Interview answer
> "For native EBS snapshot lifecycle management, I evaluate Amazon Data Lifecycle Manager; for centralized backup governance across supported resources, I evaluate AWS Backup."
Reference: https://docs.aws.amazon.com/ebs/latest/userguide/snapshot-lifecycle.html
## 17 — AWS Backup Lifecycle
AWS Backup can apply lifecycle policies to eligible recovery points.
```text
BACKUP
  ↓
RETENTION
  ↓
LIFECYCLE TRANSITION
  ↓
EXPIRATION
```
### Design principle
Choose lifecycle based on:
- Recovery requirements
- Retention
- Cost
- Resource support
## 18 — EC2 Backup Frequency
Frequency should be derived from RPO.
```text
RPO
 ↓
BACKUP FREQUENCY
 ↓
RECOVERY POINTS
```
### Example
```text
RPO = 24 HOURS
→ DAILY BACKUP MAY BE APPROPRIATE

RPO = 4 HOURS
→ MORE FREQUENT PROTECTION MAY BE REQUIRED
```
### Interview answer
> "I don't select a backup schedule first. I derive the schedule from the workload's RPO."
## 19 — EC2 RTO
RTO includes more than restoring an EBS volume.
```text
RECOVERY POINT
      ↓
CREATE / RESTORE VOLUME
      ↓
LAUNCH INSTANCE
      ↓
NETWORK CONFIGURATION
      ↓
IAM / SECURITY
      ↓
APPLICATION START
      ↓
VALIDATION
      ↓
SERVICE AVAILABLE
```
### Interview point
> **"The restore duration of the EBS volume is only one component of the EC2 RTO."**
## 20 — EC2 Restore Workflow
A typical restore:
```text
1. IDENTIFY INCIDENT
        ↓
2. SELECT RECOVERY POINT
        ↓
3. RESTORE EBS DATA
        ↓
4. CREATE / LAUNCH EC2
        ↓
5. APPLY NETWORK + SECURITY
        ↓
6. CONFIGURE APPLICATION
        ↓
7. VALIDATE DATA
        ↓
8. VALIDATE APPLICATION
        ↓
9. RETURN TO SERVICE
```
### Interview answer
> "I treat EC2 recovery as a service recovery workflow rather than simply restoring a disk."
## 21 — Restoring From an EBS Snapshot
```text
EBS SNAPSHOT
     ↓
CREATE NEW EBS VOLUME
     ↓
ATTACH TO EC2
     ↓
MOUNT / VALIDATE
     ↓
APPLICATION RECOVERY
```
### Important
The target volume type, availability zone and filesystem/application requirements must be considered.
## 22 — Restoring the Root Volume
If the root volume is damaged:
```text
RECOVERY POINT
      ↓
RESTORE ROOT DATA
      ↓
CREATE / ATTACH VOLUME
      ↓
BOOT EC2
      ↓
VALIDATE OS
```
### Alternative
For some scenarios, launching a new instance from an appropriate AMI may be simpler.
### Interview answer
> "I choose between volume-level recovery and rebuilding from an AMI based on the failure and recovery objective."
## 23 — Full EC2 Instance Recovery
A full recovery may require:
```text
AMI / INSTANCE CONFIG
        +
EBS DATA
        +
NETWORK
        +
IAM
        +
SECURITY GROUPS
        +
APPLICATION CONFIG
```
### Architecture
```text
BACKUP
 ↓
RESTORE DATA
 ↓
REBUILD INSTANCE
 ↓
RECONNECT DEPENDENCIES
 ↓
VALIDATE
```
### Senior-level insight
> "I document the infrastructure dependencies separately from the backup data."
## 24 — Application-Consistent EC2 Backups
For databases:
```text
APPLICATION
    ↓
QUIESCE / FLUSH
    ↓
BACKUP
    ↓
RESUME
```
Depending on the application, use application-aware backup mechanisms or database-native recovery features where appropriate.
### Interview answer
> "For transactional workloads, I determine whether storage-level crash consistency is enough or whether application-aware backup is required."
## 25 — Database on EC2
A database hosted on EC2 should not automatically be treated like a generic filesystem.
```text
DATABASE
   ↓
APPLICATION CONSISTENCY
   ↓
BACKUP
   ↓
RECOVERY
```
### Consider
- Database-native backups
- Transaction logs
- Point-in-time recovery
- Application consistency
- Recovery sequence
### Interview answer
> "I evaluate the database's native recovery mechanisms alongside EC2 and EBS protection."
## 26 — Ransomware Protection for EC2
A resilient design:
```text
EC2
 ↓
BACKUP
 ↓
CROSS-ACCOUNT COPY
 ↓
ISOLATED BACKUP ACCOUNT
 ↓
VAULT PROTECTION
 ↓
CROSS-REGION COPY
 ↓
RECOVERY
```
### Additional controls
- Least-privilege IAM
- Encryption
- Vault Lock where applicable
- Restricted backup administration
- Monitoring
- Restore testing
### Principle
> **Do not allow production compromise to automatically destroy every recovery copy.**
## 27 — EC2 Backup and Vault Lock
Where AWS Backup is used, Vault Lock can add protection to recovery points.
```text
EC2
 ↓
AWS BACKUP
 ↓
RECOVERY POINT
 ↓
VAULT
 ↓
VAULT LOCK
```
### Interview answer
> "For critical EC2 backups with immutability requirements, I evaluate Vault Lock and ensure retention requirements are fully understood before enabling irreversible controls."
Reference: https://docs.aws.amazon.com/aws-backup/latest/devguide/vault-lock.html
## 28 — Backup Coverage for EC2
A healthy backup job does not prove that every EC2 instance is protected.
```text
EC2 INVENTORY
      ↓
EXPECTED PROTECTION
      ↓
ACTUAL ASSIGNMENT
      ↓
BACKUP JOB
      ↓
COVERAGE
```
### Common gap
```text
NEW EC2
  ↓
NO BACKUP TAG
  ↓
NO BACKUP SELECTION
  ↓
UNPROTECTED
```
### Interview answer
> "I monitor backup coverage and not just backup job success."
## 29 — EC2 Backup Monitoring
Monitor:
```text
BACKUP JOBS
COPY JOBS
RESTORE JOBS
SNAPSHOT OPERATIONS
CONFIGURATION
```
Use appropriate AWS services such as:
```text
EVENTBRIDGE
CLOUDWATCH
CLOUDTRAIL
SNS
```
### Interview point
> "A mature design turns backup failures into operational alerts."
## 30 — CloudTrail for EC2 Backup Investigation
CloudTrail can help identify administrative actions.
```text
ADMIN ACTION
     ↓
CLOUDTRAIL
     ↓
AUDIT
     ↓
INVESTIGATION
```
### Questions
- Who changed the backup policy?
- Who deleted or modified a resource?
- Who changed IAM permissions?
- Who changed KMS configuration?
## 31 — Scenario: EC2 Instance Deleted
### Recovery approach
```text
INSTANCE DELETED
      ↓
RECOVERY POINT
      ↓
RESTORE EBS / IMAGE
      ↓
REBUILD INSTANCE
      ↓
RECONNECT NETWORK
      ↓
VALIDATE APPLICATION
```
### Important
Deleting the instance does not necessarily mean the protected EBS recovery point is gone.
The exact recovery path depends on what was protected.
## 32 — Scenario: Root Volume Corrupted
### Approach
```text
CORRUPTED ROOT
      ↓
SELECT LAST GOOD RECOVERY POINT
      ↓
RESTORE ROOT DATA
      ↓
BOOT / REBUILD
      ↓
APPLICATION VALIDATION
```
### Interview point
If application configuration is stored elsewhere, recover that dependency too.
## 33 — Scenario: Data Volume Deleted
### Recovery
```text
DATA VOLUME DELETED
        ↓
EBS SNAPSHOT / BACKUP
        ↓
CREATE NEW VOLUME
        ↓
ATTACH
        ↓
MOUNT
        ↓
VALIDATE DATA
```
### Interview answer
> "I restore the data volume from the most appropriate recovery point and validate the filesystem and application."
## 34 — Scenario: EC2 Region Is Unavailable
### Architecture
```text
PRIMARY REGION
      X
      │
      ↓
DR REGION
      ↓
RECOVERY DATA
      ↓
INSTANCE REBUILD
      ↓
NETWORK / IAM
      ↓
APPLICATION
```
### Interview answer
> "I need both a recoverable copy of the data and the infrastructure definition required to rebuild the EC2 service in the DR Region."
## 35 — Scenario: Backup Job Is Successful but Recovery Fails
### Diagnosis
```text
BACKUP SUCCESS
      ↓
RECOVERY POINT
      ↓
RESTORE
      ↓
FAILURE
```
Check:
- Recovery point
- Snapshot state
- IAM
- KMS
- Target Region
- Volume configuration
- Instance configuration
- Application dependencies
### Interview answer
> "Backup success proves the protection operation completed; it does not prove the workload can be fully recovered."
## 36 — Scenario: Encrypted Backup Cannot Be Restored
### Troubleshooting
```text
RESTORE FAILURE
      ↓
CHECK KMS KEY
      ↓
CHECK IAM
      ↓
CHECK KEY POLICY
      ↓
CHECK ACCOUNT / REGION
      ↓
RETRY RECOVERY
```
### Senior-level point
Encryption must be tested as part of the recovery process.
## 37 — Scenario: Production Account Is Compromised
### Desired architecture
```text
COMPROMISED PROD ACCOUNT
          ↓
ISOLATED COPY
          ↓
BACKUP ACCOUNT
          ↓
PROTECTED VAULT
          ↓
DR REGION
          ↓
RESTORE
```
### Interview answer
> "The backup copy should have independent administrative controls so that compromise of production does not automatically compromise the recovery boundary."
## 38 — Scenario: RPO Is 1 Hour
### Design
```text
RPO = 1 HOUR
      ↓
PROTECTION FREQUENCY
      ↓
RECOVERY POINTS
```
Validate:
- Backup frequency
- Resource support
- Backup completion time
- Copy completion time
- Recovery-point availability
### Interview point
The schedule alone is not enough; the recovery point must actually be available within the required objective.
## 39 — Scenario: RTO Is 30 Minutes
### Design questions
- Is the recovery Region ready?
- Are AMIs or launch artifacts available?
- Are EBS recovery points available?
- Is networking pre-created?
- Are IAM roles ready?
- Are security groups ready?
- Is DNS ready?
- Has the complete restore process been tested?
### Strong answer
> "For a 30-minute RTO, I would pre-stage the recovery environment and validate the entire rebuild process rather than relying on an untested manual recovery."
## 40 — Scenario: Snapshot Storage Cost Is Increasing
### Investigation
```text
COST INCREASE
     ↓
SNAPSHOT COUNT
     ↓
DATA CHANGE RATE
     ↓
RETENTION
     ↓
LIFECYCLE
```
### Actions
- Review retention
- Remove unnecessary snapshots according to policy
- Use lifecycle automation
- Check high-change volumes
- Validate that cost reduction does not violate RPO/RTO
## 41 — Scenario: New EC2 Instances Are Not Being Backed Up
### Troubleshooting
```text
NEW EC2
   ↓
TAGS
   ↓
BACKUP SELECTION
   ↓
BACKUP PLAN
   ↓
SERVICE / REGION SUPPORT
   ↓
SCHEDULE
```
### Likely causes
- Missing tag
- Incorrect tag
- Incorrect resource selection
- Backup plan mismatch
- Service opt-in issue where applicable
- Schedule has not executed
## 42 — Scenario: AMI Exists but Application Cannot Recover
### Diagnosis
```text
AMI
 ↓
INSTANCE BOOTS
 ↓
APPLICATION FAILS
```
Investigate:
- External database
- Secrets
- Network configuration
- DNS
- IAM
- Security groups
- Application data
### Interview answer
> "The AMI restored the instance image, but the application recovery chain was incomplete."
## 43 — Scenario: EC2 Backup Must Be Immutable
### Design
```text
EC2
 ↓
AWS BACKUP
 ↓
RECOVERY POINT
 ↓
BACKUP VAULT
 ↓
VAULT LOCK
 ↓
RESTRICTED ADMINISTRATION
```
### Interview answer
> "I would evaluate Vault Lock for immutable retention and separate the backup administration from production administration."
## 44 — EC2 Backup Architecture Checklist
### Protection
- [ ] EC2 workload inventory
- [ ] Root EBS volumes identified
- [ ] Data EBS volumes identified
- [ ] Backup policy defined
- [ ] Resource selection validated
### Recovery
- [ ] RPO defined
- [ ] RTO defined
- [ ] Recovery points available
- [ ] Restore runbook
- [ ] Application dependencies documented
### Security
- [ ] IAM least privilege
- [ ] EBS encryption
- [ ] KMS permissions
- [ ] Cross-account isolation
- [ ] Vault Lock where required
### DR
- [ ] Cross-Region copy
- [ ] Recovery Region dependencies
- [ ] AMI / launch configuration
- [ ] Network configuration
- [ ] IAM roles
- [ ] Security groups
### Operations
- [ ] Backup monitoring
- [ ] Copy monitoring
- [ ] Restore monitoring
- [ ] CloudTrail
- [ ] Alerting
### Testing
- [ ] Volume restore test
- [ ] Instance recovery test
- [ ] Application recovery test
- [ ] RTO measurement
## 45 — EC2 Backup vs AMI Quick Revision
```text
EBS SNAPSHOT
→ VOLUME DATA
→ POINT-IN-TIME RECOVERY
→ RESTORE VOLUME

AMI
→ IMAGE
→ LAUNCHABLE INSTANCE TEMPLATE
→ EBS SNAPSHOTS FOR MAPPED VOLUMES

AWS BACKUP
→ CENTRALIZED POLICY
→ SCHEDULE
→ RETENTION
→ VAULT
→ COPY
→ GOVERNANCE
```
### Interview memory trick
> **Snapshot = data**
> **AMI = launch**
> **AWS Backup = policy**
## 46 — EC2 Recovery Failure-Domain Model
```text
FAILURE
│
├── VOLUME FAILURE
│      ↓
│   EBS SNAPSHOT / BACKUP
│
├── INSTANCE FAILURE
│      ↓
│   AMI + EBS RECOVERY
│
├── REGION FAILURE
│      ↓
│   CROSS-REGION COPY
│
├── ACCOUNT COMPROMISE
│      ↓
│   CROSS-ACCOUNT COPY
│
├── MALICIOUS DELETION
│      ↓
│   VAULT PROTECTION
│
└── APPLICATION FAILURE
       ↓
    APPLICATION-AWARE RECOVERY
```
## 47 — Senior-Level EC2 Recovery Design
A mature architecture separates:
```text
DATA
+
INFRASTRUCTURE
+
SECURITY
+
NETWORK
+
APPLICATION
+
RECOVERY AUTOMATION
```
### Design
```text
                 EC2 APPLICATION
                       │
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
      EBS            AMI /          CONFIG
    BACKUP          LAUNCH          + IAM
        │             │              │
        └─────────────┼──────────────┘
                      ↓
                RECOVERY PLAN
                      ↓
             DR / ISOLATED ACCOUNT
                      ↓
                  VALIDATION
```
### Senior-level answer
> "I separate data protection from infrastructure reconstruction. The backup protects the data, while infrastructure definitions, IAM, networking and application configuration make the recovery usable."
## 48 — 60-Second EC2 Backup Interview Answer
> "For EC2, I first identify the EBS volumes and the infrastructure dependencies required to recover the workload. I use AWS Backup when centralized policy, scheduling, retention, vault management and governance are required, while EBS snapshots and AMIs remain important recovery artifacts. I design backup frequency around the RPO and validate the complete recovery process against the RTO. For critical workloads, I consider cross-account and cross-Region copies, encryption, KMS permissions, least-privilege IAM and Vault Lock where immutability is required. Finally, I monitor backup and restore operations and regularly test both volume recovery and full application recovery."
## 49 — EC2 Interview Traps
### Trap 1
> "An EC2 snapshot backs up the whole EC2 instance."
**Better:** An EBS snapshot protects EBS volume data.
### Trap 2
> "An AMI is a complete application backup."
**Better:** An AMI is an image/launch artifact and does not automatically recover external dependencies.
### Trap 3
> "Snapshot success means EC2 recovery is guaranteed."
**Better:** Test the full instance and application recovery path.
### Trap 4
> "Cross-Region snapshot copy equals complete DR."
**Better:** Rebuild networking, IAM, security and application dependencies too.
### Trap 5
> "Encrypted backups will always restore."
**Better:** Validate KMS and IAM access during recovery testing.
### Trap 6
> "More snapshots always means better protection."
**Better:** Balance RPO, retention, recovery needs and cost.
### Trap 7
> "AWS Backup and EBS snapshots are identical."
**Better:** AWS Backup adds centralized policy and governance around supported resources.
## 50 — Final EC2 Mental Model
Memorize:
```text
EC2
│
├── EBS
│    ↓
│  SNAPSHOT / BACKUP
│
├── AMI
│    ↓
│  INSTANCE REBUILD
│
├── AWS BACKUP
│    ↓
│  POLICY + VAULT + RETENTION
│
├── CROSS-REGION
│    ↓
│  REGIONAL RESILIENCE
│
├── CROSS-ACCOUNT
│    ↓
│  ADMINISTRATIVE ISOLATION
│
├── KMS / IAM
│    ↓
│  SECURE RECOVERY
│
└── RESTORE TEST
     ↓
   PROVEN RECOVERY
```
### Final principle
> **EC2 backup is successful only when the protected data and the surrounding infrastructure can be recovered within the business requirement.**
## Official AWS References
- [Amazon EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)
- [Amazon EBS Snapshots](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-snapshots.html)
- [Amazon EBS Encryption](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-encryption.html)
- [Copy an Amazon EBS Snapshot](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-copy-snapshot.html)
- [Amazon EBS Snapshot Lifecycle](https://docs.aws.amazon.com/ebs/latest/userguide/snapshot-lifecycle.html)
- [Amazon Machine Images](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html)
- [AWS Backup Supported Services](https://docs.aws.amazon.com/aws-backup/latest/devguide/working-with-supported-services.html)
- [AWS Backup Vault Lock](https://docs.aws.amazon.com/aws-backup/latest/devguide/vault-lock.html)
- [AWS Backup Monitoring](https://docs.aws.amazon.com/aws-backup/latest/devguide/monitoring.html)
