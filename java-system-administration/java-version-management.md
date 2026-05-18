# Java Version Management in Linux


## 1. Purpose

This SOP explains how Java version management works in Linux systems.

It covers:

1. Multiple Java versions
2. JDK installation locations
3. Environment variables
4. PATH management
5. JAVA_HOME configuration
6. Switching between Java versions
7. System-wide vs user-specific configuration
8. Linux Java management tools
9. Common troubleshooting

This is one of the most important Linux administration topics for Java-based systems.

---

## 2. Scope

This SOP applies to:

* Linux administrators
* Middleware engineers
* DevOps engineers
* Java application support engineers
* Beginners learning Java on Linux

Applicable to:

* RHEL
* CentOS
* Rocky Linux
* AlmaLinux
* Ubuntu
* Debian

---

## 3. Prerequisites

Before learning this SOP, you should already understand:

1. Java fundamentals
2. JVM, JRE, and JDK
3. Linux basic commands
4. Linux environment variables
5. Basic Linux file structure

---

## 4.1 Java Version Management Problem

In enterprise Linux servers:

* One application may require Java 8
* Another may require Java 11
* Another may require Java 17

Example:

| Application     | Required Java |
| --------------- | ------------- |
| Old Banking App | Java 8        |
| Elasticsearch   | Java 17       |
| Jenkins         | Java 11       |

>[!NOTE]Therefore:
Linux administrators must manage **multiple Java versions** properly.

---

## 5. Understanding Java Installation Structure

### 5.1 Common Java Installation Locations

Typical locations:

```bash
/usr/lib/jvm/
/opt/
/usr/java/
```

Example:

```bash
/usr/lib/jvm/java-1.8.0-openjdk
/usr/lib/jvm/java-11-openjdk
/usr/lib/jvm/java-17-openjdk
```

OR:

```bash
/opt/jdk1.8.0_381
/opt/jdk-11.0.22
/opt/jdk-17.0.10
```

---

# 6. Understanding Java Commands

## 6.1 Important Commands

| Command  | Purpose                   |
| -------- | ------------------------- |
| `java`   | Runs Java applications    |
| `javac`  | Compiles Java source code |
| `jar`    | Manages JAR files         |
| `jps`    | Shows Java processes      |
| `jstack` | Thread dump               |
| `jmap`   | Memory analysis           |
| `jstat`  | JVM statistics            |

---

## 7. Understanding PATH Variable

### 7.1 What is PATH?

PATH tells Linux:

> “Where should I search for executable commands?”

Example:

```bash
echo $PATH
```

Output example:

```bash
/usr/local/bin:/usr/bin:/usr/sbin
```

---

### 7.2 How Linux Finds Java

When you run:

```bash
java -version
```
Linux searches directories in PATH (`/usr/local/bin:/usr/bin:/usr/sbin`) sequentially.

Example Output:

```bash
/usr/bin/java
```

>[!NOTE] 
`/usr/bin/java` This is often a symbolic link.

To Check Symbolic link or not, run:

```bash
which java
```

Output:

```bash
/usr/bin/java
```

Now inspect:

```bash
ls -l /usr/bin/java
```

Output:

```bash
/usr/bin/java -> /etc/alternatives/java
```

Then:

```bash
ls -l /etc/alternatives/java
```

Example:

```bash
/etc/alternatives/java -> /usr/lib/jvm/java-17-openjdk/bin/java
```

---

# 8. Understanding JAVA_HOME

### 8.1 What is JAVA_HOME?

`JAVA_HOME` is an environment variable that points to the JDK root directory.

Example:

```bash
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk
```

#

### 8.2 Why JAVA_HOME is Important

Many applications require JAVA_HOME.

Examples:

* Tomcat
* Jenkins
* Maven
* Gradle
* Elasticsearch
* Kafka

Without proper JAVA_HOME: `Applications may fail to start.`

#

### 8.3 Verify JAVA_HOME

Check:

```bash
echo $JAVA_HOME
```

---

# 9. Difference Between PATH and JAVA_HOME

| Item      | Purpose                                         |
| --------- | ----------------------------------------------- |
| PATH      | Linux uses it to find commands                  |
| JAVA_HOME | Applications use it to locate Java installation |

---

## 10. Setting JAVA_HOME Temporarily

Temporary Session Configuration

```bash
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk
export PATH=$JAVA_HOME/bin:$PATH
```

Verify:

```bash
java -version
echo $JAVA_HOME
```

This works only for current terminal session.

---

