# Java Learning Journey: Java Version History


## Purpose

This SOP explains the historical evolution of Java in a story-based and practical way so that common industry confusion disappears permanently.

This SOP focuses on:

1. Why Java 8 shows `1.8`
2. Marketing version vs internal version
3. OpenJDK vs Oracle JDK
4. Why Oracle JDK and OpenJDK were different earlier
5. Why they became almost identical later
6. Oracle licensing changes
7. Modern Java ecosystem
8. LTS versions
9. Enterprise reality

---

## The Biggest Java Confusion

**Almost every beginner gets confused by this:**

```bash
java -version
```

**Output:**

```text
java version "1.8.0_381"
```

**But people say:**

```text
This is Java 8
```

So naturally the question becomes:

> “If this is Java 8, then why does it show 1.8?”

This confusion exists because Java has:

1. **Marketing** version names
2. **Internal technical** version names

**And for many years:**

```text
They were NOT the same.
```

---

## The Early Days of Java (1995–2004)

Java was originally developed by:

### `Sun Microsystems`

In the beginning, versioning was simple.

| Public Name | Internal Version |
| ----------- | ---------------- |
| Java 1.0    | 1.0              |
| Java 1.1    | 1.1              |
| Java 1.2    | 1.2              |
| Java 1.3    | 1.3              |
| Java 1.4    | 1.4              |

**At this stage:**

```text
Marketing version = Internal version
```

Everything was straightforward.

---

## The Big Marketing Shift (2004)

In 2004, Java released version:

```text
1.5
```

But this release was **HUGE**.

It introduced **major** features like:

* Generics
* Annotations
* Enhanced for-loop
* Autoboxing

**Sun Microsystems** felt this version was a **major milestone.**

**So instead of calling it:**

```text
Java 1.5
```

**they marketed it as:**

```text
Java 5
```

This is where the confusion started.

---

## The Legacy Compatibility Problem

**Even though the public marketing name changed to:**

```text
Java 5
```

**internally the system still used:**

```text
1.5
```

**Why?**

Because millions of **existing applications** already depended on **internal version** properties like:

```text
java.version
```

**If Sun Microsystems suddenly changed everything from:**

```text
1.5 -> 5
```

many enterprise applications would break. So they kept the old internal numbering system.

---

## This Confusion Continued for Years

Because of backward compatibility, Java versions continued like this:

| Marketing Name | Internal Version |
| -------------- | ---------------- |
| Java 5         | 1.5              |
| Java 6         | 1.6              |
| Java 7         | 1.7              |
| Java 8         | 1.8              |

This is why:

```text
Java 8 = version 1.8 internally
```

---

## Why Java 8 Became So Important

- Java 8 became one of the most important enterprise Java releases ever.
- Many organizations still use it today.
- That is why you still frequently see `1.8`
- in:

    * Linux servers
    * enterprise middleware
    * old banking systems
    * production JVMs
    * CI/CD pipelines

---

## The Modern Fix (Java 9+)

- The good news: The confusion finally ended in 2017.
- Starting from:

  ```text
  Java 9
  ```

- Oracle completely redesigned Java version numbering.

---

## The New Versioning System

**After Java 9:**

| Public Name | Internal Version |
| ----------- | ---------------- |
| Java 9      | 9                |
| Java 10     | 10               |
| Java 11     | 11               |
| Java 17     | 17               |
| Java 21     | 21               |

**Now:**

```text
Marketing version = Internal version
```

Finally simple again.

---

## Example Comparison

### Old System (Java 8)

```bash
java -version
```

**Output:**

```text
java version "1.8.0_381"
```

**Meaning:**

| Part | Meaning             |
| ---- | ------------------- |
| 1.8  | Java 8              |
| 0    | minor release       |
| 381  | patch/update number |

#

### Modern System (Java 17)

```bash
java -version
```

**Output:**

```text
openjdk version "17.0.10"
```

**Meaning:**

| Part | Meaning       |
| ---- | ------------- |
| 17   | Java 17       |
| 0    | minor release |
| 10   | patch number  |

---

## The Other Big Confusion — OpenJDK vs Oracle JDK

Another massive confusion in Java history is:

> “What is the difference between OpenJDK and Oracle JDK?”

To understand this, we need historical context.

---

## Before OpenJDK

Originally Java was mostly controlled directly by: `Sun Microsystems`

Earlier Java distributions contained:

* proprietary code
* commercial components
* closed-source tools

Java was not fully **open-source.**

---

## Why OpenJDK Was Created

**The Java community wanted:**

