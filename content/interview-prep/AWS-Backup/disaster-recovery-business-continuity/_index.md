---
title: " 🚨 Disaster Recovery & Business Continuity"
description: "Interview-focused AWS Disaster Recovery and Business Continuity covering RTO, RPO, DR strategies, backup and restore, pilot light, warm standby, multi-site architecture, AWS Backup, regional recovery, testing, failover and senior-level scenarios."
weight: 120
toc: true
---

Disaster Recovery (DR) and Business Continuity (BC) are broader than backup. A backup gives you a recovery point; a DR strategy defines how the organization continues or restores critical services when a failure affects infrastructure, applications, accounts or an entire Region.
## 01 — DR / BC Mental Model
```text
BUSINESS REQUIREMENT
        │
        ↓
      RTO / RPO
        │
        ↓
   DR STRATEGY
        │
 ┌──────┼────────┐
 ↓      ↓        ↓
BACKUP REPLICATION MULTI-REGION
 │      │        │
 └──────┼────────┘
        ↓
     RECOVERY
        ↓
    VALIDATION
        ↓
 BUSINESS SERVICE
```
### Core principle
> **Start with business impact, define RPO/RTO, identify failure domains, then select the recovery architecture.**
## 02 — Business Continuity
Business Continuity is the broader discipline of keeping critical business functions operating during and after disruption.
It includes:
- People
- Processes
- Technology
- Applications
- Data
- Facilities
- Communications
- Vendors
### Interview answer
> "Business continuity defines how the business continues operating during disruption, while disaster recovery focuses on restoring technology and services."
## 03 — Disaster Recovery
Disaster Recovery focuses on recovering technology services after a disruptive event.
```text
DISASTER
   ↓
DETECTION
   ↓
DECISION
   ↓
FAILOVER / RESTORE
   ↓
VALIDATION
   ↓
SERVICE RECOVERY
```
### Failure examples
- Availability Zone failure
- Region failure
- Data corruption
- Ransomware
- Accidental deletion
- Account compromise
- Application failure
- Infrastructure failure
## 04 — RPO
Recovery Point Objective defines the maximum acceptable amount of data loss measured in time.
```text
LAST GOOD RECOVERY POINT
          │
          ↓
       INCIDENT
          │
          ↓
      DATA LOSS
```
### Example
```text
RPO = 1 HOUR
→ BUSINESS CAN ACCEPT
  UP TO 1 HOUR OF DATA LOSS
```
### Interview question
> "If the RPO is 15 minutes, can a daily backup strategy satisfy it?"
**No.**
The protection mechanism must produce recoverable state frequently enough to meet the requirement.
## 05 — RTO
Recovery Time Objective defines how quickly the service must be restored.
```text
INCIDENT
   ↓
RECOVERY START
   ↓
SERVICE AVAILABLE
```
### Example
```text
RTO = 30 MINUTES
→ SERVICE MUST BE RECOVERED
  WITHIN 30 MINUTES
```
### Interview point
RTO includes the complete recovery process, not only the time required to copy data.
## 06 — RPO vs RTO
| Concept | Meaning | Main Question |
|---|---|---|
| RPO | Acceptable data loss | How much data can we lose? |
| RTO | Acceptable downtime | How quickly must we recover? |
### Memory
> **RPO = data**
> **RTO = time**
## 07 — Recovery Time Components
A realistic RTO includes:
```text
DETECT
  +
DECIDE
  +
RECOVER
  +
CONFIGURE
  +
VALIDATE
  +
CUTOVER
```
### Example
A database may restore in 10 minutes but the application may still require:
- Network setup
- IAM
- Secrets
- DNS
- Application startup
- Validation
Therefore:
> **Restore time ≠ application RTO**
## 08 — Failure Domains
Identify what can fail:
```text
COMPONENT
   ↓
INSTANCE
   ↓
AVAILABILITY ZONE
   ↓
REGION
   ↓
ACCOUNT
   ↓
ORGANIZATION
```
### Senior-level point
The recovery architecture should protect against the failure domain relevant to the business.
## 09 — Availability vs Disaster Recovery
```text
HIGH AVAILABILITY
→ REDUCE SERVICE INTERRUPTION
```
```text
DISASTER RECOVERY
→ RECOVER FROM MAJOR FAILURE
```
### Example
Multi-AZ architecture can protect against certain Availability Zone failures, but it does not replace:
- Historical backups
- Regional DR
- Account isolation
- Restore testing
## 10 — Backup vs DR
```text
BACKUP
→ RECOVERY POINT
```
```text
DR
→ RECOVERY SYSTEM
```
### Backup answers
> "Can I recover the data?"
### DR answers
> "Can I restore the business service within the required RTO?"
## 11 — AWS Well-Architected DR Strategies
A common AWS framework describes progressively faster and more expensive recovery strategies:
```text
BACKUP & RESTORE
       ↓
PILOT LIGHT
       ↓
WARM STANDBY
       ↓
MULTI-SITE ACTIVE / ACTIVE
```
As recovery objectives become more aggressive:
```text
LOWER RTO
   ↓
MORE PREPARED INFRASTRUCTURE
   ↓
HIGHER COST / COMPLEXITY
```
Reference: https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/disaster-recovery-dr-for-aws-workloads.html
## 12 — Backup and Restore Strategy
```text
PRIMARY REGION
      ↓
BACKUP
      ↓
DR REGION
      ↓
RESTORE
      ↓
APPLICATION
```
### Characteristics
- Lowest infrastructure cost among major DR patterns
- Higher recovery time
- Requires restore operations
- Good for less critical workloads
### Interview answer
> "Backup and restore is appropriate when the workload can tolerate a higher RTO and does not require continuously available DR infrastructure."
## 13 — Pilot Light
A pilot-light environment keeps only essential components running in the DR Region.
```text
PRIMARY
  │
  ├── APPLICATION
  ├── DATABASE
  │
  ↓
DR
  │
  ├── CORE DATA / DATABASE
  └── MINIMAL INFRASTRUCTURE
```
During disaster:
```text
ACTIVATE
   ↓
SCALE
   ↓
DEPLOY APPLICATION
   ↓
CUTOVER
```
### Interview answer
> "Pilot light keeps the core recovery foundation ready while the full application stack is activated during a disaster."
## 14 — Warm Standby
Warm standby maintains a scaled-down but functional version of the application in the DR environment.
```text
PRIMARY
  ↓
FULL SCALE
```
```text
DR
  ↓
REDUCED SCALE
  ↓
READY TO SERVE
```
During disaster:
```text
FAILOVER
   ↓
SCALE UP
   ↓
TRAFFIC
```
### Advantage
Faster recovery than rebuilding the entire environment.
## 15 — Multi-Site Active / Active
Both Regions actively serve traffic.
```text
REGION A
  ↕
GLOBAL TRAFFIC
  ↕
REGION B
```
### Characteristics
- Very low potential RTO
- Strong regional resilience
- Highest operational complexity
- Requires application and data architecture designed for multi-Region operation
### Interview answer
> "Active/active is appropriate when the business value of very low downtime justifies the additional cost and complexity."
## 16 — DR Strategy Comparison
| Strategy | Typical RTO | Complexity | Cost |
|---|---|---|---|
| Backup & Restore | Higher | Lower | Lower |
| Pilot Light | Lower | Medium | Medium |
| Warm Standby | Low | Higher | Higher |
| Multi-Site Active/Active | Very Low | Highest | Highest |
### Important
Exact RTO depends on workload design and must be tested.
## 17 — Selecting a DR Strategy
Ask:
```text
1. WHAT IS THE RTO?
2. WHAT IS THE RPO?
3. WHAT FAILURE DOMAIN?
4. HOW MUCH COST IS ACCEPTABLE?
5. HOW MUCH COMPLEXITY IS ACCEPTABLE?
6. WHAT MUST BE RUNNING DURING DR?
```
### Decision
```text
HIGH RTO + LOW COST
→ BACKUP / RESTORE
```
```text
LOW RTO
→ PILOT LIGHT / WARM STANDBY
```
```text
VERY LOW RTO
→ ACTIVE / ACTIVE
```
## 18 — AWS Backup in DR
AWS Backup can provide recovery points that support disaster recovery workflows.
```text
PRODUCTION
   ↓
AWS BACKUP
   ↓
CROSS-REGION COPY
   ↓
DR VAULT
   ↓
RESTORE
```
### Important
AWS Backup provides protection and recovery points; it does not automatically turn the DR Region into a complete application environment.
Reference: https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html
## 19 — Cross-Region Backup
```text
PRIMARY REGION
      ↓
RECOVERY POINT
      ↓
COPY
      ↓
DR REGION
```
### Protects against
- Region outage
- Regional service disruption
- Geographic disaster
### Does not automatically provide
- Running application
- Ready compute
- DNS cutover
- Network configuration
- Application validation
## 20 — Cross-Account DR
A strong recovery architecture can separate production and recovery administration.
```text
PRODUCTION ACCOUNT
       ↓
BACKUP
       ↓
BACKUP / DR ACCOUNT
       ↓
RECOVERY
```
### Benefits
- Reduced blast radius
- Security isolation
- Ransomware resilience
- Independent administration
### Interview answer
> "For critical workloads, I avoid placing every recovery dependency under the same administrative boundary."
## 21 — DR Account Strategy
A dedicated recovery account may contain:
```text
BACKUP VAULT
KMS
DR NETWORK
AUTOMATION
MONITORING
RECOVERY TOOLS
```
### Security controls
- Least privilege
- MFA
- Separate administrators
- Restricted network access
- Audit logging
- Break-glass access
## 22 — DR and KMS
Encryption keys can become recovery dependencies.
```text
BACKUP
 ↓
KMS
 ↓
RESTORE
 ↓
RESOURCE
```
### Check
- Key exists
- Key is enabled
- Key policy allows required actions
- IAM permissions are available
- Destination Region has required key configuration
### Interview point
> **A recovery plan that ignores encryption-key recovery can fail even when the backup itself exists.**
## 23 — DR Network Architecture
A DR Region may need:
```text
VPC
 ↓
SUBNETS
 ↓
ROUTE TABLES
 ↓
SECURITY GROUPS
 ↓
NACLs
 ↓
DNS
 ↓
APPLICATION
```
### Design principle
Infrastructure dependencies should be documented and, where possible, recreated through Infrastructure as Code.
## 24 — Infrastructure as Code for DR
Use tools such as:
- AWS CloudFormation
- AWS CDK
- Terraform
### Pattern
```text
CODE
 ↓
NETWORK
 ↓
SECURITY
 ↓
COMPUTE
 ↓
APPLICATION
```
### Interview answer
> "Infrastructure as Code reduces manual configuration during recovery and improves repeatability."
## 25 — DNS Failover
Traffic must move to the recovered environment.
```text
USERS
  ↓
DNS
  ↓
PRIMARY
```
During disaster:
```text
USERS
  ↓
DNS / TRAFFIC MANAGEMENT
  ↓
DR
```
### Consider
- TTL
- Health checks
- Application readiness
- DNS propagation behavior
- Client caching
### Interview point
DNS failover is part of application recovery, not a backup mechanism.
## 26 — Database DR
Database strategy depends on RPO/RTO.
```text
BACKUP
→ HISTORICAL RECOVERY
```
```text
REPLICATION
→ CURRENT / NEAR-CURRENT DATA
```
### Example
- RDS automated backup + PITR
- Cross-Region snapshot copy
- Aurora Global Database
- DynamoDB Global Tables
- EFS replication
### Principle
> **Use replication for availability and backup for historical recovery.**
## 27 — Storage DR
Different storage services have different recovery models.
```text
EBS
→ SNAPSHOTS
```
```text
EFS
→ AWS BACKUP / REPLICATION
```
```text
S3
→ VERSIONING / REPLICATION / BACKUP OPTIONS
```
```text
DYNAMODB
→ PITR / BACKUP / GLOBAL TABLES
```
### Interview point
Do not design DR without understanding the native recovery model of each service.
## 28 — Application Dependency Mapping
A recovery plan should identify:
```text
USER
 ↓
DNS
 ↓
LOAD BALANCER
 ↓
APPLICATION
 ↓
CACHE
 ↓
DATABASE
 ↓
STORAGE
 ↓
EXTERNAL SERVICES
```
### Senior-level point
Recovering only the database does not recover the application.
## 29 — Dependency Ordering
Recovery should follow dependency order.
Example:
```text
NETWORK
 ↓
SECURITY
 ↓
DATABASE
 ↓
STORAGE
 ↓
COMPUTE
 ↓
APPLICATION
 ↓
DNS
 ↓
USERS
```
Actual order can vary by architecture.
### Interview answer
> "I document dependency ordering so operators know what must exist before each recovery step."
## 30 — DR Automation
Automate where possible:
```text
DETECT
 ↓
DECIDE
 ↓
PROVISION
 ↓
RESTORE
 ↓
CONFIGURE
 ↓
VALIDATE
 ↓
CUTOVER
```
### Possible AWS services
- AWS Systems Manager
- AWS Lambda
- Step Functions
- EventBridge
- CloudFormation
- CDK
- Terraform
### Goal
Reduce manual recovery steps and human error.
## 31 — DR Runbook
A runbook should include:
```text
1. INCIDENT
2. FAILURE DOMAIN
3. BUSINESS DECISION
4. RECOVERY STRATEGY
5. RECOVERY POINT
6. INFRASTRUCTURE
7. DATA RESTORE
8. APPLICATION START
9. VALIDATION
10. DNS / TRAFFIC CUTOVER
11. BUSINESS VALIDATION
12. COMMUNICATION
```
### Interview point
> **A DR plan should be executable by someone other than its original author.**
## 32 — Disaster Declaration
Not every outage should trigger a regional failover.
Define:
```text
INCIDENT
   ↓
SEVERITY
   ↓
IMPACT
   ↓
DECISION AUTHORITY
   ↓
DR DECLARATION
```
### Senior-level considerations
- Business impact
- Expected outage duration
- Data consistency
- Current Region health
- Recovery readiness
- Customer impact
## 33 — Failover vs Failback
### Failover
```text
PRIMARY
   X
   ↓
DR
```
### Failback
```text
DR
 ↓
PRIMARY
```
### Important
Failback can be more complex than failover because data accumulated during the DR period must be reconciled or synchronized.
## 34 — Failback Strategy
```text
DR ACTIVE
   ↓
RESTORE PRIMARY
   ↓
SYNC DATA
   ↓
VALIDATE
   ↓
SWITCH TRAFFIC
   ↓
PRIMARY ACTIVE
```
### Interview answer
> "I design failback as a separate procedure instead of assuming that reversing failover is automatically safe."
## 35 — Scenario: Region Outage
### Backup-based architecture
```text
REGION A
   X
   │
   ↓
REGION B
   ↓
RESTORE
   ↓
APPLICATION
   ↓
DNS
```
### Active/active architecture
```text
REGION A
   X
   │
   ↓
REGION B
   ↓
CONTINUE TRAFFIC
```
### Interview point
The correct design depends on RTO/RPO and cost.
## 36 — Scenario: Accidental Data Deletion
This is primarily a logical recovery problem.
```text
DELETE
 ↓
IDENTIFY TIME
 ↓
PITR / BACKUP
 ↓
RESTORE
 ↓
RECOVER DATA
```
### Interview answer
> "Regional failover does not necessarily solve logical data deletion. I need historical recovery."
## 37 — Scenario: Ransomware
### Requirements
```text
ISOLATION
+
IMMUTABILITY
+
RECOVERY COPY
+
MONITORING
```
### Architecture
```text
PRODUCTION
   ↓
BACKUP
   ↓
CROSS-ACCOUNT
   ↓
CROSS-REGION
   ↓
VAULT PROTECTION
   ↓
RECOVERY
```
### Interview point
Ransomware recovery is a security and DR problem, not simply a backup scheduling problem.
## 38 — Scenario: Account Compromise
```text
PRODUCTION ACCOUNT
        X
        │
        ↓
BACKUP ACCOUNT
        ↓
PROTECTED VAULT
        ↓
RECOVERY
```
### Controls
- Separate account
- Least privilege
- Vault protection
- Independent administration
- CloudTrail
- Monitoring
## 39 — Scenario: RTO = 15 Minutes
Backup and restore may not be sufficient.
Evaluate:
```text
WARM STANDBY
+
AUTOMATED FAILOVER
+
PREPARED NETWORK
+
PREPARED COMPUTE
+
REPLICATED DATA
```
### Interview answer
> "A 15-minute RTO requires me to measure restore and cutover time; if backup restore cannot reliably meet it, I move toward a more prepared DR strategy."
## 40 — Scenario: RPO = 5 Minutes
Daily or hourly snapshots may not be enough.
Evaluate:
```text
CONTINUOUS / FREQUENT RECOVERY
+
REPLICATION
+
MULTI-REGION
```
### Interview answer
> "For a five-minute RPO, I would evaluate continuous recovery or replication capabilities supported by the specific workload rather than relying only on periodic backups."
## 41 — Scenario: DR Environment Exists but Recovery Fails
```text
DR READY
   ↓
FAILOVER
   ↓
APPLICATION FAILURE
```
Check:
- IAM
- KMS
- Secrets
- Network
- Security groups
- DNS
- Database connectivity
- External dependencies
- Certificates
### Lesson
> **A DR environment can exist and still be unusable if dependencies are not tested.**
## 42 — Scenario: Backup Exists but Cannot Restore
```text
BACKUP
 ↓
RESTORE FAILURE
 ↓
KMS
 ↓
IAM
 ↓
REGION
 ↓
RESOURCE SUPPORT
 ↓
CONFIGURATION
```
### Interview point
Backup validation must include actual restore testing.
## 43 — DR Testing
Testing should include:
- Backup restore
- Regional recovery
- Application startup
- DNS cutover
- Database recovery
- Storage recovery
- Security validation
- Failover
- Failback
### Test cycle
```text
PLAN
 ↓
TEST
 ↓
MEASURE
 ↓
IDENTIFY GAPS
 ↓
IMPROVE
 ↓
RETEST
```
## 44 — DR Game Day
A DR game day simulates a realistic failure.
Example:
```text
REGION FAILURE
      ↓
DECLARE DR
      ↓
EXECUTE RUNBOOK
      ↓
RECOVER
      ↓
VALIDATE
      ↓
MEASURE RTO / RPO
```
### Why?
- Finds hidden dependencies
- Tests human procedures
- Tests automation
- Validates communication
- Measures actual recovery
## 45 — RTO / RPO Measurement
Do not assume.
Measure:
```text
DETECTION TIME
+
DECISION TIME
+
RESTORE TIME
+
CONFIGURATION TIME
+
CUTOVER TIME
```
### Example
```text
TOTAL RTO
= 5m detection
+ 5m decision
+ 15m restore
+ 10m configuration
+ 5m cutover

= 40 MINUTES
```
### Interview point
Measured RTO is stronger than a theoretical RTO.
## 46 — DR Documentation
Document:
- Architecture
- Dependencies
- RPO
- RTO
- Contacts
- Runbooks
- Credentials / access process
- Recovery Regions
- Backup vaults
- KMS keys
- DNS
- Validation steps
### Principle
> **If the recovery knowledge exists only in one engineer's head, the organization has a continuity risk.**
## 47 — Business Continuity During Recovery
Technology recovery is only one part.
Consider:
```text
CUSTOMER COMMUNICATION
+
STAFF
+
VENDORS
+
BUSINESS PROCESS
+
SUPPORT
+
COMPLIANCE
+
FINANCE
```
### Interview answer
> "Business continuity asks whether the business can continue critical operations, while DR addresses the technology recovery needed to support that continuity."
## 48 — DR Cost Model
As recovery objectives become more aggressive:
```text
RTO ↓
   ↓
PREPAREDNESS ↑
   ↓
COST ↑
```
Examples:
```text
BACKUP / RESTORE
→ LOWER COST
```
```text
WARM STANDBY
→ HIGHER COST
```
```text
ACTIVE / ACTIVE
→ HIGHEST COST / COMPLEXITY
```
### Senior-level point
Optimize against business value, not technology preference.
## 49 — DR Architecture Checklist
### Business
- [ ] Critical workloads identified
- [ ] RPO defined
- [ ] RTO defined
- [ ] Business owners identified
### Data
- [ ] Backup enabled
- [ ] Cross-Region protection
- [ ] Cross-account isolation
- [ ] Retention defined
- [ ] Restore tested
### Infrastructure
- [ ] DR Region selected
- [ ] VPC
- [ ] Subnets
- [ ] Security
- [ ] IAM
- [ ] KMS
### Application
- [ ] Dependencies documented
- [ ] Configuration recoverable
- [ ] Secrets available
- [ ] DNS strategy
### Operations
- [ ] Monitoring
- [ ] Alerting
- [ ] Runbook
- [ ] Automation
### Testing
- [ ] Failover test
- [ ] Restore test
- [ ] Failback test
- [ ] RTO measurement
- [ ] RPO validation
## 50 — Senior-Level DR Architecture
```text
                         USERS
                           │
                           ↓
                    GLOBAL TRAFFIC
                     /           \
                    ↓             ↓
              REGION A         REGION B
              PRIMARY          DR / ACTIVE
                 │                │
        ┌────────┼────────┐       │
        ↓        ↓        ↓       ↓
      APP      DATA     STORAGE  APP
        │        │        │       │
        └────────┼────────┴───────┘
                 ↓
              BACKUP
                 ↓
          CROSS-ACCOUNT
                 ↓
          PROTECTED VAULT
                 ↓
            RECOVERY
```
### Security layers
```text
IAM
+
KMS
+
ACCOUNT ISOLATION
+
REGION ISOLATION
+
VAULT PROTECTION
+
AUDIT
```
## 51 — Backup, DR and BC Relationship
```text
BUSINESS CONTINUITY
        │
        ↓
DISASTER RECOVERY
        │
   ┌────┴─────┐
   ↓          ↓
BACKUP     REPLICATION
   │          │
   └────┬─────┘
        ↓
     RECOVERY
```
### Memory
> **BC is the business strategy. DR is the technology recovery strategy. Backup is one recovery mechanism.**
## 52 — 60-Second DR Interview Answer
> "I start disaster recovery design with business RPO and RTO rather than selecting a technology first. For workloads with relaxed recovery objectives, backup and restore may be sufficient. For more demanding RTOs, I evaluate pilot light, warm standby or active/active architectures. I use cross-Region and cross-account backup copies to protect against regional and account-level failure domains, and I include KMS, IAM, networking, DNS, application dependencies and secrets in the recovery design. I automate infrastructure where possible and regularly test failover, restore and failback. Finally, I measure the actual recovery time and recovery point because an untested DR plan is only a theoretical plan."
## 53 — DR Interview Traps
### Trap 1
> "Backup is disaster recovery."
**Better:** Backup provides recovery points; DR is the broader recovery architecture and process.
### Trap 2
> "Multi-AZ means you have DR."
**Better:** Multi-AZ improves availability but does not automatically provide regional DR.
### Trap 3
> "Cross-Region backup means the application is ready."
**Better:** You still need infrastructure, networking, security and application recovery.
### Trap 4
> "Replication replaces backup."
**Better:** Replication may copy bad data; backups provide historical recovery.
### Trap 5
> "RTO equals restore time."
**Better:** RTO includes detection, decision, recovery, configuration, validation and cutover.
### Trap 6
> "Global Tables / replication means no backups are needed."
**Better:** Replication addresses availability; backups address historical recovery.
### Trap 7
> "A DR plan only needs to be written once."
**Better:** DR plans must be tested and updated as architecture changes.
### Trap 8
> "Failback is just failover in reverse."
**Better:** Failback requires data synchronization, validation and controlled traffic movement.
## 54 — Final DR Mental Model
Memorize:
```text
BUSINESS
   ↓
RPO / RTO
   ↓
FAILURE DOMAIN
   ↓
DR STRATEGY
   ↓
BACKUP / REPLICATION
   ↓
CROSS-REGION
   ↓
CROSS-ACCOUNT
   ↓
SECURITY
   ↓
AUTOMATION
   ↓
FAILOVER
   ↓
VALIDATION
   ↓
FAILBACK
```
### Final principle
> **Good disaster recovery is not simply having a second copy of the data. It is having a tested, secure and repeatable way to restore the business service within its required RPO and RTO.**
## Official AWS References
- [AWS Well-Architected Reliability Pillar — Disaster Recovery](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/disaster-recovery-dr-for-aws-workloads.html)
- [AWS Backup Developer Guide](https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html)
- [AWS Backup Cross-Region Backup](https://docs.aws.amazon.com/aws-backup/latest/devguide/cross-region-backup.html)
- [AWS Backup Cross-Account Backup](https://docs.aws.amazon.com/aws-backup/latest/devguide/manage-cross-account-backup.html)
- [AWS Backup Vault Lock](https://docs.aws.amazon.com/aws-backup/latest/devguide/vault-lock.html)
- [AWS Disaster Recovery of Workloads on AWS](https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-workloads-on-aws/disaster-recovery-options-in-the-cloud.html)
- [AWS Elastic Disaster Recovery](https://docs.aws.amazon.com/drs/latest/userguide/what-is-drs.html)
