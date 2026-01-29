# Secure VM Access via Azure Bastion (AZ-500 Lab)

## 📌 Project Overview
This lab demonstrates how to secure administrative access to an Azure Virtual Machine. By using **Azure Bastion**, I eliminated the need for a Public IP address and closed all inbound RDP ports (3389) to the internet, significantly reducing the attack surface.

## 🛠️ Infrastructure Details
- **Virtual Network:** VNet-Main (10.0.0.0/16)
- **Subnets:** Web-Subnet (10.0.1.0/24) & AzureBastionSubnet (10.0.2.0/24)
- **Virtual Machine:** Windows Server 2022 (No Public IP)
- **Security:** RDP over HTTPS via Bastion.

## 🚀 Key Learning
- Configuration of Hub-and-Spoke ready VNet.
- Deploying Azure Bastion on a dedicated subnet.
- Understanding why Public IPs are a security risk for management ports.

## 🚀 Advanced Azure Bastion Concepts

After successfully implementing the basic deployment, I explored enterprise-level features of Azure Bastion (Standard Tier) that enhance security and administrative control.

### 1. Secure Access & Connectivity Features

| Feature | Description | Business Value |
| :--- | :--- | :--- |
| **Shareable Links** | Provides a unique URL to access a VM without requiring the Azure Portal. | Secure temporary access for external vendors/contractors. |
| **File Transfer** | Enables browser-based file upload and download functionality. | Allows patching and script deployment without a Public IP. |
| **Native Client** | Allows using local tools like `mstsc` (RDP) or Terminal (SSH). | Improved performance and familiar UI for power users. |

### 2. Enterprise Architecture: Centralized Management
In a production environment, deploying a Bastion in every VNet is not cost-effective.
* **Hub-and-Spoke Model:** A single Bastion host is deployed in a central "Hub" VNet.
* **VNet Peering:** Using peering, this central Bastion can manage VMs in all connected "Spoke" VNets.
* **Benefit:** Reduces costs and provides a single, hardened entry point for all administrative traffic.



### 3. Monitoring & Governance (The "Admin CCTV")
Azure Bastion provides full visibility into remote sessions to maintain a high-security posture:
* **Real-time Monitoring:** View active RDP/SSH sessions, including user details and session start times.
* **Session Termination:** Administrators can manually disconnect or "kill" any active session if unauthorized activity is suspected.
* **Audit Logs:** Integration with Azure Monitor to track who accessed which VM and when.



---

### 🛡️ Comparison: Basic vs. Standard Tier

| Capability | Basic Tier | Standard Tier |
| :--- | :--- | :--- |
| **RDP/SSH Connectivity** | ✅ Supported | ✅ Supported |
| **Max Instances** | 2 Fixed | Up to 50 (Scaling) |
| **File Transfer** | ❌ No | ✅ Yes |
| **Shareable Links** | ❌ No | ✅ Yes |
| **Session Monitoring** | ✅ Supported | ✅ Supported |
| **IP-Based Connection** | ❌ No | ✅ Yes |

---

## 🏗️ Architecture Diagram

This diagram illustrates the secure flow of traffic through Azure Bastion:

```text
+-----------------------------------------------------------------------+
|                         Azure Cloud (Region)                          |
|                                                                       |
|    +-------------------------------------------------------------+    |
|    |                      Virtual Network (VNet)                 |    |
|    |                                                             |    |
|    |  +------------------------+        +-----------------------+ |    |
|    |  |  AzureBastionSubnet    |        |    Workload Subnet    | |    |
|    |  |                        |        |                       | |    |
|    |  |   [ Azure Bastion ] <-----------|-----> [ Target VM ]   | |    |
|    |  |      (Service)         |  RDP   |      (No Pub IP)      | |    |
|    |  +-----------^------------+  3389  +-----------^-----------+ |    |
|    |              |                                 |             |    |
|    +--------------|---------------------------------|-------------+    |
|                   |                                 |                  |
|           HTTPS (Port 443)                  Traffic Blocked            |
|                   |                         (NSG Inbound)              |
|                   |                                 X                  |
+-------------------|---------------------------------|------------------+
                    |                                 |
             [ Administrator ]                 [ Public Internet ]
              (Secure Access)                   (Potential Attack)

```
📝 Traffic Flow Logic:
Request: Administrator connects to the Azure Portal via SSL (Port 443).

Authentication: After Entra ID authentication, the Bastion service initiates a session.

Internal Connection: Bastion connects to the Target VM using its Private IP over Port 3389.

Security: The Target VM has No Public IP, and its Network Security Group (NSG) is configured to deny all inbound traffic from the internet.
