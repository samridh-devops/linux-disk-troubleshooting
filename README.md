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

