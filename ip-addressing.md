# IP Addressing: Day 2 Notes

## 1. IPv4 address
- All devices in a network have a unique IP address.
- An IP address is four numbers separated by dots, each from 0 to 255, like 192.168.1.10.
- Each number is called an octet because it's 8 bits. Four octets make 32 bits, which gives about 4.3 billion possible addresses.
- That's not enough for every device, which is the reason private addresses and NAT exist.

### Network part and host part
In 192.168.1.x, the first 3 bytes (24 bits) represent the network and the last byte (8 bits) represents the host.

- A single network of this size can have host numbers from 0 to 255, so 256 addresses in total.
- 192.168.1.0 is the **network address** and can't be given to a device.
- 192.168.1.255 is the **broadcast address** and can't be given to a device. A message sent to it goes to every system in the network.
- So the usable host addresses are 192.168.1.1 to 192.168.1.254, which is **254 usable hosts**.

## 2. Public vs private addresses
**Public**
- Unique on the internet. My ISP gives my router one, and websites see it.
- Used in a WAN, recognised over the internet, assigned by the ISP (Internet Service Provider), and not free.

**Private**
- Used only inside a home or company network (LAN). Many networks reuse the same ones, which is fine because they aren't routed on the internet.
- Not recognised over the internet, assigned by the LAN administrator (or the router), and free.

**Private ranges**
- 10.0.0.0 to 10.255.255.255
- 172.16.0.0 to 172.31.255.255
- 192.168.0.0 to 192.168.255.255

Rule to remember: an address starting with 172 is private only if the second number is 16 to 31. So 172.20.5.9 is private and 172.40.5.9 is public.

**Example:** my laptop (192.168.1.1), phone (192.168.1.2) and TV (192.168.1.3) all have private addresses. They go through the router, which has one public address facing the internet.

## 3. Subnets and CIDR
- A subnet is a smaller group of addresses inside a network. The subnet mask shows which bits are the network and which are the host.
- CIDR writes the mask as a number. For example, /24 means 24 network bits, which is the mask 255.255.255.0.
- Addresses in a subnet = 2 to the power of (32 minus the slash number). Two are reserved (network and broadcast).

| CIDR | Device bits | Addresses |
|------|-------------|-----------|
| /16 | 16 | 65,536 |
| /24 | 8 | 256 |
| /28 | 4 | 16 |
| /30 | 2 | 4 |

- A bigger slash number means a smaller subnet. Each +1 halves the size.

## 4. Default gateway
- My device knows how to talk directly to other devices in its own subnet.
- For anything outside it, such as a website, it sends the traffic to the default gateway, which is my router.
- At home that's usually something like 192.168.1.1.

## 5. NAT (Network Address Translation)
- My phone, laptop and TV each have a private address, but my home has one public address.
- The router rewrites outgoing traffic so it appears to come from the public address, and it keeps track of which reply belongs to which device.
- This is how billions of devices share a limited pool of public addresses.

## 6. MAC address
- All networking devices have a NIC (Network Interface Card), which is where the Ethernet cable connects.
- A MAC (Media Access Control) address is a unique identifier assigned to a NIC by the manufacturer.
- Layer 2 uses MAC addresses. Layer 3 uses IP addresses.

## 7. Switches and ARP
In a star topology, all the systems in a network are connected to one switch, which helps one system connect to another.

- A switch works at the Data Link layer (Layer 2) of the OSI model, so it only knows MAC addresses. It doesn't understand IP addresses.
- If system A wants to send a message to system C, it tells the switch to send to C with C's IP address. The switch doesn't know about IP addresses, so A first needs C's MAC address.

**ARP (Address Resolution Protocol)** solves this:
1. A creates an ARP request that says, in effect: "Who has this IP address? Tell me your MAC address. My MAC address is (A's MAC), reply to this MAC address."
2. The request is sent to all systems in that network. This is known as a **broadcast**.
3. The system with that IP address replies with its MAC address.
4. A can now send the data to C using its MAC address.

## Commands I tried
- "ipconfig": My active adapter was Wi-Fi. My IPv4 address was in the 192.168.8.x range, which is private. My subnet mask was 255.255.255.0, which is /24, so my network has 256 addresses (254 usable). My default gateway was my router at 192.168.8.1. The other adapters (Ethernet, Bluetooth, Wi-Fi Direct) showed "Media disconnected" because they aren't in use. The Wi-Fi adapter also had IPv6 addresses, including one starting with fd22, which is a private IPv6 address.
- "ipconfig /all": DHCP was enabled and the DHCP server was my router (192.168.8.1). My router gave my laptop its address automatically, and the address is only leased for a set time. The DNS servers were the router's IPv4 and IPv6 addresses, which matches the router that answered my nslookup on Day 1.
- "curl.exe ifconfig.me": It printed my public address, which was completely different from my private 192.168.8.x address. The router uses NAT to translate between them. (Public address not recorded here.)
- netstat -an": My connections went from my private address (192.168.8.x) to public addresses, mostly on remote port 443 (HTTPS). The local ports on my side were high numbers such as 49422, 52232 and 55554, which are in the dynamic range (49152 to 65535). The states included ESTABLISHED (active connection), TIME_WAIT and CLOSE_WAIT (connections closing) and LISTENING (my computer waiting for incoming connections). Addresses starting with 127.0.0.1 are my own computer talking to itself (localhost).

## What confused me
- Working out the number of addresses in a subnet. I mixed up network bits and device bits at first.
- Why 172.40.5.9 is public even though it starts with 172.
- What DHCP is and why an address has a lease.
