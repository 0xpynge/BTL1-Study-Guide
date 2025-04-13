# 🌐 Network Analysis

> This section covers **Network Traffic Analysis**, an essential skill for understanding how systems communicate and for detecting malicious activities occurring over the network. We will focus on analyzing packet captures (`PCAPs`) using industry-standard tools.
>
> We will learn to filter traffic, reconstruct conversations, identify key protocols, and recognize patterns associated with different types of cyberattacks.

## 🗺️ Section Contents

* **[Key Concepts](./01_Key_Concepts.md):** Introduces what `PCAPs` are, a practical review of the OSI/TCP/IP model for analysis, and what types of evidence to look for in network traffic.
* **[Tools: Wireshark and Tshark](./02_Wireshark_Tshark.md):** Details the use of `Wireshark` (GUI) and `tshark` (CLI) for capturing and analyzing traffic, including key functionalities like following streams or exporting objects.
* **[Filters Cheatsheet](./03_Filters_Cheatsheet.md):** A quick and organized reference of the most useful display filters in `Wireshark`/`tshark` for isolating specific traffic.
* **[Specific Protocol Analysis](./04_Specific_Protocol_Analysis.md):** Delves into what to look for within critical protocols like `HTTP`, `DNS`, and `SMB`/`SMB2`.
* **[Recognizing Malicious Patterns](./05_Malicious_Patterns.md):** Describes common traffic patterns associated with activities like Command and Control (`C2`), network scanning, lateral movement, and data exfiltration.
* **[Resources and Practice](./06_Practice_Resources.md):** Links to sample `PCAP` repositories (like Malware-Traffic-Analysis.net), CTF platforms, and other resources for practicing network analysis.

---

> _Explore the files to master the art of interpreting network conversations and discover the activity hidden within the packets._