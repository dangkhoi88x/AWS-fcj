---
title: "Blog 3"
date: 2025-11-13
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

# Maximizing SAP Data Value with Amazon Q Business and Amazon Bedrock Generative AI – Part 1

Organizations have moved beyond the exploration phase of Generative AI (Gen AI) to begin actively using it to create business value. If you're working with SAP systems and wondering where to start or looking for use case ideas, you're not alone. This article explores common business use cases for SAP data and the different approaches available when using AWS services for Gen AI.

For Gen AI, organizations have multiple options to leverage technology approaches for their ERP data:

- AI integrated into software (such as SAP Joule)
- Platform solutions like SAP Gen AI Hub, which provide access to Amazon Bedrock models such as Anthropic Claude and Amazon Nova
- Pre-built solutions for data analytics, often with industry-specific solutions for Human Resources, Legal, etc.
- Building and customizing applications using Gen AI technology with their own business data

In this series, we focus on building custom solutions with the following use cases, related to using SAP data with AWS:

- Analyzing SAP Early Watch Analysis (EWA) reports using natural language
- Automating invoice processing with Intelligent Document Processing (IDP)
- Deriving business insights from structured financial data using natural language

---

## AWS Generative AI Services

In this article, we use the following Generative AI services from AWS:

- **Amazon Q Business**: a fully managed, Gen AI-powered assistant that you can configure to answer questions, provide summaries, generate content, and complete tasks based on your business data.

- **Amazon Bedrock**: a fully managed service that provides high-performing foundation models (FMs) from leading AI companies and Amazon through a unified API. Bedrock's Knowledge Bases feature allows you to provide contextual information from your company's proprietary data sources to foundation models and agents to deliver more relevant, accurate, and customized responses.

---

## Analyzing SAP Early Watch Analysis (EWA) Reports Using Natural Language

EWA reports contain critical details about the health of SAP systems and can be used to analyze trends over time. Because these documents contain detailed information, a report can often be dozens of pages long; for example, a public S/4HANA EWA report sample from SAP has 213 pages. Analyzing reports, combining recommendations, and capturing trends over time is a tedious task that can be automated with Amazon Q Business.

Figure 1 illustrates the architecture diagram of a potential solution, and this section will present the steps to build it.

- Upload your organization's EWA reports (from all SAP systems) to an Amazon S3 bucket
- Create an Amazon Q Business application, Q Index, and add S3 as a data source
- Use the web interface to analyze EWA reports

> _Figure 1: EWA Analysis Application Architecture_

---

### 1. Upload Your Organization's EWA Reports (from All SAP Systems) to an Amazon S3 Bucket

You can create a new Amazon S3 bucket or use an existing empty bucket. Download all EWA reports and store them in the Amazon S3 bucket (note the S3 bucket name, for example: sap-early-watch).

{{% notice info %}}
**Note:** If you want to experiment with this solution using public reports, you can use publicly available sample reports.
{{% /notice %}}

---

### 2. Create Amazon Q Business Application, Q Index, and Add S3 as a Data Source

Follow these steps to create an Amazon Q Business Application:

- Navigate to Amazon Q Business in the AWS Console and click the Get Started button.
- Click Create Application and use the form to create the application as illustrated in Figure 2.
- It's recommended to use AWS IAM Identity Center following AWS best practices, but you can also use AWS IAM Identity Provider if you manage user identities outside AWS.
- Select a user to get started quickly; you can edit this later (optional).
- After completing the form, click the create button, and the web application will be created in a few seconds.

> _Figure 2: Creating an Application in Amazon Q Business_

Follow these steps to add an Amazon Q Index:

- Navigate to the application you just created (ABC-EWA-Analyzer) and go to Data Sources as illustrated in Figure 3.

> _Figure 3: Data Sources for Amazon Q Business_

- Navigate to the "Add an index" option and create a new index (as illustrated in Figure 4).
- Complete the form and add the index (this process can take up to 20 minutes to complete).

> _Figure 4: Adding an Index to the ABC-EWA-Analyzer Application_

Follow these steps to add S3 as a data source:

- After creating the index, click add data source (as illustrated in Figure 5) and select the S3 bucket where EWA reports are stored (sap-early-watch in this case).

> _Figure 5: Adding Amazon S3 as a Data Source for the Application_

- Select the sync type and schedule you want.
- Complete the data source form and click the "add data source" button to complete the task.
- After the data source is added, you can use "sync now" to perform the initial sync, which can take up to 20 minutes depending on the number and length of reports.

---

### 3. Use the Web Interface to Analyze EWA Reports

After synchronization is complete, use the deployed URL (shown in the application) to access the EWA Analyzer application. You can customize the web experience by including a custom title, greeting, and company logo to display in the application.

