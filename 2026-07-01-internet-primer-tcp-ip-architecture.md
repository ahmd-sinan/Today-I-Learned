# Internet Primer: IP, DNS, and TCP/IP Architecture 

**Date:** 2026-07-01

**Category:** Networking / Computer Fundamentals
**Tags:** #Networking #IP #DNS #TCP #InternetArchitecture

Today I learned the foundational architecture of the Internet. The Internet is essentially a massive, global network of interconnected computers communicating through standardized protocols.

## 1. Internet Protocol (IP) & Addressing 
Every device on the internet requires a unique identifier to communicate.
* **IPv4 (32-bit):** The traditional addressing system. It uses a `w.x.y.z` structure. For example, a DNS server maps a host to an IPv4 address like `74.125.202.138` or `255.255.255.254`.
* **IPv6 (128-bit):** Created because the world ran out of IPv4 addresses. It uses an `s:t:u:v:w:x:y:z` structure. For example, a host like `google.ie` maps to a 128-bit address like `2a00:1450:4010:c09:0:0:0:93`.

## 2. Local Networking & Access Points 
* To reach the wider internet, a user typically routes traffic through a DHCP Server, a DNS Server, and an Access Point.
* **DHCP (Dynamic Host Configuration Protocol):** The service that automatically leases and assigns an IP address to your device when you join a network.
* **Access Points:** Modern home networks combine a router, modem, and switch into a single access point device. Large-scale business WANs keep these as separate devices to allow the network to scale more easily.
* **NAT (Network Address Translation):** To deal with the IPv4 addressing problem, multiple people are assigned to the same IP address. The local router acts as a traffic cop, processing data requests from all local devices through that single, shared public IP address.

## 3. DNS (Domain Name System) 
DNS is the "yellow pages" of the internet. We use it to translate human-readable domain names into machine-readable IP addresses. It is used for web browsing, email routing, and more.
* An IPv4 DNS record maps a host (like `info.host1.net`) to an address (like `0.0.0.0`).
* An IPv6 DNS record maps a host (like `google.ie`) to a 128-bit address.

## 4. Packet Switching & Routing 
* Because the internet consists of many networks, we cannot afford to wire every single one directly to each other. However, each network still needs to communicate with the others so that pieces of the network are not isolated.
* **Packets:** When sending a web request, FTP file, or email, the data is not sent as one huge block. Sending massive blocks of data would throttle the network and cause slowdowns for all other users. Therefore, IP splits data into smaller packets.
* **Connectionless Protocol:** IP is a connectionless protocol, meaning there is no necessarily defined path between sender and receiver.
* Packets can be routed through multiple different nodes to reach the exact same destination IP address.
* If one path gets clogged with traffic, packets can be dynamically re-routed around the jam to find the optimal path based on the current state of the network.

## 5. Transmission Control Protocol (TCP) & Reliability 
If IP is the mail truck that drives the data from point A to point B, **TCP** is the strict manager that ensures every single piece arrives intact, in order, and goes to the exact right program on the computer. TCP and IP are an inseparable pair (**TCP/IP**).

**The 3-Step TCP/IP Lifecycle:**
1.  **Fragmentation & Wrapping:** When an application wants to send data, TCP breaks that massive file into smaller chunks. It adds a "TCP Header" onto each packet (containing sequence numbers and port data). 
2.  **IP Routing:** The IP protocol then wraps that TCP packet in its own "IP Layer" (containing the sender and destination IP addresses) and fires it across the network.
3.  **Arrival & Reassembly:** Because IP routing is dynamic, packets will take completely different paths and arrive out of order! When the destination computer receives them, TCP looks at the headers, organizes them back into the exact correct sequence, and reassembles the original file.

**Fault Tolerance (Handling Dropped Packets):**
The internet is chaotic. If a router gets overwhelmed with traffic, it will simply *drop* (delete) packets. TCP is a **Guaranteed Delivery** protocol. If a packet is dropped, TCP uses the sequence numbers in its headers to realize a piece is missing. It will explicitly request the sender to re-transmit that specific missing packet so the assembly can be completed!
* *(Industry Note: This is why downloading a file or loading a webpage is reliable. By contrast, fast-paced games like Apex Legends use **UDP** instead of TCP, which does NOT ask for missing packets, because waiting for a re-transmission would cause lag!)*

## 6. Ports 
Once TCP has perfectly reassembled the packets, it needs to know *what* the packet is actually for. It uses **Port Numbers** to direct the data to the correct running service. Every program or utility on a machine is assigned a specific port to listen for incoming TCP requests.

**Common Industry Ports:**
* `21`: FTP (File Transfers)
* `22`: SSH (Secure Remote Terminal Access - Crucial for Linux SysAdmins!)
* `25`: SMTP (Email Routing)
* `53`: DNS Resolution
* `80`: HTTP (Unencrypted Web Traffic)
* `443`: HTTPS (Secure Web Traffic)