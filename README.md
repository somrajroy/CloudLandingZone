# Enterprise Guide to Cloud (agnostic) Landing Zones

A `Cloud Landing Zone` is a pre-configured, secure, and scalable foundation for deploying workloads in AWS, Azure, or GCP. Like a base camp or fully equipped office, it provides guardrails for governance, identity, security, networking, and automation, ensuring compliant and efficient cloud adoption from Day-1. <br/>

`Simple Analogy` : A Cloud Landing Zone is the digital equivalent of building a fully equipped office before inviting employees to work — you wouldn't onboard staff without security, departments, operational rules and utilities in place. Similarly, a landing zone acts as the foundation for deploying workloads securely and at scale. It ensures that governance, identity, security, networking, automation, and compliance guardrails are established before any workload is onboarded.<br/>

# Core Elements of a Landing Zone
These foundational elements apply across all major cloud providers — AWS, Azure, and GCP — ensuring secure, scalable, and consistent cloud adoption: <br/>

| **Core Element**          | **Description**                                                                                               |
| ------------------------- | ------------------------------------------------------------------------------------------------------------- |
| **Identity & Access**     | Defines **who can access what** — includes **IAM roles, groups, SSO integration, and MFA enforcement**            |
| **Resource Organization** | Logical structure of the cloud environment — hierarchy, folders, projects/accounts, naming conventions, and mandatory tagging |
| **Networking**            | Set up of **VPCs, subnets, firewalls, routing**, and **DNS zones** for connectivity and segmentation          |
| **Security & Compliance** | Guardrails like **encryption, policies, vulnerability scanning**, and audit readiness (CIS, ISO, NIST)        |
| **Monitoring & Logging**  | Centralized **logging, metrics, alerts**, and **audit trails** to support visibility and incident response    |
| **Automation & IaC**      | Use of tools like **Terraform, CloudFormation, Bicep** to automate deployments and enforce consistency        |
| **Cost Management**       | Implement **budgets, billing alerts**, **cost dashboards**, and **chargeback models** to control spend        |

<br/>Fundamental components forming a secure, scalable, and governed Cloud Landing Zone applicable across AWS, Azure, and GCP for Governance and Scale.<br/><br/>
<img width="967" height="523" alt="image" src="https://github.com/user-attachments/assets/80af6b1e-2134-4040-bdc7-34ed35945608" />
 <br/>
