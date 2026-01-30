nux Troubleshooting Labs — Day 1

Objective:
By the end of this lab, you will be able to:

Diagnose a disk full issue

Handle a service down scenario

Analyze application logs

Troubleshoot file permission issues

Tools Allowed: Terminal, your brain
Google: ❌ Not allowed until the end

Task 1: Disk Full – Incident Simulation

Objective: Simulate a real-world disk full incident on a Linux server and perform recovery.

Environment:

AWS EC2 (Amazon Linux / Ubuntu)

Root filesystem: /dev/xvda1

Steps

1. Check Disk Usage

df -h


Example output:

/dev/xvda1  8.0G  1.6G  6.5G  19% /


2. Fill the Disk (Break It)

sudo mkdir /var/tmp/fill
sudo fallocate -l 2G /var/tmp/fill/bigfile
df -h


Observation: /dev/xvda1 usage increased from 19% → 45%

3. Recovery

sudo rm -rf /var/tmp/fill
df -h


Disk space restored.

4. Key Learnings

Root filesystem fills first → app logs fail, services may crash

Monitor /var/log for errors

Disk full affects deployments, updates, SSH logins

Task 2: SSH Service Down – Incident Simulation

Objective: Simulate a real-world SSH service outage and restore it safely.

Environment:

AWS EC2 (Amazon Linux)

Service: sshd

Init system: systemd

Steps

1. Check Service Status

systemctl status sshd


Status observed: active (running)

2. Simulate Service Failure

sudo systemctl stop sshd


SSH service stopped to simulate a production outage.

3. Verify Failure

systemctl status sshd


Status changed to: inactive (dead)

4. Restore Service

sudo systemctl start sshd
systemctl status sshd


SSH service successfully restored.

5. Log Analysis

journalctl -u sshd --no-pager | tail -20


Observation: Check recent SSH logs to ensure the service is restored correctly.

Task 3: Log Analysis – Practical Lab

Objective: Analyze application logs like a real DevOps engineer.

Environment:

AWS EC2 (Amazon Linux)

Tools: mkdir, touch, echo, cat, tail, grep

Sample log file: app.log

Steps

1. Create a Fake App Log

mkdir ~/logs
cd ~/logs
touch app.log


2. Write Logs

echo "INFO: App started" >> app.log
echo "INFO: DB connected" >> app.log
echo "ERROR: DB timeout" >> app.log


3. Read Logs Like a DevOps Engineer

cat app.log       # full log
tail app.log      # last entries quickly
grep ERROR app.log # only errors


4. Key Learnings

Logs are critical for troubleshooting

tail -f allows real-time monitoring

grep ERROR speeds up issue detection

Proper log analysis helps detect and fix issues fast

Task 4: File Permissions – Practical Lab

Objective: Understand Linux file permissions and troubleshoot access issues.

Environment:

AWS EC2 (Amazon Linux)

Sample log file: app.log

Steps

1. Check File Permissions

ls -l app.log


2. Restrict Access

chmod 000 app.log
cat app.log


Observation: Permission denied error

3. Restore Access

chmod 644 app.log
cat app.log


File readable again.

4. Key Learnings

chmod controls file access

Incorrect permissions break applications

Always check permissions when debugging file access issues
