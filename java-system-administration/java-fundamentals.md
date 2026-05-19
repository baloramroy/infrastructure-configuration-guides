
# Java Learning Journey: Java Fundamentals

## Purpose

This phase focuses on understanding:

* what programming is
* what Java is
* how Java programs are structured
* how Java code is compiled and executed
* why Java is platform independent

This is the most important foundation phase for every Java learner.


---

## Learning Objectives

After completing this SOP, you should be able to:

* understand the purpose of programming
* explain what Java is
* write a basic Java program
* understand Java program structure
* explain compilation and execution flow
* understand why Java is platform independent
* prepare for JVM/JRE/JDK deep learning

---

## Prerequisites

Before starting:

* Basic computer knowledge
* Basic Linux terminal knowledge (recommended)
* Basic text editor usage

---

## What is Programming?

Programming means:

```bash
Giving instructions to a computer to perform tasks.
```

A computer cannot understand human language directly.

It understands only:

* machine language
* binary instructions
* low-level operations

Programming languages help humans communicate with computers.

Examples:

* Java
* Python
* C
* Go
* JavaScript

---

## What is a Program?

A program is:

```bash
A collection of instructions written to solve a specific problem.
```

Examples:

* calculator application
* banking software
* web application
* ATM software
* hospital management system

---

## What is Java?

Java is:

```bash
A high-level, object-oriented programming language developed by Sun Microsystems.
```

Later, Java was acquired and maintained by Oracle Corporation.

Java is widely used for:

* enterprise applications
* banking systems
* backend development
* Android applications
* distributed systems
* middleware platforms
* big data technologies

Examples of Java-based technologies:

* Apache Kafka
* Elasticsearch
* Hadoop
* Zookeeper
* Jenkins

---

## Why Java Became Popular

Java became popular because it provides:

| Feature              | Description                   |
| -------------------- | ----------------------------- |
| Platform Independent | Write once, run anywhere      |
| Object-Oriented      | Organized code structure      |
| Secure               | Strong security model         |
| Portable             | Can run on multiple systems   |
| Robust               | Good error handling           |
| Multithreaded        | Supports concurrent execution |

---

## What Does “Platform Independent” Mean?

Platform means:

* Operating System
* CPU Architecture
* Environment

Examples:

* Linux
* Windows
* macOS

Normally:

* C programs compiled on Linux may not run on Windows
* Different operating systems require different binaries

Java solved this problem.

### Bytecode

- Java programs are converted into **Bytecode** instead of direct machine code.
- This bytecode can run on any system that has a Java runtime environment.

That is why Java says:

> “Write Once, Run Anywhere (WORA)”

---

## Basic Java Program Structure

**Example:**

```java
public class HelloWorld {

    public static void main(String[] args) {

        System.out.println("Hello World");

    }

}
```

#

### Understanding Each Part

**`class`:**

A class is a blueprint or container that holds code and data.

```java
public class HelloWorld
```

Java programs are written inside classes.

Here:

* `HelloWorld` = class name

Example:

```text
HelloWorld.java
```

#

**`main` Method**

```java
public static void main(String[] args)
```

This is the entry point of the Java program.

Execution starts from:

```java
main()
```

Without the `main()` method, Java cannot start program execution normally.

#

**`System.out.println()`**

```java
System.out.println("Hello World");
```

Used to print output to the console.

Output:

```text
Hello World
```

---

## Java Source File Extension

Java source files use:

```text
.java
```

Example:

```text
HelloWorld.java
```

---

## Compilation Concept

Java is **NOT directly** executed from **source code**.

Java follows **two major** stages:

### Stage 1 — Compilation

Source code:

```text
HelloWorld.java
```

is compiled using:

```bash
javac HelloWorld.java
```

This generates:

```text
HelloWorld.class
```

The `.class` file contains: `Bytecode`

---

## Execution Concept

**After compilation:**

```bash
java HelloWorld
```

The Java runtime executes the bytecode.

**Flow:**

```text
.java → compilation → .class → execution
```

---

## Visual Java Flow

```text
Java Source Code
      ↓
HelloWorld.java
      ↓
Compilation (javac)
      ↓
Bytecode (.class)
      ↓
Java Runtime
      ↓
Program Output
```

---

## Important Beginner Understanding

At this stage, you only need to understand:

| Component | Simple Meaning    |
| --------- | ----------------- |
| `.java`   | Java source code  |
| `javac`   | Java compiler     |
| `.class`  | Bytecode          |
| `java`    | Runs Java program |

Deep topics like:

* JVM
* JRE
* JDK
* ClassLoader
* Bytecode execution

will be learned in the next phase.

---

## Hands-On Practice

### Create File

```bash
vim HelloWorld.java
```

**Add:**

```java
public class HelloWorld {

    public static void main(String[] args) {

        System.out.println("Hello World");

    }

}
```

#

### Compile

```bash
javac HelloWorld.java
```

**Expected output:**

```text
HelloWorld.class
```

#

### Execute

```bash
java HelloWorld
```

**Expected:**

```text
Hello World
```

---

## Key Takeaways

You should now understand:

* what programming is
* what Java is
* Java basic structure
* class concept
* main method concept
* compilation vs execution
* source code vs bytecode
* basic Java execution flow
* why Java is platform independent

---

## Next Learning Phase

After mastering this phase, continue with:

### Phase 2 — JVM, JRE, and JDK Architecture

Topics:

* What is JVM
* What is JRE
* What is JDK
* Relationship between JVM/JRE/JDK
* Java execution internals
* Bytecode execution
* Java memory basics

---

## Summary

This phase builds the core Java foundation.

Without understanding:

* compilation
* execution
* bytecode
* class structure
* main method

advanced Java concepts become confusing later.

Master this phase first before moving to JVM/JRE/JDK internals.
