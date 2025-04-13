# 🛠️ Key Tools for Threat Intelligence (CTI)

Threat Intelligence relies heavily on tools that allow collecting, aggregating, and analyzing data about `IoCs` (Indicators of Compromise) and `TTPs` (Tactics, Techniques, and Procedures). Here we list some essential tools, many of them publicly accessible:

## 📊 IoC Reputation and Context Platforms

> These platforms are the starting point for investigating IPs, domains, hashes, or URLs.

* **[VirusTotal (VT)](https://www.virustotal.com/)**:
    * **CTI Use:** Indispensable. Not only tells you if something is "malicious" but also provides **crucial context**:
        * **`Relations Tab`:** Shows IPs a domain resolves to, domains resolving to an IP, files downloaded from a URL, etc. Key for pivoting and finding related infrastructure.
        * **`Behavior Tab`:** If it's a file, shows sandbox analysis (if executed) with behavior and `TTPs` mapped to MITRE ATT&CK.
        * **`Community Tab`:** Comments and analyses from other researchers.
        * **`Passive DNS`:** History of DNS resolutions (see DNS section below).
        * **`VT Graph` (Advanced):** Allows visualizing complex relationships between indicators.

* **[AbuseIPDB](https://www.abuseipdb.com/)**:
    * **CTI Use:** Specific for **IP reputation**. Shows if an IP has been reported for malicious activities, by whom, when, and in which category (e.g., `SSH`, `Scanning`, `Phishing`). Comments can provide additional context.

* **[URLhaus (abuse.ch)](https://urlhaus.abuse.ch/)**:
    * **CTI Use:** Database of **URLs associated with malware distribution**. Allows searching URLs and obtaining information about related malware (if known), status (online/offline), and associated tags.

* **[OTX (AlienVault Open Threat Exchange)](https://otx.alienvault.com/)**:
    * **CTI Use:** Collaborative platform. Search any `IoC` (IP, domain, hash, URL, CVE) and find related "Pulses" (reports/collections of `IoCs`) created by the community. Often includes associated **`TTPs` (MITRE ATT&CK)**, related malware, and other linked `IoCs`.

* **[URLScan.io](https://urlscan.io/)**:
    * **CTI Use:** Safely analyzes URLs. For CTI, it's useful because it reveals:
        * **Infrastructure:** Final IP, domains contacted by the page, TLS certificates.
        * **Technologies:** Frameworks, JS libraries used (can indicate phishing kits).
        * **`IoCs`:** IPs, domains, hashes of downloaded files.
        * **Screenshot:** Allows viewing the content without risk.

## 🌐 Domain and IP Analysis Tools

> Allow obtaining information about registration and network infrastructure.

* **WHOIS Lookups:**
    * **CTI Use:** Obtain information about a domain's **registration** (registrant, creation/expiration dates, nameservers). Useful for identifying recently or suspiciously registered domains.
    * **Tools:** Multiple websites (`whois.domaintools.com`, `who.is`, etc.) or the `whois` command on Linux.
    * **Note:** Privacy (WHOIS privacy/proxy) often hides the actual registrant data.

* **DNS and Passive DNS Lookups:**
    * **CTI Use:** Investigate a domain's DNS records (`A`, `MX`, `NS`, `TXT`). **Passive DNS (PDNS)** is crucial: it allows viewing the **history** of which IPs a domain has resolved to in the past, or which domains have resolved to an IP. Key for discovering related infrastructure that is no longer active or used intermittently.
    * **Tools:**
        * `nslookup` command (Windows/Linux) or `dig` (Linux) for direct DNS queries.
        * Online services (e.g., Google Public DNS, Cloudflare DNS).
        * PDNS Databases: **VirusTotal** (`Relations` tab), RiskIQ (requires account), CIRCL Luxembourg PDNS, etc.

* **Device Search Engines (Shodan, Censys, Zoomeye):**
    * **[Shodan](https://www.shodan.io/)**, **[Censys](https://search.censys.io/)**, **[ZoomEye](https://www.zoomeye.org/)**:
    * **CTI Use:** Search for internet-connected devices by IP, port, service, banner, etc. Useful for obtaining information about a **suspicious IP address**: What ports are open? What services are running? Are there banners revealing specific software (potentially vulnerable)? Is it associated with any known service (VPN, Tor exit node)?
    * **Note:** Require careful interpretation. The free version is usually limited.

## 📈 Intelligence Sharing Platforms (Threat Sharing)

* **[MISP (Malware Information Sharing Platform & Threat Sharing)](https://www.misp-project.org/)**:
    * **CTI Use:** It's an **open-source standard and software** to store, share, correlate, and collaborate on threat intelligence (events, `IoCs`, `TTPs`). It's widely used in professional teams (CERTs, ISACs, companies).
    * **BTL1 Relevance:** You likely won't use MISP directly in the exam, but it's important to know the **concept** of structured platforms for sharing CTI.

## 📰 OSINT Sources (Open Source Intelligence)

* **Security Blogs, News, Twitter:** (Mentioned in Phishing Resources) Are vital for obtaining information about **emerging threats**, new campaigns, observed `TTPs`, and malware analyses that can provide fresh `IoCs` and context. Follow relevant researchers and security companies.

---