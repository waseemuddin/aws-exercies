# 🚀 AWS Systems Manager: Remote EC2 Command Execution Tutorial

In this hands-on guide, learn how to remotely manage your Amazon EC2 instances using **AWS Systems Manager**—no SSH or bastion hosts required! 🖥️✨

## 🌟 What You'll Learn:
- **Run Commands Remotely**: Update EC2 instance packages without direct SSH access.  
- **Use AWS Systems Manager**: Leverage the power of **Run Command** for scalable management.  
- **Best Practices**: Upgrade your Systems Manager Agent with the **AWS-UpdateSSMAgent** document.  

## 🔐 Prerequisites:
- Create an **IAM role** for EC2 instances.  
- Enable **Systems Manager Agent** on your instance.  
- Run **secure commands** with Systems Manager to maintain compliance and security.

## 🎯 Benefits:
- No need for SSH or bastion hosts.  
- Secure, scalable, and automated management.  
- Part of the **AWS Free Tier** 🆓 – perfect for admins at any level.

## 💡 Why This Tutorial:
- Ideal for both **new** and **experienced system administrators** looking to automate EC2 management.  
- Keeps your EC2 instances compliant without manual intervention.  
- Cost-effective as part of AWS's **always free** tier!

## 📘 Start Your AWS Journey:
Follow this guide to get started with **AWS Systems Manager** and elevate your EC2 management skills today! 🌐


--------------------------------------------------



# 🚀 AWS Systems Manager: Remote EC2 Command Execution Tutorial

In this hands-on guide, learn how to remotely manage your Amazon EC2 instances using **AWS Systems Manager**—no SSH or bastion hosts required! 🖥️✨

## 🌟 What You'll Learn:
- **Run Commands Remotely**: Update EC2 instance packages without direct SSH access.  
- **Use AWS Systems Manager**: Leverage the power of **Run Command** for scalable management.  
- **Best Practices**: Upgrade your Systems Manager Agent with the **AWS-UpdateSSMAgent** document.  

## 🔐 Prerequisites:
- Create an **IAM role** for EC2 instances.  
- Enable **Systems Manager Agent** on your instance.  
- Run **secure commands** with Systems Manager to maintain compliance and security.

---

## 🛠️ Implementation

### 1. Create an IAM Role for EC2 Instances
To get started, you need to create an IAM role that grants your EC2 instances the necessary permissions to communicate with AWS Systems Manager. This allows Systems Manager to securely manage your EC2 instances without SSH.

**Steps:**
1. Open the IAM console in AWS.
2. Create a new role for EC2 with the `AmazonSSMManagedInstanceCore` policy attached.
3. Assign this role to your EC2 instance.

![IAM Role Creation](./images/iam-role-creation.png)

---

### 2. Enable Systems Manager Agent on EC2 Instance
Before using Systems Manager to manage your EC2 instance, you need to ensure the **SSM Agent** is installed and running on the instance.

**Steps:**
1. Verify that the Systems Manager Agent (SSM Agent) is installed.
2. If not, install the agent using the commands provided in the AWS documentation.
3. Make sure the SSM Agent service is running.

![SSM Agent Installation](./images/ssm-agent-installation.png)

---

### 3. Use the AWS-UpdateSSMAgent Document to Upgrade the SSM Agent
It’s essential to keep the SSM Agent up-to-date for the latest features and security patches.

**Steps:**
1. In the Systems Manager console, navigate to **Run Command**.
2. Choose the `AWS-UpdateSSMAgent` document.
3. Execute the command to upgrade the agent.

![SSM Agent Update](./images/ssm-agent-update.png)

---

### 4. Run Commands Remotely on EC2 Instance
Now that you have everything set up, you can start running commands on your EC2 instance remotely using Systems Manager's **Run Command**.

**Steps:**
1. In the Systems Manager console, go to **Run Command**.
2. Select the document you want to execute (e.g., `AWS-RunShellScript`).
3. Input the necessary commands (e.g., update your EC2 instance packages).
4. Execute and monitor the command output.

![Run Command](./images/run-command-remote.png)

---

## 🎯 Benefits:
- No need for SSH or bastion hosts.  
- Secure, scalable, and automated management.  
- Part of the **AWS Free Tier** 🆓 – perfect for admins at any level.

## 💡 Why This Tutorial:
- Ideal for both **new** and **experienced system administrators** looking to automate EC2 management.  
- Keeps your EC2 instances compliant without manual intervention.  
- Cost-effective as part of AWS's **always free** tier!

## 🌐 Get Started:
Follow this step-by-step tutorial to remotely run commands on your EC2 instances using **AWS Systems Manager**:  
[Start the Hands-On Tutorial](https://aws.amazon.com/getting-started/hands-on/remotely-run-commands-ec2-instance-systems-manager/?ref=gsrchandson&id=itprohandson)

---

## 📘 Start Your AWS Journey:
Elevate your EC2 management skills today and simplify your admin tasks with **AWS Systems Manager**! 🌟
