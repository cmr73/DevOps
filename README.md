# ✅ DevOps Lab – Important Tips and Suggestions

## 📌 Account Creation Tips

To avoid unnecessary charges during the lab work:

- **Use zero-balance digital bank accounts** such as:
  1. **Airtel Payments Bank**
  2. **Jio Payments Bank**
- This avoids large bills since these accounts won't hold significant funds.

> 🔑 *You only need a PAN card to open these accounts.*

### 💳 Google Cloud Account Setup
- Use **UPI details** from the above zero-balance bank accounts.
---

## 🖥️ Accessing EC2 Instances

You can access your **AWS EC2 instance** using the following methods:

1. **EC2 Instance Connect**  
   - ✔️ Simple and direct (browser-based)  
   - ❌ Limited features, not ideal for multitasking

2. **MobiXterm**  
   - ✔️ Best for handling multiple instances via tabs  
   - ✔️ Friendly GUI with built-in SSH client

3. **Git Bash**  
   - ✔️ Lightweight and smooth terminal experience on Windows  
   - ✔️ Recommended for CLI users

---
## ⚠️ EC2 Instance Configuration Best Practices

Setting up your EC2 instance correctly is crucial to avoid access or deployment issues later. Follow these guidelines carefully:

---

### 1. ✅ **Enable SSH (Port 22) Access**

* **Never remove the SSH inbound rule (port 22)** when configuring your instance.
* 🔐 This port is essential for remote access using tools like **MobaXterm**, **Git Bash**, or **VS Code Remote SSH**.
* Without this, you won't be able to log in to your EC2 instance, which could leave you locked out.

---

### 2. 🌐 **Allow HTTP Traffic (Port 80)**

* When launching a new EC2 instance, **always check the box to allow HTTP traffic**.
* This opens **port 80**, which is commonly used for serving web applications (e.g., using Nginx or Apache).
* Not enabling this will block web traffic, and your app won’t be reachable through a browser.

---

### 3. 📦 **Handling Custom Ports (e.g., Port 3000)**

* If your application runs on a **custom port like 3000** (commonly used for Node.js apps), ensure the port is **explicitly open** in the **security group** associated with the instance.

#### 🔍 How to Check If a Port Is Open:

1. Go to the **EC2 Dashboard**.
2. Click on your **instance ID** to open its details.
3. Scroll down to the **"Security"** section.
4. Under **Inbound Rules**, check if there is a rule for:

   * **Type**: Custom TCP
   * **Port Range**: `3000`
   * **Source**: `0.0.0.0/0` (or restrict as needed)

#### ➕ How to Add a Custom Port (e.g., 3000):

1. Click on the **Security Group name** under your instance's security section.
2. Navigate to the **Inbound Rules** tab.
3. Click **Edit inbound rules** → **Add rule**.
4. Set:

   * **Type**: Custom TCP
   * **Port Range**: `3000`
   * **Source**: `Anywhere (0.0.0.0/0)`
5. Click **Save rules**.

> ⚠️ This step might not be mentioned explicitly in every experiment or tutorial.
> **Don't just blindly follow instructions** — always understand what your app needs and configure the security group accordingly.

---
## 🧪 Lab Instructions

- All weekly experiments have been **verified and tested**.
- If you follow the steps carefully and configure everything as instructed,  
  💯 **you will be able to complete all labs successfully**.

---
All The Best! 🚀
