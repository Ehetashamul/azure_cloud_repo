What problem does Azure Front Door solve?
Imagine your company has users from:
```text
• 🇮🇳 India
• 🇺🇸 USA
• 🇬🇧 UK
• 🇦🇺 Australia
But your application is deployed only in East US.
India Users  ─────┐
USA Users ────────┤
UK Users ─────────┤────► East US Web App
Australia Users ──┘
```

Problems:
❌ Users in India experience high latency.
❌ If East US region fails, the application goes down.
❌ Traffic is not intelligently routed.
❌ No Web Application Firewall at the edge.
This is where Azure Front Door comes in.

Simple Definition
	Azure Front Door is Microsoft's Global Layer-7 (HTTP/HTTPS) Load Balancer and Application Delivery Network that routes users to the nearest healthy backend while providing security, caching, SSL termination, and acceleration.
Think of it as
	The Global Intelligent Entry Gate for your web application.

Real-Life Example
Imagine Amazon.
Customers visit:
amazon.com
Nobody knows where the servers actually are.
When you open Amazon:
	1. DNS resolves.
	2. You connect to the nearest edge location.
	3. Traffic is inspected.
	4. WAF checks attacks.
	5. Front Door selects the fastest healthy backend.
	6. Response comes back quickly.
Azure Front Door works exactly like this.

```text
Without Azure Front Door
			 Users Worldwide
│
				  ▼
East US Web App
Problems
❌ Long latency
❌ Region failure
❌ Slow response
❌ No edge security

With Azure Front Door
		  Users Worldwide
India   USA   Europe
│
			 ▼
Azure Front Door
(Global Entry Point + WAF)
/        |        \
East US   West Europe   Southeast Asia
App          App             App
Benefits:
✅ Lowest latency
✅ Automatic routing
✅ Global load balancing
✅ Failover
✅ WAF protection
```

Think Like This
Azure Front Door is like a smart airport.
Passengers arrive.
Airport decides:
	• Which gate?
	• Which runway?
	• Which flight?
	• Which route?
Similarly,
Front Door decides:
	• Best region
	• Healthy backend
	• Closest backend
	• Fastest backend

OSI Layer
Azure Front Door works at
Layer 7
HTTP
HTTPS
Because it understands:
URL
/api
/login
/images
Headers
Cookies
Host Names
Unlike Layer-4 Load Balancer.

```text
Global Architecture
		 DNS
│

▼
Azure Front Door
Edge POP (India)
│

Microsoft's Global Network
│

Health Probe Check
┌───────────────┐
	│               │
	▼               ▼
East US App      West Europe App
Traffic never goes over the public internet after entering Microsoft's backbone network.
```

Main Components
1. Frontend Host
Public endpoint.
Example
www.company.com
Users connect here.

2. Backend Pool (Origin Group)
Contains actual applications.
Example
Backend Pool
App Service East US
VMSS
AKS
Static Website
Application Gateway
External Website
Front Door chooses one backend.

3. Health Probe
Every few seconds:
GET /
200 OK
Healthy
500
Unhealthy
Timeout
Unhealthy
Unhealthy backend is automatically removed from traffic.

4. Routing Rules
Example:
/images
Storage Account
/api
AKS
/admin
Application Gateway
Everything Else
App Service
This is Path-Based Routing.

5. WAF
Blocks
✔ SQL Injection
✔ XSS
✔ OWASP attacks
✔ Bots
✔ Bad IPs
✔ Geo Blocking

6. Caching
Static content
CSS
JS
Images
Videos
Stored at edge.
Users receive content much faster.

```text
Request Flow
User
│

▼
DNS
│

▼
Nearest Front Door POP
│

▼
WAF
│

▼
Routing Rules
│

▼
Health Check
│

▼
Nearest Healthy Backend
│

▼
Response
```

Supported Backends
Azure Front Door can route to:
✅ App Service
✅ AKS
✅ Virtual Machines
✅ VM Scale Sets
✅ Application Gateway
✅ Storage Static Website
✅ External HTTP Server

Routing Methods
Latency Based
Nearest backend.
India User
↓

Southeast Asia
NOT
East US

Priority Based
Primary region first.
If failed,
Secondary region.
Primary
East US
↓

If Down
↓

West Europe

