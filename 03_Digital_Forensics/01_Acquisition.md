# 💾 Digital Evidence Acquisition

> **Acquisition** is the process of creating a forensic copy (image) of original digital evidence (e.g., hard drive, RAM, USB drive) in a way that preserves the original state as much as possible. It is the pillar upon which all forensic investigation is built.

**Main Objective:** Create a verifiable bit-by-bit copy of the original to be able to analyze it without altering the primary source.

## 🧱 Types of Forensic Images

Different formats and types of images exist, each with its pros and cons:

1.  **Physical Image (Bitstream / Bit-by-Bit):**
    * **Description:** Copies the entire physical device sector by sector, including allocated space, unallocated space (where deleted data may reside), and 'slack' space. It is the most complete image.
    * **Common Formats:**
        * **RAW / DD (`.dd`, `.img`, `.bin`):** Simple format, direct bit-by-bit output. Compatible with many tools, but doesn't include metadata, compression, or internal hashing. Can be split into segments (`.001`, `.002`...).
        * **EnCase (`.E01`, `.Ex01`):** Proprietary format (but widely supported). Includes metadata (case name, examiner, notes), optional compression, and internal hashing (`MD5` and/or `SHA1`) per block for integrity verification. It is a de facto standard in many environments.
    * **Advantages:** Maximum completeness, allows recovery of deleted files and analysis of artifacts in unallocated space.
    * **Disadvantages:** Very large image files (equal to the original disk size if not compressed).

2.  **Logical Image:**
    * **Description:** Copies only the active files and directories within a selected file system or partition. Does not include unallocated space or deleted files (unless they are in the recycle bin, for example).
    * **Common Formats:** Proprietary formats (`.L01`, `.AD1`), or simply file containers (e.g., `.zip`, `.tar` - although these are less forensic as they don't robustly preserve file system metadata).
    * **Advantages:** Faster to acquire, considerably smaller image size.
    * **Disadvantages:** Incomplete, doesn't allow recovery of most deleted data or analysis of artifacts outside the active file system.

**BTL1 Focus:** You will generally work with disk images (physical, likely `.E01` or `.dd`) and memory dumps (`.raw`, `.vmem`, `.dmp`) that will be provided to you as part of the scenario. You will rarely have to perform the acquisition yourself, but understanding the formats and the importance of hashing is crucial.

## 💿 Disk Acquisition

Process to create a forensic image of storage media (HDD, SSD, USB).

* **Key Principle:**
    > Use a **write blocker** (`write-blocker`) (hardware or software) to connect the original disk to the acquisition system. This prevents any accidental writes that would alter the evidence.
* **Common Tools:**
    * **`FTK Imager` (AccessData/Exterro):** Very popular free GUI tool for Windows. Allows creating RAW(dd), E01 images, mounting images, previewing files. Includes `MD5`/`SHA1` hashing during acquisition. *(Although acquisition is done via GUI, conceptually it performs the sector-by-sector copy).*
    * **`dd` / `dcfldd` (Linux CLI):** `dd` is the classic Linux tool for bit-by-bit copying. `dcfldd` is an enhanced version for forensics that adds on-the-fly hashing and better feedback.
        * Basic `dd` syntax: `sudo dd if=/dev/sdX of=/path/to/destination/image.dd bs=4M status=progress` (**BE CAREFUL** with `if=` and `of=`!)
        * Basic `dcfldd` syntax: `sudo dcfldd if=/dev/sdX hash=md5,sha256 md5log=hash_md5.txt sha256log=hash_sha256.txt of=/path/to/destination/image.dd status=on`
    * **`Guymager` (Linux GUI):** User-friendly graphical interface for acquisition on Linux, supports E01 and dd, includes hashing and verification.
* **General Process:**
    1. Connect original disk via write-blocker.
    2. Start acquisition tool.
    3. Select source disk (e.g., `if=/dev/sdX`).
    4. Select destination and image format (e.g., `of=/path/to/image.e01`).
    5. Configure options (compression, segment size, case metadata).
    6. **Activate hash calculation (`MD5` and/or `SHA256`) during acquisition.**
    7. Start the process.
    8. **Verify:** Upon completion, compare the hash calculated by the tool with a hash calculated manually on the resulting image. They **must match**.

## 🧠 Memory (RAM) Acquisition

Captures the volatile content of RAM from a system _while it's running_. Must be done on the live system before shutting it down.

* **Importance:** Contains crucial data: running processes (even hidden malware), active/past network connections, executed commands, loaded registry keys, credentials in memory, etc.
* **Common Tools:**
    * **`FTK Imager` (Windows GUI):** "Capture Memory" option.
    * **`DumpIt` (Magnet Forensics - Windows CLI):** Simple and fast executable.
    * **`Belkasoft Live RAM Capturer` (Windows GUI/CLI):** Free tool.
    * **`LiME` (Linux Memory Extractor - Linux Kernel Module):** De facto standard on Linux. Requires loading a kernel module (compiled for the specific kernel version or using precompiled versions).
* **General Process:**
    1. Run the capture tool on the live system (with administrator/root privileges).
    2. Specify the output path for the dump file (usually `.raw`, `.mem`, `.vmem`, `.dmp`).
    3. Wait for the copy to finish.
    4. Document the time, tool used, and any relevant messages. Calculate hash of the resulting file for integrity.

## #️⃣ Hashing and Evidence Integrity

> Hashing is **fundamental** in digital forensics to verify the integrity of the evidence.

* **Purpose:** Ensure the copy (image) is identical to the original at the time of acquisition and that it has not been subsequently modified.
* **Algorithms:** Cryptographic hash functions like **`MD5`**, **`SHA-1`** (considered weak for cryptography, but still used in forensics for compatibility), and **`SHA-256`** (preferred) are used.
* **Verification Process:**
    1.  **Acquisition:** A hash of the original disk/media is calculated (ideally) and/or a hash of the image is calculated as it's created (e.g., in E01 format or with `dcfldd`).
    2.  **Post-Acquisition:** A hash of the resulting image file is calculated. This hash **must match** the hash calculated during acquisition.
    3.  **Before Analysis:** Each time the image is going to be analyzed, it is recommended to **recalculate its hash** and compare it with the original acquisition hash to ensure there has been no corruption or tampering.
* **Tools:** `FTK Imager` ('Verify Image' function), `md5sum`/`sha256sum` (Linux), `Get-FileHash` (PowerShell).

---