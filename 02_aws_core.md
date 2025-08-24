# 2) AWS Core

## Section: AWS IAM

**Q56. Difference between IAM User vs IAM Role?**

**Answer (Natural):**  
An IAM User is a long-term identity with credentials (password/keys). An IAM Role is assumed to obtain **temporary** credentials with scoped permissions; it isn't tied to a person and is ideal for workloads, cross-account access, and least privilege.


👉 **Example:** Developer uses an IAM user for console MFA; EC2 assumes a role via instance profile to read S3.


---

**Q57. What is an Instance Profile?**

**Answer (Natural):**  
A container that makes an IAM role available to an EC2 instance. The app retrieves temporary credentials from the Instance Metadata Service (IMDS) instead of storing keys.


👉 **Example:** Attach role `S3ReadOnly` via instance profile; app calls S3 using IMDSv2 tokens.


---

**Q58. Managed Policies vs Inline Policies?**

**Answer (Natural):**  
Managed policies are reusable and versionable across identities; inline policies are embedded into a single identity and tightly coupled. Prefer managed for reuse and governance.


👉 **Example:** `ReadOnlyAccess` (AWS managed) vs a one-off inline policy for a role.


---

**Q59. Permission Boundaries & SCPs?**

**Answer (Natural):**  
Permission boundaries limit the **maximum** permissions an identity-based policy can grant. Service Control Policies (SCPs) apply at org/account level to restrict even admins. Combine with least privilege.


👉 **Example:** Boundary prevents a developer role from creating roles with admin rights.


---

**Q60. Resource vs Identity vs Permission Policies?**

**Answer (Natural):**  
Identity policies attach to users/roles; resource policies attach to resources (S3 buckets, KMS keys) and control who can access them; permissions boundaries & SCPs constrain maximum potential grants.


👉 **Example:** S3 bucket policy allows a partner account; role policy allows `s3:GetObject`.


---

## Section: AWS EC2

**Q61. EC2 purchasing options: On-Demand vs Reserved vs Savings Plans vs Spot?**

**Answer (Natural):**  
On-Demand = flexible hourly; Reserved/Savings Plans = commit to 1–3 years for discounts; Spot = use spare capacity up to 90% off but can be reclaimed—good for fault-tolerant workloads.


👉 **Example:** CI runners on Spot with interruption handling + ASG; prod web on Savings Plans.


---

**Q62. AMI vs Snapshot vs EBS Volume?**

**Answer (Natural):**  
AMI is a bootable image; snapshot is a point-in-time backup of EBS; EBS volume is the block device attached to instances.


👉 **Example:** Bake hardened AMIs; nightly EBS snapshots for DR.


---

**Q63. IMDSv2—why important?**

**Answer (Natural):**  
Instance Metadata Service v2 requires session-oriented tokens, mitigating SSRF credential theft. Enforce IMDSv2-only in production.


👉 **Example:** Block IMDSv1 via `http-token-required`.


---

**Q64. Auto Scaling Group (ASG) scaling policies?**

**Answer (Natural):**  
Target tracking (keep metric at target), step scaling (react to thresholds), scheduled scaling (time-based). Combine with warm pools to reduce cold starts.


👉 **Example:** Keep CPU at 50% using target tracking; scale up at 70% with step.


---

**Q65. Placement groups (Cluster/Spread/Partition)?**

**Answer (Natural):**  
Cluster = low-latency single AZ; Spread = max separation for HA; Partition = separate racks/partitions—ideal for big data.


👉 **Example:** Hadoop uses Partition groups; latency-sensitive HPC uses Cluster.


---

**Q66. EBS GP3 vs IO1/IO2?**

**Answer (Natural):**  
GP3: general purpose with baseline IOPS and configurable throughput; IO1/IO2: provisioned IOPS for high, predictable performance. Balance cost vs latency/IOPS needs.


👉 **Example:** Databases → IO2; app servers → GP3.


---

**Q67. ENI (Elastic Network Interface) use cases?**

**Answer (Natural):**  
Attach additional NICs for MGMT/data plane separation, HA, or security appliances. Move ENI between instances for quick failover.


👉 **Example:** Firewall via ENI across instances.


---

**Q68. Spot interruptions—how to handle?**

**Answer (Natural):**  
Listen for interruption notice (2 minutes), drain connections, checkpoint state, persist to S3/DB, and rely on diversified ASG.


