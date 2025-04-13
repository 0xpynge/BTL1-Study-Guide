# 🌐 Network Analysis: Key Concepts

> **Network Forensics Analysis** or **Network Traffic Analysis (NTA)** focuses on monitoring, capturing, storing, and analyzing network communications to detect and/or investigate security incidents, intrusions, malware activity, policy violations, and other relevant events.

While disk and memory analysis look at what happens _inside_ a system, network analysis examines _how_ systems communicate with each other and _what_ information they exchange.

## 📦 PCAP: The Fundamental Evidence

> * **What is it?:** A **PCAP (Packet Capture)** file (`.pcap` or the more modern `.pcapng` format) is the de facto standard for saving captured data packets from a network. It contains a copy of the headers and, usually, the content (payload) of the packets that were traveling over the wire (or air) at a given time.

* **Capture:** Tools like `tcpdump` (Linux CLI), `Wireshark` (GUI Win/Lin/Mac), or `tshark` (Wireshark's CLI) are used to capture live traffic and save it in PCAP format.
* **Analysis:** Tools like `Wireshark`, `tshark`, `Zeek` (formerly Bro), `Suricata`, `NetworkMiner` are used to read and analyze PCAP files.
* **In BTL1:** You will very likely be provided with relevant PCAP files for the incident scenario, and you will need to analyze them to find answers.

## 📶 OSI / TCP/IP Model (Quick Review for Analysis)

> Understanding the network layers helps know where to look for what information:

* **Layer 2 (Link - e.g., Ethernet):** Provides physical **MAC** addresses. Useful for identifying specific devices on a local network.
* **Layer 3 (Network - e.g., IP):** Logical **IP** addresses (`IPv4`/`IPv6`). Fundamental for knowing the source and destination of communications between networks.
* **Layer 4 (Transport - e.g., TCP, UDP):** Manages end-to-end communication.
    * **`TCP`:** Connection-oriented (`SYN`, `ACK`, `FIN`), reliable, uses **ports** to identify applications/services. Key for stream analysis.
    * **`UDP`:** Connectionless, fast, less reliable, also uses **ports**. Common for `DNS`, `DHCP`, streaming.
    * **Ports:** Indicate the service/application (e.g., `80`=HTTP, `443`=HTTPS, `53`=DNS, `22`=SSH). Unexpected ports can be suspicious.
* **Layer 7 (Application):** The protocols applications actually use to exchange data (`HTTP`, `DNS`, `SMB`, `SMTP`, `FTP`, etc.). The actual **content** of the communication resides here (if not encrypted).

## 👀 What Do We Look For in Network Traffic?

The objectives of network traffic analysis in a security context typically include:

1.  **Known Malicious Communications:**
    * Connections to/from IPs, domains, or URLs marked as malicious (malware `C2`, phishing sites, etc. - requires CTI).
2.  **Malware Activity:**
    * Command and Control (`C2`) Beacons: Regular, periodic connections to an external server.
    * Payload Downloads: `HTTP`/`FTP`/etc. traffic downloading executables, scripts.
    * Propagation Activity: Port scanning (`TCP SYN scans`), connection attempts to other internal systems (`SMB`, `RDP`).
3.  **Data Exfiltration:**
    * Unusual large outbound data transfers.
    * Use of non-standard or 'covert' protocols to exfiltrate data (`DNS tunneling`, `ICMP tunneling`).
4.  **Policy Violations:**
    * Use of disallowed protocols (`P2P`, `Tor`).
    * Access to prohibited websites/categories.
5.  **Anomalies and Suspicious Behaviors:**
    * Protocols on incorrect ports (e.g., `SSH` over port `80`).
    * Unusual traffic patterns (spikes, strange long-duration connections).
    * Excessive connection errors (`TCP Resets`).
    * Use of insecure protocols (`Telnet`, plaintext `FTP` for credentials).

## 🔒 Metadata vs. Full Content and Encryption

* **Full Packet Capture (PCAP):** Captures the entire packet, including the payload. Allows seeing the exact content of communications **if they are not encrypted**.
* **Metadata (NetFlow, Zeek/Bro Logs):** Summarize communications (IPs, ports, duration, bytes transferred) but **do not include the payload**. Useful for high-level analysis and pattern detection, but limited for seeing exact content.
* **Encryption (TLS/SSL):** Much of the web traffic and many other communications are encrypted today. This means that even if you capture the full PCAP, you **cannot read the application content** (e.g., the exact `HTTPS` webpage, `SSH` commands). However, you **can still see important metadata**: IPs, ports, duration, data volume, and sometimes certificate information or the `SNI` (Server Name Indication) in TLS, which can reveal the destination domain.

## 🎯 BTL1 Focus

> In BTL1, network analysis generally involves working with **PCAP files** using tools like **`Wireshark`** or **`tshark`**. You will need to know how to apply filters, follow conversations (`TCP`/`UDP Streams`), identify protocols, and extract relevant information (files, plaintext credentials if any) to answer questions about the communications associated with an incident.

---

> _Understanding these concepts prepares you to effectively use analysis tools and extract the maximum possible information from network traffic._