# Shell Scripting in the Real World

Shell scripting is a powerful tool for automating tasks and managing systems. This lesson explores real-world applications of shell scripting through case studies, integration with other programming languages, and its role in DevOps workflows.

## 1. Case Studies and Real-World Examples

### 1.1 Automating System Administration Tasks

Shell scripts are frequently used to automate routine system administration tasks, such as backups, user management, and system monitoring.

#### Example: Automated Backup Script

A common use case is automating backups of important directories:

```bash
#!/bin/bash

# Configuration
SOURCE_DIR="/home/user/data"
BACKUP_DIR="/home/user/backups"
DATE=$(date +"%Y%m%d_%H%M%S")
BACKUP_FILE="$BACKUP_DIR/backup_$DATE.tar.gz"

# Create backup
tar -czf "$BACKUP_FILE" "$SOURCE_DIR"

# Log the backup
echo "Backup of $SOURCE_DIR created at $BACKUP_FILE"
```

This script compresses the specified source directory into a timestamped backup file.

### 1.2 Log Management

Shell scripts can automate log management tasks, such as rotating logs and archiving old log files.

#### Example: Log Rotation Script

```bash
#!/bin/bash

LOG_DIR="/var/log/myapp"
ARCHIVE_DIR="/var/log/myapp/archive"
MAX_LOG_SIZE=1048576  # 1MB

# Create archive directory if it doesn't exist
mkdir -p "$ARCHIVE_DIR"

# Rotate logs
for log in "$LOG_DIR"/*.log; do
    if [ -f "$log" ] && [ $(stat -c%s "$log") -gt $MAX_LOG_SIZE ]; then
        mv "$log" "$ARCHIVE_DIR/$(basename "$log").$(date +%Y%m%d_%H%M%S)"
        echo "Rotated log: $log"
    fi
done
```

### 1.3 System Monitoring

Shell scripts can be used to monitor system performance and resource usage.

#### Example: Resource Monitoring Script

```bash
#!/bin/bash

# Check CPU usage
CPU_USAGE=$(top -bn1 | grep "Cpu(s)" | sed "s/.*, *$$[0-9.]*$$%* id.*/\1/" | awk '{print 100 - $1}')
echo "CPU Usage: $CPU_USAGE%"

# Check Disk Usage
DISK_USAGE=$(df -h / | awk 'NR==2 {print $5}')
echo "Disk Usage: $DISK_USAGE"
```

This script checks and reports CPU and disk usage, which can be scheduled to run at regular intervals.

### 1.4 Deployment Automation

Shell scripts are often used in deployment processes to automate the installation and configuration of applications.

#### Example: Deployment Script

```bash
#!/bin/bash

# Update package list and install application
sudo apt update
sudo apt install -y myapp

# Start the application service
sudo systemctl start myapp
sudo systemctl enable myapp

echo "Application deployed and started."
```

## 2. Integrating with Other Programming Languages

Shell scripts can be integrated with other programming languages to leverage their capabilities.

### 2.1 Calling Python from Bash

You can call Python scripts from within a Bash script to perform complex data processing:

```bash
#!/bin/bash

# Call a Python script
python3 process_data.py input.txt output.txt
```

### 2.2 Using Bash in Python

You can also call Bash commands from Python using the `subprocess` module:

```python
import subprocess

# Run a Bash command
result = subprocess.run(['ls', '-l'], capture_output=True, text=True)
print(result.stdout)
```

### 2.3 Combining Shell and Perl

Shell scripts can call Perl for text processing tasks:

```bash
#!/bin/bash

# Use Perl to process a file
perl -pe 's/foo/bar/g' input.txt > output.txt
```

## 3. Shell Scripting in DevOps Workflows

Shell scripting plays a vital role in DevOps practices, automating deployment, monitoring, and infrastructure management.

### 3.1 Continuous Integration/Continuous Deployment (CI/CD)

Shell scripts are commonly used in CI/CD pipelines to automate testing and deployment processes.

#### Example: CI/CD Pipeline Script

```bash
#!/bin/bash

# Run tests
./run_tests.sh

# Build the application
./build_application.sh

# Deploy to staging
./deploy_to_staging.sh
```

### 3.2 Infrastructure as Code (IaC)

Shell scripts can automate the provisioning and management of infrastructure using tools like Terraform or Ansible.

#### Example: Terraform Script

```bash
#!/bin/bash

# Initialize Terraform
terraform init

# Apply the configuration
terraform apply -auto-approve
```

### 3.3 Monitoring and Alerting

In a DevOps environment, shell scripts can be used to monitor system performance and send alerts based on predefined thresholds.

#### Example: Monitoring Script with Alerts

```bash
#!/bin/bash

THRESHOLD=80
USAGE=$(df / | awk 'NR==2 {print $5}' | sed 's/%//')

if [ "$USAGE" -gt "$THRESHOLD" ]; then
    echo "Disk usage is above threshold: ${USAGE}%"
    # Send alert (e.g., email, Slack notification)
fi
```

### 3.4 Containerization

Shell scripts are often used in Docker workflows to automate the building and deployment of containers.

#### Example: Docker Build Script

```bash
#!/bin/bash

# Build the Docker image
docker build -t myapp:latest .

# Run the Docker container
docker run -d -p 80:80 myapp:latest
```

## Exercises

1. Write a shell script that automates the process of backing up a directory to a remote server using `rsync`.

2. Create a log management script that archives logs older than 30 days and compresses them.

3. Develop a resource monitoring script that logs CPU and memory usage to a file every minute.

4. Write a deployment script that installs a web server, configures it, and starts the service.

5. Create a CI/CD pipeline script that includes steps for testing, building, and deploying a sample application.

6. Implement a shell script that integrates with a Python script to process data from a CSV file and outputs the results.

7. Write a shell script that monitors disk usage and sends an alert email when usage exceeds a specified threshold.

## Review Questions

1. What are the benefits of using shell scripts for automating system administration tasks?

2. Describe how you would structure a shell script project for managing backups and logging.

3. How can you integrate shell scripts with other programming languages to enhance functionality?

4. What role does shell scripting play in DevOps practices, particularly in CI/CD workflows?

5. Explain how you would implement error handling and logging in a shell script designed for deployment automation.

6. Discuss the importance of version control in managing shell scripts, especially in collaborative environments.

7. How can shell scripts be used to facilitate infrastructure as code (IaC) practices?

8. What are the common pitfalls to avoid when writing shell scripts for production environments?

9. Describe a scenario where using a shell script in a CI/CD pipeline would be beneficial.

10. How can you ensure that your shell scripts are secure, especially when handling sensitive data or performing system-level operations?

This lesson covers practical applications of shell scripting in real-world scenarios, including automation, integration with other languages, and its role in DevOps workflows. The exercises and review questions are designed to reinforce these concepts and encourage practical application of the knowledge gained.

[Next: 22. Further Learning and Resources](./22_further_learning_and_resources.md)