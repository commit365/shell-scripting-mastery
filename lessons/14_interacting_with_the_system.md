# Interacting with the System in Shell Scripting

This lesson covers how to interact with the system using shell scripts, focusing on environment variables, retrieving system information and hardware details, and performing network operations.

## 1. Environment Variables

Environment variables are dynamic-named values that affect the way running processes behave on a computer. They are part of the environment in which a process runs.

### 1.1 Viewing Environment Variables

To view all environment variables:

```bash
env
# or
printenv
```

To view a specific environment variable:

```bash
echo $VARIABLE_NAME
# Example:
echo $HOME
```

### 1.2 Setting Environment Variables

To set an environment variable for the current session:

```bash
export VARIABLE_NAME=value
# Example:
export MY_VAR="Hello, World!"
```

To set an environment variable for a single command:

```bash
VARIABLE_NAME=value command
# Example:
DEBUG=true ./myscript.sh
```

### 1.3 Unsetting Environment Variables

To unset (remove) an environment variable:

```bash
unset VARIABLE_NAME
```

### 1.4 Persistent Environment Variables

To make environment variables persistent, add them to shell configuration files:

- For bash: `~/.bashrc` or `~/.bash_profile`
- For zsh: `~/.zshrc`

Example:

```bash
echo 'export MY_VAR="Hello, World!"' >> ~/.bashrc
```

### 1.5 Important Environment Variables

Some commonly used environment variables:

- `PATH`: Directories to search for executable files
- `HOME`: Current user's home directory
- `USER`: Current username
- `SHELL`: Path to the current user's shell
- `LANG`: Current language and locale settings
- `PWD`: Current working directory

### 1.6 Modifying PATH

To add a directory to PATH:

```bash
export PATH=$PATH:/new/directory
```

### 1.7 Environment Variables in Scripts

In scripts, you can use environment variables like any other variable:

```bash
#!/bin/bash
echo "Home directory: $HOME"
echo "Current user: $USER"
echo "Current PATH: $PATH"
```

## 2. System Information and Hardware Details

Shell scripts can retrieve various system and hardware information.

### 2.1 Basic System Information

#### 2.1.1 Operating System Details

```bash
# OS Name and Version
cat /etc/os-release

# Kernel Version
uname -r

# System Architecture
uname -m
```

#### 2.1.2 Hostname

```bash
hostname
```

#### 2.1.3 Uptime

```bash
uptime
```

### 2.2 CPU Information

```bash
# CPU Model
cat /proc/cpuinfo | grep "model name" | head -n 1

# Number of CPU Cores
nproc

# CPU Usage
top -bn1 | grep "Cpu(s)" | sed "s/.*, *$$[0-9.]*$$%* id.*/\1/" | awk '{print 100 - $1"%"}'
```

### 2.3 Memory Information

```bash
# Total and Used Memory
free -h

# Detailed Memory Info
cat /proc/meminfo
```

### 2.4 Disk Information

```bash
# Disk Usage
df -h

# Disk Partitions
fdisk -l

# Mounted Filesystems
mount
```

### 2.5 Network Interfaces

```bash
# List Network Interfaces
ip addr

# Network Interface Statistics
netstat -i
```

### 2.6 Hardware Information

```bash
# General Hardware Info
lshw

# PCI Devices
lspci

# USB Devices
lsusb
```

### 2.7 System Logs

```bash
# View System Logs
journalctl

# Kernel Ring Buffer
dmesg
```

### 2.8 Creating a System Information Script

Here's an example script that gathers various system information:

```bash
#!/bin/bash

echo "System Information:"
echo "==================="
echo "Hostname: $(hostname)"
echo "OS: $(cat /etc/os-release | grep PRETTY_NAME | cut -d'"' -f2)"
echo "Kernel: $(uname -r)"
echo "Uptime: $(uptime -p)"
echo "CPU: $(cat /proc/cpuinfo | grep "model name" | head -n 1 | cut -d':' -f2 | xargs)"
echo "CPU Cores: $(nproc)"
echo "Memory: $(free -h | awk '/^Mem:/ {print $2}')"
echo "Disk Usage: $(df -h / | awk '/\// {print $5}')"
echo "IP Address: $(hostname -I | awk '{print $1}')"
```

## 3. Network Operations

