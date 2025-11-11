# node-app-deployed-using-jenkins

# 🚀 Node.js Application Deployment using Jenkins Freestyle Project

This project demonstrates how to **deploy a Node.js web application** automatically on an **AWS EC2 (Ubuntu)** server using **Jenkins Freestyle Project**.

---

## 🧩 Project Overview

- **Goal:** Automate build and deployment of a Node.js app via Jenkins  
- **Tool:** Jenkins (Freestyle Project)  
- **Server:** Ubuntu EC2 instance  
- **Language:** Node.js  
- **Deployment:** Local deployment on Jenkins host  

---

## 🧰 Prerequisites

Before you begin, ensure you have:

| Component | Description |
|------------|-------------|
| 🖥️ **Ubuntu EC2** | Running instance with public IP |
| ⚙️ **Node.js & npm** | Installed for building and running the app |
| 🔧 **Jenkins** | Installed and running on port `8080` |
| 🧾 **Git** | Installed to pull source code |
| 🔐 **Jenkins Permissions** | Jenkins user added to `sudoers` with NOPASSWD |
| 💡 **App Files** | Your Node.js project with `package.json` and `app.js` or `server.js` |

---

## ⚙️ Step 1: Install Node.js and Git

sudo apt update -y
sudo apt install -y nodejs npm git
node -v
npm -v
git --version


---

## ⚙️ Step 2: Install Jenkins

sudo apt update
sudo apt install -y openjdk-17-jdk
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee
/usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]
https://pkg.jenkins.io/debian-stable binary/ | sudo tee
/etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install -y jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins


Access Jenkins at:

👉 `http://<your-ec2-public-ip>:8080`

---

## 🧑‍💻 Step 3: Unlock Jenkins and Install Plugins

Get the initial admin password:

sudo cat /var/lib/jenkins/secrets/initialAdminPassword


1. Login to Jenkins in your browser
2. Install Suggested Plugins
3. Create your admin user

---

## 🔐 Step 4: Configure Jenkins Permissions

Give the Jenkins user passwordless sudo access:

sudo visudo


Add the following lines:

jenkins ALL=(ALL) NOPASSWD:ALL
Defaults:jenkins !requiretty


Restart Jenkins:
sudo systemctl restart jenkins


https://github.com/<your-username>/<your-nodejs-repo>.git


---

## 🧱 Step 7: Add Build Steps

### 🧩 Step 1: Install Dependencies

Add build step → Execute Shell
echo "Installing Node.js dependencies..."
cd $WORKSPACE
sudo npm install


### 🧩 Step 2: Start Application

Add another Execute Shell step:

echo "Starting Node.js Application..."
sudo pkill -f "node" || true
sudo nohup node app.js > app.log 2>&1 &


### 🧩 Step 3 (Optional): Health Check

Add another Execute Shell step:

echo "Checking Application Status..."
curl http://localhost:3000 || echo "App not reachable"



---

## ▶️ Step 8: Build and Verify

Click **Build Now** and watch the Console Output.

Expected output:

npm install
added 50 packages

node app.js
Server running on port 3000


Then open your app in a browser:

👉 `http://<your-ec2-public-ip>:3000`

---

## 🧩 Step 9: Troubleshooting

| Issue | Cause | Fix |
|-------|-------|-----|
| Permission denied | Jenkins user lacks sudo access | Add NOPASSWD in `/etc/sudoers` |
| EADDRINUSE | Port already used | Run `sudo pkill -f node` |
| npm: command not found | Jenkins PATH missing npm | Use `/usr/bin/npm` full path |
| App not running | Wrong file name | Replace `app.js` with correct entry file |

---

## 🧹 Step 10: Useful Commands

| Action | Command |
|--------|---------|
| Stop Node.js app | `sudo pkill -f node` |
| Check running Node apps | `ps aux | grep node` |
| Remove workspace | `sudo rm -rf /var/lib/jenkins/workspace/nodejs-deploy` |

---

## 📝 Notes

- Ensure security groups allow traffic on ports 8080 (Jenkins) and 3000 (Node.js app)
- For production deployments, consider using PM2 or Docker for better process management
- Set up GitHub webhooks for automatic builds on code push

---

## 📄 License

This project is open source and available under the MIT License.

