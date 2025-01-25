# TCPDump Cheatsheet for Blue Team Analysts

## **Basic Syntax**
```
# General Syntax
tcpdump [options] [expression]
```

---

## **Common Options**
- **`-i [interface]`**: Specify the interface (e.g., `eth0`, `wlan0`).
  ```
tcpdump -i eth0
  ```
- **`-n`**: Disable DNS resolution for faster output.
  ```
tcpdump -n
  ```
- **`-nn`**: Disable both DNS and port name resolution.
  ```
tcpdump -nn
  ```
- **`-v`, `-vv`, `-vvv`**: Increase verbosity levels.
  ```
tcpdump -vv
  ```
- **`-c [count]`**: Stop after capturing a specified number of packets.
  ```
tcpdump -c 10
  ```
- **`-w [file]`**: Write packets to a file (PCAP format).
  ```
tcpdump -w capture.pcap
  ```
- **`-r [file]`**: Read packets from a file.
  ```
tcpdump -r capture.pcap
  ```

---

## **Filtering Basics**
- **Host Filter**
  ```
tcpdump host 192.168.1.10
  ```
- **Source or Destination Filter**
  ```
tcpdump src 192.168.1.10
  tcpdump dst 192.168.1.10
  ```
- **Network Filter**
  ```
tcpdump net 192.168.1.0/24
  ```
- **Port Filter**
  ```
tcpdump port 443
  ```
- **Protocol Filter**
  ```
tcpdump icmp
  tcpdump tcp
  tcpdump udp
  ```
- **Logical Operators**
  - AND: `and`
  - OR: `or`
  - NOT: `not`
  ```
tcpdump tcp and port 443
  tcpdump not host 192.168.1.10
  ```

---

## **Advanced Examples**
- **Capture Only HTTP Traffic**
  ```
tcpdump tcp port 80
  ```
- **Capture HTTPS Traffic (No Name Resolution)**
  ```
tcpdump -nn port 443
  ```
- **Capture Traffic Between Two Hosts**
  ```
tcpdump host 192.168.1.10 and 192.168.1.20
  ```
- **Capture Specific Source and Destination Ports**
  ```
tcpdump src port 22 and dst port 443
  ```
- **Filter by Specific Network and Protocol**
  ```
tcpdump net 192.168.1.0/24 and udp
  ```
- **Monitor a Specific Interface**
  ```
tcpdump -i wlan0
  ```

---

## **Useful Tips**
- **Capture Packets and View in Wireshark**
  ```
tcpdump -i eth0 -w capture.pcap
  # Open `capture.pcap` in Wireshark
  ```
- **Limit Packet Size (Snap Length)**
  ```
tcpdump -s 0  # Capture full packet
  tcpdump -s 100  # Capture first 100 bytes
  ```
- **Timestamp Formats**
  ```
tcpdump -tttt  # Human-readable timestamps
  ```
- **Follow Live Traffic**
  ```
tcpdump -l | tee output.txt
  ```
- **Capture Traffic from a Specific VLAN**
  ```
tcpdump vlan 10
  ```

---

## **Resources**
- [TCPDump Manual](https://www.tcpdump.org/manpages/tcpdump.1.html)
- [Wireshark User Guide](https://www.wireshark.org/docs/wsug_html_chunked/)

---

### **Quick Command Reference**
| Command                             | Description                         |
|-------------------------------------|-------------------------------------|
| `tcpdump -i eth0`                   | Monitor `eth0` interface            |
| `tcpdump port 80`                   | Capture HTTP traffic                |
| `tcpdump -w capture.pcap`           | Save to a PCAP file                 |
| `tcpdump -r capture.pcap`           | Read from a PCAP file               |
| `tcpdump src 192.168.1.10`          | Capture packets from a source host  |
| `tcpdump -nn port 443`              | Capture HTTPS traffic               |
| `tcpdump -s 0`                      | Capture full packets                |

---

### **Pro Tip**
- Always use **`-n`** or **`-nn`** in SOC environments to avoid delays caused by DNS resolution.
