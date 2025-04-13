# 🛠️ Essential Tool: The Volatility Framework

> **`Volatility`** is the leading **open-source** framework and de facto standard for volatile memory (RAM) forensics analysis. Written in Python, it allows extracting digital artifacts from memory dumps of a wide variety of operating systems (Windows, Linux, macOS, Android).

It works using a **plugin** system, each designed to extract a specific type of information from the memory dump.

## ⚠️ Volatility 2 vs. Volatility 3: Important!

> Two main versions of Volatility exist, and understanding their differences is crucial, as the syntax and some plugins vary:

* **Volatility 2 (Legacy):**
    * Based on Python 2 (although Python 3 compatible forks exist).
    * Relies **heavily** on **Profiles (`Profiles`)**: Specific pre-built definitions for each Operating System version, Service Pack, and architecture (e.g., `Win7SP1x64`, `Win10x64_14393`, `LinuxDebian10x64`).
    * **Identifying and specifying the correct profile is mandatory** for plugins to work properly. The `imageinfo` command (V2) is used to identify possible profiles.
    * **Typical Syntax:** `python vol.py --profile=<ProfileName> -f <dump_file> <v2_plugin> [plugin_options]`
    * Still widely used and has extensive online documentation/tutorials. Might be the version found in some exam/CTF environments.

* **Volatility 3 (Current):**
    * Based on Python 3.
    * **Goal:** To be "profile-less". Instead of profiles, it uses **Symbol Tables (`Symbol Tables`)**. Tries to detect the OS automatically and download/find the necessary symbol tables.
    * **Typical Syntax:** `python vol.py -f <dump_file> <plugin.with.dots> [plugin_options]` (e.g., `windows.pslist.PsList`).
    * It's the version under active development and the future of the framework.

**Which one to use in BTL1?** **Consult the documentation or environment provided by BTL1.** It's vital to know which version they expect you to use, as it directly affects the commands you will execute. This guide will primarily show Volatility 3 syntax and plugin names but will mention Volatility 2 equivalents when relevant.

## ⚙️ Basic Installation and Setup

