---
title: "Blog 2"
date: 2025-11-13
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

# Managing Profiles and Customizations of Amazon Q Developer in Large Organizations

As organizations scale their software development efforts, using AI coding assistants that understand internal models and standards helps make the development process more efficient while improving the quality of deployed software. Amazon Q Developer Pro addresses this challenge by enabling organizations to customize the AI assistant based on their proprietary source code and development processes. Through Amazon Q Developer Profiles, teams can efficiently manage access and distribute Amazon Q customizations across multiple AWS regions and different AWS Identity Centers.

In this post, we will explore different approaches to deploying and managing Amazon Q Developer profiles and Amazon Q customizations across large organizations.

Using an example with multiple business units, we will explore methods to manage access controls and customization governance while meeting security and compliance requirements.

Amazon Q customization is now available in both US East (N. Virginia) and EU Central (Frankfurt) regions, giving teams more flexibility in creating and deploying customizations closer to their operational hubs while meeting regional data residency requirements.

This blog post is not intended to provide recommendations on how to structure your AWS accounts or divide Q Developer subscriptions.

Instead, our goal is to explore the full capabilities of Q Developer Customizations in a comprehensive scenario that demonstrates the current art of the possible.

---

## Amazon Q Developer Pro Subscription Distribution Scenario

The following diagram illustrates a sample AWS Organizations structure with a Management Account and four Organizational Units (OUs). This is a common enterprise scenario with three business units, each requiring separate Amazon Q Developer Pro subscriptions and customizations.

> _Figure 1: AWS Organizations Structure and Resource Hierarchy_

The Infrastructure OU has a Delegated Admin Account with delegated access to AWS IAM Identity Center. There are three additional OUs: Alpha, Bravo, and Charlie, each with at least one Amazon Q Developer Pro subscription. The Alpha account has Amazon Q Developer subscriptions in both US East (N. Virginia) and EU Central (Frankfurt) regions.

Think of each business unit as a separate ecosystem within your organization. When you provision separate Amazon Q Developer Pro subscriptions for different OUs, you are essentially providing each unit with its own personalized AI assistant. This separation is valuable because it allows each team to work independently while maintaining their specific requirements and workflows.

OU Charlie maintains a separate account instance of IAM Identity Center for Amazon Q Developer Pro. In most cases, we recommend using an organization instance of IAM Identity Center with Amazon Q Developer Pro, but there are a few scenarios where member account instances may be appropriate, such as: when you don't have a single identity provider, or when you haven't decided to deploy organization-wide and want to use Amazon Q only for AWS accounts you control.

{{% notice info %}}
**Note:** When a developer has a user in an Amazon Q profile linked to two different IAM Identity Center instances (Bravo and Charlie), they will have two user subscriptions and be charged twice. However, if they belong to two different Amazon Q profiles in two different accounts (Alpha and Bravo) but under the same IAM Identity Center, they will only be charged once.
{{% /notice %}}

In our example, OU Charlie requires additional operational costs in managing separate credential information and authentication processes. Additionally, the dashboard and administrative settings will only be linked to users and groups in this account.

From an administrative perspective, rather than trying to manage a single centralized configuration to meet everyone's needs, you can distribute administrative rights to each business unit and delegate responsibility to individual groups.

This is similar to having different specialized departments in a hospital – although they all belong to the same organization and can collaborate when necessary, each department has its own specialized tools and protocols that help them perform their specific functions more efficiently.

---

## Strategic Approach to Customization Through Q Developer Profiles

> _Figure 2: Developer Connections to Amazon Q Developer Pro Subscriptions, Customizations, and IAM Identity Center_

Amazon Q Developer profiles are how developers connect to different Amazon Q Developer subscriptions through their IDEs. Each profile represents a unique combination of an Amazon Q Developer subscription and related customizations. After authentication, developers can easily select or switch between profiles in their IDE to access different customizations.

Let's examine some scenarios in this architecture.

---

### Scenario 1 – Users Accessing Two Different Customizations Linked to a Single IAM Identity Center Instance in the Management Account

Developers from the Orange Group who have access to Alpha account customizations can configure two different Amazon Q Developer profiles in their IDE:

- A "US Profile" connecting to the US East subscription in the Alpha account.
- An "EU Profile" connecting to the EU Central subscription in the Alpha account.

