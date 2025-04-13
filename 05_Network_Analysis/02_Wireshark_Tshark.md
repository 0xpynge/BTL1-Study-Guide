# 🛠️ Key Tools: Wireshark and Tshark

> **[Wireshark](https://www.wireshark.org/)** is the world's most widely used and recognized network protocol analyzer. It's an **open-source**, cross-platform tool (Windows, Linux, macOS) with a powerful graphical user interface (GUI) that allows capturing live traffic or analyzing existing PCAP files in great detail.
> 
> **`Tshark`** is the **command-line interface (CLI) version** of Wireshark. It shares the same protocol dissection engine and is ideal for scripting, automated analysis, remote capture, or quickly extracting information without loading the GUI.

## 🦈 Wireshark (Graphical Interface - GUI)

The Wireshark interface might seem overwhelming at first, but its main components are:

1.  **Display Filter Bar (`Display Filter`):** **Fundamental.** This is where you enter filters to isolate the traffic you are interested in seeing.
2.  **Packet List Pane (`Packet List`):** Shows a summary of each captured/loaded packet (number, time, source/destination IPs, protocol, brief info). Rows are colored according to rules for easier identification.
3.  **Packet Details Pane (`Packet Details`):** Hierarchical, expandable view showing all decoded layers and fields of the selected packet. Here you examine Ethernet, IP, TCP/UDP, and application headers (`HTTP`, `DNS`, etc.).
4.  **Packet Bytes Pane (`Packet Bytes`):** Shows the raw content of the selected packet in hexadecimal and ASCII format.

### Key Features for Forensics/Security Analysis:

* **Open PCAP Files:** `File > Open`.
* **Apply Display Filters:** Enter expressions in the filter bar to show only specific packets (e.g., `ip.addr == 192.168.1.1`, `http.request.method == "POST"`, `dns.qry.name contains "evil"`). Syntax is detailed in `03_Filters_Cheatsheet.md`.
* **Follow Stream:** **Essential!** Right-click on a packet from a `TCP`, `UDP`, `TLS`, `HTTP`, etc., conversation and select `Follow > [Protocol] Stream`. Wireshark will reconstruct and display the data exchanged between the two endpoints in a separate window, making it easier to read application payloads (e.g., see a complete `HTTP` request and response).
* **Statistics (`Statistics` Menu):** Very useful for getting summaries:
    * `Conversations`: Lists all conversations (Ethernet, IP, TCP, UDP) with IPs, ports, duration, bytes.
    * `Endpoints`: Lists all endpoints (MACs, IPs, TCP/UDP ports) seen in the capture.
    * `Protocol Hierarchy`: Shows the percentage of traffic corresponding to each protocol.
    * `HTTP`: Specific `HTTP` statistics (e.g., `Requests`, `Packet Counter`).
* **Export Objects (`File > Export Objects`):** Allows extracting files transferred via certain protocols like `HTTP`, `SMB`, `DICOM`, `FTP`. Very useful for recovering downloaded malware or exfiltrated data.
* **Find Packet (`Edit > Find Packet`):** Allows searching for packets containing a specific string (in string, hex, regex format) or matching a display filter.

## 🦈 Tshark (Command Line Interface - CLI)

> Offers the same analysis power as Wireshark but from the terminal.

### Common Uses:

* **Traffic Capture:**
    ```bash
    # Capture on interface eth0 and save to out.pcap (requires permissions)
    sudo tshark -i eth0 -w out.pcap

    # Capture only traffic for host 1.1.1.1 (BPF capture filter)
    sudo tshark -i eth0 -f "host 1.1.1.1" -w out.pcap
    ```
    * `-i <interface>`: Specifies the network interface.
    * `-w <file>`: Writes the raw capture to a PCAP file.
    * `-f "<capture_filter>"`: Applies a capture filter (BPF syntax).

* **Reading and Filtering PCAPs:**
    ```bash
    # Read file.pcap and show only HTTP requests (display filter)
    tshark -r file.pcap -Y "http.request"

    # Read file.pcap and stop after 50 packets matching the filter
    tshark -r file.pcap -Y "ip.addr == 10.0.0.5" -c 50 
    ```
    * `-r <file>`: Reads from a PCAP file.
    * `-Y "<display_filter>"`: Applies a display filter (Wireshark syntax).
    * `-c <N>`: Stops reading after N packets.

* **Extracting Specific Fields:** **Very powerful for scripting!**
    ```bash
    # Extract source IP, destination IP, and URI from HTTP requests
    tshark -r file.pcap -Y "http.request" -T fields -e ip.src -e ip.dst -e http.request.uri

    # Extract time, source/dest IP, and DNS query name
    tshark -r file.pcap -Y "dns.qry.name" -T fields -e frame.time -e ip.src -e ip.dst -e dns.qry.name
    ```
    * `-T fields`: Specifies the output format as fields.
    * `-e <field>`: Specifies the field to extract (you can use `tshark -G fields` to see all available fields).

* **Statistics:**
    ```bash
    # Show TCP conversation statistics
    tshark -r file.pcap -q -z conv,tcp

    # Show protocol hierarchy
    tshark -r file.pcap -q -z io,phs
    ```
    * `-q`: "Quiet" mode (suppresses packet output, useful with `-z`).
    * `-z <statistic>`: Calculates various statistics (see `tshark -z help`).

## ⚠️ Capture Filters vs. Display Filters

> It's important to distinguish them:

* **Capture Filters (`Capture Filters`):**
    * Used by `tcpdump`, `tshark -f`.
    * **BPF (Berkeley Packet Filter)** syntax (e.g., `host 1.1.1.1`, `port 80`, `tcp and dst host 2.2.2.2`).
    * Applied **before** saving packets. Only matching packets are saved to the PCAP. Less flexible but more efficient for reducing capture size live.
* **Display Filters (`Display Filters`):**
    * Used by **Wireshark (filter bar)** and `tshark -Y`.
    * **Wireshark's own syntax**, much richer and protocol-aware (e.g., `http.request.method == "POST"`, `dns.flags.response == 0`).
    * Applied **after** capture, on the existing PCAP file. They simply **hide or show** packets, not remove them from the file. These are the ones you will mainly use for analysis in BTL1.

## 🎯 BTL1 Focus

> For BTL1, you are expected to master the use of **Wireshark (GUI)** to:
* Open and navigate PCAPs.
* Effectively apply **display filters** to find relevant traffic.
* **Follow streams** (`TCP`/`UDP`/`HTTP`) to reconstruct conversations.
* **Export objects** (files) transferred via `HTTP`/`SMB` if necessary.
* Use the **packet details** pane to examine specific headers.

> Knowing `tshark` is an advantage for quickly processing large files or extracting specific information, but mastering the Wireshark GUI is usually sufficient.

---

> _`Wireshark` and `tshark` are indispensable tools for anyone working with networks. Spend time familiarizing yourself with the Wireshark interface and display filter syntax._