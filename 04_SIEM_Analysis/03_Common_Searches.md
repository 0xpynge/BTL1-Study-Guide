# 🔍 Common SIEM Searches (Splunk) - Table Format

> This table shows example SPL (Search Processing Language) queries for common security analysis scenarios in **`Splunk`**.

**Important:** Always remember to **adapt** these queries to your specific environment:
* Adjust **index** (`index=`) and **sourcetype** (`sourcetype=`) names.
* Verify and adapt **field names** (e.g., `user`, `src_ip`, `EventCode`, `dest_domain`, `NewProcessName`).
* Replace **placeholder values** (e.g., `<user>`, `<malicious_IP>`) with actual data.
* Select the appropriate **time range** in the Splunk interface.

| Objective                                                       | Example SPL Query                                                                                                                                                | Notes / Brief Explanation                                                                                                          |
| :-------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------- |
| **1. Multiple Failed Logins** (Brute Force/Guessing)            | `index="wineventlog" sourcetype="WinEventLog:Security" EventCode=4625 \| stats count BY user, src_ip \| sort -count \| head 20`                                    | Searches for `EventCode=4625` (Win). Groups by `user`/`src_ip`. Shows the top 20 with the most failed attempts.                  |
| **2. Suspicious Successful Logins** (Specific User/IP)        | `index="wineventlog" sourcetype="WinEventLog:Security" EventCode=4624 user="<compromised_user>" \| table _time, user, src_ip, LogonTypeName \| sort -_time`          | Searches for `EventCode=4624` (Win) for a `<user>`. Shows time, source IP, logon type. Sorted by most recent.                     |
| **3. Suspicious Process Execution** (Requires 4688/Sysmon 1 Logs) | `index="wineventlog" sourcetype="WinEventLog:Security" EventCode=4688 (NewProcessName="*\\powershell.exe" OR NewProcessName="*\\cmd.exe") \| table _time, CreatorProcessName, NewProcessName, CommandLine` | Searches for `EventCode=4688` (Win). Filters by process names (e.g., PowerShell, cmd). Shows parent, child process, & command line. |
| **4. Connections to Malicious Indicators** (IP/Domain)        | `(index="firewall" OR index="proxy") (dest_ip="<malicious_IP>" OR dest_domain="<malicious_domain>") \| stats count BY src_ip, dest_ip, dest_port, action \| sort -count` | Searches FW/Proxy logs for connections to a known bad IP/Domain (replace). Groups by source IP, dest, port, & action.            |
| **5. Suspicious File Downloads** | `index="proxy" sourcetype="proxy_logs" (file_extension="exe" OR url="*.ps1") \| stats count BY user, src_ip, url \| sort -count`                                   | Searches proxy logs (adapt fields) for downloads of suspicious extensions/URLs. Groups by user, source IP, and URL.             |
| **6. Investigate Specific User Activity** | `index=* (user="<compromised_user>" OR TargetUserName="<compromised_user>") \| stats count BY sourcetype, host, EventCode, action \| sort -count`                   | Searches for events related to `<user>` across all indexes. Groups by sourcetype, host, `EventCode`, action for summary.          |
| **7. General Search for an IoC** (IP, Domain, Hash, File)     | `index=* "<IOC_TO_SEARCH>" \| table _time, index, sourcetype, host, _raw \| sort -_time`                                                                        | Searches for an exact `<IoC>` string across all indexes. Shows context info and the raw event (`_raw`). Sorted by most recent.      |
| **8. Detection of Suspicious Changes** (Ex: New Service)        | `index="wineventlog" sourcetype="WinEventLog:System" EventCode=7045 \| table _time, host, ServiceName, ServiceFileName, ServiceType, StartType \| sort -_time`       | Searches for `EventCode=7045` (Win System Log - Service installed). Shows details of the new service. Sorted by most recent.     |

---

> _These examples are starting points. You will often need to combine, refine, and adapt these searches based on the specific investigation and available data._