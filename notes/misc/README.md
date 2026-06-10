# AWS Services Glossary — SAA Exam Prep (Updated)

## Compute

| Service | One-Liner | Key Use Case |
|---|---|---|
| **EC2** | Virtual servers in the cloud | Custom workloads, full OS control |
| **EC2 Instance Store** | Ephemeral high-performance storage attached to EC2 host | Maximum I/O (data lost on stop) |
| **Lambda** | Serverless functions, event-driven (128 MB–10 GB, 15 min max) | Short tasks, APIs, automation |
| **Fargate** | Serverless containers (ECS/EKS) | Run containers without managing servers |
| **ECS** | AWS-native container orchestration | Containerized apps, simpler than K8s |
| **ECS Anywhere** | ECS managing on-prem servers (EXTERNAL launch type) | Hybrid container management |
| **EKS** | Managed Kubernetes | When you specifically need Kubernetes |
| **Elastic Beanstalk** | PaaS for traditional web apps (Java/PHP/.NET/Ruby/Python/Node) | Quick deploy, env URL swapping |
| **App Runner** | Modern PaaS for containers/source code | Simpler alternative to Beanstalk |
| **App2Container** | Containerize existing .NET/Java apps | Modernizing legacy apps |
| **Auto Scaling Groups** | Automatically scale EC2 fleets | Maintain capacity based on demand |

## EC2 Pricing Models

| Model | Discount | Best For |
|---|---|---|
| **On-Demand** | None | Short-term, unpredictable |
| **Spot** | Up to 90% off | Fault-tolerant, interruptible (never databases) |
| **Reserved Instances (Standard)** | Up to 72% off | Steady, predictable, 24/7 workloads |
| **Reserved Instances (Convertible)** | Up to 54% off | Need flexibility to change instance family |
| **Compute Savings Plan** | Up to 66% off | Covers EC2 + Fargate + Lambda |
| **EC2 Instance Savings Plan** | Up to 72% off | EC2 only, specific family + Region |
| **Dedicated Hosts** | Premium | BYOL licensing (socket visibility) |

## EC2 Instance Families

| Family | Optimized For |
|---|---|
| **M-family** | General purpose (balanced) |
| **C-family** | Compute-optimized (CPU-intensive) |
| **R-family** | Memory-optimized (in-memory workloads) |
| **T-family** | Burstable (low baseline, occasional bursts — bad for sustained load) |

## Storage

| Service | Type | Best For |
|---|---|---|
| **S3** | Object storage | Files, backups, static sites, data lakes |
| **S3 Standard** | Frequent access | Active data |
| **S3 Intelligent-Tiering** | Auto-tiering | Unpredictable access patterns |
| **S3 Standard-IA** | Infrequent access | Predictable infrequent access |
| **S3 One Zone-IA** | Single-AZ infrequent | Recreatable data |
| **S3 Glacier Instant Retrieval** | Archive, instant access | Quarterly access, rare retrieval |
| **S3 Glacier Flexible Retrieval** | Archive, mins-hours | Few times per year |
| **S3 Glacier Deep Archive** | Cheapest archive, 12+ hrs | 7–10+ year retention |
| **S3 Object Lock** | WORM (immutable) | Compliance/regulatory |
| **S3 Bucket Keys** | Reduce KMS API calls with SSE-KMS | Cost optimization for encrypted buckets |
| **EBS** | Block storage for EC2 (persistent, AZ-locked) | Single-instance storage, databases |
| **EBS Fast Snapshot Restore (FSR)** | Pre-warms restored volumes for instant full IOPS | Cloning volumes with consistent performance |
| **EFS** | NFS shared file system | Multi-instance Linux file sharing |
| **FSx for Windows** | SMB/NTFS file shares | Windows file workloads |
| **FSx for Lustre** | High-performance parallel FS | HPC, ML training |
| **FSx for NetApp ONTAP** | Multi-AZ block storage via iSCSI | Enterprise apps, multi-AZ block storage |
| **Storage Gateway (S3 File Gateway)** | Hybrid file storage backed by S3 (NFS/SMB) | On-prem files in S3 |
| **Storage Gateway (Volume Gateway — Cached)** | Primary data in S3, local cache | Extend on-prem block storage to cloud |
| **Storage Gateway (Volume Gateway — Stored)** | Primary data on-prem, backup to S3 | Full local copy + cloud backup |
| **Storage Gateway (Tape Gateway)** | Virtual tape | Replace physical tape backups |

