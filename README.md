# Static_and_Default_Routing_for_State_Bank_of_India-SBI-_Branch_Network

## Project Overview

This project demonstrates the implementation of Static Routing and Default Routing using the GNS3 network simulation platform. The project focuses on providing Internet connectivity to the SBI Enterprise Network using dual ISP connectivity with primary and backup routes.

The network consists of an SBI Enterprise Router connected to two service providers (AIRTEL and TATA), which further connect to a GOOGLE service provider network. DHCP services are configured for end-user devices, and static routes are implemented to provide reliable communication and Internet access.

The project demonstrates:

- Static Routing
- Default Routing
- Floating Static Routes
- ISP Redundancy
- DHCP Configuration
- Network Connectivity Verification
- Traceroute Analysis
- Enterprise WAN Design

---

## Objectives

- Configure and implement Static Routing.
- Configure Default Routing.
- Implement Floating Static Routes.
- Configure DHCP services.
- Provide dual ISP connectivity.
- Implement primary and backup Internet paths.
- Verify routing table entries.
- Analyze traceroute path selection.
- Demonstrate enterprise network redundancy.
- Understand static route operation.

---

## Technologies Used

- GNS3
- Cisco IOS Routers
- Static Routing
- Default Routing
- Floating Static Routes
- DHCP
- VPCS
- Ethernet Switch
- ICMP
- Solar-PuTTY

---

## Network Topology

Topology image is available in:

```
topology/Static-and-Default-Routing-for-State-Bank-of-India-SBI-Branch-Network_Topology.png
```

---

## Network Architecture

The network consists of:

- R1 configured as SBI Enterprise Gateway Router.
- R2 configured as DHCP Client Router.
- R3 configured as GOOGLE Service Provider Router.
- R4 configured as AIRTEL Service Provider Router.
- R5 configured as TATA Service Provider Router.
- PC1 configured as DHCP Client.
- PC2 configured as DHCP Client.
- Switch1 providing Layer-2 connectivity.

---

## IP Addressing

### Service Provider Network

| Device | Interface | IP Address |
|---------|-----------|------------|
| R3 | Loopback0 | 8.8.8.8/32 |
| R3 | G1/0 | 192.168.34.3/24 |
| R3 | G2/0 | 192.168.35.3/24 |
| R4 | G1/0 | 192.168.34.4/24 |
| R4 | G2/0 | 192.168.14.4/24 |
| R5 | G1/0 | 192.168.15.5/24 |
| R5 | G2/0 | 192.168.35.5/24 |

### Enterprise Network

| Device | Interface | IP Address |
|---------|-----------|------------|
| R1 | G1/0 | 192.168.15.1/24 |
| R1 | G2/0 | 192.168.14.1/24 |
| R1 | E6/0 | 192.168.1.100/24 |
| PC1 | E0 | DHCP Assigned |
| PC2 | E0 | DHCP Assigned |
| R2 | E6/0 | DHCP Assigned |

---

## DHCP Configuration

### DHCP Pool Configuration

```bash
ip dhcp pool SBI-BANK
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.100
 dns-server 192.168.1.101 192.168.1.102
```

---

## Static Routing Configuration

### GOOGLE Router (R3)

Configured static routes:

```bash
ip route 192.168.14.0 255.255.255.0 192.168.34.4

ip route 192.168.15.0 255.255.255.0 192.168.35.5

ip route 192.168.1.0 255.255.255.0 192.168.35.5 10

ip route 192.168.1.0 255.255.255.0 192.168.34.4 20
```

### AIRTEL Router (R4)

Configured static routes:

```bash
ip route 192.168.1.0 255.255.255.0 192.168.14.1

ip route 8.8.8.8 255.255.255.255 192.168.34.3
```

### TATA Router (R5)

Configured static routes:

```bash
ip route 8.8.8.8 255.255.255.255 192.168.35.3

ip route 192.168.1.0 255.255.255.0 192.168.15.1
```

---

