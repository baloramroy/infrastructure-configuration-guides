Standard Operating Procedure (SOP)

> **Important:** Depending on your organization's CrowdStrike policy, **Sensor Tamper Protection (Maintenance Token)** may be enabled. If it is enabled, you **must obtain a valid maintenance token from the CrowdStrike Falcon Console** before uninstalling or modifying the sensor. Without it, the uninstall will fail.

---



# CrowdStrike Falcon Sensor Uninstallation and Reinstallation on RHEL 9

## Objective

This SOP describes the procedure to safely:

* Uninstall the CrowdStrike Falcon Sensor
* Verify complete removal
* Reinstall the sensor
* Register the sensor with the Falcon Cloud
* Validate successful operation

---

## Prerequisites

Before beginning, ensure the following are available:

* Root or sudo privileges
* Falcon Sensor RPM package
* CrowdStrike Customer ID (CID)
* Maintenance Token (if Sensor Tamper Protection is enabled)
* Internet connectivity (or configured proxy)
* Scheduled maintenance window (recommended for production servers)

---

# Part 1 – Verify Current Sensor Status

## Step 1 – Check Sensor Service

```bash
systemctl status falcon-sensor
```

Expected:

```
Active: active (running)
```

---

## Step 2 – Check Installed Package

```bash
rpm -qa | grep falcon
```

Example:

```
falcon-sensor-7.x.x-xxxx.el9.x86_64
```

---

## Step 3 – Verify CID

```bash
/opt/CrowdStrike/falconctl -g --cid
```

---

## Step 4 – Verify Agent ID (AID)

```bash
/opt/CrowdStrike/falconctl -g --aid
```

Record the AID if you need it for comparison or auditing.

---

# Part 2 – If Tamper Protection Is Enabled

Determine whether your environment requires a maintenance token.

If required, set the token before uninstalling:

```bash
/opt/CrowdStrike/falconctl -s --maintenance-token=<TOKEN>
```

Verify that the command completes successfully.

> If Sensor Tamper Protection is not enabled, skip this step.

---

# Part 3 – Stop the Sensor

Stop the service gracefully:

```bash
systemctl stop falcon-sensor
```

Verify:

```bash
systemctl status falcon-sensor
```

Expected:

```
inactive (dead)
```

---

# Part 4 – Disable Automatic Startup

```bash
systemctl disable falcon-sensor
```

Verify:

```bash
systemctl is-enabled falcon-sensor
```

Expected:

```
disabled
```

---

# Part 5 – Uninstall the Package

Using DNF (recommended):

```bash
dnf remove -y falcon-sensor
```

Alternatively:

```bash
rpm -e falcon-sensor
```

---

# Part 6 – Verify Removal

## Verify Package

```bash
rpm -qa | grep falcon
```

Expected:

```
(no output)
```

---

## Verify Service

```bash
systemctl status falcon-sensor
```

Expected:

```
Unit falcon-sensor.service could not be found.
```

---

## Verify Installation Directory

```bash
ls -ld /opt/CrowdStrike
```

If the directory remains, verify whether it contains any files:

```bash
ls -la /opt/CrowdStrike
```

> **Note:** Do **not** manually delete remaining files unless instructed by CrowdStrike Support or your organization's security team. Some files may be retained intentionally for diagnostics or future installations.

---

# Part 7 – Install the New Sensor

Install the RPM:

```bash
dnf install -y /tmp/falcon-sensor-*.rpm
```

Verify installation:

```bash
rpm -qa | grep falcon
```

---

# Part 8 – Configure Customer ID

Configure the sensor:

```bash
/opt/CrowdStrike/falconctl -s --cid=<YOUR_CID>
```

Verify:

```bash
/opt/CrowdStrike/falconctl -g --cid
```

Expected:

```
cid="xxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```

---

# Part 9 – Enable the Service

```bash
systemctl enable falcon-sensor
```

---

# Part 10 – Start the Service

```bash
systemctl start falcon-sensor
```

---

# Part 11 – Verify the Service

```bash
systemctl status falcon-sensor
```

Expected:

```
Active: active (running)
```

---

# Part 12 – Verify Sensor Registration

Check the Agent ID:

```bash
/opt/CrowdStrike/falconctl -g --aid
```

Expected:

```
aid="xxxxxxxxxxxxxxxxxxxxxxxx"
```

A populated AID indicates that the sensor has successfully registered with the CrowdStrike cloud.

---

# Part 13 – Verify Sensor Process

```bash
ps -ef | grep falcon
```

Example:

```
falcond
falcon-sensor
```

---

# Part 14 – Verify from Falcon Console

Log in to the CrowdStrike Falcon Console.

Navigate to:

```
Host Setup and Management
    └── Host Management
```

Verify that:

* The host is online.
* The sensor version matches the installed RPM.
* The host status is healthy.
* The sensor has checked in successfully.

---

# Post-Installation Validation Checklist

| Validation Item                | Status |
| ------------------------------ | ------ |
| RPM installed successfully     | ☐      |
| CID configured                 | ☐      |
| Service enabled                | ☐      |
| Service running                | ☐      |
| AID generated                  | ☐      |
| Host visible in Falcon Console | ☐      |
| Sensor version verified        | ☐      |

---

# Useful Troubleshooting Commands

## Check Installed Version

```bash
rpm -qi falcon-sensor
```

---

## Check Service Logs

```bash
journalctl -u falcon-sensor
```

Follow logs in real time:

```bash
journalctl -fu falcon-sensor
```

---

## Check Service State

```bash
systemctl is-active falcon-sensor
```

---

## Check Boot Status

```bash
systemctl is-enabled falcon-sensor
```

---

## Verify CID

```bash
/opt/CrowdStrike/falconctl -g --cid
```

---

## Verify AID

```bash
/opt/CrowdStrike/falconctl -g --aid
```

---

# Notes

* Reinstalling the sensor does **not** necessarily create a new host record in the Falcon Console. Depending on the environment and whether host identity data is preserved, the existing host entry may be reused.
* If the server uses an HTTP/HTTPS proxy, configure the proxy settings with `falconctl` before starting the service.
* If the sensor does not generate an AID within a few minutes, verify outbound connectivity (TCP 443), DNS resolution, firewall rules, and proxy configuration.
* If Sensor Tamper Protection is enabled and the maintenance token is incorrect or omitted, the uninstall or configuration change will fail.

This procedure provides a clean, auditable workflow suitable for production environments while avoiding unnecessary removal of files that CrowdStrike may retain for operational or diagnostic purposes.
