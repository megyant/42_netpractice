*This project has been created as a part of the 42 curriculum by <mbotelho>mbotelho*

# Netpractice

Netpractice is a networking project focused on understanding network addressing, subnetting, and routing principles.

## Description

### OSI layers

The Open Systems Interconnection (OSI) model is a conceptual 7-layer framework established to standardize how data is transmitted across computer networks. Rather than describing a specific software implementation, it divides the complex process of networking into seven distinct functional layers:

1. **Layer 7 — Application:** Provides networking services directly to end-user software applications (e.g., web browsers, email clients). Protocols include HTTP, HTTPS, SSH, FTP, and DNS.
2. **Layer 6 — Presentation:** Responsible for translating, formatting, compressing, and encrypting/decrypting data so the application layer can read it (e.g., SSL/TLS, ASCII, JPEG).
3. **Layer 5 — Session:** Manages, maintains, and terminates communication sessions between software applications running on separate hosts (e.g., NetBIOS, RPC).
4. **Layer 4 — Transport:** Ensures end-to-end data transfer, flow control, and error correction. It segments application data and assigns port numbers (e.g., TCP for reliable delivery, UDP for low-latency transmission).
5. **Layer 3 — Network:** Handles logical device addressing and packet routing across multiple networks. It determines the optimal physical path for data to travel (e.g., IP, ICMP).
6. **Layer 2 — Data Link:** Responsible for node-to-node data transfer across the same physical network segment. It encapsulates network packets into frames, handles hardware addressing (MAC addresses), and manages local frame delivery via switches.
7. **Layer 1 — Physical:** Transmits raw, unformatted bitstreams over a physical transmission medium (e.g., copper cables, fiber optics, radio frequencies) and defines hardware specifications like voltages and pinouts.

### TCP/IP addressing

While the OSI model serves as a theoretical reference framework, real-world internet and local network communication relies on the practical 4-layer TCP/IP protocol suite (Application, Transport, Internet, and Physical/Network Access). 

Addressing in TCP/IP operates hierarchically across different layers to ensure data reaches the correct device and the correct application running on that device:

* **Port Addressing (Transport Layer):** Transport protocols like TCP and UDP use 16-bit port numbers to identify specific software processes on a host. For instance, HTTP defaults to port 80, HTTPS to port 443, and DNS to port 53. Port addressing allows a single host to run multiple networked applications simultaneously without traffic collision.
* **IP Addressing (Internet Layer):** The Internet Protocol handles logical host identification and packet routing across network boundaries.
* **Protocol Differences:**
  * **TCP (Transmission Control Protocol):** A connection-oriented protocol that establishes a formal connection before transferring data (via a three-way handshake). It guarantees that all packets arrive in order and without errors through retransmission and sequencing.
  * **UDP (User Datagram Protocol):** A connectionless, lightweight protocol that sends datagrams without establishing a prior connection or guaranteeing delivery. It trades error checking and packet ordering for minimal latency, making it ideal for real-time applications like video streaming and DNS queries.

### IP addresses

An IP (Internet Protocol) address is a unique logical identifier assigned to every device connected to an IP network. Under the IPv4 standard, an address is a 32-bit binary number traditionally written in human-readable dotted-decimal notation as four 8-bit numbers (octets) separated by periods, ranging from `0` to `255`.

Every IPv4 address contains two distinct pieces of information:

1. **Network ID:** Identifies the specific logical network to which the host belongs.
2. **Host ID:** Identifies the specific host device (computer, server, printer, router interface) within that network.

Within any assigned IP network segment, two specific host addresses are reserved by networking standards and cannot be assigned to individual host interfaces:

* **Network Address:** The address where all host bits are set to `0`. It represents the identity of the entire subnetwork.
* **Broadcast Address:** The address where all host bits are set to `1`. Sending data to this address forwards the message to every host currently active on that subnetwork.
* **Usable Host Addresses:** The range of addresses strictly between the Network Address and the Broadcast Address. The total number of usable host IPs in any subnet is calculated as $2^{\text{host bits}} - 2$.

### Subnet masks

A subnet mask is a 32-bit value used by networking hardware and software to distinguish the Network ID from the Host ID within an IP address. It consists of a contiguous block of binary `1`s masking the network bits, followed by a contiguous block of binary `0`s marking the host bits.

#### CIDR Notation
In Classless Inter-Domain Routing (CIDR), subnet masks are written using prefix slash notation (e.g., `/24`). The number following the slash represents the exact count of leading binary `1`s in the mask:
* A `/24` prefix indicates 24 network bits (`255.255.255.0`), leaving 8 bits for host addresses ($2^8 - 2 = 254$ usable hosts).
* A `/25` prefix indicates 25 network bits (`255.255.255.128`), leaving 7 bits for host addresses ($2^7 - 2 = 126$ usable hosts).

