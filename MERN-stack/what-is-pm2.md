# PM2 - Process Manager 2

## What is PM2?

**PM2** is a production-grade process manager for Node.js applications. It's like a "supervisor" for your Node.js apps that keeps them alive forever, reloads them without downtime, and helps you manage and monitor your applications.

## Why We Use PM2

### **1. Keeps Your Application Alive Forever**
```bash
# Without PM2
$ node app.js
# App crashes → Website goes down
# Server restarts → App doesn't start automatically

# With PM2
$ pm2 start app.js
# App crashes → PM2 automatically restarts it (like magic!)
# Server restarts → PM2 starts your app automatically
```

### **2. Zero-Downtime Reloads**
```bash
# Update your code without stopping the server
pm2 reload app.js
# New instances start → Old ones finish current requests → Smooth transition
# Users don't see "Website Under Maintenance" errors
```

### **3. Load Balancing (Clustering)**
```javascript
// Use all your CPU cores automatically
pm2 start app.js -i max
// Creates multiple instances of your app
// Distributes traffic across all CPU cores
// Handles more users simultaneously
```

### **4. Monitoring & Logging**
```bash
# Real-time dashboard
pm2 monit

# View logs
pm2 logs
pm2 logs app-name --lines 100  # Last 100 lines

# Separate error logs
pm2 logs --err

# Log rotation (prevents disk filling up)
pm2 install pm2-logrotate
```

### **5. Startup Management**
```bash
# Make your app start automatically on server reboot
pm2 startup
pm2 save
# After server restart, your app comes back online automatically
```

## Key Features in Detail

### **1. Process Management**
```bash
# Start an application
pm2 start app.js
pm2 start app.js --name "my-api"  # Give it a custom name

# List all running applications
pm2 list
pm2 status

# Stop, restart, delete
pm2 stop my-api
pm2 restart my-api
pm2 delete my-api

# Stop all applications
pm2 stop all
```

### **2. Advanced Configuration (ecosystem.config.js)**
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

### **3. Monitoring Features**
```bash
# Show resource usage (CPU, memory)
pm2 show app-name

# Monitor in real-time
pm2 monit

# Generate metrics
pm2 metrics

# Generate startup script
pm2 startup
```

### **4. Log Management**
```bash
# View logs in real-time
pm2 logs

# View specific app logs
pm2 logs app-name

# Flush all logs
pm2 flush

# Reload logs
pm2 reloadLogs
```

## When to Use PM2

### **1. Production Scenarios:**

1. **Web Servers** (Express, Koa, Fastify)
2. **API Backends**
3. **Microservices**
4. **WebSocket Servers**
5. **Scheduled Tasks/Cron Jobs**
6. **Any long-running Node.js process**

### **2. Development vs Production:**
```bash
# Development (local machine)
npm run dev       # Uses nodemon for auto-restart on changes

# Production (server)
npm start         # Uses PM2 for process management
```

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

## Real-World Example

### **Deploying a Node.js API:**
```bash
# 1. Transfer code to server
scp -r my-app user@server:/home/user/

# 2. Install dependencies
npm install --production

# 3. Start with PM2
pm2 start ecosystem.config.js --env production

# 4. Save configuration
pm2 save

# 5. Setup auto-start on reboot
pm2 startup
```

### **Common PM2 Commands Cheat Sheet:**
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

## Summary

- **Node.js** = The engine that runs your JavaScript code
- **PM2** = The mechanic/manager that keeps multiple engines running smoothly
