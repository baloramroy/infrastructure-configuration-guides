# **SOP: Understanding JVM, JRE, and JDK**

## Purpose

This SOP explains the core Java architecture components — **JVM, JRE, and JDK** — to build a clear understanding of how Java applications are compiled and executed in Linux and other environments.

---

## Scope

This document is applicable for:

* Java beginners
* Linux system administrators
* DevOps engineers
* Application support engineers working with Java-based systems (Kafka, Elasticsearch, Tomcat, etc.)

---

## Java Architecture Overview

Java follows a layered architecture:

```text
JDK ⟶ JRE ⟶ JVM
```

Where:

* **JVM** → Executes Java programs
* **JRE** → Provides runtime environment
* **JDK** → Provides development + runtime tools

---

## JVM (Java Virtual Machine)

### Definition

JVM is the core execution engine responsible for running Java bytecode.


### Input and Output

* Input: `.class` file (bytecode)
* Output: Executed machine-level instructions


### Responsibilities

* Loads `.class` files
* Verifies bytecode (security validation)
* Executes instructions
* Manages memory (Garbage Collection)
* Converts bytecode into machine-level execution


### Compilation and Execution Flow

```bash
javac Hello.java   → generates Hello.class
java Hello         → JVM executes bytecode
```


### Analogy

JVM is equivalent to:

> Engine of a vehicle

Without JVM, Java programs cannot run.

---

## JRE (Java Runtime Environment)

### Definition

JRE is a runtime package that provides everything required to run Java applications.


### Components

* JVM
* Core Java libraries (java.lang, java.util, etc.)
* Supporting runtime files


### Limitations

JRE does NOT include:

* Java compiler (`javac`)
* Development/debugging tools


### Usage

* Used to run Java applications
* NOT used for development


### Analogy

> JRE = Ready-to-drive vehicle (engine + fuel system included)

---

## JDK (Java Development Kit)

### Definition

JDK is a full development toolkit used to build and run Java applications.


### Components

* JRE
* JVM
* Java compiler (`javac`)
* Debugging tools
* Monitoring tools (`jconsole`, `jcmd`, etc.)


### Usage

* Used by developers
* Required for compiling and running Java applications


### Analogy

> JDK = Full workshop (tools + runtime + engine)

---

## Comparison Table

| Feature                    | JVM | JRE | JDK |
| -------------------------- | --- | --- | --- |
| Executes Java programs     | Yes | Yes | Yes |
| Provides runtime libraries | No  | Yes | Yes |
| Includes compiler (javac)  | No  | No  | Yes |
| Used by end users          | No  | Yes | No  |
| Used by developers         | No  | No  | Yes |

---

## Java Execution Flow

**Step 1: Write Code**

```java
Hello.java
```

**Step 2: Compile (JDK)**

```bash
javac Hello.java
```

**Step 3: Bytecode Generated**

```text
Hello.class
```

**Step 4: Execution (JVM)**

```bash
java Hello
```

---

## Linux Environment Perspective

**In Linux systems:**

* Java versions are installed as multiple packages (Java 8, 11, 17, etc.)
* Active version is controlled by:

  * `alternatives` system
  * `PATH`
  * `JAVA_HOME`


**Example Commands**

```bash
java -version
javac -version
```

If both commands work → JDK is installed. 
> We will discuss more about this later.

---

## Production Relevance

In enterprise environments (Kafka, Elasticsearch, Tomcat):

* JVM performance is critical (memory, GC tuning)
* JDK version compatibility must match application requirements
* JRE is rarely managed separately in modern Java distributions

---

## Summary

* **JVM** → Executes Java bytecode
* **JRE** → Provides runtime environment
* **JDK** → Provides development + runtime tools

---

## Key Takeaway

```text
JDK = Development Toolkit
   ↓
JRE = Runtime Environment
   ↓
JVM = Execution Engine
```

---