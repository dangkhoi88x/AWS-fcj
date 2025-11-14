---
title: "Proposal"
date: 2025-09-10
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Digital Transformation for Mini-market on AWS Cloud

## .NET E-commerce 3-tier Solution Applying Repository and Unit of Work Pattern

### 1. Executive Summary

This proposal presents an end-to-end solution for "Digital Transformation for Mini-market on the AWS Cloud." Traditional mini-markets are currently facing three major challenges: (1) Manual inventory management (using Excel/notebooks) leading to revenue loss and resource waste; (2) 100% reliance on offline sales, missing the fast-growing e-commerce market and losing competitiveness; and (3) Slow operational processes (such as manual price lookup), resulting in poor customer experience.

The proposed solution is to build a comprehensive e-commerce and operational management platform. For software architecture, the team will use a .NET 3-layer architecture (ASP.NET Core MVC, EF Core) combined with the Repository Pattern and Unit of Work Pattern. For infrastructure architecture, the system is designed following the AWS Well-Architected Framework, running on AWS Elastic Beanstalk (for the .NET backend), Amazon RDS for SQL Server (for the database), and Amazon S3 (for static assets). The system is performance-optimized using CloudFront and ElastiCache, and secured using WAF, VPC, and NAT Gateway. The deployment process is fully automated using a CI/CD pipeline integrated with GitHub.

The business benefits are immediate, including automated inventory management (reducing losses) and opening a new online revenue channel. In terms of investment, infrastructure cost in the first 12 months is nearly zero thanks to AWS Free Tier (e.g., RDS Express Edition, EC2 t3.micro). The long-term operational cost (after Free Tier) is also very reasonable, estimated at only 138.06 USD/month for the entire system. With minimal initial investment and the ability to directly address revenue leakage issues, the ROI is very high and nearly instantaneous.

The project is proposed to be implemented within 11 weeks, divided into four main phases: (1) Foundation & Architecture, (2) Core Feature Development, (3) AWS Integration & CI/CD, and (4) Finalization & Deployment. Expected outcomes are measured using specific success metrics: reducing inventory errors by 90%, reducing checkout time by 50%, and achieving 20% online revenue within the first 6 months. This solution not only addresses immediate problems but also provides a scalable platform for future data-driven decision-making.

### 2. Problem Statement

_Current Situation_  
Small and medium retail businesses, especially traditional mini-market models in Vietnam, are operating based on outdated manual processes. As the market becomes increasingly digital, the lack of technology adoption has created multiple issues that directly impact their ability to survive and grow.

_Key Problems_

- **Manual inventory management leads to inaccurate data and resource waste:**  
  Most mini-markets currently manage thousands of SKUs using notebooks or Excel files. Stock-in/out and end-of-day inventory checks rely entirely on manual counting and data entry. This easily leads to data errors due to frequent mistakes in product codes or quantities, causing large discrepancies between “recorded” and “actual” stock. Manual checking also requires significant labor, as employees spend hours daily counting, reconciling, and correcting reports instead of focusing on sales or customer service. Ultimately, this results in financial losses, as inaccurate data prevents owners from monitoring expired goods, damaged items, or theft, typically causing 5–10% inventory loss monthly.

- **Dependence on offline sales, missing the E-commerce market:**  
  Most mini-markets in Vietnam depend heavily on walk-in customers. They are limited by geography (serving only nearby areas) and a small group of familiar customers. Operating offline only means missing out on the growing e-commerce customer base, especially younger generations used to online shopping. They cannot compete with the convenience of 24/7 ordering or home delivery offered by major convenience store chains like Circle K or 7-Eleven and delivery apps like Grab or Shopee, resulting in customer loss over time.

- **Operational inefficiency and poor customer experience:**  
  Checkout and information lookup processes in traditional mini-markets are usually very slow. When customers ask for product prices, details, or promotions, employees (especially new ones) must search manually, wasting time. Making customers wait long causes frustration and appears unprofessional. Employees spend too much time on simple, error-prone tasks (e.g., misreading handwritten prices), reducing the number of customers served during peak hours.

### 3. Solution Architecture

The architecture is designed to address the problems above by combining a .NET 3-tier software architecture with AWS managed cloud services. This architecture follows the principles of the AWS Well-Architected Framework, ensuring security, high performance, fault tolerance, and cost optimization.

