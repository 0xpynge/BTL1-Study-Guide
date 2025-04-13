# Suggested Workflow for Phishing Analysis

Having a structured process helps analyze suspicious emails efficiently, safely, and without forgetting important steps. This is a general workflow you can adapt depending on the case and available tools.

**Safety First!** Always perform these steps in a controlled environment (analysis VM) and avoid directly interacting with potentially malicious elements on your main machine. If possible, work with the email as a file (`.eml` or `.msg`).

## Analysis Workflow Steps

1.  **Preparation and Safety:**
    * Ensure you are in your isolated analysis environment.
    * Have your analysis tools handy (secure browser, access to online services, terminal).
    * Obtain the suspicious email as a file (`.eml`/`.msg`) if possible, to preserve the complete headers.

2.  **Initial Assessment (Visual - No Clicks):**
    * **Sender:** Do you recognize the address? Does the display name match the actual address?
    * **Subject:** Is it generic, alarming, unexpected? Does it contain errors?
    * **Tone and Content:** Read the body looking for urgency, threats, bad grammar, generic greetings, strange requests.
    * **Links (Hover):** Hover the cursor **without clicking** over the links. Does the URL shown in the browser/email client status bar match the link text? Does it look suspicious?

3.  **Header Analysis (`Headers`):**
    * Extract the full email headers.
    * Paste them into a header analyzer (MxToolbox, Google Messageheader).
    * **Verify Path:** Follow the `Received:` headers from bottom to top to identify the actual source IP.
    * **Analyze IP Reputation:** Check the source IP in services like AbuseIPDB, VirusTotal, OTX.
    * **Review Authentication:** Pay special attention to the `SPF`, `DKIM`, and `DMARC` results. Failures here are strong indicators of `spoofing`.
    * **Compare `From:`, `Reply-To:`, `Return-Path:`:** Are they different? Do they point to suspicious domains?

4.  **Detailed Analysis of the Message Body:**
    * Look for specific social engineering techniques (e.g., pretexting, baiting).
    * Identify any requests for sensitive information (credentials, personal, financial data).
    * Extract all mentioned URLs for the next step.

5.  **URL Analysis (Safe Mode):**
    * **Extract and Unify:** List all unique URLs found in the body and headers.
    * **"Defang":** Convert them to `hxxp://domain[.]com` format for safe documentation.
    * **Check Reputation:** Look up each URL/domain in VirusTotal, URLhaus, OTX.
    * **Expand Shorteners:** Use a URL expander if you find short links (bit.ly, etc.) and analyze the final URL.
    * **Preview with Sandboxing:** Submit suspicious URLs to URLScan.io to see a screenshot and analysis of the destination page without visiting it directly. You can use Any.Run or Triage for deeper interaction if needed.

6.  **Attachment Analysis (Safe Mode):**
    * **DO NOT OPEN DIRECTLY!**
    * **Calculate Hashes:** Obtain the MD5 and SHA256 hashes of the file (see `03_Commands_Cheatsheet.md`).
    * **Check Hash Reputation:** Look up the hashes in VirusTotal and OTX. This is often enough to identify known malware.
    * **Identify Real Type (Linux):** Use the `file` command to verify if the extension matches the actual file type.
    * **Sandbox Analysis (If Necessary):** If the hash is unknown or suspicious, and policies allow, upload the file to a sandbox (Any.Run, Hybrid Analysis, Triage) for dynamic analysis. **Consider the confidentiality of the file's data!**

7.  **IoC Extraction and Conclusion:**
    * **Consolidate Findings:** Gather all identified Indicators of Compromise (IoCs):
        * Malicious IPs (source, C&C)
        * Malicious Domains/URLs
        * Malicious File Hashes
        * Attacker's email addresses (`From`, `Reply-To`)
        * Specific subjects or patterns.
    * **Verdict:** Determine if the email is `Malicious`, `Suspicious`, or `Legitimate`.
    * **Summary:** Write a brief summary explaining why you reached that conclusion, mentioning the key evidence.

8.  **Documentation / Escalation:**
    * Document your findings clearly and structured (your notes are key here!).
    * Follow your organization's procedures to report the incident, block the IoCs, or escalate the analysis if necessary.

---