Weighted Routing
Traffic split.
East US
70%
West Europe
30%
Useful during migration.

SSL Termination
Instead of every server managing certificates,
Front Door handles SSL.
Client
HTTPS
↓

Front Door
Decrypts SSL
↓

HTTP/HTTPS
Backend
Much easier to manage certificates.

Custom Domain
Example
www.company.com
Instead of
company.azurefd.net
You can attach your own domain and certificate.

Failover Scenario
Initially
Front Door
↓

East US
Healthy
Suddenly
East US Down
Health Probe detects failure.
Automatically
Front Door
↓

West Europe
No manual intervention.

DevOps CI/CD Integration
Typical deployment flow:
Developer
↓

Git
↓

Azure DevOps Pipeline
↓

Terraform
↓

Azure Front Door
↓

Backend Pool
↓

App Service / AKS
Infrastructure and routing configuration can be fully automated using Terraform and Azure DevOps.

Azure Front Door vs Application Gateway vs Load Balancer
Feature	Azure Front Door	Application Gateway	Azure Load Balancer
Scope	Global	Regional	Regional
OSI Layer	Layer 7	Layer 7	Layer 4
HTTP/HTTPS Routing	✅	✅	❌
TCP/UDP	❌	❌	✅
WAF	✅	✅	❌
Global Load Balancing	✅	❌	❌
SSL Offloading	✅	✅	❌
Path-Based Routing	✅	✅	❌
Health Probes	✅	✅	✅
CDN Integration	✅	❌	❌

Interview Scenario
Question: Your application is deployed in East US and West Europe. Users should always connect to the nearest healthy region, and if one region fails, traffic should automatically fail over. You also need WAF protection and SSL offloading. What Azure service would you choose?
Answer: Azure Front Door, because it provides global Layer-7 load balancing, intelligent routing, automatic failover, Web Application Firewall (WAF), SSL termination, and traffic acceleration over Microsoft's global network.

Azure Front Door — Frequently Asked Interview Questions (5–10 Years Experience)

1. What is Azure Front Door, and how is it different from Azure Load Balancer?
Answer
Azure Front Door is a Layer-7 (Application Layer) Global Load Balancer that routes HTTP/HTTPS traffic to the closest and healthiest application endpoint worldwide.
Azure Load Balancer is a Layer-4 (Transport Layer) Regional Load Balancer that distributes TCP/UDP traffic within a single Azure region.
Comparison
Azure Front Door	Azure Load Balancer
Layer 7 (HTTP/HTTPS)	Layer 4 (TCP/UDP)
Global Service	Regional Service
URL-based routing	Port/IP-based routing
SSL Offloading	No SSL termination
Web Application Firewall	Not Available
CDN Integration	No
Health Probe across regions	Health Probe inside region
Interview Tip
	Azure Front Door is for global web applications, whereas Azure Load Balancer is for regional network traffic.

2. Why is Azure Front Door considered a Global Service?
Answer
Unlike Application Gateway or Load Balancer that are deployed inside one Azure region, Azure Front Door sits at Microsoft's global edge network.
User requests first reach the nearest Microsoft POP (Point of Presence).
From there Microsoft routes traffic through its private backbone network to the healthiest backend.
Benefits:
	• Lowest latency
	• Better performance
	• Automatic regional failover
	• Global availability
	• Reduced internet hops
Interview Follow-up
Q: Does Azure Front Door live inside your VNet?
Answer:
No.
It is a Microsoft-managed global edge service and does not reside inside customer VNets.

3. How does Health Probing work in Azure Front Door?
Answer
Azure Front Door continuously checks backend health by sending HTTP/HTTPS requests.
Example:
GET /health
If backend returns
HTTP 200 OK
it is considered healthy.
If multiple probes fail,
	• backend marked unhealthy
	• traffic redirected automatically
Health probes monitor:
	• Response code
	• Response time
	• Availability
Real-world Example
Suppose:
East US
West Europe
Central India
If East US becomes unavailable,
Front Door automatically routes users to West Europe or Central India.
No manual intervention required.

4. Explain Latency-Based, Priority-Based, and Weighted Routing.
A. Latency Routing
Routes traffic to the backend with the lowest latency.
Example:
India User
↓

Central India (20ms)
↓

