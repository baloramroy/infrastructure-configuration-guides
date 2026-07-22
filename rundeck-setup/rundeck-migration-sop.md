# Rundeck Migration & Restoration Execution Log

- **Source Version:** Rundeck 2.11.3-1.54.GA
- **Target OS:** RHEL/CentOS
- **Java Version:** Oracle JDK 8u491
- **Migration Date:** June–July 2026

---

# Purpose

This document records every command executed during the migration and restoration of a Rundeck server from the old environment to a new server.

The process includes:

* Installing Java
* Installing Rundeck
* Restoring the backup
* Updating configuration
* Setting permissions
* Creating a systemd service
* Starting Rundeck
* Verifying the installation
* Testing the API

---

# Step 1 — Install Java

### Move to installation directory

```bash
cd /opt
```

List files.

```bash
ll
```

Install Oracle JDK.

```bash
dnf install jdk-8u491-linux-x64.rpm
```

Verify installation.

```bash
ll

java -version
```

Create a shorter symbolic link.

```bash
ln -s /usr/lib/jvm/jdk-1.8.0_491-oracle-x64 jdk-1.8.0_491
```

Verify.

```bash
ll
```

Configure default Java.

```bash
alternatives --config java
```

Configure default compiler.

```bash
alternatives --config javac
```

---

# Step 2 — Basic Server Verification

Move to the home directory.

```bash
cd /home
```

Verify files.

```bash
ll
```

Check system status.

```bash
pstat

top
```

Check network information.

```bash
ip addr
```

Clear the screen.

```bash
clear
```

Check operating system version.

```bash
cat /etc/redhat-release
```

Check Java version.

```bash
java -version 2>&1
```

Check whether Rundeck is already installed.

```bash
rpm -qa | grep -i rundeck
```

Check Rundeck data directory.

```bash
ls /var/lib/rundeck 2>/dev/null && echo "EXISTS" || echo "NOT FOUND"
```

Check Rundeck configuration directory.

```bash
ls /etc/rundeck 2>/dev/null && echo "EXISTS" || echo "NOT FOUND"
```

---

# Step 3 — Extract Rundeck RPM Package

Verify archive.

```bash
ls -lh /tmp/
```

Extract RPM package.

```bash
cd /tmp && tar xzf rundeck-rpms.tar.gz
```

Verify extracted RPMs.

```bash
ls -lh /tmp/rundeck*.rpm
```

---

# Step 4 — Install Rundeck

Attempt a normal installation.

```bash
rpm -ivh \
/tmp/rundeck-config-2.11.3-1.54.GA.noarch.rpm \
/tmp/rundeck-2.11.3-1.54.GA.noarch.rpm
```

If dependency errors occur, install without dependency checking.

```bash
rpm -ivh --nodeps \
/tmp/rundeck-config-2.11.3-1.54.GA.noarch.rpm \
/tmp/rundeck-2.11.3-1.54.GA.noarch.rpm
```

Verify installation.

```bash
ls /var/lib/rundeck/

ls /etc/rundeck/
```

---

# Step 5 — Restore Migration Backup

Move to the temporary directory.

```bash
cd /tmp/

clear

ll
```

Extract migration archive to the root filesystem.

```bash
cd /

tar xzf /tmp/rundeck-migration.tar.gz
```

---

# Step 6 — Verify Restored Data

Verify configuration.

```bash
ls /etc/rundeck/
```

Verify database.

```bash
ls /var/lib/rundeck/data/
```

Verify projects.

```bash
ls /var/rundeck/projects/
```

Verify plugins.

```bash
ls /var/lib/rundeck/libext/
```

---

# Step 7 — Create Project Log Directories

Create required directories.

```bash
mkdir -p \
/var/lib/rundeck/logs/rundeck/{DFS_CLR,DFS_BDS,DFS_BKOFC,DFS_DPS,DFS_DRS,GP_BASE,ROBi_BASE,Banglalink_Base,NotificationSchedular,test_kod_rac}/{job,run/logs}
```

Assign ownership.

```bash
chown -R rundeck:rundeck /var/lib/rundeck/logs/
```

Verify.

```bash
ls -R /var/lib/rundeck/logs/rundeck/ | head -30
```

---

# Step 8 — Update Configuration

Search for old hostname and IP.

```bash
grep -rn "nagad-rundeck1\|10.220.2.30" /etc/rundeck/
```

Replace old IP.

```bash
sed -i \
's/10.220.2.30/10.220.110.30/g' \
/etc/rundeck/rundeck-config.properties \
/etc/rundeck/framework.properties
```

Replace hostname.

```bash
sed -i \
's/nagad-rundeck1/dc3-dfs-prod-core-rundeck01/g' \
/etc/rundeck/framework.properties
```

Verify replacements.

```bash
grep -n \
"10.220.110.30\|dc3-dfs-prod-core-rundeck01" \
/etc/rundeck/rundeck-config.properties \
/etc/rundeck/framework.properties
```

---

# Step 9 — Correct Ownership & Permissions

```bash
clear
```

Assign ownership.