## 11. Setting JAVA_HOME Permanently

### 11.1 User-Specific Configuration

Edit:

```bash
~/.bashrc
```

Add:

```bash
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk
export PATH=$JAVA_HOME/bin:$PATH
```

Reload:

```bash
source ~/.bashrc
```

#

### 11.2 System-Wide Configuration

Edit:

```bash
/etc/profile
```

OR:

```bash
/etc/profile.d/java.sh
```

Example:

```bash
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk
export PATH=$JAVA_HOME/bin:$PATH
```

Apply:

```bash
source /etc/profile
```

---

# 12. Understanding Multiple Java Versions

## 12.1 Example Scenario

Installed versions:

```bash
/usr/lib/jvm/java-1.8.0-openjdk
/usr/lib/jvm/java-11-openjdk
/usr/lib/jvm/java-17-openjdk
```

Now question:

> Which version will run when user executes `java`?

Answer:

Depends on:

1. PATH
2. alternatives system
3. symbolic links
4. shell environment

---

## 13. Linux Alternatives System

### 13.1 What is alternatives?

Linux provides:

* `alternatives`
* `update-alternatives`

Used to manage multiple versions of same software.

> Very important for Java. We will discuss more about this later.

#

### 14. Viewing Installed Java Alternatives

**For RHEL/CentOS/Rocky**

```bash
alternatives --display java
```

**For Ubuntu/Debian**

```bash
update-alternatives --config java
```

#

### 15. Switching Java Versions using Alternatives

Run:

```bash
alternatives --config java
```

Example output:

```text
There are 3 programs which provide 'java'.

Selection    Command
-----------------------------------------------
1            /usr/lib/jvm/java-1.8.0/bin/java
2            /usr/lib/jvm/java-11/bin/java
3            /usr/lib/jvm/java-17/bin/java
```

Select required version.

---

## 16. Verify Active Java Version

Check:

```bash
java -version
```

Example:

```text
openjdk version "17.0.10"
```

Also verify:

```bash
which java
readlink -f $(which java)
```

---

# 17. Managing javac Separately

### 17.1 Important Concept

Sometimes:

* `java` points to Java 17
* `javac` points to Java 8

This creates problems.

Verify both:

```bash
java -version
javac -version
```

### 17.2 Configure javac

```bash
alternatives --config javac
```

---

## 18. Understanding Symbolic Links

Typical flow:

```text
java
 ↓
/usr/bin/java
 ↓
/etc/alternatives/java
 ↓
Actual JDK binary
```

---

## 19. Application-Specific Java

### 19.1 Enterprise Reality

Many applications use their own Java version.

Example startup script:

```bash
export JAVA_HOME=/opt/jdk-11
```

This overrides system Java.

#

### 19.2 Why This is Common

Because applications may require:

* specific JVM behavior
* specific GC version
* specific compatibility

---

## 20. Best Practice — Do NOT Replace System Java Blindly

Important rule:

Do NOT globally replace Java on production servers without validation.

Because:

* old applications may break
* middleware may fail
* startup scripts may fail
* SSL libraries may behave differently

---

## 21. Finding Installed Java Versions

Find Java:

```bash
find / -name java 2>/dev/null
```

Find JDKs:

```bash
find / -type d -name "*jdk*" 2>/dev/null
```

Check RPM packages:

RHEL-based:

```bash
rpm -qa | grep -i java
```

Debian-based:

```bash
dpkg -l | grep -i java
```

---

## 23. Real Enterprise Best Practices

1. Keep multiple Java versions isolated
2. Use dedicated JAVA_HOME per application
3. Avoid changing global Java unnecessarily
4. Document Java dependency clearly
5. Validate after upgrade
6. Always verify:

   * java
   * javac
   * JAVA_HOME
   * PATH
7. Use service-specific environment files

---

## 24. Important Commands Summary

| Purpose                | Command                      |
| ---------------------- | ---------------------------- |
| Check Java version     | `java -version`              |
| Check compiler version | `javac -version`             |
| Find java binary       | `which java`                 |
| Resolve actual binary  | `readlink -f $(which java)`  |
| Check JAVA_HOME        | `echo $JAVA_HOME`            |
| Configure alternatives | `alternatives --config java` |
| Find installed JDKs    | `find / -name "*jdk*"`       |

---

## 25. Final Understanding

Java version management in Linux is mainly about:

1. JDK installation locations
2. PATH management
3. JAVA_HOME
4. alternatives system
5. symbolic links
6. application-specific Java configuration

---