Figure 6 illustrates what the custom application interface might look like (ensure you've selected company knowledge in the toggle button for knowledge sources).

> _Figure 6: Early Watch Analyzer Application_

Now you can use the application to perform analyses (such as those mentioned below), which not only increases productivity but also helps you dive deeper into investigations.

- Find the longest-running database queries.
- Check if your database and operating system have the latest updates.
- Get specific suggestions to improve performance as mentioned in the report.

Figure 7 shows an example of interacting with the application to ask relevant questions about EWA reports uploaded to the S3 bucket. Also note Amazon Q's transparency aspect, showing the sources used to generate the answer.

> _Figure 7: EWA Analysis Using Amazon Q Business Application_

{{% notice tip %}}
**Tip:** You can also use the Amazon Q Business application to analyze SAP readiness check reports, sizing reports, etc.
{{% /notice %}}

This concludes the EWA analysis use case, where we discussed how you can:

- Reduce time spent manually reviewing multi-page EWA reports.
- Enable quick analysis of system health across multiple SAP systems.
- Track trends and recommendations over time.

---

## Automating Invoice Processing with Intelligent Document Processing (IDP)

Traditional manual processing of paper and digital invoices is not only time-consuming but also error-prone, leading to delayed payments, compliance risks, and inefficient use of valuable human resources. As businesses scale, invoice volumes grow exponentially, making manual processing increasingly unsustainable. This is where you can leverage Generative AI solutions to reduce the burden. You can also use SAP Build Process Automation solutions alongside AWS technologies.

### Processing and Classifying Invoices

If your organization works with paper invoices or PDF/email invoices, you'll understand the trade-off between efficiency and accuracy. Manual processing also doesn't scale. This section presents how to use Amazon Bedrock's Data Automation feature for the following use cases:

- Automate processing of paper/PDF invoices in SAP systems or third-party systems.
- Classify invoices into different groups for further analysis.

{{% notice info %}}
**Note:** If you post invoices directly in SAP, you can also use an alternative approach such as using SAP Build Process Automation (a Business Technology Platform service available on AWS) to automate processing.
{{% /notice %}}

Bedrock Data Automation (BDA) simplifies extracting data from unstructured documents (such as invoices) and media by leveraging Generative AI to automatically convert multimodal data into structured formats. You can use BDA to process and classify invoices as follows:

- Extract, normalize, and validate data from invoices.
- Customize BDA output and integrate with existing workflows for processing.
- Use normalized output to classify invoices (for example, by business unit).

Figure 8 illustrates the architecture diagram of the solution, and this section presents the steps to build it.

- Upload your organization's invoices to an Amazon S3 bucket.
- Create a BDA project and add a blueprint.
- Integrate results into existing workflows for processing.

> _Figure 8: Architecture Using BDA to Process and Classify Invoices_

---

### 1. Upload Your Organization's Invoices to an Amazon S3 Bucket

You can create a new Amazon S3 bucket or use an existing empty bucket. Upload all invoices (PDFs, scanned images, etc.) to the Amazon S3 bucket.

---

### 2. Create a BDA Project and Add a Blueprint

Follow these steps to create a BDA project and blueprint:

- Navigate to Data Automation in Amazon Bedrock on the AWS Console and create a project as illustrated in Figure 9.

> _Figure 9: Creating a New BDA Project_

- After the project is created, use the test option to import data from the S3 bucket where invoices are stored, as illustrated in Figure 10.

> _Figure 10: Testing Invoice Processing from AWS Console_

- Figure 11 shows an example of standard output from BDA, which can be downloaded in JSON format.

> _Figure 11: Example of BDA Standard Output_

- Evaluate the standard output, and if you need more control, use custom output (as illustrated in Figure 12) and create a blueprint using a "blueprint prompt"; you can also select an AWS-provided blueprint from the list.

> _Figure 12: BDA Custom Output and Blueprint_

---

### 3. Integrate Results into Existing Workflows for Processing

After thorough testing, when you're confident that the blueprint extracts data in the way your organization uses, integrate the BDA project into existing workflows to process invoices in SAP, such as an application created using SAP Business Technology Platform (BTP) services.

**AWS SDK for SAP ABAP**: In your workflow, you can use the AWS SDK for SAP ABAP to seamlessly integrate between SAP ABAP and over 200 AWS services in RISE with SAP, GROW with SAP, and self-managed environments.

{{% notice tip %}}
**Tip:** You can also use the output as part of a Retrieval Augmented Generation (RAG) workflow to query extracted information using natural language.
{{% /notice %}}

This concludes the invoice processing with IDP use case, where we discussed how you can:

- Convert unstructured invoice data into structured formats.
- Optimize processing of paper and digital invoices.
- Reduce manual processing errors and improve efficiency.
- Enable invoice classification for further analysis.

---

## Sample Cost Analysis

The following table provides a sample cost analysis to deploy this solution in your AWS account with default parameters in the US East 1 (N. Virginia) region.

| AWS Service | Size | Cost (USD) |
|-------------|------|------------|
| Amazon Q Business Pro | Per user per month | $20 |
| Enterprise Index | One Index unit per month | $192.72 |
| Amazon Bedrock Data Automation (BDA) | Processing 1000 pages with blueprint | $40 |
| Amazon S3 | 1 TB storage for EWA and invoices | $23.55 |

{{% notice tip %}}
**Tip:** We recommend creating a Budget through AWS Cost Explorer to help manage costs. For full details, refer to the pricing page for each AWS service used in this article.
{{% /notice %}}

---

## Conclusion

In this article, we discussed how you can increase workforce efficiency and reduce errors with SAP data by using AWS Generative AI services. In Part 2 of this article, we will discuss in detail deriving business insights from structured financial data using natural language.

Learn more about AWS Generative AI services and get started with your own use cases with Amazon Q.

---

## Join the Discussion on SAP on AWS

Beyond your customer account team and AWS Support channels, AWS provides public Q&A forums on our re:Post site. Our SAP Solutions Architecture team regularly monitors SAP on AWS topics to discuss and answer questions to support you. If your question is not support-related, consider joining the discussion on re:Post and contributing to the community knowledge base.
