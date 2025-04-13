# 🔬 Specific Protocol Analysis (HTTP, DNS, SMB)

> Besides filtering for the presence of a protocol, it's crucial to understand what to look for _within_ those protocols to identify malicious activity. Here we focus on `HTTP`, `DNS`, and `SMB`/`SMB2`.

## 🌐 HTTP / HTTPS (Web Traffic)

* **Forensic Value:** The base protocol of the web. Used for Browse, file downloads, APIs, and frequently by malware for `C2` and payload downloads.
* **Key Fields to Examine:**
    * **Request:**
        * `http.request.method`: `GET` (request resource), `POST` (send data), `HEAD`, etc.
        * `http.request.uri`: The specific path requested on the server (e.g., `/download/payload.exe`).
        * `http.host`: The domain name the request is directed to.
        * `http.user_agent`: Identifies the browser or client making the request (malware often uses custom or generic UAs).
        * `http.referer`: Previous page from which the request originated (if applicable).
        * `http.cookie`: Cookies sent to the server.
    * **Response:**
        * `http.response.code`: Status code (e.g., `200` OK, `301`/`302` Redirect, `404` Not Found, `500` Server Error).
        * `http.content_type`: Type of content returned (e.g., `text/html`, `application/json`, `application/octet-stream` for binary downloads).
        * `http.server`: Information about the web server software.
        * `http.set_cookie`: Cookies the server asks the client to save.
* **What to Look For (Warning Signs):**
    * `GET` requests downloading executable files (`.exe`, `.dll`), scripts (`.ps1`, `.js`, `.vbs`), or suspicious compressed files (`.zip`, `.rar`, `.iso`).
    * `POST` requests sending data to unknown or suspicious URLs/domains (could be data exfiltration or `C2` check-in). Analyze the sent data if not encrypted.
    * Unusual, generic `User-Agents` (e.g., `curl`, `wget` in unexpected contexts) or those trying to mimic legitimate browsers but containing errors.
    * `C2` **beaconing patterns**: Regular, periodic `GET` or `POST` requests to the same URL/domain.
    * Redirects (`301`/`302`) leading to malicious sites.
    * Massive `404` errors can indicate web directory scanning.
* **HTTPS:** Remember the content (payload) is encrypted. However, you can still see: IPs, ports, data volume, duration, and often the destination domain name via the **SNI (Server Name Indication)** extension in the TLS handshake (filter: `tls.handshake.extensions_server_name`).
* **Tools:** `Wireshark` (`Follow > HTTP Stream`, `Export Objects > HTTP`), `tshark` (`-e http...`).

---

## ❓ DNS (Domain Name System)

* **Forensic Value:** Translates human-readable domain names (e.g., `www.google.com`) to IP addresses. It's fundamental for almost all internet communication and is **frequently abused** by malware for `C2`, `DGA`, and tunneling.
* **Key Fields to Examine:**
    * `dns.qry.name`: The domain name being queried.
    * `dns.qry.type`: The requested record type (`1`=A (IPv4), `28`=AAAA (IPv6), `5`=CNAME, `15`=MX (Mail), `16`=TXT, `6`=SOA).
    * `dns.resp.name`, `dns.resp.addr`, `dns.resp.ttl`: Name, IP address, and Time-To-Live in the response.
    * `dns.flags.rcode`: Response code (`0`=No Error, `3`=NXDomain - Non-Existent Domain).
* **What to Look For (Warning Signs):**
    * Queries to known malicious or suspicious domains (CTI lists).
    * **High Volume of `NXDomain`:** Many queries for non-existent domains can indicate **`DGA` (Domain Generation Algorithms)** activity, where malware generates many domains hoping one is active for `C2`.
    * Queries for very long or random-looking domains (possible `DGA`).
    * Queries for unusual record types (especially `TXT`) that could be used for `C2` or exfiltration (**DNS Tunneling**).
    * **Fast Flux** patterns: A domain resolving to multiple different IPs in short periods.
    * Use of non-standard or suspicious DNS servers.
* **Tools:** `Wireshark` (`dns` filters), `tshark` (`-e dns...`), Passive DNS services.

---

## 💻 SMB / SMB2 (Server Message Block)

* **Forensic Value:** Primary Windows protocol for file sharing, printing, and inter-process communication on a local network. It's legitimate but **heavily abused by attackers** for lateral movement, remote file access, remote execution, and internal exfiltration.
* **Key Fields to Examine:**
    * `smb2.cmd` or `smb.cmd`: The executed SMB command (e.g., `CREATE` (Open/Create), `READ`, `WRITE`, `CLOSE`, `TREE_CONNECT` (Connect to share), `IOCTL`).
    * `smb2.filename` or `smb.file`: Name of the file or directory being accessed.
    * `smb2.tree` or `smb.share`: Name of the share being connected to (e.g., `\\SERVER\IPC$`, `\\SERVER\C$`, `\\SERVER\Share`).
    * `smb2.user` or `smb.user`: Username performing the action (if session is authenticated).
    * `smb2.status` or `smb.nt_status`: Status code of the operation (e.g., `STATUS_SUCCESS`, `STATUS_LOGON_FAILURE`, `STATUS_ACCESS_DENIED`).
* **What to Look For (Warning Signs):**
    * Connections to administrative shares (`IPC$`, `ADMIN$`, `C$`, etc.) between workstations or from unexpected servers (possible lateral movement).
    * Repeated failed authentications (`STATUS_LOGON_FAILURE`) to shares.
    * Access to or modification of sensitive files (e.g., `sam`, `system`, `ntds.dit`, logon scripts).
    * Transfer of executable files (`.exe`, `.dll`, `.ps1`) between machines.
    * Use of remote execution tools like `PsExec` (often leaves traces like connections to `IPC$` and creation/execution of a service named `PSEXESVC` on the target, though names can change).
    * Suspicious `IOCTL` commands, sometimes used for enumeration or exploitation.
* **Tools:** `Wireshark` (`smb` or `smb2` filters), `tshark` (`-e smb...`), `NetworkMiner` (can extract files transferred via SMB).

---

> _Analyzing the content and patterns within these key protocols often reveals the exact nature of malicious activity that a simple IP and port analysis might miss._