## Databases

| Service | Type | Best For |
|---|---|---|
| **RDS** | Managed relational | Application backends, OLTP |
| **RDS Custom** | Managed RDS with OS access | Need to SSH into the DB host |
| **Aurora** | Cloud-native MySQL/PostgreSQL | High-performance OLTP, read scaling, auto-scaling storage |
| **Aurora Serverless** | Auto-scaling Aurora | Variable/unpredictable workloads, cost optimization |
| **Aurora Cloning** | Instant copy-on-write clone | Dev/test without affecting production |
| **DynamoDB** | NoSQL key-value | Single-digit ms latency at scale |
| **DynamoDB On-Demand** | Pay-per-request capacity | Unpredictable/bursty traffic |
| **DynamoDB Provisioned + Auto-Scaling** | Pre-set capacity that adjusts | Predictable traffic, cost-effective |
| **DynamoDB DAX** | In-memory cache for DynamoDB | Microsecond reads, no code changes |
| **DynamoDB TTL** | Auto-delete expired items (free) | Clean up old data automatically |
| **DynamoDB PITR** | Point-in-time recovery (35 days) | Protect against data corruption |
| **DynamoDB Global Tables** | Multi-Region replication | Geographic redundancy, HA |
| **DynamoDB Streams** | Change stream of table modifications | Trigger Lambda on data changes |
| **Redshift** | Data warehouse (OLAP) | Petabyte-scale analytics, BI |
| **RDS Proxy** | Database connection pool | Lambda + RDS, faster failover |
| **AWS Backup** | Centralized backup across services | Cross-Region backup, >35 day retention |

## Networking

| Service | Purpose |
|---|---|
| **VPC** | Isolated virtual network (min /28, max /16) |
| **Subnet** | AZ-bound IP range within a VPC (5 reserved IPs each) |
| **Security Group** | Stateful firewall (ENI-level, allow-only) |
| **NACL** | Stateless firewall (subnet-level, allow + deny) |
| **Route Table** | Defines where traffic goes (local route always wins) |
| **Internet Gateway (IGW)** | Bidirectional public internet access |
| **NAT Gateway** | Outbound-only internet for private subnets (AZ-scoped, not free) |
| **NAT Instance** | Self-managed NAT on EC2 (legacy) |
| **Egress-Only IGW** | Outbound IPv6 only (free) |
| **VPC Peering** | One-to-one VPC connection (no transitive routing) |
| **Transit Gateway** | Hub for many VPCs + on-prem (transitive routing) |
| **VPC Endpoint (Gateway)** | Private access to S3/DynamoDB (free) |
| **VPC Endpoint (Interface/PrivateLink)** | Private access to other AWS services (paid) |
| **Customer Gateway** | On-prem side of VPN |
| **Virtual Private Gateway (VGW)** | AWS side of VPN, attached to one VPC |
| **Site-to-Site VPN** | Encrypted tunnel over internet (quick, cheap) |
| **Direct Connect (DX)** | Dedicated private fiber to AWS (slow setup, expensive, consistent) |
| **Direct Connect Gateway** | One DX connection to multiple Regions |

## Load Balancers & Edge

| Service | Layer | Best For |
|---|---|---|
| **Application Load Balancer (ALB)** | Layer 7 (HTTP/HTTPS) | Web apps, microservices |
| **Network Load Balancer (NLB)** | Layer 4 (TCP/UDP) | High-performance, UDP, static IPs |
| **Gateway Load Balancer (GWLB)** | Layer 3 | Third-party security appliances ("bump in the wire") |
| **CloudFront** | Global CDN (HTTP/HTTPS only) | Static + dynamic content, multiple origins |
| **Global Accelerator** | TCP/UDP global routing | Non-HTTP apps, multi-region failover |
| **Route 53** | Managed DNS | Domain routing, health checks |
| **API Gateway** | Managed API frontend | Serverless APIs, REST, WebSocket |

