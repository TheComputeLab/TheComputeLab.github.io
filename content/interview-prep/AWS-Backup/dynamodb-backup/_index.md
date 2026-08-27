---
title: " 🗃️ DynamoDB Backup"
description: "Interview-focused Amazon DynamoDB backup and recovery covering point-in-time recovery, on-demand backups, AWS Backup, restore workflows, retention, cross-Region recovery, encryption, Global Tables, RPO/RTO and troubleshooting."
weight: 100
toc: true
---

Amazon DynamoDB provides managed backup and recovery capabilities designed to protect NoSQL table data without requiring traditional database backup servers. For interviews, clearly distinguish **Point-in-Time Recovery, on-demand backups, AWS Backup, Global Tables and application-level recovery**.
## 01 — DynamoDB Backup Mental Model
```text
DYNAMODB TABLE
      │
      ├── POINT-IN-TIME RECOVERY
      │        ↓
      │   CONTINUOUS RECOVERY WINDOW
      │
      ├── ON-DEMAND BACKUP
      │        ↓
      │   FIXED RECOVERY POINT
      │
      └── AWS BACKUP
               ↓
          CENTRAL GOVERNANCE
               ↓
       CROSS-REGION / ACCOUNT
               ↓
             RESTORE
```
### Core principle
> **PITR provides continuous recovery within its configured recovery window, while on-demand backups provide controlled recovery points that can be retained independently.**
## 02 — DynamoDB Point-in-Time Recovery
Point-in-Time Recovery, or PITR, provides continuous backups for a DynamoDB table and allows recovery to a point in time within the configured recovery window.
```text
TABLE
  ↓
CONTINUOUS BACKUP
  ↓
RECOVERY WINDOW
  ↓
SELECT TIME
  ↓
RESTORE
```
### Why it matters
PITR helps recover from:
- Accidental writes
- Accidental deletes
- Application bugs
- Data corruption caused by logical changes
### Interview answer
> "For a logical data incident, I would use DynamoDB PITR to restore the table to a point before the incident, provided that point is within the recovery window."
Reference: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/PointInTimeRecovery.html
## 03 — DynamoDB PITR Recovery Window
PITR maintains a rolling recovery window rather than an unlimited history.
```text
OLD
 │
 ├── OUTSIDE WINDOW → NOT AVAILABLE
 │
 └── RECENT HISTORY
       ↓
   AVAILABLE FOR PITR
```
### Interview point
Retention and RPO are related but different:
- **RPO** = acceptable amount of recent data loss
- **Recovery window** = how far back PITR can recover
### Design principle
Set and review the recovery window according to business and compliance requirements.
## 04 — PITR Is Not a Snapshot
A common interview question:
> "Is DynamoDB PITR the same as an on-demand backup?"
**No.**
```text
PITR
→ CONTINUOUS RECOVERY WINDOW
```
```text
ON-DEMAND BACKUP
→ FIXED RECOVERY POINT
```
### Interview memory
> **PITR = recover to a selected time**
> **On-demand backup = preserve a selected backup**
## 05 — DynamoDB On-Demand Backup
On-demand backups create a backup of a DynamoDB table at a specific point in time.
```text
TABLE
  ↓
ON-DEMAND BACKUP
  ↓
FIXED RECOVERY POINT
  ↓
RESTORE
```
### Use cases
- Long-term retention
- Pre-migration protection
- Pre-change backup
- Compliance workflows
- Controlled recovery point
### Interview answer
> "I use an on-demand backup when I need a deliberate, fixed recovery point rather than recovery to an arbitrary time."
Reference: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/BackupRestore.html
## 06 — On-Demand Backup Retention
On-demand backups can be retained independently of the PITR recovery window.
```text
PITR
→ ROLLING WINDOW
```
```text
ON-DEMAND BACKUP
→ CONTROLLED RETENTION
```
### Interview point
Use on-demand backups for recovery points that need to exist beyond the operational PITR window, subject to the service's current retention capabilities and policies.
## 07 — DynamoDB Restore
A DynamoDB backup is restored into a table.
```text
RECOVERY POINT
      ↓
RESTORE
      ↓
NEW DYNAMODB TABLE
      ↓
VALIDATE
      ↓
APPLICATION CUTOVER
```
### Important
Restore is not simply an in-place rollback of the production table.
### Interview answer
> "I restore the recovery point into a new table, validate it, and then decide how the application should transition to the recovered data."
Reference: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Restore.Tutorial.html
## 08 — Restore to a New Table
A safe recovery pattern:
```text
PRODUCTION TABLE
      │
      X INCIDENT
      │
      ↓
RECOVERY POINT
      ↓
RESTORE
      ↓
RECOVERED TABLE
      ↓
VALIDATE
      ↓
APPLICATION
```
### Why?
- Allows validation before cutover
- Protects the original table
- Supports selective recovery workflows
- Makes incident investigation easier
## 09 — DynamoDB Restore Is More Than Data
After restoring a table, evaluate application dependencies such as:
```text
TABLE
+
IAM
+
KMS
+
APPLICATION CONFIG
+
TABLE NAME / ARN
+
STREAMS
+
GLOBAL SECONDARY INDEXES
+
LOCAL SECONDARY INDEXES
+
AUTOMATION
```
### Interview point
> **"I treat the DynamoDB table as one component of the application's recovery dependency graph."**
## 10 — DynamoDB Global Secondary Indexes
Indexes are part of the table design.
```text
TABLE
 ├── PRIMARY KEY
 ├── GSI
 └── LSI
```
### Recovery consideration
Confirm that required indexes are present and usable after restore.
### Interview answer
> "I validate the restored table's key schema and required indexes before application cutover."
## 11 — DynamoDB Streams
DynamoDB Streams capture item-level modifications for a limited stream-retention period.
```text
TABLE
  ↓
DYNAMODB STREAM
  ↓
CHANGE EVENTS
```
### Important
A DynamoDB Stream is not a replacement for backup.
```text
STREAM
→ CHANGE EVENTS
```
```text
BACKUP
→ RECOVERY DATA
```
### Interview trap
> "DynamoDB Streams are backups."
**Incorrect.**
Reference: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html
## 12 — AWS Backup for DynamoDB
AWS Backup can centrally manage supported DynamoDB backup workflows.
```text
DYNAMODB
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
- Cross-Region copies
- Cross-account governance where supported
### Interview answer
> "I use AWS Backup when I need DynamoDB protection to follow centralized organizational backup policies."
Reference: https://docs.aws.amazon.com/aws-backup/latest/devguide/working-with-supported-services.html
## 13 — Native DynamoDB Backup vs AWS Backup
| Capability | DynamoDB Native | AWS Backup |
|---|---|---|
| PITR | Yes | Can govern supported protection |
| On-demand backup | Yes | Centralized backup management |
| Central policy | Service-level | Strong centralized governance |
| Backup vault | No | Yes |
| Cross-account strategy | Requires design | Centralized capabilities |
| Multi-service policy | No | Yes |
### Interview answer
> "I understand DynamoDB native recovery first, then use AWS Backup for centralized governance and multi-service policy management."
## 14 — DynamoDB Backup Frequency
PITR is not configured like a traditional hourly snapshot schedule.
```text
PITR
 ↓
