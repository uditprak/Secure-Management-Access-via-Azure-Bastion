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

Azure Bastion Features
1. Bastion Shareable Links ("Guest Access")
Concept: Allows users to connect to a specific VM without having access to the Azure Portal.

How it works: An administrator generates a unique URL for a target VM.

Security Use Case: You can provide temporary access to external vendors or contractors. They only need the link and the VM credentials—no Azure account or IAM permissions required.

Note: This is an exclusive feature of the Standard Tier.

2. Centralized Bastion via VNet Peering
Architecture: Instead of deploying a Bastion host in every Virtual Network (which is expensive), you deploy a single Bastion in a Hub VNet.

Functionality: By using VNet Peering, this single Bastion can reach and manage VMs located in multiple Spoke VNets.

Benefit: Significant cost savings and centralized management of all administrative traffic.

3. Session Monitoring & Management ("The Admin CCTV")
Monitoring: Provides real-time visibility into all active RDP and SSH sessions currently running through the Bastion host.

Administrative Control: Admins can view session start times, user info, and IP addresses.

Security Action: You have the power to disconnect (kill) any active session immediately if suspicious or unauthorized activity is detected.

4. Advanced File Transfer
Limitation (Basic Tier): Only supports simple text copy-pasting via the clipboard.

Capability (Standard Tier): Enables full File Upload and Download functionality through the web-based portal.

Use Case: Essential for transferring scripts, patches, or .exe installers directly into the VM from your local machine.
