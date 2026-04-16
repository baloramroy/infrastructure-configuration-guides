# Install SNMP v3 on Redhat Enterprise Linux

## Install SNMP Packages

### Install required packages:

```bash
dnf install -y net-snmp net-snmp-utils net-snmp-libs net-snmp-devel
```

(For older RHEL/CentOS 7 use `yum` instead of `dnf`.)



### Enable and Start SNMP Service

```bash
systemctl enable snmpd
systemctl start snmpd
```

### Check status:

```bash
systemctl status snmpd
```

---

## Creating SNMPv3 User

### Stop SNMP Service First

The `net-snmp-create-v3-user` tool requires `snmpd` to be stopped before running.

```bash
systemctl stop snmpd
```


### Create SNMPv3 User

```bash
net-snmp-config --create-snmpv3-user \
-A 'NewAuthPassword' \
-X 'NewPrivPassword' \
-a SHA \
-x AES \
opmanager
```


### Start SNMP Service Again

```bash
systemctl start snmpd
```

---

## Verify Configuration File

The command automatically adds configuration in:

### Main Config:

```bash
/etc/snmp/snmpd.conf
```

### User Security Config:

```bash
/var/lib/net-snmp/snmpd.conf
```

### Typical entries look like:

```bash
rouser monitoruser
```

or

```bash
rwuser monitoruser
```

These directives control SNMPv3 read-only/read-write access. 

---

## Configure Firewall

### If monitoring server IP is:

```bash
10.10.10.50
```

### Add firewall restriction using a Rich Rule:

```bash
firewall-cmd --permanent \
--add-rich-rule='rule family="ipv4" source address="10.10.10.50" port protocol="udp" port="161" accept'
```
> This ensures only monitoring server can poll SNMP.

### Reload Firewalld to make the change permanent 
```bash
firewall-cmd --reload
```

---

## Test SNMPv3 Locally

Use this command:

```bash
snmpwalk -v3 \
-u opmanager \
-l authPriv \
-a SHA \
-A 'NewAuthPassword' \
-x AES \
-X 'NewPrivPassword' \
localhost
```

If configured correctly, you should see SNMP output.


--- 


# Recreate SNMP User

To change the password for an existing SNMPv3 user in **Net-SNMP**, you generally **do not edit the encrypted entries manually** in the config file. The clean/supported way is:

1. **Delete the old SNMPv3 user**
2. **Recreate it with the new password**

Because SNMPv3 stores localized/encrypted credentials in the persistent config, simply editing plaintext is not reliable.

---

## Your Current User

### Suppose your SNMPv3 user is:

```text
opmanager
```

## Recommended Method: Recreate the User

### Stop SNMP Service

```bash
systemctl stop snmpd
```


### Backup Persistent Config

```bash
cp /var/lib/net-snmp/snmpd.conf /var/lib/net-snmp/snmpd.conf.bak
```


### Remove Existing User Entry

Open the persistent SNMP file:

```bash
vi /var/lib/net-snmp/snmpd.conf
```

Find lines related to the user, such as:

```conf
usmUser ...
```

Delete the entries associated with **opmanager**.



### Recreate User with New Password

Run your create command again with the new password:

```bash
net-snmp-config --create-snmpv3-user \
-A 'NewAuthPassword' \
-X 'NewPrivPassword' \
-a SHA \
-x AES \
opmanager
```

### Start SNMP Service

```bash
systemctl start snmpd
```

---

## Important Shell Warning About Your Password

Your original command contains special characters like:

```text 
#
$
```

In shell, **`$` gets interpreted as variable expansion**, and `#` can affect parsing depending on context.

So always wrap passwords in **single quotes** when running commands:

```bash id="z1r8gn"
-A 'Ops@dm1n#1226556$nmp'
-X 'Ops@dm1n#1226556$nmp'
```

Otherwise the actual password stored may **not be what you think**.

---

## Verify It Works

After recreating:

```bash
snmpwalk -v3 \
-u opmanager \
-l authPriv \
-a SHA \
-A 'NewAuthPassword' \
-x AES \
-X 'NewPrivPassword' \
localhost
```

---


