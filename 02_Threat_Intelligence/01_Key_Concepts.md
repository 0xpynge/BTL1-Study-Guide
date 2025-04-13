# 💡 Threat Intelligence (CTI): Key Concepts

**Cyber Threat Intelligence (CTI)** is evidence-based knowledge (context, mechanisms, indicators, implications, and actionable advice) about an existing or emerging threat or hazard to assets. The main goal of CTI is to **inform decision-making** related to cybersecurity, enabling a more proactive and effective defense.

In the context of BTL1, applying CTI concepts helps you understand _why_ and _how_ attacks occur, and not just identify that _something_ bad has happened.

## 🤔 What CTI is NOT (Just)

> It's important to understand that CTI goes beyond a simple list of Indicators of Compromise (`IoCs`). A list of malicious IPs or hashes without context is just *information* or *data*. _Intelligence_ requires **analysis, context, and relevance** to be actionable.

## 📊 Types of CTI (Levels)

Although several levels exist, for BTL1 we are primarily interested in:

* **Tactical:** Focused on specific and observable `IoCs` (IPs, domains, hashes) for immediate detection and blocking. It's the most common in daily alert analysis.
* **Operational:** Centered on attacker **Tactics, Techniques, and Procedures (`TTPs`)**. Seeks to understand _how_ adversaries operate (their tools, attack methods, infrastructure).

*(Strategic CTI also exists, more focused on long-term risks and the threat landscape, generally for management).*

## 📍 Indicators of Compromise (`IoCs`)

> `IoCs` are the "digital fingerprints" or pieces of forensic evidence that indicate malicious activity has occurred on a system or network. They are the most basic level of tactical intelligence.

* **Common Examples:**
    * IP Addresses (from C&C, phishing servers, scanning)
    * Domain Names (malicious, DGA)
    * File Hashes (MD5, SHA1, SHA256 of malware)
    * Specific URLs (phishing, malware download)
    * Email Addresses (attacker's)
    * Specific filenames or paths
    * Registry keys or Mutexes created by malware

* **Value:** Useful for quick detection and blocking, but relatively easy for attackers to change.

## 🧅 Tactics, Techniques, and Procedures (`TTPs`)

> `TTPs` represent the adversary's *behavior*. They are a more abstract and valuable level of operational intelligence:

* **Tactic:** The objective or purpose of an attacker's action (e.g., Initial Access, Execution, Persistence, Exfiltration).
* **Technique:** The specific method used to achieve a tactic (e.g., Spearphishing Attachment [`T1566.001`] for Initial Access, Command and Scripting Interpreter [`T1059`] for Execution).
* **Procedure:** The specific implementation of a technique by a particular actor or group (e.g., using a specific obfuscated PowerShell script to download and execute malware).

* **Value:** Understanding `TTPs` allows predicting future moves, designing more robust detections (based on behavior, not just signatures), and better understanding the adversary. They are harder for attackers to change than simple `IoCs`. The **MITRE ATT&CK®** framework is the de facto standard for cataloging `TTPs`.

## 👤 Threat Actors

> Threat Actors are the individuals, groups, or organizations responsible for cyberattacks. Understanding (even at a basic level) who might be behind an attack helps understand:

* **Motivations:** Financial? Espionage? Ideological?
* **Capabilities:** Are they sophisticated (APT) or do they use common tools?
* **Typical `TTPs`:** Certain groups tend to reuse tools or methods.

*In BTL1, deep attribution is not expected, but recognizing patterns associated with types of actors can be useful.*

## ♻️ Intelligence Lifecycle (Simplified)

Even in a BTL1 investigation, you informally follow a cycle:

1.  **Direction:** What do I need to know? (Defined by the exam scenario).
2.  **Collection:** Gathering data (logs, headers, tool results).
3.  **Processing:** Organizing and formatting data (e.g., extracting `IoCs`).
4.  **Analysis:** Interpreting data, looking for patterns, correlating, using CTI tools (VT, OTX...). _What does this IP mean? Is this hash known? What `TTPs` do I observe?_
5.  **Dissemination:** Presenting the findings (in your final report).
6.  **Feedback:** (In a real environment, this improves the future cycle).

## 📈 The Pyramid of Pain

> Created by David J Bianco, this pyramid illustrates which types of indicators are more "painful" (difficult/costly) for an attacker if defenders block or detect them. Blocking `TTPs` causes more "pain" than blocking simple hashes or IPs.

* **(Base - Easy to change):** Hashes -> IPs -> Domain Names
* **(Middle):** Network Artifacts (e.g., User-Agents) -> Host Artifacts (e.g., filenames)
* **(Top - Hard to change):** Tools -> **`TTPs`**

*Understanding this helps prioritize which type of intelligence is more valuable long-term.*

---