Switching between different sets of customizations is simply a matter of selecting the appropriate profile in their IDE.

> _Figure 3: IDE interface showing Amazon Q Developer customization panel. Developers switch between US and EU Profiles along with their customizations_

{{% notice info %}}
**Note:** Although developers can access multiple customizations through different Amazon Q Developer profiles, they only incur a single user subscription cost because they are using the organization instance of IAM Identity Center. This is because the subscription is tied to the user identity in the organization IAM Identity Center instance, independent of the number of profiles or customizations they access.
{{% /notice %}}

---

### Scenario 2 – Users Accessing Two Different Customizations Linked to a Single IAM Identity Center Instance in the Management Account

Similarly, developers from the Blue Group can also configure multiple profiles:

- One profile to access Alpha and Bravo customizations through the management account's AWS IAM Identity Center instance.
- A separate profile to access Charlie customizations through the member account AWS IAM Identity Center instance.

When developers have access to multiple customizations within the same IAM Identity Center configuration and region, they can switch between profiles in the IDE without re-authentication.

> _Figure 4: IDE interface showing Amazon Q Developer customization panel. When authenticated through AWS IAM Identity Center Organization, Blue developers can see both Alpha and Bravo customizations._

However, as demonstrated by the Blue developers' case, switching between profiles using different IAM Identity Center configurations (Organization vs. Account Instance) still requires re-authentication.

> _Figure 5: IDE interface showing Amazon Q Developer customization panel. When authenticated through AWS IAM Identity Center Account Instance, Blue developers can only see Charlie customizations._

{{% notice warning %}}
**Note:** In this scenario, developers will incur two separate user subscription costs because they are accessing customizations through two different IAM Identity Center configurations (organization and account instance). As mentioned above, this scenario is not recommended unless it fits specific situations and is presented here only to illustrate how the authentication and profile switching mechanism works between different IAM Identity Center configurations.
{{% /notice %}}

A scenario for creating code-specific customizations for each profile is that developers in the Alpha Group may need Q to understand specific libraries and internal coding conventions for Java, while developers in the Bravo Group may need Q to be proficient in proprietary technologies and development standards with Python. With separate profiles and customizations, each group will have their own "specialized" version of Q that understands their context.

For Blue developers who have access to Alpha, Bravo, and Charlie customizations, they need to set up separate profiles because these customizations belong to different IAM Identity Center configurations and AWS regions. Switching between these profiles requires re-authentication due to the different IAM Identity Center configurations involved.

---

## Access Summary

| Development Group | AWS IAM Identity Center | Customizations                                                                           |
| ----------------- | ----------------------- | --------------------------------------------------------------------------------------- |
| Orange            | Organization Instance   | Alpha Customization in US East (N. Virginia)<br>Alpha Customization in EU Central (Frankfurt) |
| Blue              | Organization Instance   | Alpha Customization in US East (N. Virginia)<br>Bravo Customization                     |
| Blue              | Account Instance        | Charlie Customization                                                                   |
| Grey              | Organization Instance   | Bravo Customization                                                                     |

You can manage access to specific Amazon Q Developer Pro customizations by adding selected users and groups who already have access to Amazon Q Developer Pro subscriptions within the same Identity Center. This granular access control allows you to create targeted customizations that can only be accessed by specific members or groups in your organization.

---

## Conclusion

In this article, we have explored comprehensive strategies for deploying Amazon Q Developer customizations in large organizations. We have demonstrated how Amazon Q Developer profiles provide a flexible way to manage access to different customizations across AWS regions and IAM Identity Center configurations. By integrating proprietary code repositories, establishing customization governance, and implementing continuous feedback loops, enterprises can maximize the value of AI-assisted development assistants while maintaining code quality and development standards.

The path forward depends on where you are in your Amazon Q Developer customization journey. If you're just getting started, begin with a clear assessment of your codebase and plan your customization approach before deployment. For existing users, review your current customizations and profile configurations to identify optimization opportunities.

In both cases, implement the customization governance we've discussed, adapting them to your specific development models and team structures. Remember that customizations evolve with your codebase – regular refinement helps ensure your AI assistant remains effective as your applications grow and development practices mature. Whether you're just starting with Amazon Q Developer customizations or optimizing existing deployments, these practices can help develop an AI assistant that truly understands and fits your organization's unique development environment.
