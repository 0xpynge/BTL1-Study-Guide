# Phishing Analysis: Key Concepts

**Phishing** is one of the most widely used social engineering techniques by attackers to gain initial access to a network, steal credentials, or distribute malware. As a security analyst, your ability to identify and analyze suspicious emails, URLs, and attachments is a crucial frontline skill.

The main objective of phishing analysis is to determine if an email is malicious and, if so, extract **Indicators of Compromise (IoCs)** that can be used for detection, blocking, and threat intelligence.

## 🔎 Main Artifacts to Examine

When analyzing a suspicious email, we will focus on several key artifacts:

### 1. Email Headers (`Headers`)

Headers contain vital metadata about the origin and path of the email, often hidden in the standard email client view. They are fundamental for verifying authenticity. Key elements to review:

* **`From:` (Sender):** Who claims to send the email. Can be easily forged (`spoofed`).
* **`Reply-To:`:** Address where replies will be sent. Often different from the `From:` in malicious emails.
* **`Return-Path:`:** Address where undelivered emails bounce. Can also indicate the actual origin.
* **`Received:`:** Series of entries showing the servers the email passed through. Read from bottom to top to trace the path. Useful for identifying the actual source server.
* **`Authentication-Results:`:** Shows the results of checks for **SPF, DKIM, and DMARC**. They are crucial:
    * **SPF (Sender Policy Framework):** Verifies if the sending server's IP is authorized by the sender's domain. A `FAIL` is a red flag.
    * **DKIM (DomainKeys Identified Mail):** Adds a digital signature to the email, verifying the message hasn't been altered and that it comes from the declared domain. A `FAIL` indicates possible tampering or `spoofing`.
    * **DMARC (Domain-based Message Authentication, Reporting & Conformance):** Policy indicating what to do if SPF or DKIM fail (e.g., `reject`, `quarantine`, `none`). A `FAIL` here is a strong indication of an illegitimate email.
* **Source IP:** Often found in the `Received:` headers. Analyzing the reputation of this IP is vital.

### 2. Email Body (`Body`)

The visible content of the message. Look for warning signs such as:

* **Sense of Urgency or Threat:** "Your account will be blocked," "Urgent pending invoice."
* **Grammatical or Spelling Errors:** Especially in emails pretending to be from large organizations.
* **Generic Greetings:** "Dear customer" instead of your name.
* **Unusual Requests:** Requests for credentials, personal information, bank transfers.
* **Inconsistent Style:** Tone or format that doesn't match previous communications from the impersonated entity.
* **Suspicious Links:** See next section.

### 3. Links and URLs

Links are a critical component. Don't trust the displayed link text; always examine the actual URL it points to (hover over it without clicking, or examine the email's HTML source code if possible):

* **Deceptive Domains:** Use of domains similar to the legitimate one (e.g., `paypaI.com` with a capital 'i' instead of `paypal.com`), strange subdomains (`paypal.security-update.com`), or completely irrelevant domains.
* **URL Shorteners:** Services like Bitly, TinyURL can hide malicious destinations. They need to be expanded and analyzed with reputation tools before visiting.
* **Direct IP Addresses:** Rarely does a legitimate service link directly to an IP instead of a domain name.
* **"Defanging":** When sharing or documenting suspicious URLs, it's good practice to "defang" them to prevent accidental clicks, replacing `http` with `hxxp` and `.` with `[.]` (e.g., `hxxp://malicious-site[.]com/login.php`).

### 4. Attachments (`Attachments`)

Attachments are a common vector for delivering malware. Be wary of:

* **Dangerous File Types:** `.exe`, `.scr`, `.bat`, `.ps1`, `.js`, `.vbs`. Also compressed files (`.zip`, `.rar`, `.iso`, `.img`) that may contain them.
* **Documents with Macros:** Office files (`.docm`, `.xlsm`, `.pptm`) that request enabling macros can execute malicious code.
* **Double Extension:** Files attempting to hide their real type (e.g., `invoice.pdf.exe`). Ensure you have file extensions visible in your operating system.
* **Password-Protected Files:** Often used to evade automatic scanning by antivirus software. The password is often provided in the email body.
* **Need for Analysis:** NEVER open a suspicious attachment directly. Use sandboxes and reputation analysis tools (based on the file hash).

## 💡 Key Indicators of Compromise (IoCs)

From the analysis of these artifacts, we will extract valuable IoCs for defense:

* **IP Addresses:** From the sending server (`Received:` headers), from linked domains.
* **Domains and URLs:** Sending domains (`From:`, `Return-Path:`), domains linked in the body, malware download domains.
* **File Hashes:** MD5, SHA1, SHA256 of malicious attachments.
* **Email Addresses:** From the sender (`From:`), `Reply-To`, `Return-Path`.
* **Subjects:** Patterns in subject lines can be used to create detection rules.

---
*These are the fundamental elements we will look for.*