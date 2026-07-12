# Cloud Computing

## Definition

Cloud computing allows organizations and individuals to access IT resources **on demand** over the internet, paying only for the resources they use.

These resources include:

- Servers
- Storage
- Databases
- Networking
- Software
- Analytics
- Artificial Intelligence (AI)
- Security services

---

## Key Benefits

- ✅ Scalability
- ✅ Cost Efficiency (Pay-as-you-go)
- ✅ Flexibility
- ✅ Reduced Infrastructure Maintenance
- ✅ High Availability
- ✅ Global Accessibility
- ✅ Faster Deployment
- ✅ Improved Security & Compliance

---

## Daily Life Examples

- **Google Drive** – Cloud Storage
- **Microsoft 365** – Office Productivity Suite
- **Netflix** – Cloud-based Video Streaming
- **Gmail** – Cloud Email Service
- **Dropbox** – File Sharing & Storage

---

# Types of Cloud Computing Services

Cloud services are mainly divided into three categories:

```text
               Cloud Computing
                     │
      ┌──────────────┼──────────────┐
      │              │              │
      ▼              ▼              ▼
    IaaS           PaaS           SaaS
Infrastructure   Platform       Software
  as a Service  as a Service   as a Service
```

---

# 1. Infrastructure as a Service (IaaS)

## Definition

Infrastructure as a Service (IaaS) provides virtualized computing resources over the internet, such as:

- Virtual Machines (VMs)
- Storage
- Networking
- Load Balancers
- Firewalls

You rent the infrastructure while managing the operating system and applications yourself.

---

## What You Manage

- Operating System
- Applications
- Runtime
- Middleware
- Data

The cloud provider manages:

- Physical Servers
- Networking
- Storage
- Virtualization

---

## Analogy

**Renting an empty plot of land.**

The landowner provides the land, but you build and maintain the house yourself.

---

## Examples

- Microsoft Azure Virtual Machines
- AWS EC2
- Google Compute Engine

---

## Best Use Cases

- Lift-and-shift migration
- Hosting applications
- Disaster Recovery
- Development & Testing
- Running custom software

---

# 2. Platform as a Service (PaaS)

## Definition

Platform as a Service (PaaS) provides a managed platform for developing, testing, and deploying applications.

The cloud provider manages the infrastructure, operating system, runtime, and middleware.

Developers only focus on writing application code.

---

## What You Manage

- Applications
- Data

The cloud provider manages:

- Servers
- Storage
- Networking
- Operating System
- Runtime
- Middleware

---

## Analogy

**Renting a fully equipped workshop.**

All tools are already available—you simply bring your project and start building.

---

## Examples

- Azure App Service
- Google App Engine
- AWS Elastic Beanstalk
- Azure Functions

---

## Best Use Cases

- Web Applications
- APIs
- Mobile Applications
- Microservices
- CI/CD Deployments

---

# 3. Software as a Service (SaaS)

## Definition

Software as a Service (SaaS) provides fully managed software applications that users access through a web browser or mobile application.

No installation, maintenance, or infrastructure management is required.

---

## What You Manage

Nothing.

You simply use the software.

The cloud provider manages everything.

---

## Analogy

**Renting a fully furnished apartment.**

You move in and start using it immediately.

---

## Examples

- Microsoft 365
- Google Workspace
- Salesforce
- Dropbox
- Zoom
- Slack

---

## Best Use Cases

- Email
- Document Editing
- CRM
- Collaboration
- File Sharing
- Video Conferencing

---

# Responsibility Comparison

| Component | IaaS | PaaS | SaaS |
|------------|:----:|:----:|:----:|
| Applications | You | You | Provider |
| Data | You | You | Provider |
| Runtime | You | Provider | Provider |
| Middleware | You | Provider | Provider |
| Operating System | You | Provider | Provider |
| Virtualization | Provider | Provider | Provider |
| Servers | Provider | Provider | Provider |
| Storage | Provider | Provider | Provider |
| Networking | Provider | Provider | Provider |

---

# Quick Comparison

| Feature | IaaS | PaaS | SaaS |
|---------|------|------|------|
| Infrastructure | Virtual Infrastructure | Managed Platform | Complete Software |
| User Responsibility | High | Medium | None |
| Flexibility | Highest | Medium | Lowest |
| Maintenance | High | Medium | None |
| Target Users | System Admins, DevOps | Developers | End Users |

---

# Easy Interview Trick

```text
IaaS
"I manage almost everything."

PaaS
"I only manage my application."

SaaS
"I simply use the software."
```

---

# Real-World Azure Example

```text
Developer
     │
     ▼
Azure App Service (PaaS)
     │
     ▼
Azure SQL Database
     │
     ▼
Azure Storage

------------------------------

Administrator
     │
     ▼
Azure Virtual Machine (IaaS)
     │
Install OS
Configure IIS
Deploy Application
Manage Updates

------------------------------

Business User
     │
     ▼
Microsoft 365 (SaaS)
```

---

# Interview Summary

- **Cloud Computing** delivers IT resources over the internet on a **pay-as-you-go** basis.
- **IaaS** provides virtual infrastructure where you manage the OS and applications.
- **PaaS** provides a managed development platform where you only manage the application and data.
- **SaaS** provides fully managed software that users access directly without managing any infrastructure.
- The responsibility shifts from the customer to the cloud provider as you move from **IaaS → PaaS → SaaS**.