# Recommended Networking Topology: Hub-and-Spoke Model
In a `cloud landing zone`, the `networking topology` plays a crucial role in ensuring secure, scalable connectivity while minimizing complexity. The `hub-and-spoke model` is the `de-facto standard` for enterprise deployments across AWS, Azure, and GCP, adopted in most implementations (~75%) according to industry patterns and best practices from cloud providers'`Well-Architected frameworks`. This approach centralizes shared networking resources in a `hub` while isolating workloads in `spokes`, providing a balanced foundation for multi-environment (e.g., dev, test, prod) and multi-cloud setups.<br/>
`Definition`: The hub acts as a central transit point for shared services like firewalls, VPN gateways, DNS resolution, and internet egress. Spokes are dedicated networks (e.g., VPCs in AWS, VNets in Azure, or VPCs in GCP) for specific environments or workloads, connected to the hub via peering or routing. This creates a star-like structure where spokes communicate through the hub, avoiding direct interconnections.<br/>
`Cross-Cloud Implementation`: Leverage provider-specific services that align with this model — AWS Transit Gateway for hub routing, [Azure hub VNets or Virtual WAN  for centralized peering](https://learn.microsoft.com/en-us/azure/developer/terraform/hub-spoke-introduction#2-understand-hub-and-spoke-topology-architecture), and GCP Shared VPC or Cloud Router for organization-wide networking. This ensures consistency in your landing zone, as outlined in our cross-cloud mapping table.<br/>
`When to Use Alternatives`: For small-scale or simple setups, a fully meshed topology (direct spoke-to-spoke connections) might suffice, but it increases complexity and management burden at scale. Flat networks are generally avoided in landing zones due to poor isolation. Evaluate based on your organization's size and compliance needs during Day 0 planning.<br/>
#### Simple guide for implementations
* Treat hub‑and‑spoke as the default recommended pattern for most enterprise landing zones because it balances governance, security, and manageability. Use it unless client(s) have specific reasons not to.<br/>
* Choose topology based on requirements: scale, number of sites, appliance needs, latency, and operational model (managed Virtual WAN vs. customer‑managed hub). <br/>
* Risk : Over‑standardizing on hub‑and‑spoke without validating traffic patterns can create cost or performance issues. Action is to run a short discovery (expected traffic flows, number of VNets, security appliances) and map to topology tradeoffs. [This link provides good advice.](https://azuresecurityarchitect.com/azure-landing-zone/building-an-azure-landing-zone-in-an-existing-hub-and-spoke-architecture)<br/>
* Performance Bottlenecks: Centralized firewalls typically have throughput limits which needs to be factored in design. <br/>
* Single Point of Failure: If the Hub’s routing table or firewall fails, the entire cloud network goes dark. <br/>
* Identify Heavy East-West Flows: Are there specific apps (e.g., Big Data, Backup) that move massive volumes between VNets? If yes, consider direct peering for these specific pairs. <br/>
* Latency Sensitivity: Does the app require sub-millisecond response times? Forcing it through a Hub NVA might add unacceptable lag. <br/>
* Cost-Benefit Analysis: Compare the cost of centralized security licenses vs. the data processing "tax" of the Hub. <br/><br/>
**Takeaway:** Most enterprise cloud landing zones start with a hub-and-spoke topology because it centralizes security, governance, and connectivity while scaling cleanly across teams and environments. Virtual WAN is essentially a managed evolution of hub-spoke, not a direct competitor & [can be cheaper for spoke peerings](https://blog.cloudtrooper.net/2026/01/16/which-azure-network-is-cheaper/) but varies by traffic volume. <br/>
* AWS/GCP Equivalents : AWS Transit Gateway for hub-spoke, GCP Shared VPC.
### Quick Comparison (Decision-Centric)

| Topology | When Chosen | Strengths | Trade-offs |
|---------|-------------|-----------|------------|
| **Hub-and-Spoke** | Centralized shared services; multi-team or enterprise environments | Centralized security, governance, and hybrid connectivity; scalable and well-governed | Hub can become a bottleneck if not designed for scale; requires careful routing, CIDR and capacity planning |
| **Mesh / Peering** | Small number of VNets/VPCs with heavy east-west traffic | Low latency; simple for a small number of networks | Poor scalability; complex peering matrix; governance becomes difficult. No central inspection point. |
| **Virtual WAN / Transit Fabric** | Global connectivity across regions or many sites | Managed transit at scale; automated "Any-to-Any" connectivity. | Different operational model; higher cost; less granular control |


Below is a simple art diagram illustrating the `hub-and-spoke topology`: <br/><br/>
<img width="1322" height="727" alt="image" src="https://github.com/user-attachments/assets/c9fd5b46-16a8-4cdd-b947-29c6463f5ba2" />


# Cross-Cloud Service Mapping (AWS vs Azure vs GCP)
Below table summarizes the elements in each CSP's. This mapping helps teams translate universal Landing Zone principles into provider-specific implementations. : <br/>

| **Core Element**          | **AWS**                                                               | **Azure**                                                            | **GCP**                                                               |
| ------------------------- | --------------------------------------------------------------------- | -------------------------------------------------------------------- | --------------------------------------------------------------------- |
| **Identity & Access**     | IAM users, roles, policies, SSO via IAM Identity Center (ex SSO), MFA | Azure AD (Entra ID), RBAC, Conditional Access, MFA                   | Cloud IAM, Google Workspace federation, SSO, MFA                      |
| **Resource Organization** | Organizations → OUs → Accounts                                        | Management Groups → Subscriptions → Resource Groups                  | Organization → Folders → Projects                                     |
| **Networking**            | VPCs, Subnets, Route Tables, NACLs, Security Groups, Transit Gateway  | VNets, Subnets, NSGs, Route Tables, ExpressRoute, Azure Firewall     | VPCs, Subnets, Firewall Rules, Routes, Shared VPC, Cloud NAT          |
| **Security & Compliance** | AWS Config, SCPs, GuardDuty, Macie, KMS, IAM Access Analyzer          | Defender for Cloud, Policies, Blueprints, Key Vault, Security Center | Security Command Center, Org Policies, Cloud KMS, Forseti (community) |
| **Monitoring & Logging**  | CloudWatch, CloudTrail, X-Ray, AWS Config                             | Azure Monitor, Log Analytics, Activity Logs                          | Cloud Logging, Cloud Monitoring, Audit Logs, Error Reporting          |
| **Automation & IaC**      | CloudFormation, Terraform, CDK, Control Tower                         | Bicep, ARM Templates, Terraform, Landing Zone Accelerator            | Terraform, Deployment Manager (legacy), GitOps (Config Connector)     |
| **Cost Management**       | AWS Budgets, Cost Explorer, Cost Categories                           | Cost Management + Billing, Budgets, Advisor                          | Cloud Billing Reports, Budgets, Recommender, FinOps tools             | <br/><br/>

# Tagging standards and strategy

Tagging is a cornerstone of a cloud landing zone, enabling resource organization, cost management, governance, and automation across AWS, Azure, and GCP. A cloud-agnostic strategy uses `lowercase` `snake_case` `keys (e.g., cost_center_id, owner_email)` and `values (e.g., env=prod)`, limited to 63 characters, to meet GCP’s strict label constraints (lowercase, letters, numbers, underscores only, starting with a letter), while ensuring compatibility with AWS (128-char key limit, case-sensitive) and Azure (512-char key limit, case-insensitive keys).  
### Universal Tagging Strategy Summary
 * Tagging rules differ across CSPs - This inconsistency makes cross-cloud automation fail unless a customer enforce common standards.
 * `Cross-cloud automation needs one rule` : Tools like Terraform, CI/CD pipelines, and governance engines require predictable, uniform tags to avoid failures or misclassification. 
 * Follow the universal (`all lowercase' 'snake_case`) tagging standard to ensure compatibility across CSP's without any transformation. 
    *  Format: `all lowercase` + `snake_case` (e.g., `app_name=energy_ai`).
    *  `Characters`: Only `letters`, `numbers`, and `underscores (_)`. Avoid spaces, hyphens, and special characters (except for IDs/emails where case sensitivity is required).
    *  `Length` : Keys limited to 63 characters (to be GCP-safe).
    *  `Enforcement (Tag Enforcement Tools)`: Use cloud-native policy tools (AWS Tag Policies, Azure Policy, GCP Organization Policies). 
    * This makes tagging portable and automation-friendly across AWS, Azure, and GCP & works natively everywhere without modification.
 * `Snake Case` is an industry standard naming convention where :
    * All letters are lowercase.<br/>
    * Words are separated by underscores `(_)`
    * Example : `cost_center_id`.
    * Explicitly avoid special characters beyond underscores (e.g., no spaces, colons, or periods).
 * `Lowest common denominator = universal and reliability`: Since GCP is the strictest, if customers design their enterprise tag policy as `all lowercase & snake_case`, those tags will work without modification in AWS and Azure too.
   * Example: `cost_center=finance_ops` works everywhere.
   * But `CostCenter=Finance Ops` (common in AWS/Azure) will break in GCP.
   * In `multi-cloud designs`, follow the `lowest common denominator principle` — if it works in the strictest cloud, it will work everywhere reliably. <br/>

<img width="804" height="550" alt="image" src="https://github.com/user-attachments/assets/7a241ad7-1443-4f66-9661-335f41ccc43d" /> <br/>



## Tag Dictionary for Landing Zones (Universal Tag Dictionary & Governance)
Below are enterprise-grade sample tags (dictionary) that you can leverage. Use this dictionary as a baseline for enterprise-grade tagging across all CSPs. If this dictionary as taken as baseline in a Landing Zone, then automatically it will be future-proof. It can serve as a reusable asset for consistent governance and cost attribution across AWS, Azure, and GCP. <br/>
#### Mandatory Tags (Cross-Cloud Baseline)
These are essential FinOps and Governance tags that should be applied to all resources for financial accountability, security, and automation.  <br/>


| **Tag Key**           | **Purpose**                  | **Why Mandatory (All CSPs)**                                                                                  |
| --------------------- | ---------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `env`                 | Environment / Lifecycle      | Critical for FinOps, security, and policy enforcement. Values: `dev`, `stage`, `prod`, `dr`. Required for core governance.                  |
| `cost_center`         | Billing / FinOps             | Enables chargeback, budget alerts, FinOps reporting. Works in automation and aligns with enterprise finance models. Crucial for financial accountability & is the primary FinOps identifier for a business unit's spend.|
| `owner_email`         | Identity / Ownership         | Fundamental for Accountability + automation (approvals, alerts, decommission notices). Enhances governance.                                       |
| `contact_email`       | Identity / Ownership         | Ops contact for support/escalation. Ensures clear operational responsibility.                                 |
| `app_name`            | Billing / FinOps             | Identifies application or service. Helps cost attribution, monitoring & resource grouping.                     |
| `project_name`        | Billing / FinOps / Ownership | Logical grouping/team mapping. Can merge with `app_name` if identical.                                             |
| `data_classification` | Security & Compliance        | Indicates data sensitivity (e.g., `public`, `confidential`, `pii`). Drives security policies & compliance posture.       |


#### Discretionary Tags (Recommended but Context-Dependent)
These tags are not mandatory, but they improve cost allocation, billing insights, or environment-level governance: Leverage these tags based on project complexity, compliance needs, automation or multi-cloud requirements. There can be other tags too (e.g. `automation_flag`, `dr_tier` etc.) <br/>

| **Tag Key**       | **Purpose**                   | **Why Useful**                                                                                 |
| ----------------- | ----------------------------- | ---------------------------------------------------------------------------------------------- |
| `owner_bu`        | Identity / Ownership          | Links resources to business units for segmentation + reporting.                                |
| `requester_email` | Identity / Ownership          | Tracks provisioning requests  (audit trails, approvals).                                   |
| `billing_code`    | Billing / FinOps              | If finance uses codes separate from cost centers.                                              |
| `lifecycle`       | Lifecycle / Automation        | Indicates resource status (e.g., ephemeral, persistent). Aids cleanup automation.              |
| `expiry_date`     | Environment / Lifecycle       | Forces cleanup of temporary resources. ISO 8601 format for automation (`YYYY-MM-DD`). Mandatory for ephemeral resources. Can act as a self-destruct timer          |
| `compliance`      | Security & Compliance         | Flags regulated workloads (e.g., `pci_dss`, `iso_27001`). Supports audit and policy automation.                                |
| `criticality`     | Security & Compliance         | Business tier/resource importance (`tier_1`, `tier_2`, `tier_3`). Can drive backup, DR, monitoring automation or de-provisioning policies.     |
| `region`          | Environment / Lifecycle       | For multi-region governance, policy enforcement.                                               |
| `managed_by`      | Operations / Automation       | Identifies provisioning method (`terraform`, `manual`, `ansible`). Helps IaC drift management/scope. |
| `support_tier`    | Operations / SLA              | SLA or support level mapping (e.g., gold, silver). Can be useful for support prioritization or tailored monitoring.                                                       |

# Landing Zone Checklist (Cloud-Agnostic)

This (Cloud agnostic) checklist helps ensure all the essential aspects during landing zone setup are covered. Refer this as a baseline validation guide before deployment. <br/>


| **Category**            | **Key Activities**                                                                                                                                     |
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Identity & Access**   | - Configure IAM roles, policies, and groups  <br> - Set up SSO and MFA  <br> - Apply least-privilege access  <br> - Integrate with identity provider |
| **Resource Organization** | - Define folder/account/project structure  <br> - Naming conventions and mandatory tags  <br> - Separate Dev/Test/Prod environments                 |
| **Networking**          | - Define CIDR ranges and subnet layout  <br> - Configure VPC/VNet, routing, firewalls  <br> - Shared networking (Hub-Spoke)  <br> - Enable DNS, hybrid connectivity |
| **Security & Compliance** | - Apply policies (Org policies/SCPs)  <br> - Enable encryption in transit/at rest  <br> - Use cloud-native security tools (Defender, GuardDuty, SCC)  <br> - Run compliance checks (CIS, NIST) |
| **Monitoring & Logging** | - Centralize logs and audit trails  <br> - Metrics, alerting, and dashboards  <br> - Route logs to SIEM or storage for long-term retention            |
| **Automation & IaC**    | - Use Terraform/Bicep/CloudFormation  <br> - Version control (e.g., Git)  <br> - CI/CD for provisioning  <br> - Policy-as-Code (OPA, Sentinel)       |
| **Cost Management**     | - Set budgets and alerts  <br> - Enable detailed billing  <br> - Tag resources for cost attribution  <br> - Use cost optimization recommendations     |

# Landing Zone Lifecycle: Plan -> Deploy -> Operate -> Evolve
A landing zone isn’t a one-time setup - it evolves through four phases. This iterative lifecycle, with regular checks for new technologies or requirements, ensures a future-proof foundation and supportive of long-term business goals. This lifecycle view is consistent with leading industry frameworks used by hyperscalers and cloud governance platforms. <br/> 
   1) `Planning & Design (Day 0)` - `Architectural blueprint stage`. Define the non-negotiable standards: governance, security policies, resource hierarchy, and network topology.
   2) `Implementation & Deployment (Day 1)` - `Implementation stage`. Automate the creation of the foundational environment using Infrastructure-as-Code (IaC).
   3) `Operations & Optimization (Day 2)` - `Run stage`. Focus is on routine tasks like monitoring, managing security posture, cost management, and ensuring the environment runs smoothly and efficiently.
   4) `Continuous Evolution: Evolve and Expand` : `Improvement stage`. Iteratively improve the landing zone by adopting new features, expanding to multi-cloud environments, and adapting to new compliance requirements. The decision node here (Check for Updates) is the feedback loop that sends requirements back to the Plan and Design phase (or back to Evolve) to restart the cycle, ensuring the zone never becomes obsolete. <br/>

   <img width="341" height="652" alt="image" src="https://github.com/user-attachments/assets/4b8e6b35-e0c7-4b78-a301-f1eee575e5c4" />

