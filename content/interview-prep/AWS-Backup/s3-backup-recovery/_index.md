---
title: " 🪣 S3 Backup & Recovery"
description: "Interview-focused AWS S3 backup and recovery covering versioning, replication, Object Lock, lifecycle, encryption, deletion protection, recovery scenarios and architecture."
weight: 60
toc: true
---

Amazon S3 is already designed for highly durable object storage, but **durability is not the same as recoverability**. A strong S3 protection strategy combines versioning, deletion protection, replication, lifecycle management, encryption, access control and tested recovery procedures.
## 01 — S3 Backup Mental Model
Think about S3 protection as layers:
```text
S3 OBJECT
   ↓
VERSIONING
   ↓
DELETION / OVERWRITE RECOVERY
   ↓
OBJECT LOCK, WHERE REQUIRED
   ↓
REPLICATION
   ↓
SECONDARY BUCKET / REGION
   ↓
RECOVERY
```
### Important distinction
> **S3 durability protects against infrastructure failure; backup and recovery controls address deletion, overwrite, corruption, account compromise and operational mistakes.**
## 02 — Is S3 Already Backed Up?
### Interview Answer
S3 is designed for extremely high durability, but you should not describe S3 durability as a complete backup strategy.
A backup and recovery design depends on the failure scenarios you need to survive.
### Strong answer
> "S3 provides high durability, but for recovery from accidental deletion, overwrites, malicious activity or regional requirements, I would add controls such as versioning, Object Lock and replication according to the workload's requirements."
## 03 — S3 Versioning
S3 Versioning keeps multiple versions of an object when objects are overwritten or deleted.
```text
OBJECT
  ↓
VERSION 1
  ↓
VERSION 2
  ↓
VERSION 3
```
If an object is overwritten, previous versions can remain available.
### Why it matters
Versioning can help recover from:
- Accidental overwrite
- Accidental deletion
- Application mistakes
- Some forms of malicious modification
### Interview phrase
> **"Versioning gives me object-level recovery points."**
Reference: https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html
## 04 — What Happens When a Versioned Object Is Deleted?
With versioning enabled, a normal DELETE request generally creates a **delete marker** rather than immediately removing the existing object version.
```text
BEFORE
OBJECT
  ↓
VERSION A
  ↓
VERSION B
AFTER DELETE
  ↓
DELETE MARKER
  ↓
VERSION B STILL EXISTS
  ↓
VERSION A STILL EXISTS
```
### Recovery
Removing the delete marker can make the previous current version accessible again, subject to permissions and the bucket's configuration.
### Interview answer
> "In a versioned bucket, a normal delete creates a delete marker, so the previous object version can still be recovered."
Reference: https://docs.aws.amazon.com/AmazonS3/latest/userguide/DeleteMarker.html
## 05 — Versioning Does Not Mean Unlimited Protection
Versioning is powerful, but it has limitations.
It does not automatically provide:
- Geographic isolation
- Immutable retention
- Protection from every administrative action
- Protection from deletion of versions by an authorized principal
- A complete disaster-recovery strategy
### Interview answer
> "Versioning is one recovery layer, not the entire S3 backup architecture."
## 06 — S3 Object Lock
S3 Object Lock can help prevent objects from being deleted or overwritten for a defined retention period.
```text
OBJECT
   ↓
OBJECT LOCK
   ↓
RETENTION
   ↓
PROTECTED OBJECT
```
### Use cases
- Compliance
- WORM-style retention
- Ransomware resilience
- Protection against accidental deletion
### Important
Object Lock requires versioning and uses retention controls such as retention periods or legal holds.
Reference: https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html
## 07 — Governance Mode vs Compliance Mode
S3 Object Lock supports retention modes with different administrative behavior.
### Governance mode
Provides protection while allowing specially authorized users to bypass or modify retention according to the permissions and configuration.
### Compliance mode
Provides stronger immutability. Protected versions cannot be overwritten or deleted by users, including the root user, until the retention period expires.
### Interview warning
> "I choose the mode based on the required security and compliance level and validate retention before enabling strong immutability."
Reference: https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html
## 08 — Legal Holds
A legal hold can prevent an object version from being deleted or overwritten regardless of a configured retention period until the legal hold is explicitly removed.
```text
OBJECT VERSION
      ↓
LEGAL HOLD
      ↓
PROTECTED
      ↓
RELEASE HOLD
      ↓
NORMAL RETENTION RULES
```
### Interview use
Mention legal holds when the interviewer asks about litigation, investigations or compliance preservation.
Reference: https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html
## 09 — S3 Replication
S3 replication automatically copies eligible objects between buckets.
Common patterns include:
```text
SOURCE BUCKET
      ↓
REPLICATION
      ↓
DESTINATION BUCKET
```
Replication can be configured across:
- Same Region
- Different Regions
### Why?
- Disaster recovery
- Geographic separation
- Compliance
- Operational isolation
Reference: https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication.html
## 10 — Same-Region vs Cross-Region Replication
### Same-Region Replication
```text
BUCKET A
   ↓
BUCKET B
SAME REGION
```
Useful for:
- Account separation
- Environment separation
- Log/data aggregation
- Operational isolation
### Cross-Region Replication
```text
REGION A
   ↓
BUCKET A
   ↓
CRR
   ↓
REGION B
   ↓
BUCKET B
```
Useful for:
- Regional disaster recovery
- Geographic resilience
- Regulatory requirements
### Interview phrase
> **"Cross-Region replication addresses a Region-level failure domain; versioning addresses object-level recovery."**
## 11 — Replication Is Not the Same as Backup
Replication copies changes.
That means accidental deletion or corruption can potentially be replicated too, depending on the configuration.
```text
SOURCE
  ↓
BAD CHANGE
  ↓
REPLICATION
  ↓
DESTINATION
  ↓
BAD CHANGE COPIED
```
### Important
A resilient design should consider:
- Versioning
- Replication configuration
- Delete-marker behavior
- Object Lock
- Separate administrative boundaries
### Interview answer
> "Replication improves availability and disaster recovery, but I don't treat replication alone as an immutable backup."
## 12 — S3 Replication and Versioning
Versioning is a prerequisite for S3 replication configurations.
```text
SOURCE VERSIONING
        ↓
REPLICATION
        ↓
DESTINATION VERSIONING
```
### Design principle
Use versioning on both sides where required by the replication configuration.
Reference: https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication-requirements.html
## 13 — Replication Time Control
For workloads with strict replication requirements, S3 Replication Time Control can provide a defined target for replication of eligible objects.
### Interview answer
> "If the business has a strict replication objective, I would evaluate S3 Replication Time Control rather than assuming ordinary replication meets the requirement."
Reference: https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication-time-control.html
## 14 — S3 Batch Replication
S3 Batch Replication can help replicate existing objects that were not previously replicated or need replication after configuration changes.
```text
EXISTING OBJECTS
      ↓
BATCH REPLICATION
      ↓
DESTINATION
```
### Interview use
Mention it when the interviewer asks:
> "What about objects that existed before replication was enabled?"
Reference: https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-batch-replication-batch.html
## 15 — S3 Lifecycle
Lifecycle rules automate actions based on object age and other supported conditions.
```text
OBJECT
  ↓
STANDARD
  ↓
TRANSITION
  ↓
ARCHIVE STORAGE
  ↓
EXPIRATION
```
### Why?
- Cost optimization
- Retention management
- Automated data lifecycle
### Important
Lifecycle expiration must be designed carefully when versioning and recovery requirements exist.
Reference: https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html
## 16 — Versioned Bucket Lifecycle
When versioning is enabled, lifecycle management can apply differently to:
- Current object versions
- Noncurrent object versions
- Delete markers
### Example
```text
CURRENT VERSION
       ↓
KEEP ACTIVE
       ↓
NONCURRENT VERSION
       ↓
RETAIN FOR RECOVERY
       ↓
EXPIRE AFTER POLICY
```
### Interview warning
Do not configure aggressive noncurrent-version expiration without understanding the recovery window it creates.
Reference: https://docs.aws.amazon.com/AmazonS3/latest/userguide/lifecycle-configuration-examples.html
## 17 — S3 Encryption
S3 supports encryption for objects at rest.
Common concepts include:
```text
SSE-S3
SSE-KMS
DSSE-KMS
SSE-C
```
### Interview answer
> "For sensitive workloads I evaluate the required encryption model, KMS key management, key policies and operational access requirements."
Reference: https://docs.aws.amazon.com/AmazonS3/latest/userguide/serv-side-encryption.html
## 18 — S3 and KMS During Recovery
Encryption can become part of the restore path.
```text
RECOVERY
   ↓
OBJECT ACCESS
   ↓
KMS PERMISSION, IF APPLICABLE
   ↓
DATA ACCESS
```
### Troubleshooting questions
- Does the identity have permission to use the KMS key?
- Is the key enabled?
- Does the destination account have the required access?
- Are bucket and object permissions correct?
### Senior-level point
> "A backup is not operationally useful if the recovery identity cannot decrypt and access it."
## 19 — S3 Access Control and Recovery
A recovery design should account for:
- IAM
- Bucket policies
- KMS permissions
- S3 Access Points where applicable
- Organization controls
- Cross-account access
### Recovery model
```text
RECOVERY IDENTITY
      ↓
IAM
      ↓
BUCKET POLICY
      ↓
OBJECT ACCESS
      ↓
KMS
      ↓
DATA
```
### Interview answer
> "I test recovery using the same security boundaries that production recovery would actually use."
## 20 — S3 Backup Architecture
A resilient S3 design can look like:
```text
                    PRIMARY BUCKET
                         │
                    VERSIONING
                         │
                 OBJECT PROTECTION
                         │
              ┌──────────┴──────────┐
              │                     │
         OBJECT LOCK           REPLICATION
              │                     │
              │              ┌──────┴──────┐
              │              ↓             ↓
              │         DR BUCKET      BACKUP ACCOUNT
              │              │             │
              │              └──────┬──────┘
              │                     ↓
              │                PROTECTED COPY
              │
              └──────────────→ RECOVERY
```
### Goal
Create multiple recovery layers across different failure scenarios.
## 21 — S3 Backup Account Architecture
For critical data:
```text
PRODUCTION ACCOUNT
        │
        ↓
SOURCE BUCKET
        │
        ↓
REPLICATION
        │
        ↓
BACKUP ACCOUNT
        │
        ↓
DESTINATION BUCKET
        │
        ↓
OBJECT LOCK
```
### Why?
If production administrators are compromised, an isolated account can provide a separate administrative boundary.
### Interview answer
> "I would consider a dedicated backup account for critical S3 data when account isolation is part of the threat model."
## 22 — S3 Cross-Region Disaster Recovery
A common DR design:
```text
PRIMARY REGION
      │
SOURCE BUCKET
      │
VERSIONING
      │
CRR
      │
SECONDARY REGION
      │
DESTINATION BUCKET
      │
RECOVERY
```
### DR considerations
- Replication status
- Replication lag
- Destination permissions
- KMS keys
- DNS/application dependencies
- Recovery access
- Data validation
## 23 — RPO for S3
RPO defines how much data the business can afford to lose.
For S3:
```text
RPO
 ↓
REPLICATION / PROTECTION FREQUENCY
 ↓
RECOVERY POINT
```
### Important
If a business requires extremely low RPO, evaluate the supported S3 replication and recovery capabilities rather than assuming ordinary asynchronous replication is sufficient.
## 24 — RTO for S3
RTO is the time required to restore application access to the data.
```text
INCIDENT
  ↓
SELECT RECOVERY LOCATION
  ↓
ACCESS / PERMISSIONS
  ↓
DATA AVAILABLE
  ↓
APPLICATION RECONFIGURATION
  ↓
SERVICE RESTORED
```
### Important
For S3 applications, the RTO may depend more on application configuration, DNS, IAM and downstream dependencies than on the object copy itself.
## 25 — Accidental Object Deletion
### Scenario
An administrator deletes an important object from a versioned bucket.
### Recovery
```text
DELETE
 ↓
DELETE MARKER
 ↓
PREVIOUS VERSION
 ↓
REMOVE / RESTORE DELETE MARKER
 ↓
OBJECT AVAILABLE
```
### Interview answer
> "If versioning is enabled, I would inspect the object's versions and delete marker, then restore the appropriate previous version."
## 26 — Accidental Object Overwrite
### Scenario
An application overwrites a valid object with incorrect content.
### Recovery
```text
BAD VERSION
    ↓
OBJECT VERSIONS
    ↓
IDENTIFY GOOD VERSION
    ↓
RESTORE / COPY GOOD VERSION
```
### Interview answer
> "Versioning gives me the previous object version, which I can use to recover the correct content."
## 27 — Ransomware Deletes S3 Data
### Scenario
An attacker obtains credentials with access to the bucket.
### Stronger design
```text
PRODUCTION
   ↓
VERSIONING
   ↓
REPLICATION
   ↓
SEPARATE ACCOUNT
   ↓
OBJECT LOCK
   ↓
PROTECTED DATA
```
### Interview answer
> "I would not rely on versioning alone because an attacker with sufficient permissions may still be able to affect versions. I would combine isolation, least privilege, Object Lock and replicated copies."
## 28 — Ransomware Encrypts Objects
If ransomware uploads encrypted versions or overwrites objects:
```text
RANSOMWARE
    ↓
BAD OBJECT VERSION
    ↓
VERSIONING
    ↓
PREVIOUS GOOD VERSION
```
### But
If the attacker can delete versions or compromise every recovery location, versioning alone is insufficient.
### Stronger design
```text
VERSIONING
+
OBJECT LOCK
+
SEPARATE ACCOUNT
+
REPLICATION
+
LEAST PRIVILEGE
```
## 29 — What If the Primary Region Fails?
### Architecture
```text
PRIMARY REGION
      X
      │
      ↓
SECONDARY REGION
      │
DESTINATION BUCKET
      ↓
APPLICATION RECOVERY
      ↓
SERVICE RESTORED
```
### Interview answer
> "I would use the existing replicated copy in the secondary Region, validate access and dependencies, and redirect the application to the recovery location."
## 30 — What If Replication Is Delayed?
### Troubleshooting
```text
REPLICATION ISSUE
       ↓
REPLICATION STATUS
       ↓
SOURCE OBJECT
       ↓
RULE
       ↓
DESTINATION
       ↓
IAM
       ↓
KMS
       ↓
METRICS / EVENTS
```
### Questions
- Is replication configured correctly?
- Is the object eligible?
- Are destination permissions valid?
- Is encryption preventing replication?
- Is the object already replicated?
- Is there a replication backlog?
## 31 — What If an Object Was Created Before Replication?
Existing objects may require an additional mechanism such as S3 Batch Replication.
```text
OLD OBJECTS
   ↓
BATCH REPLICATION
   ↓
DESTINATION
```
### Interview answer
> "I would not assume enabling replication automatically backfills every historical object. I would evaluate S3 Batch Replication for existing data."
## 32 — S3 Backup vs Replication
| Capability | Versioning | Replication | Object Lock |
|---|---|---|---|
| Recover overwrite | Yes | Depends on configuration | Protects retention |
| Recover delete | Yes | Depends on delete-marker behavior | Protects retained versions |
| Region isolation | No | Yes | No |
| Account isolation | No | Yes, where configured | No |
| Immutability | No | No by itself | Yes |
| Cost optimization | No | No | No |
### Interview summary
> **Versioning = object recovery**
> **Replication = location resilience**
> **Object Lock = immutability**
## 33 — S3 Recovery Architecture by Failure
```text
FAILURE
│
├── OVERWRITE
│      ↓
│   VERSIONING
│
├── ACCIDENTAL DELETE
│      ↓
│   VERSIONING
│
├── REGIONAL FAILURE
│      ↓
│   CROSS-REGION REPLICATION
│
├── ACCOUNT COMPROMISE
│      ↓
│   SEPARATE ACCOUNT COPY
│
├── MALICIOUS DELETION
│      ↓
│   OBJECT LOCK + LEAST PRIVILEGE
│
└── LONG-TERM RETENTION
       ↓
    LIFECYCLE
```
## 34 — S3 Recovery Runbook
A practical recovery runbook:
```text
1. IDENTIFY INCIDENT
        ↓
2. DETERMINE FAILURE TYPE
        ↓
3. IDENTIFY RECOVERY LOCATION
        ↓
4. IDENTIFY REQUIRED OBJECT VERSION
        ↓
5. VERIFY IAM / KMS ACCESS
        ↓
6. RESTORE / COPY OBJECT
        ↓
7. VALIDATE DATA
        ↓
8. VALIDATE APPLICATION
        ↓
9. RECORD RECOVERY TIME
```
### Senior-level principle
> **The recovery procedure should be documented before the incident occurs.**
## 35 — S3 Recovery Testing
Test scenarios such as:
- Accidental deletion
- Accidental overwrite
- Corrupted object
- Regional failure
- Replication failure
- Credential compromise
- Recovery from isolated account
### Example
```text
TEST
 ↓
DELETE / SIMULATE FAILURE
 ↓
RECOVER
 ↓
VALIDATE
 ↓
MEASURE RTO
 ↓
DOCUMENT
```
## 36 — S3 Monitoring
Monitor:
```text
REPLICATION
VERSIONING / CONFIGURATION
ACCESS
KMS
OBJECT OPERATIONS
APPLICATION HEALTH
```
Useful AWS services include:
```text
CLOUDWATCH
EVENTBRIDGE
CLOUDTRAIL
S3 METRICS
SNS
```
### Interview answer
> "I monitor both the protection mechanism and the access path required for recovery."
## 37 — S3 CloudTrail
CloudTrail can help investigate S3 API activity.
```text
S3 API ACTION
     ↓
CLOUDTRAIL
     ↓
AUDIT RECORD
     ↓
INVESTIGATION
```
### Questions CloudTrail can help answer
- Who deleted an object?
- Who changed a bucket configuration?
- Who changed access permissions?
- When did the operation happen?
### Interview point
CloudTrail provides audit evidence; it does not itself restore deleted data.
## 38 — S3 Cost-Aware Recovery Design
Cost considerations include:
```text
STORAGE
VERSIONED OBJECTS
REPLICATION
REQUESTS
DATA TRANSFER
ARCHIVE STORAGE
RECOVERY OPERATIONS
```
### Important
Versioning can increase storage usage because older versions remain until lifecycle policies remove them.
### Interview answer
> "I include noncurrent-version retention in the cost model because versioning can retain multiple copies of changing objects."
## 39 — Scenario: Business Requires 30-Day Recovery
### Design
```text
VERSIONING
     ↓
NONCURRENT RETENTION
     ↓
LIFECYCLE
     ↓
30-DAY RECOVERY WINDOW
```
### Validate
Make sure lifecycle does not expire versions before the required recovery period.
## 40 — Scenario: Business Requires Seven-Year Retention
### Design considerations
- Long-term retention requirements
- Storage cost
- Lifecycle
- Object Lock if immutability is required
- Compliance requirements
- Recovery accessibility
### Interview answer
> "I would separate retention requirements from availability requirements and design lifecycle and immutability around the actual policy."
## 41 — Scenario: Production and Backup Must Be Administratively Separate
### Design
```text
PRODUCTION ACCOUNT
      ↓
SOURCE BUCKET
      ↓
REPLICATION
      ↓
BACKUP ACCOUNT
      ↓
DESTINATION BUCKET
      ↓
OBJECT LOCK
```
### Interview answer
> "I would use cross-account replication to create an independent administrative boundary, then protect the destination according to the retention requirement."
## 42 — Scenario: Application Must Recover in 15 Minutes
### Design questions
Ask:
- How large is the dataset?
- Is the data already replicated?
- Where is the application running?
- Are DNS changes required?
- Are IAM roles ready?
- Are KMS keys available?
- Can the application use the secondary bucket immediately?
### Interview answer
> "For a 15-minute RTO, I would pre-stage the recovery environment and test the complete failover workflow rather than relying on an on-demand object restore."
## 43 — Scenario: Versioned Bucket Storage Is Growing Rapidly
### Investigation
```text
STORAGE GROWTH
      ↓
VERSION COUNT
      ↓
OBJECT CHANGE RATE
      ↓
NONCURRENT VERSIONS
      ↓
LIFECYCLE POLICY
```
### Remediation
- Review noncurrent-version retention
- Identify high-churn prefixes
- Adjust lifecycle where business requirements allow
- Validate recovery windows before reducing retention
## 44 — Scenario: User Deletes the Wrong Object
### Recovery
```text
VERSIONED BUCKET
      ↓
DELETE MARKER
      ↓
LIST OBJECT VERSIONS
      ↓
IDENTIFY PREVIOUS VERSION
      ↓
RESTORE
```
### Interview answer
> "I would first check version history and delete markers rather than immediately recreating the object manually."
## 45 — Scenario: Wrong Object Version Is Replicated
### Diagnosis
Replication is working, but the wrong content was copied.
### Lesson
```text
REPLICATION SUCCESS
       ≠
DATA QUALITY SUCCESS
```
### Strong design
Use versioning, validation, application controls and protected recovery copies appropriate to the workload.
## 46 — S3 Backup Architecture Checklist
### Data protection
- [ ] Versioning enabled where required
- [ ] Object recovery tested
- [ ] Noncurrent-version retention defined
### Resilience
- [ ] Replication considered
- [ ] Cross-Region copy considered
- [ ] Cross-account isolation considered
### Security
- [ ] Least privilege
- [ ] Encryption
- [ ] KMS permissions
- [ ] Object Lock where required
- [ ] Recovery account isolated
### Operations
- [ ] Replication monitoring
- [ ] CloudTrail
- [ ] EventBridge / alerts
- [ ] Recovery runbook
### Recovery
- [ ] Accidental delete test
- [ ] Overwrite test
- [ ] Regional recovery test
- [ ] Access / KMS test
- [ ] RTO measured
## 47 — 60-Second S3 Backup Interview Answer
> "For S3, I don't equate durability with a complete backup strategy. I start with the recovery requirements and failure scenarios. Versioning gives me recovery from object overwrites and many accidental deletes. If I need stronger protection against deletion or ransomware, I consider S3 Object Lock and separate administrative boundaries. For regional resilience, I use S3 replication to a secondary bucket or Region where appropriate. I also design encryption, IAM and KMS access as part of the recovery path. Finally, I monitor replication and access activity and regularly test object recovery and application failover against the required RPO and RTO."
## 48 — S3 Interview Traps
### Trap 1
> "S3 is durable, so backup is unnecessary."
**Better:** Durability and recoverability solve different problems.
### Trap 2
> "Versioning is a backup."
**Better:** Versioning is an object-recovery mechanism; it does not provide every backup property.
### Trap 3
> "Replication is immutable backup."
**Better:** Replication can copy unwanted changes too.
### Trap 4
> "Object Lock protects every S3 problem."
**Better:** Combine immutability with versioning, isolation, encryption and monitoring.
### Trap 5
> "Deleting an object from a versioned bucket removes all versions."
**Better:** A normal delete creates a delete marker; versions can remain.
### Trap 6
> "Enabling replication automatically protects all historical data."
**Better:** Evaluate S3 Batch Replication for existing objects.
## 49 — Final S3 Mental Model
Memorize:
```text
S3
│
├── VERSIONING
│     → OBJECT RECOVERY
│
├── OBJECT LOCK
│     → IMMUTABILITY
│
├── REPLICATION
│     → LOCATION RESILIENCE
│
├── ENCRYPTION
│     → DATA PROTECTION
│
├── IAM / KMS
│     → ACCESS CONTROL
│
├── LIFECYCLE
│     → RETENTION + COST
│
└── MONITORING
      → OPERATIONAL VISIBILITY
```
### Final principle
> **A strong S3 recovery architecture protects against the failure modes that matter to the business, not just against infrastructure failure.**
## Official AWS References
- [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [S3 Versioning](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html)
- [S3 Delete Markers](https://docs.aws.amazon.com/AmazonS3/latest/userguide/DeleteMarker.html)
- [S3 Object Lock](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html)
- [S3 Replication](https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication.html)
- [S3 Replication Requirements](https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication-requirements.html)
- [S3 Replication Time Control](https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication-time-control.html)
- [S3 Batch Replication](https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-batch-replication-batch.html)
- [S3 Lifecycle Management](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html)
- [S3 Lifecycle Configuration Examples](https://docs.aws.amazon.com/AmazonS3/latest/userguide/lifecycle-configuration-examples.html)
- [S3 Server-Side Encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/serv-side-encryption.html)
