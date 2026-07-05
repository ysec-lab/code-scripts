# PowerShell Introduction

*PowerShell is a command-line shell and scripting language for automating Windows administration and managing systems across platforms. Beginners should master cmdlets like **Get-Help, Get-ChildItem, Get-Process, Set-Location, New-Item, and Export-Csv.** PowerShell supports object-based pipelines, aliases for efficiency, remote sessions, and execution policies for security. With practice, these commands form the foundation for automation, system management, and secure scripting.*

---

## PowerShell Cmdlets: Verb-Noun Structure

PowerShell commands, called "**cmdlets**", follow a simple **Verb-Noun** naming structure. The verb part specifies the action to be performed, and the noun part of the command defines the object on which the action will be performed. Examples include `Get-process`, `New-item`, `Set-Date` and `Remove-Item`.

**Common verbs include:**

| Verb | Action Description |
|---|---|
| **Get** | Retrieves data |
| **Set** | Modifies the properties of an object |
| **Add** | Adds an item to a collection |
| **Stop** | Stops a process or service |
| **Start** | Starts a process or service |
| **Clear** | Removes all the items from a collection |

---

## PowerShell Aliases: Shortcuts and Legacy Support

Aliases are short names for cmdlets, functions and scripts. Aliases are used for typing efficiency and to support legacy versions. You can see all the aliases defined in the current PowerShell session by entering the `Get-Alias` command.

**Here are some commonly used aliases:**

| Alias | Cmdlet Mapping |
|---|---|
| **Ls** | Alias for `Get-ChildItem` |
| **Gci** | Another alias for `Get-ChildItem` |
| **Gc** | Alias for `Get-Content` |
| **Rm** | Alias for `Remove-Item` |
| **Cd** | Alias for `Set-Location` |
| **Cls** | Alias for `Clear-Host` |
| **Dir** | Alias for `Get-ChildItem` |

---

## Understanding Parameters in PowerShell Commands

Parameters are used to pass values or options to commands, functions and scripts to modify their actions. To see details about the parameters of a cmdlet, run `Get-Help`.

The basic types of parameters are as follows:

*   **Positional parameters** are passed in a specific order without explicitly naming them:
    ```powershell
    Copy-Item "C:\File1.txt" "D:\Backup\"
    ```
*   **Named parameters** are specified with the `-` prefix and can be provided in any order:
    ```powershell
    Get-Process -Name "notepad" -Id 1234
    ```
*   **Switch parameters** do not require a value because they work like Boolean flags:
    ```powershell
    Remove-Item "C:\temp\file.txt" -Confirm
    ```
*   **Mandatory parameters** must be supplied; if omitted, PowerShell will prompt for a value:
    ```powershell
    Get-MyProcess -ProcessName "chrome"
    ```

---

## Most Used Commands

Here is a consolidated list of the most essential and frequently used commands for system administrators:

| Command | Category | Description |
|---|---|---|
| `Get-Command` | Discovery | Lists all available cmdlets, functions, and aliases installed on your system. |
| `Get-Help` | Discovery | Provides syntax, parameter details, and usage examples for any command. |
| `Get-Member` | Discovery | Reveals the hidden properties and methods of the object returned by a command. |
| `Get-Process` | System | Lists all active processes running on your computer. |
| `Get-Service` | System | Shows the status of all software services installed on the OS. |
| `Get-EventLog` | Monitoring | Gets entries from local event logs. |
| `Get-ComputerInfo` | System | Pulls massive system specs, including OS version, hardware, and BIOS details. |
| `Test-Connection` | Network | Sends an ICMP echo request (ping) to check connectivity. |
| `Out-File` | Output | Sends output directly to a file. |
| `Export-Csv` | Output | Converts objects into a series of comma-separated value strings and saves them to a file. |
| `Get-ExecutionPolicy` | Security | Shows your current script security restrictions. |
| `Set-ExecutionPolicy` | Security | Changes the user preference for the Windows PowerShell execution policy. |
| `Get-AdUser` | Active Directory | Gets one or more Active Directory users. |
| `Get-AdComputer` | Active Directory | Gets one or more Active Directory computers. |
| `Get-AdGroup` | Active Directory | Gets one or more Active Directory groups. |
| `Get-ChildItem` | File System | Lists all files and folders inside the current directory (Alias: ls / dir). |
| `Set-Location` | File System | Changes your working directory to a new path (Alias: cd). |
| `Get-WmiObject` | System | Queries Windows Management Instrumentation (WMI) objects for detailed system info. |
| `Enter-PSSession` | Remote Mgmt | Starts an interactive PowerShell session with a remote computer. |
| `Invoke-Command` | Remote Mgmt | Runs commands on local and remote computers non-interactively. |

