# CloudLandingZone
A `Cloud Landing Zone` is a pre-configured Cloud Environment within a cloud provider (like AWS, Azure, or GCP)  that that acts as a foundation for deploying applications and workloads.  It establishes `guardrails, governance, automation and infrastructure` allowing teams to deploy workloads securely & efficiently from Day-1. Think of it like setting up a robust base camp before starting to build houses in a new territory. It's not just a single server or a VPC; it’s a collection of pre-configured services, policies, controls, and automated provisioning that ensures everything built on top is secure, compliant, cost-effective, and scalable. <br/>

🔍 Another analogy : it’s like setting up a new office building before the teams move in. You wouldn’t just put up a sign and open the doors — you’d set up : <br/>
- 🛡️ Security (Access Control): Locks, access cards, cameras.
- 🌐 Utilities (Networking): Electricity, plumbing, internet.
- 🗂️ Departments (Resource Hierarchy): Dedicated spaces for teams.
- 📜 Rules & Regulations (Policies): Fire exits, safety codes.
- 📈 Monitoring (Logging): Utility meters tracking usage.
- 🤖 Automation (IaC - Provisioning & Management): Pre-wired systems for consistency. <br/><br/>
A `Cloud Landing Zone` provides the digital equivalent of this foundational setup, enabling efficient, secure, and automated cloud adoption at scale. Automation ensures that every environment — whether for dev, staging, or production — is built and governed in a repeatable, scalable, and secure way.
 <br/>

# Cloud-Agnostic Core Elements of a Landing Zone
These foundational elements apply across all major cloud providers — AWS, Azure, and GCP — ensuring secure, scalable, and consistent cloud adoption: <br/>

| **Core Element**          | **Description**                                                                                               |
| ------------------------- | ------------------------------------------------------------------------------------------------------------- |
| **Identity & Access**     | Defines **who can access what** — includes **IAM roles, groups, SSO integration, and MFA enforcement**            |
| **Resource Organization** | Logical structure of the cloud environment — hierarchy, folders, projects/accounts, naming conventions, and mandatory tagging |
| **Networking**            | Set up of **VPCs, subnets, firewalls, routing**, and **DNS zones** for connectivity and segmentation          |
| **Security & Compliance** | Guardrails like **encryption, policies, vulnerability scanning**, and audit readiness (CIS, ISO, NIST)        |
| **Monitoring & Logging**  | Centralized **logging, metrics, alerts**, and **audit trails** to support visibility and incident response    |
| **Automation & IaC**      | Use of tools like **Terraform, CloudFormation, Bicep** to automate deployments and enforce consistency        |
| **Cost Management**       | Implement **budgets, billing alerts**, **cost dashboards**, and **chargeback models** to control spend            |

# Cloud-Specific Mapping of Landing Zone Core Elements (AWS vs Azure vs GCP)
Below table summarizes the elements in each CSP's : <br/>

| **Core Element**          | **AWS**                                                               | **Azure**                                                            | **GCP**                                                               |
| ------------------------- | --------------------------------------------------------------------- | -------------------------------------------------------------------- | --------------------------------------------------------------------- |
| **Identity & Access**     | IAM users, roles, policies, SSO via IAM Identity Center (ex SSO), MFA | Azure AD (Entra ID), RBAC, Conditional Access, MFA                   | Cloud IAM, Google Workspace federation, SSO, MFA                      |
| **Resource Organization** | Organizations → OUs → Accounts                                        | Management Groups → Subscriptions → Resource Groups                  | Organization → Folders → Projects                                     |
| **Networking**            | VPCs, Subnets, Route Tables, NACLs, Security Groups, Transit Gateway  | VNets, Subnets, NSGs, Route Tables, ExpressRoute, Azure Firewall     | VPCs, Subnets, Firewall Rules, Routes, Shared VPC, Cloud NAT          |
| **Security & Compliance** | AWS Config, SCPs, GuardDuty, Macie, KMS, IAM Access Analyzer          | Defender for Cloud, Policies, Blueprints, Key Vault, Security Center | Security Command Center, Org Policies, Cloud KMS, Forseti (community) |
| **Monitoring & Logging**  | CloudWatch, CloudTrail, X-Ray, AWS Config                             | Azure Monitor, Log Analytics, Activity Logs                          | Cloud Logging, Cloud Monitoring, Audit Logs, Error Reporting          |
| **Automation & IaC**      | CloudFormation, Terraform, CDK, Control Tower                         | Bicep, ARM Templates, Terraform, Landing Zone Accelerator            | Terraform, Deployment Manager (legacy), GitOps (Config Connector)     |
| **Cost Management**       | AWS Budgets, Cost Explorer, Cost Categories                           | Cost Management + Billing, Budgets, Advisor                          | Cloud Billing Reports, Budgets, Recommender, FinOps tools             |


# Appendix
Below are some additional resources and references for further learning: <br/>
1. [What is a Cloud Landing Zone?](https://www.appvia.io/blog/what-is-a-cloud-landing-zone)<br/>
2. [TFLint, covering installation, configuration, usage](https://www.devopsschool.com/blog/terraform-tutorials-tflint-covering-installation-configuration-usage/)<br/>
3. [Enhancing Terraform Deployments with TFLint in GitHub Actions](https://dev.to/techielass/integrating-tflint-into-your-workflow-iea)<br/>
4. [TFLint rules : terraform_unused_declarations](https://github.com/terraform-linters/tflint-ruleset-terraform/blob/main/docs/rules/terraform_unused_declarations.md)<br/>
5. [TFLint AWS Complete ruleset](https://github.com/terraform-linters/tflint-ruleset-aws/blob/master/docs/rules/README.md)<br/>
6. [TFLint with Jenkins](https://awstip.com/tflint-with-jenkins-858957b87c7e)<br/>
