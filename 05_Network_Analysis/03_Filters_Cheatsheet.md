# 📋 Display Filters Cheatsheet (Wireshark / Tshark)

> **Display Filters** (`Display Filters`) are the most powerful tool for analyzing PCAP files. They are applied _after_ the capture (in the Wireshark GUI or with `tshark -Y`) and allow you to isolate exactly the packets you are interested in, hiding the rest.

**Basic Syntax:**

* **Comparison:** `==` (equal), `!=` (not equal), `>` (greater than), `<` (less than), `>=`, `<=`
* **Contains:** `contains` (searches for a substring, e.g., `http.host contains "google"`)
* **Matches (Regex):** `matches` or `~` (uses regular expressions, e.g., `frame matches "[Pp][Aa][Ss][Ss][Ww][Oo][Rr][Dd]"`)
* **Logical Operators:** `and` (or `&&`), `or` (or `||`), `not` (or `!`)
* **Fields:** Refer to specific protocol fields (e.g., `ip.src`, `tcp.port`, `http.request.method`). You can explore field names in Wireshark's "Packet Details" pane.

Below are useful filters grouped by category:

## 🌐 IP-Based Filters

| Objective                   | Example Filter                         | Notes                                                                       |
| :-------------------------- | :------------------------------------- | :-------------------------------------------------------------------------- |
| Traffic to/from an IP       | `ip.addr == 192.168.1.54`              | Shows all traffic where the IP appears as source **OR** destination.         |
| Traffic FROM an IP          | `ip.src == 192.168.1.54`               | Only packets originating from that IP.                                      |
| Traffic TO an IP            | `ip.dst == 10.0.0.10`                  | Only packets destined for that IP.                                          |
| Traffic within a Subnet     | `ip.addr == 192.168.1.0/24`            | Shows traffic to/from any IP in that CIDR subnet.                            |
| Exclude an IP               | `not (ip.addr == 192.168.1.1)`         | Hides all traffic related to that IP.                                       |

## 🚦 Port / TCP / UDP Based Filters

| Objective                     | Example Filter                              | Notes                                                                         |
| :------------------------------ | :------------------------------------------ | :-------------------------------------------------------------------------- |
| Traffic on a specific port    | `tcp.port == 80`                            | Shows TCP traffic where source OR destination port is 80 (`HTTP`).         |
|                                 | `udp.port == 53`                            | Shows UDP traffic where source OR destination port is 53 (`DNS`).          |
|                                 | `port 443`                                  | Simpler, searches for port 443 in TCP or UDP (less precise than `tcp.port`). |
| TCP traffic only                | `tcp`                                       |                                                                             |
| UDP traffic only                | `udp`                                       |                                                                             |
| TCP SYN packets (conn. start) | `tcp.flags.syn == 1 and tcp.flags.ack == 0` | Useful for seeing connection attempts.                                      |
| TCP RST packets (conn. reset) | `tcp.flags.reset == 1`                      | Can indicate problems or scans.                                             |
| TCP FIN packets (conn. end)   | `tcp.flags.fin == 1`                        |                                                                             |
| Traffic from a TCP Stream     | `tcp.stream eq 5`                           | Shows only packets from TCP stream with index 5 (index assigned by Wireshark).|

## 📝 Specific Protocol Filters

| Protocol     | Objective                   | Example Filter                                           | Notes                                                                        |
| :----------- | :-------------------------- | :------------------------------------------------------- | :--------------------------------------------------------------------------- |
| **`HTTP`** | All HTTP traffic            | `http`                                                   |                                                                              |
|              | HTTP Requests only          | `http.request`                                           |                                                                              |
|              | HTTP Responses only         | `http.response`                                          |                                                                              |
|              | POST Requests               | `http.request.method == "POST"`                          | Useful for finding data submissions (credentials, forms).                   |
|              | Error Responses             | `http.response.code >= 400`                              | Looks for client-side (4xx) or server-side (5xx) errors.                   |
|              | Traffic to/from a Host      | `http.host contains "example.com"`                       | Searches for the hostname in the HTTP `Host` header.                        |
| **`DNS`** | All DNS traffic             | `dns`                                                    |                                                                              |
|              | DNS Queries only            | `dns.flags.response == 0`                                |                                                                              |
|              | DNS Responses only          | `dns.flags.response == 1`                                |                                                                              |
|              | Query for a Name            | `dns.qry.name == "malware.com"`                          | Searches for queries for a specific domain.                                  |
|              | Response with an IP         | `dns.resp.addr == 8.8.8.8`                               | Searches for responses containing a specific IP.                             |
| **`SMB/SMB2`**| All SMB/SMB2 traffic        | `smb or smb2`                                            | Windows file sharing protocol.                                               |
|              | Specific file access        | `smb2.filename contains ".exe"`                          | Searches SMB2 operations related to `.exe` files.                            |
|              | CREATE Command              | `smb2.cmd == CREATE`                                     | Searches for attempts to create/open files/directories.                     |
| **`ARP`** | All ARP traffic             | `arp`                                                    | MAC address resolution on local network.                                      |
| **`ICMP`** | All ICMP traffic            | `icmp`                                                   | Ping, traceroute, error messages.                                            |
| **`TLS/SSL`**| All TLS/SSL traffic         | `ssl or tls`                                             | Shows handshakes and encrypted traffic (without content view).              |
|              | Specific Handshake (SNI)    | `tls.handshake.extensions_server_name contains "site.com"`| Searches for the domain name (SNI) in the TLS handshake (if present).      |

## 🔍 Content / Exclusion / Combination Filters

| Objective                   | Example Filter                                         | Notes                                                                           |
| :-------------------------- | :----------------------------------------------------- | :------------------------------------------------------------------------------ |
| Search for a String (Payload)| `frame contains "password"`                            | Searches for "password" (case-insensitive) **anywhere** in the packet. Can be slow! |
|                             | `tcp contains "USER"`                                  | Searches for "USER" only in the TCP payload.                                    |
| Exclude Common Protocols    | `not (arp or dns or icmp)`                             | Hides ARP, DNS, and ICMP traffic to focus on other protocols.                     |
| Exclude Local Traffic       | `not (ip.addr == 192.168.1.0/24)`                      | Hides internal traffic if the local network is 192.168.1.0/24.                    |
| Combine Filters (Example)   | `ip.src == 10.0.0.5 and tcp.port == 443`               | Traffic from `10.0.0.5` on `TCP` port `443`.                                   |
|                             | `(http.request or dns.qry.name) and ip.dst == 8.8.8.8` | `HTTP` requests or `DNS` queries destined for `8.8.8.8`.                         |

---

> _Mastering display filters is the most important skill for analyzing PCAPs efficiently. Experiment and combine filters to isolate what you need!_