## 1. Web Server

###  What is a Web Server?

A **web server** is software that **receives HTTP/HTTPS requests from clients (browsers)** and **serves responses** such as:

- HTML pages
- Images, CSS, JS files
- API responses

### How it works

1. Client sends HTTP request
2. Web server receives the request
3. Web server:

   * Serves a static file **OR**
   * Forwards request to backend application
4. Sends response back to client

### Common Web Servers

- Apache HTTP Server
- NGINX
- Microsoft IIS

### Example

```text
Browser → NGINX → index.html
```

---

## 2. Reverse Proxy

### What is a Reverse Proxy?

A **reverse proxy** is a server that **sits in front of backend servers** and **forwards client requests to them**.

>Client **never directly communicates** with backend servers.

### What it does

1. Receives client request
2. Decides **which backend server** should handle it
3. Forwards request
4. Receives response
5. Sends response back to client

### Key Responsibilities

- Load balancing
- SSL termination
- Security (hide backend servers)
- Caching
- Rate limiting

### Example

```text
Browser → NGINX (Reverse Proxy) → App Server
```

### Common Reverse Proxies

- NGINX
- HAProxy
- Traefik
- Envoy

### One Line Memory:
- When it **serves request** then its a **web server**.
- When it **forward request** then its a **reverse proxy**.
- Same application handle **different role**.
- Exmaple: **Nginx**

---

## 3. Difference Between Web Server and Reverse Proxy

| Aspect              | Web Server          | Reverse Proxy     |
| ------------------- | ------------------- | ----------------- |
| Main role           | Serve web content   | Forward requests  |
| Client talks to     | Web server directly | Reverse proxy     |
| Backend awareness   | Optional            | Mandatory         |
| Load balancing      | Limited             | Core feature      |
| Security role       | Basic               | Strong            |
| Static file serving | Yes                 | Usually No        |
| SSL termination     | Yes                 | Yes (very common) |

**Key Idea**

- **Web server** → serves content
- **Reverse proxy** → routes traffic

---
## 4. Web Engine

## What is a Web Engine?

A **web engine** (or application engine) is the **runtime that executes backend application code**.

>It **does NOT** talk to browsers directly.

### What it does 
- Executes application logic
- Handles business rules
- Generates dynamic responses

### Examples

| Language | Web Engine      |
| -------- | --------------- |
| PHP      | PHP-FPM         |
| Python   | Gunicorn, uWSGI |
| Java     | Tomcat          |
| Node.js  | Node runtime    |

### Example

```text
NGINX → PHP-FPM → PHP Code Execution
```

---

## 5. Difference Between Web Server and Web Engine

| Aspect              | Web Server | Web Engine |
| ------------------- | ---------- | ---------- |
| Talks to browser    | Yes        | No         |
| Executes code       | No         | Yes        |
| Serves static files | Yes        | No         |
| Handles HTTP        | Yes        | No         |
| Runs business logic | No         | Yes        |

**Simple Rule to Remember**

- Web Server = **Traffic handler**
- Web Engine = **Code executor**

---

## 6. Real Production Architecture

```text
Client Browser
      ↓
Reverse Proxy (NGINX)
      ↓
Web Server (NGINX / Apache)
      ↓
Web Engine (PHP-FPM / Gunicorn)
      ↓
Application Code
```

**Example (PHP Application)**

```text
Browser → NGINX → PHP-FPM → Laravel Code
```

---

## 7. One-Line Summary

1. **Web Server** serves web content and handles HTTP
2. **Reverse Proxy** forwards and controls traffic
3. **Web Engine** executes application code

---
