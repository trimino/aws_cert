# AWS Services Cheat Sheet

A quick reference of every AWS service we've covered, organized by category.

## Compute

| Service | One-Liner | Key Use Case |
|---|---|---|
| **EC2** | Virtual servers in the cloud | Custom workloads, full OS control |
| **EC2 Instance Store** | Ephemeral high-performance storage attached to EC2 host | Maximum I/O for processing (data lost on stop) |
| **Lambda** | Serverless functions, event-driven | Short tasks, APIs, automation, event processing |
| **Fargate** | Serverless containers (ECS/EKS) | Run containers without managing servers |
| **ECS** | AWS-native container orchestration | Containerized apps, simpler than K8s |
| **EKS** | Managed Kubernetes | When you specifically need Kubernetes |
| **Elastic Beanstalk** | PaaS for traditional web apps | Quick deployment, multi-language support |
| **App Runner** | Modern PaaS for containers/source code | Newer, simpler alternative to Beanstalk |
| **Auto Scaling Groups** | Automatically scale EC2 fleets | Maintain capacity based on demand |
| **Launch Template** | Modern instance template (versioning, mixed types) | Used by ASG (replaces Launch Configuration) |
| **Capacity Reservations** | Guarantee EC2 capacity in a specific AZ | Short-term events, no long commitment |

## EC2 Pricing Models

| Model | Discount | Best For |
|---|---|---|
| **On-Demand** | None | Short-term, unpredictable |
| **Spot** | Up to 90% off | Fault-tolerant, batch, interruptible |
| **Reserved Instances (Standard)** | Up to 72% off | Steady, predictable, EC2-only |
| **Reserved Instances (Convertible)** | Up to 54% off | Need flexibility to change instance family |
| **Compute Savings Plan** | Up to 66% off | Covers EC2 + Fargate + Lambda, most flexible |
| **EC2 Instance Savings Plan** | Up to 72% off | EC2 only, specific family + Region |
| **Dedicated Instances** | Premium | Compliance, hardware isolation |
| **Dedicated Hosts** | Premium | BYOL licensing (socket visibility) |

## Storage

| Service | Type | Best For |
|---|---|---|
| **S3** | Object storage | Files, backups, static websites, data lakes |
| **S3 Standard** | Frequent access | Active data |
| **S3 Intelligent-Tiering** | Auto-tiering | Unpredictable access patterns |
| **S3 Standard-IA** | Infrequent access | Predictable infrequent access |
| **S3 One Zone-IA** | Single-AZ infrequent | Recreatable data, cost-optimized |
| **S3 Glacier Instant Retrieval** | Archive, instant access | Quarterly access, rare retrieval |
| **S3 Glacier Flexible Retrieval** | Archive, mins-hours | Few times per year, delay OK |
| **S3 Glacier Deep Archive** | Cheapest archive, 12+ hours | 7-10+ year retention |
| **S3 Object Lock** | WORM (immutable) | Compliance, regulatory requirements |
| **EBS** | Block storage for EC2 | Single-instance storage, databases |
| **EFS** | NFS shared file system | Multi-instance Linux file sharing |
| **FSx for Windows** | SMB/NTFS file shares | Windows file workloads |
| **FSx for Lustre** | High-performance parallel FS | HPC, ML training |
| **FSx for NetApp ONTAP** | NetApp features | Enterprise apps with NetApp needs |
| **Storage Gateway (S3 File Gateway)** | Hybrid file storage backed by S3 | On-prem files in S3 |
| **Storage Gateway (FSx File Gateway)** | Hybrid Windows file storage | On-prem cache for FSx |
| **Storage Gateway (Volume Gateway)** | Hybrid block storage | On-prem block backup to S3 |
| **Storage Gateway (Tape Gateway)** | Virtual tape | Replace physical tape backups |

## Databases

| Service | Type | Best For |
|---|---|---|
| **RDS** | Managed relational (MySQL, PostgreSQL, etc.) | Application backends, OLTP |
| **RDS Custom** | Managed RDS with OS access | When you need to SSH into the database host |
| **Aurora** | Cloud-native MySQL/PostgreSQL | High-performance OLTP, read scaling, cloning |
| **Aurora Multi-AZ** | HA Aurora | Production workloads with replicas |
| **Aurora Cloning** | Instant database clone (copy-on-write) | Dev/test/staging without affecting production |
| **DynamoDB** | NoSQL key-value | Single-digit ms latency at scale |
| **DynamoDB PITR** | Point-in-time recovery (35 days) | Protect against data corruption |
| **DynamoDB Global Tables** | Multi-Region replication | Geographic redundancy |
| **Redshift** | Data warehouse (OLAP) | Petabyte-scale analytics, BI |
| **RDS Proxy** | Database connection pool | Lambda + RDS, faster failover |

