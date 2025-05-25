# 🚀 EC2 & IAM Project: Step-by-Step Guide

Welcome to the **Step-by-Step Guidance** version of this project! You'll be guided through setting up EC2 instances, IAM policies, and user roles. Let's go!

> 📣 **Important**: Delete all your resources by the end of the day to avoid charges.

---

## 🧠 Objective

You've just joined the **DevOps team** to:

1. Boost computing power by launching EC2 instances.
2. Onboard a new intern with appropriate permissions.

---

## 🖥️ Part 1: Launch EC2 Instances

### Step 1: Open EC2 Console

* Log in to AWS Console
* On the AWS Console home page, search for **EC2** in the search bar.
* Click on **EC2** to open the dashboard.
* On the top right, switch to your nearest **Region**.

### Step 2: Launch Production Instance

* In EC2 Dashboard, click **Launch Instance**.
* Instance Name: `prod-yourname`
* Select AMI: Choose a **Free tier eligible Amazon Linux 2 AMI**.
* Instance type: Choose **t2.micro** (Free tier eligible)
* Key Pair: Select **Proceed without a key pair**
* Under **Network Settings** → click **Edit** and allow SSH and HTTP.
* Under **Advanced details**, add a tag:

  * Key: `Env`, Value: `production`
* Click **Launch Instance**.

### Step 3: Launch Development Instance

* Repeat Step 2.
* Change name to `dev-yourname`
* Change Tag: `Key: Env`, `Value: development`

> 🎉 You've launched both Production and Development EC2 Instances!

---

## 🔐 Part 2: Set Up IAM Policy

### Step 4: Create IAM Policy

* Go to AWS Console → Search **IAM** → Open IAM Console
* In the left menu, click **Policies** → Click **Create Policy**
* Switch to the **JSON tab** and paste the following:

```json
{  
  "Version": "2012-10-17",  
  "Statement": [    
    {      
      "Effect": "Allow",      
      "Action": "ec2:*",      
      "Resource": "*",      
      "Condition": {        
        "StringEquals": {          
          "ec2:ResourceTag/Env": "development"        
        }      
    },
    {      
      "Effect": "Allow",      
      "Action": "ec2:Describe*",      
      "Resource": "*"    
    },
    {      
      "Effect": "Deny",      
      "Action": ["ec2:DeleteTags", "ec2:CreateTags"],      
      "Resource": "*"    
    }  
  ]
}
```

* Click **Next: Tags** → **Next: Review**
* Name: `DevEnvironmentPolicy`
* Description: IAM Policy for development environment
* Click **Create Policy**

---

## 👥 Part 3: Setup Account Alias & IAM Users

### Step 5: Create Account Alias

* In IAM Console → Click **Dashboard** → Click **Create Account Alias**
* Enter: `alias-yourname`
* Click **Save**

### Step 6: Create IAM Group

* Go to **User groups** → Click **Create group**
* Group name: `dev-group`
* Attach policy: Search and select `DevEnvironmentPolicy`
* Click **Create group**

### Step 7: Create IAM User

* Go to **Users** → Click **Add users**
* User name: `dev-yourname`
* Select **Provide user access to the AWS Management Console**
* Select **Custom password**, uncheck **require password reset**
* Click **Next**
* Add user to group: `dev-group`
* Click **Next** → **Create user**

---

## 🧪 Part 4: Test Access

### Step 8: Test User Permissions

* Open a new **Incognito browser window**
* Go to `https://alias-yourname.signin.aws.amazon.com/console`
* Log in using IAM user: `dev-yourname`

#### Test Actions:

* Go to EC2 → Try stopping Production instance → should FAIL ✅
* Try stopping Development instance → should SUCCEED ✅

---

## 🕵️ Secret Mission: IAM Policy Simulator

* Go to [IAM Policy Simulator](https://policysim.aws.amazon.com/)
* Select user `dev-yourname`
* Choose services and test permissions

---

## 🧹 Cleanup Resources

### Terminate EC2 Instances

* Go to EC2 Dashboard → Select `prod-yourname` & `dev-yourname`
* Click **Instance State** → **Terminate Instance**

### Delete IAM Components

* IAM Console → Users → Delete `dev-yourname`
* IAM Console → User groups → Delete `dev-group`
* IAM Console → Policies → Delete `DevEnvironmentPolicy`


---

## 🏁 What You Learned

* 💻 How to launch and tag EC2 instances
* 🏷️ Use tags to manage environments
* 🔐 Create IAM policies with JSON
* 👤 Set up groups and users in IAM
* 🧪 Simulate and test user access