* **Requirement:** Python (preferably Python 3 for Volatility 3).
* **Installation:** Generally via `pip install volatility3` or cloning the repository from GitHub and installing dependencies (`pip install -r requirements.txt`).
* **Volatility 3 Symbol Packs:** V3 needs symbol tables. It tries to download them automatically, but if it fails, you might need to download them manually ([Volatility 3 Symbol Packs](https://github.com/volatilityfoundation/symbol-packs)) and place them in the `volatility3/symbols/` folder or a location specified in your configuration.

## 🔄 Basic Usage Workflow

1.  **Identify Dump Information:**
    * **Vol3:** `python3 vol.py -f <dump_file> windows.info.Info` or `linux.info.Info`. Usually detects the OS automatically.
    * **Vol2:** `python vol.py -f <dump_file> imageinfo`. **Crucial** for obtaining the list of suggested profiles (`Suggested Profile(s)`).

2.  **Execute Plugins:**
    * **Vol3:** `python3 vol.py -f <dump_file> <plugin.with.dots> [options]`
    * **Vol2:** `python vol.py --profile=<ChosenProfile> -f <dump_file> <v2_plugin> [options]`

3.  **Analyze Output:** Interpret the information returned by the plugin.

4.  **Filter Results (Optional):** For very long outputs, you can pipe to `grep` (Linux) or `findstr` (Windows) to filter by keywords, or process the output with scripts.
    * Example: `python3 vol.py -f <dump_file> windows.pslist.PsList | grep suspicious.exe`

## 🔌 Key Plugins (Common V3 / V2 Examples)

> This table lists some of the most useful plugins, many relevant for BTL1. (Notation is V3, with V2 equivalent in parentheses if different).

| Task/Objective           | Volatility 3 Plugin (Example)               | Volatility 2 Plugin (Example) | Notes                                                         |
| :----------------------- | :------------------------------------------ | :---------------------------- | :------------------------------------------------------------ |
| **General Info** | `windows.info.Info` / `linux.info.Info`       | `imageinfo`                   | Identifies OS, version, arch. Crucial for V2 (get Profile).   |
| **Processes** | `windows.pslist.PsList` / `linux.pslist.PsList` | `pslist`                      | Lists active processes (similar `tasklist`/`ps`).             |
|                          | `windows.pstree.PsTree` / `linux.pstree.PsTree` | `pstree`                      | Shows processes in a tree (parent-child relationships).       |
|                          | `windows.psscan.PsScan` / `linux.psscan.PsScan` | `psscan`                      | Scans for process structures (finds hidden/terminated).       |
|                          | `windows.cmdline.CmdLine` / `linux.cmdline.CmdLine` | `cmdline`                     | Shows command-line arguments.                                 |
|                          | `windows.dlllist.DllList` / `linux.ldrmodules.LdrModules` | `dlllist` / `ldrmodules`      | Lists DLLs/libraries loaded by process.                       |
|                          | `windows.handles.Handles`                   | `handles`                     | Lists open handles by process (files, reg keys, etc.).        |
|                          | `windows.memmap.Memmap` / `linux.proc.Maps` | `memmap` / `proc_maps`        | Shows the memory map of a process.                            |
|                          | `windows.procdump.ProcDump` / `linux.procdump.ProcDump` | `procdump`                    | Dumps the **executable** memory of a process to a file.     |
| **Networking** | `windows.netscan.NetScan` / `linux.netstat.NetStat` | `netscan` / `netstat`         | Shows connections (TCP/UDP) and listening ports.              |
|                          | `windows.netstat.NetStat`                   | `connections`/`connscan`      | (V3 Win) Alt to NetScan. (V2) `connscan` finds closed TCP.  |
| **Registry (Win)** | `windows.registry.hivelist.HiveList`      | `hivelist`                    | Lists registry hives loaded in memory.                        |
|                          | `windows.registry.printkey.PrintKey`      | `printkey`                    | Shows value(s) of a specific registry key in memory.          |
|                          | `windows.registry.hivedump.HiveDump`      | `hivedump`                    | Dumps a complete hive from memory to a file.                  |
| **System** | `windows.modules.Modules` / `linux.lsmod.Lsmod` | `modules` / `lsmod`           | Lists loaded kernel modules/drivers.                          |
|                          | `windows.modscan.ModScan` / `linux.modscan.ModScan` | `modscan`                     | Scans for module structures (finds hidden).                   |
|                          | `windows.svcscan.SvcScan`                   | `svcscan`                     | Scans registered services (Windows).                          |
| **Files in Memory** | `windows.filescan.FileScan` / `linux.filescan.FileScan` | `filescan`                    | Scans for file objects in memory.                             |
|                          | `windows.dumpfiles.DumpFiles` / `linux.dumpfiles.DumpFiles` | `dumpfiles`                   | Dumps cached/mapped files from memory to disk.                |
| **Malware/Misc** | `windows.malfind.Malfind` / `linux.malfind.Malfind` | `malfind`                     | Searches for injected/suspicious code in process memory.      |
|                          | `windows.cmdscan.CmdScan`                   | `cmdscan`/`consoles`          | Searches for commands in consoles (`cmd.exe`).                |
|                          | `windows.clipboard.Clipboard`               | `clipboard`                   | Extracts clipboard content.                                   |
|                          | `timeliner.Timeliner`                       | `timeliner`                   | Creates a timeline with events from various plugins.          |
|                          | `yarascan.YaraScan`                         | `yarascan`/`linux_yarascan`   | Scans memory with YARA rules.                                 |
|                          | `windows.hashdump.Hashdump`                 | `hashdump`                    | Dumps LM/NTLM hashes from memory (Windows).                   |

*(Note: Exact availability and plugin names may vary slightly between minor versions. Always consult Volatility's own help: `python3 vol.py -h` or `python vol.py -h`)*

---

> _Mastering **`Volatility`** is essential for memory analysis. Start with the basic plugins (`pslist`, `netscan`, `cmdline`, `dlllist`) and explore others as needed._