## Networking

| Service | Purpose |
|---|---|
| **VPC** | Isolated virtual network |
| **Subnet** | AZ-bound IP range within a VPC |
| **Security Group** | Stateful firewall (instance-level, allow-only) |
| **NACL** | Stateless firewall (subnet-level, allow + deny) |
| **Route Table** | Defines where traffic goes |
| **Internet Gateway (IGW)** | Bidirectional public internet access |
| **NAT Gateway** | Outbound-only internet for private subnets |
| **Egress-Only IGW** | Outbound IPv6 only |
| **VPC Peering** | One-to-one VPC connection |
| **Transit Gateway** | Hub for many VPCs |
| **VPC Endpoint (Gateway)** | Private access to S3/DynamoDB (free) |
| **VPC Endpoint (Interface/PrivateLink)** | Private access to other AWS services |
| **PrivateLink** | Private SaaS-to-customer connectivity |
| **Customer Gateway** | On-prem side of VPN |
| **Virtual Private Gateway** | AWS side of VPN, attached to one VPC |
| **Site-to-Site VPN** | Encrypted tunnel over internet |
| **Direct Connect** | Dedicated private fiber to AWS |
| **Direct Connect Gateway** | One DX connection to multiple Regions |

## Load Balancers & Edge

| Service | Layer | Best For |
|---|---|---|
| **Application Load Balancer (ALB)** | Layer 7 (HTTP/HTTPS) | Web apps, microservices |
| **Network Load Balancer (NLB)** | Layer 4 (TCP/UDP) | High-performance, UDP, static IPs |
| **Gateway Load Balancer (GWLB)** | Layer 3 | Third-party security appliances |
| **CloudFront** | Global CDN (HTTP/HTTPS only) | Static + dynamic web content |
| **Global Accelerator** | TCP/UDP global routing | Non-HTTP apps, multi-region failover |
| **Route 53** | Managed DNS | Domain routing, health checks |
| **API Gateway** | Managed API frontend | Serverless APIs, REST, WebSocket |

## Security & Identity

| Service | Purpose |
|---|---|
| **IAM** | Identity and access management |
| **IAM Identity Center (AWS SSO)** | Multi-account SSO |
| **AWS Organizations** | Manage multiple AWS accounts |
| **KMS** | Encryption key management |
| **KMS Multi-Region Keys** | Same key across Regions |
| **Secrets Manager** | Store + auto-rotate secrets |
| **Parameter Store (SSM)** | Store config + secrets (no auto-rotation) |
| **ACM** | Free SSL/TLS certificates (issued or imported) |
| **ACM Private CA** | Internal/private certificates |
| **WAF** | Web application firewall (SQLi, XSS) |
| **Shield Standard** | Free basic DDoS protection |
| **Shield Advanced** | Premium DDoS protection ($3K/mo) |
| **Firewall Manager** | Centrally manage WAF/Shield across accounts |
| **Network Firewall** | Stateful VPC traffic inspection |
| **GuardDuty** | Threat detection (log analysis) |
| **Inspector** | Vulnerability scanning (EC2, containers) |
| **CloudTrail** | API call audit log ("who did what") |
| **AWS Config** | Resource configuration tracking + compliance rules |

## Messaging & Streaming

| Service | Purpose |
|---|---|
| **SNS** | Pub/sub messaging (fan-out to many) |
| **SQS Standard** | Queue (one message → one consumer) |
| **SQS FIFO** | Ordered queue with deduplication |
| **EventBridge** | Event routing between AWS services + SaaS |
| **Kinesis Data Streams** | Real-time streaming (multi-consumer, replay) |
| **Kinesis Data Firehose** | Managed delivery to S3/Redshift/OpenSearch |
| **Kinesis Data Analytics** | Real-time SQL/Flink on streaming data |
| **MSK** | Managed Apache Kafka |