![Mini-market Architecture](/images/2-Proposal/IMG_2934.JPG)

_AWS Services Used_

- _AWS Elastic Beanstalk_: A Platform as a Service (PaaS) chosen to deploy the .NET 3-tier application (including the WebShop Presentation Layer and Application Services Layer). Beanstalk automates 100% of infrastructure management, including automatically creating an Auto Scaling Group (ASG) to ensure scalability and cost efficiency.

- _Amazon RDS (SQL Server)_: A Managed Relational Database Service that hosts the Persistence Layer. SQL Server is chosen because the .NET application has been developed and optimized for SQL Server. Using RDS for SQL Server allows migrating the application to AWS without modifying the Data Layer code. RDS automates daily backups, patching, and failover. For security, RDS is placed in a Private Subnet, preventing direct Internet access and allowing only the Beanstalk application to connect. For cost optimization, SQL Server Express Edition on RDS (Free Tier eligible) is used during the early phase.

- _Amazon S3_: Object storage used for static assets such as product images, CSS files, and JavaScript files. S3 provides extremely low cost and unlimited scalability.

- _Amazon CloudFront_: A Content Delivery Network (CDN). CloudFront caches static files from S3 at global edge locations, significantly improving page load speed and reducing load on Beanstalk, allowing the .NET application to focus on business logic.

- _Amazon WAF & Route 53_: WAF (Web Application Firewall) is integrated with CloudFront to block common attacks (SQL injection, XSS). Route 53 provides domain and DNS routing.

- _Amazon ElastiCache (Redis)_: An in-memory data store. It reduces load on RDS for repeated queries (e.g., homepage product list). The .NET application will cache hot data in Redis to improve response times. Like RDS, ElastiCache is placed in a Private Subnet for security.

- _NAT Gateway_: Provides secure outgoing Internet access for private resources (such as Elastic Beanstalk), allowing servers to download security patches without being exposed to the Internet.

- _AWS CodePipeline/CodeBuild_: CI/CD services that integrate with GitHub to automate: (1) CodeBuild compiles the .NET code, (2) CodePipeline deploys new versions to Elastic Beanstalk.

_Data Flow_

- [1]-[2] Users access the domain (via Route 53) and are routed to CloudFront. WAF filters the request.
- [3] (Static Flow) If static files (images, CSS), CloudFront serves directly from Amazon S3.
- [4]-[6] (Dynamic Flow) If dynamic requests, CloudFront forwards traffic through the Internet Gateway to the Application Load Balancer, then to Elastic Beanstalk.
- [7]-[8] The .NET application checks ElastiCache first; if not available, it queries Amazon RDS.
- [9]-[10] When Elastic Beanstalk requires Internet access (e.g., downloading patches), it communicates through NAT Gateway to the Internet Gateway.
- [11]-[14] (CI/CD Flow) When developers push code to GitHub, CodePipeline and CodeBuild automatically build and deploy the new version to Elastic Beanstalk.

### 4. Technical Implementation

_Phases of Implementation_  
The project is divided into 4 main phases over 11 weeks to ensure progress and quality:

1. _Building technical foundation_: Focus on setting up the technical foundation, finalizing data models for core entities, establishing the .NET 3-tier solution structure (Domain, Application, Persistence, WebShop), initializing the GitHub repository, and learning AWS services. (Week 1–4)

2. _Developing core features_: Complete the Persistence Layer (Repositories, Unit of Work) and Application Layer (Services) for core tasks such as product, user, and order management. Meanwhile, the WebShop Layer (Controllers, Views) will be built for login, cart, checkout flows, and Unit Tests for Services will begin. (Week 5–7)

3. _Integrating AWS services_: Integrate Amazon S3 for product images and ElastiCache (Redis) for caching. The team will complete the CI/CD pipeline for automatic deployment to the Staging environment on Elastic Beanstalk and conduct Integration Testing. (Week 8–10)

4. _Finalization and deployment_: Configure security services such as CloudFront, WAF, and Route 53. Deploy version 1.0 to Elastic Beanstalk, conduct final UAT, and set up monitoring using CloudWatch. (Week 11)

_Technical Requirements_

