# Snort Cheatsheet for Blue Team Analysts

---

## Snort Operating Modes
- **Sniffer Mode** → Capture and display packets
- **Logger Mode** → Capture and save packets to disk
- **IDS/IPS Mode** → Detect and/or prevent intrusions

---

## Basic Snort Commands

| Command                                | Description                                         |
|-----------------------------------------|-----------------------------------------------------|
| `snort -V`                              | Check Snort installation version                   |
| `snort -c /path/to/conf/file`            | Specify configuration file                         |
| `snort -c /path/to/conf/file -T`         | Test the configuration file for errors             |
| `snort -q`                               | Run Snort quietly (no startup banner)              |

[Official Snort Documentation](https://www.snort.org/documents)

---

## Running Snort in Sniffer Mode

| Command                                | Description                                         |
|-----------------------------------------|-----------------------------------------------------|
| `snort -v`                              | Verbose output (display packet headers)            |
| `snort -i [interface]`                  | Select interface (auto-picks if only one exists)    |
| `snort -d`                              | Display application layer (payload) data           |
| `snort -e`                              | Display link-layer headers                         |
| `snort -X`                              | Show payload in hex and ASCII                      |

---

## Running Snort in Logger Mode

| Command                                               | Description                                         |
|--------------------------------------------------------|-----------------------------------------------------|
| `snort -l [log_directory]`                             | Start logging packets to a directory               |
| `snort -n [number] -l [log_directory]`                 | Log a specific number of packets                   |
| `snort -r [log_file]`                                  | Read and analyze logged packets                    |
| `snort -K ASCII`                                       | Log packets in human-readable ASCII format         |
| `snort -r /path/to/log 'tcp or udp or icmp and port 90'`| Read logs and filter by protocols/port             |

---

## Quick Pro Tips
- **Use `-q`** when running Snort on production systems to suppress noisy banners.
- **Always test configs** with `-T` before going live.
- **Combine flags** like `-vde` for rich packet views when sniffing.

---
