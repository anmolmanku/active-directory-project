# Active Directory Lab: DogWood Solutions

This project simulates the IT infrastructure for a small company called DogWood Solutions. A Windows Server 2022 Domain Controller runs Active Directory Domain Services, DNS, and Group Policy, managing employee accounts, security groups, and company-wide policies across five departments.

Built on a Proxmox VE 9.1 home lab environment.

## What This Lab Demonstrates

- Deploying Windows Server 2022 as a virtual machine on Proxmox
- Configuring static IP and DNS settings on the server
- Installing Active Directory Domain Services and DNS
- Promoting the server to a Domain Controller
- Creating Organizational Units to reflect a company department structure
- Creating and managing users and security groups
- Configuring Group Policies (password policy, screen lock, Control Panel restriction)
- Setting up GPO inheritance and security filtering to exempt specific groups
- Enabling and connecting via Remote Desktop Protocol (RDP)

## Technologies Used

- Windows Server 2022 (Evaluation)
- Active Directory Domain Services (AD DS)
- DNS Server
- Group Policy Management (GPMC)
- Remote Desktop Protocol (RDP)
- PowerShell
- Proxmox VE 9.1

## Network Diagram

![Network Diagram](screenshots/Network-Diagram-Proxmox.jpeg)

| Machine | Hostname | IP Address | Role |
|---------|----------|------------|------|
| Proxmox Host | hydrogen | 10.0.0.71 | Hypervisor, NAT Gateway |
| Windows Server VM | DC01 | 10.10.10.10 | Domain Controller, DNS, AD DS |

- Domain: dogwood.local
- Internal VM Network: 10.10.10.0/24 (NAT through Proxmox Wi-Fi)
- External Network: 10.0.0.0/24 (Home Wi-Fi)

## Configuration Steps

### 1. Windows Server Setup
To begin with, downloaded the ISO file for Windows Server from Microsoft. It comes in a trial
Imported it into promox hypervisor, and created a VM of it with 4 cores, 50gb storage, and 4gb ram
After it was done,  ggve it a static IP of 10.10.10.10 inside 10.10.10.0/24 network, and change the hostname to DC01

### 2. Active Directory Domain Services
Afterwards, open powershell as admin, (RDP can be configured here and access through laptop), and insall AD Services and management tools
Promote it to Domain Controller, and the domain is dogwood.local which was created after promoting it


### 3. Organizational Units
Inside domain, dogwood.local, created OU - DogWood Users, and inside DogWood Users, IT, Marketing, Management, HR, Finance were created. All done through powershell

### 4. Users and Security Groups
a total of 10 users were created and user names and SAM names were given to them
5 security groups were created - IT department, Marketing Department, Management Team, HR Department, Finance Department
And each having 2 users

### 5. Group Policy
First GP created through powershell was to configure 8 length password with maximum 5 trials and complexity on with 30 minutes locking period
Second GPO was to Screen Lock after 5 minutes of inactivity. It was done through GUI of Active Directory, and it was domainwide. 
Third GPO Was to restrict control panel access for all groups except for IT. It was done through GUI of AD, and it was on DogWood Users with the exemption of IT 

### 6. Remote Desktop Access
RDP was configured on the Windows Server through powershell
And on the laptop, routing was done on the VM network of 10.10.10.0/255 to be accessed by 10.0.0.71 (which is Proxmox IP)
And then using mstsc, and adding the IP of DC01, 10.10.10.10 and logging in as Administrator with the login password, you can login into the RDP