# Jio ISP Network Simulator — Network Configuration Guide

## 1. Introduction

The **Jio ISP Network Simulator** is a computer networking project developed using Cisco Packet Tracer 9.0.

It demonstrates how an Internet Service Provider (ISP) can connect multiple customers, provide network services, manage website access, and maintain connectivity when a network path fails.

The project uses a Jio-inspired ISP theme and a simulated network environment.

> **Note:** This project simulates internet services inside Cisco Packet Tracer. The demonstration websites are locally hosted sample pages, not the real Google, YouTube, Amazon, Netflix, Wikipedia, or Jio websites.

## 2. Software and Devices

| Component | Technology |
|---|---|
| Network simulation | Cisco Packet Tracer 9.0 |
| Routers | Cisco 2911 |
| Routing protocol | OSPF |
| Address allocation | DHCP |
| Domain resolution | DNS |
| Website service | HTTP |
| Network access control | Extended ACL |
| Address translation | NAT |
| Wireless connectivity | Wireless Access Point |
| Customer devices | PCs and laptops |

## 3. Network Architecture

The simulated ISP network contains the following major components:

### ISP Network

The ISP network connects the customer networks and provides access to the server network.

### Routers

Cisco 2911 routers are used to connect different network segments. OSPF is configured to exchange routing information between participating routers.

### Customer Switches

Switches connect wired customer devices to their respective customer networks.

### Wireless Access Points

Access points allow wireless customer devices, such as laptops, to connect to the network.

### ISP Servers

The server network hosts services such as DNS and HTTP. These services help customers resolve website names and open locally hosted demonstration pages.

## 4. Customer Networks and Addressing

The project uses separate IP networks for different customers.

| Network | Example network address |
|---|---|
| Customer A | `192.168.10.0/24` |
| Customer B | `192.168.20.0/24` |
| Customer C | `192.168.30.0/24` |
| ISP Server Network | `192.168.40.0/24` |

The example DNS server address used in the project is:

`192.168.40.10`

The customer gateway addresses and other server addresses should be checked against the final Packet Tracer topology.

## 5. Network Services and Protocols

### OSPF — Dynamic Routing

OSPF allows participating routers to exchange routing information and learn available network paths.

This helps the ISP routers communicate with different customer and server networks.

Useful verification commands:

```text
show ip ospf neighbor
show ip route
show ip protocols
```

### DHCP — Automatic IP Address Allocation

DHCP automatically provides network configuration to customer devices.

A DHCP configuration normally includes:

- IP address
- Subnet mask
- Default gateway
- DNS server address

Useful verification command:

```text
show ip dhcp binding
```

### DNS — Domain Name Resolution

DNS translates a domain name into an IP address.

Example:

`www.youtube.com` → `192.168.40.15`

The example address above represents the YouTube demonstration server in this project. Verify it against your DNS records.

Useful customer-side test:

```text
nslookup www.youtube.com
```

### HTTP — Website Access

The HTTP service hosts locally simulated websites.

Customers can open the configured website domain in a browser after the network and DNS services are working.

### NAT — Network Address Translation

NAT translates IP addresses between network domains. It is used in ISP network designs to demonstrate address translation.

Useful verification command:

```text
show ip nat translations
```

The presence of NAT translations depends on the NAT configuration and traffic generated in the simulation.

### ACL — Access Control List

Extended ACLs control which traffic is permitted or denied.

For example, an ISP administrator can block Customer B from accessing the YouTube demonstration server while allowing other traffic.

Useful verification command:

```text
show access-lists
```

### QoS — Quality of Service

QoS can be used to classify and prioritize selected network traffic. A Basic-versus-Premium service demonstration should only be claimed as completed if the QoS policy is configured, applied, and verified in the Packet Tracer project.

## 6. Website Demonstration

The project contains locally simulated website pages for:

- Google
- YouTube
- Amazon
- Netflix
- Wikipedia
- Jio

Each website has a separate folder containing:

- `index.html` — the website page with its internal CSS.
- Image files — the images used by that website.

Example:

```text
Website-Assets/
└── YouTube/
    ├── index.html
    ├── yt_gaming.jpg
    ├── yt_music.jpg
    └── yt_technology.jpg
```

All website images are stored in the same folder as their respective `index.html` file. There is no separate `Images` directory.

The HTML files use internal CSS. The website assets are stored separately in GitHub for backup, reference, and maintenance.

**Important:** Uploading these files to GitHub does not automatically configure the Packet Tracer server. The corresponding HTML and image files must also be present in the relevant Packet Tracer HTTP service. Keep the image filenames and HTML image paths consistent.

## 7. Network Recovery

The project demonstrates backup routing by using an alternative available network path.

When a primary link is disconnected, routers can use another learned route if the topology and OSPF configuration provide an alternative path.

Useful verification commands:

```text
show ip ospf neighbor
show ip route
ping <destination-ip>
```

## 8. Basic Troubleshooting Commands

| Purpose | Command |
|---|---|
| Check router interfaces | `show ip interface brief` |
| Check routing table | `show ip route` |
| Check OSPF neighbors | `show ip ospf neighbor` |
| Check DHCP bindings | `show ip dhcp binding` |
| Check ACL rules | `show access-lists` |
| Check NAT translations | `show ip nat translations` |
| Test connectivity | `ping <destination-ip>` |
| Test DNS resolution | `nslookup www.youtube.com` |

## 9. How to Open the Project

1. Install Cisco Packet Tracer 9.0.
2. Open the downloaded `.pkt` file.
3. Wait for the topology to load.
4. Check that the router and server links are active.
5. Test a customer device's IP configuration.
6. Test connectivity to the DNS server.
7. Open a configured demonstration website in the customer browser.

## 10. Important Notes

- The project is an educational network simulation.
- The websites are locally hosted demonstration pages.
- IP addresses shown in this guide are examples from the project addressing plan. Verify exact addresses and router interfaces in the final `.pkt` file.
- Some Packet Tracer services have simplified behavior compared with real production networks.
- Do not assume that a website is blocked simply because DNS resolution fails. Test the actual HTTP connection and verify the ACL counters.

## 11. Project Purpose

This project demonstrates how an ISP-style network can connect multiple customers, provide essential network services, manage website access, and maintain connectivity using routing and network management techniques.