## Data Processing & Analytics

| Service | Purpose |
|---|---|
| **Athena** | Serverless SQL on S3 |
| **Redshift Spectrum** | Query S3 from Redshift |
| **Glue** | Serverless ETL service |
| **Glue Data Catalog** | Central metadata repository |
| **Glue Crawlers** | Auto-discover schemas |
| **Glue Job Bookmarks** | Track processed data (incremental ETL) |
| **EMR** | Big data processing (Spark, Hadoop) |
| **QuickSight** | BI dashboards and visualization |
| **Lake Formation** | Data lake permissions and governance |

## AI/ML

| Service | What It Does |
|---|---|
| **Rekognition** | Image/video analysis, content moderation |
| **Comprehend** | NLP, sentiment analysis |
| **Textract** | Extract text/data from documents |
| **Translate** | Language translation |
| **Transcribe** | Speech-to-text |
| **Polly** | Text-to-speech |
| **Lex** | Chatbots |
| **Personalize** | Recommendation engine |
| **Forecast** | Time-series forecasting |
| **SageMaker** | Build/train/deploy custom ML models |
| **A2I (Augmented AI)** | Human-in-the-loop ML workflows |

## Application Integration

| Service | Purpose |
|---|---|
| **Step Functions** | Visual workflow orchestration |
| **AppFlow** | SaaS-to-AWS data transfer (no-code) |
| **Lambda Resource-Based Policy** | Who can invoke a Lambda |
| **Lambda Execution Role** | What a Lambda function can do |

## Migration & Transfer

| Service | Purpose |
|---|---|
| **DataSync** | Automated network transfer (NFS/SMB ↔ S3/EFS/FSx) |
| **DMS** | Database Migration Service |
| **Snowcone** | 8 TB physical transfer device |
| **Snowball Edge** | Up to 80 TB physical transfer device |
| **Snowmobile** | Up to 100 PB (retired 2024) |

## Monitoring & Operations

| Service | Purpose |
|---|---|
| **CloudWatch** | Metrics, logs, alarms ("how is it performing") |
| **CloudWatch Logs Subscription** | Stream logs to OpenSearch/Lambda/Firehose |
| **CloudWatch Synthetics** | Canary testing |
| **CloudWatch Container Insights** | Container monitoring |
| **Trusted Advisor** | Best practice recommendations |
| **Cost Explorer** | Cost analysis and visualization |
| **AWS Budgets** | Cost/usage alerts |
| **Cost and Usage Reports** | Detailed billing data for analytics |
| **Systems Manager Run Command** | Execute commands on EC2 fleets |
| **Systems Manager Patch Manager** | OS patching (not third-party software) |

## Compute Patterns Cheat Sheet

| Need | Right Service |
|---|---|
| **Least operational overhead serverless API** | **API Gateway + Lambda** |
| **Real-time data ingestion + transform + store** | **API Gateway → Kinesis Streams → Firehose (with Lambda) → S3** |
| **Variable workload, auto-scale** | **Lambda** (or Fargate) |
| **Fan-out to multiple consumers** | **SNS → multiple SQS queues → Lambda** |
| **Decouple from unreliable downstream** | **SQS** as buffer |
| **Periodic scheduled job** | **EventBridge schedule → Lambda** |

## Storage Decision Tree

| Need | Service |
|---|---|
| **Single EC2, high IOPS** | **EBS** (or instance store for max performance) |
| **Multiple EC2 sharing files (Linux)** | **EFS** |
| **Multiple EC2 sharing files (Windows)** | **FSx for Windows** |
| **HPC/ML parallel file system** | **FSx for Lustre** |
| **Object storage for static files** | **S3** |
| **Long-term archive (rare access)** | **Glacier Deep Archive** |
| **Immutable for compliance** | **S3 Object Lock (compliance mode)** |

## Networking Quick Picks

| Need | Service |
|---|---|
| **Free private access to S3/DynamoDB from VPC** | **Gateway VPC Endpoint** |
| **Private access to other AWS services** | **Interface VPC Endpoint (PrivateLink)** |
| **Connect 2 VPCs** | **VPC Peering** |
| **Connect many VPCs** | **Transit Gateway** |
| **On-prem to AWS, cheap, encrypted** | **Site-to-Site VPN** |
| **On-prem to AWS, dedicated, fast** | **Direct Connect** |
| **DR with cost-effective HA** | **Direct Connect + VPN backup** |
| **Multi-region failover for HTTP** | **CloudFront + Route 53** |
| **Multi-region failover for TCP/UDP** | **Global Accelerator** |
| **DDoS at scale** | **CloudFront + Shield Advanced** |