CONTINUOUS BACKUP
 ↓
RECOVERY WINDOW
```
On-demand backups are explicit recovery points:
```text
REQUEST
 ↓
ON-DEMAND BACKUP
 ↓
FIXED RECOVERY POINT
```
### Interview point
This distinction is often tested in interviews.
## 15 — DynamoDB RPO
RPO defines acceptable data loss.
For DynamoDB:
```text
BUSINESS RPO
      ↓
PITR / BACKUP / REPLICATION
      ↓
RECOVERY POINT
```
### Example
```text
LOGICAL CORRUPTION
      ↓
PITR
      ↓
RECOVER TO BEFORE INCIDENT
```
For regional resilience:
```text
REGIONAL FAILURE
      ↓
GLOBAL TABLES / DR DESIGN
```
### Interview answer
> "I select the recovery mechanism based on the failure domain and the required RPO."
## 16 — DynamoDB RTO
RTO is the time required to make the recovered data usable.
```text
INCIDENT
   ↓
SELECT RECOVERY METHOD
   ↓
RESTORE
   ↓
VALIDATE TABLE
   ↓
UPDATE APPLICATION
   ↓
SERVICE RESTORED
```
### Factors
- Table size
- Recovery method
- Restore duration
- Application cutover
- IAM
- KMS
- Indexes
- Streams
- Downstream services
### Interview point
> "The table restore time is only one component of the application RTO."
## 17 — Cross-Region Backup
For regional disaster recovery, consider protected copies in another Region.
```text
PRIMARY REGION
      ↓
