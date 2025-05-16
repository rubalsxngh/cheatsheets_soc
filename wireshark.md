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

## Resources

- [Wireshark Display Filter Reference](https://www.wireshark.org/docs/dfref/)
- [Wireshark Capture Filters](https://wiki.wireshark.org/CaptureFilters)
- [Wireshark Filtering Expressions](https://wiki.wireshark.org/DisplayFilters)

---
