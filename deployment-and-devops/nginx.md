# Nginx

### **Table of Contents**

1. **Introduction to Reverse Proxy**
2. **Why Use Nginx as a Reverse Proxy?**
3. **Installing Nginx**
4. **Basic Reverse Proxy Configuration**
5. **Reverse Proxy for a Node.js App**
6. **SSL Configuration with Let's Encrypt**
7. **Load Balancing with Nginx**
8. **Conclusion**

***

### **1. Introduction to Reverse Proxy**

1. A **reverse proxy** is a server that sits between clients (users) and backend servers.
2. &#x20;Instead of users directly accessing a web server, they make requests to the **reverse proxy**, which then forwards them to the actual backend server and returns the response.

#### **How It Works?**

* A client (browser) sends a request to `yourdomain.com`.
* The **Nginx reverse proxy** forwards this request to the backend server (e.g., a Node.js app running on `localhost:3000`).
* The backend server processes the request and sends a response.
* The reverse proxy sends this response back to the client.

***

### **2. Why Use Nginx as a Reverse Proxy?**

✔ **Security:** Hides the backend server details from users.\
✔ **Load Balancing:** Distributes traffic across multiple backend servers.\
✔ **SSL Termination:** Handles HTTPS encryption efficiently.\
✔ **Caching:** Improves performance by storing static responses.\
✔ **Compression:** Reduces data size for faster transmission.

***

### **3. Installing Nginx**

#### **For Ubuntu/Debian:**

```bash
sudo apt update
sudo apt install nginx -y
```

#### **For CentOS/RHEL:**

```bash
sudo yum install epel-release -y
sudo yum install nginx -y
```

#### **Start & Enable Nginx:**

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

***

### **4. Basic Reverse Proxy Configuration**

Nginx configuration files are stored in `/etc/nginx/sites-available/` (Ubuntu) or `/etc/nginx/nginx.conf` (CentOS).

#### **Create a Configuration File**

```bash
sudo nano /etc/nginx/sites-available/myapp
```

#### **Add Reverse Proxy Settings**

```nginx
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

#### **Enable the Configuration (Ubuntu/Debian only)**

```bash
sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/
```

#### **Restart Nginx**

```bash
sudo systemctl restart nginx
```

***

### **5. Reverse Proxy for a Node.js App**

#### **Step 1: Start a Node.js App on Port 3000**

Create a simple Node.js server (`server.js`):

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.send('Hello from Node.js server!');
});

app.listen(3000, () => {
    console.log('Server running on port 3000');
});
```

Run the server:

```bash
node server.js
```

#### **Step 2: Configure Nginx**

Ensure your Nginx configuration is correctly set up to forward traffic to `http://localhost:3000`.

#### **Step 3: Test the Setup**

Now, visit `http://yourdomain.com`, and you should see:

```
Hello from Node.js server!
```

***

### **6. SSL Configuration with Let's Encrypt**

To enable HTTPS for free using **Let's Encrypt**, install **Certbot**:

#### **Install Certbot & Generate SSL Certificate**

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
```

#### **Auto-Renew SSL Certificate**

```bash
sudo certbot renew --dry-run
```

***

### **7. Load Balancing with Nginx**

If you have multiple Node.js instances, you can distribute traffic among them using **Nginx Load Balancing**.

#### **Update Nginx Configuration**

```nginx
upstream backend_servers {
    server localhost:3000;
    server localhost:3001;
}

server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://backend_servers;
    }
}
```

This setup distributes traffic between servers running on ports **3000 and 3001**, improving performance and fault tolerance.

***

### **8. Conclusion**

✅ Nginx **reverse proxy** improves security, performance, and scalability.\
✅ It **forwards requests** from users to backend servers like **Node.js**.\
✅ **SSL with Let's Encrypt** provides free HTTPS security.\
✅ **Load balancing** helps handle more traffic efficiently.
