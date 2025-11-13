---
title: "Proposal Document"
date: 2025-09-10
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Digital Transformation for Mini-market on AWS Cloud

## E-commerce .NET 3-tier Solution with Repository and Unit of Work Pattern

### 1. Executive Summary

This proposal presents an end-to-end solution for **“Digital Transformation for Mini-market on AWS Cloud.”** Traditional mini-markets are currently facing three major challenges:  
(1) Manual inventory management (Excel/notebooks) leading to revenue loss and resource waste;  
(2) Complete dependence on offline sales, missing the rapidly growing e-commerce market and losing competitiveness; and  
(3) Slow operational workflow (such as manually checking prices), resulting in poor customer experience.

Our solution is to build a fully integrated e-commerce and operational management platform. For software architecture, we adopt a **.NET 3-tier architecture** (ASP.NET Core MVC, EF Core) with **Repository Pattern** and **Unit of Work Pattern**. For infrastructure architecture, we design the system based on the **AWS Well-Architected Framework**, deployed on **AWS Elastic Beanstalk** (for .NET backend), **Amazon RDS for SQL Server** (database), and **Amazon S3** (static assets). The system’s performance is optimized using **CloudFront** and **ElastiCache**, and security is ensured by **AWS WAF**, **VPC**, and **NAT Gateway**. Deployment is fully automated through a **CI/CD pipeline** integrated with GitHub.

The business benefits are immediate: automated inventory management (reducing losses) and opening a new online revenue channel. Infrastructure costs in the first 12 months are close to zero thanks to AWS Free Tier (e.g., RDS Express Edition, EC2 t3.micro). Long-term operational expenses (after Free Tier) are also practical—**only about $138.06/month** for the entire architecture. Because the initial investment is minimal while solving major operational pain points, the ROI is extremely high and nearly instantaneous.

The project is planned over **11 weeks**, divided into 4 phases:  
(1) Foundation & Architecture,  
(2) Core Feature Development,  
(3) AWS Integration & CI/CD,  
(4) Finalization & Deployment.

Expected results will be measured with specific success metrics:

- 90% reduction in inventory errors
- 50% faster checkout process
- 20% revenue from online channel in the first 6 months

This solution not only addresses the current problems but also provides a scalable foundation for future data-driven decisions.

---

### 2. Problem Statement

_Current Situation_  
Small and medium-sized retail shops—especially traditional mini-markets in Vietnam—still rely on outdated manual workflows. As the market becomes increasingly digitized, the lack of technology adoption creates several issues that directly impact their ability to survive and grow.

_Key Problems_

- **Manual Inventory Management → Inaccuracy & resource waste:**  
  Most mini-markets manage thousands of SKUs using notebooks or Excel. Manual entry for stock-in/out is highly error-prone (wrong SKUs, wrong quantities), leading to mismatches between recorded and actual inventory. Staff spend hours daily counting stock instead of serving customers. This leads to financial losses of **5–10% of inventory value** each month.

- **Dependency on offline sales → Missed e-commerce market:**  
  Traditional mini-markets rely heavily on walk-in customers, limited by location and local customer base. They miss out on the rapidly growing online market, especially younger customers accustomed to online shopping. Without online sales (delivery, 24/7 ordering), they cannot compete with Circle K, 7-Eleven, Grab, or Shopee.

- **Slow operations → Poor customer experience:**  
  Price checks, promotions, and product information are done manually by staff, leading to slow service and long waiting times. This creates frustration, reduces service capacity during peak hours, and damages perceived professionalism.

---

### 3. Solution Architecture

The architecture is designed to solve the problems above by combining **.NET 3-tier architecture** with AWS fully managed services. It follows the **AWS Well-Architected Framework** to ensure security, performance, resiliency, and cost optimization.

![Mini-market Architecture](/images/2-Proposal/project_architecture_3.1.png)

_AWS Services Used_

- **AWS Elastic Beanstalk:**  
  PaaS chosen to deploy the .NET 3-tier application (Presentation + Application layers). Beanstalk automates infrastructure management and creates an Auto Scaling Group for scalability and cost efficiency.

- **Amazon RDS (SQL Server):**  
  A managed relational database service hosting the Persistence Layer. SQL Server is selected for compatibility with the existing .NET system. RDS handles daily backups, patching, and failover automatically. The DB is placed in a Private Subnet for security. We start with **SQL Server Express Edition (Free Tier)** to minimize cost.

- **Amazon S3:**  
  Stores static assets (product images, CSS, JS). Highly scalable and cost-efficient.

- **Amazon CloudFront:**  
  A global CDN caching static files from S3 for faster page load and reduced load on Beanstalk.