## Route 53 Routing Policies

| Policy | Behavior |
|---|---|
| **Failover** | Active-passive (primary → secondary on failure) |
| **Weighted** | Distribute traffic by percentage |
| **Multivalue Answer** | Returns up to 8 healthy records randomly (basic LB at DNS level) |
| **Latency** | Route to lowest-latency Region |
| **Geolocation** | Route by user's location |

## Security & Identity

| Service | Purpose |
|---|---|
| **IAM** | Identity and access management (within one account) |
| **IAM Identity Center (AWS SSO)** | Workforce SSO across multiple AWS accounts |
| **Cognito** | Customer/end-user authentication for your apps |
| **Cognito User Pool Authorizer** | Native API Gateway auth via Cognito (no code) |
| **AWS Organizations** | Manage multiple AWS accounts |
| **SCPs** | Org-wide preventive permission guardrails |
| **Tag Policies** | Define valid tag keys/values across the org |
| **KMS** | Encryption key management |
| **KMS Multi-Region Keys** | Same key across Regions |
| **CloudHSM** | Dedicated hardware security module (FIPS 140-2 L3) |
| **Secrets Manager** | Store + auto-rotate secrets (native RDS/Aurora rotation, multi-Region) |
| **Parameter Store (SSM)** | Store config + simple secrets (no auto-rotation) |
| **ACM** | Free SSL/TLS certs (CloudFront certs MUST be us-east-1) |
| **WAF** | Web app firewall (SQLi, XSS) — works on CloudFront, ALB, API Gateway, AppSync |
| **Shield Standard** | Free basic DDoS protection (automatic) |
| **Shield Advanced** | Premium DDoS protection — works on NLB, ALB, CloudFront, Route 53, Global Accelerator |
| **Firewall Manager** | Centrally manage WAF/Shield across accounts |
| **Network Firewall** | Managed stateful VPC traffic inspection (replaces on-prem inspection appliance) |
| **GuardDuty** | Threat detection from log analysis (alerts only) |
| **Inspector** | Vulnerability scanning (EC2, containers) |
| **Macie** | Discover sensitive data (PII) in S3 |
| **CloudTrail** | API call audit log ("who did what") |
| **AWS Config** | Resource configuration tracking + compliance rules |

## IAM Policy Types

| Type | Attached To | Answers |
|---|---|---|
| **Identity-based (IAM policy)** | User, group, role | "What can this identity do?" |
| **Resource-based policy** | S3 bucket, SQS, Lambda, etc. | "Who can access this resource?" |
| **Trust policy** | IAM role | "Who can assume this role?" |
| **Instance profile** | EC2 instance | Wrapper connecting EC2 to an IAM role |
| **SCP** | Org OU/account | Org-wide permission ceiling |
| **Permissions boundary** | User or role | Caps max permissions |

## Messaging & Streaming

| Service | Purpose |
|---|---|
| **SNS** | Pub/sub messaging (fan-out to many) |
| **SQS Standard** | Queue (one message → one consumer) |
| **SQS FIFO** | Ordered queue with deduplication |
| **EventBridge** | Event routing between AWS services + SaaS; reacts to API calls |
| **Kinesis Data Streams** | Real-time streaming (multi-consumer, replay, ~200ms) |
| **Kinesis Data Firehose** | Managed delivery to S3/Redshift/OpenSearch (~60s buffer) |
| **Kinesis Data Analytics** | Real-time SQL/Flink on streaming data |
| **Amazon Pinpoint** | Marketing campaigns + two-way SMS + analytics |

## Data Processing & Analytics

| Service | Purpose |
|---|---|
| **Athena** | Serverless SQL on S3 (ad-hoc queries) |
| **Glue** | Serverless ETL service (low overhead) |
| **Glue Data Catalog** | Central metadata repository |
| **Glue Crawlers** | Auto-discover schemas |
| **Glue DynamoDB Export Connector** | Analyze DynamoDB without consuming RCUs |
| **EMR** | Big data clusters (Spark, Hadoop, Hive, Presto) |
| **EMR Runtime Roles** | Per-team IAM permissions on shared EMR cluster |
| **QuickSight** | BI dashboards (access via QuickSight users/groups, not IAM) |
| **Lake Formation** | Data lake with fine-grained (column/row-level) permissions |
| **OpenSearch** | Real-time search + log analytics |

