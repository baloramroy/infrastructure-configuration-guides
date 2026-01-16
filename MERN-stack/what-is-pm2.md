## What is PM2?

**PM2** (Process Manager 2) is a production-grade process manager for Node.js applications. It's like a "supervisor" for your Node.js apps that keeps them alive forever, reloads them without downtime, and helps you manage and monitor your applications.

---

## Why We Use PM2

### **1. Keeps Your Application Alive Forever**

**Without PM2**
```bash
$ node app.js
```
App crashes → Website goes down\
Server restarts → App doesn't start automatically

**With PM2**

```bash
$ pm2 start app.js
```
App crashes → PM2 automatically restarts it (like magic!)\
Server restarts → PM2 starts your app automatically

#

### **2. Zero-Downtime Reloads**

**Update your code without stopping the server**
```bash
pm2 reload app.js
```
What happens internally:

- New instances start
- Old instances finish current requests
- Traffic switches smoothly
- No service interruption

Users **never see downtime**.

#

### **3. Load Balancing (Clustering)**
```javascript
pm2 start app.js -i max
```
- Use all your CPU cores automatically
- Creates multiple instances of your app
- Distributes traffic across all CPU cores
- Handles more users simultaneously

#

### **4. Monitoring & Logging**

**Real-time dashboard**
```bash
pm2 monit
```

**View logs**
```bash
pm2 logs
```

#

### **5. Startup Management**

**Make your app start automatically on server reboot**
```bash
pm2 startup
pm2 save
```
Result:
- Server reboot → PM2 starts automatically
- All saved apps come back online

---

## Key Features in Detail

### **1. Process Management Features**

**Start an application**

```bash
pm2 start app.js
pm2 start app.js --name "my-api"  # Give it a custom name
```

**List all running applications**
```bash
pm2 list
pm2 status
```

**Stop, restart, delete**
```bash
pm2 stop my-api
pm2 restart my-api
pm2 delete my-api
```

**Stop all applications**
```bash
pm2 stop all
```

#

### **2. Advanced Configuration Features (ecosystem.config.js)**
```javascript
module.exports = {
  apps: [{
    name: 'my-app',           // Application name
    script: './app.js',       // Entry point
    instances: 4,             // Number of instances (or 'max' for all CPUs)
    exec_mode: 'cluster',     // Enable clustering
    watch: false,             // Don't watch for changes in production
    env: {
      NODE_ENV: 'development',
      PORT: 3000
    },
    env_production: {
      NODE_ENV: 'production',
      PORT: 80
    },
    error_file: 'logs/err.log',    // Error logs
    out_file: 'logs/out.log',      // Output logs
    log_date_format: 'YYYY-MM-DD HH:mm:ss',
    max_memory_restart: '1G'       // Restart if memory exceeds 1GB
  }]
};

// Start with: pm2 start ecosystem.config.js --env production
```

#

### **3. Monitoring Features**

**Show resource usage (CPU, memory)**
```bash
pm2 show app-name
```

**Monitor in real-time**
```bash
pm2 monit
```

**Generate metrics**
```bash
pm2 metrics
```

**Generate startup script**
```bash
pm2 startup
```

#

### **4. Log Management Features**

**View logs in real-time**

```bash
pm2 logs
```

**View specific app logs**
```bash
pm2 logs app-name
```

**Flush all logs**
```bash
pm2 flush
```

**Reload logs**
```bash
pm2 reloadLogs
```
**Last 100 lines**
```bash
pm2 logs app-name --lines 100
```

**Separate error logs**
```bash
pm2 logs --err
```

**Log rotation (prevents disk filling up)**
```bash
pm2 install pm2-logrotate
```

---

## When to Use PM2

### **1. Production Scenarios:**

1. **Web Servers** (Express, Koa, Fastify)
2. **API Backends**
3. **Microservices**
4. **WebSocket Servers**
5. **Scheduled Tasks/Cron Jobs**
6. **Any long-running Node.js process**

#

### **2. Development vs Production:**

- Development (local machine)
  ```bash
  npm run dev
  ```
  >Uses nodemon for auto-restart on changes

- Production (server)
  ```bash
  npm start
  ```
  >Uses PM2 for process management

---

## Installation & Basic Usage

```bash
# Install globally
npm install pm2 -g

# Start your application
pm2 start app.js

# Save current process list
pm2 save

# Generate startup script
pm2 startup

# To make PM2 start on boot (Linux)
pm2 startup systemd
sudo env PATH=$PATH:/usr/bin pm2 startup systemd -u username --hp /home/username
```

---

## Deploying a Node.js API

**Transfer code to server**
```bash
scp -r my-app user@server:/home/user/
```

**Install dependencies**
```bash
npm install --production
```

**Start with PM2**
```bash
pm2 start ecosystem.config.js --env production
```

**Save configuration**

```bash
pm2 save
```

**Setup auto-start on reboot**
```bash
pm2 startup
```

---

## Common PM2 Commands Cheat Sheet
```bash
pm2 start app.js              # Start application
pm2 start app.js -i 4         # Start 4 instances
pm2 restart app-name          # Restart application
pm2 reload app-name           # Zero-downtime reload
pm2 stop app-name             # Stop application
pm2 delete app-name           # Remove from PM2
pm2 list                      # List all processes
pm2 logs                      # Show logs
pm2 monit                     # Monitor dashboard
pm2 update                    # Update PM2
pm2 ping                      # Check if PM2 is running
```

---

## Why Not Just Use Node Directly?

**Without PM2:**
- App crashes = Website down
- Need manual restart after updates
- No load balancing
- Hard to manage multiple apps
- No built-in monitoring
- Logs can fill up disk space

**With PM2:**
- 24/7 uptime (auto-recovery)
- Smooth updates (no downtime)
- Automatic load balancing
- Easy multi-app management
- Built-in monitoring dashboard
- Professional log management

**PM2 is essential** for any serious Node.js production deployment because it turns your simple Node.js script into a robust, enterprise-ready service that can handle real-world production demands.

---

## Summary

- **Node.js** = The engine that runs your JavaScript code
- **PM2** = The mechanic/manager that keeps multiple engines running smoothly

---
