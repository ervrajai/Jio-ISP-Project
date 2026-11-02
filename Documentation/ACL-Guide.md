# Website Access Control — ACL Guide
## Jio ISP Network Simulator

## 1. Introduction

An Access Control List (ACL) is a set of rules used by routers to permit or deny network traffic.

In this project, ACLs demonstrate how a Jio ISP administrator can control website access for individual customers.

For example:

- Customer A can access YouTube.
- Customer B can be blocked from accessing YouTube.
- Customer C can continue accessing other websites.

This demonstrates customer-level website access management.

## 2. Example Network Information

The following example will be used throughout this guide.

| Component | Example IP / Network |
|---|---|
| Customer A | `192.168.10.0/24` |
| Customer B | `192.168.20.0/24` |
| Customer C | `192.168.30.0/24` |
| DNS Server | `192.168.40.10` |
| YouTube Demo Server | `192.168.40.15` |

These are example addresses from the project. Verify that your DNS record and YouTube server actually use these addresses.

## 3. How ACL Works

Suppose Customer B tries to open:

```text
http://www.youtube.com
```

The DNS server resolves the domain to the YouTube demonstration server IP.

```text
www.youtube.com
       |
       v
DNS Server
192.168.40.10
       |
       v
192.168.40.15
YouTube Demo Server
```

The customer's traffic reaches the router. The router checks the ACL rules.

- If the rule permits the traffic, it can continue.
- If the rule denies the traffic, the router drops it.
- If no rule denies the traffic and the ACL has a final permit rule, the traffic can continue.

**Important:** The ACL example below blocks traffic to the YouTube server IP, not the text of the domain name. If multiple websites share one IP address, an IP-based ACL cannot reliably distinguish them by domain.

# PART A — Allow All Website Access

## 4. Remove the Existing Website Block

Suppose Customer B was previously blocked from YouTube.

To restore normal access, remove the ACL from the customer-facing interface.

First, check the existing ACL:

```text
show access-lists
```

Check the router interfaces:

```text
show ip interface brief
```

Find the interface that receives traffic from Customer B.

Enter configuration mode:

```text
enable
configure terminal
```

Enter the actual Customer B-facing interface. For example, if it is GigabitEthernet0/2:

```text
interface gigabitEthernet0/2
no ip access-group BLOCK-YOUTUBE in
exit
```

If your existing ACL has a different name or is numbered, use that exact ACL attachment instead.

If you want to remove the named ACL completely:

```text
no ip access-list extended BLOCK-YOUTUBE
```

Save the configuration:

```text
end
write memory
```

## 5. Test Website Access

On the Customer B PC:

1. Open Desktop.
2. Open Web Browser.
3. Enter:

```text
http://www.youtube.com
```

If DNS, routing, and HTTP are working, the demonstration website should open.

You can also test other configured websites.

# PART B — Block a Website for One Customer

## 6. Example: Block YouTube for Customer B

Our goal:

- Customer B cannot access YouTube.
- Customer B can access other permitted websites.
- Customer A and Customer C remain unaffected by this customer-specific rule.

We will create an extended named ACL.

### Step 1: Open the router CLI

Click the router that receives Customer B's traffic.

Open the CLI and enter:

```text
enable
configure terminal
```

### Step 2: Create the ACL

Copy the following commands:

```text
ip access-list extended BLOCK-YOUTUBE
deny tcp 192.168.20.0 0.0.0.255 host 192.168.40.15 eq 80
deny tcp 192.168.20.0 0.0.0.255 host 192.168.40.15 eq 443
permit ip any any
exit
```

Explanation:

| Command | Purpose |
|---|---|
| `ip access-list extended BLOCK-YOUTUBE` | Creates an extended ACL |
| `deny tcp` | Denies matching TCP traffic |
| `192.168.20.0 0.0.0.255` | Matches Customer B |
| `host 192.168.40.15` | Matches the YouTube demo server |
| `eq 80` | Matches HTTP traffic |
| `eq 443` | Matches HTTPS traffic |
| `permit ip any any` | Allows all other IP traffic |

Port 443 is included for HTTPS. The HTTP service in your Packet Tracer project may only demonstrate port 80.

### Step 3: Apply the ACL

The ACL must be applied to the interface where Customer B's traffic enters the router.

Check the router interfaces:

```text
show ip interface brief
```

