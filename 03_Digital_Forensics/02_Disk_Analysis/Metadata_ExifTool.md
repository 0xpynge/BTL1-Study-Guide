# 📑 Metadata Analysis with ExifTool

## What is Metadata?

> **Metadata** is, simply put, "data about data". It is descriptive information about a file that isn't part of the main visible content but can reveal a lot about its origin, history, and context.

Several types exist, but `ExifTool` primarily focuses on **embedded metadata**, which is stored _inside_ the file itself. Some common examples include:

* **EXIF (Exchangeable image file format):** Found in `JPEG` and `TIFF` images. Includes camera details (model, settings), capture date/time, and sometimes **GPS coordinates**.
* **IPTC:** Metadata for images used in journalism (captions, keywords, copyright).
* **XMP (Extensible Metadata Platform):** Adobe standard, embedded in `PDF`s, images, etc.
* **Document Metadata:** Author information, software used, editing dates, comments (in `PDF`s, Office documents, etc.).
* **Audio/Video Tags:** ID3 tags in `MP3`s, information in `MP4`, `AVI`, etc.

## 🕵️ Forensic Value of Metadata

Metadata analysis can provide crucial clues in an investigation:

* **Attribution:** Identifying the author of a document or the camera/device that took a photo/video.
* **Geolocation:** Determining where a photo or video was taken if it contains GPS coordinates.
* **Timelines:** Establishing creation or modification dates/times (which sometimes differ from the file system's).
* **Software Used:** Identifying the program (and its version) used to create or edit a file (can indicate tampering).
* **Hidden Information:** Finding comments, annotations, or deleted revisions within documents.
* **Consistency:** Comparing metadata between different files to find relationships or inconsistencies.

## 🛠️ ExifTool: The Tool

> **[ExifTool](https://exiftool.org/)** (by Phil Harvey) is a Perl library and a command-line application, **free and open-source**. It is extremely powerful and versatile, capable of reading, writing, and editing metadata from a **huge variety of file types** (images, audio, video, PDF, Office documents, and many more).

* **Platform:** Runs on Windows, macOS, and Linux.
* **Strength:** Recognizes a massive amount of different metadata tags.

### ⌨️ Basic Usage (Command Line)

Here is a table with the most common commands for using `ExifTool`:

| Task/Objective                 | Command                             | Notes/Usage                                                                                      |
| :----------------------------- | :---------------------------------- | :----------------------------------------------------------------------------------------------- |
| View **ALL** Metadata          | `exiftool <file>`                   | Displays all found metadata. Useful for initial exploration and seeing available tag names.        |
| View Specific Metadata         | `exiftool -Tag1 -Tag2 <file>`       | Displays only the requested tags (e.g., `-Author`, `-GPSPosition`). Use `exiftool <file>` to see names. |
| Extract Specific Tags (Table)  | `exiftool -T -Tag1 -Tag2 <file>`    | `-T` for tab-delimited output, easy to import into spreadsheets.                                 |
| Process Directory Recursively  | `exiftool -r <directory>`           | Analyzes all files within a directory and its subdirectories.                                     |
| Save Output to File            | `exiftool <file> > output.txt`      | Standard output redirection to save results to a text file.                                      |
| Save Metadata per File (Txt)   | `exiftool -w txt <directory>`       | Creates a `.txt` file for each processed file in `<directory>`, containing its metadata.       |
| **Remove** All Metadata        | `exiftool -all= <file>`             | **CAUTION!** Removes all metadata. Use only on copies if preserving evidence. Useful for sanitization. |
| Show ExifTool Version          | `exiftool -ver`                     | Displays the installed version.                                                                  |

* **Note on Tag Names:** Tag names (`TagName`) can vary. Use `exiftool <file>` to see all available tags in that specific file or consult the [ExifTool Tag Names documentation](https://exiftool.org/TagNames/).

### 🤔 Interpreting the Output

`ExifTool` generally displays output in the format `Tag Name : Value`. It's important to understand what each relevant tag means for your investigation (e.g., differentiating between the file's `CreateDate` and the camera's `DateTimeOriginal` in a photo).

### ⚠️ Forensic Considerations

* Run `ExifTool` on copies of the evidence whenever possible.
* Document the commands used and the relevant metadata found.
* Be aware that some metadata can be easily modified by a user. Compare with other evidence sources if possible.

---

> _`ExifTool` is an indispensable Swiss army knife for extracting hidden information within the files themselves, complementing file system analysis._