## AI/ML (Match Service to Input Type)

| Service | Input → Output |
|---|---|
| **Rekognition** | Images/video → analysis, content moderation |
| **Comprehend** | Text → sentiment, entities (NLP) |
| **Comprehend Medical** | Medical text → medical entities |
| **Textract** | Documents/PDFs → extracted text, forms, tables |
| **Transcribe** | Audio → text (speaker diarization) |
| **Transcribe Medical** | Medical audio → text |
| **Translate** | Text → translated text |
| **Polly** | Text → speech |
| **SageMaker** | Build/train/deploy custom ML models |
| **A2I** | Human-in-the-loop ML review |
| **Elastic Transcoder / MediaConvert** | Convert video to mobile-optimized formats |

## Caching

| Service | Purpose |
|---|---|
| **ElastiCache for Redis** | In-memory cache — persistent, HA, sorted sets (leaderboards) |
| **ElastiCache for Memcached** | In-memory cache — simple, ephemeral, multi-threaded |
| **DynamoDB DAX** | DynamoDB-specific cache (transparent, no code changes) |
| **CloudFront** | Edge caching for web content |

## Migration & Transfer

| Service | Purpose |
|---|---|
| **DataSync** | Automated network transfer (NFS/SMB ↔ S3/EFS/FSx); bandwidth throttling |
| **DMS** | Database Migration Service |
| **AWS Transfer Family** | Managed SFTP/FTPS/FTP to S3 |
| **Snowcone** | 8 TB physical transfer device |
| **Snowball Edge** | Up to 80 TB physical transfer device |

## Monitoring & Operations

| Service | Purpose |
|---|---|
| **CloudWatch** | Metrics, logs, alarms ("how is it performing") |
| **CloudWatch Agent** | Collects OS-level metrics (memory, disk usage) + app logs — not built-in |
| **CloudWatch Detailed Monitoring** | 1-minute EC2 metrics (vs. 5-min basic) |
| **Trusted Advisor** | Best practice recommendations |
| **Cost Explorer** | Cost analysis + granular filtering (low overhead) |
| **AWS Budgets** | Proactive cost/usage alerts |
| **Cost and Usage Reports (CUR)** | Detailed billing data for deep analytics |
| **Resource Groups Tag Editor** | Find/inventory resources by tag across all services + Regions |
| **Systems Manager Run Command** | Execute commands on EC2 fleets (third-party software) |
| **Systems Manager Patch Manager** | OS patching only |
| **Systems Manager Session Manager** | SSH/RDP without opening ports |
| **Systems Manager Maintenance Windows** | Schedule patching/automation tasks |
| **AWSEC2-PatchLoadBalancerInstance** | Automation doc: deregister → patch → re-register |

## Disaster Recovery Strategies

| Strategy | RTO | Cost | Description |
|---|---|---|---|
| **Backup & Restore** | Hours | Lowest | Restore from backups during failover |
| **Pilot Light** | ~30 min | Low | Minimal infra + data replication, scale up on failover |
| **Warm Standby** | Minutes | Medium | Scaled-down running copy, scale up on failover |
| **Multi-Site Active-Active** | Near-zero | Highest | Full capacity in both Regions simultaneously |

## Common Exam Phrases → Service

