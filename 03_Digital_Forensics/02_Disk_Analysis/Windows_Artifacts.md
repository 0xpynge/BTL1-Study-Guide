# 🪟 Key Forensic Artifacts in Windows

> Windows records a huge amount of information about system and user activity. Knowing where to look for these artifacts is fundamental for any forensic investigation on this operating system.

## 🗝️ Windows Registry (`Windows Registry`)

> A hierarchical database storing low-level configurations for the operating system and applications. It's a goldmine for forensic information.

* **Forensic Value:** Malware persistence, user accounts, connected USB devices, executed programs, network history, recent files, and much more.
* **Structure (Hives):** Main files where the registry is stored. The most important ones are:
    * `SAM`: Local user account and group information (`C:\Windows\System32\config\SAM`).
    * `SECURITY`: Security policies, logon cache (`C:\Windows\System32\config\SECURITY`).
    * `SOFTWARE`: System-level software configurations (`C:\Windows\System32\config\SOFTWARE`).
    * `SYSTEM`: Hardware and system service configuration (`C:\Windows\System32\config\SYSTEM`).
    * `NTUSER.DAT`: User-specific profile settings (per user: `C:\Users\<username>\NTUSER.DAT`).
    * `UsrClass.dat`: Information related to COM and user interface settings (`C:\Users\<username>\AppData\Local\Microsoft\Windows\UsrClass.dat`).
* **Keys/Areas of Interest (Examples):**
    * **Persistence:** `Run`, `RunOnce`, `Services` (in `SOFTWARE` and `SYSTEM`), `Scheduled Tasks` (see separate section).
    * **Execution:** `UserAssist` (`NTUSER.DAT`), `Shimcache`/`AppCompatCache` (`SYSTEM`), `Amcache.hve` (own hive file).
    * **USB Devices:** `USBStor` (`SYSTEM`).
    * **Recent/Accessed Files:** `RecentDocs` (`NTUSER.DAT`), `OpenSavePidlMRU` (`NTUSER.DAT`), `Shellbags` (`NTUSER.DAT`, `UsrClass.dat`).
    * **Network:** `NetworkList` (`SOFTWARE`), `Interfaces` (`SYSTEM`).
    * **Web History (IE/Edge Legacy):** `TypedURLs` (`NTUSER.DAT`).
* **Tools:** Registry Editor (live), `RegRipper`, `Registry Explorer` (Eric Zimmerman), `Autopsy`/`FTK` modules.

## 📜 Event Logs (`Event Logs`)

> Chronological logs of important system, security, and application events.

