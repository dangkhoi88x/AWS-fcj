---
title: "Week 7 Worklog"
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Goals

- Strengthen understanding of **AWS Security and Encryption**.
- Learn to design and manage secure **Amazon VPC** networking environments.
- Understand and implement **Disaster Recovery (DR)** and backup strategies to ensure data availability and system resilience.

---

### Weekly Tasks Overview

| Day | Activity                                                                                                                                                                                                                                                                                 | Start Date | End Date   | Reference                          |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | ---------- | ---------------------------------- |
| 1   | - Studied **AWS Security & Encryption** <br> + Learned about **AWS Key Management Service (KMS)** for data encryption <br> + Explored **CloudHSM** for hardware-based key protection <br> + Studied **AWS Shield** and **Web Application Firewall (WAF)** for application-layer security | 10/20/2025 | 10/20/2025 | <https://aws.amazon.com/security/> |
| 2   | - Practiced **KMS and Shield/WAF** configuration <br> + Created and managed **Customer Managed Keys (CMKs)** in KMS <br> + Configured **AWS WAF rules** to block malicious traffic <br> + Tested **AWS Shield Standard** for DDoS protection                                             | 10/21/2025 | 10/21/2025 | <https://docs.aws.amazon.com/kms/> |
| 3   | - Learned the fundamentals of **Amazon VPC** <br> + Created **VPC, Subnets, Route Tables, and Internet Gateway** <br> + Configured **Security Groups** and **Network ACLs** to manage inbound/outbound access <br> + Connected **EC2** and **RDS** within the VPC                        | 10/22/2025 | 10/22/2025 | <https://aws.amazon.com/vpc/>      |
| 4   | - Advanced **VPC Networking** concepts <br> + Established **VPN Connection** between on-premise and AWS VPC <br> + Explored **AWS Direct Connect** for dedicated network links <br> + Designed a **secure segmented network** using private and public subnets                           | 10/23/2025 | 10/23/2025 | <https://docs.aws.amazon.com/vpc/> |
| 5   | - Focused on **Disaster Recovery & Backup** <br> + Used **AWS Backup** to automate resource backups <br> + Configured **RDS Read Replicas** for high availability <br> + Set up **Cross-Region Failover** for disaster recovery (DR) simulation                                          | 10/24/2025 | 10/24/2025 | <https://aws.amazon.com/backup/>   |
| 6   | - Week 7 Review and Summary <br> + Validated **VPC**, **Backup**, and **WAF** setups <br> + Created a short **security evaluation report** <br> + Summarized learnings about Security, Networking, and Disaster Recovery                                                                 | 10/25/2025 | 10/25/2025 | -                                  |

---

### Week 7 Achievements

- Strengthened knowledge of **AWS Security and Encryption**:

  - Configured and managed **KMS Keys** for data encryption and decryption.
  - Understood how **CloudHSM** enables hardware-based key management.
  - Implemented **AWS WAF** and **Shield** to protect web applications from DDoS and intrusion attacks.

- Mastered **Amazon VPC Networking and Security**:

  - Built and configured **VPCs**, **Subnets**, **Route Tables**, **Internet Gateways**, and **NAT Gateways**.
  - Applied **Security Groups** and **Network ACLs** to control inbound and outbound traffic.
  - Learned how to connect on-premise environments securely via **VPN** and **Direct Connect**.

- Developed strong understanding of **Disaster Recovery and Data Resilience**:

  - Automated backups using **AWS Backup**.
  - Configured **RDS Read Replicas** for redundancy and failover.
  - Simulated **Cross-Region Failover** to ensure system availability during disasters.

- Enhanced ability to design **secure, reliable, and fault-tolerant AWS architectures**:
  - Combined best practices across **Security**, **Networking**, and **Resilience**.
  - Prepared foundational knowledge for the next stage — **Automation & Infrastructure as Code (IaC)** using CloudFormation, CDK, and Terraform.