| Phrase | Service |
|---|---|
| "Least operational overhead" | Serverless / managed services |
| "Real-time streaming" | Kinesis Data Streams |
| "Load streaming data into S3/Redshift" | Kinesis Firehose |
| "Decouple components" | SQS or SNS |
| "Multiple consumers of same event" | SNS → SQS fan-out |
| "Order matters" | SQS FIFO |
| "Scale worker fleet on queue depth" | Backlog per instance metric |
| "Notify when AWS API is called" | EventBridge rule → SNS |
| "Notify on configuration drift" | AWS Config + EventBridge + SNS |
| "Extract text from documents" | Textract |
| "Audio with multiple speakers" | Transcribe |
| "Two-way SMS / marketing campaigns" | Pinpoint |
| "Find resources by tag across Regions" | Resource Groups Tag Editor |
| "EC2 to S3 cost reduction / private" | Gateway VPC Endpoint |
| "Windows file shares" | FSx for Windows |
| "Multi-instance Linux file sharing" | EFS |
| "Multi-AZ block storage" | FSx for NetApp ONTAP |
| "Petabyte-scale analytics" | Redshift |
| "Ad-hoc SQL on S3" | Athena |
| "Real-time search" | OpenSearch |
| "Fine-grained data lake permissions" | Lake Formation |
| "Restrict S3 to AWS Org" | aws:PrincipalOrgID |
| "Auto-delete old DynamoDB items" | TTL |
| "Speed up DynamoDB reads, no code change" | DAX |
| "Leaderboard / rankings" | ElastiCache Redis |
| "Bulk transfer + bandwidth control" | DataSync |
| "Bulk transfer, tight deadline, limited bandwidth" | Snowball |
| "Containerize legacy .NET/Java" | App2Container |
| "Run containers, least overhead" | Fargate |
| "Cross-account access" | IAM role with trust policy |
| "SQL injection / XSS" | WAF |
| "Large sophisticated DDoS" | Shield Advanced |
| "Traffic inspection/filtering in VPC" | Network Firewall |
| "Third-party security appliance" | Gateway Load Balancer |
| "In-memory workload performance" | R-family instances |
| "Encrypt before S3 + customer-specific keys" | CSE-KMS |
| "Reduce KMS API costs" | S3 Bucket Keys |
| "Random traffic to all instances" | Route 53 multivalue answer |
| "30-min RTO + data loss OK" | Pilot Light DR |
| "Clone EBS fast + consistent IOPS" | EBS Fast Snapshot Restore |

## Memory Hacks

- **Lambda + RDS** → use **RDS Proxy** for connection pooling
- **EC2 ↔ S3 privately** → **Gateway VPC Endpoint** (free)
- **CloudFront** = HTTP only; **Global Accelerator** = TCP/UDP
- **EBS** = single instance, persistent, AZ-locked; **EFS** = multi-instance Linux; **FSx Windows** = multi-instance Windows; **Instance store** = ephemeral
- **CloudTrail** = "who did it"; **CloudWatch** = "how is it doing"; **Config** = "what changed"
- **Object Lock Compliance** = no one overrides (not even root); **Governance** = privileged users can override; **Legal Hold** = indefinite, no end date
- **PITR** = DynamoDB data corruption; **Global Tables** = DynamoDB regional DR
- **WAF** works on CloudFront/ALB/API Gateway/AppSync — NOT NLB or Global Accelerator
- **CloudFront certs** must be in **us-east-1**
- **DataSync** needs bandwidth; **Snowball** is for when you don't have it
- **DynamoDB on-demand** = unpredictable; **provisioned + auto-scaling** = predictable + cost-effective
- **Redis** = persistent, HA, leaderboards; **Memcached** = simple, ephemeral
- **Secrets Manager** = auto-rotation; **Parameter Store** = no auto-rotation
- **Glue** = serverless ETL (low overhead); **EMR** = cluster control (Hive/HBase/Presto)
- **Athena** = SQL on S3; **OpenSearch** = real-time search; **Redshift** = data warehouse
- **EMR runtime roles** = per-team permissions + block IMDSv2
- **CloudWatch Agent** required for memory/disk metrics — NOT built-in
- **24/7 workload** → Reserved Instances; **storage grows** → Aurora auto-scaling
- **On-Demand (RDS/DynamoDB)** = pricing model, NOT "scales on demand"
- **Cross-account** = IAM role + trust policy, never shared IAM users
- **Lake Formation** = the only option for fine-grained (column/row) data lake permissions
- **Read each option carefully** — wrong answers often contain technical errors (e.g., "gateway endpoint with security groups")