
# CIDR (Classless Inter-Domain Routing) – Complete Interview Notes (Azure DevOps 5–10 Years)

> This section is intended to be appended to your existing CIDR notes.

# CIDR Interview Questions & Answers

## Q1. What is CIDR?
**Answer:** CIDR (Classless Inter-Domain Routing) replaces class-based addressing and uses a variable-length prefix (for example, `/24`, `/16`) for efficient IP allocation, subnetting, and route aggregation.

## Q2. Why was CIDR introduced?
**Answer:**
- Eliminates IP wastage from classful networking.
- Supports Variable Length Subnet Masking (VLSM).
- Enables route aggregation (supernetting).
- Improves routing efficiency and scalability.

## Q3. What does /24 mean?
- 24 bits = Network
- 8 bits = Host
- Host bits = 32 − 24 = 8
- Total IPs = 2^8 = 256
- Traditional usable = 254
- Azure usable = 251

## Q4. Formula for IP calculation
```
Host bits = 32 − Prefix
Total IPs = 2^(Host bits)
Traditional usable = Total − 2
Azure usable = Total − 5
```

## Q5. Why are two IPs reserved in traditional networking?
The first IP is the Network ID and the last IP is the Broadcast address.

## Q6. Why does Azure reserve five IP addresses?
Azure reserves:
1. Network address
2. Default gateway
3. Azure DNS
4. Azure internal services
5. Last IP (reserved by Azure; Azure VNets do not use broadcast)

## Q7. How many usable IPs are available in Azure /24?
256 total − 5 reserved = **251 usable**.

## Q8. Difference between CIDR and subnet mask?
CIDR is slash notation (`/24`), while the subnet mask is dotted decimal (`255.255.255.0`).

## Q9. Calculate total IPs in /27.
- Host bits = 5
- Total = 32
- Traditional usable = 30
- Azure usable = 27

## Q10. Calculate total IPs in /20.
- Host bits = 12
- Total = 4096
- Traditional usable = 4094
- Azure usable = 4091

## Q11. Which subnet is larger: /16 or /24?
/16 (65,536 IPs) is much larger than /24 (256 IPs).

## Q12. Why must subnet sizes be powers of two?
Binary addressing requires subnet boundaries at powers of two.

## Q13. Can Azure VNets have overlapping CIDRs?
Yes, if isolated. **No** if they need VNet peering.

## Q14. Why can't overlapping VNets be peered?
Because Azure routing becomes ambiguous.

## Q15. Can two subnets overlap?
No.

## Q16. Can Azure subnets be resized?
Expansion may be possible if adjacent address space is available; shrinking isn't directly supported.

## Q17. Why plan CIDR carefully?
For future growth, VNet peering, VPN, ExpressRoute, AKS, Private Endpoints, and Hub-Spoke architecture.

## Q18. Recommended subnet for AKS?
Typically `/22` or larger depending on cluster growth.

## Q19. Recommended subnet for Azure Bastion?
Dedicated **AzureBastionSubnet**, typically `/26` or larger.

## Q20. Can Azure Firewall share a subnet?
No. It requires **AzureFirewallSubnet**.

## Q21. Public vs Private IP?
Private IPs are internal only; public IPs are Internet-routable.

## Q22. Name the RFC1918 private ranges.
- 10.0.0.0/8
- 172.16.0.0/12
- 192.168.0.0/16

## Q23. What is APIPA?
169.254.0.0/16 assigned automatically when DHCP fails.

## Q24. What is loopback?
127.0.0.1

## Q25. What is route aggregation?
Combining multiple routes into one larger route to reduce routing table size.

## Q26. What is VLSM?
Using different subnet sizes in the same network to reduce IP wastage.

## Q27. Would you choose /28 or /24 for a growing application?
Choose /24 (or larger) if significant growth is expected.

## Q28. A /24 subnet is full in Azure. What next?
Add another subnet, expand if possible, or redesign the VNet.

## Q29. VNet peering fails because of overlapping address spaces. Resolution?
Change one VNet to a non-overlapping CIDR, then recreate peering.

## Q30. How do you choose a CIDR block?
Consider:
- Current resources
- Future growth
- AKS
- VPN/ExpressRoute
- Hub-and-Spoke
- On-premises overlap

# Rapid Revision

|Question|Answer|
|---|---|
|IPv4 bits|32|
|IPv6 bits|128|
|Formula|2^(32-Prefix)|
|/24 total|256|
|Azure usable in /24|251|
|Azure reserved IPs|5|
|Loopback|127.0.0.1|
|APIPA|169.254.0.0/16|
|Private ranges|10/8, 172.16/12, 192.168/16|
|Can subnets overlap?|No|
|Can overlapping VNets be peered?|No|
|Purpose of CIDR|Efficient addressing & routing|
|VLSM|Variable Length Subnet Mask|
|Supernetting|Route aggregation|
