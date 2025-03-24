# Scaling Node JS App

### **Table of Contents**

1. **Introduction to Scaling**
2. **What is Load Balancing?**
3. **Types of Load Balancers**
4. **Using Nginx as a Load Balancer**
5. **Using Node.js Cluster Module for Scaling**
6. **Horizontal vs Vertical Scaling**
7. **Load Balancing with Cloud Providers (AWS, GCP, Azure)**
8. **Conclusion**

***

### **1. Introduction to Scaling**

When an application gets **high traffic**, a single Node.js server **cannot handle too many requests**.\
Scaling helps distribute requests **across multiple servers**, ensuring:

✔ **Faster response times**\
✔ **Better performance**\
✔ **No single point of failure**

***

### **2. What is Load Balancing?**

A **Load Balancer** is a system that:

* **Distributes traffic** across multiple servers.
* Ensures **no single server is overloaded**.
* Improves **reliability** and **scalability**.

#### **Example Without Load Balancer**

If one server gets 10,000 requests, it might **slow down** or **crash**.

#### **Example With Load Balancer**

Traffic is split between multiple servers:

```
Client → Load Balancer → Server 1  
                       → Server 2  
                       → Server 3  
```

This way, each server handles fewer requests, **improving performance**.

***

### **3. Types of Load Balancers**

#### **1. Software Load Balancers**

* Example: **Nginx, HAProxy**
* Runs on a server and distributes traffic to backend servers.

#### **2. Hardware Load Balancers**

* Expensive physical devices used in **enterprise setups**.

#### **3. Cloud Load Balancers**

* Provided by **AWS, GCP, Azure**.
* Example: **AWS Elastic Load Balancer (ELB)**.

***

### **4. Using Nginx as a Load Balancer**

Nginx is a popular **software load balancer**.

#### **Steps to Set Up Nginx Load Balancer**

**1. Install Nginx**

For Linux (Ubuntu):

```bash
sudo apt update
sudo apt install nginx -y
```

**2. Configure Nginx as a Load Balancer**

Edit the Nginx config file:

```bash
sudo nano /etc/nginx/nginx.conf
```

Add the following configuration:

```nginx
http {
    upstream node_servers {
        server 127.0.0.1:3000;
        server 127.0.0.1:3001;
        server 127.0.0.1:3002;
    }

    server {
        listen 80;
        location / {
            proxy_pass http://node_servers;
        }
    }
}
```

**3. Restart Nginx**

```bash
sudo systemctl restart nginx
```

Now, Nginx will distribute traffic among **three Node.js servers**.

***

### **5. Using Node.js Cluster Module for Scaling**

Node.js **runs on a single thread**, meaning it can only use **one CPU core**.\
The **Cluster module** allows **using multiple CPU cores**.

#### **Example: Clustering in Node.js**

```javascript
const cluster = require("cluster");
const http = require("http");
const os = require("os");

if (cluster.isMaster) {
    const numCPUs = os.cpus().length;
    console.log(`Forking ${numCPUs} workers...`);
    for (let i = 0; i < numCPUs; i++) {
        cluster.fork();
    }
} else {
    http.createServer((req, res) => {
        res.writeHead(200);
        res.end("Hello from Worker!");
    }).listen(3000);

    console.log(`Worker ${process.pid} started`);
}
```

#### **How it Works?**

* **Master process** creates multiple **worker processes**.
* Each worker handles requests, making use of **all CPU cores**.

***

### **6. Horizontal vs Vertical Scaling**

#### **1. Vertical Scaling** (Upgrading Server)

✔ Increase CPU, RAM of a **single server**.\
✔ Simple but **limited by hardware**.

#### **2. Horizontal Scaling** (Adding More Servers)

✔ Add **multiple servers** and use a **load balancer**.\
✔ **Better for large-scale applications**.

**Example:**\
Instead of **one powerful server**, use **10 smaller servers** with a load balancer.

***

### **7. Load Balancing with Cloud Providers**

Cloud providers offer **managed load balancers**, such as:

✔ **AWS Elastic Load Balancer (ELB)**\
✔ **Google Cloud Load Balancer**\
✔ **Azure Load Balancer**

These automatically distribute traffic **across multiple servers** in the cloud.

#### **Example: AWS Load Balancer Setup**

1. Create multiple EC2 instances running Node.js.
2. Use **AWS Elastic Load Balancer (ELB)** to distribute traffic.
3. Users now connect to the **ELB**, which routes traffic to the instances.

***

### **8. Conclusion**

✅ Load balancing helps scale Node.js apps by distributing traffic.\
✅ Nginx is a simple and powerful software load balancer.\
✅ The Cluster module allows Node.js to use multiple CPU cores.\
✅ Cloud providers offer managed load balancing solutions.\
✅ Horizontal scaling with multiple servers is the best long-term solution.
