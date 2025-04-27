# TCPDump Cheatsheet for Blue Team Analysts

### Basic Syntax

tcpdump [options] [expression]


[Comparitech TCPDump Cheat Sheet](https://www.comparitech.com/net-admin/tcpdump-cheat-sheet/)


### Common Options

| Option                  | Description                                 | Example                          |
|--------------------------|---------------------------------------------|----------------------------------|
| `-i [interface]`         | Specify interface (e.g., eth0, wlan0)        | `tcpdump -i eth0`                |
| `-n`                     | Disable DNS resolution                      | `tcpdump -n`                     |
| `-nn`                    | Disable DNS and port name resolution        | `tcpdump -nn`                    |
| `-v`, `-vv`, `-vvv`       | Increase verbosity levels                   | `tcpdump -vv`                    |
| `-c [count]`             | Stop after capturing [count] packets         | `tcpdump -c 10`                  |
| `-w [file]`              | Write packets to a file (PCAP)               | `tcpdump -w capture.pcap`        |
| `-r [file]`              | Read packets from a PCAP file                | `tcpdump -r capture.pcap`        |

---

## Basic Filtering

| Filter Type             | Example                         | Command                                 |
|--------------------------|---------------------------------|-----------------------------------------|
| **Host**                 | Capture all traffic from/to host| `tcpdump host 192.168.1.10`             |
| **Source Host**          | Capture only source traffic     | `tcpdump src 192.168.1.10`              |
| **Destination Host**     | Capture only destination traffic| `tcpdump dst 192.168.1.10`              |
| **Network**              | Capture network traffic         | `tcpdump net 192.168.1.0/24`            |
| **Port**                 | Capture traffic on a port       | `tcpdump port 443`                      |
| **Protocol**             | Capture specific protocol       | `tcpdump icmp` `tcpdump tcp` `tcpdump udp` |
| **Logical Operators**    | Combine filters                 | `tcpdump tcp and port 443`<br>`tcpdump not host 192.168.1.10` |

---

## Advanced Filtering Examples

| Scenario                                  | Command                                         |
|-------------------------------------------|-------------------------------------------------|
| Capture only HTTP traffic                 | `tcpdump tcp port 80`                           |
| Capture HTTPS traffic without name resolution | `tcpdump -nn port 443`                        |
| Capture traffic between two hosts         | `tcpdump host 192.168.1.10 and 192.168.1.20`    |
| Capture specific source and destination ports | `tcpdump src port 22 and dst port 443`         |
| Filter by network and protocol            | `tcpdump net 192.168.1.0/24 and udp`            |
| Monitor traffic on specific interface     | `tcpdump -i wlan0`                              |

---

## Useful Techniques

| Use Case                                 | Command                                         |
|-------------------------------------------|-------------------------------------------------|
| Save packets to file for Wireshark        | `tcpdump -i eth0 -w capture.pcap`               |
| Read and analyze saved capture            | `tcpdump -r capture.pcap`                       |
| Capture full packets (snap length)        | `tcpdump -s 0`                                  |
| Limit capture to specific bytes           | `tcpdump -s 100`                                |
| Human-readable timestamps                 | `tcpdump -tttt`                                 |
| Follow live traffic and save to file      | `tcpdump -l | tee output.txt`                   |
| Capture VLAN-specific traffic             | `tcpdump vlan 10`                               |

---

## Pro Tip

> **Always use `-n` or `-nn` while capturing live traffic in SOC environments** to avoid delays caused by DNS resolution!

---

## Additional Resources
- 📖 [TCPDump Official Manual](https://www.tcpdump.org/manpages/tcpdump.1.html)
- 🛠️ [Wireshark User Guide](https://www.wireshark.org/docs/wsug_html_chunked/)

---

## Quick Command Reference Table

| Command                             | Description                         |
|-------------------------------------|-------------------------------------|
| `tcpdump -i eth0`                   | Monitor `eth0` interface            |
| `tcpdump port 80`                   | Capture HTTP traffic                |
| `tcpdump -w capture.pcap`           | Save capture to a PCAP file         |
| `tcpdump -r capture.pcap`           | Read and analyze from a PCAP file   |
| `tcpdump src 192.168.1.10`          | Capture packets from a specific source |
| `tcpdump -nn port 443`              | Capture HTTPS traffic without DNS resolution |
| `tcpdump -s 0`                      | Capture full packets (no truncation) |

---