# Landing Zone Checklist by Day 0 / Day 1 / Day 2
Leverage this phased checklist to ensure completeness across planning, deployment, and operations — from identity to automation — across AWS, Azure, and GCP. Use the “lowest common denominator” principle (e.g., `lowercase` `snake_case` for tags) to ensure compatibility and reliability in multi-cloud environments. <br/>
### Day 0 – Planning & Design

| **Category**              | **Key Activities**                                                                                                                                              |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Identity & Access**     | - Define Org-level IAM roles, groups, policies  <br> - Plan SSO, MFA, and identity federation (Azure AD / Okta / Workspace)  <br> - Set least-privilege model   |
| **Resource Organization** | - Design folder/account/project hierarchy  <br> - Define naming conventions  <br> - Establish tagging standards  <br> - Plan Dev/Test/Prod environments         |
| **Networking**            | - Define IP CIDR ranges  <br> - Plan VPC/VNet and subnet structure  <br> - Plan shared networking (Hub-Spoke, Transit Gateway)  <br> - Plan DNS strategy        |
| **Security & Compliance** | - Define Org-level policies or SCPs  <br> - Choose baseline frameworks (e.g., CIS, NIST)  <br> - Plan encryption strategy  <br> - Plan vulnerability management |
| **Automation & IaC**      | - Set up Terraform/Bicep/CloudFormation baseline modules  <br> - Define Git repo structure  <br> - Plan CI/CD pipelines  <br> - Plan Policy-as-Code             |
| **Cost Management**       | - Define budget controls  <br> - Plan tagging for cost allocation  <br> - Enable billing export or APIs                                                         |

