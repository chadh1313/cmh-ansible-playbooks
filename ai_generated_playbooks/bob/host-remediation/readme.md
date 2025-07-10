# Linux Host Remediation Playbook

This Ansible playbook provides automated remediation for common issues on Linux hosts. It's designed to identify and fix various system problems that can affect performance, stability, and availability.

## Features

The playbook addresses the following common Linux host issues:

- **Disk Space Management**
  - Identifies filesystems with high usage
  - Cleans package manager caches
  - Removes old log files
  - Truncates large log files
  - Cleans temporary files

- **iNode Usage Remediation**
  - Identifies filesystems with high inode usage
  - Finds directories with many small files

- **Memory Usage Remediation**
  - Identifies memory-intensive processes
  - Clears page cache when memory usage is high

- **CPU Usage Remediation**
  - Identifies CPU-intensive processes
  - Detects runaway processes

- **Service Management**
  - Checks status of critical services
  - Automatically restarts failed services

- **Zombie Process Cleanup**
  - Detects zombie processes
  - Sends SIGCHLD to parent processes to clean up zombies

- **Network Connectivity Check**
  - Verifies network interface configuration
  - Checks default gateway
  - Tests DNS resolution
  - Tests connectivity to external targets

- **System Logs Check**
  - Scans for errors in system logs
  - Checks journal logs for critical issues

## Usage

### Basic Usage

```bash
ansible-playbook -i inventory hostRemediation.yaml
```

### Targeting Specific Hosts

```bash
ansible-playbook -i inventory hostRemediation.yaml -e "target_hosts=webservers"
```

### Customizing Thresholds

You can override default thresholds defined in `vars.yaml` by passing them as extra variables:

```bash
ansible-playbook -i inventory hostRemediation.yaml -e "disk_usage_threshold=90 memory_usage_threshold=95"
```

## Configuration

All configurable parameters are defined in `vars.yaml`. You can modify these values to suit your environment:

- `disk_usage_threshold`: Percentage threshold for disk usage alerts
- `memory_usage_threshold`: Percentage threshold for memory usage alerts
- `cpu_usage_threshold`: Percentage threshold for CPU usage alerts
- `inode_usage_threshold`: Percentage threshold for inode usage alerts
- `log_max_size_mb`: Maximum size for log files in MB
- `service_list`: List of critical services to monitor
- `log_paths`: Directories to scan for log files
- `log_retention_days`: Number of days to keep log files
- `network_test_targets`: IP addresses to test network connectivity
- `max_zombie_threshold`: Maximum allowed zombie processes before remediation
- `temp_file_paths`: Directories to scan for temporary files
- `temp_file_age_days`: Age threshold for temporary file cleanup
- `runaway_process_cpu_threshold`: CPU percentage to identify runaway processes
- `memory_hog_threshold`: Memory percentage to identify memory-intensive processes

## Output

The playbook provides detailed output for each remediation step and a summary at the end showing:

- Issues detected
- Actions taken
- Current system status

## Requirements

- Ansible 2.9 or higher
- Root or sudo access on target hosts
- Basic Linux commands (ps, top, df, etc.) on target hosts

// Made with Bob
