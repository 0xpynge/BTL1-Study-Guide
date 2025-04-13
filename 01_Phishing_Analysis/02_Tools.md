# Essential Tools for Phishing Analysis

Analyzing phishing emails requires a set of tools that allow us to investigate URLs, files, IPs, and domains without directly exposing ourselves to risks. Below are some of the most useful ones, grouped by category:

## 🌐 Reputation Services (URL, IP, Domain, Hash)

These services maintain massive databases of known malicious indicators.

* **[VirusTotal](https://www.virustotal.com/)**:
    * **Use:** Essential. Allows analysis of URLs, IPs, domains, and file hashes (MD5, SHA256). Cross-references information with dozens of antivirus engines and blocklists. Searches for relationships between indicators.
    * **Important!**: Be careful with confidentiality when uploading files.

* **[URLhaus](https://urlhaus.abuse.ch/)**:
    * **Use:** Specific database of URLs associated with malware distribution. Useful for verifying suspicious links and obtaining additional context (associated malware, tags). Allows reporting malicious URLs.

* **[AbuseIPDB](https://www.abuseipdb.com/)**:
    * **Use:** Focused on the reputation of IP addresses. Reports if an IP has been reported for malicious activities (spam, C&C, attacks, etc.). Very useful for analyzing source IPs of emails or IPs that suspicious domains resolve to.

* **[OTX (AlienVault Open Threat Exchange)](https://otx.alienvault.com/)**:
    * **Use:** Open threat intelligence platform. Search IPs, domains, URLs, hashes and get related "pulses" (intelligence reports), including IoCs, TTPs (MITRE ATT&CK), and associated malware.

## 🔬 Sandboxes (Controlled Dynamic Analysis) 
Allow executing files or URLs in a safe and isolated environment to observe their real behavior.

* **[Any.Run](https://any.run/)**:
    * **Use:** Interactive sandbox (the free version allows some interaction). Allows seeing in real-time how a file or URL behaves in a Windows VM, including created processes, network connections, system modifications. Very visual.

* **[Hybrid Analysis](https://www.hybrid-analysis.com/)**:
    * **Use:** Free sandbox combining static and dynamic analysis. Provides detailed reports with MITRE ATT&CK indicators, screenshots, network traffic (PCAP), and comparison against a known malware database.

* **[Triage](https://tria.ge/)**:
    * **Use:** Another powerful sandbox, with a generous free tier. Allows analysis on Windows and Linux, IoC extraction and mapping with MITRE ATT&CK.

* **[Joe Sandbox Cloud](https://www.joesandbox.com/)**:
    * **Use:** Very advanced sandbox, often paid, but sometimes offers limited free analyses. Known for its deep detection capabilities and evasion of anti-sandbox techniques.

## ✉️ Header Analysis

Tools to visualize and analyze email headers in a more readable way.

* **[MxToolbox Email Header Analyzer](https://mxtoolbox.com/EmailHeaders.aspx)**:
    * **Use:** Paste the full email header and the tool parses it, showing the delivery path and other information more clearly. Includes SPF/DKIM/DMARC analysis.

* **[Google Admin Toolbox - Messageheader](https://toolbox.googleapps.com/apps/messageheader/)**:
    * **Use:** Similar to the previous one, Google-specific but works with any header. Analyzes and clearly shows the path, delays, and authentication results (SPF, DKIM, DMARC).

## 🔗 URL Analysis and Expansion

Tools to investigate URLs safely.

* **[URLScan.io](https://urlscan.io/)**:
    * **Use:** Scans a URL and provides a screenshot of the page, information about the technologies it uses, IPs it resolves to, contacted domains, found IoCs, and more. Very useful to see what's behind a link without visiting it yourself.

* **URL Expanders:** (Search "URL expander" online)
    * **Use:** For shortened URLs (Bitly, TinyURL). Paste the short URL and it shows you the final URL it redirects to, allowing you to analyze it before clicking.

* **Safe Browse (Sandboxed Browsers):**
    * **Concept:** Tools like Browserling or your own virtual machine environments allow opening links in an isolated browser, separate from your main system.

## 🛠️ Other Useful Tools

* **[PhishTool](https://www.phishtool.com/)**:
    * **Use:** Specific platform for phishing analysis that integrates many of the previous functionalities (analysis of headers, URLs, files) and helps automate the process. Has free and paid plans.

---

**⚠️ Important Security Note!**

* **NEVER** click links or open attachments directly from a suspicious email on your main machine.
* **ALWAYS** use isolated virtual machines or online sandbox services for any dynamic analysis.
* Be aware of **confidentiality** when uploading files or full emails to public online services like VirusTotal. If working with sensitive information, consider using offline tools or private sandboxes.