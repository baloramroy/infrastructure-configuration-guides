
## Troubleshooting Nessus Aget Link Error Problem.

- If agent shows `Not linked` or 
- Error like this `[error] [agent] Link fail: [409] An agent with the uuid '38c62e2c-4bb4-402c-b3c5-453a1bdfdbf1' already exists.`

### Step 1 — First Check Current Agent Status

**Run:**

```
/opt/nessus_agent/sbin/nessuscli agent status
```

**If it shows:**

```
Linked to: 10.50.80.14
```

Then nothing is wrong — it is already linked.

**If it shows:**

```
Not linked
```

Then continue with reset procedure.

#

### Step 2 — Proper Fix

You need to unlink and reset the agent UUID.

Stop the service

```bash
systemctl stop nessusagent
```

Unlink the agent locally

```bash
/opt/nessus_agent/sbin/nessuscli agent unlink
```

If unlink fails, continue anyway.

#

### 3. Reset the Agent ID

```bash
/opt/nessus_agent/sbin/nessuscli prepare-image
```

This command:
- Removes old UUID
- Clears previous linkage
- Prepares the agent as a fresh installation

This is especially required in cloned VMs

#

### 4. Start service again

```bash
systemctl start nessusagent
```

#

### 5. Link again

```bash
/opt/nessus_agent/sbin/nessuscli agent link \
  --key=YOUR_KEY \
  --host=10.10.10.14 \
  --port=1034
```

---

## Alternative Fix (If Manager Already Has This Agent)

Sometimes the UUID exists in the **manager** database.

Then you must:

1. Login to Nessus Manager Web UI
2. Go to Agents
3. Find agent with UUID:

   ```text
   38c62e2c-4bb4-402c-b3c5-453a1bdfdbf1
   ```
4. Delete that agent entry
5. Then link again from server

---

