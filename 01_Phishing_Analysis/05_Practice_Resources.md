# Practice Resources and Platforms for Phishing Analysis

Theory and workflows are important, but **constant practice** is key to becoming truly proficient in phishing analysis. Here are some resources where you can find examples, practice, and learn more:

## 💻 Practice Platforms (Hands-On Labs)

These platforms offer simulated scenarios where you can analyze phishing emails in a safe environment:

* **[LetsDefend](https://letsdefend.io/)**:
    * Offers a specific "Phishing Analysis" module where you receive simulated emails and must analyze them using virtual tools. Very practical and focused. (Has free and paid plans).

* **[Blue Team Labs Online (BTLO)](https://blueteamlabs.online/)**:
    * Sister platform to the BTL1 certification. Frequently includes free and paid Challenges that involve phishing analysis as part of broader investigations (DFIR, SIEM).

* **[CyberDefenders](https://cyberdefenders.org/)**:
    * Platform with CTF-style challenges focused on Blue Team and DFIR. Many of its challenges start with an initial access vector like phishing, requiring analysis of emails, URLs, or attachments.

* **[TryHackMe](https://tryhackme.com/)**:
    * Although broader, it has rooms and paths focused on SOC and security analysis that may include modules or tasks related to phishing analysis.

## 🎣 Sources of Samples and Data (URLs/Emails)

These sites list URLs reported as phishing. **⚠️ EXTREME CAUTION!** Do not visit these URLs directly on your main machine. Use them for investigation in reputation services (VT, URLScan) or in highly controlled environments.

* **[PhishTank](https://phishtank.org/)**:
    * A collaborative database where users report and verify phishing sites. You can explore reported URLs.

* **[OpenPhish](https://openphish.com/)**:
    * Provides feeds (lists) of active phishing URLs. Useful for intelligence, but handle the data with extreme care.

* **GitHub/Internet Search:**
    * Sometimes you can find repositories or blogs with collections of sample `.eml` emails for offline analysis. Search for terms like `"phishing email samples"`, `"eml phishing examples"`. **Always verify the source and handle these files with the same precautions as a real attachment.**

## 📰 Blogs and Continuous Learning

Stay up-to-date with the latest phishing techniques and campaigns by following blogs from security companies and analysts:

* **[Cofense Blog](https://cofense.com/blog/)**: Specialized in defense against phishing.
* **[Proofpoint Threat Insight](https://www.proofpoint.com/us/blog/threat-insight)**: Research on threats, including phishing.
* **[KnowBe4 Blog](https://blog.knowbe4.com/)**: Focused on awareness, but with frequent technical analyses.
* **[SANS Internet Storm Center (ISC)](https://isc.sans.edu/)**: Daily reports on threats, often including analyses of phishing campaigns.
* **Specific Searches:** Search Google or Twitter for analyses of recent campaigns (e.g., `"Qakbot phishing analysis"`, `"IcedID email analysis"`).

## 🛠️ Tool Documentation

Don't forget to consult the official documentation of the tools mentioned in `02_Tools.md`. They usually contain detailed guides, examples, and explanations of all their functionalities:

* Documentation for VirusTotal, URLScan.io, Any.Run, Hybrid Analysis, MxToolbox, etc.

---