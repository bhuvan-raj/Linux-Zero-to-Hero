
# **Linux Network Commands – Detailed Flag Guide**

---

## **1. `ip` Commands** – Modern replacement for `ifconfig`/`route`

### **1.1 `ip addr`**

**Purpose:** Show or manipulate IP addresses assigned to interfaces.

**Common Flags:**

| Flag                              | Description                            |
| --------------------------------- | -------------------------------------- |
| `show` or `addr show`             | Display IP addresses of all interfaces |
| `add <IP>/<CIDR> dev <interface>` | Assign an IP to an interface           |
| `del <IP>/<CIDR> dev <interface>` | Remove an IP from an interface         |
| `-4`                              | Show only IPv4 addresses               |
| `-6`                              | Show only IPv6 addresses               |

**Examples:**

```bash
ip addr show                # Display all interfaces and IPs
ip -4 addr show             # Show only IPv4 addresses
ip addr add 192.168.1.10/24 dev eth0
ip addr del 192.168.1.10/24 dev eth0
```

**Notes:**

* Replaces `ifconfig`.
* Shows state (`UP/DOWN`), MAC address, IP, and interface.

---

### **1.2 `ip link`**

**Purpose:** Show and configure network interfaces.

**Common Flags:**

| Flag                         | Description                                      |
| ---------------------------- | ------------------------------------------------ |
| `show`                       | Display all network interfaces                   |
| `set <interface> up`         | Bring interface up                               |
| `set <interface> down`       | Bring interface down                             |
| `set <interface> mtu <size>` | Set the MTU size                                 |
| `promisc on/off`             | Enable/disable promiscuous mode (packet capture) |

**Examples:**

```bash
ip link show
ip link set eth0 up
ip link set eth0 down
ip link set eth0 mtu 1400
ip link set eth0 promisc on
```

**Notes:**

* Interface must be **UP** to assign IP or send traffic.

---

### **1.3 `ip route`**

**Purpose:** Show or modify routing table.

**Common Flags/Options:**

| Option                                               | Description                   |
| ---------------------------------------------------- | ----------------------------- |
| `show`                                               | Display current routing table |
| `add <network>/<mask> via <gateway> dev <interface>` | Add route                     |
| `del <network>/<mask> via <gateway>`                 | Remove route                  |
| `default via <gateway>`                              | Set default gateway           |

**Examples:**

```bash
ip route show
ip route add default via 192.168.1.1
ip route add 10.0.0.0/24 via 192.168.1.1 dev eth0
ip route del 10.0.0.0/24
```

**Notes:**

* Critical for multi-network servers.
* DevOps usage: verifying connectivity to private/public subnets, multi-network routing.

---

## **2. Connectivity Testing**

### **2.1 `ping`**

**Purpose:** Test if host is reachable; measure latency.

**Common Flags:**

| Flag            | Description                        |
| --------------- | ---------------------------------- |
| `-c <count>`    | Number of ICMP packets to send     |
| `-i <interval>` | Interval between packets (seconds) |
| `-s <size>`     | Payload size in bytes              |
| `-t <ttl>`      | Time-to-live for packets           |
| `-4/-6`         | Force IPv4 or IPv6                 |

**Examples:**

```bash
ping google.com               # Continuous ping
ping -c 4 google.com          # Send 4 packets
ping -i 2 google.com          # Interval of 2s
ping -s 100 google.com        # Payload size 100 bytes
ping -6 google.com            # Force IPv6
```

---

### **2.2 `traceroute`**

**Purpose:** Trace the network path to a host.

**Common Flags:**

| Flag            | Description                             |
| --------------- | --------------------------------------- |
| `-n`            | Show numeric IPs only (skip DNS lookup) |
| `-m <max_hops>` | Set maximum number of hops              |
| `-p <port>`     | Specify UDP port for probes             |
| `-I`            | Use ICMP echo instead of UDP packets    |
| `-T`            | Use TCP packets (firewall-friendly)     |

**Examples:**

```bash
traceroute google.com
traceroute -n google.com
traceroute -m 20 google.com
traceroute -I google.com
traceroute -T -p 443 google.com
```

---

### **2.3 `mtr`**

**Purpose:** Real-time combination of ping + traceroute.

**Common Flags:**

| Flag         | Description                         |
| ------------ | ----------------------------------- |
| `-r`         | Report mode (show final statistics) |
| `-c <count>` | Number of pings per host            |
| `-w`         | Wide output format                  |
| `-n`         | Show numeric IPs only               |

**Examples:**

```bash
mtr google.com
mtr -r -c 10 google.com
mtr -w google.com
mtr -n google.com
```

**Notes:**

* Continuous live stats of each hop’s latency and packet loss.
* Excellent for monitoring network stability.

---

### **2.4 `curl`**

**Purpose:** Transfer data to/from a server, test HTTP endpoints.

**Common Flags:**

| Flag             | Description                                  |
| ---------------- | -------------------------------------------- |
| `-I`             | Show only HTTP headers                       |
| `-X <method>`    | Specify HTTP method (GET, POST, PUT, DELETE) |
| `-d <data>`      | Send POST data                               |
| `-v`             | Verbose output                               |
| `-o <file>`      | Save response to file                        |
| `-L`             | Follow redirects                             |
| `-u <user:pass>` | HTTP authentication                          |