Shell scripts can perform various network operations to interact with remote systems and services.

### 3.1 Checking Network Connectivity

#### 3.1.1 Ping

To check if a host is reachable:

```bash
ping -c 4 example.com
```

#### 3.1.2 Traceroute

To trace the route packets take to a host:

```bash
traceroute example.com
```

### 3.2 DNS Lookups

#### 3.2.1 nslookup

To query DNS records:

```bash
nslookup example.com
```

#### 3.2.2 dig

For more detailed DNS information:

```bash
dig example.com
```

### 3.3 Downloading Files

#### 3.3.1 wget

To download files from the web:

```bash
wget https://example.com/file.zip
```

#### 3.3.2 curl

To transfer data from or to a server:

```bash
curl -O https://example.com/file.zip
```

### 3.4 Network Scanning

#### 3.4.1 netstat

To display network connections:

```bash
netstat -tuln
```

#### 3.4.2 ss

A more modern alternative to netstat:

```bash
ss -tuln
```

### 3.5 Firewall Management

#### 3.5.1 iptables (for older systems)

To list firewall rules:

```bash
sudo iptables -L
```

#### 3.5.2 ufw (Uncomplicated Firewall)

To manage firewall on Ubuntu-based systems:

```bash
sudo ufw status
sudo ufw allow 22
```

### 3.6 SSH Operations

#### 3.6.1 Remote Command Execution

To execute a command on a remote system:

```bash
ssh user@remote_host "command"
```

#### 3.6.2 SCP (Secure Copy)

To copy files securely between hosts:

```bash
scp local_file user@remote_host:/remote/directory
```

### 3.7 Network Monitoring

#### 3.7.1 tcpdump

To capture and analyze network traffic:

```bash
sudo tcpdump -i eth0
```

#### 3.7.2 iftop

To display bandwidth usage on an interface:

```bash
sudo iftop
```

### 3.8 Creating a Network Diagnostics Script

Here's an example script that performs basic network diagnostics:

```bash
#!/bin/bash

target="example.com"

echo "Network Diagnostics for $target"
echo "==============================="

echo "Ping Test:"
ping -c 4 $target

echo -e "\nTraceroute:"
traceroute $target

echo -e "\nDNS Lookup:"
nslookup $target

echo -e "\nCurrent Network Connections:"
netstat -tuln

echo -e "\nNetwork Interface Information:"
ip addr

echo -e "\nCurrent Bandwidth Usage:"
iftop -t -s 5
```

## Exercises

1. Create a script that displays all environment variables sorted alphabetically, along with their values.

2. Write a script that checks if a given directory is in the PATH, and if not, adds it to the PATH temporarily for the script's execution.

3. Develop a system monitoring script that periodically logs CPU usage, memory usage, and disk space to a file.

4. Create a network scanning script that checks for open ports on a given IP address or hostname.

5. Write a script that performs a series of DNS lookups for a list of domain names and reports any that fail to resolve.

6. Develop a script that monitors network traffic on a specific interface and alerts if bandwidth usage exceeds a certain threshold.

7. Create a script that automates the process of setting up SSH key-based authentication with a remote server.

## Review Questions

1. What is the difference between shell variables and environment variables? How can you convert a shell variable to an environment variable?

2. Explain the purpose of the PATH environment variable. How would you modify it to include a new directory?

3. Describe three methods to obtain the CPU usage of a Linux system using shell commands.

4. What is the purpose of the `/proc` filesystem in Linux? Give examples of how it can be used to obtain system information.

5. Explain the differences between `ping`, `traceroute`, and `nslookup`. In what situations would you use each?

6. How can you use `iptables` or `ufw` to secure a system? Provide examples of basic firewall rules.

7. Describe the process of setting up passwordless SSH authentication between two systems.

8. What are the advantages and disadvantages of using `wget` versus `curl` for downloading files?

9. How would you monitor real-time network traffic on a specific network interface using command-line tools?

10. Explain how you would write a script to automatically update system packages and reboot if necessary, ensuring it can run unattended.

This lesson covers fundamental and advanced concepts of interacting with the system through shell scripting, including working with environment variables, retrieving system information, and performing network operations. The exercises and review questions are designed to reinforce these concepts and encourage practical application of the knowledge gained.

[Next: 15. Scripting for System Administration](./15_scripting_for_system_administration.md)