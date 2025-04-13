# 📉 Recognizing Malicious Traffic Patterns

> Beyond analyzing individual packets or protocols, recognizing **patterns** in network traffic over time or across multiple connections is crucial for detecting malicious activities that might otherwise go unnoticed.
>
> These patterns often correspond to different phases of a cyberattack (`C2`, lateral movement, exfiltration, etc.).

---

<details>
<summary><strong>📡 Command and Control (C2 / C&amp;C) Patterns</strong></summary>
<br>

> Once a system is compromised, malware needs to communicate with the attacker to receive commands and/or send data. This `C2` traffic often follows characteristic patterns:

* **Beaconing / Heartbeat:**
    * **Description:** **Regular and periodic** connections from the infected host to an external `C2` server. Can occur every few seconds, minutes, or hours.
    * **What to Look For:** Repetitive outbound traffic to the same IP/domain with consistent time intervals (delta time). Packet sizes might be small and similar (for check-ins) or vary if downloading commands/sending data. Pay attention to connections outside business hours or to geographically unusual destinations. Common protocols: `HTTP`/`HTTPS`, `DNS`, `ICMP`, or custom `TCP`/`UDP`.

* **Unusual or Covert Protocols:**
    * **Description:** Use of non-standard protocols or non-standard ports for `C2` communication to evade detection.
    * **What to Look For:** Traffic on common ports (`80`, `443`, `53`) that doesn't match the expected protocol (e.g., non-HTTP traffic on port `80`, non-DNS traffic on port `53`). Use of high or random ports for persistent communications. Traffic with payloads that appear obfuscated or encrypted (high entropy) without being standard `TLS`/`SSH`.

* **DNS for C2:**
    * **Description:** Use of `DNS` queries/responses to send and receive commands/data. Includes `DGA` and `DNS Tunneling`.
    * **What to Look For:** (See DNS section in `04_Specific_Protocol_Analysis.md`) High volume of `NXDomain` responses, random/long domains, heavy use of `TXT` records or long subdomains.

</details>

---

<details>
<summary><strong>🎯 Network Scanning Patterns</strong></summary>
<br>

> Attackers (or malware like worms) scan networks to find other vulnerable systems.

* **Horizontal Scanning:**
    * **Description:** One host tries to connect to **many different hosts** on the network, probing the **same port** or a small set of ports.
    * **What to Look For:** From a single source IP, a large number of connection attempts (`TCP SYN` packets without `SYN-ACK` responses, or followed by `RST`) to _multiple destination IPs_ (often sequential) on the same subnet or range, all targeting the **same destination port** (e.g., `445`-SMB, `3389`-RDP, `22`-SSH).

* **Vertical Scanning:**
    * **Description:** One host tries to connect to **many different ports** on a **single destination host**.
    * **What to Look For:** From a single source IP, a large number of connection attempts to a _single destination IP_, but targeting _different destination ports_.

</details>

---

<details>
<summary><strong>🚶‍♂️ Lateral Movement Patterns</strong></summary>
<br>

> Once inside, attackers try to move to other systems on the internal network.

* **What to Look For:**
    * Unusual **`SMB`/`RPC`** connections: Between workstations, from unexpected servers to workstations, or targeting administrative shares (`ADMIN$`, `C$`, `IPC$`).
    * **`RDP` (port `3389`)** connections between systems where it's not usual.
    * **`SSH` (port `22`)** connections between internal systems if not standard practice.
    * Traffic associated with remote execution tools like **`PsExec`**, `WinRM`, `WMI`. (`PsExec` often uses `SMB` and creates/starts a service named `PSEXESVC` on the target, though names can change).
    * Failed authentication attempts followed by success between internal machines.

</details>

---

<details>
<summary><strong>📤 Data Exfiltration Patterns</strong></summary>
<br>

> The final goal is often to steal sensitive information.

* **What to Look For:**
    * **Large Outbound Transfers:** Unusually large volumes of data sent from the internal network to external destinations, especially if occurring off-hours or to suspicious IPs/domains. Analyze conversation duration and total bytes (`Statistics > Conversations`).
    * **Unusual Protocols for Transfers:** Use of **`DNS Tunneling`** (lots of data in `TXT`/subdomain queries/responses), **`ICMP Tunneling`** (data in `ICMP` payloads), `FTP`, or custom protocols to get information out.
    * **Transfers to Cloud/Public Services:** Massive uploads to cloud storage services (Mega, Dropbox, etc.) or pastebins if not normal activity.
    * **Compression/Encryption:** Exfiltration traffic is often compressed (`.zip`, `.rar`) and/or encrypted. Look for `TLS` connections to suspicious destinations with large data transfers.

</details>

---

<details>
<summary><strong>📥 Malware Download / Additional Components Patterns</strong></summary>
<br>

* **What to Look For:**
    * `HTTP`/`FTP`/etc. connections downloading executables, DLLs, scripts, or suspicious compressed files from URLs/IPs known to host malware (CTI).
    * "Staged Payload" pattern: Initial malware (dropper) downloads additional components from different locations after the initial infection.

</details>

---

<details>
<summary><strong>🚫 Denial of Service (DoS/DDoS) Patterns</strong></summary>
<br>

> Less Common in Post-Mortem Forensic Analysis.

* **Description:** Flooding a target with traffic to exhaust its resources.
* **What to Look For:** Massive volume of packets (`SYN`, `UDP`, `ICMP`) from one or many sources towards a single destination. Often with spoofed source IPs.

</details>

---

> _Recognizing these patterns requires observing traffic in context, often using the statistics and time filtering features of **`Wireshark`**/**`tshark`**, or correlating events in a **SIEM** or **IDS**._