# Linux Disk Full Incident – Hands-on Lab

## Objective
Simulate a real-world disk full incident on a Linux server and perform recovery.

---

## Environment
- AWS EC2 (Amazon Linux)
- Root filesystem: /dev/xvda1

---

## Step 1: Check Disk Usage
df -h

Output:
/dev/xvda1  8.0G  1.6G  6.5G  19% /

---

## Step 2: Simulate Disk Full
sudo mkdir /var/tmp/fill
sudo fallocate -l 2G /var/tmp/fill/bigfile
df -h

Result:
/dev/xvda1  8.0G  3.6G  4.5G  45% /

---

## Step 3: Impact Analysis
- Root filesystem filled
- Log writes may fail
- Services may not restart
- CI/CD deployments can break
- Package manager operations fail

---

## Step 4: Troubleshooting
du -sh /var/*
lsof | grep deleted

---

## Step 5: Recovery
sudo rm -rf /var/tmp/fill
df -h

Result:
/dev/xvda1  8.0G  1.6G  6.5G  19% /

---

## Learnings
- Disk issues impact multiple services
- df shows where, du shows what
- Deleted files may still consume space if held by processes