1. Open-source Java
2. Community collaboration
3. Vendor-neutral Java platform

So OpenJDK was created.

**OpenJDK means:**

```text
Open Java Development Kit
```

It became the official **open-source** Java implementation.

---

## Oracle Acquires Sun Microsystems

- In 2010
   
  **Oracle Corporation** acquired **Sun Microsystems**

- After this:
  
  **Oracle became the owner of Java.**

---

## Earlier Difference Between Oracle JDK and OpenJDK

- In older Java versions:

  ```text
  Oracle JDK != OpenJDK
  ```

- Oracle JDK included additional proprietary components like:

  * monitoring tools
  * commercial support features
  * browser plugins
  * enterprise utilities

- OpenJDK lacked some of these.

---

## Enterprise Reality in Earlier Years

- **Historically:**

  | Environment           | Common Choice |
  | --------------------- | ------------- |
  | Large enterprises     | Oracle JDK    |
  | Linux/community users | OpenJDK       |

- Because Oracle JDK was considered more “official.”

---

## The Huge Change Starting Around Java 11

>[!IMPORTANT]
This is VERY IMPORTANT.

- Starting around **Java 11**:

  ```text
  Oracle JDK and OpenJDK became almost identical technically.
  ```

- Today:
  - Oracle JDK is mostly built directly from **OpenJDK source code**.
  - Core **JVM behavior** is nearly the same.

---

## Modern Reality of OpenJDK vs Oracle JDK

- Today the major differences are mostly:

  | Area               | Difference         |
  | ------------------ | ------------------ |
  | Licensing          | Different          |
  | Commercial support | Different          |
  | Vendor branding    | Different          |
  | Packaging          | Slightly different |

- But technically:

  ```text
  OpenJDK ≈ Oracle JDK
  ```

  >for most real-world workloads.

---

## The Oracle Licensing Shock

- Starting from Java 11:

  ```text
  Oracle Corporation changed Java licensing significantly.
  ```
  > This created massive **industry panic.**

- Many organizations realized:

  ```text
  Oracle JDK may require paid licensing for commercial usage.
  ```

- As a result:

  ```text
  many companies migrated to OpenJDK-based distributions.
  ```
---

## Modern OpenJDK Vendors

Today many companies provide their **own OpenJDK** builds.

**Examples:**

| Vendor             | Distribution      |
| ------------------ | ----------------- |
| Oracle Corporation | Oracle OpenJDK    |
| Amazon             | Corretto          |
| Microsoft          | Microsoft OpenJDK |
| Red Hat            | Red Hat OpenJDK   |
| Azul Systems       | Zulu OpenJDK      |
| Eclipse Foundation | Temurin           |

> All are mostly based on OpenJDK source.

---

## Long-Term Support (LTS) Versions

- Modern Java releases every:

  ```text
  6 months
  ```

- But enterprises usually use only:

  ```text
  LTS (Long-Term Support) versions
  ```

- Important LTS versions:

  | Version | Status |
  | ------- | ------ |
  | Java 8  | LTS    |
  | Java 11 | LTS    |
  | Java 17 | LTS    |
  | Java 21 | LTS    |

---

## Real Enterprise Reality Today

- Current real-world usage often looks like this:

  | Environment                   | Common Version |
  | ----------------------------- | -------------- |
  | Old enterprise applications   | Java 8         |
  | Middleware platforms          | Java 11        |
  | Modern enterprise systems     | Java 17        |
  | Latest cloud-native platforms | Java 21        |

---

## 23. Final Mental Model

### Java Version Confusion

Remember:

- Old Java system:

  | Marketing Name | Internal Version |
  | -------------- | ---------------- |
  | Java 8         | 1.8              |


- Modern Java system:

  | Marketing Name | Internal Version |
  | -------------- | ---------------- |
  | Java 17        | 17               |

#

### OpenJDK vs Oracle JDK

- Earlier:

  ```text
  Oracle JDK and OpenJDK had noticeable differences.
  ```

- Modern Java:

  ```text
  They are now almost identical technically.
  ```

- Main differences today:

  1. licensing
  2. support
  3. vendor packaging

---

## Learning Outcome

After completing this SOP, you should clearly understand:

1. Why Java 8 shows `1.8`
2. Marketing version vs internal version
3. Why version confusion existed
4. Why Java 9 fixed the confusion
5. History of OpenJDK
6. Oracle acquisition history
7. Oracle JDK vs OpenJDK evolution
8. Oracle licensing changes
9. Modern Java ecosystem
10. Enterprise Java realities today

---