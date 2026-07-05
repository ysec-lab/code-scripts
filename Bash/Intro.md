# Bash Scripting Introduction

*Bash (Bourne Again SHell) is the default command-line interpreter and scripting language for most Linux distributions and macOS (historically). Beginners should master core utilities like **ls, cd, grep, cat, chmod, and ssh.** Bash relies heavily on text streams, piping (`|`), file permissions, and combining small, single-purpose utilities to perform complex automation, system administration, and secure operations.*

---

## Bash Commands: Structure and Syntax

Unlike PowerShell's Verb-Noun structure, Bash commands generally follow a `Command [Options] [Arguments]` syntax. Options (or flags) modify the command's behavior, and arguments specify the targets (like files or directories).

**Common Command Syntax:**

| Component | Description | Example |
|---|---|---|
| **Command** | The executable or built-in utility | `ls` |
| **Short Options** | Single letters prefixed with a single dash (`-`) | `-l`, `-a`, or combined as `-la` |
| **Long Options** | Descriptive words prefixed with a double dash (`--`) | `--human-readable`, `--help` |
| **Arguments** | The target file, directory, or string | `/var/log` |

*Example:* `ls -la /var/log` (List all files, in long format, in the /var/log directory).

---

## Bash Aliases: Shortcuts for Efficiency

Aliases in Bash allow you to create custom shortcuts for long or frequently used commands. You can view current aliases by typing `alias`. To make them permanent, add them to your `~/.bashrc` or `~/.bash_profile`.

**Commonly Used Custom Aliases:**

| Alias | Command Mapping | Description |
|---|---|---|
| **ll** | `alias ll='ls -alF'` | Long listing format, showing hidden files and file types |
| **la** | `alias la='ls -A'` | List almost all (hidden files, minus `.` and `..`) |
| **..** | `alias ..='cd ..'` | Go up one directory level |
| **update** | `alias update='sudo apt update && sudo apt upgrade'` | Quickly update Debian/Ubuntu systems |
| **c** | `alias c='clear'` | Clear the terminal screen |

---

## Understanding Parameters and Variables in Bash

Bash scripts use positional parameters to handle command-line arguments passed to the script, and special variables for execution status.

*   **Positional Parameters:** Passed in order. `$1` is the first argument, `$2` is the second, etc.
    ```bash
    ./backup.sh /source/dir /dest/dir  # $1 is /source/dir, $2 is /dest/dir
    ```
*   **All Arguments (`$@` or `$*`):** Represents all arguments passed to the script.
    ```bash
    for arg in "$@"; do echo "$arg"; done
    ```
*   **Argument Count (`$#`):** The total number of arguments passed.
    ```bash
    if [ "$#" -ne 2 ]; then echo "Illegal number of parameters"; fi
    ```
*   **Exit Status (`$?`):** The exit code of the last run command (0 means success, >0 means error).
    ```bash
    ping -c 1 google.com; echo $?
    ```

---

## Most Used Commands

Here is a consolidated list of the most essential and frequently used commands for Linux/Unix system administrators:

| Command | Category | Description |
|---|---|---|
| `man` / `help` | Discovery | Opens the manual or help page for a specific command. |
| `pwd` / `cd` | File System | Print working directory / Change directory. |
| `ls` | File System | Lists files and directories. |
| `cat` / `less` | File System | Reads and concatenates files / Views file contents interactively. |
| `grep` | Text/Data | Searches text using regular expressions. |
| `awk` / `sed` | Text/Data | Powerful stream editors for text manipulation and extraction. |
| `top` / `htop` | System | Interactive process viewer and system monitor. |
| `ps` | System | Reports a snapshot of current processes. |
| `systemctl` | System | Controls the systemd system and service manager. |
| `ping` | Network | Sends ICMP echo requests to test connectivity. |
| `ip` / `ifconfig`| Network | Shows or manipulates routing, network devices, and interfaces. |
| `ssh` | Remote Mgmt | Secure Shell for remote login and command execution. |
| `chmod` / `chown`| Security | Changes file permissions / Changes file ownership. |
| `tar` / `zip` | Archiving | Creates, extracts, and manages compressed archive files. |
| `df` / `du` | Storage | Reports file system disk space usage / Estimates file space usage. |
| `find` | File System | Searches for files in a directory hierarchy. |

