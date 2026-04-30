## Check Agent Status

### If you run this to check agent status:

```bash
/opt/nessus_agent/sbin/nessuscli agent status
```

### What a Healthy Agent Should Show
- Plugins loaded: Yes
- Last successful connection: few seconds/minutes ago
- Last scanned: some timestamp

> But if you didn't get this parameter correctly then you have to analyse the log for better preview

## See below logs to check runtime issues

### You MUST check this:

```bash
tail -f /opt/nessus_agent/var/nessus/logs/nessusd.messages
OR
tail -f /opt/nessus_agent/var/nessus/logs/backend.log
```

### What We Are Looking For in the logs

In those logs, check for:

**A. Plugin download issues**

Examples:
```text
plugin download failed
feed update failed
```

**B. SSL / certificate issues**

Examples:
```text
SSL_connect failed
certificate verify failed
```

**C. Connectivity issues**

Examples:

```text
connection refused
timeout
```

>Take meassures according to the issue.

---

## Root Cause Identification

### From the `backend.log`:

```text
[error] [globaldb] Can't find the downgrader:
/opt/nessus_agent/var/nessus/tools/downgrader.11.1.2.ntool
```


### What this actually means

Your Nessus Agent is trying to:

- Downgrade internal database (globalDB)
- From version **11.1.2 → 11.0.4**
- But required downgrade file is **missing**

👉 Result:

* Agent initialization is **broken**
* Plugin system never loads
* Agent never becomes “active”
* Manager shows **OFFLINE**



### Why this happens

This is a **version mismatch / corrupted installation scenario**

Common reasons:

1. Agent version ≠ Manager version
2. Partial upgrade/downgrade
3. Corrupted plugin/db files
4. Old data left after reinstall


## Important Observation

You have:

• `Plugins loaded: No`
• `globaldb downgrade error`
• Agent “running” but not functional

👉 This confirms:
**Agent is stuck during startup (DB handling stage)**

---

## Correct Fix


1. Need to **clean reset of the agent state** or
2. Need to **reinstall the agent completely**

## Clean reset of the agent state

Step 1: Stop agent

```bash
systemctl stop nessusagent
```

Step 2: Clean corrupted data

⚠️ This is the most important step

```bash
rm -rf /opt/nessus_agent/var/nessus/*
```


Step 3: Start agent again

```bash
systemctl start nessusagent
```


Step 4: Re-link agent

```bash
/opt/nessus_agent/sbin/nessuscli agent link \
  --key=YOUR_KEY \
  --host=10.50.80.14 \
  --port=8834
```

Step 5: Monitor logs

```bash
tail -f /opt/nessus_agent/var/nessus/logs/nessusd.messages
```

#

**Expected Result After Fix**

After 2–5 minutes:

```id
Plugins loaded: Yes
Last successful connection: few seconds ago
```

> Manager will show: 👉 **ONLINE**

But if you found error like this

```
Error: Can not find /opt/nessus_agent/var/nessus/tools/tool_dispatch.ntool
```

- As we run this command `rm -rf /opt/nessus_agent/var/nessus/*`
- Agent installation is now **broken/incomplete**
- Normally agent **recreates** them — but here it didn’t → means install is **inconsistent**
- So now follow the second option to fix

---

## Reinstall the agent completely

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





