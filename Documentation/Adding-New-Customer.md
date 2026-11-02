# Adding a New Customer or Device — Jio ISP Network Simulator

## 1. Introduction

This guide explains how to connect a new customer device to the Jio ISP simulated network.

The project supports two types of customer connectivity:

1. Wired connectivity using a PC and switch.
2. Wireless connectivity using a laptop and wireless access point.

A new device must connect to the correct customer network and receive valid network settings before it can access the ISP services.

## 2. Before You Start

Check the following:

- The Packet Tracer topology is open.
- The customer's router and switch are powered on.
- The relevant network links are active.
- DHCP is configured for the customer's network, if you intend to use automatic addressing.
- The correct gateway and DNS server are configured in the customer's DHCP pool.

Example customer networks:

| Customer   | Network           |
| ---------- | ----------------- |
| Customer A | `192.168.10.0/24` |
| Customer B | `192.168.20.0/24` |
| Customer C | `192.168.30.0/24` |

Use the actual customer network you want to connect to.

# PART A — Adding a Wired Customer Device

## 3. Devices Required

- One PC
- One customer switch
- One Ethernet cable
- An available switch port

## 4. Physical Connection

### Step 1: Add a PC

1. Open Cisco Packet Tracer.
2. Select **End Devices**.
3. Drag a PC into the workspace.
4. Give it a clear name, for example:

`Customer-B-PC-02`

### Step 2: Connect the PC to the switch

1. Select **Connections** (the lightning icon).
2. Choose **Copper Straight-Through**.
3. Click the PC.
4. Select `FastEthernet0`.
5. Click the customer switch.
6. Select an available Ethernet port, such as `FastEthernet0/3`.

The connection should look like:

```text
Customer PC
    |
    | Ethernet cable
    |
Customer Switch
    |
    |
Customer Router
    |
Jio ISP Network
```

The exact router and switch connections depend on the existing topology.

### Step 3: Obtain an IP address

1. Click the new PC.
2. Open **Desktop**.
3. Select **IP Configuration**.
4. Select **DHCP**.
5. Wait for the PC to receive its network settings.

The PC should receive:

- An IP address from the correct customer subnet.
- The correct subnet mask.
- The customer gateway.
- The DNS server address.

For example, a Customer B PC may receive an address such as `192.168.20.21`, provided that address is available in the configured DHCP pool.

### Step 4: Verify the connection

Open the PC's **Desktop → Command Prompt**.

Run:

```text
ipconfig
```

Test the default gateway:

```text
ping 192.168.20.1
```

The gateway address above is an example for Customer B. Use the actual gateway for the selected customer.

Test the DNS server:

```text
ping 192.168.40.10
```

Test domain resolution:

```text
nslookup www.youtube.com
```

Finally, open **Desktop → Web Browser** and enter a configured website address, such as:

```text
http://www.youtube.com
```

If the website does not open, check the IP configuration, routing, DNS, HTTP service, and ACL rules.

# PART B — Adding a Wireless Customer Device

## 5. Devices Required

- One laptop
- One wireless access point
- One customer switch
- One Ethernet cable for the access point
- A configured wireless network name (SSID)
- A wireless security password, if security is enabled

## 6. Connect the Access Point

### Step 1: Add an access point

1. Open **Network Devices**.
2. Select **Wireless Devices**.
3. Drag an Access Point into the workspace.
4. Rename it, for example:

`AP-Customer-B`

### Step 2: Connect the access point to the switch

1. Select **Connections**.
2. Choose **Copper Straight-Through**.
3. Click the access point.
4. Select its Ethernet port.
5. Click the customer switch.
6. Select an available switch port.

The access point must connect to the intended customer network.

```text
Laptop
   )))
   ))) Wireless connection
   )))
Access Point
    |
    | Ethernet cable
    |
Customer Switch
    |
Customer Router
    |
Jio ISP Network
```

## 7. Configure the Wireless Network

1. Click the access point.
2. Open its **Config** tab.
3. Select the wireless interface, if available.
4. Configure the wireless network name (SSID).

Example:

```text
SSID: JIO-CUSTOMER-B
```

5. If your access point supports wireless security, select the security mode supported by the device.
6. Configure a wireless password or pre-shared key (PSK).
7. Save or apply the settings.

Use a strong password for your demonstration network. Do not publish a real personal or reused password in GitHub screenshots or documentation.

The exact menu names may differ depending on the Packet Tracer access point model.

## 8. Connect the Laptop

### Step 1: Add a laptop

1. Open **End Devices**.
2. Drag a Laptop into the workspace.
3. Rename it, for example:

`Customer-B-Laptop-01`

### Step 2: Check wireless capability

1. Click the laptop.
2. Open the **Physical** tab.
3. Check whether a wireless network adapter is installed.
4. If the selected laptop model requires a wireless module, power the laptop off, install a compatible module such as `WPC300N` in the supported slot, and power it back on.

Do not remove or change modules while the laptop is powered on.

### Step 3: Join the wireless network

1. Open the laptop's **Desktop** tab.
2. Open **PC Wireless** or the available wireless configuration utility.
3. Find the configured SSID.
4. Select `JIO-CUSTOMER-B`.
5. Enter the configured password if security is enabled.
6. Connect to the network.

The laptop should now have a wireless connection to the access point.

### Step 4: Obtain an IP address

1. Open **Desktop → IP Configuration**.
2. Select **DHCP**.
3. Wait for the laptop to receive an IP address.

For example, a Customer B laptop should receive an address from the `192.168.20.0/24` network.

### Step 5: Verify the wireless connection

Open **Desktop → Command Prompt**.

Run:

```text
ipconfig
```

Test the gateway:

```text
ping 192.168.20.1
```

Test the DNS server:

```text
ping 192.168.40.10
```

Test DNS resolution:

```text
nslookup www.youtube.com
```

Open the Web Browser and test a configured website:

```text
http://www.youtube.com
```

Use the actual gateway and DNS addresses configured in your project.

# PART C — Common Troubleshooting

## 9. Wired Device Problems

| Problem             | What to check                              |
| ------------------- | ------------------------------------------ |
| No link light       | Check the cable and switch port            |
| No IP address       | Check DHCP service and pool                |
| Cannot ping gateway | Check VLAN, cabling, interface and gateway |
| DNS not working     | Check DNS server address and DNS service   |
| Website not opening | Check HTTP service, DNS, routing and ACL   |

## 10. Wireless Device Problems

| Problem               | What to check                             |
| --------------------- | ----------------------------------------- |
| SSID not visible      | Check AP wireless settings                |
| Laptop cannot connect | Check wireless adapter and SSID           |
| Password rejected     | Check the configured security password    |
| No IP address         | Check AP-to-switch connection and DHCP    |
| Website not opening   | Check gateway, DNS, routing, HTTP and ACL |

## 11. Important Points

- A wired PC connects to a switch using an Ethernet cable.
- A wireless laptop connects to an access point using Wi-Fi.
- The access point connects to the customer switch using Ethernet.
- Both wired and wireless devices should use the correct customer network.
- DHCP can automatically provide the IP configuration.
- DNS must be configured correctly for domain-based website access.
- A new device should not be connected to a different customer's network unless that is intentional.

## 12. Final Verification Checklist

- [ ] Device is connected to the correct switch or access point.
- [ ] Device receives an IP address.
- [ ] Device has the correct default gateway.
- [ ] Device has the correct DNS server.
- [ ] Gateway ping succeeds.
- [ ] DNS lookup succeeds.
- [ ] Configured demonstration website opens.

This completes the process of adding a new wired or wireless customer device to the Jio ISP Network Simulator.
