# `npm` - Node Package Manager

## What is npm?

**npm (Node Package Manager)** is the **default package manager for Node.js**.
It is used to **install, manage, share, and run JavaScript libraries and tools** for Node.js projects.

In simple words:

- **Node.js** → runs JavaScript
- **npm** → manages the tools and libraries your JavaScript code uses

---

## Why `npm` Exists

**Without `npm`:**
- You must manually download libraries
- Manage versions yourself
- Copy files into projects
- Handle dependency conflicts manually

**With `npm`:**
- One command installs everything
- Versions are managed automatically
- Projects are reproducible
- Scripts can be automated

---

## What `npm` Actually Does

### 1. Installs Packages (Libraries & Tools)

```bash
npm install express
```

- Downloads Express
- Saves it in `node_modules/`
- Records it in `package.json`

#

### 2. Manages Dependencies

`npm` keeps track of:
- Which package depends on which
- Compatible versions
- Nested dependencies

Files involved:
- `package.json` → what your project needs
- `package-lock.json` → exact versions installed

#

### 3. Runs Project Scripts

```bash
npm run dev
npm start
npm test
```

Scripts are defined in **package.json**.

Example:

```json
{
  "scripts": {
    "dev": "nodemon app.js",
    "start": "node app.js"
  }
}
```

#

### 4. Publishes Packages

Developers can:
- Create libraries
- Publish them to npm registry
- Share them globally

Example:

```bash
npm publish
```

---

## Key `npm` Files Explained

### 1. package.json

The **heart of a Node.js project**.

**Contains:**
- Project name & version
- Dependencies
- Scripts
- Metadata

**Example:**

```json
{
  "name": "blog-app",
  "version": "1.0.0",
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

#

### 2. node_modules/

- Folder where all installed packages live
- Can be very large
- Never commit to Git

#

### 3. package-lock.json

- Exact dependency tree
- Ensures same versions across machines
- Critical for production consistency

---

## Local vs Global Packages

### Local (Project-specific)

```bash
npm install express
```

- Installed inside project
- Used only by that project

#

### Global (System-wide tools)

```bash
npm install -g nodemon
npm install -g pm2
```

- CLI tools
- Available system-wide

---

## Common `npm` Commands

```bash
npm init -y           # Create package.json
npm install           # Install all dependencies
npm install express   # Install a package
npm uninstall express # Remove a package
npm update            # Update packages
npm run dev           # Run a script
npm start             # Start app
```

---

## `npm` vs `npx`

| Topic              | npm              | npx          |
| ------------------ | ---------------- | ------------ |
| Purpose            | Install packages | Run packages |
| Installs globally  | Yes              | No           |
| One-time execution | No               | Yes          |

**Example:**

```bash
npx create-react-app myapp
```

No global install needed.

---

## `npm` in Development vs Production


**Development dependencies**
```bash
npm install nodemon --save-dev
```
Packages **only needed during development** saves into `devDependencies` in `package.json`.

**Production install**
```bash
npm install --production
```

Packages **required to run the app in production** saves into `Dependencies` in `package.json`.

---

## `npm` in Real Projects

You will use npm to:
- Initialize project
- Install Express, Mongoose, JWT
- Install Nodemon (dev)
- Install PM2 (prod)
- Run scripts

---

## Common Misconceptions

- npm ≠ Node.js
- npm ≠ JavaScript
- npm does not run code by itself
- npm does not improve performance

---
