# CI/CD for Node JS App

### **Table of Contents**

1. **Introduction to CI/CD**
2. **Why Use CI/CD for Node.js?**
3. **CI/CD Workflow Explained**
4. **Setting Up GitHub Actions for CI/CD**
5. **Setting Up Jenkins for CI/CD**
6. **Automated Testing in CI/CD**
7. **Deploying to Production with CI/CD**
8. **Conclusion**

***

### **1. Introduction to CI/CD**

#### **What is CI/CD?**

CI/CD stands for:

* **Continuous Integration (CI):** Automates code testing and merging.
* **Continuous Deployment/Delivery (CD):** Automates deployment to production or staging environments.

#### **How It Works?**

Whenever a developer pushes code to GitHub:

1. The **CI system** (GitHub Actions/Jenkins) **runs tests**.
2. If tests pass, the **CD system deploys the code** to production/staging.

***

### **2. Why Use CI/CD for Node.js?**

✔ **Automates Testing:** Ensures every code change is tested before deployment.\
✔ **Reduces Errors:** Catch bugs early by running automated tests.\
✔ **Faster Releases:** Automate deployments to speed up the release cycle.\
✔ **Better Collaboration:** Developers can merge changes more frequently.

***

### **3. CI/CD Workflow Explained**

A **CI/CD pipeline** includes these steps:

1. **Code Commit:** Developer pushes code to GitHub/GitLab.
2. **Build Step:** Installs dependencies (`npm install`).
3. **Test Step:** Runs automated tests (`npm test`).
4. **Deploy Step:** If tests pass, the code is deployed to a staging/production server.

***

### **4. Setting Up GitHub Actions for CI/CD**

#### **Step 1: Create a GitHub Actions Workflow**

Inside your Node.js project, create a file:

```
.github/workflows/ci-cd.yml
```

#### **Step 2: Add CI/CD Configuration**

```yaml
name: Node.js CI/CD

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3

      - name: Install Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Install Dependencies
        run: npm install

      - name: Run Tests
        run: npm test

      - name: Deploy to Server
        if: success()
        run: |
          ssh user@yourserver 'cd /path/to/project && git pull && npm install && pm2 restart app'
```

#### **Step 3: Push Code to GitHub**

Once you push your code, GitHub Actions will:\
✅ Install dependencies\
✅ Run tests\
✅ Deploy to the server (if tests pass)

***

### **5. Setting Up Jenkins for CI/CD**

#### **Step 1: Install Jenkins**

On an Ubuntu server, run:

```bash
sudo apt update
sudo apt install openjdk-11-jdk -y
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

#### **Step 2: Access Jenkins**

Visit: `http://your-server-ip:8080`

Use the initial admin password from:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

#### **Step 3: Create a CI/CD Pipeline in Jenkins**

1. Click **New Item** → **Pipeline**.
2. In the **Pipeline Script**, add:

```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-repo.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }
        stage('Deploy') {
            steps {
                sshagent(['your-ssh-key']) {
                    sh 'ssh user@yourserver "cd /path/to/project && git pull && npm install && pm2 restart app"'
                }
            }
        }
    }
}
```

#### **Step 4: Save and Run the Pipeline**

Whenever you push code, Jenkins will:\
✅ Pull the latest changes\
✅ Install dependencies\
✅ Run tests\
✅ Deploy to the server

***

### **6. Automated Testing in CI/CD**

Testing ensures that broken code is not deployed.

#### **Example: Jest Test for a Node.js App**

Install Jest:

```bash
npm install --save-dev jest
```

Add a test (`sum.test.js`):

```javascript
const sum = (a, b) => a + b;

test('adds 2 + 3 to equal 5', () => {
  expect(sum(2, 3)).toBe(5);
});
```

Run tests:

```bash
npm test
```

Both **GitHub Actions** and **Jenkins** will fail the pipeline if tests fail.

***

### **7. Deploying to Production with CI/CD**

#### **Deployment Strategies:**

1. **Rolling Deployment:** Deploys changes gradually across servers.
2. **Blue-Green Deployment:** Two environments (one active, one on standby).
3. **Zero-Downtime Deployment:** Uses **PM2** to keep the app running.

#### **Example: Restarting Node.js App After Deployment**

```bash
ssh user@yourserver 'cd /path/to/project && git pull && npm install && pm2 restart app'
```

***

### **8. Conclusion**

✅ **CI/CD automates testing & deployment** to ensure smooth releases.\
✅ **GitHub Actions** provides an easy way to set up a CI/CD pipeline.\
✅ **Jenkins** offers more flexibility for enterprise environments.\
✅ **Automated testing** ensures only quality code is deployed.\
✅ **Deployment strategies** help avoid downtime.