#### Subnetting Purpose
Subnetting is the architectural technique of borrowing host bits to turn them into additional network bits. This divides a single large network into multiple smaller, isolated subnetworks (subnets). Subnetting serves three key purposes:
* **Reducing Broadcast Traffic:** By shrinking the size of individual subnets, broadcast messages are contained within smaller boundaries, preventing network performance degradation known as broadcast storms.
* **Security & Access Control:** Separating different departments or service tiers into distinct subnets allows network administrators to enforce security boundaries and restrict access using routers or firewalls.
* **Address Efficiency:** Subnetting prevents address waste by matching the allocated subnet size closely to the actual number of host devices required.

### Routers and Switches

Local network communication and internetwork routing rely on two distinct types of physical networking hardware that operate at different layers of the network stack:

* **Switches (Layer 2 Devices):**
  * Operate primarily at the Data Link layer of the OSI model.
  * Connect host devices within the same Local Area Network (LAN).
  * Use **MAC addresses** (hardware addresses burned into network interface cards) to direct traffic.
  * Maintain an internal MAC address table to forward incoming frames directly to the specific port connected to the target destination, allowing simultaneous full-duplex communication without collisions.

* **Routers (Layer 3 Devices):**
  * Operate at the Network layer of the OSI model.
  * Connect separate logical networks together (such as connecting a private LAN to a Wide Area Network or the public Internet).
  * Use **IP addresses** and routing tables to determine the optimal path for forwarding data packets between different subnets.
  * Routers act as boundary devices that stop Layer 2 broadcast traffic from leaving the local network.

### Default gateways

A Default Gateway is the designated network node—typically a local interface on a router—that serves as the forwarding point for traffic destined outside the local subnetwork.

Whenever a host on a network attempts to transmit a packet, it compares the target IP address against its own IP address and subnet mask:

1. **Intra-subnet Traffic (Local):** If the destination IP shares the same Network ID as the sender, the host determines the recipient is local. It uses ARP (Address Resolution Protocol) to resolve the recipient's MAC address and sends the data directly across the local switch.
2. **Inter-subnet Traffic (Remote):** If the destination IP has a different Network ID, the host cannot deliver the frame directly. Instead, it encapsulates the packet and forwards it directly to the configured **Default Gateway**. The router receiving the packet inspects the destination IP, consults its routing table, and routes the packet onward to external networks toward its destination.

## Instructions
How to run the training interface and to export configurations and submission requirements

### Instalation and usage
Clone this repository
```
git
cd netpractice
```

To launch the application, navigate to the program directory and execute the setup script:
```
cd net_practice.1.9/netpractice
./run.sh
```

If you encounter a permission denied error, grant execution permissions with:
```
chmod +x run.sh
```

Once the application opens, select your preferred mode:
- `Training`: Plays through all levels sequentially.
- `Evalutation`: Randomly selects 3 levels, starting from level 6.

### Submission details

To generate your submission files, click `Get my config` after completing a level and save the file in the root directory of the repository. There are 10 levels total and there should be a configuration file per level.

## Resources

### TCP/IP addressing
- [TCP/IP Model](https://www.geeksforgeeks.org/computer-networks/tcp-ip-model/)
- [What is TCP/IP?](https://www.techtarget.com/it-infrastructure/definition/What-is-TCP-IP)

### Subnet Masks
- [What is a Subnet Mask](https://www.portnox.com/cybersecurity-101/networking/what-is-a-subnet-mask/)
- [Subnet mask cheat sheet](https://www.aelius.com/njh/subnet_sheet.html)

### Default gateways
- [Default gateway wikipedia](https://en.wikipedia.org/wiki/Default_gateway)
- [Default Gateway in Networking](https://www.geeksforgeeks.org/computer-networks/default-gateway-in-networking/)

### Routers and Switches
- [What is a switch vs a router?](https://www.cisco.com/site/us/en/learn/topics/small-business/network-switch-vs-router.html)
- [Difference Between Router and Switch](https://www.geeksforgeeks.org/computer-networks/difference-between-router-and-switch/)

### OSI layers
- [Layers of OSI Model](https://www.geeksforgeeks.org/computer-networks/open-systems-interconnection-model-osi/)
- [OSI model wikipedia](https://en.wikipedia.org/wiki/OSI_model)

### Others
- [Subnet Mask - Explained](www.youtube.com/watch?v=s_Ntt6eTn94)
- [NetPractice: An Intro to IP Addresses and Subnets](https://www.youtube.com/watch?v=HQUw0CfQWAM&t=1097s)

### Use of Artificial Intelligence
Gemini was used to assist in the making of this README