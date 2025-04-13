# 💾 Disk Forensic Analysis

> **Disk forensic analysis** is the process of examining the content of digital storage media (HDD hard drives, SSD solid-state drives, USB drives, etc.) to extract relevant evidence about system and user activity. It is one of the most fundamental areas of digital forensics.

Working on a previously acquired forensic image (see section [`01_Acquisition.md`](../01_Acquisition.md)), the objective is to find answers to questions such as: What programs were executed? What files were created, modified, or deleted? Was there access to external resources? Are there traces of malware? When did certain events occur?

## 🎯 Common Objectives of Disk Analysis

* **Identify User Activity:** Logins, program execution, files opened/modified/deleted, searches performed, web Browse history, communications.
* **Find Malware Artifacts:** Executables, scripts, configuration files, persistence registry keys, C2 indicators.
* **Recover Deleted Data:** Attempt to recover files no longer in the active file system (from unallocated space, slack space, etc.).
* **Establish Timelines (`Timelines`):** Correlate timestamps from different artifacts to reconstruct the sequence of events.
* **Analyze File System Structure:** Understand how data is organized (`NTFS`, `ext4`, `FAT32`...) and extract important metadata (MACB timestamps - Modify, Access, Change, Birth).

## 🧭 General Methodological Approach

1.  **Secure Mounting:** Mount the forensic image in an analysis environment, preferably in **read-only** mode to prevent accidental modifications.
2.  **Tools:** Utilize integrated forensic suites (like `Autopsy`, `FTK`) which automate many processes, or specialized tools for specific tasks (`TSK`, `KAPE`, Eric Zimmerman's tools, etc.).
3.  **Analysis of Specific Artifacts:** Focus on known operating system artifacts (Windows or Linux) that record relevant activity.
4.  **Keyword Search:** Search for relevant terms (usernames, filenames, IPs, domains) across the entire disk or in specific areas.
5.  **Timeline Creation:** Aggregate and sort events based on their timestamps.
6.  **Rigorous Documentation:** Note down each finding, its location, the tool used, and its relevance.

## 📂 Structure of this Subsection

Within this folder (`02_Disk_Analysis`), we will explore the following topics in detail:

* **[Key Artifacts in Windows](./Windows_Artifacts.md):** Registry, Event Logs, Prefetch, Shellbags, LNK, Jumplists, `$MFT`, `$UsnJrnl`, SRUM, etc.
* **[Key Artifacts in Linux](./Linux_Artifacts.md):** Logs (`/var/log`), command history, `cron jobs`, user accounts, `/proc`, etc.
* **[Common Disk Analysis Tools](./Disk_Tools.md):** Introduction to `Autopsy`, `The Sleuth Kit (TSK)`, `KAPE`, and others.
* **[File Carving with Scalpel](./File_Carving_Scalpel.md):** Technique for recovering files based on their headers/footers.
* **[Metadata Analysis with ExifTool](./Metadata_ExifTool.md):** Extracting hidden information within files.

**Note:** A basic understanding of file systems like **`NTFS`** (Master File Table - `$MFT`, `$LogFile`, `$UsnJrnl`, Alternate Data Streams - `ADS`) and **`ext4`** (inodes, journal) is very beneficial for knowing where to look for evidence.

---