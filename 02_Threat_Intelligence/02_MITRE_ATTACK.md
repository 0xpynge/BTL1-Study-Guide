# 🗺️ The MITRE ATT&CK® Framework

The **MITRE ATT&CK®** framework has become the de facto standard for describing and categorizing the behavior of cyber adversaries. It is a **globally accessible knowledge base**, curated by [The MITRE Corporation](https://www.mitre.org/), based on **real-world observations** of cyberattacks.

ATT&CK stands for **A**dversarial **T**actics, **T**echniques, and **C**ommon **K**nowledge.

## 🏗️ Framework Structure: The Matrices

ATT&CK organizes adversary behavior into matrices. The most relevant for BTL1 (and most enterprise environments) is the **Enterprise Matrix**, which covers platforms like Windows, macOS, Linux, Networks, Cloud, etc.

The matrix is structured like this:

* **Tactics (Columns):** Represent the **tactical objective** or the reason behind an adversary's action. They are the high-level categories of an attack lifecycle. Key tactics in the Enterprise Matrix include:
    * `Reconnaissance`
    * `Resource Development`
    * `Initial Access`
    * `Execution`
    * `Persistence`
    * `Privilege Escalation`
    * `Defense Evasion`
    * `Credential Access`
    * `Discovery`
    * `Lateral Movement`
    * `Collection`
    * `Command and Control` (C2)
    * `Exfiltration`
    * `Impact`

* **Techniques (Cells):** Represent _how_ an adversary achieves a tactical objective. Each technique has a unique ID (e.g., `T1566 Phishing`). They describe a specific action.

* **Sub-techniques (Technique Details):** Represent a more specific way of implementing a technique. They have IDs with a decimal point (e.g., `T1566.001 Spearphishing Attachment`, `T1566.002 Spearphishing Link`). Not all techniques have sub-techniques.

* **Procedures (Examples):** Are the specific implementations of techniques and sub-techniques observed in the real world by threat actor groups (APTs, crime groups) or specific malware. ATT&CK documents these procedures as examples.

## ✅ Why is ATT&CK Useful for Blue Team / BTL1?

> ATT&CK provides immense value for defenders:

1.  **Common Language:** Offers a standard taxonomy to describe attacker actions, facilitating communication and analysis.
2.  **Incident Contextualization:** Allows mapping the evidence found during an investigation (logs, artifacts) to known `TTPs`, helping to understand what the attacker is doing and what they might do next.
3.  **Investigation Guide:** When identifying a `TTP`, you can consult ATT&CK to know which artifacts to look for or what other techniques attackers using that `TTP` often employ.
4.  **Detection Improvement:** Helps design detection rules (e.g., in a SIEM) based on behaviors (`TTPs`) instead of just fragile `IoCs`.
5.  **Coverage Assessment:** Allows analyzing which `TTPs` your current defenses cover (or don't).
6.  **CTI Enrichment:** Links `IoCs` with behaviors (`TTPs`) for more robust intelligence.

## 🚀 How to Use ATT&CK in Practice?

* **Official Website:** The main resource is [https://attack.mitre.org/](https://attack.mitre.org/). You can browse the matrices, search for techniques, read detailed descriptions, see procedure examples and references.
* **`ATT&CK Navigator`:** A web tool ([https://mitre-attack.github.io/attack-navigator/](https://mitre-attack.github.io/attack-navigator/)) to visualize the matrices, create "layers" to annotate, color techniques (e.g., mark those observed in an incident, those you have detections for), compare layers, etc. Very useful for analysis and presentations.
* **Mapping Evidence to `TTPs` (Mental Process):**
    1.  **Observe:** You find a suspicious artifact or activity (e.g., a new Windows service created that executes a suspicious script).
    2.  **Search/Consult ATT&CK:** Search the ATT&CK site for relevant keywords ("Windows service", "persistence", "service creation").
    3.  **Identify the Technique:** You find `T1543.003 Create or Modify System Process: Windows Service`.
    4.  **Understand the Context:** The technique's page tells you that it belongs to the `Persistence` and `Privilege Escalation` tactics. It gives details on how it's used, examples, and potential mitigations/detections.
    5.  **Document:** Note the identified `TTP` in your notes/report to give context to your finding.

## 🎯 Relevance for BTL1

> In the BTL1 exam, although it's not always explicitly required to map every finding to an ATT&CK `TTP`, doing so (when relevant and you are confident in the identification) demonstrates a deeper understanding of the attacker's behavior. It can add **significant value** to your final report by explaining _why_ a specific artifact is important in the context of the broader attack.

---