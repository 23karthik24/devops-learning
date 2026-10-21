# Networking Notes: Day 1

## The four basics
- **IP address:** A number that identifies a device on a network, like a street address for a computer, so data knows where to go.
- **DNS:** The system that turns a human-friendly name like google.com into the IP address a computer needs to connect.
- **Port:** A numbered entry point on a machine that tells it which service the traffic is for. HTTP uses port 80 and HTTPS uses port 443.
- **HTTP/HTTPS:** The set of rules browsers and web servers use to request and send web pages. The "S" means the connection is encrypted, so others on the network can't read or change the data.

## What happens when I type a website address and press Enter
1. My browser sees the address (for example, google.com) but doesn't know where that server is.
2. It asks DNS to look up the name, and DNS replies with an IP address.
3. My computer connects to that IP address on port 443, the standard port for HTTPS.
4. Before any page data is sent, the browser and server set up encryption and the server proves its identity with a certificate.
5. The browser sends an HTTP request asking for the page.
6. The server sends back an HTTP response with a status code (200 means success) and the page content.
7. The browser reads the content (HTML, CSS, images) and shows the page. Each extra file may trigger more requests.

## Commands I tried
- "nslookup google.com": My home router (homerouter.cpe) answered the DNS question. It returned several IP addresses for google.com: IPv6 ones starting with 2a00:1450:400b:c02:: and IPv4 ones like 172.253.116.100, .101, .102, .113, .138 and .139. The answer was labelled "non-authoritative" because it came from a DNS cache, not directly from Google's own DNS server.
  
- "ping google.com": It resolved google.com to 172.253.116.100 and sent 4 packets. All 4 came back (0% loss), taking 32 ms to 45 ms, averaging 39 ms. So the server is reachable and the connection is fast.
  
- "curl.exe -I https://example.com": The first line was HTTP/1.1 200 OK, so the request succeeded (200 = success). The -I option asks for only the headers, not the page. The headers showed the content type (text/html; charset=utf-8), so the browser knows it's a web page, and the server was cloudflare, so the site is served through Cloudflare's network. The header cf-cache-status: HIT means the answer came from a cached copy rather than the original server, and the CF-RAY value ends in DUB, which suggests it was served from a Cloudflare location in Dublin.

## Things I noticed and questions I had
- Plain `curl` in PowerShell didn't work. It asked for a `Uri:` because PowerShell uses `curl` as a shortcut for a different command (Invoke-WebRequest). Using `curl.exe` runs the real curl tool.
- nslookup showed both long IPv6 addresses and short IPv4 addresses for the same name. Both are valid addresses for the same service. IPv4 is the older format and IPv6 is the newer, longer one.
- One name returned many IP addresses. Big services run many servers, and DNS can hand out different ones to spread the load.
- "Non-authoritative" means the answer came from a cache, not from the server that officially owns the domain's records.
