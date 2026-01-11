# PHP-FPM

In PHP, **FPM** stands for **FastCGI Process Manager**.

It is a **PHP execution engine** designed to handle PHP requests **efficiently and at high scale**, especially when used with web servers like **Nginx** and **Apache (event/worker MPM)**.

---

## 1. What is PHP-FPM?

**PHP-FPM (FastCGI Process Manager)** is:

- A **FastCGI implementation for PHP**
- A **process manager** that controls how PHP processes are started, stopped, and reused
- A **replacement for mod_php** in modern server setups

Instead of running PHP inside the web server, PHP runs as a **separate service**, and the web server forwards PHP requests to it.

---

## 2. Why PHP-FPM is used?

PHP-FPM is used because it provides:

1. **Better performance**
   - Reuses PHP worker processes
   - Handles many concurrent requests efficiently

2. **Better resource control**
   - Limit memory per pool
   - Control number of PHP processes

3. **Process isolation**
   - Different websites can run under different users
   - Each site can have its own PHP configuration

4. **Scalability**
   - Ideal for high-traffic websites
   - Works very well with Nginx

---

## 3. How PHP-FPM works

```
Browser
   ↓
Web Server (Nginx / Apache)
   ↓  (FastCGI)
PHP-FPM
   ↓
PHP Script Execution
   ↓
Response back to Web Server
   ↓
Browser
```

---

## 4. PHP-FPM vs mod_php

| Feature               | PHP-FPM          | mod_php       |
| --------------------- | ---------------- | ------------- |
| PHP runs as           | Separate service | Inside Apache |
| Performance           | High             | Lower         |
| Works with Nginx      | Yes              | No            |
| Per-site PHP settings | Yes              | No            |
| Recommended today     | Yes              | No            |

---

## 5. PHP-FPM Pools

PHP-FPM uses **pools**.

Each pool:
- Runs under a specific **Linux user/group**
- Has its own **php.ini settings**
- Has its own **process limits**

Example:

```
/etc/php-fpm.d/www.conf
```

You can create:

```
site1.conf
site2.conf
```

Each site runs independently.

---

## 6. Common PHP-FPM Service Commands

```bash
systemctl status php-fpm
systemctl start php-fpm
systemctl stop php-fpm
systemctl restart php-fpm
```

Check listening socket:

```bash
ss -lntp | grep php-fpm
```

---

## 7. When should you use PHP-FPM?

You should use PHP-FPM when:

• Using **Nginx**
• Running **multiple PHP websites**
• Need **better performance**
• Hosting **production servers**

---

## 8. One-line summary

PHP-FPM = A high-performance PHP process manager that executes PHP scripts separately from the web server using FastCGI.

---
