
**Windows Server**it is a Microsoft server operating system family 
Its primary function is to act as a reliable infrastructure platform that delivers services

---

###  windows server design concept
At its core, Windows Server works through:
client-server pattern 
role-based architecture 
A "role" is a major function the server is configured to perform — for example, being a file server,a domain controller.
A "feature" is an Additional capabilities that support roles

- **Predictability.** A 10-year support lifecycle with the **Long-Term Servicing Channel (LTSC)**: each version is supported for a full ten years in total . 
- **A single source of accountability.** When something breaks, there is one vendor — Microsoft — with a support contract, instead of stitching together support from multiple open-source communities.
- **Deep tooling for everyday operations** such as Group Policy , Windows Server Backup, and PowerShell scripting for automation.
- ### The Ten-Year Support Promise

Every LTSC release of Windows Server, including 2019, 2022, and 2025, is supported under Microsoft's **Fixed Lifecycle Policy** for a total of about ten years, split into two phases:

- **Mainstream support (about 5 years):** the server receives new features, non-security bug fixes, and security updates.
- **Extended support (about 5 more years):** only security updates continue; no new features and generally no free non-security bug fixes.

For Windows Server 2019 specifically: it was released on November 13, 2018, mainstream support ended on January 9, 2024, and extended support is scheduled to run until January 9, 2029.


---

##  Windows Server Versions
We start from Windows Server 2008 because it marks the transition from traditional server systems to the modern virtualization, automation, and cloud-ready architecture that still evolves today.

| Version | Released | Key Highlights |
|---|---|---|
| Windows Server 2008 / 2008 R2 | 2008 / 2009 | Introduced Hyper-V virtualization (Microsoft's built-in hypervisor) and PowerShell as a serious management tool. |
| Windows Server 2012 / 2012 R2 | 2012 / 2013 | Brought Storage Spaces (software-defined storage pools) and a major overhaul of Hyper-V and clustering. |
| Windows Server 2016 | 2016 | Added Windows Containers, Nano Server, Storage Spaces Direct, and Shielded VMs for stronger virtualization security. |
| Windows Server 2019 | November 13, 2018 | Focused on hybrid cloud (System Insights, Storage Migration Service), Linux containers on Windows, and improved Storage Spaces Direct. |
| Windows Server 2022 | August 2021 | Introduced Secured-core server, TLS 1.3 support, and tighter Azure Arc integration for hybrid management. |
| Windows Server 2025 | November 1, 2024 | Added hotpatching (installing security updates without a reboot, on supported editions), SMB over QUIC for Standard/Datacenter editions, a higher Active Directory functional level supporting larger 32K database page sizes for huge directories, and a strong push toward replacing the older NTLM authentication protocol with Kerberos. |

each version changes the internal engine and adds new ways to manage systems more securely, more automatically, and more efficiently.
### What's Actually Different
1. Same roles across versions
2. capabilities added in newer releases:
   Default security settings
   New protocols
   Automation features
   Cloud integration
   Performance improvements

---

### Windows Server 2019 ecosystem

### Core Architecture

- **Execution Modes:** Windows operates in two modes:
1/Kernel Mode: the Windows NT kernel, responsible for:
CPU / Memory management/ Hardware interaction/ Security enforcement it has Full access to system hardware and memory; used by core system components.
2/User Mode: Restricted environment where applications and services run safely. those services are managed by **System Services:** with the Service Control Manager (SCM), which starts, stops, and monitors background services.

### Networking Architecture

- **TCP/IP:** Core communication protocol suite
- **DNS (Domain Name System):** translating names into IP addresses
- **DHCP (Dynamic Host Configuration Protocol):** a service that automatically assigns IP addresses to devices joining the network
- **Subnetting:** dividing a large network into smaller and manageable segments
- **VLANs (Virtual Local Area Networks):** a way to logically separate traffic on the same physical network hardware, commonly used to keep guest Wi-Fi traffic isolated from internal company traffic

### Roles Architecture :

A) IDENTITY & SECURITY : “Who can log in and what they are allowed to do”
AD DS → “Login system (users, computers, domain)”
DNS → “Name system (google.com → IP)”
DHCP → “Auto IP provider”
AD CS → “the system that proves identity using certificates instead of passwords using certification authority”
AD FS → “Single login for many apps related with Azure Active Directory (Entra ID) that Controls cloud/app logins and internet services”
AD RMS → “Protect documents”
Device Health Attestation → “Check if device is safe”

B) FILES & RESOURCES : “Store and share company resources”
File Server → “Shared folders”
Print Server → “centralizes management of network printers”

C) NETWORK ACCESS : “How users connect to company network”
Remote Access (VPN / RRAS) → “Connect from outside”
Network Policy & Access Services (NPAS) → “Control who can connect to network”

D) REMOTE WORK :“How users work remotely”
RDS (Remote Desktop Services) → “Full desktop on server”

E) VIRTUALIZATION : “Run servers inside servers”
Hyper-V → “Create virtual machines”
Host Guardian Service → “Protect virtual machines (advanced security)”
includng also containers, a lighter-weight form of virtualization that packages an application with just what it needs to run Containers share the host OS kernel

F) WEB & APPLICATIONS: “Hosting websites and apps”
IIS (Web Server) → “Host websites / web apps”

G) SYSTEM DEPLOYMENT & MANAGEMENT: “Install, update, and activate systems”
WSUS → “Manage Windows updates centrally”
WDS → “Install Windows over network without inserting a USB drive or DVD”
Volume Activation Services → “Activate Windows/Office in company”

H) LEGACY / RARE USE: “Old or special systems”
Fax Server → “Send/receive fax (old technology)”

###  Storage Architecture 

