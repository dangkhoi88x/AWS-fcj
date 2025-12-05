---
title: "Week 10 Worklog"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Goals

- **Deploy the Mini-Market system** on AWS environment.
- **Complete code and integrate** all designed AWS services.
- **Perform testing and optimization** of system performance.
- **Prepare demo** and video presentation of main system features.

**Time Period:** December 1, 2025 - December 7, 2025

---

### Weekly Tasks Overview

| Day | Activity                                                                                                                                                                                                                                                                              | Start Date | End Date   | Reference                                  |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | ---------- | ------------------------------------------ |
| 1   | - **Deploy Infrastructure on AWS** <br> + Create VPC with public and private subnets <br> + Configure Security Groups and Network ACLs <br> + Set up NAT Gateway and Internet Gateway <br> + Create RDS SQL Server instance in private subnet                                         | 12/01/2025 | 12/01/2025 | <https://aws.amazon.com/vpc/>              |
| 2   | - **Deploy Application Layer** <br> + Create Elastic Beanstalk environment for .NET application <br> + Configure Auto Scaling and Load Balancer <br> + Connect application to RDS database <br> + Set up environment variables and connection strings                                 | 12/02/2025 | 12/02/2025 | <https://aws.amazon.com/elasticbeanstalk/> |
| 3   | - **Integrate Storage and CDN** <br> + Create S3 bucket for product image storage <br> + Configure CloudFront distribution for content delivery <br> + Set up ElastiCache Redis cluster for caching <br> + Connect application to S3 and Redis                                        | 12/03/2025 | 12/03/2025 | <https://aws.amazon.com/s3/>               |
| 4   | - **Configure Security and Monitoring** <br> + Set up IAM Roles and Policies for services <br> + Configure AWS WAF to protect application <br> + Set up CloudWatch alarms and dashboards <br> + Enable CloudTrail for activity logging                                                | 12/04/2025 | 12/04/2025 | <https://aws.amazon.com/iam/>              |
| 5   | - **Testing and Optimization** <br> + Perform load testing to check performance <br> + Test High Availability <br> + Optimize database queries and caching strategy <br> + Adjust Auto Scaling policies based on metrics                                                              | 12/05/2025 | 12/05/2025 | <https://aws.amazon.com/cloudwatch/>       |
| 6   | - **Prepare Demo and Documentation** <br> + Record video demo of main features: Login, View Products, Add to Cart, Checkout <br> + Create user guide and technical documentation <br> + Prepare demo script for presentation <br> + Create screenshots and diagrams for documentation | 12/06/2025 | 12/06/2025 | -                                          |
| 7   | - **Week 10 Summary** <br> + Evaluate deployment and testing results <br> + Document issues encountered and solutions <br> + Update Proposal with actual results <br> + Prepare for Week 11: Final review and presentation                                                            | 12/07/2025 | 12/07/2025 | -                                          |

---

### Week 10 Achievements

- **Successfully deployed Mini-Market system on AWS**:

  - Created and configured complete VPC with public/private subnets, NAT Gateway, and Internet Gateway.
  - Deployed .NET application on Elastic Beanstalk with Auto Scaling and Load Balancer.
  - Set up RDS SQL Server in private subnet with Multi-AZ for High Availability.
  - Integrated S3 for storage and CloudFront for CDN.

- **Optimized performance and security**:

  - Configured ElastiCache Redis to cache data and reduce database load.
  - Set up AWS WAF to protect application from common attacks.
  - Configured IAM Roles and Policies following least privilege principle.
  - Set up CloudWatch monitoring and CloudTrail logging.

- **Completed testing and validation**:

  - Performed load testing and confirmed system can handle high traffic.
  - Tested High Availability by simulating failover scenarios.
  - Optimized database queries and caching strategy based on testing results.
  - Adjusted Auto Scaling policies to ensure cost efficiency.

- **Prepared professional demo and documentation**:

  - Recorded comprehensive video demo of main system features.
  - Created detailed user guide and technical documentation.
  - Prepared demo script and slides for presentation.
  - Created screenshots and diagrams for documentation.

- **Gained practical experience**:

  - Understood the process of deploying systems on AWS from design to production.
  - Mastered troubleshooting common issues during deployment.
  - Learned best practices for security, monitoring, and cost optimization.
  - Developed skills working with multiple AWS services simultaneously.

- **Ready for final phase**:

  - System has been successfully deployed and tested.
  - Documentation and demo materials are fully prepared.
  - Ready for Week 11: Final review and Week 12: Final presentation.
