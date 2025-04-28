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
[Manual](http://manual-snort-org.s3-website-us-east-1.amazonaws.com/)
[Snorpy](https://snorpy.cyb3rs3c.net/)

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

## Running Snort as IDS/ IPS

| Command                                               | Description                                         |
|--------------------------------------------------------|-----------------------------------------------------|
| `snort -c [config file]`                             | Point to config/ rule file               |
| `snort -c [config file] -T`                 | Test the config file                   |
| `snort -N `                                  | Disble logging                    |
| `snort -D `                                       | Background mode         |
| `snort -A 'full/fast/console/cmg/none'`| Alert mode             |

- full/fast mode doesn't generate alerts on console. console for limited info on console and cmg for hex output.
- kill background snort job- kill -9 2898

## Read from a PCAP file

| Command                                               | Description                                         |
|--------------------------------------------------------|-----------------------------------------------------|
| `snort -r [pcap file]`                             | Point to pcap file               |
| `snort -r --pcap-list="file1 file2"`                 | multiple pcaps                   |
| `--pcap-show `                                  | show on console which pcap it's reading                    |

## Rule Structure and example rule

- action protocol src_ip src_port direction dst_ip dst_port (msg; sid; rev; reference; content; nocase; fast_pattern; sameip; id; flags; dsize)
- alert tcp any any <> any any (MSG: "detect all"; SID: 1000001; REV:1)