---

## Essential PowerShell Commands for Beginners

### The "Big Three" Discovery Commands
You can find, understand, and safely test almost any command in PowerShell using just these three built-in tools.

*   **Get-Command**: Lists all commands.
    ```powershell
    Get-Command *Service*
    ```
*   **Get-Help**: Displays information about a cmdlet.
    ```powershell
    Get-Help Get-ChildItem
    Get-Help Get-ChildItem -examples
    ```
*   **Get-Member**: Explores object properties and methods.
    ```powershell
    Get-Date | Get-Member -MemberType property
    ```

### File and Folder Management
*   **Navigating Directories (`Set-Location`, `Get-ChildItem`):**
    ```powershell
    Get-Location  # Displays current path (pwd)
    Set-Location -Path "D:\Office\Project"
    Get-ChildItem "D:\Office\Project" # Lists files/folders
    Get-ChildItem "D:\Office\Project" -file # List only files
    ```
*   **Creating, Copying and Deleting Files:**
    ```powershell
    New-Item -Path "D:\Office \Project\myfile.txt" -ItemType File
    Copy-Item -Path "D:\Office \Project\myfile.txt" -Destination "D:\Office \Project\startup\myfile.txt"
    Remove-Item -Path "D:\Office \Project\myfile.txt"
    Remove-Item -Path "D:\Office \Project" -Recurse # Deletes folder and contents
    ```
*   **Checking Folder Contents and Searching:**
    ```powershell
    Get-ChildItem -Path "D:\Office\Project" -Filter "*.txt"
    Select-String -Path "D:\Office\Project\projectlogs.txt" -Pattern "error"
    ```

### System and Process Management / Automation
*   **Managing System Services (`Get-Service`, `Start-Service`, `Stop-Service`):**
    ```powershell
    Get-Service -Name "*SQL*"
    Get-Service | Where-Object { $_.Status -eq 'Running' }
    Start-Service -Name "spooler"
    Stop-Service -Name "autotimesvc"
    ```
*   **Working with Processes (`Get-Process`, `Start-Process`, `Stop-Process`):**
    ```powershell
    Get-Process -Name notepad
    Get-Process | Where-Object {$_.CPU -gt 10}
    Start-Process chrome.exe "https://www.google.com"
    Stop-Process -Name notepad, chrome
    ```
*   **Accessing and Monitoring System Logs:**
    ```powershell
    Get-EventLog -LogName System -Newest 10
    Get-WinEvent -LogName Application -MaxEvents 10
    ```

### Data and Content Handling Commands
*   **Reading and Writing to Files:**
    ```powershell
    Get-Content -Path "D:\Office\Project\myfile.txt"
    Set-Content -Path "D:\Office\Project\myfile.txt" -Value "Welcome to PowerShell blog"
    "Hello, World!" | Out-File -FilePath "D:\Office\Project\myfile.txt"
    ```
*   **Exporting and Importing Data:**
    ```powershell
    Get-Process -Name notepad | Select-Object Name, Id, CPU | Export-Csv -Path "D:\Office\Project\Processes.csv" -NoTypeInformation
    Import-Csv -Path "D:\Office\Project\Processes.csv" | ForEach-Object { Get-Process -Id $_.Id }
    ```

### Windows System Configuration
*   **System Information Retrieval:**
    ```powershell
    Get-ComputerInfo | Select-Object CsName, WindowsVersion, WindowsBuildLabEx, OsArchitecture
    ```
*   **Get-WmiObject (Hardware & OS Details):**
    ```powershell
    Get-WmiObject -Class Win32_OperatingSystem
    Get-WmiObject -Class Win32_BIOS
    ```

