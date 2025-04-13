# 🔥 Containment and Eradication: Key Concepts

> Once an incident has been identified and analyzed (at least initially), the next critical phases in the incident response lifecycle (`PICERL`) are **Containment** and **Eradication**.

## 🧱 Containment (`Containment`)

* **Main Objective:** Stop the incident from spreading and limit the damage as quickly as possible. It's about "stopping the bleeding".
* **Importance:** Prevents more systems from being compromised, more data from being exfiltrated, or the attacker from causing more impact. It's a phase where time is often critical.
* **Common Strategies (Examples):**
    * **Isolation of Affected Systems:**
        * Disconnecting the network cable.
        * Disabling the virtual/physical network adapter.
        * Moving the system to a quarantine `VLAN` with restrictive firewall rules.
        * Applying host-based or network firewall rules to block specific communications.
    * **Blocking Indicators:**
        * Blocking known malicious IP addresses or domains (`C2`, phishing) at the perimeter firewall, proxy, or internal DNS server (sinkholing).
    * **Disabling Accounts:**
        * Immediately disabling user or service accounts known or suspected to be compromised.
    * **Stopping Processes:**
        * Terminating identified malicious processes on affected systems (though this is volatile, and malware may have mechanisms to restart).
* **Considerations:** Requires rapid decision-making and weighing the impact of containment on business operations. Sometimes short-term containment (quick and dirty) is applied, followed by a more planned long-term strategy.

---

## 🧹 Eradication (`Eradication`)

* **Main Objective:** Completely remove the **root cause** of the incident and all malicious artifacts from the affected environment to ensure the attacker cannot easily re-enter.
* **Importance:** Ensures a complete cleanup before restoring services. If not eradicated correctly, the incident can re-emerge.
* **Common Strategies (Examples):**
    * **Malware Removal:** Deleting executable files, scripts, malicious DLLs identified during analysis.
    * **Persistence Removal:** Removing registry keys, scheduled tasks, services, or user accounts created by the attacker.
    * **Vulnerability Patching:** Applying necessary security patches to close the vulnerability that was originally exploited.
    * **Secure Reconfiguration:** Correcting insecure configurations that allowed the attack (e.g., weak permissions, unnecessary enabled services).
    * **System Reinstallation / Reimaging (`Reimaging`):** **Often the safest and recommended option for compromised systems.** Involves formatting the disk and reinstalling the OS from a known 'golden' (clean and secure) image. Data is then restored from clean backups. This ensures the removal of any hidden rootkits or backdoors.
    * **Credential Reset:** Forcing password changes for all potentially exposed or compromised accounts.
* **Validation:** Before moving to recovery, it's crucial to validate that eradication was successful (e.g., re-scanning the system, monitoring activity).

---

## ✅ Relationship with Analysis (Identification Phase)

> The effectiveness of Containment and Eradication directly depends on the quality of the analysis performed in the Identification phase. A good analysis should determine:

* Which systems are compromised.
* Which accounts are compromised.
* What malware/tools the attacker used and where they reside.
* Which vulnerabilities were exploited (if applicable).
* What persistence mechanisms were established.

> This information guides the specific actions to be taken during containment and eradication.

---

## 🎯 Note on BTL1

> Remember that the BTL1 exam focuses primarily on **Identification and Analysis**. Although you should conceptually understand what Containment and Eradication are, **you are not expected to perform these actions** directly in the simulated exam environment. Your role is to provide the detailed analysis that would allow an IR team to make these decisions in a real scenario.

---

> _Containment seeks to stop immediate damage, while Eradication seeks to eliminate the threat at its root for a safe and lasting recovery._