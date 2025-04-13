# 📋 Splunk Cheatsheet for Security Analysis

> **`Splunk`** is a powerful platform widely used as a SIEM and for general log analysis. It allows searching, analyzing, and visualizing large volumes of machine data. Its search language is called **`SPL` (Search Processing Language)**.

---

<details>
<summary><strong>💡 Basic Search Concepts (SPL)</strong></summary>
<br>

> The foundation of `Splunk` is its search bar, where you enter `SPL` queries. Key elements:

* **Time Range Picker:** **Fundamental.** Select the time range you want to search (last 15 min, last 24h, specific dates...). _Make sure it covers the period of your incident!_
* **Index (`index`):** Data is organized into indexes. You must specify which one to search.
    * `index="index_name"` (Ex: `index="os"`, `index="security"`, `index="firewall"`)
    * `index=*` (Searches all indexes, less efficient).
* **Common Metadata:** Fields that `Splunk` automatically adds or extracts during indexing. Useful for quick filtering:
    * `sourcetype="source_type"` (Ex: `sourcetype="WinEventLog:Security"`, `sourcetype="linux_secure"`, `sourcetype="cisco:asa"`)
    * `source="/path/or/filename"` (Ex: `source="/var/log/auth.log"`)
    * `host="hostname"` (Ex: `host="webserver01"`)
* **Keywords / Terms:** Search for events containing certain words. Use quotes `"` for exact phrases.
    * `sshd`
    * `"failed login"`
    * `error OR warning` (Boolean operators: `AND`, `OR`, `NOT`)
* **Fields (`field=value`):** Filter by specific values in fields extracted by `Splunk`.
    * `src_ip="192.168.1.10"`
    * `user="admin"`
    * `status_code=404`
    * `EventCode=4625` (Comparators can be used: `>`, `<`, `>=`, `<=`, `!=`)
* **Pipe (`|`): Essential.** Sends the results of an initial search to subsequent commands for processing (filter, transform, aggregate, format). `command1 | command2 | command3 ...`

</details>

---

<details>
<summary><strong>⌨️ Essential SPL Commands (Cheatsheet Table)</strong></summary>
<br>

> Here is a table with common and useful SPL commands for security analysis:

| SPL Command (Example)                       | Description                                                                                                                                      |
| :------------------------------------------ | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| `search <terms>` (implied)                  | Base command. Searches for matching events. Ex: `index="security" host="dc01" EventCode=4625`                                                      |
| **Filtering/Selection:** |                                                                                                                                                  |
| `head [N]`                                  | Shows the **first N** events (default 10) based on the current order (usually reverse chronological). Ex: `... \| head 20`                        |
| `tail [N]`                                  | Shows the **last N** events based on the current order.                                                                                           |
| `where <condition>`                         | Filters results **after** the initial search, using comparisons (`>`, `<`, `!=`, etc.), functions (`like()`, `cidrmatch()`). Ex: `... \| where process_name like "%.exe"` |
| **Grouping/Statistics:** |                                                                                                                                                  |
| `stats <func(field)> BY <group_field>`      | **Very powerful.** Performs statistical calculations. Functions: `count`, `dc` (distinct_count), `avg`, `sum`, `min`, `max`, `values`, `list`. Groups with `BY`. |
|  _*Ex: `... \| stats count BY src_ip`*_     | _*Counts events by source IP.*_                                                                                                                   |
|  _*Ex: `... \| stats dc(user) BY host`*_    | _*Counts unique users per host.*_                                                                                                                  |
|  _*Ex: `... \| stats values(action) AS Actions BY user`*_ | _*Lists unique actions per user (renaming the field to 'Actions').*_                                                               |
| `top <field> [limit=N]`                     | Shows the N most frequent values of a field, with their count and percentage. Ex: `index=fw "action=blocked" \| top src_ip limit=10`               |
| `rare <field> [limit=N]`                    | Shows the N least frequent values of a field. Useful for finding anomalies. Ex: `... \| rare user_agent limit=10`                              |
| **Formatting/Display:** |                                                                                                                                                  |
| `table <field1> <field2> ...`               | Displays results in a table, **only** with the specified fields. Ex: `... \| table _time, user, src_ip, dest_ip, status`                         |
| `fields [+]/- <field1> ...`                 | **Adds (`+`) or removes (`-`)** fields from the results view without hiding others. Ex: `... \| fields - _raw, host`                             |
| `rename <original_field> AS <new_field>`    | Renames a field for clarity. Ex: `... \| rename user AS Usuario`                                                                                 |
| `sort [+]/- <field>`                        | Sorts results by a field (`+` asc, `-` desc). Ex: `... \| sort -count` (sorts by 'count' field, largest to smallest)                            |
| **Time Visualization:** |                                                                                                                                                  |
| `timechart [span=<duration>] <func> BY <field>` | Creates time series charts. Similar to `stats` but with time as the implicit X-axis. `span` defines the interval (e.g., `1h`, `5m`).          |
|  _*Ex: `... \| timechart span=1h count BY EventCode`*_ | _*Chart of events per hour, split by event code.*_                                                                                       |

</details>

---

<details>
<summary><strong>🚀 Combined Examples (Formatted Table)</strong></summary>
<br>

<table>
  <thead>
    <tr>
      <th>Description</th>
      <th>SPL Query (Formatted to hinder easy copy-paste)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Searches <code>main</code> index for <code>src_ip=192.168.1.1</code>.</td>
      <td><code>index="main" src_ip="192.168.1.1"</code></td>
    </tr>
    <tr>
      <td>Searches <code>security</code> index for <code>syslog</code> events with <code>error</code>.</td>
      <td><code>index="security" sourcetype="syslog" error</code></td>
    </tr>
    <tr>
      <td>Counts events by <code>sourcetype</code> in <code>security</code> index.</td>
      <td><code>index="security" | stats count BY sourcetype</code></td>
    </tr>
    <tr>
      <td>Shows 10 most recent events from <code>security</code> index.</td>
      <td><code>index="security" | head 10</code></td>
    </tr>
    <tr>
      <td>Shows table with specific fields from <code>security</code> index.</td>
      <td><code>index="security" | table _time, host, src_ip, dest_ip, _raw</code></td>
    </tr>
    <tr>
      <td>Creates hourly chart by <code>sourcetype</code> for <code>security</code> index.</td>
      <td><code>index="security" | timechart span=1h count BY sourcetype</code></td>
    </tr>
    <tr>
      <td>Finds <code>"failed login"</code> in <code>main</code> index, shows top <code>src_ip</code>.</td>
      <td><code>index="main" "failed login" | top src_ip</code></td>
    </tr>
  </tbody>
</table>

</details>

---

<details>
<summary><strong>⭐ Quick Search Tips</strong></summary>
<br>

* **Filter Early, Filter Often:** Use `index`, `sourcetype`, `host`, and keywords at the beginning to reduce the data volume before using costly `|` commands.
* **Iterate:** Start with broad searches and progressively add filters or `|` commands to refine results.
* **`_time`:** Use the interface's **time range picker**; it's more efficient than filtering by `_time` in the search itself.
* **`Fast` vs `Smart` vs `Verbose` Mode:** Affects how `Splunk` processes and displays data. `Fast` is quicker but extracts fewer fields; `Verbose` extracts everything. `Smart` (default) tries to balance.
* **Read the Docs!** `Splunk SPL` has many commands and functions: [Splunk Search Reference](https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/WhatsInThisManual).

</details>

---

> _Mastering `SPL` is a key skill for any analyst working with `Splunk`. Start with these basic commands and practice building queries to answer specific questions._