For example, if Customer B connects through GigabitEthernet0/2:

```text
configure terminal
interface gigabitEthernet0/2
ip access-group BLOCK-YOUTUBE in
exit
end
write memory
```

**Do not blindly copy `gigabitEthernet0/2`.** Replace it with the actual interface connected to Customer B's network. Applying the ACL to the wrong interface can affect the wrong traffic or have no effect.

### Step 4: Verify the ACL

Run:

```text
show access-lists
```

You should see the ACL rules and, after matching traffic is generated, their match counters.

### Step 5: Test from Customer B

On Customer B's PC:

1. Open Desktop.
2. Open Web Browser.
3. Enter:

```text
http://www.youtube.com
```

The YouTube demonstration page should no longer be reachable if the ACL is applied correctly and the server address matches.

Now test another permitted website, such as:

```text
http://www.google.com
```

It should still work if its server IP is different and the rest of the network is functioning.

# PART C — Restore YouTube Access

## 7. Remove the YouTube Block

If you want to allow Customer B to access YouTube again, remove the ACL attachment.

Enter:

```text
enable
configure terminal
interface gigabitEthernet0/2
no ip access-group BLOCK-YOUTUBE in
exit
end
write memory
```

Replace the example interface with the actual Customer B-facing interface.

You can then test:

```text
http://www.youtube.com
```

If the network and web server are working, access should be restored.

Alternatively, you can edit the ACL to remove only the two YouTube deny rules while keeping other rules. Check the ACL contents before making changes.

# PART D — Understand the Difference

## 8. Allow vs Block

| Action | Result |
|---|---|
| Remove the ACL attachment | The ACL no longer filters traffic on that interface |
| Add a deny rule | Matching traffic is blocked |
| Add a permit rule | Matching traffic is allowed |
| Add `permit ip any any` | All other IP traffic is permitted by that ACL |
| Apply ACL `in` | Filters traffic entering the selected router interface |

Remember: An ACL is processed from top to bottom. The first matching rule decides the result.

If traffic does not match any rule, an implicit deny at the end of the ACL blocks it. That is why the example includes:

```text
permit ip any any
```

## 9. Useful ACL Commands

| Purpose | Command |
|---|---|
| View all ACLs | `show access-lists` |
| View interface details | `show ip interface` |
| View router interfaces | `show ip interface brief` |
| Enter configuration mode | `configure terminal` |
| Create extended ACL | `ip access-list extended ACL-NAME` |
| Apply ACL inbound | `ip access-group ACL-NAME in` |
| Remove ACL attachment | `no ip access-group ACL-NAME in` |
| Delete named ACL | `no ip access-list extended ACL-NAME` |
| Save configuration | `write memory` |

## 10. Important Notes and Limitations

1. The example blocks TCP traffic to one server IP on ports 80 and 443.
2. DNS queries are not blocked by these two TCP rules because DNS normally uses UDP port 53, and sometimes TCP port 53.
3. DNS resolution can still succeed even when the website connection is blocked.
4. If a website uses a different server IP, the ACL must be updated with that IP.
5. If multiple websites share one server IP, blocking that IP can affect all of them.
6. The ACL must be applied to the correct customer-facing router interface.
7. An ACL does not create a web filter that understands website names. It filters network traffic using addresses, protocols, and ports.
8. Verify the result with `show access-lists` and a browser test.

## 11. Demonstration Scenario for Project Presentation

### Scenario 1 — Website Access Allowed

1. Open Customer B's PC.
2. Open its browser.
3. Visit `www.youtube.com`.
4. Show that the website opens.

### Scenario 2 — Website Access Blocked

1. Open the customer-facing router CLI.
2. Configure and apply the `BLOCK-YOUTUBE` ACL.
3. Return to Customer B's PC.
4. Refresh the YouTube page.
5. Show that the page is no longer reachable.
6. Run `show access-lists` to show matching rule counters.

### Scenario 3 — Website Access Restored

1. Remove the ACL attachment from the customer-facing interface.
2. Return to Customer B's PC.
3. Refresh the YouTube page.
4. Show that access is restored.

## 12. Conclusion

This ACL demonstration shows how an ISP administrator can control customer network traffic.

The Jio ISP Network Simulator uses extended ACL rules to demonstrate website access restrictions and restoration for individual customer networks.