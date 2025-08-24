## TShark 


### Command line packet analysis tool

| Option                  | Description                                 | Example                          |
|--------------------------|---------------------------------------------|----------------------------------|
| `-h`         | command help        | `tshark -h`                |
| `-D`         | list of all interfaces | `tshark -D`             |
| `-i`         | choose the interface   | `tshark -i eth0`        |
| `r`          | read the pcap file     | `tshark -r capture1.pcap`  |
| `w`          | write to a capture file | `tshark -w capture2.pcap`  |
| `V`          | provide packet details pane like wireshark | `tshark -V` |
| `q`          | silent mode                         | `tshark -q`        |
| `x`          | ouput hex and ASCII                 | `tshark -x`        |
| `c`          | number of packets                   |  `tshark -c 10`    |


### Capture parameter conditions (only works in sniffing mode)

| Option                  | Description                                 | Example                          |
|--------------------------|---------------------------------------------|----------------------------------|
| `-a`         | single loop      | `tshark -i eth0 -w sample.pcap -a [duration:1, filesize:10 (KB), files:3`                |
| `b`          | infinte loop, create a new file after one condition | `tshark -i eth0 -w sample.pcap -a [duration:1, filesize:10 (KB), files:3`                |


### Capture filter : Berkley packet filter: used to captue options

| Option                  | Description                                 | Example                          |
|--------------------------|---------------------------------------------|----------------------------------|
| -f "host <hostname>      | capture packet for hostname                 | tshark -f "host hello.domain.com" |
| -f "ip <ip addr>         | capture for perticular IP                   | tshark -f "ip 10.10.1.1"          |
| -f "net <net range>      | capture for a network range                 | tshark -f "net 10.10.1.1/24"      |
| -f "src/dst host <ip>"            | capture for a src ip                        | tshark -f "src/dst host 10.10.1.1"    |
| -f "port 80"                 | capture for perticular port                      | tshark -f "port 80"                 |
| -f "port 80-8080"            | capture for range of ports                       |  tshark -f "port 80-8080"           |
| -f "tcp/udp/icmp | capture for icmp/ tcp/ udp traffic                           | tsahrk -f "tcp"                     |

