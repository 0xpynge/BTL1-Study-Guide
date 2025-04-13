# 🛡️ Practical Study Guide for BTL1 (Blue Team Level 1) 🛡️

<p align="center">
  <img src="assets/img/banner.jpg" alt="BTL1 Study Guide Banner" width="80%"> 
  </p>

<p align="center">
  <a href="https://elearning.securityblue.team/home/certifications/blue-team-level-1" target="_blank">
    <img src="https://img.shields.io/badge/Certification-BTL1_Official-0078D4?style=for-the-badge" alt="Official BTL1 Certification">
  </a>
  <a href="./LICENSE" target="_blank">
    <img src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge" alt="License">
  </a>
  <img src="https://img.shields.io/badge/Status-Work_In_Progress-orange?style=for-the-badge" alt="Status: Work In Progress">
</p>

> **Welcome to this personal study guide for preparing for the Security Blue Team BTL1 certification.**
> 
> This repository compiles notes, cheatsheets, workflows, and resources aiming to help you consolidate the practical skills needed to tackle the BTL1 exam and grow as a cyber defense (Blue Team) professional.
> 
> The focus is **eminently practical**, reflecting the nature of the BTL1 exam. It centers on knowing _what to do_, _how to do it_, and _with which tools_ in realistic scenarios.
---

## 🎯 Who is This For?

* Students actively preparing for the BTL1 certification.
* Junior Cybersecurity Professionals (SOC Analysts Tier 1, Jr. Incident Responders) looking to strengthen practical foundations.
* Cyber defense enthusiasts seeking a practical and centralized guide.
## ✨ Key Features:

* **Practical Focus:** Aligned with the BTL1 exam philosophy.
* **Detailed Cheatsheets:** Key commands and steps for fundamental tools.
* **Suggested Workflows:** Step-by-step guides for common tasks.
* **Modular Structure:** Organized by BTL1 domains for focused study.
* **Selected Resources:** Direct links to tools, documentation, and practice sites.

---

## 🛠️ Main Tools Covered

This repository delves into the practical use of key tools found in the BTL1 environment and the day-to-day work of a security analyst:

* **SIEM:** `Splunk` (SPL), `Elastic Stack` (ELK/Basic KQL)
* **Network Analysis:** `Wireshark`, `Tshark`
* **Memory Forensics:** `Volatility 2/3`
* **Disk Forensics:** `Autopsy`, `The Sleuth Kit (TSK)`, `FTK Imager`
* **Rapid Collection & Analysis:** `KAPE` (Kroll Artifact Parser and Extractor)
* **Malware/File Analysis:** `VirusTotal`, `Hybrid Analysis`, `Any.Run`, `ExifTool`
* **File Carving:** `Scalpel`, `Foremost`
* **Log Analysis:** Native commands (Linux/Windows), `Sysinternals Suite`
* **Threat Intelligence:** `MITRE ATT&CK Framework`, `URLhaus`, `AbuseIPDB`
---

## 🧭 Repository Structure

The guide is divided into modules corresponding to the core BTL1 domains:

| Module                     | Brief Description                                                          |
| :------------------------- | :------------------------------------------------------------------------- |
| `00_Introduction_BTL1`   | BTL1 fundamentals, mindset, and exam strategy.                             |
| `01_Phishing_Analysis`   | Dissecting emails, analyzing URLs and attachments.                         |
| `02_Threat_Intelligence` | Practical application of CTI, IoCs, TTPs, and MITRE ATT&CK.                |
| `03_Digital_Forensics`   | Acquisition and analysis of digital evidence on disk and memory (Win/Lin). |
| `04_SIEM_Analysis`       | Searching, correlating, and analyzing centralized logs (`Splunk`).           |
| `05_Network_Analysis`    | Interpreting network traffic, identifying anomalies and protocols (PCAPs). |
| `06_Incident_Response`   | Techniques and commands for initial identification and analysis on live systems. |
| `assets`                 | Images, diagrams, and other supporting visual resources.                   |
---

## 🗺️ Detailed Table of Contents

<details>
<summary><strong>► Click to expand/collapse Detailed Table of Contents</strong></summary>
<br> * [**🚀 Introduction to BTL1**](./00_Introduction_BTL1/)
    * [What is BTL1?](./00_Introduction_BTL1/01_What_is_BTL1.md)
    * [Exam Philosophy](./00_Introduction_BTL1/02_Exam_Philosophy.md)
    * [General Strategy](./00_Introduction_BTL1/03_General_Strategy.md)
* [**🎣 Phishing Analysis**](./01_Phishing_Analysis/)
    * [Key Concepts](./01_Phishing_Analysis/01_Key_Concepts.md)
    * [Tools](./01_Phishing_Analysis/02_Tools.md)
    * [Commands Cheatsheet](./01_Phishing_Analysis/03_Commands_Cheatsheet.md)
    * [Analysis Workflow](./01_Phishing_Analysis/04_Analysis_Workflow.md)
    * [Practice Resources](./01_Phishing_Analysis/05_Practice_Resources.md)
