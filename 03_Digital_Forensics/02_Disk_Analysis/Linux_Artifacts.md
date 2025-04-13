# 🐧 Key Forensic Artifacts in Linux

> Linux, due to its open nature and standard file structure, offers many places to look for digital evidence. Knowing the main log directories and files is essential.

## 📜 System Logs (`/var/log`)

> This directory is the central repository for most system and application logs in almost all Linux distributions.

* **Forensic Value:** Records system events, authentication, service activity (web, mail, etc.), kernel errors.
* **Key Files (May vary slightly between distributions):**
    * `syslog` or `messages`: General system log, varied events.
    * `auth.log` (Debian/Ubuntu) or `secure` (CentOS/RHEL): **Very important!** Logs login attempts (successful and failed), `sudo` usage, SSH activity, user/group creation.
    * `kern.log`: Kernel messages (hardware, drivers).
    * `dmesg`: Kernel ring buffer messages, useful for boot events.
    * `boot.log`: Specific messages from the boot process.
    * `faillog`: Records failed login attempts (binary format).
    * `lastlog`: Records the last login time for each user (binary format).
    * Application Logs: Subdirectories like `/var/log/apache2/`, `/var/log/httpd/`, `/var/log/nginx/`, `/var/log/mysql/`, etc., contain logs specific to those services.
    * `wtmp`: Records user login/logout history (binary format, read by `last`).
    * `btmp`: Records failed login attempts (binary format, read by `lastb`).
    * `apt/` (Debian/Ubuntu) or `yum.log` (CentOS/RHEL): Package installation/update history.
* **Analysis Tools:** `cat`, `less`, `more`, `tail`, `head`, `grep`, `awk`, `sed` for text viewing and filtering. `journalctl` for systems using `systemd`. `last`, `lastb`, `who`, `utmpdump` for binary login logs.

## ⌨️ Command History (`Bash History`)

> Records commands executed by users in the `bash` terminal (the most common).

* **Forensic Value:** Allows reconstructing actions performed by a user (or attacker) on the command line. **Extremely valuable!**
* **Typical Location:** Hidden file `.bash_history` in each user's home directory (`/home/<user>/.bash_history` or `/root/.bash_history`).
* **Considerations:**
    * The user can delete or manipulate this file.
    * By default, it may not include timestamps (configured with `HISTTIMEFORMAT`).
    * Other shells (`zsh`, `fish`) use different files (`.zsh_history`, `.local/share/fish/fish_history`).
* **Tools:** `cat`, `less`, `grep`, `strings` (if the file is corrupt).

## 👤 User Accounts and Groups

> Information about who can access the system and with what permissions.

* **Forensic Value:** Identify legitimate users, users created by attackers, memberships in privileged groups (e.g., `sudo`, `wheel`, `adm`, `docker`).
* **Key Files:**
    * `/etc/passwd`: Basic user information (name, UID, GID, home, shell). Readable by all.
    * `/etc/shadow`: Contains password hashes and expiration policies. **Only readable by root.**
    * `/etc/group`: Defines groups and which users belong to them.
    * `/etc/sudoers` or files in `/etc/sudoers.d/`: Defines who can use `sudo` and with what privileges.
* **Tools:** `cat`, `less`, `grep`, `getent passwd <user>`, `getent group <group>`, `id <user>`.

## 🌐 Network Information

> Configuration and status of network interfaces and connections.

* **Forensic Value:** Understand the system's network configuration, identify active connections (potential C2), listening ports.
* **Key Commands/Locations:**
    * Interface Configuration: `ip addr` command (or legacy `ifconfig`). Static configuration files vary (e.g., `/etc/network/interfaces`, `/etc/sysconfig/network-scripts/`).
    * Active Connections: `ss -tulnp` command (or legacy `netstat -tulnp`). Shows TCP/UDP, listening/connected, IPs/ports, and the associated process (with `-p`).
    * DNS Configuration: `/etc/resolv.conf`.
    * ARP Cache: `ip neigh` (or legacy `arp -a`).

## ⏰ Scheduled Tasks (`Cron`)

> Allows running commands or scripts automatically at defined times. Used legitimately, but also as a common persistence technique for malware.

* **Forensic Value:** Identify suspicious scripts or commands that run periodically.
* **Locations:**
    * System-wide: `/etc/crontab`, files within `/etc/cron.d/`, `/etc/cron.hourly/`, `/etc/cron.daily/`, `/etc/cron.weekly/`, `/etc/cron.monthly/`.
    * User-specific: Managed with `crontab -e`. Files are usually in `/var/spool/cron/crontabs/<user>` (requires root to view others').
* **Tools:** `cat`, `ls` to view directories/files. `crontab -l -u <user>` to list a specific user's tasks.

## ⚙️ Process Information (Mainly Live/Memory)

> Although best obtained from a live system or memory dump, some traces might remain in logs.

* **Forensic Value:** Identify malicious running processes.
* **Commands (Live):** `ps aux`, `ps -elf`, `top`, `htop`.
* **`/proc` File System:** Virtual. Contains a directory for each running process PID (`/proc/<PID>/`) with detailed info (`cmdline` - command, `environ` - environment variables, `maps` - mapped memory, `fd` - open files).

## 🏠 User Home Directories (`/home/<user>` or `/root`)

> Contain the user's personal files and many application configurations.

* **Forensic Value:** Files downloaded by the user (potential malware), application configurations (browsers, email clients), SSH keys (`.ssh/`), personal scripts, command history.
* **Tools:** `ls -la`, `find`, `grep`.

## 🗑️ Temporary Files (`/tmp`, `/var/tmp`)

> Directories designed to store temporary files. Often have lax permissions and are used by malware.

* **Forensic Value:** Downloaded malware, intermediate scripts, extracted files.
* **Tools:** `ls -la`, `find`.

---