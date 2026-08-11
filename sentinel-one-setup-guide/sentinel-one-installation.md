# SentinelOne Linux Agent Installation SOP


| **Document ID** | S1-LINUX-INSTALL-001 |
|----------------|----------------------|
| **Version** | 1.0 |
| **Effective Date** | 2026-08-11 |
| **Owner** | IT Security Team |


---

## 1. Purpose

To provide a standardized procedure for installing and configuring the SentinelOne Linux Agent on Linux endpoints to ensure consistent deployment, proper association with the Management Console, and operational readiness.

---

## 2. Scope

This procedure applies to:
- All Linux endpoints (physical and virtual) requiring SentinelOne protection
- Both x86 and ARM architectures
- RPM-based distributions (RHEL, CentOS, Fedora, etc.)
- DEB-based distributions (Ubuntu, Debian, etc.)

---

## 3. Prerequisites

Before installation, confirm the following:

| # | Prerequisite | Verification Method |
|---|--------------|---------------------|
| 1 | Endpoint meets Linux system requirements | Review SentinelOne system requirements documentation |
| 2 | Site/Group Token is available from Management Console | Navigate to **Settings > Site > Token** |
| 3 | Correct installer package for architecture (x86/ARM) | Verify file name matches architecture |
| 4 | Signed certificate for console communication | Confirm certificate is in place |
| 5 | Endpoint will NOT reboot during installation | Schedule maintenance window if needed |
| 6 | Root/sudo access to endpoint | Run `sudo -v` to verify |

---

## 4. Required Tools and Files

| Item | Description |
|------|-------------|
| SentinelOne Agent Package | `.rpm` or `.deb` file (appropriate for OS) |
| Site/Group Token | Alphanumeric string from Management Console |
| Terminal/SSH access | For remote or local execution |
| Sudo privileges | Required for installation commands |

---

## 5. Pre-Installation Checks

### 5.1 Verify System Architecture
```bash
uname -m
```
- `x86_64` → Use x86 package
- `aarch64` → Use ARM package

### 5.2 Verify Package Integrity
```bash
# For RPM
rpm -K <package_file>.rpm

# For DEB
dpkg -I <package_file>.deb
```

### 5.3 Confirm Token Availability
- Log in to SentinelOne Management Console
- Navigate to: **Settings > Site > [Your Site] > Token**
- Copy the token value for use during installation

---

## 6. Installation Procedure

### 6.1 Install the Package

**For RPM-based Systems:**
```bash
sudo rpm -i <package_file>.rpm
or
sudo dnf install <package_file>.rpm
```

**For DEB-based Systems:**
```bash
sudo dpkg -i <package_file>.deb
```

> **Note:** If `/var` has `noexec` permissions on DEB systems, bind mount dpkg to an executable directory:
> ```bash
> mkdir -p /opt/dpkg
> mount --bind /var/lib/dpkg /opt/dpkg
> mount -o remount,exec /opt/dpkg
> ```

### 6.2 Associate Agent with Management Console
```bash
sudo /opt/sentinelone/bin/sentinelctl management token set <TOKEN_VALUE>
```

**Expected Output:**
```
Setting registration token...
Registration token successfully set
```

### 6.3 Start Agent Services
```bash
sudo /opt/sentinelone/bin/sentinelctl control start
```

**Expected Output:**
```
Starting agent...
Agent is running
```

### 6.4 Verify Agent Status
```bash
sudo /opt/sentinelone/bin/sentinelctl control status
```

**Expected Output Example:**
```
Agent state      Enabled
Process Name     PID
orchestrator     41586
network          41587
scanner          41588
agent            41589
firewall         41590
```

### 6.5 Validate Installation
```bash
sudo /opt/sentinelone/bin/sentinelctl version
sudo /opt/sentinelone/bin/sentinelctl management status
```

Verify that:
- **Connectivity** parameter shows **"on"**
- **Agent version** matches installed package

---

## 7. Post-Installation Steps

### 7.1 Confirm Console Visibility
- Log in to SentinelOne Management Console
- Navigate to **Endpoints**
- Verify the new endpoint appears and shows as **Active**

### 7.2 Document Installation
Record the following details:
- Endpoint hostname
- Installation date and time
- Agent version installed
- Technical contact for the endpoint

### 7.3 Schedule Regular Health Checks
- Add to monitoring for periodic status verification
- Document any issues or anomalies

---

## 8. Proxy Configuration

This section covers configuring, viewing, and clearing proxy settings for both Console connectivity and Deep Visibility (DV) services.

### 8.1 Console Proxy Commands

#### 8.1.1 Set Console Proxy
```bash
sudo /opt/sentinelone/bin/sentinelctl management proxy set <protocol>://[user:password@]proxy-address:port
```

| Parameter | Description |
|-----------|-------------|
| protocol | `http`, `https`, or `socks5` |
| user:password | Optional - for authenticated proxy |
| proxy-address | IP address or FQDN of proxy server |
| port | Access port number |

