# 🔒 Encrypt Data with AWS KMS

Protect a database using encryption with AWS KMS!

---

## 📊 Project Overview

This project walks you through implementing **data-at-rest encryption** for an AWS DynamoDB database using **AWS Key Management Service (KMS)**. You'll learn to create and manage customer-managed keys (CMKs), apply encryption settings to a database, and test how permissions affect access. The goal is to simulate a secure environment where only authorized users can view sensitive data while unauthorized access is effectively blocked.

By the end of the project, you'll have hands-on experience with one of the most important data protection practices in cloud security—one that is foundational for compliance, secure application development, and cloud architecture design.


* **KEY SERVICES**: AWS KMS, Amazon DynamoDB, AWS IAM


---

## 🚀 Project Steps

### 🔑 Step 1: Create a KMS Key

* Open **KMS** in AWS Console
* Choose **Customer managed key**
* Select **Symmetric** key
* Choose **Encrypt and Decrypt**
* Set alias: `project-kms-key`
* Add description: “KMS key that will encrypt a DynamoDB table.”
* Add yourself as **Key Administrator** and **Key User**
* Finish and take a screenshot

### 🗄️ Step 2: Create and Encrypt a DynamoDB Table

* Go to **DynamoDB** in AWS Console
* Click **Create table**
* Name it `project-kms-table`
* Set partition key to `id` (String)
* Leave default settings
* After creation, go to **Additional Settings > Manage Encryption**
* Choose: “Stored in your account, owned and managed by you”
* Select your `project-kms-key`
* Save changes and screenshot

### ➕ Step 3: Add Data to Table

* Go to table settings → **Actions > Create item**
* Add a simple item: `{ "id": "1" }`
* Go to **Explore items** tab and confirm visibility
* Take a screenshot

### 🧑‍💼 Step 4: Create a Test IAM User

* Go to **IAM > Users > Add user**
* Name: `project-kms-user`
* Grant console access, disable reset
* Attach policy: `AmazonDynamoDBFullAccess`
* Download `.csv` credentials

### 🕵️ Step 5: Validate KMS Encryption

* Log in with test user in Incognito
* Try accessing `project-kms-table`
* Expect: **Access Denied** (no `kms:Decrypt` permission)
* Take a screenshot

---

## 💎 Secret Mission: Grant Test User Decrypt Access

* As Admin, go to **KMS > Keys > project-kms-key**
* Modify the **key policy** to allow `project-kms-user` to decrypt
* Re-test DynamoDB access as the test user

---

## 🗑️ Cleanup Resources

### Delete DynamoDB Table

* Go to **DynamoDB > Tables > project-kms-table** → Delete

### Schedule KMS Key Deletion

* Go to **KMS > Keys > project-kms-key** → Schedule deletion
* Set waiting period to **7 days**

### Delete IAM User

* Go to **IAM > Users > project-kms-user** → Delete user
* Delete `.csv` credentials locally

---

## ✅ What You Learned

* 🔑 Create encryption keys using AWS KMS.
* 🗄️ Encrypt a DynamoDB database with a KMS key.
* ➕ Add and retrieve data from your encrypted table.
* 🕵️‍♀️ Simulate access denial for unauthorized users.
* 💎 Grant encryption access to a test IAM user.

🎉 Great job protecting cloud data the secure way!


---

🚀 Onward, encryption master!
