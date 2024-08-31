# Scripting for System Administration

System administration often involves repetitive tasks that can be automated using shell scripts. This lesson covers automating common tasks, user and group management, and system monitoring and maintenance.

## 1. Automating Common Tasks

### 1.1 Backup Scripts

Creating regular backups is crucial for system administration. Here's a simple backup script:

```bash
#!/bin/bash

# Configuration
source_dir="/path/to/source"
backup_dir="/path/to/backup"
date_format=$(date +"%Y%m%d_%H%M%S")
backup_filename="backup_$date_format.tar.gz"

# Create backup
tar -czf "$backup_dir/$backup_filename" "$source_dir"

# Verify backup
if [ $? -eq 0 ]; then
    echo "Backup created successfully: $backup_filename"
else
    echo "Backup failed"
    exit 1
fi

# Remove old backups (keep last 5)
cd "$backup_dir" || exit
ls -t | tail -n +6 | xargs -I {} rm -- {}
```

### 1.2 Log Rotation

Log rotation helps manage log files efficiently:

```bash
#!/bin/bash

log_dir="/var/log"
max_log_size=10M
num_to_keep=5

for log_file in "$log_dir"/*.log; do
    if [ -f "$log_file" ] && [ $(stat -c%s "$log_file") -gt $(numfmt --from=iec $max_log_size) ]; then
        for i in $(seq $((num_to_keep-1)) -1 1); do
            if [ -f "${log_file}.$i" ]; then
                mv "${log_file}.$i" "${log_file}.$((i+1))"
            fi
        done
        mv "$log_file" "${log_file}.1"
        touch "$log_file"
        gzip "${log_file}.1"
    fi
done
```

### 1.3 System Updates

Automating system updates ensures your system stays current:

```bash
#!/bin/bash

# Update package lists
sudo apt update

# Upgrade packages
sudo apt upgrade -y

# Remove unnecessary packages
sudo apt autoremove -y

# Clean up
sudo apt clean

echo "System update completed."
```

### 1.4 Service Management

Monitoring and managing services is a common task:

```bash
#!/bin/bash

service_name="apache2"

check_service() {
    if systemctl is-active --quiet "$service_name"; then
        echo "$service_name is running"
    else
        echo "$service_name is not running"
        echo "Attempting to start $service_name..."
        sudo systemctl start "$service_name"
        if systemctl is-active --quiet "$service_name"; then
            echo "$service_name started successfully"
        else
            echo "Failed to start $service_name"
        fi
    fi
}

check_service
```

## 2. User and Group Management

### 2.1 Creating Users

Script to create a new user with a home directory:

```bash
#!/bin/bash

read -p "Enter username: " username
read -s -p "Enter password: " password
echo

# Create user
sudo useradd -m -s /bin/bash "$username"

# Set password
echo "$username:$password" | sudo chpasswd

# Verify user creation
if id "$username" >/dev/null 2>&1; then
    echo "User $username created successfully"
else
    echo "Failed to create user $username"
    exit 1
fi
```

### 2.2 Managing Groups

Script to add a user to multiple groups:

```bash
#!/bin/bash

read -p "Enter username: " username
read -p "Enter groups (space-separated): " groups

for group in $groups; do
    sudo usermod -a -G "$group" "$username"
    if [ $? -eq 0 ]; then
        echo "Added $username to group $group"
    else
        echo "Failed to add $username to group $group"
    fi
done
```

### 2.3 Bulk User Management

Script to create multiple users from a CSV file:

```bash
#!/bin/bash

input_file="users.csv"

while IFS=',' read -r username password groups; do
    # Create user
    sudo useradd -m -s /bin/bash "$username"
    echo "$username:$password" | sudo chpasswd

    # Add to groups
    IFS=' ' read -ra group_array <<< "$groups"
    for group in "${group_array[@]}"; do
        sudo usermod -a -G "$group" "$username"
    done

    echo "User $username created and added to groups: $groups"
done < "$input_file"
```

### 2.4 User Expiration

Script to set user account expiration:

```bash
#!/bin/bash

read -p "Enter username: " username
read -p "Enter expiration date (YYYY-MM-DD): " expiration_date

sudo usermod -e "$expiration_date" "$username"

if [ $? -eq 0 ]; then
    echo "Expiration date for $username set to $expiration_date"
else
    echo "Failed to set expiration date for $username"
fi
```

## 3. System Monitoring and Maintenance

### 3.1 Disk Space Monitoring

