# Intro to Cloud Architecture: AWS Global Infrastructure, EC2 & Storage 

**Date:** 2026-07-02
**Category:** Cloud Computing / AWS
**Tags:** #AWS #Cloud #Infrastructure #DevOps #Storage

Today I shifted my focus from local server management to Cloud Architecture. I learned the foundational concepts of Amazon Web Services (AWS) and how physical data centers are abstracted into scalable, on-demand cloud resources.

## 1. On-Premises vs. Cloud Computing 
Before the cloud, every company had to build their own infrastructure from scratch.

*   **On-Premises (On-Prem) Data Center:** You physically buy, rack, and stack the servers in your own building. You are 100% responsible for the hardware, electricity, cooling, physical security, and replacing dead hard drives. 
    *   *The Downside:* You have to guess your capacity. If you buy 10 servers and suddenly go viral, your site crashes. If nobody visits, you wasted money on unused hardware. (Historically used by strict institutions like banks or government agencies).
*   **Cloud Computing:** The delivery of computing services (servers, storage, databases, networking) over the internet. Instead of buying hardware, you *rent* it on a **pay-as-you-go** pricing model.
    *   *The Upside:* Infinite scalability. You can spin up 1,000 servers in 10 seconds, and terminate them an hour later, only paying for that exact hour of compute time. (Used by Netflix, Airbnb, and modern startups).

## 2. AWS Global Infrastructure 
AWS doesn't just have one giant server room. The infrastructure is meticulously designed for high availability and fault tolerance.

### A. Regions (The Macro Level)
*   **What it is:** A physical, geographical area in the world where AWS clusters its data centers (e.g., `ap-south-1` is in Mumbai, `us-east-1` is in Northern Virginia).
*   **Industry Concept:** You choose a Region based on legal compliance (data residency laws) and proximity to your users (to reduce latency). If most of your users are in Kerala, deploying to the Mumbai region gives the fastest response time!

### B. Availability Zones (AZs) (The Micro Level)
*   **What it is:** Inside every Region, there are multiple (usually 3 or more) isolated Availability Zones (e.g., `ap-south-1a`, `ap-south-1b`). An AZ is one or more discrete physical data centers with completely independent power grids, cooling, and networking.
*   **Industry Concept (High Availability):** If a massive flood or power grid failure takes out `ap-south-1a`, the data centers in `ap-south-1b` will stay online. Top-tier developers always deploy their applications across *multiple* AZs so the app never goes down.

### C. Edge Locations (The Speed Boost)
*   **What it is:** AWS has hundreds of smaller, specialized data points scattered globally, separate from the main Regions.
*   **What they do:** They act as caching stations for Content Delivery Networks (CDNs), specifically AWS CloudFront. If a user in London requests a video hosted on your Mumbai server, AWS caches a copy at the London Edge Location. The next user in London downloads it instantly from the Edge, completely bypassing the long trip to India!

## 3. Compute: Amazon EC2 (Elastic Compute Cloud) 
*   **What it is:** EC2 is the core compute service of AWS. It allows you to rent a virtual machine (VM) in the cloud. You select the Operating System (e.g., Ubuntu Linux, Windows), the CPU cores, and the RAM. 
*   **The "Elastic" Part:** You can easily scale the size of your EC2 instance up or down with a simple reboot.
*   **Industry Example:** This is where you would log in via SSH and install your Apache web server, Python backend, or Docker containers, just like you would on a local Linux machine.

## 4. Storage: EBS vs. EFS 
When you rent an EC2 server, you need a hard drive to store its files. AWS splits storage into distinct types based on how it attaches to the server.

### EBS (Elastic Block Store) - The Internal Drive
*   **What it is:** A virtual hard drive (block storage) that is attached to a *single* EC2 instance. 
*   **The Rule:** Think of this like the physical NVMe SSD inside a laptop. It is blazing fast and holds your root operating system, but **it can only be attached to ONE server at a time**, and it is locked to a specific Availability Zone.

### EFS (Elastic File System) - The Shared Network Drive
*   **What it is:** A fully managed, scalable network file system (NFS). 
*   **The Rule:** Unlike EBS, EFS can be attached to **hundreds or thousands of EC2 instances simultaneously**, even across different Availability Zones within a Region! 
*   **Industry Example:** Imagine you have 5 different EC2 web servers running your application. If users upload profile pictures, you don't want those pictures scattered across 5 different EBS drives. You save them to a single EFS drive, and all 5 web servers instantly have read/write access to the exact same files!