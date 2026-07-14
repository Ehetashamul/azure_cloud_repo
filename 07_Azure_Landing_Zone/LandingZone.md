# What is Azure Landing Zone?

### One-line Interview Answer

> **Azure Landing Zone is a pre-configured, secure, governed, and scalable Azure foundation where application teams can safely deploy workloads according to organizational standards.**


What Does a Landing Zone Actually Contain?

```
Azure Tenant
│
├── Management Groups
│
├── Subscriptions
│      ├── Identity
│      ├── Connectivity
│      ├── Management
│      ├── Security
│      └── Workload
│
├── RBAC
│
├── Azure Policy
│
├── Networking
│
├── Monitoring
│
├── Security Controls
│
├── Cost Management
│
└── Automation (Terraform/Bicep)
```

Every component in the Landing Zone has a specific responsibility.

Components Explained (Interview Ready)
1. Azure Tenant
What is it?
An Azure Tenant is the highest-level container in Azure that represents your organization. It is associated with Microsoft Entra ID and manages identities, authentication, and access across all Azure resources.
Interview One-Liner:
	Azure Tenant is the root of the Azure hierarchy where users, groups, applications, and subscriptions are managed.

2. Management Groups
What is it?
Management Groups organize multiple subscriptions into a hierarchy so you can apply governance consistently across them.
Why is it used?
Instead of applying policies and permissions to each subscription individually, you apply them once at the Management Group level.
Interview One-Liner:
	Management Groups provide centralized governance by grouping subscriptions and applying RBAC and Azure Policies at scale.

3. Subscriptions
What is it?
A Subscription is a logical boundary for Azure resources that provides billing, access control, and resource isolation.
Why is it used?
Different teams or environments (Production, Development, Security) often use separate subscriptions to isolate workloads and manage costs.
Interview One-Liner:
	A subscription is an isolated container for Azure resources with its own billing, quotas, and access controls.

4. Identity Subscription
What is it?
A dedicated subscription that hosts identity-related services such as Microsoft Entra ID integration, Managed Identities, and sometimes domain services.
Why is it used?
Keeping identity services separate improves security and simplifies centralized identity management.
Interview One-Liner:
	The Identity subscription centralizes authentication and identity-related services used across the organization.

5. Connectivity Subscription
What is it?
A subscription dedicated to shared networking resources.
Usually Contains
	• Hub Virtual Network
	• VPN Gateway
	• ExpressRoute Gateway
	• Azure Firewall
	• Bastion
	• DNS
Why is it used?
It acts as the central networking hub that connects all application environments securely.
Interview One-Liner:
	The Connectivity subscription hosts shared networking services that securely connect all workloads.

6. Management Subscription
What is it?
A subscription dedicated to monitoring and operational management services.
Usually Contains
	• Azure Monitor
	• Log Analytics Workspace
	• Automation Account
	• Backup Vault
	• Alerts
	• Update Management
Why is it used?
It provides centralized monitoring, logging, backup, and automation for all subscriptions.
Interview One-Liner:
	The Management subscription provides centralized monitoring, logging, backup, and operational automation.

7. Security Subscription
What is it?
A subscription that hosts centralized security and compliance services.
Usually Contains
	• Microsoft Defender for Cloud
	• Microsoft Sentinel
	• Azure Policy
	• Security Center
	• Compliance dashboards
Why is it used?
To monitor security posture and enforce compliance across the organization.
Interview One-Liner:
	The Security subscription centralizes threat detection, compliance, and security monitoring.

8. Workload Subscription
What is it?
This is where application teams deploy their business applications.
Usually Contains
	• Virtual Machines
	• AKS
	• App Service
	• Azure SQL
	• Storage Accounts
	• Azure Functions
Why is it used?
Developers deploy only application resources without affecting shared platform services.
Interview One-Liner:
	Workload subscriptions are dedicated to hosting business applications while platform resources remain centrally managed.

9. RBAC (Role-Based Access Control)
What is it?
RBAC controls who can perform what actions on which Azure resources.
Example
	• Reader
	• Contributor
	• Owner
	• Custom Roles
Why is it used?
To implement the Principle of Least Privilege (PoLP), ensuring users have only the permissions they need.
Interview One-Liner:
	RBAC manages user permissions by assigning roles at different scopes such as Management Group, Subscription, Resource Group, or Resource.

10. Azure Policy
What is it?
Azure Policy enforces organizational standards by evaluating and controlling resource configurations.
Example
	• Allow only East US region
	• Enforce resource tags
	• Require encryption
	• Restrict VM SKUs
Why is it used?
To ensure all deployed resources comply with company governance rules.
Interview One-Liner:
	Azure Policy enforces governance by auditing, denying, or automatically remediating non-compliant resources.

