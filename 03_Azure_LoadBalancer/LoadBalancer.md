What is Azure Load Balancer?
Definition
Azure Load Balancer is a Layer 4 (Transport Layer) load balancing service that distributes TCP and UDP traffic across multiple backend resources to provide:
	• High Availability
	• Fault Tolerance
	• Scalability
	• Low Latency
It makes sure that no single server receives all client requests.
	Interview Answer (30 seconds):
	"Azure Load Balancer is a Layer 4 load balancing service that distributes incoming TCP and UDP traffic across healthy backend resources such as Virtual Machines, Virtual Machine Scale Sets, or IP addresses. It improves availability by routing traffic only to healthy instances using health probes."

Why Do We Need Azure Load Balancer?
Without Load Balancer
                 Internet
                     │
                     ▼
                 Web Server
                     │
                     ▼
              Single Point of Failure
Problems
	• Server overload
	• Downtime
	• Poor scalability
	• No redundancy

With Load Balancer
                    Internet
                        │
                        ▼
              Azure Load Balancer
                │      │      │
                ▼      ▼      ▼
              VM1     VM2     VM3
Benefits
	• Traffic distribution
	• Automatic failover
	• Horizontal scaling
	• High availability

OSI Layer
One of the most commonly asked interview questions.
Service	OSI Layer	Protocol
Azure Load Balancer	Layer 4	TCP / UDP
Azure Application Gateway	Layer 7	HTTP / HTTPS
Azure Front Door	Layer 7	Global HTTP / HTTPS

How Azure Load Balancer Works Internally
              Client Request
                     │
                     ▼
              Public IP Address
                     │
                     ▼
          Frontend IP Configuration
                     │
                     ▼
            Load Balancing Rule
                     │
                     ▼
          Backend Address Pool
                     │
                     ▼
        Health Probe Validation
                     │
        ┌────────────┴────────────┐
        ▼                         ▼
     Healthy VM               Unhealthy VM
        │                           │
        ▼                           ▼
 Receives Traffic             No Traffic

Components Explained
1. Frontend IP Configuration
The entry point of the Load Balancer.
It accepts client traffic.
Types
	• Public Frontend IP
	• Private Frontend IP
Example
Internet
↓

20.45.xx.xx
↓

Frontend IP

2. Backend Address Pool
Contains backend resources.
Supported resources
	• Virtual Machines
	• Virtual Machine Scale Sets
	• IP addresses
	• Availability Sets
Example
Backend Pool
VM1
VM2
VM3

3. Health Probe
The Health Probe continuously checks whether backend instances are healthy.
Example
LB
│

├── VM1 ✓
├── VM2 ✓
└── VM3 ✗
If VM3 fails,
Traffic
VM1 ✓
VM2 ✓
VM3 ✗
↓

No traffic to VM3
Health Probe Protocols
	• TCP
	• HTTP
	• HTTPS
Interview Tip
	Health Probe never restarts a VM. It only decides whether that VM should receive traffic.

4. Load Balancing Rule
Defines
Frontend IP
↓
Frontend Port
↓
Backend Pool
↓
Backend Port
Example
Public IP
80
↓

Backend Pool
↓

VM Port 80

5. Inbound NAT Rule
Unlike Load Balancing Rules,
Inbound NAT Rule forwards traffic to one specific VM.
Example
52.xx.xx.xx
33891
↓

VM1
3389
52.xx.xx.xx
33892
↓

VM2
3389
Used for
	• SSH
	• RDP
	• Administrative access

Request Flow (Step by Step)
User opens website
↓

DNS resolves Public IP
↓

Azure Load Balancer
↓

Health Probe checks backend
↓

Healthy VM selected
↓

Traffic forwarded
↓

VM responds
↓

Response reaches user

Load Distribution Algorithm
Azure Load Balancer does not use Round Robin.
It uses a hashing algorithm.
Default
5-Tuple Hash
Uses
	• Source IP
	• Source Port
	• Destination IP
	• Destination Port
	• Protocol
Benefits
	• Even distribution
	• Connection persistence
	• Better performance
Other options
	• 2-Tuple Hash
	• 3-Tuple Hash

Types of Azure Load Balancer
Public Load Balancer
Internet
↓

Public LB
↓

Web Servers
Use Cases
	• Websites
	• APIs
	• Internet-facing applications

Internal Load Balancer
Application Tier
↓

Internal LB
↓

