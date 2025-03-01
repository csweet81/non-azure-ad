<p align="center">
<img src="https://i.imgur.com/pJSsvpx.png" alt=""/>
</p>

<h1>Installing & Configuring Non-Azure Active Directory Infrastructure via Remote Desktop</h1>
<p>
In this lab we'll setup up an Active Directory environment on Mac. By deploying a Windows Server as a Domain Controller (DC-1) and a Windows 10 machine as a client (Client-1), we’ll explore how to configure private networks, assign static IPs, and test connectivity between virtual machines.

Through this step-by-step guide, you’ll gain practical experience in:

- Establishing a domain environment in Azure.
- Configuring DNS settings and testing network connectivity.
- Troubleshooting and ensuring seamless communication between virtual machines.

<h2>Environments and Technologies Used</h2>

- Microsoft Active Directory
- VirtualBox

<h2>Operating Systems Used</h2>

- Windows Server 2022</b>

<h2>List of Steps</h2>

- Part I: Setup Domain Controller
  - Step 1: Create virtual machines
  - 
- Part II: Setup Client-1 
  - Step 1: 


<h2>Installation Steps</h2>
<h3>Part I: Setup Domain Controller</h3>

<h4>Step 1: Create a Resource Group</h4>

<img src="https://i.imgur.com/kfSXdsg.png" height="80%" width="80%" alt=""/>

- Log into the Azure Portal.
- Navigate to Resource Groups in the left-hand menu.
- Click Create.
- Enter the following details:
  - Resource Group Name: (e.g., "LabResourceGroup").
  - Region: Select your preferred Azure region (e.g., "East US").
- Click Review + Create and then Create.

<h4>Step 2: Create a Virtual Network and Subnet</h4>

<img src="https://i.imgur.com/dc07sEq.png" height="80%" width="80%" alt=""/>

- Navigate to Virtual Networks in the left-hand menu.
- Click Create.
- Enter the following details:
  - Name: (e.g., "LabVNet").
  - Region: Select the same region as the resource group.
  - Under IP Addresses, configure the address space and subnet:
  - Address Space: (e.g., "10.0.0.0/16").
  - Subnet Name: (e.g., "LabSubnet").
  - Subnet Address Range: (e.g., "10.0.0.0/24").
- Click Review + Create and then Create.

<h4>Step 3: Create the Domain Controller VM</h4>

<img src="https://i.imgur.com/oiQGFP9.png" height="80%" width="80%" alt=""/>

- Navigate: In the Azure Portal, search for Virtual Machines in the top search bar and select it.
- Create VM:
  - Click + Create and choose Azure Virtual Machine.
  - Under Basics, fill out the details:
    - Select the previously created Resource Group.
    - Give the VM a name (e.g., "TestVM").
    - Choose an image: Windows 10 Pro or Enterprise.
    - Select a region in a different country (e.g., Europe or Asia).
    - Choose a size (e.g., Standard D2s_v3 for light use).
    - Set admin username and password for login.
- Click Review + Create, then Create.
- Wait for Deployment: Once deployment completes, navigate to the VM's Overview page.

<h4>Step 4: Create the Client-1 VM</h4>

<img src="https://i.imgur.com/lV0YqAr.png" height="80%" width="80%" alt=""/>

- Navigate to Virtual Machines in the Azure Portal.
- Click Create and choose Azure Virtual Machine.
- Enter the following details:
  - VM Name: Client-1
  - Region: Same as the Domain Controller (e.g., East US)
  - Image: Windows 10 Pro.
  - Size: Standard B2s (or equivalent).
  - Username: labuser
  - Password: Cyberlab123!
  - Virtual Network: Select the same network as DC-1 (LabVNet)
  - Subnet: Select the same subnet as DC-1 (LabSubnet)
- Click Review + Create and then Create to deploy Client

<h4>Step 5: Set DC-1’s NIC Private IP Address to Static</h4>

<img src="https://i.imgur.com/JrkGYMM.png" height="80%" width="80%" alt=""/>

- Once the VM is created, navigate to Virtual Machines and select DC-1.
- Under Settings, click Networking and select the NIC attached to the VM.
- Under IP Configurations, click the configuration and set the private IP to Static.
- Save the changes.

<h4>Step 5: Log Into the VM and Disable the Windows Firewall</h4>

- Navigate to Virtual Machines and select DC-1.
- Click Connect, select RDP, and download the RDP file.
- Use the RDP file to log into the VM using:
  - Username: labuser.
  - Password: Cyberlab123!
- Open the Windows Firewall Settings:
  - Navigate to Control Panel > System and Security > Windows Defender Firewall.
  - Alternately, you can type 'wf.msc' in the Start dock search bar.  
  - Click Turn Windows Defender Firewall on or off.
  - Disable the firewall for all profiles (Domain, Private, Public).
- Save the changes.

<h3>Part II: Setup Client-1 in Azure</h3>

<h4>Step 1: Create the Client VM</h4>

<img src="https://i.imgur.com/dc07sEq.png" height="80%" width="80%" alt=""/>

- Navigate to Virtual Machines in the left-hand menu.
- Click Create and select Azure Virtual Machine.
- Enter the following details:
  - VM Name: Client-1.
  - Region: Same as DC-1.
  - Image: Windows 10 Pro.
  - Size: Choose an appropriate size (e.g., Standard B2s).
  - Username: labuser.
  - Password: Cyberlab123!
  - Virtual Network: LabVNet.
  - Subnet: LabSubnet.
- Click Review + Create and then Create.

<h3>Step 2: Set Client-1’s DNS Settings to DC-1’s Private IP</h3>

<img src="https://i.imgur.com/MlCUQ6t.png" height="80%" width="80%" alt=""/>

- Once the VM is created, navigate to Virtual Machines and select Client-1.
- Under Settings, click Networking.
- Select the NIC attached to Client-1 and go to DNS Servers.
- Set the DNS server to Custom and input DC-1’s Private IP Address.
- Save the changes.

<h3>Step 3: Restart Client-1</h3>

<img src="https://i.imgur.com/FRe2Vj0.png" height="80%" width="80%" alt=""/>

- From the Azure Portal, select Client-1.
- Click Restart to apply the DNS changes.

<h3>Step 4: Login to Client-1</h3>

<img src="https://i.imgur.com/cdemX9a.png" height="80%" width="80%" alt=""/>

- Connect to Client-1 using RDP (same method as DC-1).
- Login with:
  - Username: labuser.
  - Password: Cyberlab123!

<h3>Step 5: Test Connectivity</h3>
<img src="https://i.imgur.com/4HdBb4U.png" height="80%" width="80%" alt=""/>

- Open a Command Prompt or PowerShell on Client-1.
- Run the following command:
  - ping <DC-1 Private IP>
- Ensure you see replies from DC-1's private IP.

<h3>Step 6: Verify DNS Settings</h3>
<img src="https://i.imgur.com/ROvkxrC.png" height="80%" width="80%" alt=""/>

- From Client-1, open PowerShell.
- Run the following command:
  - ipconfig /all
- Verify that the DNS Server is set to DC-1’s private IP.

