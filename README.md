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

 The diagram below summarizes the core components of a cloud-agnostic Landing Zone across all major cloud providers. It visually represents the foundational elements — such as identity, networking, security, and automation — that are essential for building a secure and scalable environment in AWS, Azure, or GCP. <br/>

 <img width="790" height="578" alt="image" src="https://github.com/user-attachments/assets/ac56397d-2be4-4540-abc9-6bdfca46ba38" />


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

# Cloud-Agnostic Landing Zone Checklist
You may leverage the below practical checklist to ensure your landing zone covers all the foundational elements — from identity to automation — regardless of the cloud provider.<br/>
### 🗂️ Cloud Landing Zone Checklist by Day 0 / Day 1 / Day 2

| **Category**              | **Key Activities**                                                                                                                                              |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Identity & Access**     | - Define Org-level IAM roles, groups, policies  <br> - Plan SSO, MFA, and identity federation (Azure AD / Okta / Workspace)  <br> - Set least-privilege model   |
| **Resource Organization** | - Design folder/account/project hierarchy  <br> - Define naming conventions  <br> - Establish tagging standards  <br> - Plan Dev/Test/Prod environments         |
| **Networking**            | - Define IP CIDR ranges  <br> - Plan VPC/VNet and subnet structure  <br> - Plan shared networking (Hub-Spoke, Transit Gateway)  <br> - Plan DNS strategy        |
| **Security & Compliance** | - Define Org-level policies or SCPs  <br> - Choose baseline frameworks (e.g., CIS, NIST)  <br> - Plan encryption strategy  <br> - Plan vulnerability management |
| **Automation & IaC**      | - Set up Terraform/Bicep/CloudFormation baseline modules  <br> - Define Git repo structure  <br> - Plan CI/CD pipelines  <br> - Plan Policy-as-Code             |
| **Cost Management**       | - Define budget controls  <br> - Plan tagging for cost allocation  <br> - Enable billing export or APIs                                                         |
                                        |
                                     |

| **Category**              | **Key Activities**                                                                                                                           |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **Identity & Access**     | - Implement IAM roles/groups  <br> - Integrate SSO and MFA  <br> - Enforce least-privilege policies                                          |
| **Resource Organization** | - Create folder/project/account structure  <br> - Apply naming conventions  <br> - Enforce tagging via policies                              |
| **Networking**            | - Deploy VPC/VNet, subnets, routes, and firewalls  <br> - Set up DNS and hybrid connectivity (VPN/Interconnect)                              |
| **Security & Compliance** | - Apply encryption at rest/in transit  <br> - Deploy baseline security tools (e.g., GuardDuty, SCC, Defender)  <br> - Implement org policies |
| **Monitoring & Logging**  | - Enable centralized logging and audit trails  <br> - Set up log sinks and storage  <br> - Start basic metrics/alerting setup                |
| **Automation & IaC**      | - Deploy environments using IaC  <br> - Integrate IaC into CI/CD  <br> - Validate using Policy-as-Code                                       |
| **Cost Management**       | - Apply budgets and alerts  <br> - Enable detailed billing reports  <br> - Tag deployed resources                                            |



### ✅ Practical Landing Zone Checklist (Cloud-Agnostic)

| **Category**            | **Key Activities**                                                                                                                                     |
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Identity & Access**   | - Configure IAM roles, policies, and groups  <br> - Set up SSO and MFA  <br> - Apply least-privilege access  <br> - Integrate with identity provider |
| **Resource Organization** | - Define folder/account/project structure  <br> - Naming conventions and mandatory tags  <br> - Separate Dev/Test/Prod environments                 |
| **Networking**          | - Define CIDR ranges and subnet layout  <br> - Configure VPC/VNet, routing, firewalls  <br> - Shared networking (Hub-Spoke)  <br> - Enable DNS, hybrid connectivity |
| **Security & Compliance** | - Apply policies (Org policies/SCPs)  <br> - Enable encryption in transit/at rest  <br> - Use cloud-native security tools (Defender, GuardDuty, SCC)  <br> - Run compliance checks (CIS, NIST) |
| **Monitoring & Logging** | - Centralize logs and audit trails  <br> - Metrics, alerting, and dashboards  <br> - Route logs to SIEM or storage for long-term retention            |
| **Automation & IaC**    | - Use Terraform/Bicep/CloudFormation  <br> - Version control (e.g., Git)  <br> - CI/CD for provisioning  <br> - Policy-as-Code (OPA, Sentinel)       |
| **Cost Management**     | - Set budgets and alerts  <br> - Enable detailed billing  <br> - Tag resources for cost attribution  <br> - Use cost optimization recommendations     |


<img width="596" height="521" alt="image" src="https://github.com/user-attachments/assets/295d29fd-7735-4fd5-b4bb-5af0db482d92" />


# Appendix
Below are some additional resources and references for further learning: <br/>
1. [What is a Cloud Landing Zone?](https://www.appvia.io/blog/what-is-a-cloud-landing-zone)<br/>
2. [Checklist : Building Azure Right: A Practical Checklist for Infrastructure Landing Zones](https://techcommunity.microsoft.com/blog/azureinfrastructureblog/building-azure-right-a-practical-checklist-for-infrastructure-landing-zones/4409975)<br/>
3. [Enhancing Terraform Deployments with TFLint in GitHub Actions](https://dev.to/techielass/integrating-tflint-into-your-workflow-iea)<br/>
4. [TFLint rules : terraform_unused_declarations](https://github.com/terraform-linters/tflint-ruleset-terraform/blob/main/docs/rules/terraform_unused_declarations.md)<br/>
5. [TFLint AWS Complete ruleset](https://github.com/terraform-linters/tflint-ruleset-aws/blob/master/docs/rules/README.md)<br/>
6. [TFLint with Jenkins](https://awstip.com/tflint-with-jenkins-858957b87c7e)<br/>