DYNAMODB BACKUP
      ↓
COPY
      ↓
DR REGION
      ↓
RESTORE
```
### Why?
- Regional disaster
- Geographic resilience
- Compliance
### Important
A backup copy is a recovery artifact, not automatically an active database.
## 18 — AWS Backup Cross-Region Copy
AWS Backup supports cross-Region copy capabilities for supported resources and configurations.
```text
PRIMARY REGION
      ↓
AWS BACKUP
      ↓
CROSS-REGION COPY
      ↓
DR REGION VAULT
      ↓
RECOVERY
```
### Interview answer
> "For backup-based regional DR, I can use centralized cross-Region backup copies where the DynamoDB resource and architecture support them."
Reference: https://docs.aws.amazon.com/aws-backup/latest/devguide/cross-region-backup.html
## 19 — Cross-Account Backup
For account-level isolation:
```text
PRODUCTION ACCOUNT
       ↓
DYNAMODB
       ↓
BACKUP
       ↓
BACKUP ACCOUNT
       ↓
PROTECTED RECOVERY POINT
```
### Why?
- Reduced blast radius
- Independent administration
- Ransomware resilience
- Governance
### Interview answer
> "For critical DynamoDB workloads, I consider a separate backup account so compromise of production does not automatically compromise the recovery boundary."
## 20 — Backup Vault Lock
Where supported by the architecture, AWS Backup Vault Lock can provide stronger protection for recovery points.
```text
DYNAMODB
 ↓
AWS BACKUP
 ↓
RECOVERY POINT
 ↓
VAULT
 ↓
VAULT LOCK
 ↓
PROTECTED RETENTION
```
### Interview answer
> "For immutable retention requirements, I evaluate Vault Lock and validate the retention policy before enabling controls that are difficult or impossible to reverse."
Reference: https://docs.aws.amazon.com/aws-backup/latest/devguide/vault-lock.html
## 21 — DynamoDB Encryption
DynamoDB encrypts data at rest.
Workloads can use AWS owned keys by default or configure customer-managed KMS keys where supported and required.
```text
DYNAMODB
    ↓
ENCRYPTION AT REST
    ↓
KMS
```
### Recovery consideration
If a customer-managed KMS key is involved, verify:
- Key state
- IAM permissions
- Key policy
- Account / Region
### Interview answer
> "Encryption is part of the recovery dependency chain because access to the recovery data must also be authorized."
Reference: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/EncryptionAtRest.html
## 22 — DynamoDB KMS Troubleshooting
If a restore or backup operation involving a customer-managed key fails:
```text
FAILURE
  ↓
KMS KEY
  ↓
KEY STATE
  ↓
IAM
  ↓
KEY POLICY
  ↓
ACCOUNT
  ↓
REGION
```
### Interview point
Do not troubleshoot only DynamoDB; inspect the encryption dependency.
## 23 — DynamoDB Global Tables
Global Tables provide multi-Region, multi-active replication for DynamoDB.
```text
REGION A
DYNAMODB
   │
   ├──────────────→ REGION B
   │                  DYNAMODB
   │
   └──────────────→ REGION C
                      DYNAMODB