👉 **Example:** Use Capacity-Optimized allocation strategy with multiple AZs.


---

**Q69. Nitro vs Xen?**

**Answer (Natural):**  
Nitro offloads virtualization to hardware for better performance and security isolation; most modern instance families are Nitro-based.


👉 **Example:** Nitro enclaves for isolated compute of sensitive data.


---

**Q70. User Data vs Cloud-Init?**

**Answer (Natural):**  
User Data passes bootstrap scripts to instances; Cloud-Init processes them to configure OS. Use idempotent scripts or config management for repeatability.


👉 **Example:** Install agents, join domain, fetch app artifacts at boot.


---

## Section: AWS VPC & Networking

**Q71. Public vs Private subnets—rule of thumb?**

**Answer (Natural):**  
Public subnets route to an Internet Gateway; private subnets don't. Place ALBs and bastions public; app/db private with egress via NAT.


👉 **Example:** Route table: 0.0.0.0/0 → IGW (public) vs NAT (private).


---

**Q72. NAT Gateway vs NAT Instance?**

**Answer (Natural):**  
NAT Gateway is managed, scalable, HA in one AZ; NAT Instance is DIY, cheaper at low traffic, but needs patching and scaling. Prefer Gateway for prod.


👉 **Example:** Dev VPC with NAT instance + autoscaling; prod uses NAT Gateway.


---

**Q73. VPC Peering vs Transit Gateway (TGW) vs PrivateLink?**

**Answer (Natural):**  
Peering = point-to-point L3; TGW = hub-and-spoke for many VPCs; PrivateLink exposes services via ENIs without full routing—good for SaaS/private services.


👉 **Example:** Org with 50 VPCs uses TGW; offer internal API via PrivateLink.


---

**Q74. Security Group vs NACL?**

**Answer (Natural):**  
SG is stateful and attached to ENIs; NACL is stateless at subnet boundary. Use SGs for instance/service-level rules; NACLs for coarse subnet protections.


👉 **Example:** Allow 443 inbound on ALB SG; NACL default allows ephemeral return traffic.


---

**Q75. Route 53 Resolver endpoints?**

**Answer (Natural):**  
Inbound and outbound endpoints enable hybrid DNS between on-prem and AWS VPCs. Use rules to forward specific domains.


👉 **Example:** Forward `corp.local` to on-prem DNS via outbound endpoint.


---

**Q76. VPC Endpoints (Gateway vs Interface)?**

**Answer (Natural):**  
Gateway endpoints (S3/DynamoDB) update route tables; Interface endpoints create ENIs for private access to many AWS services. Cut NAT/IGW traffic.


👉 **Example:** Access S3 privately via Gateway endpoint; call SSM via Interface endpoint.


---

**Q77. IPv6 in VPC—why enable?**

**Answer (Natural):**  
Avoid NAT, abundant addresses, and better end-to-end connectivity. Use Egress-Only IGW for outbound-only IPv6.


👉 **Example:** Dual-stack load balancers and instances.


---

**Q78. AWS Network Firewall vs NACL/SG?**

**Answer (Natural):**  
Network Firewall is managed stateful inspection at VPC scale with rulesets; SG/NACL are basic filtering. Use NF for deep inspection and egress controls.


👉 **Example:** Block malicious domains by domain list at firewall.


---

**Q79. Hybrid connectivity options?**

**Answer (Natural):**  
Site-to-Site VPN (quick), Direct Connect (dedicated, low latency), or both (DX + VPN for HA).


👉 **Example:** Critical workloads use DX with VPN backup.


---

**Q80. Multi-account strategy with AWS Organizations?**

**Answer (Natural):**  
Isolate workloads, apply SCPs, centralize logging/billing, share services via RAM. Improves blast radius and compliance.


👉 **Example:** Separate prod, staging, security, and shared services accounts.


---

## Section: Amazon S3

**Q81. Storage classes—how to choose?**

**Answer (Natural):**  
Standard (frequent), IA (infrequent), One Zone-IA (single AZ), Intelligent-Tiering (automatic), Glacier Instant/FL/Deep Archive (cold). Choose by access pattern and retrieval time/cost.


👉 **Example:** Logs to IA after 30 days; archives to Deep Archive.


---

**Q82. Consistency model today?**

**Answer (Natural):**  
S3 provides **strong read-after-write and list consistency** across all regions—simplifies app logic.


