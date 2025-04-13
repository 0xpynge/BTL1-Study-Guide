# 🕵️‍♀️ Digital Forensics: Reconstructing the Incident

> **Digital Forensics** is the branch of forensic science that deals with the recovery, investigation, examination, and analysis of material found on digital devices, often in relation to security incidents or computer crimes. The main objective is to reconstruct events based on digital evidence to understand **what happened, how, when, where**, and potentially, **who** was involved.

In the context of defensive cybersecurity (Blue Team) and BTL1, digital forensics is essential for investigating compromised systems, analyzing malware, understanding an attacker's post-intrusion actions, and recovering relevant data.

## 📜 Fundamental Principles of Digital Forensics

Although BTL1 focuses on practical application within scenarios, it's vital to understand the principles guiding any forensic investigation to ensure the integrity and reliability of the results:

1.  **Evidence Preservation:**
    * **Objective:** Minimize alteration of the original evidence. Whenever possible, work is done on a **bit-by-bit copy (forensic image)** of the original media, not directly on the original.
    * **Actions:** Use write blockers (hardware/software) when connecting the original media, document every step.

2.  **Chain of Custody (`Chain of Custody`):**
    * **Objective:** Maintain a detailed chronological record of who has had control over the evidence from its collection to its final presentation.
    * **Actions:** Document collection (date, time, person, method), secure storage, custody transfers, and every access for analysis. This ensures the evidence has not been tampered with. (In BTL1, this translates to **meticulously documenting your analysis steps**).

3.  **Exhaustive Documentation:**
    * **Objective:** Record every action taken, tool used, command executed, and finding observed during the analysis.
    * **Actions:** Take detailed notes, relevant screenshots, export tool results. Good documentation allows another analyst to reproduce your steps and validate your conclusions (reproducibility).

4.  **Order of Volatility (`Order of Volatility`):**
    * **Objective:** Collect evidence starting with the most volatile (the one that disappears most quickly) and ending with the most persistent.
    * **General Order (Most to Least Volatile):**
        1. CPU Registers, Cache
        2. Routing Table, ARP Cache, Process Table, RAM
        3. Temporary Network Connections
        4. Hard Drive / SSD Data
        5. Remote System Logs
        6. Backups
    * **Importance:** Information in RAM (processes, connections) is lost when the system is shut down. Therefore, memory analysis (if a dump is available) or live response often precedes offline disk analysis.

## 🔍 Main Types of Analysis in BTL1

This section of the repository will focus on the key areas of digital forensics relevant to BTL1:

* **[Evidence Acquisition](./01_Acquisition.md):** How to create forensic images of disks and memory and verify their integrity (hashing).
* **[Disk Analysis](./02_Disk_Analysis/):** Examining file systems (`Windows NTFS`, `Linux ext4`), recovering deleted files, analyzing operating system artifacts (logs, registry, user history), and extracting metadata. Includes techniques like File Carving.
* **[Memory Analysis](./03_Memory_Analysis/):** Investigating RAM dumps to find hidden processes, past network connections, executed commands, memory-resident malware, etc., using tools like `Volatility`.

## 🎯 BTL1 Focus

> BTL1 assesses your ability to **apply specific forensic techniques** and **use standard tools** (`Autopsy`, `Volatility`, `FTK Imager`, `Scalpel`, `ExifTool`, `KAPE`, `TSK`, etc.) to find answers to specific questions within an incident scenario. A legal forensic expert level is not expected, but rather the correct application of basic principles and tools.

---

---

