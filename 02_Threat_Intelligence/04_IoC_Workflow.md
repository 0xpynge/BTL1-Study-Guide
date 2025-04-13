# 🕵️‍♂️ Suggested Workflow for Investigating IoCs with CTI

Once you have extracted Indicators of Compromise (`IoCs`) during your analysis (whether from a phishing email, SIEM logs, forensic analysis, etc.), the next step is to enrich them using Threat Intelligence (CTI) tools to understand their context, risk, and potential relationships.

This is a general and flexible workflow. Often you will need to "pivot" from one `IoC` to another based on the findings. **Always document your steps and results.**

## 📍 Investigating an IP Address

1.  **General Reputation:**
    * Look up the IP in **VirusTotal**. Review the detection score, community comments, and the `Relations` tab (domains that have resolved to this IP historically - Passive DNS).
    * Look up the IP in **AbuseIPDB**. Review the reported abuse categories, the number of reports, and the comments.
    * Look up the IP in **OTX (AlienVault)**. Is it associated with any 'Pulse' (report)? Are there related `IoCs` or `TTPs`?

2.  **Network/Registration Information (WHOIS):**
    * Perform a WHOIS lookup for the IP (you can use online tools or `whois` on Linux). Obtain information about the network block it belongs to (ASN, owner/ISP, country). Is the country of origin expected or suspicious?

3.  **Passive DNS (History):**
    * Dig deeper into Passive DNS data (via VT or other tools). What domains have pointed to this IP recently or in the past? Do any of those domains look suspicious? This is key to finding related infrastructure.

4.  **Service Information (Optional/Advanced):**
    * Look up the IP in **Shodan / Censys**. What ports are open? What services is it running? Do they seem like legitimate services, misconfigured, or potentially malicious (e.g., an unusual port for C2)?

## 🌐 Investigating a Domain Name

1.  **General Reputation:**
    * Look up the domain in **VirusTotal** (review detection, Passive DNS, associated URLs, downloaded files).
    * Look up the domain in **URLhaus** (if you suspect it distributes malware).
    * Look up the domain in **OTX** (pulses, related `IoCs`).

2.  **Registration Information (WHOIS):**
    * Perform a WHOIS lookup for the domain. Pay attention to:
        * **Dates:** Very recent creation date? (Suspicious). Near expiration date?
        * **Registrar and Nameservers (NS):** Are they known or suspicious?
        * **Registrant Information:** Although often hidden by privacy, if visible, does it seem legitimate?

3.  **DNS Resolution (Current and Historical - Passive DNS):**
    * What IP addresses does the domain currently resolve to? (Use `nslookup`, `dig`, or online tools).
    * Investigate those IP addresses using the IP workflow.
    * Consult the Passive DNS history (VT, etc.). Has it resolved to other IPs in the past? Are those IPs suspicious?

4.  **Related Domains and URLs:**
    * Explore the `Relations` tab in VirusTotal. Are there associated subdomains? Are specific URLs under that domain marked as malicious?
    * Use **URLScan.io** with the domain name to see if there are previous scans or to perform a new one and analyze the main page or known paths.

## 📄 Investigating a File Hash (MD5, SHA256)

1.  **Extensive Reputation Check (VirusTotal is Key):**
    * Look up the hash in **VirusTotal**. Analyze in detail:
        * **`Detection Ratio`:** How many engines detect it? With what names (malware family)?
        * **`First/Last Seen`:** When was it first and last seen? Does it match your incident?
        * **`Associated Names`:** With what filenames has this hash been seen?
        * **`Behavior Tab`:** If there are sandbox reports, review them! Look at processes, network connections, files created/modified, MITRE ATT&CK `TTPs`.
        * **`Relations Tab`:** Where has this hash been seen (download URLs, IPs)? Is it related to other files (e.g., a dropper and its payload)?
        * **`Community Tab`:** Comments from other analysts.

2.  **Other Platforms:**
    * Look up the hash in **OTX** to find related pulses.
    * Look up the hash in **Hybrid Analysis** or **Triage** to find additional sandbox reports if not in VT.

3.  **Own Sandbox Analysis (If Necessary):**
    * If the hash is unknown ("0 detections" in VT) but you have strong suspicions (e.g., it came from a confirmed phishing email), consider submitting it to a sandbox (Any.Run, Triage, Hybrid Analysis, or your own Cuckoo/local environment) for dynamic analysis, **always following your organization's policies and confidentiality considerations.**

## 🔗 Investigating a Full URL

1.  **Component Reputation:**
    * Extract the **domain name** from the URL and analyze it using the domain workflow.
    * Find out which **IP address** the domain resolves to and analyze it using the IP workflow.
    * Look up the **full URL** in **VirusTotal**, **URLhaus**, and **OTX**.

2.  **Safe Analysis with URLScan.io:**
    * Submit the full URL to **URLScan.io**. Examine the results:
        * Screenshot (Is it a fake login page? An exploit kit?).
        * Redirects (Is the final URL different?).
        * IPs and Domains contacted during page load (could be additional `IoCs`!).
        * Hashes of files downloaded by the page.

3.  **Shortener Expansion:**
    * If the original URL is shortened (e.g., bit.ly), use a URL expander to get the final URL **before** performing the previous steps.

## 🔄 Pivoting and Documenting

> * **Pivoting:** The key is to use information found about one `IoC` to discover others. If a malicious IP hosted 5 domains, investigate all 5. If a malicious hash is downloaded from a specific URL, investigate the domain and IP of that URL.
> * **Documenting:** Keep a clear record of which `IoCs` you've investigated, what tools you used, what results you obtained, and how the `IoCs` relate to each other. This is essential for your final analysis and report.

---