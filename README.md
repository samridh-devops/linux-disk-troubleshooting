Linux Troubleshooting Lab – Day 1        #
#################################################################

Prepared by: Samridh Gupta
Date: 30-Jan-2026
Environment: AWS EC2 (Amazon Linux)

-----------------------------------------------------------------
Objective
-----------------------------------------------------------------
The goal of this lab is to simulate real-world Linux incidents and
troubleshoot them like a DevOps engineer.

By the end of the lab, you will be able to:
  - Diagnose disk full issues
  - Diagnose service down issues
  - Analyze application logs
  - Understand and fix file permission problems

-----------------------------------------------------------------
Environment
-----------------------------------------------------------------
  - AWS EC2 Instance (Amazon Linux)
  - Root filesystem: /dev/xvda1
  - Services used: sshd (SSH daemon)
  - Init system: systemd
  - Terminal commands: df, systemctl, journalctl, ls, chmod, cat, tail, grep

-----------------------------------------------------------------
Task 1 – Disk Full Incident
-----------------------------------------------------------------

Step 1: Check Disk Usage
Command:
  df -h
Explanation:
  - df -h shows disk usage of all partitions in human-readable format.
  - Observed /dev/xvda1 had 1.6G used of 8G (19%).

Step 2: Fill the Disk (Simulate Failure)
Commands:
  sudo mkdir /var/tmp/fill
  sudo fallocate -l 2G /var/tmp/fill/bigfile
  df -h
Explanation:
  - We created a big file to simulate a disk full situation.
  - After creating 2GB file, /dev/xvda1 usage increased from 19% → 45%.
  - This simulates real production risk where log files or apps fill the disk.

Step 3: Observe Impact
  - Partition filled: /dev/xvda1
  - % usage increase: 26%
  - Potential failures in production:
      1. Application logs stop writing
      2. Services fail to start/restart
      3. CI/CD deployments fail
      4. Package manager operations fail

Step 4: Recovery
Commands:
  sudo rm -rf /var/tmp/fill
  df -h
Explanation:
  - Removed the big file to free space
  - Usage reverted back to 19%
  - Disk is stable, services can continue running

-----------------------------------------------------------------
Task 2 – SSH Service Down
-----------------------------------------------------------------

Step 1: Check SSH Service
Command:
  systemctl status sshd
Explanation:
  - Checks the status of SSH daemon.
  - Status observed: active (running)
  - ![image alt](https://github.com/samridh-devops/linux-disk-troubleshooting/blob/959e9365e34758f7a2c1684213b165d93c227862/images/t1.png)

Step 2: Simulate Service Failure
Command:
  sudo systemctl stop sshd
Explanation:
  - Stops SSH service to simulate production outage
  - Useful for testing monitoring and recovery procedures

Step 3: Verify Failure
Command:
  systemctl status sshd
Observation:
  - Status changed to inactive (dead)
  - Shows service is not running

Step 4: Restore Service
Command:
  sudo systemctl start sshd
  systemctl status sshd
Explanation:
  - Starts SSH service back
  - Status: active (running) → Service recovered

Step 5: Log Analysis
Command:
  journalctl -u sshd --no-pager | tail -20
Explanation:
  - Shows last 20 log lines for SSH service
  - Helps identify causes of failure

-----------------------------------------------------------------
Task 3 – Log Analysis
-----------------------------------------------------------------

Step 1: Create Fake App Logs
Commands:
  mkdir ~/logs
  cd ~/logs
  touch app.log

Step 2: Write Logs
Commands:
  echo "INFO: App started" >> app.log
  echo "INFO: DB connected" >> app.log
  echo "ERROR: DB timeout" >> app.log

Step 3: Read Logs
Commands:
  cat app.log
  tail app.log
  grep ERROR app.log
Explanation:
  - Use cat to view all logs
  - Use tail to see last entries
  - Use grep ERROR to filter critical errors
  - This simulates real DevOps log troubleshooting

-----------------------------------------------------------------
Task 4 – File Permissions
-----------------------------------------------------------------

Step 1: Check File Permissions
Command:
  ls -l app.log

Step 2: Break Permissions
Command:
  chmod 000 app.log
  cat app.log
Observation:
  - You cannot read the file → permission denied

Step 3: Restore Permissions
Command:
  chmod 644 app.log
  cat app.log
Explanation:
  - Restores normal read/write permissions
  - Demonstrates impact of permissions on application

#################################################################
#                          End of Lab                          #
#################################################################
