# 📚 Resources and Practice for SIEM and Log Analysis

> Log analysis in a SIEM is a skill perfected through practice. Here are resources to train with `Splunk`, explore other platforms, and find datasets to analyze:

## 📚 Splunk Specific Resources

* **[Official Splunk Documentation](https://docs.splunk.com/Documentation/Splunk)**:
    * Essential. Consult the **Search Manual** and the **SPL Reference** to understand all commands and functions.

* **[Free Splunk Training](https://www.splunk.com/en_us/training/free-courses/overview.html)**:
    * `Splunk` offers free online courses. **"Splunk Fundamentals 1"** is an excellent starting point to get familiar with the interface and basic `SPL`.

* **[Splunk Boss of the SOC (BOTS)](https://www.splunk.com/en_us/campaigns/boss-of-the-soc.html)**:
    * It's a CTF (Capture The Flag) created by `Splunk`. They release the datasets (`datasets`) from previous versions (v1, v2, v3...). You can download these datasets and ingest them into a free `Splunk` instance to practice solving the CTF questions. Search "Splunk BOTS dataset download" to find available versions.

## 🧅 SIEM/Log Analysis Lab Platforms

> Setting up your own environment is a great way to learn.

* **[Security Onion](https://securityonionsolutions.com/)**:
    * **Free and open-source** Linux distribution designed for threat hunting, security monitoring, and log management. Includes the `Elastic Stack` (Elasticsearch, Logstash, Kibana) or `Wazuh` as a SIEM, plus NIDS (`Suricata`, `Zeek`) and other tools. Ideal for a home lab.

* **[Wazuh](https://wazuh.com/)**:
    * **Open-source** unified security platform (XDR and SIEM). Offers host-based intrusion detection, file integrity monitoring, log analysis, and more. Can be integrated with `Elastic Stack`.

* **[Elastic Stack (ELK)](https://www.elastic.co/training/free)**:
    * If you want to explore the alternative to `Splunk`, Elastic offers free training on its stack (`Elasticsearch`, `Kibana`) and its query language `KQL` (Kibana Query Language).

## 💾 Public Datasets for Practice

> Analyzing pre-generated data from simulated attacks is very useful.

* **[Mordor Datasets (SpecterOps)](https://github.com/hunters-forge/mordor)**:
    * Collection of pre-recorded security datasets based on the execution of specific adversary techniques, **mapped to MITRE ATT&CK**. Available in `JSON`, `CSV`, `EVTX`, etc. formats, ready to import into `Splunk`/`Elastic`.

* **[EVTX-ATTACK-SAMPLES (SBousseaden)](https://github.com/sbousseaden/EVTX-ATTACK-SAMPLES)**:
    * Repository with Windows event log samples (`.evtx`) related to specific ATT&CK techniques. Useful for practicing the detection of specific artifacts.

* **General Search:** Search online for "sample security logs", "sysmon logs dataset", "apache access log sample", etc., to find specific data from different sources.

## 💻 CTF Platforms (Reiteration)

* **[CyberDefenders](https://cyberdefenders.org/)**, **[Blue Team Labs Online (BTLO)](https://blueteamlabs.online/)**, **[TryHackMe](https://tryhackme.com/)**: Their challenges frequently include log analysis in `Splunk`, `Elastic`, or other formats as a central part of the investigation.

## 📰 Blogs and Continuous Learning

* **[Splunk Blogs (Security)](https://www.splunk.com/en_us/blog/platform.html)**: Articles on using `Splunk` for security use cases.
* **Detection/Threat Hunting Blogs:** Follow blogs focusing on creating SIEM detections and hunting for threats (e.g., SpecterOps, Red Canary, Nasreddine Bencherchali, Olaf Hartong).

---

> _The best way to master SIEM analysis is to spend time searching through real or simulated data, testing different queries, and learning to interpret the results._