## Default Route Configuration

### Primary Default Route

```bash
ip route 0.0.0.0 0.0.0.0 192.168.14.4 10
```

### Backup Default Route

```bash
ip route 0.0.0.0 0.0.0.0 192.168.15.5 20
```

Where:

- 192.168.14.4 represents the AIRTEL ISP Gateway.
- 192.168.15.5 represents the TATA ISP Gateway.
- Administrative Distance 10 acts as the preferred path.
- Administrative Distance 20 acts as the backup path.

---

## Verification Commands

### Router Verification

```bash
show ip interface brief

show ip route

show running-config
```

---

## DHCP Client Verification

### PC1

```bash
ip dhcp
```

Assigned:

```text
IP Address : 192.168.1.2/24
Gateway    : 192.168.1.100
```

### Router R2

Verified using:

```bash
show ip interface brief
```

---

## Connectivity Verification

### PC1 to GOOGLE

```bash
ping 8.8.8.8
```

Successful communication verified.

---

### Traceroute Verification

```bash
trace 8.8.8.8
```

Output:

```text
192.168.1.100
192.168.14.4
192.168.34.3
8.8.8.8
```

---

## ISP Failover Verification

The network is configured with two ISP connections.

- AIRTEL is configured as the primary ISP.
- TATA is configured as the backup ISP.

When the AIRTEL connection fails, traffic automatically switches to the TATA path using Floating Static Routing.

This provides enterprise-level WAN redundancy.

---

## Screenshots

Available in the screenshots folder:

```text
 Service_Provider_GOOGLE_Routing_Table.png
 SBI_Enterprise_Routing_Table.png
 AIRTEL_Routing_Table.png
 TATA_Routing_Table.png
 GOOGLE_Loopback_Route_Verification.png
 AIRTEL_Loopback_Route_Verification.png
 AIRTEL_Loopback_Routing_Table.png
 TATA_Loopback_Route_Verification.png
 SBI_Enterprise_End_to_End_Connectivity_Test.png
```

---

## Project Structure

```text
Static-and-Default-Routing-for-State-Bank-of-India-SBI-Branch-Network

├── README.md

├── Topology
│   └── Static-and-Default-Routing-for-State-Bank-of-India-SBI-Branch-Network_Topology.png

├── Screenshots
│   ├── Service_Provider_GOOGLE_Routing_Table.png
│   ├── SBI_Enterprise_Routing_Table.png
│   ├── AIRTEL_Routing_Table.png
│   ├── TATA_Routing_Table.png
│   ├── GOOGLE_Loopback_Route_Verification.png
│   ├── AIRTEL_Loopback_Route_Verification.png
│   ├── AIRTEL_Loopback_Routing_Table.png
│   ├── TATA_Loopback_Route_Verification.png
│   └── SBI_Enterprise_End_to_End_Connectivity_Test.png

├── Configurations
│   ├── R1_DHCP_Server_Configuration.txt
│   ├── R3_GOOGLE_Configuration.txt
│   ├── R4_AIRTEL_Configuration.txt
│   ├── R5_TATA_Configuration.txt
│   └── PC1_Configuration.txt

└── Project_file
    └── SBI_Static_Default_Routing_Lab.gns3
```

---

## Learning Outcomes

- Static Routing
- Default Routing
- Floating Static Routing
- DHCP Configuration
- ISP Redundancy
- Enterprise WAN Design
- Route Selection
- Administrative Distance
- Network Troubleshooting
- Cisco Router Configuration
- Traceroute Analysis
- GNS3 Network Simulation

---

## Conclusion

This project demonstrates how enterprise branch networks use Static Routing and Default Routing to establish Internet connectivity through multiple ISPs. By implementing Floating Static Routes, the network provides redundancy and automatic failover, ensuring continuous connectivity even if the primary ISP link fails.

---

## Author

**Chanakya Burugu**

Computer Science and Engineering (Networks)

Networking and Infrastructure Enthusiast
