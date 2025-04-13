# 🧠 Memory Analysis: Key Concepts and Artifacts

> A RAM memory dump is a goldmine of information about the state of a system at a specific moment. By analyzing it with tools like **`Volatility`**, we can extract a wide variety of artifacts that are not available (or are hard to find) in disk analysis.

These are some of the key types of information we look for:

## ⚙️ Process Information

> Understanding which processes were running is fundamental.

* **Process Lists:**
    * **What:** Obtain the list of active processes at the time of the capture.
    * **Value:** Identify legitimate processes, known malicious processes, hidden processes (`rootkits`), or recently terminated processes. See parent-child relationships (how a process was launched).
    * **Volatility Plugins (Examples):** `pslist`, `psscan`, `pstree`, `psaux` (Linux).

* **Process Details:**
    * **What:** Specific information about a process: command-line arguments it was launched with, environment variables, loaded libraries (`DLLs`), security identifiers (`SID`), open handles (to files, registry keys, etc.).
    * **Value:** Understand what the process was doing, if it loaded malicious DLLs (`DLL hijacking`/`injection`), what resources it was accessing, with what configuration it ran.
    * **Volatility Plugins (Examples):** `cmdline`, `environ`, `dlllist`, `handles`, `getsids`, `vadinfo`, `memmap`.

* **Process Memory Dump:**
    * **What:** Extract the complete memory space allocated to a specific process.
    * **Value:** Allows deeper analysis of the suspicious process with other tools (disassemblers, debuggers, strings analysis). Crucial for analyzing packed or injected malware, as you get the "unpacked" code as it resided in memory.
    * **Volatility Plugins (Examples):** `procdump`, `memdump`, `vadump`.

## 🔌 Network Information

> Identify the system's network activity at the time of capture.

* **Network Connections:**
    * **What:** List active TCP and UDP connections, as well as listening ports and recently closed connections.
    * **Value:** Detect connections to malware Command and Control (`C2`) servers, data exfiltration activity, connections to known malicious IPs/ports, ports opened by malware for listening. Shows local and remote IP/port, state, and the owning process PID.
    * **Volatility Plugins (Examples):** `netscan` (Win/Lin), `connections` (Win), `connscan` (Win), `sockets` (Lin), `sockscan` (Lin).

## 🖥️ Operating System Activity

> Information about the internal state of the kernel and services.

* **Kernel Modules / Drivers:**
    * **What:** List the drivers (`.sys` on Win, `.ko` on Lin) loaded in memory.
    * **Value:** Identify legitimate drivers, but also `rootkits` (which often operate as malicious drivers) or drivers loaded by malware. Allows detecting hidden drivers (`modscan`).
    * **Volatility Plugins (Examples):** `modules` (Win/Lin), `modscan` (Win/Lin), `driverscan` (Win).

* **Services:**
    * **What:** Enumerate registered services on the system (Windows).
    * **Value:** Identify malicious services installed for persistence.
    * **Volatility Plugins (Examples):** `svcscan` (Win).

* **Hooks and Callbacks (Advanced):**
    * **What:** Detect modifications in important system tables (like the System Service Dispatch Table - `SSDT`) or interception points (`IRP hooks`, `callbacks`) that malware uses to intercept or redirect operating system functions.
    * **Value:** Detection of `rootkits` and advanced evasion/hiding techniques.
    * **Volatility Plugins (Examples):** `ssdt` (Win), `driverirp` (Win), `callbacks` (Win).

## 🔑 Registry Artifacts in Memory (Windows)

> The Windows registry is partially loaded into memory. Analyzing it can reveal recent information or keys accessed by malware.

* **Loaded Hives:**
    * **What:** Find the memory location of the registry 'hive' files (`SAM`, `SYSTEM`, `SOFTWARE`, `NTUSER.DAT`, etc.).
    * **Value:** Necessary to later extract ('dump') these hives.
    * **Volatility Plugins (Examples):** `hivelist`.

* **Hive Dumping:**
    * **What:** Extract registry hives directly from memory to disk.
    * **Value:** Allows offline analysis of the hives with registry analysis tools (`Registry Explorer`, `RegRipper`) to find persistence keys, recent execution, etc., as they existed in memory (might differ slightly from disk).
    * **Volatility Plugins (Examples):** `hivedump`.

* **Key/Value Querying:**
    * **What:** Search and display the content of specific registry keys or values directly in memory.
    * **Value:** Useful for quickly checking specific values (e.g., Run keys) without dumping the entire hive.
    * **Volatility Plugins (Examples):** `printkey`.

## 🔒 Credentials and Secrets (Potential)

> Memory can contain sensitive credentials the system needs to operate.

* **Password Hashes (Windows):**
    * **What:** Extract user password LM/NTLM hashes from the `SAM` hive loaded in memory.
    * **Value:** Useful if the `SAM` file on disk cannot be accessed or to get the hashes as they existed in memory at that time. They can be attempted to be cracked offline.
    * **Volatility Plugins (Examples):** `hashdump`.

* **Other Secrets (Advanced):** Depending on the processes and system, sometimes encryption keys, session tokens, LSA secrets, etc., can be found with specific plugins (`lsa_secrets`, `mimikatz` plugin, etc.).

## 👤 Recent User Activity

> Remnants of user interaction with the system.

* **Executed Commands:**
    * **What:** Recover commands typed in `cmd.exe` or `PowerShell` consoles.
    * **Value:** See what commands a user or attacker executed recently.
    * **Volatility Plugins (Examples):** `cmdscan`, `consoles`.

* **Clipboard (`Clipboard`):**
    * **What:** Extract the content that was in the clipboard at the time of capture.
    * **Value:** May contain recently copied information (passwords, URLs, code snippets).
    * **Volatility Plugins (Examples):** `clipboard`.

## 🦠 Specific Malware Evidence

> Plugins designed to search for common malware traces.

* **Injected / Suspicious Code:**
    * **What:** Search for memory regions within processes that don't correspond to a known file on disk, or have unusual permissions (e.g., Executable _and_ Writable), indicating possible code injection.
    * **Value:** Detect malware operating by injecting itself into legitimate processes.
    * **Volatility Plugins (Examples):** `malfind`, `ldrmodules`, `hollowfind` (Process Hollowing detection).

* **YARA Scanning:**
    * **What:** Scan the entire memory dump (or specific process memory) using custom or known `YARA` rules.
    * **Value:** Detect patterns (strings, byte sequences) associated with known malware families or suspicious techniques.
    * **Volatility Plugins (Examples):** `yarascan`.

---

> _This list covers the most important artifacts. The main tool for extracting them is **`Volatility`**, whose usage we will detail in the next section._