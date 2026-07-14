Azure Application Gateway
What problem does Azure Application Gateway solve?
Imagine your company has multiple web applications running on Azure.
Users
   │
   ▼
Public IP
   │
   ▼
VM-1 (Website)
VM-2 (API)
VM-3 (Admin Portal)
Problems:
	• Every application needs its own Public IP ❌
	• SSL certificates need to be managed separately ❌
	• No protection from common web attacks ❌
	• Difficult to route users to different applications ❌
Azure Application Gateway solves all these problems.

Simple Definition
	Azure Application Gateway is a Layer-7 (Application Layer) Load Balancer that intelligently routes HTTP/HTTPS traffic based on URL, Hostname, Headers, or Cookies while providing SSL termination and Web Application Firewall (WAF) protection.
Unlike Azure Load Balancer (Layer 4), Application Gateway understands HTTP requests.

Real World Example
Suppose your company owns:
www.company.com
api.company.com
admin.company.com
Without Application Gateway
Internet
│
   ├────────► Website VM
   │
   ├────────► API VM
   │
   └────────► Admin VM
Three Public IPs
Three SSL Certificates
Three Entry Points

With Application Gateway
                Internet
                    │
             Public IP
                    │
                    ▼
        Azure Application Gateway
        +-----------------------+
        | SSL Termination       |
        | URL Routing           |
        | WAF                   |
        | Health Probes         |
        +-----------------------+
            │       │        │
            ▼       ▼        ▼
        Website    API     Admin
Single Public IP
Single Entry Point
Centralized SSL
Centralized Security

OSI Layer
Azure Application Gateway works at
Layer 7
(Application Layer)
It understands
	• URL
	• HTTP Headers
	• Cookies
	• Hostnames
	• Query Strings
This makes intelligent routing possible.

How Request Flows
User
│

▼
DNS
│

▼
Public IP
│

▼
Azure Application Gateway
│

├── SSL Decryption

├── WAF Inspection

├── URL Matching

├── Health Check

└── Route Request

│

▼
Backend Pool
│

▼
Web Server

Major Components
1. Frontend IP
The public or private IP where users connect.
Example
20.50.100.10
or
app.company.com

2. Listener
Listener waits for incoming requests.
It listens on
	• HTTP (80)
	• HTTPS (443)
Example
HTTPS Listener
Host:
company.com
Port:
443

3. Backend Pool
Collection of backend servers.
Example
VM1
VM2
VM3
AKS
App Service

4. Backend Settings
Defines
	• Port
	• Protocol
	• Timeout
	• Cookie Affinity
Example
Protocol
HTTPS
Port
443
Timeout
30 sec

5. Health Probe
Application Gateway continuously checks
"Is backend healthy?"
Gateway
│

├── /health

├── /status

└── /

If unhealthy
↓
Traffic is not sent.

