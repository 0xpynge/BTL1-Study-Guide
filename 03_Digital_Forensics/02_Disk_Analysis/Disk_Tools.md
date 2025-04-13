# 🛠️ Common Disk Analysis Tools

Numerous tools exist for disk forensic analysis, from complete suites to specialized command-line utilities. Here we describe some of the most relevant and common ones, especially in the context of BTL1.

## 💻 Integrated Forensic Suites (GUI)

These platforms provide a graphical interface and combine multiple analysis functionalities.

* **[Autopsy](https://www.sleuthkit.org/autopsy/)**:
    > A free and **open-source** digital forensic platform, widely used by both beginners and professionals. It is essentially a graphical interface for **The Sleuth Kit** (see below) and other tools.
    * **Key Features:**
        * Processing of disk images (`RAW/dd`, `E01`, etc.).
        * Detailed analysis of file systems (`NTFS`, `FAT`, `ext`, `HFS+`, etc.).
        * Automatic extraction of artifacts (web history, registry activity, emails, Exif metadata, etc.) using modules ("Ingest Modules").
        * Keyword searching (indexed or literal).
        * File viewer (hex, text, images, etc.).
        * Creation of timelines (`Timelines`) of file system activity and other artifacts.
        * Integrated file carving to recover deleted files.
        * Report generation.
    * **Basic Use (Conceptual - Mostly GUI):**
        1. Create a new case (`New Case`).
        2. Add a data source (`Add Data Source`), selecting the disk image.
        3. Configure and run the ingest modules (`Ingest Modules`) for automatic analysis.
        4. Explore the directory tree, extracted artifacts, timeline views, etc.
    * **CLI Commands (Less Common for General Analysis):** `Autopsy` has some modules that can be used from the command line, but the main analysis is via GUI. Commands like `autopsy -i <image>` or `autopsy -p <project>` are usually for launching the application or specific modules in more advanced or scripting contexts.

* **FTK (Forensic Toolkit - AccessData/Exterro)**:
    > A very powerful and recognized commercial forensic suite in the industry. Similar in features to `Autopsy` but with additional characteristics and often considered faster in processing large volumes of data.
    * **Note:** Not to be confused with **`FTK Imager`** (free and focused on acquisition and previewing), although they integrate. Using the full **FTK** is less likely in learning scenarios like BTL1 due to its cost.

## ⌨️ Frameworks and Toolkits (CLI)

> Command-line tools offering great power and flexibility, often used by experienced analysts or for scripting.

* **[The Sleuth Kit (TSK)](https://www.sleuthkit.org/)**:
    > The **open-source** engine behind `Autopsy`. It's a collection of command-line tools for deep forensic analysis of file systems and disk images. Allows accessing data at a low level.
    * **Key Tools (Examples):**
        * `fsstat`: Displays file system details (type, block size, etc.).
        * `mmls`: Lists the partitions within a disk image.
        * `fls`: Lists files and directories (similar to `ls`), including deleted ones.
        * `icat`: Extracts the content of a file based on its inode number (metadata node).
        * `ffind`: Finds filenames pointing to a specific inode.
        * `ifind`: Finds the inode pointing to a specific filename.
        * `tsk_recover`: Attempts to recover deleted files.
        * `tsk_gettimes`: Extracts MACB timestamps.
    * **Usage:** Powerful for specific analysis or scripting, requires deeper knowledge of file systems.

* **[Eric Zimmerman's Tools (EZ Tools)](https://ericzimmerman.github.io/)**:
    > An **essential** suite of **free** and highly efficient tools developed by Eric Zimmerman, focused on parsing specific **Windows** artifacts. They are the de facto standard for many analyses.
    * **Key Tools (Examples):** `Registry Explorer`/`RECmd` (Registry), `EvtTxECmd` (Event Logs), `PECmd` (Prefetch), `AmcacheParser`, `SrumECmd`, `LECmd` (LNK files), `JLECmd` (Jumplists), `SBECmd` (Shellbags), `MFTECmd` (`$MFT`, `$LogFile`, `$UsnJrnl`), `RBCmd` (Recycle Bin), and many more.
    * **Usage:** Run from the Windows command line, they are fast and generate output in easy-to-process formats (e.g., CSV).

## 🔧 Specific and Complementary Tools

* **[KAPE (Kroll Artifact Parser and Extractor)](https://www.kroll.com/en/services/cyber-risk/kape)**:
    > Extremely efficient tool to **collect** (Targets) and/or **process** (Modules) key forensic artifacts from a live system or mounted image.
    * **Usage:** Ideal for rapid triage or for extracting specific artifacts and then analyzing them with other tools (like Eric Zimmerman's, which can be integrated as Modules in KAPE).
    * **Basic Commands (Examples):**
      ```powershell
      # Example: Extract Registry Hives from C: and process with Registry Explorer
      kape.exe --tsource C: --tdest C:\KAPE_Output\ --target RegistryHives --module RegistryExplorer
      ```
      ```powershell
      # Example: Collect basic Windows info from a mounted image path
      kape.exe --tsource \\path\to\mounted_image --tdest C:\KAPE_Output\ --target Windows_Basic_Info 
      ```

* **Specific Log Parsers:**
    * **`LogParser` (Microsoft):** Powerful CLI tool allowing SQL-like syntax to query various log file types (text, CSV, EVTX, XML, etc.). Steep learning curve, but very flexible.
    * Standard command-line tools (`grep`, `awk`, `sed` on Linux/WSL) for filtering text logs.

* **Hex Viewers:**
    * **Examples:** [HxD](https://mh-nexus.de/en/hxd/) (Windows, free), [010 Editor](https://www.sweetscape.com/010editor/) (Commercial, powerful with binary templates).
    * **Usage:** Allow viewing and editing the raw content (in hexadecimal and ASCII) of any file or even a disk/image. Useful for manually searching file headers/footers (file carving), analyzing unknown file structures, finding hidden data.

---

> _The choice of tool often depends on the specific task, the analyzed operating system, and the analyst's personal preferences. It's common to use a combination of GUI suites and specialized CLI tools._