* **Forensic Value:** Logons (success/failure), process creation, service installation, policy changes, log clearing, application errors, etc.
* **Format:** `.evt` (Windows XP/2003), `.evtx` (Vista and later).
* **Main Location:** `C:\Windows\System32\winevt\Logs\`
* **Key Logs:** `Security.evtx`, `System.evtx`, `Application.evtx`. Many other specific logs exist (PowerShell, TaskScheduler, WMI, etc.).
* **Key Event IDs (Examples in `Security.evtx`):**
    * `4624`: Successful Logon.
    * `4625`: Failed Logon.
    * `4634`/`4647`: Logoff.
    * `4688`: Process Creation (requires advanced auditing enabled).
    * `4720`: User Account Created.
    * `1102`: Security Audit Log Cleared.
    * `7045` (System Log): Service Installation.
* **Tools:** Windows Event Viewer, `EvtTxECmd` (Eric Zimmerman), `Chainsaw`, `LogParser`, forensic suite modules, SIEMs.

## 🚀 Program Execution Evidence

> Various artifacts track which programs have executed, when, and how often.

* **Prefetch (`.pf`):**
    * **Value:** Records program executions to speed up future loads. Stores: executable name, execution count, last 8 execution times, files loaded by the program.
    * **Location:** `C:\Windows\Prefetch\`
    * **Tools:** `PECmd` (Eric Zimmerman), `Autopsy`.

* **Superfetch / SysMain (`Ag*.db`):**
    * **Value:** Similar to Prefetch, aims to optimize application loading based on usage patterns. Its database can contain execution evidence.
    * **Location:** `C:\Windows\Prefetch\`. ESE database format.
    * **Tools:** `SrumECmd` can parse related data.

* **Amcache / AppCompatCache (Shimcache):**
    * **Value:** Track program execution for compatibility purposes. Can contain information about executables even if Prefetch is disabled or the file has been deleted.
    * **Location:** Shimcache (in memory and in the `SYSTEM` Registry hive), Amcache (own hive at `C:\Windows\AppCompat\Programs\Amcache.hve`).
    * **Tools:** `AppCompatCacheParser`, `AmcacheParser` (Eric Zimmerman), `RegRipper`, `Autopsy`.

* **SRUM (`SRUDB.dat`):**
    * **Value:** System Resource Usage Monitor. Very detailed database on process executions, network usage per process/user, etc. Extremely useful but complex.
    * **Location:** `C:\Windows\System32\sru\SRUDB.dat` (ESE Database).
    * **Tools:** `SrumECmd` (Eric Zimmerman).

## 🔗 File and Folder Access Evidence

> Artifacts indicating which files, folders, or applications a user has used.

* **LNK Files (`.lnk`):**
    * **Value:** Shortcuts created (automatically or manually) when accessing files/programs. Store the target's original path, its MACB timestamps, and information about the volume where it resided (type, serial number). Evidence of access to files/paths (even on removable drives).
    * **Location:** Desktop, Start Menu, `%UserProfile%\AppData\Roaming\Microsoft\Windows\Recent\`, and embedded in Jumplists.
    * **Tools:** `LECmd` (Eric Zimmerman), `Autopsy`.

* **Jumplists:**
    * **Value:** Lists of recent files/destinations associated with applications (e.g., recent Word files, recent Explorer folders). Contain data similar to LNK files.
    * **Location:** `%UserProfile%\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations\` and `CustomDestinations\`.
    * **Tools:** `JLECmd` (Eric Zimmerman), `Autopsy`.

* **Shellbags:**
    * **Value:** The Registry stores Explorer folder view preferences (icon size, view...). Crucially, it records folders a user has navigated, **even if the folder no longer exists or was on removable media!** Evidence of knowledge/access to specific paths.
    * **Location:** `NTUSER.DAT` and `UsrClass.dat` hives.
    * **Tools:** `SBECmd` (Eric Zimmerman), `Registry Explorer`, `Autopsy`.

## 🗑️ Recycle Bin (`$Recycle.Bin`)

* **Value:** Contains files deleted by the user via Windows Explorer. Allows recovering files and obtaining metadata about the deletion.
* **Location:** Hidden folder `$Recycle.Bin` at the root of each volume, with subfolders per user SID. Inside, `$I` files (metadata: original path, deletion date) and `$R` files (file content).
* **Tools:** File system browsers (`Autopsy`, `FTK Imager`), `RBCmd` (Eric Zimmerman).

## ⏰ Scheduled Tasks (`Scheduled Tasks`)

* **Value:** Legitimate mechanism for automating tasks, but heavily used by malware for persistence (run at startup, periodically).
* **Location:** `C:\Windows\System32\Tasks\` (XML files defining the task), also in the Registry (older versions).
* **Tools:** Task Scheduler Viewer (live), direct XML analysis, `Autoruns` (Sysinternals), `Autopsy`.

## 📄 NTFS File System Artifacts

> The NTFS file system itself stores very valuable metadata.

* **`$MFT` (Master File Table):**
    * **Value:** The "index" of the entire NTFS volume. Contains at least one record for every file and directory, with its attributes (name, size, MACB timestamps, permissions) and, for small files, even the data itself ("resident"). Persists information even after files are deleted (the record is marked as unused, but not immediately overwritten). Fundamental for timelines and file recovery.
    * **Tools:** `MFTECmd` (Eric Zimmerman), `analyzeMFT`, `Autopsy`/`TSK`.

* **`$LogFile`:**
    * **Value:** NTFS transactional log. Records metadata operations before they are written to the MFT. May contain information about very recent or recently deleted files.
    * **Tools:** `MFTECmd` (Eric Zimmerman), `LogFileParser`.

* **`$UsnJrnl` (Update Sequence Number Journal):**
    * **Value:** Log of changes made to files and directories (creation, deletion, renaming, etc.). Very useful for tracking file activity over a period of time.
    * **Location:** `$Extend\$UsnJrnl` (hidden system file).
    * **Tools:** `MFTECmd` (Eric Zimmerman), `UsnJrnl_parser`, `Autopsy`.

* **ADS (Alternate Data Streams):**
    * **Value:** NTFS feature allowing data to be attached to a file "hiddenly" (not visible in the normal file size). Sometimes used by malware to hide executables or configurations.
    * **Tools:** `dir /r` (CMD), `Get-Item -Stream *` (PowerShell), forensic suites usually detect them.

## 🌐 Web Browse History

* **Value:** Sites visited, downloads, searches, cookies, cache. Crucial for tracking online activity.
* **Location:** Varies greatly by browser (Chrome, Firefox, Edge...). Generally in subfolders of `%UserProfile%\AppData\Local\` or `Roaming\`. Usually SQLite databases.
* **Tools:** Browser-specific tools (e.g., from NirSoft), SQLite viewers (DB Browser for SQLite), `Autopsy`/`FTK` modules.

---