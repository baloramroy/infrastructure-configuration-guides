**Redis 5.0.7 Installation with Master–Slave Replication and Sentinel**

# PART 1 - Redis 5.0.7 Installation on RHEL 8 / 9

- **Document Version:** 1.0
- **Redis Version:** 5.0.7
- **Target OS:** RHEL 8 / 9
- **Environment:** Production

## Purpose

This SOP describes the complete installation and configuration of:

- Redis 5.0.7 from source
- Master–Slave replication
- Redis Sentinel for High Availability
- Production-ready multi-node setup

## Environment Overview

### Architecture

- 1 Master
- 2 Slaves
- 3 Sentinel instances

#

### Cluster Nodes

| Role                 | IP Address    | Port  |
| -------------------- | ------------- | ----- |
| Master               | 10.20.2.36    | 6379  |
| Slave-1              | 10.20.2.37    | 6379  |
| Slave-2              | 10.20.2.38    | 6379  |
| Sentinel (all nodes) | respective IP | 26379 |

---

## Prerequisites

### First Check Below Development Tools are Available or not

Check if gcc is installed
```bash
rpm -q gcc
```

Check if make is installed
```bash
rpm -q make
```

Check if tcl is installed
```bash
rpm -q tcl
```

Check if jemalloc-devel is installed
```bash
rpm -q jemalloc-devel
```

#

### Install Required Development Tools

Install Development Tools group:

```bash
dnf groupinstall "Development Tools"
```

Install required packages:

```bash
dnf install gcc make tcl
```

Optional (for older dependencies):

```bash
dnf install autoconf automake libtool
```

Install jemalloc development package:

```bash
yum install jemalloc-devel
```

---

### Verify `/home` Mount Options

Redis compilation requires executable permission in the working directory.

Check mount options

```bash
mount | grep home
```

If output shows `noexec`:

`/dev/mapper/rhel-home on /home type xfs (rw,nosuid,nodev,noexec,...)`

Remount with execution permission

```bash
mount -o remount,exec /home
```

---

## Redis Package Download and Preparation

### Create Redis System User

Create redis user:

```bash
useradd redis
```

Verify user:

```bash
id redis
```

#

### Download Redis Package

Switch to redis user

```bash
su - redis
```

Download Redis 5.0.7
```bash
curl -L -O https://download.redis.io/releases/redis-5.0.7.tar.gz
```

If proxy is required:

```bash
curl -x http://10.20.10.173:49 -L -O https://download.redis.io/releases/redis-5.0.7.tar.gz
```

#

### Extract and Prepare Source

Extract package:

```bash
tar -xzf redis-5.0.7.tar.gz
```

Change directory permissions:

```bash
chmod -R 775 redis-5.0.7
```


---

## Compile Source Code and Fixxing Errors 

### Build Redis to Executable Binaries 

Navigate into source directory:

```bash
cd redis-5.0.7
```

Compile Redis Source Code running following command 

```bash
make
```

---

## Fix Multiple Definition Error (If Occurs)

### Error Example

```
multiple definition of `SDS_NOINIT'
collect2: error: ld returned 1 exit status
```

**Note:** This occurs due to incorrect variable definition in `sds.h`.

#

### Required Modifications `src/sds.c`

Run `vim src/sds.c` and find this line:

```c
const char *SDS_NOINIT = "SDS_NOINIT";
```

Comment or delete it:

```c
// const char *SDS_NOINIT = "SDS_NOINIT";
```

Add correct definition:

```c
const size_t SDS_NOINIT = (size_t)-1;
```

#

### Then Modify `src/sds.h` File

Run `vim src/sds.h` and find this line and comment out:

```c
// const char SDS_NOINIT;
```

Then Add Modified Declaration:

```c
extern const size_t SDS_NOINIT;
```

This prevents multiple-definition linker errors.

---

## Clean and Rebuild After Fixxing Error

After making changes:

```bash
make clean
make distclean
make
```

Successful build output will show:

```
Hint: It's a good idea to run 'make test' ;
```

---

## Run Test Suite

```bash
make test
```

Successful test result:

```
\o/ All tests passed without errors!
```

---

## Verify Redis Server

### Check Version

```bash
src/redis-server --version
```

#

### Test Using Redis CLI

Open second terminal:

```bash
src/redis-cli ping
```

Expected output:

```
PONG
```

---


# PART 2 — REPLICATION & SENTINEL CONFIGURATION

## Redis Master Configuration - `10.20.2.36`

Switch to **redis** user:

```bash
su - redis
```

### Config Redis File for Master Node 

Edit `redis.conf`:

```bash
vim redis-5.0.7/redis.conf
```

Modify and add below entry:

```ini
bind 10.20.2.36
port 6379 
daemonize yes 
protected-mode no 

logfile "/home/redis/log/redis.log" 
dir /home/redis 

requirepass "123456" 
masterauth "123456" 
```

#

### Configure Sentinel File for Master Node

Edit `sentinel.conf`
```bash
vim redis-5.0.7/sentinel.conf
```

Modify and add below entry:

```ini
bind 10.20.2.36
port 26379 
daemonize yes 
protected-mode no 

logfile "/home/redis/log/sentinel.log" 
dir "/tmp" 

