# Node.js Dependencies & Dependency Resolution

In a Node.js project, **dependency management** has two parts:

1. **What dependencies exist** (`dependencies` vs `devDependencies`)
2. **How those dependencies are resolved and loaded**

Both are inseparable in real-world applications.

---

# Dependencies Section

There are **two main dependency sections** in `package.json`:


## 1. Classification

### 1.1 Production Dependencies - `dependencies`

- Packages **required to run the app in production**
- Needed when the server is actually running

Examples:

- `express`
- `mongoose`
- `cors`
- `dotenv`

```json
"dependencies": {
  "express": "^4.19.2",
  "mongoose": "^8.1.0"
}
```

These packages **must be resolvable at runtime**, or the app will crash.

#

### 1.2 Development Dependencies - `devDependencies`

- Packages **only needed during development**
- **NOT required in production runtime**

Examples:

- `nodemon` (auto-restart server)
- `eslint` (linting)
- `jest` (testing)
- `webpack`, `babel`

```json
"devDependencies": {
  "nodemon": "^3.0.2",
  "eslint": "^8.56.0"
}
```

These are **never resolved in production** because they are not installed.

---

## 2. How dependencies go into each section

When installing packages:

**2.1 Production dependency**

```bash
npm install express
```

→ Added to **`dependencies`**


**2.2 Development dependency**

```bash
npm install nodemon --save-dev
```

or

```bash
npm install nodemon -D
```

→ Added to **`devDependencies`**

**npm records:**
- Package name
- Version range
- Dependency type

---

## 3. What happens in production (Docker / Server)

```bash
npm install --production
```

or

```bash
NODE_ENV=production npm install
```

Result:

- **Only `dependencies` are installed**
- `devDependencies` are skipped
- Smaller image
- Faster startup
- Reduced attack surface

This directly affects **dependency resolution**, because missing packages **cannot be resolved at runtime**.

---

# Dependency Resolution

How does Node.js find the correct package when your code runs?

## 1. Files Involved in Resolution

### 1.1 `package.json`

- Declares **what is required**
- Uses **version ranges**

```json
"dependencies": {
  "express": "^4.19.2"
}
```

Meaning:
- Any version `>=4.19.2 <5.0.0` is acceptable

#

### 1.2 `package-lock.json` (critical)

- Locks **exact versions**
- Stores the full dependency tree
- Ensures identical installs everywhere

```json
"express": {
  "version": "4.19.2"
}
```

>npm **always follows `package-lock.json` first**

---

## 2. How npm resolves dependencies

### Step 1: Read dependency sections

- Reads `dependencies`
- Reads `devDependencies` (only if not production)

#

### Step 2: Read `package-lock.json`

- If exists → use locked versions
- If missing → fetch latest matching versions

#

### Step 3: Build dependency tree

Each dependency may have **its own dependencies**:

```text
your-app
 └─ express
     ├─ body-parser
     ├─ debug
     └─ cookie
```

npm resolves this **recursively**.

#

### Step 4: Handle version conflicts

If two packages need different versions:

```text
node_modules/
 ├─ lodash@4.x
 └─ some-lib/
     └─ node_modules/
         └─ lodash@3.x
```

Node.js fully supports **multiple versions**.

---

## 3. How Node.js resolves dependencies at runtime

When code executes:

```js
import express from 'express';
```

Node.js searches in this order:

1. `./node_modules/express`
2. `../node_modules/express`
3. `../../node_modules/express`
4. Stops at filesystem root

This is the **Node Module Resolution Algorithm**.

---

## 4. Why devDependencies don’t break production

- They are **not installed**
- They are **never resolved**
- They must **never be imported in runtime code**

Bad practice (breaks production):

```js
import nodemon from 'nodemon';
```

---

## 5. One-line mental model

- `dependencies` → must resolve at runtime
- `devDependencies` → exist only during development
- npm → installs & builds tree
- Node.js → resolves & loads at runtime

---