**Examples:**

```bash
curl http://example.com
curl -I http://example.com
curl -X POST -d "name=Bubu" http://example.com/api
curl -v http://example.com
curl -L http://example.com/redirect
curl -o file.html http://example.com
```

---

### **2.5 `wget`**

**Purpose:** Download files, check URL reachability.

**Common Flags:**

| Flag                     | Description                   |
| ------------------------ | ----------------------------- |
| `--spider`               | Check URL without downloading |
| `-O <file>`              | Save output to file           |
| `-r`                     | Recursive download            |
| `-nc`                    | Skip existing files           |
| `--limit-rate`           | Limit download speed          |
| `--no-check-certificate` | Ignore SSL issues             |

**Examples:**

```bash
wget http://example.com/file.zip
wget --spider http://example.com
wget -O myfile.html http://example.com
wget -r http://example.com/site
```

---

## **3. Port & Socket Utilities**

### **3.1 `netstat`**

**Purpose:** Show network connections, routing, listening ports (legacy).

**Common Flags:**

| Flag | Description                                     |
| ---- | ----------------------------------------------- |
| `-t` | TCP connections                                 |
| `-u` | UDP connections                                 |
| `-l` | Listening ports                                 |
| `-n` | Show numeric IP/port instead of resolving names |
| `-p` | Show process using the socket                   |
| `-a` | Show all connections                            |

**Examples:**

```bash
netstat -tulnp
netstat -anp
netstat -r  # Show routing table
```

---

### **3.2 `ss`**

**Purpose:** Modern alternative to `netstat`.

**Common Flags:**

| Flag | Description               |
| ---- | ------------------------- |
| `-t` | TCP connections           |
| `-u` | UDP connections           |
| `-l` | Listening sockets         |
| `-n` | Show numeric IP/port      |
| `-p` | Show process using socket |
| `-a` | Show all connections      |
| `-s` | Show summary              |

**Examples:**

```bash
ss -tuln
ss -p
ss -s
ss -tulnp
```

---

### **3.3 `lsof -i`**

**Purpose:** List processes using network ports.

**Common Flags:**

| Flag  | Description                  |
| ----- | ---------------------------- |
| `:80` | Show processes using port 80 |
| `TCP` | Show TCP connections         |
| `UDP` | Show UDP connections         |
| `-n`  | Show numeric IPs             |
| `-P`  | Show numeric ports           |

**Examples:**

```bash
lsof -i :22
lsof -i TCP
lsof -i UDP
lsof -i -nP
```

---

### **3.4 `telnet`**

**Purpose:** Test connectivity to a TCP port.

**Common Flags:** Telnet has few flags; mainly specify **host** and **port**.

**Examples:**

```bash
telnet google.com 80
telnet 192.168.1.10 22
```

* If connection succeeds → port is open.
* If fails → blocked by firewall or service not listening.

---

## **4. DNS Commands**

### **4.1 `nslookup`**

**Purpose:** Query DNS servers.

**Common Flags:**

| Flag       | Description         |
| ---------- | ------------------- |
| `<domain>` | Domain to resolve   |
| `<server>` | DNS server to query |

**Examples:**

```bash
nslookup google.com
nslookup google.com 8.8.8.8
nslookup -type=MX google.com
```

---

### **4.2 `dig`**

**Purpose:** Advanced DNS lookup.

**Common Flags:**

| Flag        | Description               |
| ----------- | ------------------------- |
| `@<server>` | Query specific DNS server |
| `A`         | Get IPv4 addresses        |
| `AAAA`      | Get IPv6 addresses        |
| `MX`        | Mail server               |
| `NS`        | Name server               |
| `+short`    | Short output              |
| `+trace`    | Trace resolution path     |

**Examples:**

```bash
dig google.com
dig google.com A
dig google.com MX
dig google.com @8.8.8.8 +short
dig google.com +trace
```

---

### **4.3 `host`**

**Purpose:** Simple DNS lookup.

**Common Flags:**

| Flag        | Description                           |
| ----------- | ------------------------------------- |
| `-t <type>` | Specify record type (A, AAAA, MX, NS) |

**Examples:**

```bash
host google.com
host -t MX google.com
host -t NS google.com
```

---

## **5. Packet Capture & Analysis**

### **5.1 `tcpdump`**

**Purpose:** Capture and analyze network traffic.

**Common Flags:**

| Flag             | Description                      |
| ---------------- | -------------------------------- |
| `-i <interface>` | Specify interface to capture     |
| `port <num>`     | Filter by port                   |
| `host <IP>`      | Filter by host                   |
| `-n`             | Don’t resolve hostnames          |
| `-nn`            | Don’t resolve hostnames or ports |
| `-c <count>`     | Stop after N packets             |
| `-w <file>`      | Write capture to file            |
| `-r <file>`      | Read packets from file           |

**Examples:**

```bash
tcpdump -i eth0
tcpdump -i eth0 port 80
tcpdump -i eth0 host 8.8.8.8
tcpdump -i eth0 -c 10 -nn
tcpdump -i eth0 -w capture.pcap
tcpdump -r capture.pcap
```

---
