# Nodemon - Node Monitor Process Manager

## What is Nodemon?

**Nodemon** is a development-time utility for **Node.js** that automatically restarts your application whenever source code changes are detected.

It is mainly used during **development**, not in **production**.

---

## Why Nodemon is Used

### Without Nodemon
When you run a Node.js app normally:

```bash
node app.js
```

- You change code
- App does **not** reload
- You must stop and restart manually

This slows down development.

### With Nodemon:

```bash
nodemon app.js
```

- You change code
- Nodemon detects file changes
- App restarts automatically
- Faster development cycle

---

## How It Works

1. Nodemon **watches your project files**
2. When a file changes:
   - It stops the running Node process
   - Restarts it automatically
3. Your app runs with the updated code

✅Nodemon does **not** improve performance\
✅It only improves **developer productivity**

---

## Installation

### Local (Recommended)

```bash
npm install --save-dev nodemon
```

### Global (Optional)

```bash
npm install -g nodemon
```

---

## Common Use Cases

### When to use:

**During coding on your local machine**
- **API/Backend Development**: When building Express, Koa, or other Node.js servers
- **Full-stack Applications**: During backend development phases
- **Script Development**: When working on Node.js scripts that need frequent testing


### When Not to Use Nodemon

- In production servers
- With PM2
- For performance tuning
- For long-running background jobs

---

## Usage

### Basic

```bash
nodemon app.js
```

### With npm script

Edit **package.json** file and input this

```json
{
  "scripts": {
    "dev": "nodemon app.js",
    "start": "node app.js"
  }
}
```

Run:

```bash
npm run dev
```

---

## Common Nodemon Options

```bash
nodemon app.js --ext js,json
nodemon app.js --ignore node_modules
```

**Using config file:**

Edit **nodemon.json** file

```json
{
  "watch": ["src"],
  "ext": "js,json",
  "ignore": ["node_modules"],
  "exec": "node app.js"
}
```

---

## Alternatives
- **node-dev**: Similar functionality
- **ts-node-dev**: For TypeScript projects
- **PM2**: More feature-rich (production focus)

**Note**: Nodemon is primarily a **development tool**. It should not be used in production environments where process managers like **PM2** are more appropriate for stability and monitoring.

---
