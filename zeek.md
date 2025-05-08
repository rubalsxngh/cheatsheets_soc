# Zeek (Bro) Cheatsheet for Blue Team Analysts

Zeek is a **passive, open-source network traffic analyzer** focused on **network security monitoring** (NSM). It does not block, only observes and logs.

---

## Zeek Architecture Overview

| Component                  | Description                                                                 |
|----------------------------|-----------------------------------------------------------------------------|
| **Event Engine**           | Breaks down packets into events (source, destination, protocol, etc.)       |
| **Policy Script Interpreter** | Applies logic and correlation to events using Zeek’s scripting language     |

**Output**: Zeek generates **50+ log files** grouped into **7 major categories**.  

[Frameworks](https://docs.zeek.org/en/master/frameworks/index.html)
[Zeek Log Files Reference](https://docs.zeek.org/en/current/script-reference/log-files.html)
[Zeek Signature Framework](https://docs.zeek.org/en/master/frameworks/signatures.html)
[Zeek_Scripting_language](https://try.bro.org/#/?example=hello)
[Zeek_Scripts_repo](https://docs.zeek.org/en/master/script-reference/scripts.html)
[Zeek_Events](https://docs.zeek.org/en/master/scripts/base/bif/event.bif.zeek.html)
[Zeek_Frameworks](https://packages.zeek.org/)


---

## Zeek Service Control (`zeekctl`)

| Command                  | Description                                  |
|---------------------------|----------------------------------------------|
| `zeekctl start`           | Start Zeek as a service                      |
| `zeekctl stop`            | Stop the Zeek service                        |
| `zeekctl status`          | Show service status, PID, uptime, etc.       |

---

## Running Zeek on PCAPs

| Command                                 | Description                                       |
|------------------------------------------|---------------------------------------------------|
| `zeek -r /path/to/file.pcap`            | Analyze PCAP file                                |
| `zeek -r file.pcap -C`                  | Ignore checksum errors while reading             |
| `zeek -v`                               | Check Zeek version                               |
| `zeek -s`                               | Point to signature file                          |

---

## Zeek Logs (Examples)

| Log File         | Description                              |
|-------------------|------------------------------------------|
| `conn.log`        | All network connections                  |
| `dns.log`         | DNS queries and replies                  |
| `http.log`        | HTTP requests and responses              |
| `ssl.log`         | SSL/TLS handshake details                |
| `notice.log`      | Notices, alerts, and warnings            |
| `weird.log`       | Protocol violations or anomalies         |
| `files.log`       | Files seen in network traffic            |


---

## Zeek Signature Framework

- Zeek signatures are **low-level rules** to match conditions in traffic.
- Complete detection chains are handled by **Zeek scripts**.
- A signature contains:
  - `signature-id`
  - `conditions` (headers, payload, context)
  - `actions` (e.g., generate an event)



---

## Signature Conditions

| Type     | Filter Examples                          |
|----------|-------------------------------------------|
| Header | `src-ip`, `dst-ip`, `src-port`, `dst-port`, `ip-proto` |
| Content | `payload`, `http-request`, `http-reply-body`, `ftp`    |
| Context | `same-ip` (same src and dst IP)         |
| Action | `event` (trigger custom event/message)  |
| Comparison Operators |  `==`, `!=`, `<`, `<=`, `>`, `>=` | 
| NOTE! | Value Types: string, numeric, regex |

---

## Signature Example

```zeek
signature my-first-sig {
    ip-proto == tcp
    dst-port == 80
    payload /.*root/
    event "Found root!"
}
```

---

## Zeek Scripts

Powerful high level language for detections and correlations, .zeek files

| Type     | Filter Examples                          |
|----------|-------------------------------------------|
| base scripts | /opt/zeek/share/zeek/base             |
| user generated or modified scripts | /opt/zeek/share/zeek/site |
| policy related                     | /opt/zeek/share/zeek/policy |
| configuration file                 | /opt/zeek/share/zeek/site/local.zeek |


- @load /path/to/script in live env.

- zeek scripts blueprint:
``` event event_name {
  action
}
```

## Zeek Scripts examples

```
event new_connection(c: Connection) {
  print("new connection found");
  print("Source host %s # %s", c$id$orig_h, c$id$orig_p);
}
```
---

## Zeek Framework ZKG

- used to install third party scripts and plugins
- zeek -Cr /path/to/pcap zeek_framework

| Type     | Filter Examples                          |
|----------|-------------------------------------------|
| zkg install path/to/plugin | Install thirds party plugins |
| zkg install /github/link | Install github link |
| zkg list                 | List all the installed packages |
| zkg remove               | Removed installed packages      |
| zkg refresh              | check version updates           | 
| zeek upgrade             | upgrade installed packages      |

---