11. Networking
What is it?
Networking enables secure communication between Azure resources and on-premises environments.
Usually Includes
	• VNets
	• Subnets
	• NSGs
	• Route Tables
	• Azure Firewall
	• VPN Gateway
	• ExpressRoute
	• Private Endpoints
Interview One-Liner:
	Networking provides secure connectivity between Azure resources, users, and on-premises environments.

12. Monitoring
What is it?
Monitoring collects metrics, logs, and alerts to track the health and performance of Azure resources.
Usually Uses
	• Azure Monitor
	• Log Analytics
	• Application Insights
	• Alerts
Interview One-Liner:
	Monitoring provides visibility into application health, performance, and resource utilization through metrics and logs.

13. Security Controls
What is it?
Security controls protect Azure resources from threats and ensure they comply with organizational security standards.
Includes
	• Microsoft Defender for Cloud
	• NSGs
	• Azure Firewall
	• Just-In-Time VM Access
	• Microsoft Sentinel
	• Azure Policy
Interview One-Liner:
	Security controls protect workloads using preventive, detective, and corrective security measures.

14. Cost Management
What is it?
Cost Management helps organizations monitor, analyze, and optimize Azure spending.
Features
	• Budgets
	• Cost Analysis
	• Alerts
	• Recommendations
	• Chargeback
Interview One-Liner:
	Azure Cost Management helps monitor cloud spending, create budgets, and optimize resource costs.

15. Automation (Terraform/Bicep)
What is it?
Automation provisions Azure infrastructure using Infrastructure as Code (IaC), ensuring repeatable and consistent deployments.
Tools
	• Terraform
	• Bicep
	• ARM Templates
	• Azure DevOps Pipelines
	• GitHub Actions
Why is it used?
To eliminate manual deployments and maintain consistency across environments.
Interview One-Liner:
	Automation uses Infrastructure as Code to deploy Azure resources consistently, quickly, and with minimal human error.

⭐ 5–10 Years Interview Tip
Interviewers often ask: "What are the pillars of an Azure Landing Zone?"
A simple way to remember them is:
Pillar	Purpose
Identity	Authentication and access management
Governance	Management Groups, Azure Policy, RBAC
Networking	Secure connectivity using Hub-Spoke or Virtual WAN
Security	Defender for Cloud, Sentinel, Firewall, Compliance
Management	Monitoring, Logging, Backup, Automation
Cost Management	Budgets, Cost Analysis, Resource Optimization
Automation (IaC)	Terraform, Bicep, ARM for repeatable deployments
This level of explanation is typically sufficient for 5–10 years Azure DevOps interviews, as it demonstrates not just the names of the components but also their purpose, the services involved, and how they fit into an enterprise Azure Landing Zone.

Azure Landing Zone Using Terraform
Infrastructure is usually deployed through Terraform rather than the Azure portal.
Example project structure:

```
terraform/
├── backend.tf
├── providers.tf
├── variables.tf
├── outputs.tf
├── versions.tf
├── main.tf
│
├── modules/
│     ├── management-groups/
│     ├── subscriptions/
│     ├── networking/
│     ├── firewall/
│     ├── monitoring/
│     ├── policies/
│     ├── role-assignments/
│     ├── keyvault/
│     └── loganalytics/
│
├── environments/
│     ├── dev/
│     ├── test/
│     └── prod/
│
└── pipelines/
      └── azure-pipelines.yml

```
Best Practices
	• Remote backend (Azure Storage) 
	• State locking 
	• Separate state files per environment 
	• Reusable modules 
	• CI/CD automation 
	• Pull Request approval before Apply 

Azure Landing Zone Deployment Lifecycle
A typical enterprise implementation follows this sequence:
Create Azure Tenant
        │
Create Management Groups
        │
Create Platform Subscriptions
        │
Configure Microsoft Entra ID
        │
Configure RBAC
        │
Apply Azure Policies
        │
Deploy Hub-Spoke Network
        │
Configure Firewall & Bastion
        │
Configure Monitoring
        │
Configure Security
        │
Create Workload Subscriptions
        │
Deploy Applications


Azure DevOps CI/CD Pipeline for Landing Zone
Landing Zones are treated as Infrastructure as Code (IaC).

Developer
      │
Git Push
      │
Pull Request
      │
Terraform fmt
      │
Terraform validate
      │
Terraform init
      │
Terraform plan
      │
Manual Approval
      │
Terraform apply
      │
Landing Zone Updated
      │
Application Deployment
Pipeline Tasks
	• Code Quality Checks 
	• Terraform Validation 
	• Security Scanning (Checkov/TFSec) 
	• Plan Artifact Publishing 
	• Manual Approval 
	• Apply Stage 
	• Post Deployment Validation 

Explain the Azure Hierarchy

This is one of the most frequently asked interview questions.

Add this section:

```text
Azure Tenant
      │
Management Groups
      │
Subscriptions
      │
Resource Groups
      │
Resources
```

Explain each:

| Level            | Purpose               |
| ---------------- | --------------------- |
| Tenant           | Root of Azure         |
| Management Group | Governance            |
| Subscription     | Billing & Isolation   |
| Resource Group   | Logical grouping      |
| Resource         | VM, Storage, SQL, AKS |

Platform Landing Zone vs Application Landing Zone

This is often asked in enterprise interviews.

Example:

```
Landing Zone

├── Platform Landing Zone
│      ├── Identity
│      ├── Networking
│      ├── Security
│      └── Monitoring
│
└── Application Landing Zone
       ├── HR App
       ├── CRM
       ├── SAP
       └── Finance
```

Explain:

Platform Landing Zone

* Managed by Cloud Team
* Shared Services
* Networking
* Security

Application Landing Zone

* Managed by App Team
* Hosts workloads

---
Hub-Spoke Diagram

You mention Hub-Spoke, but never show it.

Interviewers LOVE this.

Example

```text
                 On-Premises
                      │
               ExpressRoute/VPN
                      │
                 Hub VNet
        ┌────────────┼────────────┐
        │            │            │
 Azure Firewall  Bastion     DNS
        │
────────┼───────────────
│        │         │
Spoke1  Spoke2   Spoke3

HR      CRM      SAP
```

Then explain:

Hub

* Shared services

Spokes

* Application VNets


Interview Questions (5–10 Years)
1. What is Azure Landing Zone?
Answer:
Azure Landing Zone is a secure, scalable, and governed Azure foundation that provides standardized identity, networking, security, governance, monitoring, and resource organization before workloads are deployed.

2. Why do enterprises use Landing Zones?
Answer:
	• Standardization
	• Security
	• Governance
	• Compliance
	• Scalability
	• Cost management
	• Faster onboarding of new projects

3. Is Azure Landing Zone a service?
Answer:
No. It is not an Azure service. It is a reference architecture and implementation approach based on Microsoft's Cloud Adoption Framework, combining Azure services, policies, and best practices into a reusable foundation.

4. What is the difference between a Landing Zone and a Subscription?
Landing Zone	Subscription
Architectural foundation	Billing and resource boundary
Includes governance, identity, networking, security	Contains Azure resources
Can span multiple subscriptions	Single Azure subscription

5. Which Azure services are commonly part of a Landing Zone?
```
Answer:
	• Microsoft Entra ID
	• Management Groups
	• Azure Policy
	• RBAC
	• Azure Monitor
	• Log Analytics
	• Azure Firewall
	• Azure Bastion
	• Key Vault
	• Virtual Networks (Hub-Spoke)
	• Defender for Cloud
	• Private DNS
	• VPN Gateway / ExpressRoute
```
6. How is Terraform used with Azure Landing Zones?
Answer:
Terraform is commonly used to provision and manage Landing Zone resources as Infrastructure as Code (IaC). Teams version the code in Git, run terraform plan for review, and terraform apply through Azure DevOps or GitHub Actions pipelines to ensure repeatable, auditable deployments.

7. What is the role of Azure Policy in a Landing Zone?
Answer:
Azure Policy enforces organizational standards, such as restricting allowed regions, requiring tags, preventing public IP creation, and ensuring only approved resource SKUs are deployed.

8. What is the role of Management Groups?
Answer:
Management Groups organize subscriptions hierarchically, allowing policies, RBAC assignments, and governance controls to be inherited across multiple subscriptions.

9. How does a Landing Zone improve security?
Answer:
	• Centralized identity with Entra ID
	• Least-privilege access using RBAC
	• Azure Policy enforcement
	• Network segmentation (Hub-Spoke)
	• Azure Firewall and NSGs
	• Private Endpoints
	• Key Vault for secrets
	• Continuous monitoring with Defender for Cloud

10. What happens if an organization skips a Landing Zone?
Answer:
Without a Landing Zone, environments often become inconsistent and difficult to manage. Common issues include weak governance, security gaps, inconsistent networking, poor cost visibility, and slower project onboarding due to the lack of standardized infrastructure.

Quick Interview Summary
Topic	Key Point
Purpose	Standardized Azure foundation
Based On	Microsoft Cloud Adoption Framework (CAF)
Main Goal	Secure, governed, scalable cloud environment
Core Components	Identity, Networking, Security, Governance, Monitoring
IaC	Terraform, Bicep, ARM
Network Design	Hub-Spoke
Governance	Management Groups, Azure Policy, RBAC
Monitoring	Azure Monitor, Log Analytics, Application Insights
Security	Defender for Cloud, Firewall, Key Vault, Private Endpoints
DevOps Role	Automate Landing Zone provisioning, enforce governance, and deploy compliant workloads through CI/CD pipelines