## Task 2: SSH Service Down – Incident Simulation

### Objective
Simulate a real-world SSH service outage on an Amazon Linux EC2 instance and restore it safely.

---

### Environment
- AWS EC2 (Amazon Linux)
- Service: sshd
- Init system: systemd

---

### Step 1: Check Service Status
systemctl status sshd

Status observed: active (running)

### Step 2: Simulate Service Failure
sudo systemctl stop sshd

SSH service stopped to simulate production outage.

### Step 3: Verify Failure
systemctl status sshd

Status changed to: inactive (dead)

### Step 4: Restore Service
sudo systemctl start sshd
systemctl status sshd

SSH service successfully restored.

### Step 5: Log Analysis
journalctl -u sshd --no-pager | tail -20

_______________________________________________________________________________________________________________________

Task 3: Log Analysis – Practical Lab
Objective

Simulate and analyze application logs like a real DevOps engineer.

Environment

AWS EC2 (Amazon Linux)

Tools: mkdir, touch, echo, cat, tail, grep

Sample log file: app.log

###Step 1: Create a Fake App Log
mkdir ~/logs
cd ~/logs
touch app.log

Step 2: Write Logs
echo "INFO: App started" >> app.log
echo "INFO: DB connected" >> app.log
echo "ERROR: DB timeout" >> app.log

Step 3: Read Logs Like a DevOps Engineer
cat app.log
tail app.log
grep ERROR app.log


Observation:

cat → full log

tail → last entries quickly

grep ERROR → only errors

Step 4: Key Learnings

Logs are critical for troubleshooting

tail -f allows real-time monitoring

grep ERROR speeds up issue detection

Proper log analysis helps detect and fix issues fast
