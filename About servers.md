# Server Explained

## 1 Why Do We Even Need Servers?
A/workgroup vs domain :

In a workgroup each computer keeps its own local user accounts and passwords inside a file called the SAM (Security Account Manager) database . This is fine for a tiny office with two or three computers, but it becomes unmanageable fast as a company grows, since there's no central place to add, remove, or reset a user, and no way to push the same security rule to every machine at once.

A domain solves exactly this problem by moving from SAM into one shared, centralized database managed by a Domain Controller (DC) — a server running Active Directory. Once a computer joins the domain, it stops trusting its own local SAM for domain logins and instead asks the DC "is this username and password valid?" every time someone signs in. 
That's the whole reason domains exist: one account, created once, works on every machine in the company, and one administrator can manage permissions, password policies, and security settings for the entire organization from a single place.
This relationship is known as the **client-server model**.

B/server vs desktop:

You might be wondering: if a server is "just a computer," why can't I turn my home desktop PC into one? Technically you can for small tests, but in any serious business environment, the answer is no — and the reason is **design philosophy**. that single difference in purpose changes everything about how the hardware and software are built.

Let's break down what makes server hardware fundamentally different:
- **Server-grade CPUs and ECC RAM.** Servers use processors built for this job, such as Intel Xeon or AMD EPYC, instead of the consumer chips found in desktops (like Intel Core i5/i7). These server CPUs support **ECC RAM (Error-Correcting Code memory)**. Normal RAM occasionally flips a bit by accident (caused by electrical noise or even cosmic radiation), and on a desktop you'd never notice. On a server handling financial transactions or medical records, an unnoticed bit flip could corrupt critical data. ECC RAM automatically detects and corrects these small errors before they cause damage.

- **Dual CPU sockets.** Many server motherboards have **two physical CPU sockets** instead of one. This is called a dual-socket or multi-socket design. It means the server can run two separate processors at the same time, giving it far more processing power for heavy workloads like databases, and also giving some redundancy, since the system can sometimes keep operating in a degraded state if there's a hardware issue with one path.

- **RAID (Redundant Array of Independent Disks).** is a technology that combines multiple physical disks into a single logical storage system. It was created to solve the problem of disk failures in servers. A RAID controller manages how data is stored across the disks using techniques such as striping (splitting data for speed), mirroring (duplicating data for protection), and parity (storing recovery information). Depending on the RAID level used, RAID can improve performance, increase data availability, and allow a server to continue operating even when one or more disks fail.
  
- **Redundant, hot-swappable power supplies.** Most servers ship with two power supply units (PSUs) instead of one. If one PSU fails, or even if someone accidentally unplugs one cable, the server keeps running on the second supply without any downtime. Technicians can often replace ("hot-swap") a failed PSU while the server keeps running, with no shutdown needed.

- **A purpose-built operating system.** A desktop OS like Windows 11 is optimized for one user clicking around a graphical interface. A server OS like Windows Server is optimized to run unattended for long stretches, handle hundreds of simultaneous network connections, and expose specialized management tools instead of consumer features like a Start menu full of games.

## 2 The Many Types of Servers
Servers aren't a single "thing" — they're categorized in three different ways, and it's worth understanding all three before going further.

**By function (what software role the server plays):**
- **Web Servers** – deliver websites and web applications to browsers (example software: IIS on Windows, or Apache/Nginx on Linux).
- **Database Servers** – store and manage structured data so applications can read and write it (example: Microsoft SQL Server).
- **File Servers** – store shared files and folders that many users can access over the network.
- **Mail Servers** – send, receive, and store email (example: Microsoft Exchange Server).
- **Application Servers** – run the business logic of a software application, sitting between the database and the end user.
- **Proxy Servers** – sit between clients and the internet, forwarding requests, often for security, filtering, or caching.
- **DNS Servers** – translate human-friendly names (like `www.example.com`) into the numeric IP addresses computers actually use to find each other.

**By form factor (the physical hardware shape):**
- **Tower Servers** – look like a large desktop PC tower; common in small businesses with no dedicated server room.
- **Rack Servers** – server that is built specifically to be mounted within a server rack they are typically built with all the needed components to operate as a stand-alone system. They can be very powerful and are used to run high end applications.
- **Blade Servers** – even more compact; individual "blades" slot into a shared chassis that provides shared power, cooling, and networking, allowing very high density.
Choose rack servers if you need flexible, independent computers, have a smaller budget, or want to expand slowly. Choose blade servers if you need extreme processing power in a small space, require massive virtualization (running many virtual computers on one machine), and have a larger budget

**By architecture/deployment model:**
- **Cloud Servers** – virtual machines rented from a provider like Microsoft Azure, AWS, or Google Cloud, running on the provider's physical hardware.
- **Virtual Servers** – software-based servers that share the physical resources of one real machine, created using a technology called a **hypervisor** (explained later in this article).
- **Edge Servers** – smaller servers placed physically closer to end users (for example, in a regional office or a cell tower site) to reduce delay (latency) for things like video streaming or IoT data processing.

## 3 how this is done :

Requests constantly move between these two worlds.
User Mode
   IIS
   DNS
   DHCP

Kernel Mode
   Scheduler
   Drivers
   TCP/IP Stack
   Memory Manager


Power On
 ↓
BIOS/UEFI(firmware stored on the motherboard by running POST (Power-On Self-Test) whose only purpose is:
1. Wake up hardware
2. Check hardware
3. Find the operating system
4. Load the operating system)
 ↓
Boot Loader(Windows Boot Manager is a small program that starts Windows from your hard drive and brings Windows into memory)
 ↓
Windows Kernel(The kernel is not started by another program — it starts executing itself immediately after the Windows Loader loads it into RAM)

 ↓
System Services Start(decides which services must start automatically
launches them
Network service
Windows Update
Security services
Server roles (DNS, AD DS, etc.))
 ↓
Server Application Opens Port(services starting (handled by SCM))
 ↓
Client Sends Request
 ↓
NIC Receives Packet
 ↓
TCP/IP Stack Processes Packet(packet processing is done in the kernel)
 ↓
Request Assigned (Event / Thread / Process)
 ↓
Application Logic Executes(
Event-driven
Request arrives
 ↓
Event generated
 ↓
Event loop wakes up
 ↓
Request handled

Thread-based
Request arrives
 ↓
Thread selected
 ↓
Thread handles request

Process-based
Request arrives
 ↓
Process assigned
 ↓
Process handles request
)IIS (web server):

It uses a hybrid model:

worker processes (W3WP.exe)
multiple threads inside them
async/event-based networking
 ↓
Response Generated
 ↓
TCP/IP Stack Sends Response
 ↓
Client Receives Response