**Examples:**
```bash
# Unauthenticated proxy
sudo /opt/sentinelone/bin/sentinelctl management proxy set http://192.168.17.10:8080

# Authenticated proxy
sudo /opt/sentinelone/bin/sentinelctl management proxy set https://usr1:password@192.0.2.5:443

# Multiple proxies (version 25.4 and later)
sudo /opt/sentinelone/bin/sentinelctl management proxy set "http://192.168.17.10:8080,http://192.168.17.10:3128"
```

#### 8.1.2 View Console Proxy
```bash
sudo /opt/sentinelone/bin/sentinelctl management proxy get
```

**Expected Output:**
```
Proxy address: http://192.168.17.10:8080
```

#### 8.1.3 Clear Console Proxy
```bash
sudo /opt/sentinelone/bin/sentinelctl management proxy clear
```

**Expected Output:**
```
Stopping agent...
Agent stopped
Proxy address successfully removed
Starting agent...
Agent is running
```

#### 8.1.4 Verify Proxy Cleared
```bash
sudo /opt/sentinelone/bin/sentinelctl management proxy get
```

**Expected Output:**
```
No proxy address configured
```

---

### 8.2 Deep Visibility (DV) Proxy Commands

#### 8.2.1 Set DV Proxy
```bash
sudo /opt/sentinelone/bin/sentinelctl management dv proxy set <protocol>://[user:password@]proxy-address:port
```

**Examples:**
```bash
# Unauthenticated proxy
sudo /opt/sentinelone/bin/sentinelctl management dv proxy set http://test.test:8050

# Authenticated proxy
sudo /opt/sentinelone/bin/sentinelctl management dv proxy set https://usr1:password@192.0.2.5:443

# Multiple proxies (version 25.4 and later)
sudo /opt/sentinelone/bin/sentinelctl management dv proxy set "http://192.168.17.10:8080,http://192.168.17.10:3128"
```

**Expected Output:**
```
Stopping agent...
Agent stopped
Setting proxy address...
Processed address: http://test.test:8050
Proxy address successfully set
Starting agent...
Agent is running
```

#### 8.2.2 View DV Proxy
```bash
sudo /opt/sentinelone/bin/sentinelctl management dv proxy get
```

**Expected Output:**
```
Proxy address: http://test.test:8050
```

#### 8.2.3 Clear DV Proxy
```bash
sudo /opt/sentinelone/bin/sentinelctl management dv proxy clear
```

**Expected Output:**
```
Stopping agent...
Agent stopped
Proxy address successfully removed
Starting agent...
Agent is running
```

#### 8.2.4 Verify DV Proxy Cleared
```bash
sudo /opt/sentinelone/bin/sentinelctl management dv proxy get
```

**Expected Output:**
```
No proxy address configured
```

---

### 8.3 Proxy Configuration Summary Table

| Command | Purpose | Example |
|---------|---------|---------|
| `management proxy set` | Configure Console proxy | `set http://proxy:8080` |
| `management proxy get` | View Console proxy | `get` |
| `management proxy clear` | Remove Console proxy | `clear` |
| `management dv proxy set` | Configure DV proxy | `set http://dvproxy:8050` |
| `management dv proxy get` | View DV proxy | `get` |
| `management dv proxy clear` | Remove DV proxy | `clear` |

#

### 8.4 Important Proxy Notes

> **⚠️ Critical Information for Versions 25.4 and 26.1.1 GA:**

1. **Separate proxies required** - Console and DV proxies must be configured separately
2. **Downgrade behavior** - Proxy configuration is REMOVED when downgrading to earlier versions
3. **Reconfiguration required** - After downgrade, reconfigure proxy in the earlier version
4. **Multiple proxy support** - Version 25.4 and later accept multiple proxy addresses

---


## 9. Troubleshooting

| Issue | Probable Cause | Resolution |
|-------|---------------|------------|
| Package installation fails | Wrong architecture | Verify architecture and use correct package |
| Token set fails | Invalid token or network issue | Verify token from Console and check network |
| Agent not starting | Conflict with other security software | Disable conflicting services temporarily |
| Connectivity shows "off" | Firewall blocking communication | Allow outbound ports to Management Console |
| SSL/CACERT error | Missing certificates | Manually add certificates per documentation |

---

## 10. Rollback Procedure

If rollback is required:
1. Stop the agent: `sudo sentinelctl control stop`
2. Uninstall the package:
   - DNF: `sudo dnf uninstall <package_name>`
   - RPM: `sudo rpm -e <package_name>`
   - DEB: `sudo dpkg -r <package_name>`
3. Reinstall previous version using this SOP

> **Note:** If downgrading from versions 25.4 or 26.1.1 GA, proxy configuration must be reconfigured after rollback.

---


## 11. Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-08-11 | Baloram Roy | Initial Release |

---

## 12. References

- SentinelOne Linux Agent Installation Guide
- SentinelOne Management Console User Guide
- System Requirements for Linux Agents
- Proxy Configuration for Linux Agents

---

**END OF DOCUMENT**

---

*This SOP is a controlled document. Unauthorized reproduction or distribution is prohibited.*