---

## Essential Bash Commands for Beginners

### The "Big Three" Discovery Commands
You can find information on almost any command in Linux using these built-in tools.

*   **man (Manual Pages):** Displays the comprehensive manual for a command.
    ```bash
    man ls
    man chmod
    ```
*   **help / --help:** Displays built-in bash help or quick usage syntax.
    ```bash
    cd --help
    ls --help
    ```
*   **whatis / type:** Shows a brief description or indicates how a command is interpreted.
    ```bash
    whatis grep
    type cd
    ```

### File and Directory Management
*   **Navigating Directories (`pwd`, `cd`, `ls`):**
    ```bash
    pwd                 # Print working directory
    cd /var/log         # Change to /var/log
    cd ~                # Change to current user's home directory
    ls -lah             # List all files, long format, human-readable sizes
    ```
*   **Creating, Copying, Moving, and Deleting Files:**
    ```bash
    mkdir -p /tmp/new/dir      # Create directory tree
    touch myfile.txt           # Create an empty file
    cp myfile.txt /tmp/        # Copy file
    cp -r /dir1 /dir2          # Copy directory recursively
    mv myfile.txt newfile.txt  # Rename or move file
    rm myfile.txt              # Remove file
    rm -rf /tmp/new/dir        # Force remove directory and contents (Use with caution!)
    ```
*   **Searching for Files (`find`, `locate`):**
    ```bash
    find /var/log -name "*.log"      # Find files ending in .log in /var/log
    find / -type f -size +100M       # Find files larger than 100MB
    ```

### System and Process Management / Automation
*   **Managing System Services (`systemctl`):**
    ```bash
    sudo systemctl status nginx      # Check service status
    sudo systemctl start nginx       # Start service
    sudo systemctl stop nginx        # Stop service
    sudo systemctl restart nginx     # Restart service
    sudo systemctl enable nginx      # Enable service to start on boot
    ```
*   **Working with Processes (`ps`, `kill`, `top`):**
    ```bash
    ps aux | grep nginx              # Find specific running processes
    top                              # Live process monitor (press 'q' to quit)
    htop                             # Enhanced interactive process monitor (if installed)
    kill 1234                        # Terminate process with PID 1234 gracefully
    kill -9 1234                     # Force kill process 1234
    killall nginx                    # Kill all processes named nginx
    ```

### Data and Content Handling Commands
Linux uses standard input (stdin), output (stdout), and error (stderr). You can pipe (`|`) output from one command to another, or redirect (`>`) it to a file.

*   **Reading and Writing to Files:**
    ```bash
    cat /var/log/syslog              # Output entire file to screen
    head -n 10 file.txt              # Read first 10 lines
    tail -f /var/log/syslog          # Read last 10 lines and follow live updates
    echo "Hello World" > file.txt    # Overwrite file with text
    echo "New Line" >> file.txt      # Append text to file
    ```
*   **Filtering and Text Manipulation (`grep`, `awk`):**
    ```bash
    grep "error" /var/log/syslog     # Find lines containing "error"
    grep -r "TODO" /var/www/         # Search recursively in a directory
    awk '{print $1}' access.log      # Print the first column of a file (e.g., IPs)
    ```

### System Configuration and Monitoring
*   **System Information Retrieval:**
    ```bash
    uname -a                         # Print all system information (kernel, architecture)
    lscpu                            # Display CPU architecture information
    free -m                          # Show free and used RAM in Megabytes
    df -h                            # Show disk space usage in human-readable format
    du -sh /var/log                  # Show total size of a specific directory
    ```
