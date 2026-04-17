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


## Configuration Steps

### 1. Windows Server Setup
- Downloaded the Windows Server 2022 Evaluation ISO from Microsoft (180-day trial)
- Created a VM in Proxmox with 4 cores, 4GB RAM, and 50GB storage
![VM Configuration Confirmation](screenshots/VM-Configuration.png)
![Server Setup](screenshots/VM-Installation.png)
- Assigned a static IP of 10.10.10.10/24 with gateway 10.10.10.1
![VM Static IP Assignment](screenshots/VM-static-IP-assignment.png)
- Renamed the server hostname to DC01
![Hostname Change](screenshots/VM-hostname-confirmation.png)

### 2. Active Directory Domain Services
- Installed AD DS and management tools via PowerShell using `Install-WindowsFeature`
![AD DS Installation Success](screenshots/AD-DS-Installation-Success.png)
- Promoted the server to a Domain Controller using `Install-ADDSForest`
- Created the domain: dogwood.local
![Promotion of server](screenshots/signout-after-domain-controller-promotion.png)
![Domain Login](screenshots/Domain-Login.png)

### 3. Organizational Units
- Created a parent OU called "DogWood Users" under dogwood.local
- Created five department OUs inside it: IT, HR, Finance, Marketing, Management
- All OUs created via PowerShell using `New-ADOrganizationalUnit`

![OU Commands](screenshots/OU-Commands-Creation.png)
![OU Structure](screenshots/OU-Structure.png)

### 4. Users and Security Groups
- Created 5 security groups matching each department
![Security Groups Creation](screenshots/ADGroup-Creations.png)
- Created 10 users across the five departments (2 per department)
- Assigned users to their respective department groups using `Add-ADGroupMember`
![Creating New Users and Assigning them](screenshots/New-Users-And-Assigning-Them.png)
![IT Group Members](screenshots/IT-Group-Members.png)


### 5. Group Policy
- **Password Policy** (PowerShell): Minimum 8 characters, complexity enabled, account lockout after 5 failed attempts for 30 minutes
![Password Policy](screenshots/Password-Policy.png)
- **Screen Lock Policy** (GPMC, domain-wide): Locks screen after 5 minutes of inactivity. Applied at dogwood.local level so it affects all machines
![Screen Lock Policy](screenshots/Screen-Lock-Policy.png)
- **Restrict Control Panel** (GPMC, OU-level): Blocks Control Panel access for all users in DogWood Users OU. IT Department group is exempted using Deny on Apply Group Policy
![Restict Control Panel except IT Department](screenshots/IT-Excemption-Restrict-Control_Panel.png)
![GPO Overview](screenshots/GPO-Overview.png)

### 6. Remote Desktop Access
- Enabled RDP on Windows Server via PowerShell
- Added a network route on the laptop to reach the 10.10.10.0/24 VM network through Proxmox (10.0.0.71)
![Network Route for RDP](screenshots/Network-Route-RDP.png)
- Connected using `mstsc` to 10.10.10.10 as DOGWOOD\Administrator
![RDP Connection](screenshots/RDP-Connection.png)