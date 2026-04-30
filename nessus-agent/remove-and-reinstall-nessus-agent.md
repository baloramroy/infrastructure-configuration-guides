# SOP: Clean Uninstall + validation + reinstall of Nessus agent.

## PART 1: Stop and Remove Nessus Agent

### Stop the service

```bash
systemctl stop nessusagent
```

### Disable service (avoid auto-start leftovers)

```bash
systemctl disable nessusagent
```


### 3. Remove package

```bash
rpm -e NessusAgent
```

---

## PART 2: Remove ALL leftover files

Nessus leaves data even after uninstall.

#

### Remove installation directories

```bash
rm -rf /opt/nessus_agent
rm -rf /opt/nessus
```

### Remove config & temp leftovers

```bash
rm -rf /etc/nessus
rm -rf /var/tmp/nessus*
rm -rf /var/lib/nessus
```

### 6. Remove systemd leftovers (if exists)

```bash
rm -f /etc/systemd/system/nessusagent.service
systemctl daemon-reload
```

---

## PART 3: VALIDATE CLEAN REMOVAL

This is where most people make mistakes — **you must verify**

#

### Check binary is gone

```bash
which nessuscli
or
command -v nessuscli
```

👉 Expected: **no output**

### Check for leftover file

```bash
find / -iname "*nessus*" 2>/dev/null
```

### Check directory is gone

```bash
ls -ld /opt/nessus_agent
```

👉 Expected:

```bash
No such file or directory
```


### Check package removed

```bash
rpm -qa | grep -i nessus
```

👉 Expected: **no output**


### Check service removed

```bash
systemctl status nessusagent
```

👉 Expected:

```bash
Unit nessusagent.service could not be found
```


### Check no running process

```bash
ps -ef | grep -i nessus
```

👉 Only grep line should appear

 
---

## PART 4: Reinstall Nessus Agent

If ALL above are clean → system is safe for reinstall

#

### 1. Install RPM

```bash
dnf install NessusAgent-<version>.rpm
```


### 2. Start service

```bash
systemctl start nessusagent
```


### 3. Enable service

```bash
systemctl enable nessusagent
```

---

## Link Agent

```bash
/opt/nessus_agent/sbin/nessuscli agent link \
  --key=YOUR_KEY \
  --host=10.10.10.14 \
  --port=8834
```

---

## FINAL VALIDATION

### Check status

```bash
/opt/nessus_agent/sbin/nessuscli agent status
```

### Healthy output should be:

```bash
Running: Yes
Plugins loaded: Yes
Link status: Connected
Last successful connection: few seconds ago
```

---

## Pro Tips

1. **Wait for plugin download**

   * First time can take **5–15 minutes**

2. **Monitor logs**

    ```bash
    tail -f /opt/nessus_agent/var/nessus/logs/nessusd.messages
    ```

---

## Quick Checklist

- Uninstall → Remove `/opt/nessus_agent`
- Validate → no rpm, no service, no binary
- Reinstall → Start → Link → Verify plugins

---
