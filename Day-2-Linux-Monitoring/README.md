
## Task 1: CPU & Memory Monitoring

### Commands Used
top
free -h
uptime

### Observations
- CPU usage is very low with ~94% idle CPU with most time spent idle, indicating no heavy workload.
- Load average is 0.00, indicating no CPU pressure
- Total memory is ~961 MB, with sufficient free and available memory.
- Available memory is ~681 MB, which is healthy
- No swap memory configured
- System uptime is stable at 3 days

### Conclusion
The system is stable with no CPU or memory bottlenecks. Current resource usage is within safe limits.
### Screenshot
![CPU & Memory1](https://github.com/samridh-devops/linux-disk-troubleshooting/blob/ffb20a247d84e1f81ccef3e2d5a9dd5ebe61ef5b/Day-2-Linux-Monitoring/d2a.png)

![CPU & Memory1](https://github.com/samridh-devops/linux-disk-troubleshooting/blob/ffb20a247d84e1f81ccef3e2d5a9dd5ebe61ef5b/Day-2-Linux-Monitoring/d2b.jpg)
-------------------

## Task 2: Disk & Inode Monitoring

### Commands Used
df -h
df -i
du -sh /var/log/*

### Disk Usage Observation
- Root filesystem size: 8 GB
- Used space: ~1.7 GB
- Available space: ~6.3 GB
- Disk usage: 21% (safe threshold)

### Inode Usage Observation
- Total inodes: ~4.1 million
- Used inodes: ~50K
- Inode usage: 2%
- No risk of inode exhaustion

### Log Directory Analysis
- /var/log/journal is the largest consumer (~57 MB)
- System logs are a common cause of disk full issues

### Screenshot
![Disk & Inodes](https://github.com/samridh-devops/linux-disk-troubleshooting/blob/5b69427a6ffbaffc7eafbe36aa1884a3315f1f5f/Day-2-Linux-Monitoring/d2c.jpg)
### Conclusion
Disk space and inode usage are within healthy limits. Regular monitoring of /var/log is essential to prevent disk-related outages.


----------------------------


## Task 3: Network Connectivity & Port Verification

### Commands Used
ip a  
ping google.com  
ss -tuln  

### Network Interface Check
Network interface is UP with a private IP assigned.
- Interface: enX0
- Private IP: 172.31.41.72
- State: UP
- MTU: 9001 (AWS Jumbo Frames enabled)

### Connectivity Test
Internet connectivity is confirmed with 0% packet loss to google.com.
- Successfully pinged google.com
- 0% packet loss
- Confirms DNS resolution and outbound internet access

### Port & Service Verification
- SSH service listening on port 22
- Bound to all interfaces (0.0.0.0)
- Confirms SSH availability at network level

### Screenshot
![Network Verification](https://github.com/samridh-devops/linux-disk-troubleshooting/blob/ac293ac1620853f1b96a2afdbbb0b71049a74b83/Day-2-Linux-Monitoring/d2d.jpg)
### Conclusion
Network configuration is healthy. Interface, routing, connectivity, and critical service ports are functioning correctly.

----------------
## Task 4: SSH Process & Security Verification

### Commands Used
pgrep sshd  
systemctl status sshd  

### Observations
- System processes are running normally with no abnormal CPU or memory usage.
- Multiple sshd processes observed, indicating active SSH sessions.
- SSH service is active, running, and enabled at boot.
- SSH logs show invalid user attempts (normal on public EC2 instances).
- Successful public key authentication for ec2-user confirms secure access.
### Process Verification
- Multiple sshd processes observed
- One parent daemon with child processes for active sessions
- Confirms SSH is running and handling connections correctly

### Service Status
- sshd service is active (running)
- Enabled at boot
- Running continuously without restart issues

### Log Observations
- Invalid user login attempts detected from external IPs
- Valid public key authentication for ec2-user succeeded
- Confirms secure SSH access with key-based authentication

 ### Screenshot
![SSH Status](https://github.com/samridh-devops/linux-disk-troubleshooting/blob/ac293ac1620853f1b96a2afdbbb0b71049a74b83/Day-2-Linux-Monitoring/d2e.jpg)

### Conclusion
SSH service is healthy, secure, and operating as expected. Unauthorized access attempts are blocked, and legitimate users can connect successfully.