### Day 1 – Implementation & Deployment

| **Category**              | **Key Activities**                                                                                                                           |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **Identity & Access**     | - Implement IAM roles/groups  <br> - Integrate SSO and MFA  <br> - Enforce least-privilege policies                                          |
| **Resource Organization** | - Create folder/project/account structure  <br> - Apply naming conventions  <br> - Enforce tagging via policies                              |
| **Networking**            | - Deploy VPC/VNet, subnets, routes, and firewalls  <br> - Set up DNS and hybrid connectivity (VPN/Interconnect)                              |
| **Security & Compliance** | - Apply encryption at rest/in transit  <br> - Deploy baseline security tools (e.g., GuardDuty, SCC, Defender)  <br> - Implement org policies |
| **Monitoring & Logging**  | - Enable centralized logging and audit trails  <br> - Set up log sinks and storage  <br> - Start basic metrics/alerting setup                |
| **Automation & IaC**      | - Deploy environments using IaC  <br> - Integrate IaC into CI/CD  <br> - Validate using Policy-as-Code                                       |
| **Cost Management**       | - Apply budgets and alerts  <br> - Enable detailed billing reports  <br> - Tag deployed resources                                            |
### Day 2 – Operations & Optimization

| **Category**              | **Key Activities**                                                                                                           |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| **Identity & Access**     | - Audit IAM access regularly  <br> - Rotate credentials and keys  <br> - Monitor access logs                                 |
| **Resource Organization** | - Refactor/optimize structure as needed  <br> - Review tagging consistency                                                   |
| **Networking**            | - Monitor network traffic and update routes/firewalls as needed  <br> - Fine-tune hybrid links                               |
| **Security & Compliance** | - Run periodic vulnerability scans and patching  <br> - Conduct compliance audits and reporting                              |
| **Monitoring & Logging**  | - Refine dashboards and alerts  <br> - Integrate logs with SIEM  <br> - Optimize log retention and storage costs             |
| **Automation & IaC**      | - Improve reusability of modules  <br> - Extend CI/CD to include drift detection  <br> - Add guardrails and pre-commit hooks |
| **Cost Management**       | - Use cost reports for optimization  <br> - Apply right-sizing recommendations  <br> - Review chargeback reports             |