East US (180ms)
Central India is selected.
Use Case
Global applications.

B. Priority Routing
Primary backend handles all traffic.
If unhealthy,
Secondary backend takes over.
Example
Primary
↓

East US
↓

Backup
↓

West Europe
Ideal for Disaster Recovery (DR).

C. Weighted Routing
Traffic distributed according to configured percentages.
Example
Server A → Weight 80

Server B → Weight 20

80% users → Server A
20% users → Server B
Use Cases:
	• Blue-Green Deployment
	• Canary Releases
	• A/B Testing

5. What is the difference between an Origin and an Origin Group?
Origin
An Origin is an actual backend resource.
Examples:
	• Azure App Service
	• AKS
	• VM
	• Public IP
	• On-premises server
Example
App Service East US
One Origin.

Origin Group
A logical collection of multiple Origins.
Example
Origin Group
├── East US

├── West Europe

└── Central India

Routing policies apply to the Origin Group.
Interview Tip
Origin = Backend server
Origin Group = Collection of backend servers

6. How does Front Door integrate with Azure WAF?
Answer
Azure Front Door integrates natively with Azure Web Application Firewall.
Incoming requests flow as:
Client
↓

Azure Front Door
↓

WAF Inspection
↓

Backend
WAF protects against:
	• SQL Injection
	• Cross-Site Scripting (XSS)
	• OWASP Top 10 attacks
	• Malicious bots
	• IP blocking
	• Geo filtering
	• Rate limiting
Benefits
	• Protection at Microsoft's edge
	• Traffic blocked before reaching backend
	• Reduced backend load

7. Can Front Door route traffic to on-premises or external applications?
Answer
Yes.
Front Door supports any publicly reachable backend.
Examples:
	• Azure App Service
	• AKS
	• Azure VM
	• AWS EC2
	• Google Cloud
	• On-premises web server
	• External public website
Requirement:
Backend must be accessible over Public HTTP/HTTPS.
Interview Follow-up
Q: Can Front Door connect directly to a private on-premises server?
Answer:
No, not directly. The origin must be publicly reachable, or you can use Azure Front Door Premium with Private Link for supported Azure private origins. A purely private on-premises server cannot be used unless it is exposed through a supported secure architecture.

8. How does SSL Termination improve performance?
Answer
Without SSL termination:
Client
↓

Encrypted Traffic
↓

Backend decrypts
Every backend performs SSL decryption.
CPU usage increases.

With SSL Termination:
Client
↓

Azure Front Door decrypts
↓

Backend (optional HTTPS or HTTP depending on configuration)
Benefits:
	• Lower backend CPU utilization
	• Faster response times
	• Centralized certificate management
	• Easier certificate renewal
	• Improved scalability

9. How would you deploy Azure Front Door using Terraform in an Azure DevOps Pipeline?
Answer
Typical CI/CD flow:
Developer
↓

Git Push
↓

Azure DevOps
↓

Terraform Init
↓

Terraform Validate
↓

Terraform Plan
↓

Approval
↓

Terraform Apply
↓

Azure Front Door Created
Typical Terraform resources:
	• azurerm_cdn_frontdoor_profile
	• azurerm_cdn_frontdoor_endpoint
	• azurerm_cdn_frontdoor_origin_group
	• azurerm_cdn_frontdoor_origin
	• azurerm_cdn_frontdoor_route
	• azurerm_cdn_frontdoor_firewall_policy (if using WAF)
Best Practices
	• Store Terraform state in Azure Storage Account.
	• Use Azure Key Vault for secrets.
	• Use remote state locking.
	• Separate environments using workspaces or separate state files.
	• Add manual approval before production deployment.
Interview Tip
Be ready to explain how you would implement:
	• Branch policies
	• Pull request validation
	• terraform fmt and terraform validate
	• Security scanning (Checkov/Terrascan)
	• Drift detection

10. When would you choose Azure Front Door over Application Gateway?
Choose Azure Front Door when:
	• Application serves users globally.
	• Need global load balancing.
	• Need automatic regional failover.
	• Need CDN acceleration.
	• Need edge-based WAF.
	• Multiple Azure regions are involved.
Choose Application Gateway when:
	• Traffic stays within one Azure region.
	• Need VNet integration.
	• Require private backend access.
	• Need WebSocket support and Layer-7 routing inside a region.
	• Internal enterprise applications.