```bash
chown -R rundeck:rundeck /var/lib/rundeck/

chown -R rundeck:rundeck /etc/rundeck/

chown -R root:root /var/rundeck/
```

Directory permissions.

```bash
chmod 775 /etc/rundeck/
```

Configuration permissions.

```bash
chmod 640 \
/etc/rundeck/*.properties \
/etc/rundeck/*.aclpolicy \
/etc/rundeck/*.conf \
/etc/rundeck/profile \
/etc/rundeck/log4j.properties
```

SSL directory permissions.

```bash
chmod 750 /etc/rundeck/ssl
```

Verify.

```bash
ls -ld \
/var/lib/rundeck/ \
/var/rundeck/ \
/etc/rundeck/

ls -la /etc/rundeck/
```

---

# Step 10 — Verify Java

```bash
which java

java -version

ls /opt/jdk* 2>/dev/null || ls /usr/lib/jvm/
```

---

# Step 11 — Create systemd Service

Create the service.

```bash
cat > /etc/systemd/system/rundeckd.service << 'EOF'
[Unit]
Description=Rundeck Server
After=network.target

[Service]
Type=simple
User=rundeck
Group=rundeck
Environment=JAVA_HOME=/opt/jdk-1.8.0_491
EnvironmentFile=-/etc/sysconfig/rundeckd
ExecStartPre=/bin/mkdir -p /tmp/rundeck
ExecStartPre=/bin/chown rundeck:rundeck /tmp/rundeck
ExecStart=/bin/bash -c 'source /etc/rundeck/profile && exec $rundeckd'
ExecStop=/bin/kill -TERM $MAINPID
LimitNOFILE=65535
TimeoutStopSec=60
Restart=on-failure

[Install]
WantedBy=multi-user.target
EOF
```

Verify.

```bash
cat /etc/systemd/system/rundeckd.service
```

---

# Step 12 — Start Rundeck

Reload systemd.

```bash
systemctl daemon-reload
```

Start service.

```bash
systemctl start rundeckd
```

Check status.

```bash
systemctl status rundeckd
```

Wait and verify listening port.

```bash
sleep 30

ss -tlnp | grep 4440
```

---

# Step 13 — Enable Auto Start

```bash
clear
```

Enable service.

```bash
systemctl enable rundeckd
```

Verify HTTP.

```bash
curl -s -o /dev/null -w "%{http_code}" http://10.220.110.30:4440
```

Create workaround.

```bash
ln -s /bin/true /usr/lib/systemd/systemd-sysv-install
```

Enable again.

```bash
systemctl enable rundeckd
```

Verify.

```bash
systemctl is-enabled rundeckd
```

---

# Step 14 — Verify Authentication

Display local users.

```bash
cat /etc/rundeck/realm.properties
```

---

# Step 15 — Test REST API

Attempt API authentication.

```bash
curl -s \
-u admin:admin \
-H "Accept: application/json" \
http://10.220.110.30:4440/api/14/projects \
| python3 -m json.tool
```

Create authenticated session.

```bash
curl -s \
-c /tmp/rd-cookie \
-b /tmp/rd-cookie \
-d "j_username=admin&j_password=admin" \
http://10.220.110.30:4440/j_security_check \
-L \
-o /dev/null \
-w "%{http_code}"
```

Retrieve project list.

```bash
curl -s \
-b /tmp/rd-cookie \
-H "Accept: application/json" \
http://10.220.110.30:4440/api/14/projects \
| python3 -m json.tool
```

Retrieve jobs for project **DFS_CLR**.

```bash
curl -s \
-b /tmp/rd-cookie \
-H "Accept: application/json" \
http://10.220.110.30:4440/api/14/project/DFS_CLR/jobs \
| python3 -m json.tool
```

---

# Summary

This migration performed the following tasks in order:

1. Installed Oracle JDK 8u491 and configured it as the default Java runtime.
2. Verified the target server environment and checked for any existing Rundeck installation.
3. Extracted the Rundeck RPM packages and installed Rundeck 2.11.3.
4. Restored the Rundeck migration backup (`rundeck-migration.tar.gz`) onto the target server.
5. Verified that configuration files, project data, plugins, and database files were restored successfully.
6. Created the required project log directory structure and assigned the correct ownership.
7. Updated the Rundeck configuration to replace the old hostname (`nagad-rundeck1`) and IP address (`10.220.2.30`) with the new server hostname (`dc3-dfs-prod-core-rundeck01`) and IP address (`10.220.110.30`).
8. Corrected file ownership and permissions for the Rundeck installation.
9. Created a custom `systemd` service for Rundeck, configured it to use Oracle JDK 8, and started the service.
10. Enabled Rundeck to start automatically at boot and applied a workaround for the missing `systemd-sysv-install` utility.
11. Verified that the Rundeck web interface was accessible on port **4440**.
12. Validated the restored environment by authenticating with the Rundeck REST API, listing the available projects, and retrieving the jobs for the `DFS_CLR` project.

This document reflects the complete sequence of commands you provided, preserving every significant step while organizing them into a structured migration procedure.