6. Routing Rule
Maps incoming requests to backend pools.
Example
/api/*
↓
API Server
/admin/*
↓
Admin Server
/
↓
Website

URL Path Routing Example
User requests
company.com/images/logo.png
↓
Images Server

User requests
company.com/api/users
↓
API Server

User requests
company.com/admin
↓
Admin Server

Configuration
/api/*
→ API Pool

/images/*
→ Image Pool

/admin/*
→ Admin Pool

/*
→ Website Pool


Host-Based Routing
One Gateway
Many Domains
shop.company.com
↓

Shopping Server
--------------------
hr.company.com
↓

HR Server
--------------------
crm.company.com
↓

CRM Server
Only one Application Gateway is required.

SSL Termination
Normally
User
↓

Encrypted
↓

Backend Server decrypts
Every server handles SSL.

With Application Gateway
User
↓

Encrypted
↓

Application Gateway decrypts
↓

Plain HTTP or HTTPS
↓

Backend
Benefits
	• Better performance
	• Easier certificate management
	• Lower CPU usage on backend servers

End-to-End SSL
Sometimes security policies require encryption all the way to the backend.
User
↓

HTTPS
↓

Application Gateway
↓

HTTPS
↓

Backend
This is called End-to-End SSL.

Cookie-Based Session Affinity
Some applications require the same user to always hit the same server.
Without Affinity
User
↓

VM1
↓

VM2
↓

VM3
Session may break.

With Cookie Affinity
User
↓

VM2
↓

VM2
↓

VM2
Application Gateway inserts a cookie to keep requests on the same backend.

Web Application Firewall (WAF)
Application Gateway can include WAF.
It blocks attacks like
	• SQL Injection
	• Cross-Site Scripting (XSS)
	• Remote File Inclusion
	• Command Injection
	• Malicious Bots
Example
User
↓

Malicious Request
↓

Application Gateway WAF
↓

Blocked
×
Never reaches backend

Autoscaling
Traffic
Morning
500 Users
↓

2 Instances
--------------------
Evening
20,000 Users
↓

8 Instances
--------------------
Night
100 Users
↓

2 Instances
Application Gateway automatically scales based on demand.

Backend Types Supported
Application Gateway can route traffic to:
	• Azure Virtual Machines
	• Virtual Machine Scale Sets (VMSS)
	• Azure App Service
	• Azure Kubernetes Service (AKS) via Ingress Controller
	• Private IPs
	• On-premises servers (connected through VPN/ExpressRoute)

Azure Application Gateway vs Azure Load Balancer
Feature	Azure Application Gateway	Azure Load Balancer
OSI Layer	Layer 7	Layer 4
Protocol	HTTP/HTTPS	TCP/UDP
URL Routing	✅	❌
Host-Based Routing	✅	❌
SSL Termination	✅	❌
WAF	✅	❌
Cookie Affinity	✅	❌
Health Probe	✅	✅
TCP Load Balancing	❌	✅

Azure Application Gateway vs Azure Front Door
Application Gateway	Front Door
Regional Service	Global Service
Works inside one Azure region	Global load balancing across regions
Layer 7	Layer 7
Internal or External	Public Internet only
Ideal for regional applications	Ideal for global applications

DevOps Interview Scenario
Scenario: Your company hosts:
	• company.com
	• company.com/api
	• company.com/admin
You want:
	• One Public IP
	• SSL termination
	• WAF protection
	• URL-based routing
	• Automatic health checks
Solution:
	• Deploy one Azure Application Gateway.
	• Configure an HTTPS listener with a single SSL certificate.
	• Create backend pools for Website, API, and Admin.
	• Add URL path-based routing rules (/, /api/*, /admin/*).
	• Enable WAF in Prevention mode.
	• Configure health probes for each backend.
	• Enable autoscaling if traffic varies significantly.

Common Interview Questions (5–10 Years)
1. Why use Azure Application Gateway instead of Azure Load Balancer?
Answer: Application Gateway operates at Layer 7 and supports URL-based routing, host-based routing, SSL termination, cookie affinity, and WAF. Azure Load Balancer works at Layer 4 and distributes TCP/UDP traffic without inspecting HTTP requests.
2. What is SSL termination?
Answer: The Application Gateway decrypts HTTPS traffic, processes it, and forwards it to the backend using HTTP or HTTPS, reducing certificate management overhead on backend servers.
3. What is a Backend Pool?
Answer: A logical group of backend targets (VMs, VMSS, App Services, AKS, or IP addresses) that receive traffic from the gateway.
4. What happens if a backend becomes unhealthy?
Answer: Health probes detect the failure, and the gateway stops routing traffic to that backend until it becomes healthy again.
5. What is the difference between WAF Detection and Prevention mode?
Answer: Detection mode logs malicious requests without blocking them, while Prevention mode actively blocks requests that match WAF rules.
6. Can Azure Application Gateway route traffic to AKS?
Answer: Yes. It integrates with AKS using the Application Gateway Ingress Controller (AGIC) or the newer Application Gateway for Containers, enabling Layer-7 routing to Kubernetes services.

Quick Revision (30 Seconds)
	• Azure Application Gateway = Layer 7 Load Balancer
	• Routes HTTP/HTTPS traffic intelligently.
	• Supports URL-based and host-based routing.
	• Performs SSL termination or end-to-end SSL.
	• Uses health probes to avoid unhealthy backends.
	• Provides Web Application Firewall (WAF) for application security.
	• Supports cookie-based session affinity.
	• Can route traffic to VMs, VMSS, App Services, AKS, and on-premises backends.
	• Best choice when you need smart web traffic routing and application-level security.
