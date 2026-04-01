# Standard Operating Procedure (SOP)

## Installation and Configuration of Nessus Agent 11.1.1 (EL8)

---

## 1. Purpose

To define the standard process for installing, configuring, and verifying the Nessus Agent version 11.1.1 on Enterprise Linux 8 systems.

---

## 2. Scope

This SOP applies to:

- RHEL 8
- CentOS 8
- Rocky Linux 8
- AlmaLinux 8

Systems that require vulnerability scanning via centralized management.

---

## 3. Prerequisites

Before installation, ensure:

1. Root or sudo access is available.
2. Internet connectivity (if linking to Tenable.io).
3. Manager IP / FQDN and Linking Key are available.
4. Port 8834 is open between agent and manager.
5. The installation package is downloaded:

   ```
   NessusAgent-11.1.1-el8.x86_64.rpm
   ```

---

## 4. Installation Procedure

### Download Paackage from the Official Site

```
https://www.tenable.com/downloads/nessus-agents?loginAttempted=true
```

### Move to Download Directory

```bash
cd /tmp
```

(Adjust path if RPM is stored elsewhere.)

---

### Install Nessus Agent

Using yum:

```
yum install -y NessusAgent-11.1.1-el8.x86_64.rpm
```

OR using dnf:

```
dnf install -y NessusAgent-11.1.1-el8.x86_64.rpm
```

Verify installation:

```
rpm -qa | grep NessusAgent
```

---

### Start and Verify Service

Start Nessus Agent Service

```bash
systemctl start nessusagent
```

Enable Service at Boot

```bash
systemctl enable nessusagent
```


Verify Service Status

```bash
systemctl status nessusagent
```

Expected Result: Service should be **active (running)**.

---

## 5. Agent Linking Procedure

After installation, the agent must be linked to the Nessus Manager.

Run:

```text
/opt/nessus_agent/sbin/nessuscli agent link \
  --key=YOUR_LINKING_KEY \
  --host=MANAGER_IP \
  --port=8834
```

Example:

```bash
/opt/nessus_agent/sbin/nessuscli agent link \
  --key=0be22a3ac329b0497835cb59717be338e3e03cd15471c0c4947fe8eaef7b9ad9 \
  --host=10.50.80.14 \
  --port=8834
```

---

### Verify Agent Status

```bash
/opt/nessus_agent/sbin/nessuscli agent status
```

Expected Result:
Agent should show **Linked to manager**.

---

## 6. Firewall Validation

Check if port 8834 is reachable:

```bash
telnet MANAGER_IP 8834
```

If firewall is enabled:

```bash
firewall-cmd --list-ports
```

Open port if required:

```bash
firewall-cmd --add-port=8834/tcp --permanent
firewall-cmd --reload
```

---

## 7. Troubleshooting

If agent shows "Not linked":

1. Verify linking key.
2. Confirm manager IP/FQDN.
3. Check network connectivity.
4. Check firewall rules.
5. Restart agent:

```bash
systemctl restart nessusagent
```

---

## 8. Rollback Procedure

Stop service:

```bash
systemctl stop nessusagent
```

Remove package:

```bash
dnf remove NessusAgent -y
```

---

