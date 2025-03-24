# PM2

### **Table of Contents**

1. **Introduction to PM2**
2. **Installing PM2**
3. **Starting and Managing Applications with PM2**
   * Start an App
   * View Running Processes
   * Restart and Stop Apps
4. **Running Apps in the Background (Daemon Mode)**
5. **Auto-Restart on Server Reboot (PM2 Startup Script)**
6. **Logging and Monitoring with PM2**
7. **Scaling Applications with PM2**
8. **Conclusion**

***

### **1. Introduction to PM2**

PM2 (Process Manager 2) is a **process management tool** for Node.js applications. It helps to:\
✅ **Run applications in the background** (even after closing the terminal)\
✅ **Auto-restart apps** when they crash\
✅ **Monitor logs and performance**\
✅ **Scale apps** to multiple instances

***

### **2. Installing PM2**

Install PM2 globally using npm:

```bash
npm install -g pm2
```

***

### **3. Starting and Managing Applications with PM2**

#### **Start an App**

To start a Node.js application using PM2:

```bash
pm2 start app.js
```

#### **View Running Processes**

To check all running applications:

```bash
pm2 list
```

#### **Restart and Stop Apps**

*   Restart an app:

    ```bash
    pm2 restart app.js
    ```
*   Stop an app:

    ```bash
    pm2 stop app.js
    ```
*   Delete an app from PM2:

    ```bash
    pm2 delete app.js
    ```

***

### **4. Running Apps in the Background (Daemon Mode)**

If you close the terminal, the application will stop running. PM2 allows apps to run **in the background**:

```bash
pm2 start app.js
```

Now, even if you close the terminal, the app will **keep running**.

***

### **5. Auto-Restart on Server Reboot (PM2 Startup Script)**

By default, if the server restarts, the application will stop running. PM2 provides a startup script to **keep applications running after a server restart**:

```bash
pm2 startup
```

After running this command, PM2 will show instructions to run a system-specific command (e.g., for Ubuntu, it may look like this):

```bash
sudo env PATH=$PATH:/usr/bin pm2 startup systemd -u ubuntu --hp /home/ubuntu
```

Save the current PM2 process list to restart apps automatically:

```bash
pm2 save
```

Now, if the server restarts, the application will **automatically start**.

***

### **6. Logging and Monitoring with PM2**

#### **Check Logs**

To view logs of an application:

```bash
pm2 logs app
```

#### **Monitor Real-Time Performance**

PM2 provides a real-time dashboard for CPU, memory, and request monitoring:

```bash
pm2 monit
```

***

### **7. Scaling Applications with PM2**

To scale a Node.js application across multiple CPU cores, use:

```bash
pm2 scale app 4
```

This will start **4 instances** of `app.js`, improving performance.

***

### **8. Conclusion**

✔ **PM2** helps manage Node.js applications efficiently.\
✔ It provides **auto-restart, background execution, logging, and monitoring**.\
✔ **Scaling apps** with PM2 improves performance in production.

PM2 is **essential** for running and managing Node.js applications in production environments.