## Database Decision Tree

| Need | Service |
|---|---|
| **Relational, application backend** | **RDS** or **Aurora** |
| **Relational + need OS access** | **RDS Custom** |
| **Read scaling, MySQL/Postgres-compatible** | **Aurora with replicas** |
| **NoSQL, single-digit ms latency** | **DynamoDB** |
| **Data warehouse, complex analytics** | **Redshift** |
| **Serverless SQL on S3** | **Athena** |
| **Lambda + database connection management** | **RDS Proxy** |
| **Protect DynamoDB from data corruption** | **DynamoDB PITR** |
| **Multi-Region DynamoDB** | **Global Tables** |

## Security Decision Tree

| Need | Service |
|---|---|
| **Track API calls (who did what)** | **CloudTrail** |
| **Track config changes / compliance** | **AWS Config** |
| **Detect threats from logs** | **GuardDuty** |
| **Vulnerability scan EC2/containers** | **Inspector** |
| **DDoS protection (basic, free)** | **Shield Standard** (auto) |
| **DDoS protection (advanced)** | **Shield Advanced** + CloudFront |
| **Block SQL injection / XSS** | **WAF** |
| **Manage WAF across accounts** | **Firewall Manager** |
| **Stateful VPC firewall** | **Network Firewall** |
| **Auto-rotate database credentials** | **Secrets Manager** |
| **Store config + simple secrets** | **Parameter Store** |
| **Free SSL certificate** | **ACM** (issued) |
| **External CA certificate** | **ACM** (imported, manual rotation) |

## Common Exam Phrases → Service

| Phrase | Service |
|---|---|
| "**Least operational overhead**" | Serverless / managed services |
| "**Fully managed**" | Managed services (Lambda, Glue, Firehose) |
| "**Real-time streaming**" | Kinesis Data Streams |
| "**Load streaming data into S3/Redshift**" | Kinesis Firehose |
| "**Decouple components**" | SQS or SNS |
| "**Multiple consumers of same event**" | SNS → SQS fan-out |
| "**Order matters**" | SQS FIFO |
| "**Notify on configuration drift**" | AWS Config + EventBridge + SNS |
| "**Inappropriate content in images**" | Rekognition |
| "**Extract data from documents**" | Textract |
| "**SaaS data transfer (Salesforce, etc.)**" | AppFlow |
| "**Bulk file transfer to AWS**" | DataSync (with bandwidth) or Snowball (without) |
| "**Custom Lambda for repeating logic**" | Usually a wrong answer if a managed feature exists |
| "**EC2 to S3 cost reduction**" | Gateway VPC Endpoint |
| "**Windows file shares**" | FSx for Windows File Server |
| "**Multi-instance Linux file sharing**" | EFS |
| "**Petabyte-scale analytics**" | Redshift |
| "**Ad-hoc SQL on S3**" | Athena |

## Memory Hacks

- **Lambda + RDS** → use **RDS Proxy** to manage connections
- **EC2 ↔ S3 over NAT** → use **Gateway VPC Endpoint** (free)
- **CloudFront** = HTTP only; **Global Accelerator** = TCP/UDP
- **EBS** = single instance; **EFS** = multi-instance (Linux); **FSx Windows** = multi-instance (Windows)
- **CloudTrail** = "who did it"; **CloudWatch** = "how is it doing"; **Config** = "what does it look like and what changed"
- **Reserved Instances** = locked config; **Compute Savings Plans** = flexible (covers EC2 + Fargate + Lambda)
- **Object Lock Compliance** = no one can override (not even root); **Governance** = privileged users can override
- **PITR** for DynamoDB data corruption; **Global Tables** for regional DR
- **Aurora cloning** for instant dev/test copies (no production impact)
- **Imported ACM cert** = manual renewal + EventBridge alerts
- **Multi-region SSO** = AWS Identity Center + 2-way trust to on-prem AD
- **Shield Advanced + CloudFront** = the standard DDoS pattern
- **DataSync** needs network bandwidth; **Snowball** is for when you don't have it
