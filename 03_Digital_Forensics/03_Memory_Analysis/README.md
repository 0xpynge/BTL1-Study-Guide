# 🧠 Memory Forensics Analysis (RAM)

> **Memory Forensics Analysis**, also known as volatile memory analysis or RAM analysis, is the process of examining the contents of a computer system's Random Access Memory (RAM), usually from a previously acquired **memory dump** (`memory dump`) file.

Unlike disk analysis which examines data "at rest", memory analysis captures a **snapshot of the system's running state** at the moment of acquisition. This information is extremely volatile and is lost when the system is shut down.

## ✨ Why is Memory Analysis Important?

> Memory analysis is vital because it captures volatile data not always written to disk, detects advanced malware (fileless, packed, rootkits), reveals recent activity, and can recover sensitive artifacts like credentials or keys.

1.  **Captures Volatile Data:** Obtains information not always written to disk, such as:
    * Running processes (including hidden or malicious ones).
    * Active and recent network connections.
    * Commands executed in terminals.
    * Loaded drivers.
    * Registry keys loaded in memory.
    * Clipboard contents.
2.  **Detects Advanced Malware:** It is especially useful against:
    * **Fileless Malware (`fileless`):** Which runs directly in memory without leaving an executable file on disk.
    * **Obfuscated or Packed Malware (`packed`):** Often, malware unpacks itself in memory, revealing its actual code.
    * **Rootkits:** Which can hide processes or files in the operating system but may be visible in a memory dump.
3.  **Reveals Recent Activity:** Can show what a user or process was doing just before acquisition.
4.  **Recovers Sensitive Artifacts:** Sometimes it's possible to find credentials (passwords, hashes), encryption keys, or fragments of files/communications that resided temporarily in memory.
5.  **Complements Disk Analysis:** Provides execution context for artifacts found on disk. _Was this malicious executable actually running? With what parameters? What did it connect to?_

## ⚠️ Challenges of Memory Analysis

* **Extreme Volatility:** Data changes constantly. Acquisition must be quick and at the right time.
* **Delicate Acquisition:** Must be performed on the live system, which can slightly alter the state and requires specific tools (see [`01_Acquisition.md`](../01_Acquisition.md)). There's a risk of corrupt or incomplete dumps.
* **Analysis Complexity:** Requires specialized tools (like `Volatility`) and a good understanding of how the operating system manages memory and its internal structures.

## 🔄 General Analysis Process

1.  **Acquisition:** Obtain the memory dump (`.raw`, `.dmp`, `.vmem`, etc.) from the target system.
2.  **Profile Identification (`Profile`):** Determine the exact operating system, architecture (x86/x64), and sometimes the Service Pack or build from the dump. This is **crucial** for the analysis tool to correctly interpret the data structures in memory.
3.  **Plugin Execution:** Use a tool like `Volatility` with its various plugins to extract specific information (process list, network connections, registry hives, etc.).
4.  **Results Analysis:** Interpret the output of the plugins to find evidence of malicious activity or relevance to the investigation.
5.  **Correlation:** Compare findings from memory with evidence obtained from disk and network analysis.

## 📂 Structure of this Subsection

Within this folder (`03_Memory_Analysis`), we will explore:

* **[Key Concepts](./Key_Concepts.md):** What specific types of information we can find in memory.
* **[Volatility Tool](./Volatility_Tool.md):** How to use the `Volatility` framework (the de facto standard) to perform the analysis.

## 🎯 BTL1 Focus

> Memory analysis is a frequent and important part of BTL1 scenarios. Candidates are expected to be able to use **`Volatility`** to extract key information from a provided memory dump and answer questions about processes, networks, and other artifacts in memory.

---

> _Memory analysis opens a unique window into a system's execution state, revealing activity that would otherwise be invisible._