👉 **Example:** No need for self-built consistency layers for new objects/overwrites.


---

**Q83. S3 security basics?**

**Answer (Natural):**  
Block Public Access (account/bucket), least-privilege IAM, bucket policies, SSE (SSE-S3/SSE-KMS), Object Ownership to disable ACLs. Enforce TLS.


👉 **Example:** Use AWS KMS CMKs and bucket policies denying unencrypted puts.


---

**Q84. Multipart upload—why and when?**

**Answer (Natural):**  
Parallelizes large uploads for speed and resiliency. You can resume failed parts. Required for files >5GB.


👉 **Example:** Upload 100GB backup in 10MB parts with retries.


---

**Q85. S3 eventing options?**

**Answer (Natural):**  
EventBridge, SQS, or Lambda notifications on object events. Decouple pipelines.


👉 **Example:** Trigger image resize Lambda on `ObjectCreated`.


---

**Q86. Lifecycle policies & object locking?**

**Answer (Natural):**  
Automate transitions between classes and expirations. Object Lock (WORM) prevents deletion—use for compliance with governance/legal hold.


👉 **Example:** Transition to Glacier after 90 days; expire after 365 days.


---

**Q87. Access Points & VPC endpoints?**

**Answer (Natural):**  
Access Points create distinct policies per application; combine with VPC endpoints for private access and granular control.


👉 **Example:** Analytics job uses an Access Point with restricted paths.


---

## Section: Amazon RDS

**Q88. RDS Multi-AZ vs Read Replicas?**

**Answer (Natural):**  
Multi-AZ provides synchronous standby for HA/failover; Read Replicas are asynchronous and used for read scaling and offloading. They solve different problems.


👉 **Example:** Prod OLTP: Multi-AZ; analytics: read from replicas.


---

**Q89. Parameter Group vs Option Group?**

**Answer (Natural):**  
Parameter groups control DB engine settings; option groups enable engine features (e.g., Oracle TDE).


👉 **Example:** Tune `max_connections` in parameter group.


---

**Q90. Backups & PITR?**

**Answer (Natural):**  
Automated backups enable point-in-time restore; snapshots are manual backups you control. Test restores regularly.


👉 **Example:** Restore to a new instance after accidental drop.


---

**Q91. Storage types for RDS?**

**Answer (Natural):**  
GP3 for general workloads; IO2 for high IOPS; Magnetic is legacy. Match to latency and throughput needs.


👉 **Example:** Write-heavy OLTP → IO2.


---

**Q92. Aurora vs RDS engines?**

**Answer (Natural):**  
Aurora is cloud-native with distributed storage, faster failover, and up to 15 replicas; compatible with MySQL/Postgres. Traditional RDS manages single-node storage.


👉 **Example:** Multi-Region Aurora Global Database for DR.


---

## Section: Route 53, Monitoring, and Cost Optimization

**Q93. Routing policies in Route 53?**

**Answer (Natural):**  
Simple, Weighted, Latency, Failover, Geolocation, Geoproximity, and Multivalue answer—solve different traffic steering needs.


👉 **Example:** Latency-based routing to nearest region; failover to DR site.


---

**Q94. Health checks integration?**

**Answer (Natural):**  
Route 53 can attach health checks to records and remove unhealthy endpoints from DNS answers. Combine with CloudWatch alarms.


👉 **Example:** Failover record switches to secondary if health check fails.


---

**Q95. Monitoring stack on AWS?**

**Answer (Natural):**  
CloudWatch metrics/alarms/logs, X-Ray for traces, CloudTrail for API audit, Config for drift/compliance. Centralize in an observability account.


👉 **Example:** Set SLOs and dashboards per service with alarms on error budget burn.


---

**Q96. Cost levers you actually control?**

**Answer (Natural):**  
Right-size (compute/storage), pick pricing models (Savings Plans/Reserved), turn off idle, use Serverless when spiky, use S3 lifecycle, compress, and tag everything for visibility.


👉 **Example:** 100% tagged resources with cost allocation tags drive chargeback.


---

**Q97. Budgets and anomaly detection?**

**Answer (Natural):**  
Use AWS Budgets for forecast alerts and Cost Anomaly Detection for ML-based outlier spend. Alert to Slack/Email.


👉 **Example:** Alert on 20% month-over-month spike in NAT data transfer.


---