```
### Why?
- Regional resilience
- Low-latency global access
- Multi-Region application architecture
### Important
Global Tables are not a replacement for historical backups.
Reference: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GlobalTables.html
## 24 — Global Tables vs Backup
```text
GLOBAL TABLES
→ CURRENT REPLICATED DATA
→ MULTI-REGION AVAILABILITY
→ FAST REGIONAL RECOVERY
```
```text
BACKUP / PITR
→ HISTORICAL RECOVERY
→ LOGICAL ERROR RECOVERY
→ RECOVERY POINTS
```
### Strong architecture
```text
GLOBAL TABLES
+
PITR
+
ON-DEMAND BACKUPS
+
ISOLATED BACKUP COPY
```
### Interview answer
> "Global Tables address availability and regional resilience; PITR and backups address historical recovery."
## 25 — Logical Corruption Scenario
### Scenario
An application writes incorrect values to thousands of items.
```text
APPLICATION BUG
      ↓
BAD WRITES
      ↓
DYNAMODB
```
### Recovery
```text
IDENTIFY INCIDENT TIME
      ↓
PITR
      ↓
RESTORE BEFORE INCIDENT
      ↓
VALIDATE
      ↓
RECOVER DATA
```
### Interview answer
> "PITR is a strong choice when I know approximately when the logical corruption occurred."
## 26 — Accidental Delete Scenario
### Scenario
An operator deletes records accidentally.
```text
ACCIDENTAL DELETE
      ↓
IDENTIFY TIME
      ↓
PITR
      ↓
RESTORE
      ↓
RECOVER REQUIRED ITEMS
```
### Important
Do not automatically replace production with the entire restored table.
For large systems, selective data recovery may be safer.
## 27 — Application Deletes the Wrong Dataset
A buggy application can delete valid data.
```text
BUG
 ↓
DELETE
 ↓
TABLE
```
### Recovery workflow
```text
PITR
 ↓
RESTORE NEW TABLE
 ↓
COMPARE
 ↓
EXPORT / COPY REQUIRED ITEMS
 ↓
PRODUCTION
```
### Senior-level point
> "The safest recovery target is not always a complete production replacement."
## 28 — Ransomware / Credential Compromise
DynamoDB itself is a managed service, but compromised credentials can still create destructive application or administrative operations.
### Layered protection
```text
PRODUCTION
   ↓
PITR
   ↓
ON-DEMAND BACKUP
   ↓
CROSS-ACCOUNT COPY
   ↓
BACKUP VAULT
   ↓
VAULT LOCK
```
### Additional controls
- Least privilege
- MFA / strong administrative controls
- Separate backup administration
- CloudTrail
- Monitoring
- Restore testing
## 29 — DynamoDB Backup Monitoring
Monitor:
```text
PITR STATUS
BACKUP JOBS
COPY JOBS
RESTORE JOBS
TABLE CONFIGURATION
```
Useful services:
```text
AWS BACKUP
CLOUDWATCH
EVENTBRIDGE
CLOUDTRAIL
SNS
```
### Interview answer
> "I monitor both backup coverage and recovery operations, not only backup job completion."
## 30 — CloudTrail for DynamoDB Investigation
CloudTrail can help audit DynamoDB API activity.
```text
API ACTION
    ↓
CLOUDTRAIL
    ↓
AUDIT EVENT
    ↓
INVESTIGATION
```
### Questions
- Who changed the table?
- Who deleted data?
- Who changed backup configuration?
- Who modified IAM access?
### Interview point
CloudTrail provides audit evidence; it is not itself the backup.
Reference: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/logging-using-cloudtrail.html
## 31 — Scenario: PITR Is Disabled
### Problem
A logical corruption incident occurs, but PITR was not enabled.
```text
INCIDENT
   ↓
PITR CHECK
   ↓
DISABLED
   ↓
NO PITR RECOVERY WINDOW
```
### Recovery options
Evaluate:
- Existing on-demand backups
- AWS Backup recovery points
- Application-level recovery
- Replicated copies
### Lesson
> **Backup capability must be enabled before the incident.**
## 32 — Scenario: Recovery Point Is Too Old
### Problem
The required recovery time is outside the PITR window.
```text
REQUESTED TIME
      ↓
