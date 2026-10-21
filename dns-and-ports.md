# DNS and Ports: Study Notes

## What is DNS?
- DNS (Domain Name System) is the phonebook of the internet.
- People use domain names like nytimes.com or espn.com. Web browsers and servers communicate using IP addresses, so DNS translates names into IP addresses.
- Servers can't use the name. They only understand IP addresses.
- Each ISP (Internet Service Provider) has its own DNS servers.

### Simple flow
1. The user (web browser) sends the name of the website to DNS.
2. DNS gives back the IP address.
3. The browser sends its request to the server at that IP address.
4. The server sends back the website, and the browser shows it.

Try it: `nslookup www.google.com`

## What is a computer port?
- A port is a virtual point where network connections start and end.
- It is not a physical connection. Ports are software-based and managed by the computer's operating system.
- Each port is associated with a specific process or service, which lets a computer tell different kinds of traffic apart. For example, emails go to a different port than web pages.
- Ports are logical connections used by programs and services to exchange information.
- A port number always goes together with an IP address.
- Each port has a unique number from 0 to 65535.
- The IP address identifies the server, and the port number identifies which program or service on that server I want to use.

### Common examples
| Port | Used for |
|------|----------|
| 80 | Web pages (HTTP) |
| 443 | Web pages (HTTPS, secure) |
| 21 | FTP (File Transfer Protocol), used to transfer files over a network |
| 25 | Email (SMTP) |

## Port number ranges
Port numbers are assigned by IANA (Internet Assigned Numbers Authority) and split into three categories.

| Range | Category | Used for |
|-------|----------|----------|
| 0 to 1023 | System / well-known ports | Used on servers that our computer connects to. Examples: 80, 443, 25, 21 |
| 1024 to 49151 | User / registered ports | Can be registered by companies and developers for a particular service. Examples: 1102 Adobe Server, 1433 Microsoft SQL Server, 1416 Novell, 1527 Oracle |
| 49152 to 65535 | Dynamic / private ports | Client-side ports that our computer assigns temporarily to itself during a session. Free to use |

Example: when viewing a web page, the server uses port 80 (HTTP) or 443 (HTTPS), and my computer uses a dynamic port on its side.

## Netstat
- Short for network statistics.
- A command-line tool that displays current network connections and port activity on my computer.
- You don't see the IP address or the port number in the browser by default. If you want to see them, use netstat.

Try it: `netstat -an`
