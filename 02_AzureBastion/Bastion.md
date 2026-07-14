# Azure Bastion – Complete Interview Guide

---

# What is Azure Bastion?

Azure Bastion is a fully managed **Platform as a Service (PaaS)** that provides secure **RDP (Windows)** and **SSH (Linux)** access to Azure Virtual Machines **without exposing them to the internet using public IP addresses**.

You connect through the **Azure Portal** or supported **native RDP/SSH clients** over **TLS (HTTPS/443)**.

---

# Why Do We Need Azure Bastion?

## Without Azure Bastion

- VM requires a **Public IP**
- NSG must allow:
  - **RDP (3389)** for Windows
  - **SSH (22)** for Linux
- VM becomes vulnerable to:
  - Port scanning
  - Brute-force attacks
  - Unauthorized access

---

## With Azure Bastion

- ❌ No Public IP on VM
- ❌ No inbound ports **22** or **3389**
- ✅ Secure access through Azure Portal
- ✅ Traffic remains on Microsoft's network
- ✅ Better security and compliance

---

# Architecture

```text
                 Admin
                   │
                   │ HTTPS (443)
                   ▼
             Azure Portal
                   │
                   ▼
             Azure Bastion
        (AzureBastionSubnet)
                   │
               Private IP
                   ▼
               Azure VM
          (No Public IP)
```

## Connection Flow

1. User logs into Azure Portal.
2. Clicks **Connect → Bastion**.
3. Azure Bastion establishes a secure RDP/SSH session.
4. Connection reaches the VM using its **private IP**.

---

# Key Features

- Secure browser-based RDP/SSH
- No Public IP required
- No VPN required
- No Jump Server required
- HTML5 browser access
- Native RDP/SSH client support (Standard/Premium)
- File transfer
- Copy/Paste support
- Session recording (Premium)
- Supports Windows and Linux VMs
- Supports VNet Peering

---

# Azure Bastion Requirements

Before deployment, ensure the following:

- Virtual Network (VNet)
- Dedicated subnet named:

```text
AzureBastionSubnet
```

- Minimum subnet size:

```text
/26
```

- Public IP (except certain Premium private-only scenarios)
- Azure Bastion Host resource

---

# Azure Bastion SKUs

| SKU | Best For | Features |
|------|----------|----------|
| Developer | Learning & Testing | Free, limited capabilities |
| Basic | Small Production | Browser-based RDP/SSH |
| Standard | Enterprise | Native client, file transfer, scaling |
| Premium | Highly Regulated Environments | Session recording, private-only deployment |

---

# Azure Bastion vs Jump Server

| Azure Bastion | Jump Server |
|---------------|-------------|
| Managed PaaS | Self-managed VM |
| No maintenance | Requires patching |
| Highly secure | Security depends on configuration |
| Auto scaling | Manual scaling |
| No Public IP on VMs | Usually requires public endpoint |
| Lower operational overhead | Higher operational overhead |

---

# Azure Bastion vs VPN Gateway

| Azure Bastion | VPN Gateway |
|---------------|-------------|
| Access individual VMs | Connect entire network |
| RDP/SSH only | Full network connectivity |
| No VPN client required | VPN client required |
| Administrative access | Site-to-site or Point-to-Site connectivity |

---

# Advantages

- Improves VM security
- Removes public exposure
- Reduces attack surface
- Easy deployment
- Centralized administration
- Supports VNet Peering
- Simplifies compliance and auditing

---

# Limitations

- Additional cost
- Dedicated subnet required
- Not suitable for high-volume data transfer
- Advanced features depend on the selected SKU

---

# Real-World Scenario

A company hosts:

- 20 Windows Servers
- 10 Linux Servers

Instead of assigning **30 Public IPs** and opening ports **3389** and **22**, they:

- Deploy one Azure Bastion Host
- Remove Public IPs from all VMs
- Connect securely through Azure Portal

**Result**

- Greatly reduced attack surface
- Easier administration
- Better security compliance

---

# Interview Questions & Answers

## 1. What is Azure Bastion?

Azure Bastion is a managed PaaS service that provides secure RDP and SSH access to Azure VMs without requiring Public IP addresses.

---

## 2. Why use Azure Bastion instead of a Public IP?

Because it:

- Removes internet exposure
- Eliminates open RDP/SSH ports
- Reduces the risk of cyberattacks

---

## 3. Which ports are avoided by using Bastion?

- SSH → **22**
- RDP → **3389**

---

## 4. Which subnet is mandatory?

```text
AzureBastionSubnet
```

---

## 5. Can Azure Bastion connect to Linux VMs?

Yes.

It connects using **SSH**.

---

## 6. Can Azure Bastion connect to Windows VMs?

Yes.

It connects using **RDP**.

---

## 7. Is a VPN required?

No.

---

## 8. Does the VM need a Public IP?

No.

---

## 9. What subnet size is required for Bastion scaling?

To support scaling, the **AzureBastionSubnet** must be **/26 or larger**.

Smaller subnets limit the number of Bastion instances.

---

## 10. Why does subnet size matter?

Each Bastion instance consumes IP addresses from the subnet.

A **/26 subnet** provides **64 IP addresses**, allowing enough space for multiple Bastion instances and internal operations.

---

## 11. How many concurrent sessions can one Bastion instance handle?

- **20 RDP sessions** (Windows)
- **40 SSH sessions** (Linux)

---

## 12. What happens if additional sessions are required?

You can **scale out** Azure Bastion by adding more instances.

Scaling is supported only in:

- Standard SKU
- Premium SKU

---

## 13. Does scaling disrupt active connections?

Yes.

Adding or removing Bastion instances interrupts active sessions.

Scaling should be performed during maintenance windows.

---

## 14. Which SKU supports scaling?

- **Developer** → ❌ No
- **Basic** → ❌ No
- **Standard** → ✅ Supports **2–50 instances**
- **Premium** → ✅ Supports **2–50 instances** plus:
  - Session recording
  - Private-only deployment

---

# Quick Interview Revision

| Question | Answer |
|----------|--------|
| Service Type | PaaS |
| Purpose | Secure RDP/SSH to Azure VMs |
| Public IP Required | No |
| VPN Required | No |
| Jump Server Required | No |
| Browser Access | Yes |
| Native Client Support | Standard & Premium |
| Mandatory Subnet | AzureBastionSubnet |
| Minimum Subnet Size | /26 |
| Windows Protocol | RDP |
| Linux Protocol | SSH |
| Secure Port | HTTPS (443) |
| VM Private IP Used | Yes |
| Scaling Supported | Standard & Premium |
| Maximum Scaling | 2–50 instances |
| Session Recording | Premium |
| File Transfer | Standard & Premium |
| Copy/Paste | Yes |
| Supports VNet Peering | Yes |

---

# One-Line Interview Summary

> **Azure Bastion is a fully managed Azure PaaS service that provides secure browser-based and native RDP/SSH connectivity to Azure Virtual Machines over HTTPS without requiring Public IP addresses, VPNs, or Jump Servers, thereby reducing the attack surface and improving security.**