### Network and Remote Management
*   **Checking Connectivity (`Test-Connection`, `Resolve-DnsName`):**
    ```powershell
    Test-Connection -ComputerName google.com
    Test-NetConnection -ComputerName google.com -Port 443
    Resolve-DnsName -Name DC1
    ```
*   **Working with Remote Sessions & Commands:**
    ```powershell
    Enable-PSRemoting -Force
    Enter-PSSession -ComputerName "DC1"
    Invoke-Command -ComputerName DC1 -ScriptBlock { Get-Process -Name Notepad }
    ```

### Security and Execution Policies
*   **PowerShell Execution Policy:**
    ```powershell
    Get-ExecutionPolicy
    Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine
    ```
*   **Managing Permissions and Roles (`Get-ACL`, `Set-ACL`):**
    ```powershell
    $SourcefileACL = Get-Acl -Path "D:\Office\Project\Processes.csv"
    Set-ACL -Path "D:\Office\Project\Processes1.json" -AclObject $SourcefileACL
    ```

### Active Directory Management
*   **Domain & User Commands:**
    ```powershell
    Get-ADDomain
    Get-ADUser username -Properties * | Select name, department, title
    Search-ADAccount -LockedOut
    Unlock-ADAccount –Identity john.smith
    ```
*   **Group & Computer Commands:**
    ```powershell
    Get-ADGroupMember -identity "HR Full"
    Add-ADGroupMember -Identity group-name -Members User1, user2
    Get-AdComputer -filter * | select name
    ```

### Office 365 PowerShell Commands
*   **Connect to Exchange Online:**
    ```powershell
    $UserCredential = Get-Credential 
    $Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://ps.outlook.com/powershell/ -Credential $UserCredential -Authentication Basic -AllowRedirection 
    Import-PSSession $Session
    ```
*   **Mailbox Management:**
    ```powershell
    Get-Mailbox email-address | fl
    Enable-RemoteMailbox username -RemoteRoutingAddress "username@tenant.mail.onmicrosoft.com"
    ```

### System Maintenance & Cleanup Scripts
*   **Clear Temporary Files & Recycle Bin:**
    ```powershell
    Remove-Item -Path "$env:TEMP\*" -Recurse -Force -ErrorAction SilentlyContinue
    Remove-Item -Path "C:\Windows\Temp\*" -Recurse -Force -ErrorAction SilentlyContinue
    Clear-RecycleBin -Force -ErrorAction SilentlyContinue
    ```
*   **Clean Windows Update Cache:**
    ```powershell
    Stop-Service -Name wuauserv -Force
    Remove-Item -Path "C:\Windows\SoftwareDistribution\Download\*" -Recurse -Force -ErrorAction SilentlyContinue
    Start-Service -Name wuauserv
    ```

### Useful Tips & General Administration
*   **Interactive Terminal Keyboard Shortcuts:**
    *   **Ctrl + C**: Sends a break signal to abort the active command instantly.
    *   **Ctrl + Break**: Forcibly terminates the console session if Ctrl + C is unresponsive.
*   **Kill Background Process (From another terminal):**
    ```powershell
    Stop-Process -Name "powershell" -Force
    ```

---

## External PowerShell Scripts and Resources
*   **Maintain system administration tasks:** [Microsoft Learn Path](https://learn.microsoft.com/en-us/training/paths/maintain-system-administration-tasks-windows-powershell/)
*   **Ultimate Windows Utility (Chris Titus):** `iwr -useb https://christitus.com/win | iex`
*   **Windows 10 Decrapifier:** [Spiceworks Script](https://community.spiceworks.com/scripts/show/4378-windows-10-decrapifier-1803-1809)
*   **Run PowerShell as Local Admin:** [GitHub](https://github.com/samersultan/Powershell-as-local-admin)
*   **Intune PowerShell Scripts:** [Microsoft Graph GitHub](https://github.com/microsoftgraph/powershell-intune-samples)
*   **Active Directory Script Library:** [GitHub](https://github.com/AndrewEllis93/PowerShell-Scripts)
