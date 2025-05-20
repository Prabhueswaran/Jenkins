# Jenkins

# Configuring GitHub Commit Automatic Trigger in Jenkins (with ngrok)

This guide explains how to configure **Jenkins** to trigger builds automatically when you push commits to **GitHub**, using **ngrok** to expose your local Jenkins instance.

---

## 📌 Prerequisites
- Jenkins installed locally
- **Required Jenkins Plugins:**
  - Git Plugin
  - GitHub Integration Plugin
  - Pipeline Plugin
  - GitHub WebHook Plugin
- GitHub repository

---

## 🚀 Step 1: Install Required Jenkins Plugins
1. Open **Jenkins Dashboard** → **Manage Jenkins** → **Manage Plugins**.
2. Go to the **Available** tab.
3. Search for the following plugins and install them:
   - `Git Plugin`
   - `GitHub Integration Plugin`
   - `Pipeline Plugin`
   - `GitHub WebHook Plugin`
4. Restart Jenkins after installation.

---

## 🔗 Step 2: Install and Set Up ngrok

### 🛠 Installation

#### Windows
1. Download ngrok from [ngrok.com](https://ngrok.com/download).
2. Extract the file and move `ngrok.exe` to `C:\ngrok`.
3. Open **Command Prompt** and navigate to the directory:
   ```sh
   cd C:\ngrok

#### Linux / Mac

1. Download and extract ngrok: 
    ```sh
    wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
    unzip ngrok-stable-linux-amd64.zip

2. Move it to /usr/local/bin for global access:
 
    ```sh
    sudo mv ngrok /usr/local/bin

3. Verify installation:

   ```sh
   ngrok --version

## 🔑 Authenticate ngrok (One-time Setup)
1. Get your Auth Token from the [ngrok Dashboard](https://dashboard.ngrok.com).
2. Run the following command:

   ```sh
   ngrok config add-authtoken YOUR_AUTH_TOKEN

## 🌐 Step 3: Expose Jenkins Using ngrok
1. Start Jenkins on your local machine. 
2. Find your Jenkins port (default is
8080). 
3. Run ngrok to expose Jenkins: 
    ```sh 
    Copy Edit ngrok http 8080 

4. You will see an output similar to: 
   ```nginx  
   Forwarding https://random-id.ngrok.io -\> http://localhost:8080 
  
5. Copy the HTTPS URL (e.g., https://random-id.ngrok.io). 

## 🔄 Step 4: Configure GitHub Webhook

1. Open your GitHub repository. 
2. Navigate to Settings → Webhooks. 
3. Click Add Webhook. 
4. In the Payload URL, enter: 
   ```lua
   https://random-id.ngrok.io/github-webhook/ 
   
5. Set Content Type to application/json. 
6. Choose Just the push event. 
7. Click Add Webhook. 

## 🛠 Step 5: Configure Jenkins Job 

### 📝 Option 1: Freestyle Job 
1. Create a new Jenkins Job (New Item → Freestyle Project). 
2. Under Source Code Management, select Git and enter your repository URL. 
3. Under Build Triggers, check GitHub hook trigger for GITScm polling. 
4. Save the job. 

### 📜 Option 2: Pipeline Job
1. Create a new Pipeline Job in Jenkins.

2. Use the following example `Jenkinsfile`

    ```groovy
      pipeline {
          agent any
          triggers {
              pollSCM('* * * * *') // Runs every minute (optional with webhook)
          }
          stages {
              stage('Checkout') {
                  steps {
                      git branch: 'main', url: 'https://github.com/your-repo.git'
                  }
              }
              stage('Build') {
                  steps {
                      echo 'Building...'
                  }
              }
              stage('Test') {
                  steps {
                      echo 'Running tests...'
                  }
              }
              stage('Deploy') {
                  steps {
                      echo 'Deploying...'
                  }
              }
          }
      }

3. Save and run once manually to verify.

## ✅ Step 6: Test GitHub Webhook 

1. Push a commit to your GitHub repository.
2. Open Jenkins Dashboard → Your Pipeline Job. 
3. If Jenkins is triggered automatically, the setup is working! 🎉 

## 🔍 Troubleshooting 

#### Webhook not triggering?

* Check GitHub webhook logs: GitHub → Settings → Webhooks → Recent
Deliveries 
* Ensure ngrok is running before pushing changes. 

#### ngrok session expires?

* Free version of ngrok creates temporary URLs. Restart with: 
  ```sh Copy Edit
  ngrok http 8080 
  
* Use the paid version for a stable URL
