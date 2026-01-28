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
