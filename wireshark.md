# Wireshark Cheatsheet for Blue Team Analysts

Wireshark is a GUI-based packet analyzer used for **live capture**, **PCAP analysis**, and **protocol inspection**.

---

## Capture vs Display Filters

| Filter Type      | Usage Purpose                                              |
|------------------|------------------------------------------------------------|
| **Capture Filter** | Applies while **capturing traffic** (e.g., `tcp port 80`) |
| **Display Filter** | Applies when **viewing/analyzing packets** (e.g., `http`)  |

🔹 **Capture filters** use **libpcap syntax** (like TCPDump)  
🔹 **Display filters** use **Wireshark's own syntax**

---

## Resources

- [Wireshark Display Filter Reference](https://www.wireshark.org/docs/dfref/)
- [Wireshark Capture Filters](https://wiki.wireshark.org/CaptureFilters)
- [Wireshark Filtering Expressions](https://wiki.wireshark.org/DisplayFilters)

---

## IP Filters (Display)

| Filter                            | Description                                         |
|-----------------------------------|-----------------------------------------------------|
| `ip`                              | Show all IP packets                                 |
| `ip.addr == 10.10.10.111`         | Packets with that IP as src or dst                 |
| `ip.addr == 10.10.10.0/24`        | Packets with IPs in the subnet                     |
| `ip.src == 10.10.10.111`          | Packets **from** IP 10.10.10.111                   |
| `ip.dst == 10.10.10.111`          | Packets **to** IP 10.10.10.111                     |

🔹 `ip.addr` matches **either direction**  
🔹 `ip.src` and `ip.dst` match by **direction**

---

## TCP Filters

| Filter                  | Description                                  |
|--------------------------|----------------------------------------------|
| `tcp.port == 80`         | TCP packets with port 80 (either src or dst) |
| `tcp.srcport == 1234`    | TCP packets from source port 1234            |
| `tcp.dstport == 80`      | TCP packets to destination port 80           |

---

## UDP Filters

| Filter                  | Description                                  |
|--------------------------|----------------------------------------------|
| `udp.port == 53`         | UDP packets with port 53                     |
| `udp.srcport == 1234`    | UDP packets from source port 1234            |
| `udp.dstport == 5353`    | UDP packets to destination port 5353         |

---

## HTTP Filters

| Filter                             | Description                             |
|------------------------------------|-----------------------------------------|
| `http`                             | All HTTP traffic                        |
| `http.response.code == 200`        | HTTP responses with code 200 (OK)       |
| `http.request.method == "GET"`     | All HTTP GET requests                   |
| `http.request.method == "POST"`    | All HTTP POST requests                  |

---

## DNS Filters

| Filter                           | Description                         |
|----------------------------------|-------------------------------------|
| `dns`                            | All DNS packets                     |
| `dns.flags.response == 0`        | DNS queries                         |
| `dns.flags.response == 1`        | DNS responses                       |
| `dns.qry.type == 1`              | DNS A record queries                |

---

## Advanced Filters

### `contains`

| Type       | Comparison Operator |
|------------|----------------------|
| Usage      | `http.server contains "Apache"` |
| Purpose    | Case-sensitive search inside a specific field |

---

### `matches`

| Type       | Regex Match (case-insensitive) |
|------------|-------------------------------|
| Usage      | `http.host matches "\.(php|html)"` |
| Purpose    | Match using regex (e.g., all PHP or HTML pages) |

---

### `in`

| Type       | Set Membership |
|------------|----------------|
| Usage      | `tcp.port in {80 443 8080}` |
| Purpose    | Check if a field is part of a set |

---

### `upper`

| Type       | Function |
|------------|----------|
| Usage      | `upper(http.server) contains "APACHE"` |
| Purpose    | Convert field to uppercase before filtering |

---

### `string`

| Type       | Function |
|------------|----------|
| Usage      | `string(frame.number) matches "[13579]$"` |
| Purpose    | Convert numeric fields to string (e.g., match odd frame numbers) |

---

## Wireshark analysis on different attacks

## Nmap

| Type       | Function |
|------------|----------|
| TCP Connect     | tcp.flags.syn==1 && tcp.flags.ack==1 && tcp.window_size>1024 |
| Syn attack    | tcp.flags.syn==1 and tcp.flags.ack==0 and tcp.window_size <= 1024 |
| UDP Scan closed port output | icmp.type==3 and icmp.code==3                       |
| UDP Scan for open ports     | udp && udp.port==80 |

## ARP Poising/Spoofing

- ARP posing is messing with default gateways( switch) CAM address table.

| Type       | Function |
|------------|----------|
| ARP request      | arp.opcode==1 |
| ARP response   | arp.opcode==2 |
| ARP Scanning | arp.dst.hw_mac==00:00:00:00:00:00                      |
| Detecting ARP Poising attack    | arp.duplicate-address-detected or arp.duplicate-address-frame |
| ARP flooding to avoid detection | ((arp) && (arp.opcode == 1)) && (arp.src.hw_mac == target-mac-address) |


## DHCP, NetBios and Kerberos


| Type       | Function |
|------------|----------|
| DHCP Request     | dhcp.option.dhcp==3 |
| DHCP ACK     | dhcp.option.dhcp==5 |
| DHCP NAK     | dhcp.option.dhcp==6                    |
| DHCP hostname discover    |     dhcp.option.hostname contains "keyword" |
| DHCP domain name discover | dhcp.option.domain_name contains "keyword" |
| NetBios name discover     | nbns.name contains "keyword"               |
| Hostname discover in kerberos | kerberos.CNameString contains "keyword" |
| only usernmae search hostname ends with $ | kerberos.CNameString and !(kerberos.CNameString contains "$" ) |
| Kerberos protcol version                  | kerberos.pvno == 5 |
| domain name of generated ticket           |     kerberos.realm contains ".org" |
| service name and domain of generated ticket | kerberos.SNameString == "krbtg"  | 


- grab low hanging fruits DHCP request
    Option 50: Requested IP address.
    Option 51: Requested IP lease time.
    Option 61: Client's MAC address.

- grab low hanging fruits DHCP ACK

    Option 15: Domain name.
    Option 51: Assigned IP lease time.

- grab low hanging fruits NetBios

    Queries: Query details.
    Query details could contain "name, Time to live (TTL) and IP address details"



## ICMP and DNS

| Type       | Function |
|------------|----------|
| Check for ICMP tunnels     | data.len > 64 and icmp |
| Check for abnormal strings | dns contains "dnscat   |
| Check for dns tunnels      | dns.qry.name.len > 15 and !mdns |


## FTP
- read FTP response codes

| Type       | Function |
|------------|----------|
| FTP Reponse code    | ftp.response.code==200 |
| check for spefic user login | ftp.request.command == "USER" |
| Brute via common username   | (ftp.response.code == 530) and (ftp.response.arg contains "username") |
| Brute via static password   | (ftp.request.command == "PASS" ) and (ftp.request.arg == "password")  |


## HTTP and HTTP2
- read HTTP response code
- for http2 analysis, keylog file is needed, need to setup env variable for dumping keys
- add in wireshark, edit-> preference-> protocols-> tls-> add the keylogger file

| Type       | Function |
|------------|----------|
| HTTP response    | http.response.code==200 |
| Request method   | http.request.method=="GET/POST" |
| User agent, can find attacker tools | http.user_agent contains "nmap" |
| URL, can find a C2                  | http.request.uri contains "admin" |
| hostname                            | http.host contains "keyword"      |
| log4J                               | (http.user_agent contains "$") or (http.user_agent contains "==")  |
| client hello                        | (http.request or tls.handshake.type == 1) and !(ssdp)              |