* [**💡 Threat Intelligence**](./02_Threat_Intelligence/)
    * [Key Concepts](./02_Threat_Intelligence/01_Key_Concepts.md)
    * [MITRE ATT&CK](./02_Threat_Intelligence/02_MITRE_ATTACK.md)
    * [Tools](./02_Threat_Intelligence/03_Tools.md)
    * [IoC Workflow](./02_Threat_Intelligence/04_IoC_Workflow.md)
    * [Practice Resources](./02_Threat_Intelligence/05_Practice_Resources.md)
* [**🕵️ Digital Forensics**](./03_Digital_Forensics/)
    * [Principles and Acquisition](./03_Digital_Forensics/01_Acquisition.md)
    * [Disk Analysis](./03_Digital_Forensics/02_Disk_Analysis/)
        * [Windows Artifacts](./03_Digital_Forensics/02_Disk_Analysis/Windows_Artifacts.md)
        * [Linux Artifacts](./03_Digital_Forensics/02_Disk_Analysis/Linux_Artifacts.md)
        * [Disk Tools (`Autopsy`, `TSK`, `KAPE`)](./03_Digital_Forensics/02_Disk_Analysis/Disk_Tools.md)
        * [File Carving (`Scalpel`)](./03_Digital_Forensics/02_Disk_Analysis/File_Carving_Scalpel.md)
        * [Metadata (`ExifTool`)](./03_Digital_Forensics/02_Disk_Analysis/Metadata_ExifTool.md)
    * [Memory Analysis](./03_Digital_Forensics/03_Memory_Analysis/)
        * [Key Concepts](./03_Digital_Forensics/03_Memory_Analysis/Key_Concepts.md)
        * [`Volatility` Tool](./03_Digital_Forensics/03_Memory_Analysis/Volatility_Tool.md)
    * [Practice Resources](./03_Digital_Forensics/04_Practice_Resources.md)
* [**📊 SIEM Analysis**](./04_SIEM_Analysis/)
    * [Key Concepts](./04_SIEM_Analysis/01_Key_Concepts.md)
    * [`Splunk` Cheatsheet](./04_SIEM_Analysis/02_Splunk_Cheatsheet.md)
    * [Common Searches](./04_SIEM_Analysis/04_Common_Searches.md)
    * [Practice Resources](./04_SIEM_Analysis/05_Practice_Resources.md)
* [**🌐 Network Analysis**](./05_Network_Analysis/)
    * [Key Concepts](./05_Network_Analysis/01_Key_Concepts.md)
    * [`Wireshark` / `Tshark`](./05_Network_Analysis/02_Wireshark_Tshark.md)
    * [Filters Cheatsheet](./05_Network_Analysis/03_Filters_Cheatsheet.md)
    * [Specific Protocol Analysis](./05_Network_Analysis/04_Specific_Protocol_Analysis.md)
    * [Malicious Patterns](./05_Network_Analysis/05_Malicious_Patterns.md)
    * [Practice Resources](./05_Network_Analysis/06_Practice_Resources.md)
* [**🔥 Incident Response**](./06_Incident_Response/)
    * [IR Lifecycle](./06_Incident_Response/01_IR_Lifecycle.md)
    * [Live Response Windows](./06_Incident_Response/02_Live_Response_Windows.md)
    * [Live Response Linux](./06_Incident_Response/03_Live_Response_Linux.md)
    * [Containment & Eradication](./06_Incident_Response/04_Containment_Eradication.md)
    * [Practice Resources](./06_Incident_Response/05_Practice_Resources.md)
    </details>

---

## ✨ How to Make the Most of It

1.  **Explore by Domain:** Use the Table of Contents to jump directly to the area you need to review.
2.  **Deep Dive into Tools:** Check the specific cheatsheets and workflows for each key tool (`Volatility`, `Splunk`, `Wireshark`, etc.).
3.  **Practice:** Apply the commands and techniques in labs (BTLO, CyberDefenders, TryHackMe). This repo is your quick reference companion.
4.  **Adapt and Expand:** This is your space! Edit, add your own discoveries, notes, or useful scripts.
5.  **Visualize:** Where you see diagrams or mentions of flows, try to visualize them mentally or draw them out.
---

## 🤝 Contributions and Feedback

> Although this is primarily a personal study repository, if you find any errors, have suggestions for improving the content, or want to propose additions, please **open an Issue** in this repository. All constructive feedback is welcome.
---

## ⚠️ Disclaimer and Confidentiality

> The information contained herein is based on personal experience, the original provided material, and publicly available resources. Cybersecurity is a dynamic field. **Always verify information with the official BTL1 syllabus and tool documentation.** The author is not responsible for the use of the information presented here.
>
> **Important: The Security Blue Team Non-Disclosure Agreement (NDA) is strictly respected.** This repository does not, and will not, contain leaked questions, direct solutions to exam scenarios, or any proprietary information covered by said agreement. The goal is to consolidate applicable general knowledge and techniques, not to compromise the integrity of the certification.
---

## 📄 License

Distributed under the **MIT License**. See the [`LICENSE`](./LICENSE) file for more details.
