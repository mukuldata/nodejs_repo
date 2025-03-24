# Deploying Node JS App

### **Table of Contents**

1. **Introduction to Deployment**
2. **Choosing a Hosting Provider**
3. **Deploying on DigitalOcean (Droplet)**
4. **Deploying on AWS (EC2 Instance)**
5. **Deploying on Render**
6. **Conclusion**

***

### **1. Introduction to Deployment**

1. Deployment is the process of **making your Node.js application live on the internet** so that users can access it.&#x20;
2. There are multiple ways to deploy an application, including **Virtual Private Servers (VPS), cloud providers, and serverless platforms**.

**Common Deployment Platforms:**\
✔ **DigitalOcean** – Provides VPS (Droplets) for full control.\
✔ **AWS (Amazon Web Services)** – Offers EC2 instances for scalable hosting.\
✔ **Render** – Simple and free hosting for small projects.

***

### **2. Choosing a Hosting Provider**

| Provider         | Type       | Best For                       | Free Plan?                |
| ---------------- | ---------- | ------------------------------ | ------------------------- |
| **DigitalOcean** | VPS        | Full control over the server   | No (Basic plan: $4/month) |
| **AWS EC2**      | Cloud VM   | Scalable applications          | Yes (for 12 months)       |
| **Render**       | Full-stack | Easy deployment with free plan | Yes                       |

***

### **3. Deploying on DigitalOcean (Droplet)**

#### **Step 1: Create a Droplet**

1. Sign up at [DigitalOcean](https://www.digitalocean.com/).
2. Create a **Droplet** with Ubuntu.
3. Note down the **Droplet IP Address**.

#### **Step 2: Connect via SSH**

On your local machine, run:

```bash
ssh root@your_droplet_ip
```

#### **Step 3: Install Node.js**

Inside the server:

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | bash -
sudo apt install -y nodejs
```

#### **Step 4: Deploy Your App**

1. Upload your Node.js project using Git:

```bash
git clone your_repo_url
cd your_project
npm install
```

2. Start your app with **PM2** (to keep it running):

```bash
npm install -g pm2
pm2 start index.js
```

#### **Step 5: Configure Nginx (Optional for Domain & Load Balancing)**

1. Install Nginx:

```bash
sudo apt install nginx
```

2. Configure **Nginx as a reverse proxy** to forward traffic to your Node.js app.

***

### **4. Deploying on AWS (EC2 Instance)**

#### **Step 1: Launch an EC2 Instance**

1. Go to [AWS EC2 Console](https://aws.amazon.com/ec2/).
2. Create an instance (Ubuntu).
3. Download the **.pem key file**.

#### **Step 2: Connect to EC2 via SSH**

Run:

```bash
ssh -i your_key.pem ubuntu@your_ec2_public_ip
```

#### **Step 3: Install Node.js and Deploy Your App**

Follow the same steps as DigitalOcean (install Node.js, clone repo, install dependencies, and start app with PM2).

#### **Step 4: Configure Security Group (Allow Traffic)**

1. Go to **AWS Security Groups**.
2. Open **port 80 (HTTP)** and **port 443 (HTTPS)** for web access

***

### **5. Deploying on Render**

#### **Step 1: Sign Up on Render**

Go to [Render](https://render.com/) and create an account.

#### **Step 2: Create a New Web Service**

1. Click on **New Web Service**.
2. Connect your GitHub repository.

#### **Step 3: Configure Deployment**

*   Set **Build Command**:

    ```bash
    npm install
    ```
*   Set **Start Command**:

    ```bash
    node index.js
    ```

#### **Step 4: Deploy & Get Live URL**

Render will automatically build and deploy your app. You will get a URL like:

```
https://your-app.onrender.com
```

***

### **6. Conclusion**

✅ **DigitalOcean** – Best for full control using VPS (manual setup).\
✅ **AWS EC2** – Scalable cloud hosting (good for production).\
✅ **Render** – Free and simple full-stack hosting.

For **quick deployment**,  **Render** is the best.\
For **full control**, go with **DigitalOcean** or **AWS EC2**.&#x20;