Script to monitor disk space and send alerts:

```bash
#!/bin/bash

threshold=90
email="admin@example.com"

df -h | awk '{print $5 " " $6}' | while read output;
do
    usage=$(echo $output | awk '{print $1}' | cut -d'%' -f1)
    partition=$(echo $output | awk '{print $2}')
    if [ $usage -ge $threshold ]; then
        echo "ALERT: Partition $partition is ${usage}% full" | mail -s "Disk Space Alert" $email
    fi
done
```

### 3.2 System Load Monitoring

Script to monitor system load average:

```bash
#!/bin/bash

threshold=4
log_file="/var/log/load_monitor.log"

while true; do
    load=$(uptime | awk '{print $10}' | cut -d',' -f1)
    if (( $(echo "$load > $threshold" | bc -l) )); then
        echo "$(date): High load average detected: $load" >> "$log_file"
        # Add actions here (e.g., email alert, kill processes)
    fi
    sleep 60
done
```

### 3.3 Log Analysis

Script to analyze Apache access logs:

```bash
#!/bin/bash

log_file="/var/log/apache2/access.log"

echo "Top 10 IP addresses:"
awk '{print $1}' "$log_file" | sort | uniq -c | sort -nr | head -n 10

echo -e "\nTop 10 requested pages:"
awk '{print $7}' "$log_file" | sort | uniq -c | sort -nr | head -n 10

echo -e "\n404 errors:"
grep "HTTP/1.1\" 404" "$log_file" | awk '{print $7}' | sort | uniq -c | sort -nr
```

### 3.4 Automated System Cleanup

Script to clean up temporary files and old logs:

```bash
#!/bin/bash

# Clean /tmp directory
find /tmp -type f -atime +7 -delete

# Clean old log files
find /var/log -type f -name "*.log" -mtime +30 -delete

# Clean user cache
find /home/*/.cache -type f -atime +30 -delete

# Clean apt cache
sudo apt-get clean

echo "System cleanup completed."
```

### 3.5 Security Auditing

Script to perform basic security checks:

```bash
#!/bin/bash

echo "Checking for failed login attempts:"
grep "Failed password" /var/log/auth.log | tail -n 5

echo -e "\nChecking for open ports:"
netstat -tuln

echo -e "\nChecking for processes running as root:"
ps -ef | grep root

echo -e "\nChecking for users with empty passwords:"
sudo cat /etc/shadow | awk -F: '($2==""){print $1}'

echo -e "\nChecking for world-writable files:"
find / -xdev -type f -perm -0002 -ls 2>/dev/null
```

## Exercises

1. Create a comprehensive backup script that includes incremental backups and remote storage options.

2. Develop a user management script that allows for creating, modifying, and deleting users, as well as managing their group memberships and permissions.

3. Write a system health monitoring script that checks CPU usage, memory usage, disk space, and network connectivity, and sends alerts when predefined thresholds are exceeded.

4. Create a log analysis script that parses multiple log files, identifies patterns or anomalies, and generates a summary report.

5. Develop a script to automate the process of setting up a new server, including software installation, user creation, and basic security configurations.

6. Write a script that performs a security audit of the system, checking for common vulnerabilities and misconfigurations, and provides recommendations for improvements.

7. Create a maintenance script that automates routine tasks such as updating software, cleaning up temporary files, optimizing databases, and verifying system integrity.

## Review Questions

1. What are the key considerations when writing a backup script for a production environment?

2. Explain the difference between useradd and adduser commands. In what scenarios would you use each?

3. How can you ensure that system monitoring scripts run continuously without consuming excessive resources?

4. What are some best practices for securely managing passwords when creating new user accounts through scripts?

5. Describe the process of setting up a cron job to run a system maintenance script periodically.

6. How would you modify the log rotation script to handle logs with different retention periods based on their content or importance?

7. What security precautions should be taken when writing scripts that perform system administration tasks with elevated privileges?

8. Explain how you would implement error handling and logging in a complex system administration script to ensure reliability and traceability.

9. How can you use shell scripting to automate the process of creating and managing virtual hosts in a web server environment?

10. Describe a strategy for managing and deploying system administration scripts across multiple servers in a network.

This lesson covers fundamental and advanced concepts of scripting for system administration, including task automation, user and group management, and system monitoring and maintenance. The exercises and review questions are designed to reinforce these concepts and encourage practical application of the knowledge gained in real-world system administration scenarios.

[Next: 16. Version Control with Git](./16_version_control_with_git.md)