# Networking Concepts

## LAN (Local Area Network)
- A LAN is a network that connects devices within a limited area such as an office or home.
- **Ethernet** is commonly used to connect LAN devices.
- Every Network Interface Card (NIC) has a unique **MAC address**.
- LAN typically uses **Star Topology**, where a central **Ethernet Switch** connects all devices.
- OSI Model is used to understand networking layers.
- Check MAC address using:
  - Windows: `ipconfig /all`
  - Linux/macOS: `ifconfig -a`

## Hub, Bridge, and Switches
- **Unicast**: One device communicates directly with another (one-to-one).
- **Hub**: Broadcasts to all; considered a collision domain; rarely used today.
- **Bridge**: Divides collision domains, better than hubs.
- **Switch**: Maintains a **MAC address table** mapping MAC addresses to ports. Broadcast domain per VLAN.
- **Router**: Breaks Layer 2 broadcast domains and routes packets between networks.

## OSI Model (All People Seem To Need Data Processing)
1. **L1 - Physical**
2. **L2 - Data Link**
3. **L3 - Network**
4. **L4 - Transport**
5. **L5 - Session**
6. **L6 - Presentation**
7. **L7 - Application**

## BUM Traffic (Broadcast, Unknown Unicast, Multicast)
- **ARP Request**: Layer 2 broadcast to discover MAC for a given IP.
- **Unknown Unicast**: Switch floods if MAC is unknown.
- **Multicast**: Sent to a group of devices.

## Routers
- Connect multiple switches and networks.
- Used to route traffic between subnets.

## TCP vs UDP

### UDP (User Datagram Protocol)
- Connectionless, faster, less reliable.
- Used in: VOIP, DNS, DHCP, TFTP
- Data units called **datagrams**.

### TCP (Transmission Control Protocol)
- Connection-oriented, error-checking, reliable.
- Used in: HTTP/HTTPS, FTP, Email
- Data units called **segments**.
- Each TCP segment has a **sequence number** and a **checksum**.

## IP Addressing & Subnetting
- Example: `10.1.1.1` (IPv4 address - 4 octets).
- `10.1.1.0/24` -> 24 bits network, 8 bits host.
- `10.1.1.0`: Network address.
- `10.1.1.1`: Default gateway.
- `10.1.1.255`: Broadcast address.

## VLAN (Virtual LAN)
- Logical segmentation within a physical switch.
- **Access Port**: Carries traffic for one VLAN.
- **Trunk Port**: Carries traffic for multiple VLANs.
- VLANs isolate traffic and increase security and performance.

## WAN (Wide Area Network)
- Connects networks over large geographical areas.
- Uses static or dynamic routing protocols.
- Metrics include bandwidth and latency.
- **Border Router** connects local networks to the internet.

## IPSEC VPN
- Secures data transmission over the public internet.
- VPN tunnel with shared secret and AES encryption.
- Interface with public IP is created on the router.

## Layer 2 VPN
- Keeps the same IP subnet at multiple sites (e.g., PDC and DR).
- Supported by technologies like VMware vSphere with long-distance vMotion.

## LTE/5G and SD-WAN
- **SD-WAN**: Software-defined WAN, uses multiple internet links for better performance and cost efficiency.

## Wi-Fi Standards
- **2.4 GHz**: ~45m indoor, ~90m outdoor, ~150 Mbps.
- **5 GHz**: ~15m indoor, ~1300 Mbps.
- **Access Point**: Device for connecting wireless devices.
- **WLAN Controller**: Manages multiple access points.
- **SSID**: Network name.

## Network Management, Design, and Troubleshooting

### Redundancy
- Eliminate Single Points of Failure (SPoF) using redundant NICs, switches, routers.
- Protocols: HSRP, VRRP for gateway redundancy.

### Load Balancing
- Distribute traffic across multiple targets.
- Components:
  - **Listener**: Port (e.g., HTTP 80)
  - **Targets**: VMs, containers
  - **Health Checks**
 

### DHCP (Dynamic Host Configuration Protocol)
- Automatically assigns IP addresses.
- Use `ipconfig /release` and `ipconfig /renew` to refresh IP.

### DNS (Domain Name System)
- Resolves domain names to IP addresses.
- `nslookup www.google.com` for testing.

## NetFlow and IPFIX
- **NetFlow**: Cisco protocol for traffic analysis.
- **IPFIX**: Industry standard equivalent.

## NTP (Network Time Protocol)
- Ensures consistent time across devices.
- Uses UDP port 123.
- Important for logging, certificates, and event correlation.

## NAT and IP Types
- **Private IP ranges**:
  - `10.0.0.0 – 10.255.255.255`
  - `172.16.0.0 – 172.31.255.255`
  - `192.168.0.0 – 192.168.255.255`
- **NAT** translates private IP to public IP on the router.

## Firewall Zones
- **Inside**: Private network
- **DMZ**: Semi-protected (Web, Email servers)
- **Outside**: Untrusted internet

## Firewall Types

### Layer 3 & 4
- Based on IP and port.
- Top-down rule processing.

### Layer 7 (Next-Gen Firewall)
- Deep packet inspection.
- Application-specific rules and updatable signatures.

## IDS and IPS
- Intrusion Detection & Prevention.
- Signature-based: Matches known patterns.
- Behavior-based: Monitors for anomalies.

## VPN Components
- AES-256 encryption ensures secure communication.
- VPN clients connect to nearest VPN server.

## IP Addressing Overview

### IPv4 Addressing
- Format: `192.168.0.6`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `192.168.0.1`

### Address Classes
- **Class A**: 1.0.0.0 to 127.0.0.0 (~16 million hosts)
- **Class B**: 128.0.0.0 to 191.255.0.0 (~65k hosts)
- **Class C**: 192.0.0.0 to 223.255.255.0 (254 hosts)

### Binary Conversion Example
- Decimal 135 to Binary:
  - 135 = `10000111`

### Subnetting
- Divide a network for efficient use.
- Hosts per subnet = `2^h - 2` (where h = number of host bits)
"""