OUTSIDE PITR WINDOW
      ↓
PITR UNAVAILABLE
```
### Recovery
Look for:
```text
ON-DEMAND BACKUP
AWS BACKUP RECOVERY POINT
CROSS-REGION COPY
OTHER VALIDATED RECOVERY SOURCE
```
### Interview point
This is why retention planning matters.
## 33 — Scenario: Backup Succeeds but Application Fails
```text
RESTORE TABLE
      ↓
TABLE AVAILABLE
      ↓
APPLICATION FAILURE
```
Investigate:
- Table name / ARN
- IAM
- KMS
- Indexes
- Streams
- Application configuration
- Environment variables
- Downstream integrations
### Interview answer
> "A successful DynamoDB restore proves the table was recovered; it does not prove the application was recovered."
## 34 — Scenario: Global Table Region Fails
### Recovery options
If another replica remains available:
```text
FAILED REGION
      X
      ↓
HEALTHY GLOBAL TABLE REPLICA
      ↓
APPLICATION
```
### Backup recovery
If the requirement is historical recovery:
```text
BACKUP
 ↓
RECOVERY POINT
 ↓
RESTORE
```
### Interview point
Global Tables can reduce regional recovery time, while backups protect against logical history problems.
## 35 — Scenario: RPO Is 1 Hour
### Design
Evaluate:
```text
PITR
+
ON-DEMAND BACKUPS
+
GLOBAL TABLES / DR
```
The right mechanism depends on whether the failure is:
- Logical
- Regional
- Application-level
### Interview answer
> "I map the one-hour RPO to the failure domain rather than selecting a single backup mechanism."
## 36 — Scenario: RTO Is 15 Minutes
A restore-based recovery may require time to create and validate a new table.
### Evaluate
```text
PREPARED DR
      +
GLOBAL TABLES
      +
AUTOMATED CUTOVER
      +
PITR / BACKUPS
```
### Interview answer
> "For a very aggressive RTO, I would evaluate a continuously available regional architecture rather than depending only on on-demand restoration."
## 37 — Scenario: Pre-Migration Backup
Before a major schema or application change:
```text
PRE-CHANGE
    ↓
ON-DEMAND BACKUP
    ↓
CHANGE
    ↓
VALIDATE
    ↓
ROLLBACK / RECOVER
```
### Interview answer
> "I create a deliberate recovery point before high-risk changes."
## 38 — Scenario: Long-Term Retention
### Requirement
Keep a known state for an extended period.
### Design
```text
DYNAMODB
    ↓
ON-DEMAND BACKUP
    ↓
RETENTION
    ↓
OPTIONAL COPY / ISOLATION
```
### Consider
- Compliance
- Cost
- Recovery requirements
- Backup-account isolation
## 39 — DynamoDB Restore Validation
After restore:
```text
RESTORE
 ↓
TABLE STATUS
 ↓
KEY SCHEMA
 ↓
INDEXES
 ↓
ITEM VALIDATION
 ↓
IAM
 ↓
KMS
 ↓
APPLICATION
```
### Validate
- Table exists
- Expected data exists
- Required indexes work
- Permissions are correct
- Application can connect
- Downstream integrations work
## 40 — DynamoDB Recovery Runbook
```text
1. IDENTIFY INCIDENT
        ↓
2. DETERMINE FAILURE TYPE
        ↓
3. IDENTIFY INCIDENT TIME
        ↓
4. SELECT PITR / BACKUP / DR
        ↓
5. RESTORE
        ↓
6. VALIDATE TABLE
        ↓
7. VALIDATE IAM / KMS
        ↓
8. VALIDATE APPLICATION
        ↓
9. RECOVER / CUT OVER
        ↓
10. MEASURE RTO
```
### Senior-level principle
> **The recovery runbook should define both data recovery and application cutover.**
## 41 — DynamoDB Backup Coverage
A healthy backup strategy should answer:
```text
WHICH TABLES?
      ↓