Database Servers
Use Cases
	• Database tier
	• Internal applications
	• Microservices
	• Private APIs

Azure Load Balancer SKU
Interviewers frequently ask this.
Basic SKU	Standard SKU
Legacy	Recommended
Small workloads	Production workloads
Limited features	Zone redundant support
Limited scalability	Highly scalable
Closed by default: No	Closed by default: Yes (NSG required)
No Availability Zones	Supports Availability Zones
Interview Tip: Always use Standard Load Balancer for production deployments because Basic SKU is being retired and lacks several enterprise features.

Session Persistence
Also called
Source IP Affinity
Options
	• None
	• Client IP (2-Tuple)
	• Client IP + Protocol (3-Tuple)
Useful when
	• Shopping carts
	• Banking sessions
	• Stateful applications

Health Probe Failure Scenario
          LB
/ | \
VM1 VM2 VM3
✓  ✗  ✓
Traffic
VM1 ← Requests

VM2 ← Blocked

VM3 ← Requests

When VM2 recovers
Health Probe ✓
↓

Traffic resumes automatically

Real-Time Azure DevOps Scenario
Suppose your application is deployed using an Azure DevOps pipeline.
Azure DevOps Pipeline
↓

Terraform
↓

Virtual Machine Scale Set
↓

Azure Load Balancer
↓

Users
Deployment Flow
	1. Azure DevOps pipeline deploys a new application version.
	2. New VM instances are added to the backend pool.
	3. Health probes verify the new instances.
	4. Only healthy instances begin receiving traffic.
	5. Existing user traffic continues without interruption.
This enables rolling updates and zero-downtime deployments.

Azure Load Balancer vs Application Gateway
Azure Load Balancer	Application Gateway
Layer 4	Layer 7
TCP/UDP	HTTP/HTTPS
No SSL termination	SSL termination supported
No URL routing	URL/path-based routing
No Web Application Firewall	WAF available
Best for network traffic	Best for web applications

Azure Load Balancer vs Azure Front Door
Azure Load Balancer	Azure Front Door
Regional	Global
Layer 4	Layer 7
TCP/UDP	HTTP/HTTPS
Single-region traffic	Multi-region traffic
Internal or public	Global entry point with CDN acceleration

Limitations
	• Layer 4 only (no HTTP header inspection)
	• No SSL/TLS termination
	• No URL/path-based routing
	• No cookie-based session affinity
	• Cannot act as a Web Application Firewall
	• Regional service (not global)

Common Azure DevOps Interview Questions
Q1. Why is Azure Load Balancer a Layer 4 service?
Answer: Because it makes routing decisions using TCP/UDP information (IP addresses, ports, and protocol) rather than HTTP content, URLs, or headers.

Q2. Does Azure Load Balancer terminate SSL?
Answer: No. SSL/TLS termination is handled by Azure Application Gateway, Azure Front Door, or the application itself.

Q3. What happens when a backend VM fails?
Answer: Health probes detect the failure, mark the VM as unhealthy, and stop sending new traffic to it. Once the VM passes the health checks again, it automatically re-enters the backend pool.

Q4. Can Azure Load Balancer distribute HTTP traffic?
Answer: It forwards HTTP traffic as TCP packets but cannot inspect HTTP headers or perform Layer 7 routing. For URL-based routing or SSL termination, use Azure Application Gateway.

Q5. Which Azure service should you use for a public website requiring SSL offloading and Web Application Firewall?
Answer: Azure Application Gateway (or Azure Front Door for global scenarios).

Q6. Can Azure Load Balancer work with Virtual Machine Scale Sets?
Answer: Yes. It's commonly used with VM Scale Sets to automatically distribute traffic across dynamically scaling VM instances.

Q7. Does Azure Load Balancer support cross-zone load balancing?
Answer: Yes. Standard Load Balancer supports Availability Zones and Zone-Redundant deployments for higher resiliency.

Quick Revision
Azure Load Balancer
↓

Layer 4
↓

TCP / UDP
↓

Frontend IP
↓

Load Balancing Rule
↓

Backend Pool
↓

Health Probe
↓

Healthy VM
↓

Response
Interview Memory Trick
F → B → H → R → N
	• F = Frontend IP (Entry point)
	• B = Backend Pool (Servers)
	• H = Health Probe (Health checks)
	• R = Load Balancing Rule (Traffic mapping)
	• N = NAT Rule (Direct access to a specific VM)
