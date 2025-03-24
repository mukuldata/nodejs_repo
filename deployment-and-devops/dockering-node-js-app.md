# Dockering Node JS App

### **Table of Contents**

1. **Introduction to Docker**
2. **Why Dockerize a Node.js App?**
3. **Installing Docker**
4. **Writing a Dockerfile for Node.js**
5. **Building and Running a Docker Container**
6. **Using Docker Compose for Multi-Container Apps**
7. **Persisting Data with Docker Volumes**
8. **Deploying a Dockerized Node.js App**
9. **Conclusion**

***

### **1. Introduction to Docker**

#### **What is Docker?**

1. Docker is a tool that allows you to package applications and their dependencies into **containers**. These containers can run on any system without worrying about missing dependencies or configuration issues.

#### **How It Works?**

* Instead of installing Node.js and dependencies manually, everything is **packaged inside a container**.
* A container includes:
  * The **Node.js runtime**
  * The **application code**
  * All required **dependencies**

***

### **2. Why Dockerize a Node.js App?**

✔ **Works on Any System:** No need to worry about OS differences.\
✔ **Lightweight & Fast:** Unlike virtual machines, containers use fewer resources.\
✔ **Easy to Deploy:** The same container can run on your laptop, staging, and production servers.\
✔ **Isolated Environment:** Prevents conflicts with other apps on the same machine.

***

### **3. Installing Docker**

#### **Windows & Mac:**

Download and install **Docker Desktop** from [Docker’s website](https://www.docker.com/get-started).

#### **Linux (Ubuntu Example):**

```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```

Check installation:

```bash
docker --version
```

***

### **4. Writing a Dockerfile for Node.js**

A **Dockerfile** is a script that defines how to build a Docker image for your Node.js app.

#### **Example: Dockerfile**

Create a file named **Dockerfile** in your project folder:

```dockerfile
# Use an official Node.js image
FROM node:18

# Set the working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Expose the port the app runs on
EXPOSE 3000

# Command to start the app
CMD ["node", "server.js"]
```

***

### **5. Building and Running a Docker Container**

#### **Step 1: Build the Docker Image**

Run this command in the project directory:

```bash
docker build -t my-node-app .
```

This will create an image named **my-node-app**.

#### **Step 2: Run the Container**

```bash
docker run -p 3000:3000 my-node-app
```

Now, your Node.js app is running inside a container!&#x20;

***

### **6. Using Docker Compose for Multi-Container Apps**

If your app uses a **database (MongoDB, MySQL)**, you can define multiple containers using **Docker Compose**.

#### **Example: docker-compose.yml**

Create a **docker-compose.yml** file:

```yaml
version: '3'
services:
  app:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - db

  db:
    image: mongo
    ports:
      - "27017:27017"
```

Now run:

```bash
docker-compose up
```

This will start **both Node.js and MongoDB containers** together.

***

### **7. Persisting Data with Docker Volumes**

By default, Docker containers do not persist data after stopping.

#### **Solution: Use Volumes**

Modify `docker-compose.yml`:

```yaml
  db:
    image: mongo
    ports:
      - "27017:27017"
    volumes:
      - mongodb_data:/data/db

volumes:
  mongodb_data:
```

This ensures that MongoDB data is **saved permanently** on the host machine.

***

### **8. Deploying a Dockerized Node.js App**

#### **Step 1: Push Image to Docker Hub**

```bash
docker tag my-node-app your-dockerhub-username/my-node-app
docker push your-dockerhub-username/my-node-app
```

#### **Step 2: Run on Any Server**

On a new server, install Docker and run:

```bash
docker pull your-dockerhub-username/my-node-app
docker run -p 3000:3000 your-dockerhub-username/my-node-app
```

Now, your app is running in production! 🚀

***

### **9. Conclusion**

✅ **Docker makes Node.js apps portable** and easy to deploy.\
✅ **A Dockerfile defines how to build the app image.**\
✅ **Docker Compose helps manage multi-container apps (Node.js + DB).**\
✅ **Volumes help store persistent data.**\
✅ **You can deploy the app anywhere by pushing the image to Docker Hub.**
