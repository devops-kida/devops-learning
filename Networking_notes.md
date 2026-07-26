# 💻💻💻 Networking Notes  
- **CDN** (Content delivery network) : server which serve static data like images and videos across servers worldwide to reduce latency and improve speed. CDN caches copies of your content across multiple servers worldwide.  
When a user requests your site, the CDN serves it from the server closest to them, reducing latency and improving speed.

- **Cache**: temprary stoeage for static data.  
- **Edge location** :  Specific sites where cached data is stored
  A CDN is the global system; edge locations are the local points of presence that make the CDN effective.
  A CDN is the entire global system of distributed servers. An edge location is a specific physical data center within the CDN where content is cached and served.

  ----------

## Internet Protocal
  There is total 4.3 billion ipv4 address. Ipv4 is limited and not enough for all the devices thats why we have ipv6 address.  
  ipv6 128 bits alphanumeric number consist of Eight groups of four hexadecimal digits separated by colons.but solves IPv4 exhaustion  

  ipv6 is a bit complex and difficul to rememeber. Hence they find a way to to continue using ipv4 address.  
  This is how concept of subneting is intoducesd.  

1. **IPv4 Structure — The Basics**  
   An IPv4 address has 4 numbers (octets) separated by dots:  
    ```bash
    192  .  168  .  1  .  10
    ```
    Each octet is 8 bits, ranging from 0 to 255 (since 8 bits = 256 possible values, 0–255).  
    Total = 32 bits = 4 × 8 bits.  
    
    Binary View (Important!)
    ``` bash
    192.168.1.10  →  11000000.10101000.00000001.00001010
    ```
  
    Quick conversion table (memorize this — it's the "magic numbers"):  
    `Bit position	128	64	32	16	8	4	2	1`
    Any octet value is a sum of these. E.g., 192 = 128 + 64.

2. Network Portion vs Host Portion  
- Every IP address is split into two parts:

  **Network portion** — identifies which network the device belongs to
  **Host portion** — identifies which specific device within that network
  
  This split is defined by the Subnet Mask.
  
  Example:
  
  IP:          192.168.1.10
  Subnet Mask: 255.255.255.0
  
  This means the first 3 octets (192.168.1) = network, last octet (10) = host.  
  So all devices from 192.168.1.1 to 192.168.1.254 are on the same local network and can talk directly without a router.

3. Public vs Private IP Addresses
- Private IP ranges (reserved — not routable on the internet):
  |    Range    |	  CIDR	    |  Typical Use    |
  |   ---------| --------      | -------        |
  | 10.0.0.0 – 10.255.255.255 |	10.0.0.0/8 |	Large corporate/cloud networks (AWS VPCs) |
  |172.16.0.0 – 172.31.255.255 |	172.16.0.0/12  |	Docker default networks  |
  |192.168.0.0 – 192.168.255.255 |	192.168.0.0/16 |	Home routers, small office LANs |
  
- Public IPs  
  Everything else is potentially a public IP, routable directly on the internet (e.g., your server's public IP, a website's IP).  
  Practical relevance: Your EC2 instance has a private IP (for internal VPC communication) and often a public IP or Elastic IP (for internet access). Security groups and NAT gateways manage the boundary between them.


- **Subnet:**  Divides large networks into smaller ones for efficient management
  
- ----------------- 

## CIDR (Classless Inter-Domain Routing):

Compact way to represent IP ranges and masks (e.g., /28 gives 16 addresses, 14 usable.   
2<sup>32-n</sup>
if n=4 the CIDR notation will be /4
if n=28 the cidr notation will be /28, total number of ip address will be 16  
    Network address : first ip address in that subnet  
    Broadcast address : last ip address in that subnet  
    only 14 ip address will be available for use for VIDR notation /28  
e.g: for subnet 192.168.1.0/28  
* Network ip address : 192.168.1.0  
* broadcast ip address :192.168.1.15  
* First Usable IP:192.168.1.1  
* Last Usable IP : 192.168.1.14
* Subnet mask : 255.255.255.240


- Instead of writing 255.255.255.0, DevOps tools use CIDR (Classless Inter-Domain Routing) notation:

  `192.168.1.0/24`
  
  The /24 means 24 bits are the network portion, and the remaining 32 - 24 = 8 bits are for hosts.
  
 | CIDR	| Subnet Mask	|  Hosts Available	| Network/Host bits  | Common Use |
 | ---- | --------    | -----------        | --------    |  ------------  |
 |  /8	  |  255.0.0.0	|  ~16 million	  | 8 network bit & 24 host bits | Huge corporate networks|
 |/16	  |  255.255.0.0	|  ~65,000	  |  16 Network & 16 host bits | AWS VPC default  |
 | /24	|  255.255.255.0  |	  254	  |  24 Network & 8 Host bits |Typical subnet/office LAN  |
 | /28	|  255.255.255.240  |	  14	  |  28 network & 4 host bits |Small subnet (e.g., a small AWS subnet)  |
 | /32	|  255.255.255.255	|    1	  |  32 Network & 0 host bits | A single specific host  |

## 🤝 OSI(open system interconnection) Model:  
1. **Physical layer**  
   e.g :  wifi, repeater, hub, lan etc.
2. **data link layer**  
   e.g : MAc address, bridgge, NIC
3. **Network layer**  
   work on IP protocal e.g Router
4. **Transport layer**  
    TCP and UDP protocal, data transfer in packets
   TCP use three handshake model to tranfer the data.
     1. client sync with server
     2. server acknowledge the sync
     3. server send scknowledgement to the client
    SSH use TCP in background to connect the server securily
        
6. **Session layer**
7. **Presentation layer**
8. **Application Layer**
    
   ---
## DNS(Domain name System):
Step 01 — Browser requests a URL and the local cache is checked first  
Step 02 — If not in cache, the hosts file is checked  
Step 03 — If still not found, the query goes to the ISP's DNS Resolver  
Step 04 — The resolver does a recursive search: Root Server → TLD Server → Authoritative Name Server, collecting the real IP  
Step 05 — The DNS Resolver returns the final IP to the computer  

![dns working](Images/Working-of-dns.PNG)

1. User Request  
When we type a domain name like https://www.geeksforgeeks.org/ into our browser, our computer starts the process of finding the corresponding IP address needed to connect to the website.

2. Check Local Cache  
The first place our system looks is in its local cache, which may include:  
. Browser Cache  
. Operating System (OS) Cache  
. Router Cache

3. Check Host Files  
If the IP address is not in the local cache, the system may check host files, which are manually configured mappings of domain names to IP addresses. This is rare in modern systems, but it might still be used for certain network configurations.

4. Query DNS Resolver  
If no IP address is found locally, the request is sent to a DNS Resolver. The Resolver is a server provided by our Internet Service Provider (ISP) or a public DNS service like Google DNS (8.8.8.8) or Cloudflare (1.1.1.1). The Resolver acts as the intermediary that communicates with various DNS servers to find the IP address.

5. Contact the Root Server  
Resolver first contacts the Root DNS Server which is the starting point for DNS lookups. The Root server doesn’t know the exact IP address of geeksforgeeks.org but directs the query to the Top-Level Domain (TLD) Server responsible for .org.

6. Query TLD Server  
Resolver sends the query to the TLD Server for .org domains. The TLD server handles domain names ending in .org and knows where to find the authoritative nameserver for geeksforgeeks.org.

7. Query the Authoritative Server  
The Resolver then queries the authoritative nameserver for geeksforgeeks.org. This server is responsible for storing DNS records for the domain, including the mapping of the domain name to its IP address.

8. Retrieve the IP Address  
Authoritative nameserver responds to the Resolver with the exact IP address (e.g., 192.0.2.1) for geeksforgeeks.org.

9. Return IP Address to Computer  
Resolver receives the IP address from the authoritative nameserver and sends it back to our computer. At this point, our computer knows how to connect to the website.

----------------------------------

## DNS Record  
A DNS record is just a piece of information stored in the Domain Name System (DNS) that tells the internet how to handle requests for a domain name.

- A Record (Address Record) : Maps a domain name to an IPv4 address.  
  When you type example.com in a browser, the resolver looks up its A record to get the IPv4 address to connect to.
  A domain can have multiple A records (round-robin DNS) for basic load distribution — e.g., pointing to several server IPs.
  
- AAAA Record ("Quad-A") : Same purpose as A, but maps a domain to an IPv6 address.
  A domain can have both an A and AAAA record simultaneously — clients prefer IPv6 if available (dual-stack), falling back to IPv4 if not.

- CNAME Record (Canonical Name): It’s a type of DNS record that creates an alias, Points a domain name to another domain name (not directly to an IP).
  Instead of mapping directly to an IP (like an A record), it says: “If someone asks for alias.example.com, go look up target.example.com instead.”  
  ` www.example.com.    CNAME    example.com.` or www.example.com → example.com

  **Important rule**: A CNAME record cannot coexist with any other record type on the same name (e.g., you can't have both a CNAME and an MX record on blog.example.com)

- MX Record (Mail Exchange) : Specifies which mail servers handle email for the domain, and in what priority order.
  ```bash
  example.com.    MX    10    mail1.example.com.
  example.com.    MX    20    mail2.example.com.
  ```

  The number (10, 20) is the priority — lower number = higher priority.
  MX records point to a hostname, which must itself resolve via an A/AAAA record — it can never point directly to an IP.
  mail1.example.com. & mail2.example.com. is hostname, a subdomain created to handle the mail server.

- NS Record (Name Server) : Specifies the name servers that manage the domain.  
  1. These are set at your domain registrar (GoDaddy, Namecheap, etc.) to delegate control to your DNS hosting provider (Route 53, Cloudflare, Google Cloud DNS).
  2. Usually there are 2–4 NS records for redundancy — if one name server is down, others still answer.
  3. This is the very first thing resolved in the DNS chain: root servers → TLD servers (.com) → your domain's NS servers → then finally your A/CNAME/MX records are fetched from there.
  
  
   




  
