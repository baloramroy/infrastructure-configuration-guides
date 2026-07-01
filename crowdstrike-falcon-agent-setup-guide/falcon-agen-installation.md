**Standard Operating Procedure (SOP)**\
Below is a production-ready SOP for installing the CrowdStrike Falcon Sensor on **RHEL 9**.

---

# CrowdStrike Falcon Sensor Installation on RHEL 9

## Document Information

| Item                | Details                      |
| ------------------- | ---------------------------- |
| Operating System    | Red Hat Enterprise Linux 9.x |
| Software            | CrowdStrike Falcon Sensor    |
| Installation Type   | RPM Package                  |
| Required Privileges | root or sudo                 |
| Estimated Time      | 5–10 minutes                 |

---

## Objective

This SOP describes the procedure to install, configure, and verify the CrowdStrike Falcon Sensor on a Red Hat Enterprise Linux 9 server.

---

## Prerequisites

Before starting, ensure the following requirements are met:

* RHEL 9 server
* Root or sudo privileges
* Internet connectivity to CrowdStrike Cloud (unless using a organizational proxy)
* Falcon Sensor RPM package
* CrowdStrike Customer ID (CID)
* Subscription/license available in CrowdStrike Falcon Console

---

## Step 1 – Verify Operating System

- Verify the server is running RHEL 9.

  ```bash
  cat /etc/redhat-release
  ```

- Example

  ```
  Red Hat Enterprise Linux release 9.6 (Plow)
  ```

---

## Step 2 – Verify Architecture

- Run

  ```bash
  uname -m
  ```

- Example

  ```
  x86_64
  ```

  > Ensure the downloaded Falcon Sensor RPM matches the system architecture.

---

## Step 3 – Copy the Falcon Sensor RPM

- Example:

  ```
  falcon-sensor-<version>.el9.x86_64.rpm
  ```

- Copy the RPM to the server.

  Example location:

  ```
  /tmp
  ```

- Verify:

  ```bash
  ls -lh /tmp/falcon-sensor*.rpm
  ```

---

## Step 4 – Install the Falcon Sensor

- Install the RPM package.

  ```bash
  dnf install /tmp/falcon-sensor-*.rpm
  ```

  or

  ```bash
  rpm -ivh /tmp/falcon-sensor-*.rpm
  ```

- Expected output

  ```
  Complete!
  ```

---

# Step 5 – Verify Package Installation

- Check whether the package has been installed.

  ```bash
  rpm -qa | grep falcon
  ```

- Example

  ```
  falcon-sensor-<version>.x86_64
  ```

---

## Step 6 – Configure the Customer ID (CID)

- Configure the Falcon Sensor with your organization's Customer ID.

  ```bash
  /opt/CrowdStrike/falconctl -s --cid=<YOUR_CID>
  ```

- Example

  ```bash
  /opt/CrowdStrike/falconctl -s --cid=1234567890ABCDEF1234567890ABCDEF-12
  ```

- To verify:

  ```bash
  /opt/CrowdStrike/falconctl -g --cid
  ```

- Expected output

  ```
  cid="1234567890ABCDEF1234567890ABCDEF-12"
  ```

---

## Step 7 – Enable, Start and Varify the Service

- Enable the Falcon Sensor to start automatically after reboot.

  ```bash
  systemctl enable falcon-sensor
  ```

- Expected output

  ```
  Created symlink...
  ```

#

- Start the Service

  ```bash
  systemctl start falcon-sensor
  ```

#

- Verify Service Status

  ```bash
  systemctl status falcon-sensor
  ```

  Expected output

  ```
  Active: active (running)
  ```

---

## Step 10 – Verify Sensor Process

- Run

  ```bash
  ps -ef | grep falcon
  ```

- Expected output

  ```
  falcond
  falcon-sensor
  ```

---

## Step 11 – Verify Agent ID (AID)

- The Agent ID confirms the sensor has successfully registered with the CrowdStrike cloud.

  ```bash
  /opt/CrowdStrike/falconctl -g --aid
  ```

- Example

  ```
  aid="7f0dxxxxxxxxxxxxxxxxxxxxxxxx"
  ```

>[!NOTE]
If the Agent ID is **blank**, the sensor has **not yet communicated** with the **CrowdStrike cloud.**

---

## Step 12 – Verify Connectivity

- Check sensor status.

  ```bash
  /opt/CrowdStrike/falconctl -g --cid
  ```

- Check Agent ID.

  ```bash
  /opt/CrowdStrike/falconctl -g --aid
  ```

  > Both values should be populated.

---

## Step 13 – Verify from CrowdStrike Console (ADMIN Part) 

Log in to the CrowdStrike Falcon Console.

- Navigate to:

  ```
  Host Setup and Management
          ↓
  Host Management
  ```

Confirm the new RHEL server appears in the host list.

---

## Common Verification Commands

- Check Package

  ```bash
  rpm -qa | grep falcon
  ```

- Check Service

  ```bash
  systemctl status falcon-sensor
  ```

- Check Boot Status

  ```bash
  systemctl is-enabled falcon-sensor
  ```

  - Expected

    ```
    enabled
    ```


- Check Running Process

  ```bash
  ps -ef | grep falcon
  ```

- Check CID

  ```bash
  /opt/CrowdStrike/falconctl -g --cid
  ```

- Check Agent ID

  ```bash
  /opt/CrowdStrike/falconctl -g --aid
  ```

---

## Uninstall Procedure

- Stop the service.

  ```bash
  systemctl stop falcon-sensor
  ```

- Disable startup.

  ```bash
  systemctl disable falcon-sensor
  ```

- Remove the package.

  ```bash
  dnf remove -y falcon-sensor
  ```

- Verify removal.

  ```bash
  rpm -qa | grep falcon
  ```

  > No output should be returned.

---

## Troubleshooting

| Issue                              | Possible Cause                        | Resolution                                             |
| ---------------------------------- | ------------------------------------- | ------------------------------------------------------ |
| Service will not start             | Installation failed                   | Reinstall the RPM                                      |
| AID is empty                       | Sensor cannot reach CrowdStrike Cloud | Verify internet access, firewall, proxy, and DNS       |
| Host not visible in Falcon Console | CID incorrect                         | Reconfigure the correct CID and reinstall if necessary |
| `falconctl` command not found      | Package not installed correctly       | Verify RPM installation                                |

---

## Firewall Requirements

The sensor must be able to establish outbound HTTPS (TCP 443) connections to the CrowdStrike cloud. If your environment uses an HTTP/HTTPS proxy, configure the Falcon sensor with the appropriate proxy settings using `falconctl`. ([CrowdStrike Developer Center][1])

---

## References

* CrowdStrike documents installing the Linux sensor using the RPM package, configuring the Customer ID (CID), starting the `falcon-sensor` service, and verifying installation. ([CrowdStrike][2])
* RHEL-based installations use `yum`/`dnf` with the RPM package, followed by `falconctl` configuration and `systemctl start falcon-sensor`. ([Dell][3])

---
