# Zero to Hero > 01 - General IT Review
## 01 - General IT Review - 02 - LinkedIn Learning 

#### Overview of Networking / Networking Basics


| Topics | Specific Notes | Extended Notes |
| :----| :------- | :--- |
| HTTP/HTTPS | Ports-TCP-80/TCP-443 | DNS/NTP (Ports-TCP/UDP-53/123) | 
|DNS - TCP or UDP Port 53 | NTP/DHCP UDP - 123/67| NTP-UDP-123 / DHCP-UDP-67|

### DHCP 
```
Discover - Offer - Request - Acknowledgement
PC A to Switch 1 to DHCP Server 
PC A to DHCP Server (Discover)
DHCP Server to PC A (Offer)
PC A to DHCP Server (Request)
DHCP Server to PC A (Acknowledgement)

DHCP Relay Agent:
- Forwarding from location coming into the Switch. 
Stateful / Stateless DHCPv6
- NDP Protocol - PC A Self-Generates via EUI-64 ; Extra Info pulled from DHCPv6 Server that is Stateless
```
### DNS / NAT / NTP / QoS
```
Domain Name System:
PC to Switch to Router to Internet / DNS Server

NAT: 
- Allows one set of addresses stored internally & can forward outward to the public internet
- NAT Network Address Translation

Client 1 & Client 2 to Switch 1 to Router 1 (Internal Gig1) to Router 1 (External Gig2) - (NAT-Enabled-Router) to Internet / Web Server
 - Router R1 has a NAT Translation Table (Inside Local / Inside Global)
- Dynamic NAT / Port Address Translation (PAT)
- PAT = Mapping between Internal Local &/ Internal Global = Allows forwarding from one application to one certain IP & forward to two internal locations. 
- If Client A goes outbound on a certain port - PAT allows the response to forward back to Client A & B

NTP: "Who do we trust to give us an accurate time in our server"
- Atomic Clocks / UDP Port 123 "It's easy as 1...2..3...baby you and NTP!"(Not Jackson 5) 
- Stratum number is the believability of a time source.
- Stratum numbers range 1-15 (Where when 16 is reached = "You too far from the clock")
- Each hops increases stratum value by 1

QoS: "Managed Unfairness"
Router out to 1 Gbps Out to 100 Mbps to Internet = Speed Mismatch
or
Multiple to Switch to Router = Network Aggregation

Classification & Marking: QoS Features
- Prioritize traffic by classifying it
- Queueing traffic (Doesn't prioritize)
- i.e. VoIP - Take the single queue of lots of requests & move it over to "Best Effort" meanwhile never let VoIP drop.
```

### Wireless Networks

```
Ad-Hoc WLAN
- No network infrastructure
- 2 Wireles clients communicating between each other (Bluetooth or Wi-fi)

Infrastructure WLAN:
- Wireless clients connect to access points / Scalable by adding access points with an ethernet switch to access point where wireless clients can then connect.
- Supports management and monitoring not found within ad hoc wireless network

Mesh WLAN:
- Multiple Access Points that bounce between each other (MESH) not connected by wires.
- Mesh Aps receive and retransmit wireless signal
- Allows for flexibility in AP placement
- When troubleshooting - Will have to reset individually
```
#### 2.4 GHz vs 5 GHz
```
Channel 1 / Channel 6 / Channel 11 - 5 Channel Separation = 2.4GHz Best
- Center frequencies 
- 2.4 GHz Channels require 5 channels of separation (i.e. 1-6-11)
- Think of honeycomb cells (1 with 6 & 11 spaces around where 6 will only overlap with 11 on either side 6 & 1 with 6 on the outside)

5 GHz Band - Allow for 40 GHz Channel Width / 60 GHz / 80 GHz 
- Allows for bleeding in between 

2.4GHz has a longer range & can go through walls for instance.
5GHz has a smaller range but is more compatible with overlaps / more devices. 
```
### Software-defined networking (SDN)
```
Use an application for setting network behavior intentions to a controller.

Southbound interfaces (South of controller) - Southbound interfaces with the clients on the network.
REST = Representational State Transfer (REST)
- Uses HTTP verbs / Allows XMP / JSON
- Northbound Interfaces hit the SDN then the SDN goes through the Southbound Interfaces & communicate back to the client
```
#### Virtualiziation

```
Ethernet Switch to Computer running Hypervisor

Type 1 Hypervisor
- Runs directly on top of a computer's underlying hardware (VMWares ESXi Hypervisor)
Type 2 Hypervisor
- Runs on top of an underlying OS
- VMware's fusion

Example: If you need 3 Virtual Servers
- Containers
- Can share an underlying OS with separate applications that rely on one central location. 
- i.e. Docker
```

#### Cloud Computing
```
Software as a Service - i.e. GSuite - offers applications that are typically accessible from a web browser. 
Infrastructure as a Service - Offers computer & storage resources in the cloud. 
Platform as a Service - offers a variety of computing platforms tuple OS systems and hardware platforms

Public Cloud
- Computing resources are owned by and maintained by the cloud service provider.
- Hardware for compute resources is shared among the pool of customers.

Private Cloud
- Computing resources are maintained by the same organization.
- Hardware computing resources is dedicated specifically to one organization. No general pooling of one tenant into one specific location. Allows dedicated services.

**Accessing the Cloud Provider?**
- Setup a VPN between Corporate Resources & Cloud Resources
- Even if data is interfered with - VPN will have it encrypted. 
```
#### So what if Wi-Fi 6 / IEEE 802.11ax & Why is it the Hottest Thing since Sliced Bread? 


| Year Released | Frequency Band | Maximum Bandwidth | QAM | Supported Channel Widths | Spatial Streams | Transmission Method |
| :----| :------- | :--- | :--- | :--- | :--- | :--- |
| 2019 | 2.4 and 5 GHz |  9.6 Gbps | 1024-QAM (10 bits) | 20-40-80-160 MHz | 8 Upstream and Downstream (Simultaneous) | OFDMA / Target Wake Time (TWT) |

```
TWT - feature that allows a wireless access point to tell a client when it's allowed to transmit and receive, which improves the issue of having multiple clients contenting for the same bandwidth at the same time.
QAM -  method of encoding data. Although, 802.11ax uses 1024-QAM, a QAM variant with a higher transmission rate than its predecessors, QAM does not prevent the issue of bandwidth contention.
```


```
Credits to Kevin Wallace for the above information
```

### Basics of IP Addressings

Breakdown of information
```
IPv4 = 32 bits (2^32) = Network Portion + Host Portion

IPv6 = 128 bits (2^128) = Prefix Portion + Host Portion

IP Address = unique identifier of a device on a network

Subnet mask - indicates dividing lines between the network and host portions of an IP address

Default gateway -IP address of a router where forwarding of packets can take place off of the local  network based on destination IP address information

DNS Server - IP address of a server than can translate to an easy to remember name

DHCP Server - Dynamically assign IPv4 and IPv6 addresses
    - Discover, Offer, Request, Acknowledgement
    - PC A to Switch to DHCP Server
    - If the router sits between these two:
    - Routers need a DHCP Relay Agent to forward it along to the DHCP Server - By default this will not occur.
IPv6 = Stateful DHCPv6 = Learn all the information from the DHCPv6 = Here is everything (Prefix/Length-Host-DNS Addressing)
    - Use NDP / Stateless DHCPv6
    - Uses Host/PC Self-Generated EUI-64
    - Self generates from Router & then requests only needed information / DNS Sserver's IPv6 Address from DHCPv6 Server
```
