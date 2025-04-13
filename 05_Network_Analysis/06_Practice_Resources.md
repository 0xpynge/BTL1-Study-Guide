# 📚 Resources and Practice for Network Analysis (PCAP)

> Network traffic analysis, especially of PCAP files, is a skill that improves greatly with practice on real data or from challenges. Here are key resources:

## 💾 Sources of PCAP Files for Practice

* **[Malware-Traffic-Analysis.net](https://www.malware-traffic-analysis.net/)**:
    * **Essential Resource!** Brad Duncan's blog contains detailed analyses of real malware campaigns, and most importantly, **provides the associated PCAP files** so you can analyze them yourself. Often includes exercises or questions to guide your analysis.

* **[Wireshark Sample Captures Wiki](https://wiki.wireshark.org/SampleCaptures)**:
    * The official Wireshark wiki has a page that collects a wide variety of sample captures for different protocols and scenarios (normal traffic, problems, some attacks).

* **[NETRESEC - Publicly Available PCAP Files](https://www.netresec.com/?page=PcapFiles)**:
    * Page maintained by Erik Hjelmvik (creator of `NetworkMiner`) that lists publicly available PCAP files, often sourced from CTFs, training exercises, or research.

* **[Digital Corpora](https://digitalcorpora.org/)**:
    * (Mentioned in Forensics) Also hosts network traffic datasets, sometimes related to their disk images.

* **[PacketTotal](https://packettotal.com/)**:
    * It's an online PCAP analyzer (similar to VirusTotal but for PCAPs), but also functions as a repository where users upload captures. You can explore PCAPs uploaded by others (**with caution**, verify the source and content!).

## 💻 CTF Platforms and Labs

> Many challenge platforms include PCAP analysis as part of their scenarios:

* **[CyberDefenders](https://cyberdefenders.org/)**, **[Blue Team Labs Online (BTLO)](https://blueteamlabs.online/)**, **[TryHackMe](https://tryhackme.com/)**, **[Hack The Box (Sherlocks)](https://app.hackthebox.com/sherlocks)**: Look for challenges tagged as "Network Forensics", "PCAP Analysis", or incident response scenarios that will surely require you to examine network traffic.

## 🛠️ Tool Documentation

* **[Wireshark User's Guide & Wiki](https://www.wireshark.org/docs/)**
* **`tshark` `man` pages** (run `man tshark` on Linux) or Wireshark's online documentation.

## 📰 Blogs, Channels, and Continuous Learning

* **[Malware-Traffic-Analysis.net Blog](https://www.malware-traffic-analysis.net/blog-entries.html)**: (Again) The write-ups are as valuable as the PCAPs.
* **[Wireshark Blog](https://blog.wireshark.org/)** and **[SharkFest Presentations](https://sharkfestus.wireshark.org/retrospective)** (Search on YouTube): Presentations from the annual Wireshark conference, often with deep dives.
* **[SANS ISC Handlers Diary](https://isc.sans.edu/)**: Sometimes include quick analyses of PCAPs related to current threats.
* **YouTube Channels:** Search for channels like **13Cubed**, **Chris Greer**, **Packet Bomb** that often have tutorials on `Wireshark` and packet analysis.

---

> _Analyzing PCAPs from real scenarios or CTFs is the best way to become familiar with how normal vs. malicious traffic looks and to gain speed using `Wireshark`/`tshark`._