# Nodemon - Node Monitor Process Manager

## What is Nodemon?

**Nodemon** is a development tool that automatically restarts your Node.js application when file changes are detected in the directory. The name comes from "Node Monitor".

---

## How It Works

Instead of running your app with `node app.js`, you run it with `nodemon app.js`. Nodemon then:
1. Starts your application
2. Watches files in your project directory
3. Automatically restarts the server when any watched file changes
4. Shows restart notifications in the terminal

---

## Why We Use Nodemon

### 1. **Automatic Restarts During Development**
No more manually stopping and restarting your server after every code change. This saves significant time and eliminates repetitive actions.

#

### 2. **Improved Development Workflow**
- Without nodemon
  ```bash
  $ node server.js
  ```
  >Make a change → Ctrl+C → node server.js again

- With nodemon
  ```bash
  $ nodemon server.js
  ```
  >Make a change → Server auto-restarts

#

### 3. **Flexible Configuration**
You can configure what to watch/ignore:
```json
// package.json or nodemon.json
{
  "watch": ["src", "config"],
  "ignore": ["tests/", "node_modules/"],
  "ext": "js,json,ts"
}
```

#

### 4. **Supports Various File Types**
Watches JavaScript, TypeScript, JSON files, and more by default.

#

### 5. **Easy to Install and Use**
```bash
# Install globally or as dev dependency
npm install -g nodemon
# or
npm install --save-dev nodemon

# Usage
nodemon server.js
```
---

## Common Use Cases
**When to use:** During coding on your local machine
- **API/Backend Development**: When building Express, Koa, or other Node.js servers
- **Full-stack Applications**: During backend development phases
- **Script Development**: When working on Node.js scripts that need frequent testing

---

## Basic Configuration Example

Create a `nodemon.json` file:
```json
{
  "watch": ["src"],
  "ext": "js,ts,json",
  "ignore": ["src/**/*.test.js"],
  "exec": "node"
}
```

---

## Alternative in package.json scripts:
```json
{
  "scripts": {
    "dev": "nodemon server.js",
    "start": "node server.js"
  }
}
```

---

## Alternatives
- **node-dev**: Similar functionality
- **ts-node-dev**: For TypeScript projects
- **PM2**: More feature-rich (production focus)

**Note**: Nodemon is primarily a **development tool**. It should not be used in production environments where process managers like **PM2** are more appropriate for stability and monitoring.

---