### Continuous Evolution – Evolve and Expand

| **Category**              | **Key Activities**                                                                                                           |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| **Identity & Access**     | - Update IAM policies for new services or compliance needs  <br> - Expand SSO to additional clouds                           |
| **Resource Organization** | - Refactor/optimize structure as needed  <br> - Review tagging consistency & Update tagging standards (e.g., new  tags)      |
| **Networking**            | - Monitor network traffic and integrate new regions/cloud as needed  <br> - Fine-tune and/or adopt new connectivity solutions|
| **Security & Compliance** | - Run periodic vulnerability scans and patching  <br> - Conduct compliance audits & reporting. Deploy updated security tools |
| **Monitoring & Logging**  | - Refine dashboards and alerts  <br> - Integrate logs with SIEM  <br> - Optimize log retention and storage costs, multi-cloud|
| **Automation & IaC**      | - Improve reusability of modules  <br> - Extend CI/CD to include drift detection  <br> - Add guardrails and pre-commit hooks |
| **Cost Management**       | - Use cost reports for optimization  <br> - Apply right-sizing recommendations  <br> - Review chargeback reports             |

# Azure Landing Zone accelerators
Below are some official material to jumpstart Azure landing zones. <br/>
   * [ALZ Accelerator](https://azure.github.io/Azure-Landing-Zones/accelerator/) - current "source of truth" for implementation.[This YouTube video is also good](https://www.youtube.com/watch?v=8VftZLHE2Zc).<br/>
   * [Leverage AI Landing Zones in Azure](https://www.youtube.com/watch?v=8FWdFG55nXw) - this also includes a Microsoft training on AI Landing zones.
   * [Implementing Zero Trust with Azure Landing Zones](https://www.youtube.com/watch?v=sZlbmqxA6TQ)<br/>
   * [Azure Landing Zone Accelerator playlist](https://www.youtube.com/playlist?list=PLFUSGymaphfBzMvjG-6VqPgpf280LC6tb) - This is not official Microsoft material but the links discussed are official.
   * [Understanding Azure Landing Zone Design Areas](https://www.youtube.com/watch?v=5D_o9b4qRNg)<br/>

# Appendix
Below are some additional resources and references for further learning: <br/>
1. [What is an Azure landing zone?- Official Azure Landing Zones Documentation by Microsoft](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/)<br/>
2. [Checklist : Building Azure Right: A Practical Checklist for Infrastructure Landing Zones](https://techcommunity.microsoft.com/blog/azureinfrastructureblog/building-azure-right-a-practical-checklist-for-infrastructure-landing-zones/4409975)<br/>
3. [What are Day-0, Day-1, and Day-2 Operations?](https://spacelift.io/blog/day-0-day-1-day-2-operations)<br/>
4. [Cloud Landing Zone Lifecycle explained](https://www.meshcloud.io/en/blog/cloud-landing-zone-lifecycle-explained/)<br/>
5. [What is a Cloud Landing Zone?](https://www.appvia.io/blog/what-is-a-cloud-landing-zone)<br/>
6. [How to Use Tags in Terraform? [Overview & Examples]](https://spacelift.io/blog/terraform-tags)<br/>
7. [Best Practices for Tagging AWS Resources](https://docs.aws.amazon.com/whitepapers/latest/tagging-best-practices/tagging-best-practices.html)<br/>
8. [The Complete Azure Tagging Guide](https://www.cloudzero.com/blog/azure-tagging-guide/)<br/>
9. [Define your tagging strategy in Azure](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-tagging)<br/>
10. [Azure Landing Zones | Architectural Blueprint, Tooling & Best Practices](https://www.youtube.com/watch?v=VTnqUDMchXA)<br/>
11. [Document your Landing Zone](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/document-your-aws-landing-zone-design.html#attachments-9e39a05a-8f51-4fe3-8999-522feafed6ca)<br/>