Windows uses this layered system
- **File Server:** Shares folders across network
- **NTFS (New Technology File System):** the traditional, well-tested file system Windows Server uses to organize data on disks, supporting file permissions, encryption, and compression
- **ReFS (Resilient File System):** a newer file system designed for very large data sets and better resilience against data corruption, often used for storage-heavy roles like Hyper-V virtual machine storage.
- **Storage Spaces:** a software layer that pools multiple physical disks together and presents them as flexible virtual disks, similar in spirit to RAID but managed through software.
- **Storage Spaces Direct (S2D):** an advanced version of Storage Spaces that pools local storage across multiple servers in a cluster, creating shared, highly available storage without needing a separate, expensive storage array.
- **physical disks:** (HDD/SSD)
- **Storage Replica:** a feature that copies data between two servers (even in different locations) in near real-time, used for disaster recovery.

###  High Availability and Failover architecture

Types of Failover
 Active-Passive(running/standby)
 Active-Active(running/running)
- **Failover Clustering:** a group of servers (called nodes) that work together so that if one node fails, another node automatically takes over its workload with minimal interruption.
- **Quorum:** a voting mechanism used by a cluster to decide which nodes are healthy and should keep running. 
- **Network Load Balancing (NLB):** distributing incoming network traffic across multiple servers so no single server becomes overwhelmed, and so traffic can be redirected if one server goes down.

###  Security architecture

- **Identity Protection:** This is mainly handled by Active Directory.
- **Access Control:** What are you allowed to do.
- **BitLocker:** full-disk encryption, protecting data if a physical drive is stolen.
- **Microsoft Defender Antivirus:** built-in malware protection running directly in the OS.
- **Credential Guard:** isolates and protects login credentials in a separate, hardened part of memory so malware has a much harder time stealing passwords from a compromised machine.
- **Just Enough Administration (JEA):** Give the minimum permissions necessary.
- **Group Policy security baselines:** Microsoft-published, pre-configured sets of security settings that organizations can apply as a starting point rather than guessing at safe defaults.
- **Windows Firewall:** decides what is allowed.
- **TLS Encryption:**

### Management and Administration architecture

Server Manager: Central management interface.
Windows Admin Center: Web-based management platform.
PowerShell: Command-line automation and scripting tool.
Event Viewer: System log monitoring and troubleshooting.
Performance Monitor: Tracks system performance metrics.

### Cloud and Hybrid Integration 

Modern Windows Server is built to bridge on-premises and cloud environments. 
- **Azure Arc** it allows a company to manage on-premises Windows Servers from the same Azure portal used for cloud resources
- **Microsoft Entra Connect** (formerly Azure AD Connect) synchronizes an on-premises Active Directory with Azure Active Directory. so an employee can use one set of credentials to log into their office computer and into cloud services like Microsoft 365.
- Windows Admin Center vs azure arc ?
  



### 6.3 What "Out of Support" Really Means, Technically
It is important to note this does not mean the server suddenly stops working the day support ends — the operating system keeps running exactly as before. 
The real danger is silent and cumulative: every month without patches widens the gap between the server's defenses and the latest known attack techniques.
- **No more security patches.**  the organization needs to pay for extended Security Updates(ESU) 
- **Compliance failures.** An unsupported server can fail an audit even if it's technically still working fine.
- **Vendor and insurance risk.** Cyber-insurance policies increasingly require supported software; running unsupported servers can void coverage after a breach. Third-party software vendors may also drop support for their own applications running on an unsupported OS.
- **Growing attack surface over time.** The longer a server stays unpatched after end of support, the more publicly known exploits accumulate against it, while defenses stay frozen in time.


---

### Windows Server 2019 Licensing

### A/ choose Edition
- **Standard Edition:** for normal companies
1 physical server
up to 2 virtual machines
- **Datacenter Edition:** for big companies / cloud / many VMs
unlimited virtual machines
advanced features
- **Essentials Edition:** very small business
max 25 users
simple setup

### B/ Core Licenses
Both Standard and Datacenter require licensing 
You are not buying a “package”, You are buying core units, and 16 is just the minimum starting point
1 license(pack) = 2 cores so if you have a 16 core server you need to buy 8 licenses

### C/ Client Access Licenses (CALs)
In addition to the server license itself, nearly every user or device that connects to a Windows Server typically needs a **Client Access License (CAL)**. There are two broad categories:
-- Windows Server Base CAL: It gives permission to access:
AD DS (logins / domain)
File servers
DNS / DHCP management access
General Windows Server services

-- Add-on CALs: These are extra licenses added ON TOP of base CALs.
RDS CAL(if users use Remote Desktop Services (RDS))
AD RMS CAL(for Document protection / rights management)
Exchange CAL (mail system)
SharePoint CAL (collaboration services)

CALs can be assigned **per user** (one person can connect from any device) or **per device** (one device can be used by any person) — organizations typically choose whichever model results in a lower total cost based on their staffing patterns.

### D/ HOW you buy it (Licensing Channels)
- **Software Assurance programs:** larger organizations typically buy through volume licensing agreements rather than retail boxes, which can also unlock benefits like the **Azure Hybrid Benefit**, letting a company reuse existing Windows Server licenses to reduce the cost of running Windows VMs in Azure.
- **OEM Licensing:** License comes pre-installed with hardware.
- **Hyper-V Server (legacy free edition):** a free, standalone product containing only the hypervisor itself, with no Windows Server roles or GUI — useful for pure virtualization hosts, Free OS only for running virtual machines — nothing else.
- 
NOTE:  **Use Microsoft's official licensing calculators and documentation**, or work with a licensed Microsoft partner, before assuming a license configuration is correct — especially in virtualized environments.


---