WHICH ACCOUNTS?
      ↓
WHICH REGIONS?
      ↓
WHICH BACKUP POLICY?
      ↓
WHICH RETENTION?
```
### Common failure
A newly created table may not automatically have the intended protection unless backup policy automation covers it.
## 42 — DynamoDB Backup Cost Considerations
Consider:
```text
BACKUP STORAGE
PITR
ON-DEMAND BACKUPS
CROSS-REGION COPIES
RESTORE OPERATIONS
```
### Design principle
> "I optimize backup retention and copies only after confirming the recovery requirements."
## 43 — DynamoDB Backup Architecture
A resilient design:
```text
                     PRODUCTION
                         │
                    DYNAMODB
                         │
              ┌──────────┴──────────┐
              ↓                     ↓
             PITR             ON-DEMAND BACKUP
              │                     │
              ↓                     ↓
       RECOVERY WINDOW        FIXED RECOVERY
                                    │
                                    ↓
                              AWS BACKUP
                                    │
                         ┌──────────┴──────────┐
                         ↓                     ↓
                   BACKUP ACCOUNT         DR REGION
                         │                     │
                    VAULT LOCK            RECOVERY
```
## 44 — DynamoDB Failure-Domain Model
```text
FAILURE
│
├── LOGICAL CORRUPTION
│      ↓
│   PITR
│
├── ACCIDENTAL DELETE
│      ↓
│   PITR / BACKUP
│
├── LONG-TERM RECOVERY
│      ↓
│   ON-DEMAND BACKUP
│
├── REGION FAILURE
│      ↓
│   GLOBAL TABLES / DR COPY
│
├── ACCOUNT COMPROMISE
│      ↓
│   CROSS-ACCOUNT BACKUP
│
└── APPLICATION FAILURE
       ↓
    DATA + CONFIG + CUTOVER
```
## 45 — Senior-Level DynamoDB Recovery Design
A mature architecture separates:
```text
DATA RECOVERY
+
REGIONAL RESILIENCE
+
ACCOUNT ISOLATION
+
ENCRYPTION
+
APPLICATION RECOVERY
+
MONITORING
+
TESTING
```
### Reference architecture
```text
                    DYNAMODB
                       │
             ┌─────────┴─────────┐
             ↓                   ↓
            PITR             ON-DEMAND
             │                BACKUP
             │                   │
             └─────────┬─────────┘
                       ↓
                 AWS BACKUP
                       ↓
              ISOLATED RECOVERY
                       ↓
              CROSS-REGION COPY
                       ↓
                    RESTORE
                       ↓
                  APPLICATION
```
## 46 — Backup vs High Availability vs DR
This distinction is essential.
```text
BACKUP
→ HISTORICAL DATA RECOVERY
```
```text
HIGH AVAILABILITY
→ CONTINUE SERVICE DURING CERTAIN FAILURES
```
```text
DISASTER RECOVERY
→ RECOVER FROM MAJOR FAILURE DOMAIN
```
### Example
Global Tables can provide multi-Region availability, but they should not be treated as a replacement for historical backups.
## 47 — DynamoDB Restore Testing
Test:
- PITR
- On-demand backup restore
- AWS Backup recovery
- Cross-Region recovery
- Cross-account recovery
- KMS-protected recovery
- Application reconnection
- Global Table failover where applicable
### Test model
```text
RECOVERY POINT
      ↓
RESTORE
      ↓
VALIDATE DATA
      ↓
VALIDATE ACCESS
      ↓
APPLICATION TEST
      ↓
