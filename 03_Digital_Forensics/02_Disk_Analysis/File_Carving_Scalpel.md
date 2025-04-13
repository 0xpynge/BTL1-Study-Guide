# 🔪 File Carving with Scalpel

## What is File Carving?

> **File Carving** is a technique for **recovering files** by directly searching for their data, without using file system information (which might be damaged or deleted).

* **When is it used?:** Primarily to search for deleted files in the 'empty' (unallocated) space of a disk, or on disks with corrupt file systems.
* **How does it work?:** It searches for known patterns that mark the beginning (header) and sometimes the end (footer) of common file types (like `JPG`, `PDF`, `DOCX`).
* **Limitations:** Does not recover original names or dates. Recovered files may be incomplete or corrupt.

## 🛠️ Scalpel: The Tool

**`Scalpel`** is a popular **open-source** command-line tool (Linux/WSL) for performing file carving.

### ⚙️ The Configuration File: `scalpel.conf`

* **Essential!:** `Scalpel` needs this file to know what to look for. Without configuring it, it won't find anything useful.
* **Location:** Usually at `/etc/scalpel/scalpel.conf` (you can copy it and modify your copy).
* **What to do?:** You need to **edit** the `scalpel.conf` file and **uncomment** (remove the `#` from the beginning) the lines for the file types you want to search for (`JPG`, `PDF`, `DOCX`, etc.). Each line defines how to recognize a file type by its header/footer.

    **Example (searching for JPG):** Make sure the line for `jpg` does not have a `#` at the beginning:
    ```
    # This line is commented out (ignored):
    # jpg	y	200000000	\xff\xd8\xff\xe0\x00\x10\x4a\x46	\xff\xd9

    # This line is uncommented (active):
    jpg	y	200000000	\xff\xd8\xff\xe0\x00\x10\x4a\x46	\xff\xd9
    ```

### ⌨️ Basic Usage (Command Line)

Here are the main commands to run `Scalpel` in table format:

| Command                                        | Description                                                                       |
| :--------------------------------------------- | :-------------------------------------------------------------------------------- |
| `scalpel -c <config> -o <output> <image>`      | Basic execution using `<config>` file, saving to `<output>`, analyzing `<image>`. |
| `scalpel -b -c <config> -o <output> <image>`   | **Recommended (disk images):** Same, but `-b` carves only from unallocated space. |

**Key Parameters and Options:**

* `-c <config>`: **Path** to your edited `scalpel.conf` file (Essential!).
* `-o <output>`: **Path** to the directory where results will be saved (Must be new or empty!).
* `-b`: (Optional) Tells `Scalpel` to carve **only** from the unallocated space of the image. Very useful for disk images.
* `<image>`: **Path** to the forensic image file (e.g., `image.dd`) or device (e.g., `/dev/sdb`).

**Combined Practical Example:**

To search for `JPG`s and `PDF`s (defined in `my_scalpel.conf`) only in the unallocated space of `disk_image.dd` and save the results in `/mnt/forensics/scalpel_out`:

```bash
# Ensure the output directory exists and is empty before running
mkdir -p /mnt/forensics/scalpel_out
# Run scalpel targeting unallocated space (-b) with a custom config
scalpel -b -c /etc/scalpel/my_scalpel.conf -o /mnt/forensics/scalpel_out /media/forensics/disk_image.dd