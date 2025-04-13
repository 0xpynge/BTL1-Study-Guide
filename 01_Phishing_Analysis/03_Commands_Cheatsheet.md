# Command Cheatsheet for Phishing Analysis

This is a quick reference table for useful command-line commands during the initial analysis of phishing artifacts.

**⚠️ Important:** Always execute these commands in a controlled and safe environment (like an isolated analysis virtual machine), especially if handling potentially malicious attachments. Safety is paramount.

## Common Commands Table

| Task/Objective                     | Command                                      | Platform       | Notes/Usage                                                       |
| :--------------------------------- | :------------------------------------------- | :------------- | :---------------------------------------------------------------- |
| **File Hashing** |                                              |                | **Fundamental for checking reputation on VirusTotal, etc.** |
| Calculate MD5 Hash                 | `md5sum <file>`                              | Linux          | Gets MD5 hash.                                                    |
| Calculate MD5 Hash                 | `Get-FileHash -Algorithm MD5 <file>`         | Windows (PS)   | Gets MD5 hash. (PS = PowerShell)                                  |
| Calculate SHA256 Hash              | `sha256sum <file>`                           | Linux          | Gets SHA256 hash (generally preferred over MD5).                  |
| Calculate SHA256 Hash              | `Get-FileHash -Algorithm SHA256 <file>`      | Windows (PS)   | Gets SHA256 hash (generally preferred over MD5).                  |
| **File Identification** |                                              |                | **Useful for detecting fake extensions.** |
| Identify Real File Type            | `file <file>`                                | Linux          | Determines file type by content ("magic numbers"), not extension. |
| **Artifact Download (RISKY!)** |                                              |                | **ONLY in isolated environments (sandbox/VM)!! NEVER execute.** |
| Download File (with curl)          | `curl -O <suspicious_URL>`                   | Linux          | Saves the remote file with its original name.                     |
| Download File (with wget)          | `wget <suspicious_URL>`                      | Linux          | Downloads the file from the URL.                                  |
| **Basic Content Analysis** |                                              |                | **Can reveal embedded indicators.** |
| Extract Readable Strings           | `strings <file>`                             | Linux/Win* | Searches for and displays readable text (URLs, IPs, phrases) within a file (even binary). Can generate a lot of noise. (*Requires `strings` on Windows, e.g., from Sysinternals). |

---

*This cheatsheet focuses on initial tasks. More advanced forensic or malware analysis commands are covered in their respective sections.*