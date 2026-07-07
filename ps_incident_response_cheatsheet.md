# PowerShell Incident Response Cheat Sheet

---

## Table of Contents
- [Core Cmdlets](#core-cmdlets)
- [Formatting and Output Cmdlets](#formatting-and-output-cmdlets)
- [Collection Examples](#collection-examples)
  - [Remote Collection](#remote-collection)
  - [Machine and OS Information](#machine-and-os-information)
  - [Local User Accounts and Login Artifacts](#local-user-accounts-and-login-artifacts)
  - [Running Process Artifacts](#running-process-artifacts)
  - [Network Artifacts](#network-artifacts)
  - [Share and Drive Artifacts](#share-and-drive-artifacts)
  - [Autorun Artifacts](#autorun-artifacts)
  - [Service Artifacts](#service-artifacts)
  - [Scheduled Task Artifacts](#scheduled-task-artifacts)
  - [Event Log Artifacts](#event-log-artifacts)
  - [File System and Software Artifacts](#file-system-and-software-artifacts)
  - [Microsoft Defender Exclusions](#microsoft-defender-exclusions)
  - [Installed and Loaded Drivers](#installed-and-loaded-drivers)
- [PowerShell Incident Response Frameworks](#powershell-incident-response-frameworks)
- [References](#references)

---

## Core Cmdlets

A cmdlet is a lightweight, single-function command built into PowerShell designed to perform specific tasks. They serve as the core building blocks for gathering and analyzing data efficiently.

| Command | Alias | Description |
| :--- | :--- | :--- |
| `Get-Item` | `gi` | Retrieves metadata about a single file or registry key. |
| `Get-ChildItem` | `dir`, `gci`, `ls` | Lists files and directories (browsing file system paths/artifacts). |
| `Get-ItemProperty` | `gp` | Retrieves file or registry key properties (`LastAccessTime`, data, etc.). |
| `Get-Content` | `cat`, `gc`, `type` | Reads contents of text-based log files, scripts, and configuration files. |
| `Select-String` | `sls` | Searches file contents or command output for keywords (regex supported). |
| `Get-WmiObject` | `gwmi` | Queries WMI providers for system information (*Deprecated legacy use*). |
| `Get-CimInstance` | `gcim` | Queries CIM/WMI providers for system info (*Modern replacement*). |
| `Get-Process` | `gps`, `ps` | Lists all running processes with names, PIDs, and resource usage. |
| `Get-Service` | `gsv` | Lists services and their states (running, stopped, startup type, etc.). |
| `Get-ScheduledTask` | *(none)* | Lists scheduled tasks and their execution details. |
| `Get-WinEvent` | *(none)* | Retrieves event logs (System, Security, Application, etc.) with advanced filtering. |
| `Get-NetTCPConnection`| *(none)* | Lists active TCP connections and listening ports, including owning PIDs. |
| `Get-NetAdapter` | *(none)* | Displays network interface details and configuration. |
| `Get-NetIPAddress` | *(none)* | Displays IP address configuration (IPv4/IPv6) and bound interfaces. |
| `Get-SmbShare` | *(none)* | Lists SMB file shares hosted on the system, including their paths. |
| `Get-DnsClientCache` | *(none)* | Shows cached DNS query results on the local machine. |
| `Get-LocalUser` | `glu` | Lists local user accounts on the system. |
| `Get-LocalGroupMember`| `glgm` | Shows members of local groups (e.g., Administrators). |
| `Get-HotFix` | *(none)* | Lists installed updates and patches on the system. |
| `Get-PnpDevice` | *(none)* | Enumerates plug-and-play devices, including USB hardware history. |
| `Get-MpPreference` | *(none)* | Retrieves Windows Defender preferences, exclusions, and scan settings. |
| `Get-Acl` | *(none)* | Displays file or registry permissions (Access Control Lists). |
| `Get-Date` | *(none)* | Displays current system time. |
| `Get-FileHash` | *(none)* | Generates file hashes (MD5/SHA1/SHA256) for integrity or IOC matching. |
| `Write-Host` | *(none)* | Displays messages directly to the console (used for interactive output). |

---

## Formatting and Output Cmdlets

PowerShell’s pipeline enables you to quickly sort, filter, and transform raw output to surface anomalies and malicious patterns.

| Command | Alias | Description |
| :--- | :--- | :--- |
| `Format-Table` | `ft` | Displays output in a clean table layout with explicit columns. |
| `Format-List` | `fl` | Displays all selected properties in an expanded list format. |
| `Format-Wide` | `fw` | Shows only a single property per object, across multiple columns. |
| `Select-Object` | `select` | Chooses specific properties from objects; selects first/last `N` results. |
| `Sort-Object` | `sort` | Sorts objects in ascending or descending order based on properties. |
| `Group-Object` | `group` | Groups objects that share a common property and displays a frequency count. |
| `Measure-Object` | `measure` | Calculates statistics (count, sum, average, min, max) for numeric/string values. |
| `Out-String` | *(none)* | Converts pipeline objects into a single string representation. |
| `Out-File` | *(none)* | Sends formatted output to a raw text file instead of the console display. |
| `ConvertTo-Json` | *(none)* | Converts structured PowerShell objects into standard JSON format. |
| `ConvertTo-Csv` | *(none)* | Converts pipeline objects into comma-separated values (CSV) string format. |
| `Export-Csv` | `epcsv` | Writes object data directly to an external flat CSV file. |

---

## Collection Examples

> **Note:** It is generally recommended to use more than one method to validate findings and reduce the risk of missing or tampered data during an investigation.

### Remote Collection
Scale data collection efforts across multiple target systems across the network:
```powershell
Get-CimInstance -ClassName Win32_OperatingSystem -ComputerName WKS-4F1D, WKS-8N3K
```
*The `-ComputerName` parameter specifies the target hosts (can be FQDN, NetBIOS, or an IP address).*

### Machine and OS Information
Collect operating system details, version metadata, and system uptime milestones:
```powershell
# Includes OS version, build, serial number, install date, etc.
Get-CimInstance -ClassName Win32_OperatingSystem | Select-Object * | Format-List

# Retrieves the system’s last boot time to map out unexpected restarts
Get-CimInstance Win32_OperatingSystem | Select-Object LastBootUpTime
```

### Local User Accounts and Login Artifacts
Audit persistence mechanisms, hidden accounts, and active session environments:
```powershell
# Lists all local user accounts configured, including disabled or hidden accounts
Get-LocalUser

# Retrieves information about currently active logon sessions and connected users
Get-CimInstance -ClassName Win32_LoggedOnUser

# Displays the computer's hostname and the current interactive logged-in user
Get-CimInstance -ClassName Win32_ComputerSystem | Select-Object Name, UserName
```

### Running Process Artifacts
Investigate active binaries, running processes, resource consumption, and process lineage:
```powershell
# Lists all running processes with process name, PID, and resource usage
Get-Process

# Retrieves processes by a specific name or wildcard matching
Get-Process -Name svchost

# Retrieves a process by its PID and lists its full executable path
Get-Process -Id 14012 | Select-Object Name, Path

# Displays all DLL modules loaded by a specific process (including full path and size)
Get-Process -Name audiodg -Module

# Sorts all processes by CPU usage and returns the top 5 most resource-intensive instances
Get-Process | Sort-Object CPU -Descending | Select-Object -First 5 Name, Id, CPU

# Generates SHA256 file hashes for all running processes that have a valid disk path
# Change -Algorithm to SHA1 or MD5 if required for legacy IOC correlation
Get-Process | Where-Object { $_.Path } | ForEach-Object { Get-FileHash $_.Path }

# Retrieves running processes with their parent PIDs and full launch command lines
Get-CimInstance -ClassName Win32_Process | Select-Object Name, ProcessId, ParentProcessId, ExecutablePath, CommandLine

# Filters running processes via CIM to find specific targets
Get-CimInstance -ClassName Win32_Process -Filter "Name = 'chrome.exe'"
```

### Network Artifacts
Enumerate current outbound/inbound network connections, listeners, and DNS history:
```powershell
# Retrieves all active, established TCP connections on the target system
Get-NetTCPConnection -State Established

# Retrieves active UDP listening endpoints, showing local IP, port, and owning PID
Get-NetUDPEndpoint | Select-Object LocalAddress, LocalPort, OwningProcess

# Gathers full IP configurations, adapter profiles, and local routing table maps
Get-NetIPConfiguration; Get-NetIPAddress; Get-NetAdapter; Get-NetRoute

# Retrieves the DNS resolver cache entries to find outbound beaconing or resolution history
Get-DnsClientCache

# Audits active Windows Firewall rules to check for unauthorized host exceptions
Get-NetFirewallRule
```

### Share and Drive Artifacts
Enumerate attached volumes, networked file systems, and historical peripheral attachments:
```powershell
# Exports the list of active SMB shares on the host directly into CSV format
Get-SmbShare | ConvertTo-Csv -NoTypeInformation

# Displays active SMB client connections and remote servers the local machine is connected to
Get-SmbConnection

# Lists all file system drives, including mapped network drives and local logical volumes
Get-PSDrive -PSProvider FileSystem

# Retrieves information about connected USB hubs and device endpoints
Get-CimInstance Win32_USBHub

# Reads registry entries that track USB storage devices previously attached to the system
Get-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Enum\USBSTOR"
```

### Autorun Artifacts
Identify persistence configurations across standard persistent registry hive paths:
```powershell
# Lists programs set to execute automatically at system startup for all users (System-wide)
Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows\CurrentVersion\Run"

# Lists programs set to execute automatically at system startup for the current user
Get-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run"

# Lists programs configured to execute exactly once at next system startup
Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows\CurrentVersion\RunOnce"
```
*For a full list of expanded registry run keys, see: [soc201.com/he1z97](https://soc201.com/he1z97)*

### Service Artifacts
Audit running background services, persistent binaries, and trigger modes:
```powershell
# Lists all services showing their name, display name, status, and startup configuration
Get-Service | Select-Object Name, DisplayName, Status, StartType

# Filters and returns only the background services actively running on the host
Get-Service | Where-Object { $_.Status -eq 'Running' }

# Displays all services explicitly configured to automatically boot with the OS
Get-Service | Where-Object StartType -eq 'Automatic'

# Retrieves deep service definitions via CIM/WMI, exposing the absolute path to the binary
Get-CimInstance -ClassName Win32_Service | Select-Object Name, DisplayName, State, StartMode, PathName
```

### Scheduled Task Artifacts
Inspect configured task plans, historical runtimes, and anomalous task automation:
```powershell
# Lists all scheduled tasks with names, internal folder paths, and operational status
Get-ScheduledTask | Select-Object TaskName, TaskPath, State

# Obtains comprehensive runtime execution analytics on all non-disabled scheduled tasks
Get-ScheduledTask | Where-Object {$_.State -ne "Disabled"} | Get-ScheduledTaskInfo

# Lists all active scheduled tasks that have either never run or executed within the past 7 days
Get-ScheduledTask | Where-Object {($_.State -ne 'Disabled') -and (($_.LastRunTime -eq $null) -or ($_.LastRunTime -gt (Get-Date).AddDays(-7)))} | ConvertTo-Csv -NoTypeInformation
```

### Event Log Artifacts
Interact directly with Windows Event Logs to parse security audit events and error frames:
```powershell
# Retrieves a clean catalog listing of all event logs registered across the system
Get-WinEvent -ListLog *

# Queries the Security log for Event ID 4625 (Failed User Logon Attempts)
Get-WinEvent -FilterHashtable @{LogName="Security"; ID=4625}

# Bulk retrieves events tied to account management, privilege adjustments, or log clearing actions
# Includes Event IDs: 4720, 4722, 4724, 4738, 4732 (User/Group management) and 1102 (Audit log cleared)
Get-WinEvent -FilterHashtable @{LogName="Security"; ID=4720,4722,4724,4738,4732,1102}
```

### File System and Software Artifacts
Discover misplaced binaries, local software registries, and user history points:
```powershell
# Recursively searches the C:\ drive for specific files while ignoring access errors
Get-ChildItem C:\ -Recurse -Filter calc.exe -ErrorAction SilentlyContinue | Select-Object -ExpandProperty FullName

# Lists recent shortcut (.LNK) files indicating accessed documents, directories, or programs
Get-ChildItem "$env:APPDATA\Microsoft\Windows\Recent"

# Retrieves a file listing of the Prefetch directory to reconstruct execution histories
Get-ChildItem C:\Windows\Prefetch

# Fetches user-typed URLs stored in the historical registry hive for Internet Explorer
Get-ItemProperty -Path "HKCU:\Software\Microsoft\Internet Explorer\TypedURLs"

# Lists installed software packages derived from the Windows Installer database
Get-CimInstance Win32_Product | Select-Object Name, Version, InstallDate
```

#### Registry Alternatives for Fast Software Auditing
Querying the Windows Installer database service can be slow or heavy. Use these fast registry alternative queries:
```powershell
# List installed software from the 64-bit machine uninstall registry key
Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\*" | Select-Object DisplayName, DisplayVersion, InstallDate

# List installed software from the 32-bit machine uninstall registry key
Get-ItemProperty -Path "HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | Select-Object DisplayName, DisplayVersion, InstallDate

# List installed software registered exclusively for the current user profile
Get-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Uninstall\*" | Select-Object DisplayName, DisplayVersion, InstallDate

# Retrieves current information about Volume Shadow Copies (snapshots) on the volume
Get-CimInstance Win32_ShadowCopy
```
*Browser history database files for Edge, Chrome, and Firefox generally require raw database parsing outside of PowerShell. Paths of interest include:*
* `C:\Users\$Username\AppData\Roaming\Mozilla\Firefox\Profiles\`
* `C:\Users\$Username\AppData\Local\*\*\User Data\*\`

### Microsoft Defender Exclusions
Identify potential blind spots or directory bypasses injected by an adversary within Windows Defender:
```powershell
# Retrieves all Antivirus exclusion paths, extensions, process criteria, and IP limits
Get-MpPreference | Select-Object -Property ExclusionPath, ExclusionExtension, ExclusionIpAddress, ExclusionProcess
```

### Installed and Loaded Drivers
Audit the kernel space for rogue drivers or Bring Your Own Vulnerable Driver (BYOVD) tactics:
```powershell
# Lists all system drivers showing service identifier, current state, and disk path
Get-CimInstance Win32_SystemDriver | Select-Object Name, State, PathName
```

---

## PowerShell Incident Response Frameworks

When standard playbooks face unique investigative challenges, modular, community-driven frameworks can significantly accelerate large-scale live collection and deep disk analysis:

* **Kansa:** A modular incident response framework for large-scale data collection and baseline analysis across networks.
  * [GitHub Repository](https://soc201.com/YygzAJ)
* **Invoke-IR (PowerForensics):** An all-inclusive framework built for hard drive forensic analysis and raw disk parsing.
  * [GitHub Repository](https://soc201.com/u3cBSA)
* **PowerShell Rapid Response:** A powerful set of WMI scripts engineered specifically for live triage investigators.
  * [GitHub Repository](https://soc201.com/U4iCFN)
* **PSRecon:** Automatically handles live host triage collections and structures data points elegantly into operational report files.
  * [GitHub Repository](https://soc201.com/jLNgnU)