MEASURE RTO
```
## 48 — DynamoDB Backup Checklist
### Protection
- [ ] PITR enabled where required
- [ ] Recovery window defined
- [ ] On-demand backup strategy
- [ ] AWS Backup policy where required
### Security
- [ ] IAM least privilege
- [ ] Encryption
- [ ] KMS permissions
- [ ] Backup administration isolated
- [ ] Vault protection where required
### Resilience
- [ ] Cross-Region strategy
- [ ] Cross-account strategy
- [ ] Global Tables evaluated for regional requirements
### Recovery
- [ ] PITR tested
- [ ] Snapshot / backup restore tested
- [ ] Table validation
- [ ] Application cutover documented
### Operations
- [ ] Backup monitoring
- [ ] Restore monitoring
- [ ] CloudTrail
- [ ] EventBridge / alerting
### Testing
- [ ] Logical corruption test
- [ ] Accidental deletion test
- [ ] Regional recovery test
- [ ] Encryption test
- [ ] Application recovery test
## 49 — 60-Second DynamoDB Backup Interview Answer
> "For DynamoDB, I distinguish Point-in-Time Recovery from on-demand backups. PITR provides a continuous recovery window that is useful for logical errors such as accidental deletes or bad application writes, while on-demand backups provide deliberate fixed recovery points for long-term retention or pre-change protection. I use AWS Backup when centralized governance and backup-vault management are required. For regional resilience, I evaluate Global Tables for continuously available multi-Region workloads and backup copies for historical recovery. I also consider encryption, KMS, IAM, cross-account isolation and application cutover. Finally, I test restore procedures so the RTO is proven rather than assumed."
## 50 — DynamoDB Interview Traps
### Trap 1
> "PITR and on-demand backups are the same."
**Better:** PITR provides recovery within a rolling time window; on-demand backups provide fixed recovery points.
### Trap 2
> "PITR restores the production table in place."
**Better:** Restore creates a recovered table that must be validated and integrated.
### Trap 3
> "Global Tables are backups."
**Better:** Global Tables provide multi-Region replication and availability, not historical backup by themselves.
### Trap 4
> "DynamoDB Streams are backups."
**Better:** Streams provide change events, not a general historical recovery mechanism.
### Trap 5
> "Backup success means application recovery."
**Better:** Validate table configuration, IAM, KMS and application connectivity.
### Trap 6
> "A replicated table protects against logical corruption."
**Better:** Bad writes can propagate; historical backups and PITR are still required.
### Trap 7
> "RPO equals backup retention."
**Better:** RPO is acceptable data loss; retention defines how far back recovery is available.
## 51 — Final DynamoDB Mental Model
Memorize:
```text
DYNAMODB
│
├── PITR
│     ↓
│   POINT-IN-TIME RECOVERY
│
├── ON-DEMAND BACKUP
│     ↓
│   FIXED RECOVERY POINT
│
├── AWS BACKUP
│     ↓
│   CENTRALIZED GOVERNANCE
│
├── GLOBAL TABLES
│     ↓
│   MULTI-REGION RESILIENCE
│
├── CROSS-ACCOUNT
│     ↓
│   ADMINISTRATIVE ISOLATION
│
├── KMS / IAM
│     ↓
│   SECURE RECOVERY
│
└── RESTORE TEST
      ↓
   PROVEN RECOVERY
```
### Final principle
> **DynamoDB backup design should protect against logical mistakes, preserve required recovery history, survive the relevant failure domain and restore the application—not just the table.**
## Official AWS References
- [Amazon DynamoDB Developer Guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)
- [DynamoDB Point-in-Time Recovery](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/PointInTimeRecovery.html)
- [DynamoDB Backup and Restore](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/BackupRestore.html)
- [DynamoDB Restore Tutorial](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Restore.Tutorial.html)
- [DynamoDB Streams](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html)
- [DynamoDB Global Tables](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GlobalTables.html)
- [DynamoDB Encryption at Rest](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/EncryptionAtRest.html)
- [DynamoDB CloudTrail Logging](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/logging-using-cloudtrail.html)
- [AWS Backup Supported Services](https://docs.aws.amazon.com/aws-backup/latest/devguide/working-with-supported-services.html)
- [AWS Backup Cross-Region Backup](https://docs.aws.amazon.com/aws-backup/latest/devguide/cross-region-backup.html)
- [AWS Backup Vault Lock](https://docs.aws.amazon.com/aws-backup/latest/devguide/vault-lock.html)