Simple Rule
Global Application
        ↓
Azure Front Door

Regional Application
        ↓
Application Gateway


Additional Interview Questions (Recommended for 5–10 Years)
11. What is the request flow in Azure Front Door?
Answer
User
   │
   ▼
Nearest Microsoft Edge POP
   │
   ▼
WAF (Optional)
   │
   ▼
Routing Rules Evaluation
   │
   ▼
Origin Group
   │
   ▼
Healthy Origin
The request enters Microsoft's edge network first, where routing, WAF inspection, caching (if enabled), and health-based origin selection occur before reaching the backend.

12. Does Azure Front Door cache content?
Answer
Yes. Azure Front Door can cache static content such as images, CSS, JavaScript, and videos at edge locations.
Benefits:
	• Reduced backend load
	• Faster page load times
	• Lower latency
	• Improved user experience
Dynamic content is typically forwarded to the origin without caching unless configured otherwise.

13. How does Azure Front Door support Blue-Green or Canary deployments?
Answer
By using Weighted Routing.
Example:
	• Blue version → Weight 90
	• Green version → Weight 10
Initially, 10% of users access the new version. If no issues are detected, gradually increase the weight until all traffic is routed to the new version.

14. What monitoring options are available for Azure Front Door?
Answer
Azure Front Door integrates with:
	• Azure Monitor
	• Log Analytics
	• Azure Metrics
	• Azure Alerts
	• Application Insights
Common metrics include:
	• Request count
	• Backend health
	• Latency
	• HTTP status codes
	• Cache hit ratio
	• WAF events

15. What happens if all origins become unhealthy?
Answer
If every origin in an Origin Group fails health probes, Azure Front Door cannot successfully route requests to a healthy backend. Clients may receive error responses (such as HTTP 503/502 depending on the failure scenario). To minimize this risk, deploy applications across multiple regions and ensure health probes accurately reflect application readiness.

16. What are Route, Rule Set, and Rule Engine in Azure Front Door?
Answer
	• Route: Maps incoming requests (domain, path, protocol) to an Origin Group.
	• Rule Set (Rule Engine): Applies conditions and actions such as URL rewrite, redirects, header modification, cache behavior, or origin override.
	• Origin Group: Decides which healthy backend receives the request.

17. What security features does Azure Front Door provide besides WAF?
Answer
	• HTTPS enforcement
	• TLS/SSL certificate management
	• Custom domains
	• Managed certificates
	• Bot protection (through WAF)
	• Rate limiting
	• Geo-filtering
	• IP restrictions (via WAF rules)
	• DDoS resilience through Microsoft's global edge infrastructure

18. What are common troubleshooting steps when traffic is not reaching the backend?
Answer
Check the following:
	1. Origin health status.
	2. Health probe endpoint (/health) returns HTTP 200.
	3. DNS correctly points to the Front Door endpoint.
	4. Route configuration matches the request path.
	5. WAF rules are not blocking legitimate traffic.
	6. Backend firewall or NSG allows Front Door traffic.
	7. TLS/SSL configuration between Front Door and the origin is valid.
	8. Review Azure Monitor, Front Door diagnostic logs, and WAF logs.

Interview Summary (One-Liners)
Question	Short Interview Answer
Why Azure Front Door?	Global Layer-7 load balancing with edge routing and WAF.
Global or Regional?	Global service.
Protocols?	HTTP and HTTPS only.
Layer?	Layer 7 (Application Layer).
Health Check?	HTTP/HTTPS health probes monitor origin availability.
Failover?	Automatic to healthy origins.
WAF?	Integrated at the Microsoft edge.
SSL?	Supports SSL termination and end-to-end HTTPS.
CDN?	Yes, built-in edge caching for static content.
Terraform?	Fully supported through AzureRM Front Door resources.
Azure DevOps?	Deploy using CI/CD with terraform init, validate, plan, apply, approvals, and remote state.
Best Use Case?	Global applications requiring low latency, high availability, and secure traffic routing.
These questions cover the topics most commonly discussed in Azure DevOps interviews for professionals with 5–10 years of experience, including architecture, routing, security, Terraform automation, troubleshooting, and real-world deployment scenarios.