- **AWS WAF & Route 53:**  
  WAF protects against common attacks (SQL injection, XSS). Route 53 provides DNS routing.

- **Amazon ElastiCache (Redis):**  
  In-memory caching for frequently accessed data, reducing load on RDS.

- **NAT Gateway:**  
  Allows instances in private subnets to securely access the internet (for patching, updates).

- **AWS CodePipeline / CodeBuild:**  
  Automates building, testing, and deploying the application to Elastic Beanstalk.

_Data Flow_

- Users → Route 53 → CloudFront → (Static assets) S3
- Dynamic requests → CloudFront → ALB → Elastic Beanstalk
- Application → ElastiCache → RDS
- Patch/update traffic → Beanstalk → NAT Gateway → Internet
- CI/CD flow → GitHub push → CodePipeline → CodeBuild → Elastic Beanstalk

---

### 4. Technical Implementation

_Project Phases_

1. **Build Technical Foundation** (Weeks 1–4)  
   Finalize data models, set up .NET 3-tier architecture, create GitHub repository, and study AWS services.

2. **Develop Core Features** (Weeks 5–7)  
   Build Persistence Layer (Repositories, UoW), Application Layer (Services), and WebShop (Login, Cart, Payment). Begin unit testing.

3. **Integrate AWS Services** (Weeks 8–10)  
   Integrate S3 for product images, ElastiCache for caching, and build a complete CI/CD pipeline to deploy to Staging.

4. **Finalize & Deploy** (Week 11)  
   Configure CloudFront, WAF, Route 53. Deploy v1.0 to Production on Elastic Beanstalk and enable CloudWatch monitoring.

_Technical Requirements_

- Backend: ASP.NET Core MVC 9.0
- ORM: Entity Framework Core
- Database: SQL Server 2022 (local) & Amazon RDS SQL Server
- Frontend: Bootstrap 5, jQuery
- AWS: Beanstalk, RDS, VPC, S3, CloudFront, WAF, ElastiCache, NAT Gateway
- Tools: Visual Studio 2022, Docker Desktop

_Development Methodology_

- Agile (Scrum-like)
- Kanban board for tracking tasks
- Code reviews via GitHub merge requests
- Continuous integration and automated deployments

_Testing Strategy_

1. **Unit Testing** – Application Layer
2. **Integration Testing** – Staging environment
3. **User Acceptance Testing** – Production environment

_CI/CD Strategy_

- GitHub push → triggers AWS CodePipeline
- CodeBuild builds, tests, packages .NET app
- CodePipeline deploys to Elastic Beanstalk (Staging → Production)

---

### 5. Timeline & Milestones

- **Phase 1 (Weeks 1–4):** Architecture foundation, repository setup, AWS environment (VPC, subnets)
- **Phase 2 (Weeks 5–7):** Core features (Auth, Products, Cart, Checkout), unit tests
- **Phase 3 (Weeks 8–10):** AWS integrations (S3, ElastiCache), CI/CD pipeline
- **Phase 4 (Week 11):** Deployment, UAT, monitoring setup

---

### 6. Budget Estimation

Using AWS Pricing Calculator (link provided).  
Estimated monthly cost: **$138.06** (post Free Tier).  
Includes:

- EC2 t3a.small: $19.15
- ALB: $18.69
- NAT Gateway: $46.02
- RDS db.t3.micro: $25.86
- ElastiCache t4g.micro: $17.52
- WAF: $7.20
- CloudFront: $2.43
- Others (S3, Route 53, CodeBuild)

**First-year cost is nearly zero** because Free Tier covers most services except NAT Gateway and WAF.

ROI is extremely high due to low cost and immediate operational benefits.

---

### 7. Risk Assessment

1. **Technical: System Overload**

   - Mitigation: ElastiCache, Auto Scaling, CloudFront
   - Backup: CloudWatch Alarms, vertical scaling for RDS

2. **Business: Low User Adoption**

   - Mitigation: Simple UI, early shop-owner feedback
   - Backup: Additional Sprint for UX improvements

3. **Operational: Data Loss / Breach**
   - Mitigation: Daily RDS backups, Private Subnets, WAF, secret management
   - Backup: RDS PITR recovery

---

### 8. Expected Outcomes

Expected business results:

- 90% reduction in inventory errors
- 50% faster checkout
- 20% revenue from online channel within 6 months

Expected technical results:

- 99.9% uptime
- <2 seconds average page load time (CloudFront + caching)
- Stable CI/CD deployment frequency

Value over time:

- **Short-term:** Better UX and faster operations
- **Medium-term:** Online customer acquisition, business analytics
- **Long-term:** Data-driven decisions and scalable multi-shop expansion