*   **Viewing System Logs:**
    ```bash
    journalctl -xe                   # View recent systemd logs
    journalctl -u nginx.service      # View logs for a specific service
    ```

### Network and Remote Management
*   **Checking Connectivity (`ping`, `curl`):**
    ```bash
    ping -c 4 google.com             # Send 4 ping requests
    curl -I https://google.com       # Fetch HTTP headers to test web connectivity
    ```
*   **Network Configuration (`ip`, `ss`):**
    ```bash
    ip a                             # Show IP addresses for all interfaces
    ip route                         # Show routing table
    ss -tuln                         # Show all listening TCP/UDP ports
    ```
*   **Working with Remote Sessions (`ssh`, `scp`):**
    ```bash
    ssh user@192.168.1.10            # Connect to remote host
    ssh -i ~/.ssh/key.pem user@host  # Connect using a specific SSH key
    scp file.txt user@host:/tmp/     # Securely copy file to remote host
    ```

### Security and Permissions
*   **Managing Permissions (`chmod`, `chown`):**
    Linux uses Read (r=4), Write (w=2), and Execute (x=1) permissions for User, Group, and Others.
    ```bash
    chmod +x script.sh               # Make a script executable
    chmod 755 /var/www/html          # Set rwx for user, rx for group and others
    chmod 600 ~/.ssh/id_rsa          # Set rw for user only (highly secure)
    sudo chown user:group file.txt   # Change file owner and group
    ```
*   **Privilege Escalation:**
    ```bash
    sudo command                     # Run a single command as root
    sudo su -                        # Switch to a root shell
    ```

## System Maintenance & Cleanup Scripts

*   **Package Management (Debian/Ubuntu/Mint):**
    ```bash
    sudo apt update                  # Update local package index
    sudo apt upgrade -y              # Upgrade all installed packages
    sudo apt autoremove -y           # Remove unused dependencies
    sudo apt clean                   # Clear downloaded archive cache
    ```
*   **Package Management (RHEL/CentOS/Fedora):**
    ```bash
    sudo dnf check-update            # Check for updates
    sudo dnf upgrade -y              # Upgrade packages
    sudo dnf clean all               # Clean package cache
    ```
*   **Clear System Logs & Temp Files:**
    ```bash
    sudo journalctl --vacuum-time=3d # Delete systemd logs older than 3 days
    sudo rm -rf /tmp/*               # Clear temporary directory
    ```

## Useful Tips & General Administration

*   **Interactive Terminal Keyboard Shortcuts:**
    *   **Ctrl + C**: Abort the currently running command.
    *   **Ctrl + Z**: Suspend the current process (send it to background). Use `fg` to bring it back.
    *   **Ctrl + R**: Reverse search your command history. Type a few letters of a past command to find it.
    *   **Ctrl + L**: Clear the terminal screen (same as `clear` command).
    *   **Ctrl + A / Ctrl + E**: Move cursor to the beginning / end of the line.
*   **Running Scripts in the Background:**
    ```bash
    ./long_script.sh &               # Run in background
    nohup ./long_script.sh &         # Run in background and keep running after logout
    ```

---

## External Bash Scripts and Resources
*   **Bash Reference Manual:** [GNU.org](https://www.gnu.org/software/bash/manual/bash.html)
*   **ShellCheck:** Static analysis tool for shell scripts (highly recommended to prevent bugs). [shellcheck.net](https://www.shellcheck.net/)
*   **ExplainShell:** Breaks down complex bash commands and explains what each flag does. [explainshell.com](https://explainshell.com/)
*   **Awesome Bash:** A curated list of delightful Bash scripts and resources. [GitHub](https://github.com/awesome-lists/awesome-bash)
*   **Linux Command Line Cheat Sheet:** [DaveChild Cheatography](https://cheatography.com/davechild/cheat-sheets/linux-command-line/)
