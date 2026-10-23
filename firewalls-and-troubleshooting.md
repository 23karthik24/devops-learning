# Firewalls and Troubleshooting: Day 3 Notes

## 1. What is a firewall?
A firewall is a security guard at the school gate. Everyone who wants to come in or go out must pass the guard. The guard has a list of rules, such as:
- Students with a uniform can come in.
- Strangers can't come in.
- Delivery people can only use the side door.

A firewall does the same with network traffic. It checks every message that tries to pass and asks:
- Where is it from? (source IP address)
- Where is it going? (destination IP address)
- Which door is it using? (port)
- What type is it? (protocol, like TCP)

If the traffic matches an "allow" rule, it passes. If it matches a "deny" rule, it's blocked.

**Real example:** I open youtube.com. My laptop's firewall sees traffic going out on port 443 and allows it. Someone on the internet tries to connect straight to my laptop on port 3389. The firewall blocks it, because I never asked for it.

**Two kinds**
- **Host firewall:** a firewall on my laptop, such as Windows Defender Firewall.
- **Network firewall:** a router at home, or a firewall in Azure.

## 2. Inbound and outbound
- **Inbound** means traffic coming in to my device.
- **Outbound** means traffic going out from my device.

**Normal setup:** outbound is allowed (I can visit any website), but inbound from strangers is blocked (nobody can walk in uninvited).

**Example**
- I open a website: my laptop sends a request outbound. Allowed.
- A stranger tries to connect to my laptop: that's inbound. Blocked.

## 3. Allow, deny and default deny
Imagine the guard has two ways of working.

**Default allow:** everyone can come in, except the people on the list. The problem is that I have to know every bad person in advance, and I'll miss some.

**Default deny:** nobody comes in, except the people on the list. This is much safer. If someone isn't on the list, they're stopped automatically. A good firewall uses default deny.

**Example:** I run a web server.
- Allow port 443 (HTTPS), so visitors can see my website.
- Everything else: deny.
- Even if a hacker tries port 22 or 3389, they're blocked.

Rules are checked from the top, and the first match wins.

## 4. Stateful and stateless
- A **stateful** firewall has a memory. If I start a connection to a website, the reply is allowed back automatically, with no extra rule. It's like the guard remembering a student who just left to buy sweets and letting them back in.
- A **stateless** filter has no memory. It checks every packet on its own, so rules are needed for both directions.
- The ESTABLISHED connections in my `netstat -an` output are the connections the firewall is tracking.

## 5. Firewalls in the cloud (Azure)
When I rent a computer in Azure, it's on the internet. Without protection, anyone could try to reach it. So Azure gives me a Network Security Group (NSG), which is the guard for my cloud computer.

An NSG rule is a list of fields:

| Field | Example |
|-------|---------|
| Priority | 100 (a lower number is checked first) |
| Source | Any, or my own IP |
| Port | 443 |
| Protocol | TCP |
| Action | Allow or Deny |

**Example:** I create a virtual machine. I add a rule: "Allow port 22 only from my home IP address." Now only I can log in, and strangers are blocked.

## 6. The troubleshooting ladder
Go bottom-up and stop at the first step that fails. That's where the problem is.

| Step | Question | Command |
|------|----------|---------|
| 1 | Is the device connected? | Check Wi-Fi |
| 2 | Do I have an IP address and gateway? | `ipconfig` |
| 3 | Can I reach my router? | `ping 192.168.8.1` |
| 4 | Can I reach the internet by IP? | `ping 8.8.8.8` |
| 5 | Does DNS work? | `ping google.com` |
| 6 | Does the service answer on its port? | `Test-NetConnection google.com -Port 443` |
| 7 | Is a firewall blocking it? | Check firewall rules |

- If step 4 works but step 5 fails, DNS is the problem.
- If steps 1 to 5 work but step 6 fails, the service or a firewall is the problem.

## Commands I tried
- `ipconfig`: Step 2 passed. My Wi-Fi adapter had a private IPv4 address (192.168.8.x), a subnet mask of 255.255.255.0 (/24) and a default gateway at 192.168.8.1. The other adapters showed "Media disconnected" because they aren't in use.
- `ping 192.168.8.1`: Step 3 passed. My router replied to all 4 packets with 0% loss, in about 3 to 4 ms. That's fast because it's the next device on my own network.
- `ping 8.8.8.8`: Step 4 passed. 0% loss, about 25 to 54 ms (average 38 ms). So I can reach the internet by IP address, with DNS not involved.
- `ping google.com`: Step 5 passed. The name resolved to an IP address, 0% loss, average 34 ms. So DNS works.
- `tracert google.com`: The trace took 9 hops and completed with no `*` lines. Hop 1 was my router (192.168.8.1). Hops 2 to 7 were private-range addresses (10.x.x.x and 172.24.x.x), which look like my ISP's internal network. Hop 8 was a public address and hop 9 was Google. One reading in hop 2 jumped to 240 ms once, which is a normal blip.
- `Test-NetConnection google.com -Port 443`: Step 6 passed. TcpTestSucceeded was True, so the HTTPS port answers.
- `Test-NetConnection google.com -Port 25`: TcpTestSucceeded was False and PowerShell tried several addresses (IPv4 and IPv6) and failed on all of them. But PingSucceeded was True, so the server itself is reachable and only port 25 isn't answering. This is probably a firewall rule, either my ISP blocking outbound port 25 (common, to stop spam) or Google not accepting connections there.

**Result:** every step of the troubleshooting ladder passed, so my connection is healthy.

## Windows Defender Firewall
- I opened Windows Defender Firewall with Advanced Security and looked at Inbound Rules. Each rule shows a Name, whether it's Enabled, an Action (Allow or Block), a Protocol and a Local Port, which are the same fields an Azure NSG rule uses.
- This is the host firewall from section 1, running on my own laptop.

## What I noticed
- Day 2 links to Day 3: the traceroute hops starting with 10. and 172.24. are private ranges, so my ISP uses private addressing inside its own network too.
- Ping alone isn't enough. A host can answer ping while one specific port is still blocked, as with port 25. This is why the ladder has a step 6.

## What confused me
- Port 25 failed even though ping worked. I had to think about why a reachable host can still refuse one port.