- _Backend_: ASP.NET Core MVC 9.0
- _ORM_: Entity Framework Core
- _Database_: MS SQL Server 2022 (Local) and Amazon RDS for SQL Server (Cloud)
- _Frontend_: Bootstrap 5, jQuery, Bootstrap Icons
- _Cloud Platform (AWS)_: Elastic Beanstalk, RDS, S3, CloudFront, WAF, Route 53, ElastiCache, VPC, NAT Gateway, CodePipeline, CodeBuild
- _Source Control_: Git
- _Tools_: Visual Studio 2022, Docker Desktop

_Development Methodology_

Agile (Scrum-like) is applied for flexibility and progress assurance, aligned with the four planned phases. All tasks (features, bugs) are tracked via a Kanban board. All new code must be reviewed via GitHub merge requests before merging into the main branch to maintain code quality.

_Testing Strategy_

Three levels of testing will be executed:

- Unit Testing (100% focused on Application Layer, using mocked repositories)
- Integration Testing (on Staging with real RDS and EF Core)
- User Acceptance Testing (on Production UI for flows like Register, Login, Checkout)

_Deployment Plan_

A fully automated CI/CD pipeline is used. When code is pushed to GitHub, AWS CodePipeline triggers CodeBuild to compile the project, run Unit Tests, and package a .zip file. If successful, CodePipeline deploys the new version to Staging on Elastic Beanstalk.

### 5. Roadmap & Milestones

The project is planned over 11 weeks, divided into 4 phases:

Phase 1 (Week 1–4): Technical foundation (data models, .NET architecture, GitHub, AWS VPC/Subnets).  
Milestone: Architecture & repositories completed.

Phase 2 (Week 5–7): Core features (Products, Orders, Auth, Cart, Checkout, Unit Tests).  
Milestone: All core flows working locally.

Phase 3 (Week 8–10): AWS integration (S3, Redis, CI/CD, Staging deployment).  
Milestone: Pipeline working, Staging deployment stable.

Phase 4 (Week 11): Security configuration, Production deployment, UAT, CloudWatch monitoring.  
Milestone: Version 1.0 live on Production.

### 6. Budget Estimate

View full estimate:  
[AWS Pricing Calculator](https://calculator.aws/#/estimate?id=d9984dcf32da018859c676f29d2d4d255a2933ca)  
Or download the [budget file](/attachments/pricing.pdf).

_Infrastructure Cost_

The AWS Pricing Calculator estimates monthly operational cost at 138.06 USD with 0.00 USD upfront. The cost optimization strategy focuses on maximizing AWS Free Tier and managed services.

Breakdown:

- EC2 t3a.small (via Beanstalk): $19.15/month
- ALB: $18.69/month
- VPC (NAT Gateway): $46.02/month
- RDS SQL Server Express (db.t3.micro): $25.86/month
- ElastiCache (t4g.micro): $17.52/month
- WAF: $7.20/month
- CloudFront, S3, Route 53, CodeBuild: small remaining cost

Most core services fall under Free Tier during the first 12 months, reducing real costs significantly (mainly NAT Gateway + WAF remain).

ROI is extremely high because the solution directly reduces inventory loss and opens new revenue channels.

### 7. Risk Assessment

Three types of risks were identified: Technical, Business, and Operational.

1. Technical: System Overload (Performance Bottleneck)

   - Impact: **High** | Probability: **Medium**
   - Mitigation: Redis caching, Auto Scaling, CloudFront caching
   - Contingency: CloudWatch alarms, immediate RDS vertical scaling

2. Business: Low User Adoption

   - Impact: **High** | Probability: **Medium**
   - Mitigation: Simple UI, early feedback, training materials
   - Contingency: Extra Sprint to adjust features based on feedback

3. Operational: Data Loss / Breach
   - Impact: **Critical** | Probability: **Low**
   - Mitigation: Automated RDS backups, Private Subnet isolation, WAF protection, secured environment variables
   - Contingency: Point-in-Time Recovery (PITR)

### 8. Expected Outcomes

Business metrics:

- 90% reduction in inventory errors
- 50% faster checkout time
- 20% online revenue within 6 months

Technical metrics:

- 99.9% uptime
- <2s page load time (CloudFront + Redis)
- Stable CI/CD deployment frequency

Short-term (0–6 months):

- Immediate UX improvements
- Inventory automation
- Reduced operational errors

Medium-term (6–18 months):

- Market expansion
- Collection of valuable business data

Long-term:

- Full data-driven decision-making
- Easy multi-store scaling via Elastic Beanstalk + RDS