sentinel deny-scripts-reconfig yes 
sentinel monitor mymaster 10.20.2.36 6379 2 
sentinel auth-pass mymaster 123456 
sentinel down-after-milliseconds mymaster 5000 
sentinel failover-timeout mymaster 10000 
sentinel parallel-syncs mymaster 1 
```

---

## Redis Slave-1 Configuration - `10.20.2.37`

### Edit redis.conf:

Run this `vim redis-5.0.7/redis.conf`

```ini
bind 10.20.2.37
port 6379
daemonize yes 
protected-mode no 

logfile "/home/redis/log/redis.log" 
dir "/home/redis" 

replicaof 10.20.2.36 6379
requirepass "123456" 
masterauth "123456"
```

### Edit sentinel.conf:

Run this `vim redis-5.0.7/sentinel.conf`

```ini
bind 10.20.2.37
port 26379
daemonize yes 
protected-mode no 

logfile "/home/redis/log/sentinel.log" 
dir "/tmp" 

sentinel deny-scripts-reconfig yes 
sentinel monitor mymaster 10.20.2.36 6379 2 
sentinel auth-pass mymaster 123456 
sentinel down-after-milliseconds mymaster 5000 
sentinel failover-timeout mymaster 10000 
sentinel parallel-syncs mymaster 1
```

---

## Redis Slave-2 Configuration - `10.20.2.38`

### Edit redis.conf:

Run this `vim redis-5.0.7/redis.conf`

```ini
bind 10.20.2.38
port 6379
daemonize yes 
protected-mode no 

logfile "/home/redis/log/redis.log" 
dir "/home/redis" 

replicaof 10.20.2.36 6379
masterauth 123456
requirepass "123456"
```

### Edit sentinel.conf:

Run this `vim redis-5.0.7/sentinel.conf`

```ini
bind 10.20.2.38
port 26379
daemonize yes 
protected-mode no 

logfile "/home/redis/log/sentinel.log" 
dir "/tmp" 

sentinel deny-scripts-reconfig yes 
sentinel monitor mymaster 10.20.2.36 6379 2 
sentinel auth-pass mymaster 123456 
sentinel down-after-milliseconds mymaster 5000 
sentinel failover-timeout mymaster 10000 
sentinel parallel-syncs mymaster 1
```

---

## Create Log Directory (All Nodes)

```bash
su - redis
mkdir -p /home/redis/log
```

## Firewall Permission (All Nodes) 

Allow these ports 6379, 26379 in Firewall

```bash
firewall-cmd –add-port=6379/tcp --permanent 
firewall-cmd –add-port=26379/tcp --permanent 
firewall-cmd –-reload 
```
---

## Service Control Scripts (All Nodes)

### Start Script

Create Stop File:

```bash
vim start.sh
```

Add Following Line:

```bash
#!/bin/bash

/home/redis-new/redis-5.0.7/src/redis-server \
/home/redis-new/redis-5.0.7/redis.conf > /dev/null 2>&1 &

/home/redis-new/redis-5.0.7/src/redis-server \
/home/redis-new/redis-5.0.7/sentinel.conf --sentinel > /dev/null 2>&1 &
```

#

### Stop Script

First Create Stop File:

```bash
vim stop.sh
```

Add Following Line:

```bash
#!/bin/bash
kill -9 `ps -ef | grep redis-server | grep -v grep | awk '{print $2}'` 
```

#

### Restart Script

First Create Restart File:

```bash
vim restart.sh
```

Add Following Line:

```bash
#!/bin/bash

echo "Stopping Redis processes..."
pkill -f redis-server
sleep 2

echo "Starting Redis server..."
/home/redis/redis-5.0.7/src/redis-server /home/redis/redis-5.0.7/redis.conf > /dev/null 2>&1 &

echo "Starting Redis sentinel..."
/home/redis/redis-5.0.7/src/redis-server /home/redis/redis-5.0.7/sentinel.conf --sentinel > /dev/null 2>&1 &

echo "Restart complete." 
```

### Make Scripts Executable:

```bash
chmod +x start.sh stop.sh restart.sh
```

---

## Start Redis on All Nodes

```bash
./start.sh
```

Verify processes:

```bash
ps -ef | grep redis-new
```

Expected processes:

- redis-server 6379
- redis-server 26379 (sentinel mode)

---

## Installation Verification  

### Log Verification

On each node:

```bash
tail -f /home/redis-new/log/redis.log
tail -f /home/redis-new/log/sentinel.log
```

#

### Replication Verification

On Master:

```bash
src/redis-cli -h 10.20.2.36 -a 123456
> INFO replication
```

Expected:

```
role:master
connected_slaves:2
```

#

On Slave:

```bash
src/redis-cli -h 10.20.2.37 -a 123456
> INFO replication
```

Expected:

```
role:slave
master_host:10.220.2.36
```

#

### Sentinel Verification

Check master status:

```bash
src/redis-cli -h 10.20.2.36 -p 26379
> SENTINEL masters
```

Check quorum:

```
num-other-sentinels:2
```

---

## High Availability / Failover Test

Stop master:

```bash
./stop.sh
```

*Monitor Sentinel logs on slaves.\
Within ~5 seconds (as configured), one slave will be promoted to master.*

Check:

```redis
INFO replication
```

New master should show:

```
role:master
```

**Or Run this one liner command**

```bash
/home/redis/redis-5.0.7/src/redis-cli -h 10.20.2.36 -p 26379 SENTINEL get-master-addr-by-name mymaster
```

---

## Final Architecture Summary
```
# Master:
10.20.2.36:6379

# Slaves:
10.20.2.37:6379
10.20.2.38:6379

# Sentinel running on:
All three nodes (26379)

# Quorum:
2 (Majority required)
```
---