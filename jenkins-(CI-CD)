### ✅ **AWS EC2 Instance Setup**

1. **Launch two EC2 instances:**

   * `devm` (Dev Machine)
   * `setm` (Sit Server)
   * Choose Ubuntu, allow HTTP traffic.

---

### 🔧 **On `devm`: Jenkins Setup**

1. **Update and Install Java 21:**

   ```bash
   sudo apt update
   sudo apt install fontconfig openjdk-21-jre
   java -version
   ```

2. **Install Jenkins:**

   ```bash
   sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
   echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
   sudo apt-get update
   sudo apt-get install jenkins
   sudo systemctl start jenkins
   ```

3. **Access Jenkins via Browser:**

   ```
   http://<devm-public-ip>:8080
   ```

4. **Install Suggested Plugins**

---

### ⚙️ **Install and Configure Maven (on devm)**

1. **Download & Extract Maven:**

   ```bash
   cd /opt
   sudo wget https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz
   sudo tar -xvzf apache-maven-3.9.9-bin.tar.gz
   sudo mv apache-maven-3.9.9 maven
   ```

2. **Configure Environment Variables:**

   ```bash
   sudo vim ~/.profile
   ```

   Add:

   ```bash
   export JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64
   export M2_HOME=/opt/maven
   export MAVEN_HOME=/opt/maven
   export PATH=${M2_HOME}/bin:${PATH}
   ```

   Reload:

   ```bash
   source ~/.profile
   mvn --version
   ```

---

### 🧩 **Configure Jenkins Tools**

1. **Install "Maven Integration" Plugin**
2. **Configure JDK & Maven (UNCHECK "Install Automatically"):**

   * JDK path: `/usr/lib/jvm/java-21-openjdk-amd64`
   * Maven path: `/opt/maven`

---

### 🛠️ **Create Maven Job (WAR Project)**

1. **New Item → Maven Project**

2. **Git SCM:**

   * Fork the repo: [https://github.com/Mahi-Repalle/practice](https://github.com/Mahi-Repalle/practice)
   * Use HTTPS URL

3. **Build Goal:**

   ```
   clean install
   ```

4. **Build Job → Check WAR created**

---

### 🌐 **On `setm`: Tomcat Setup**

1. **Install Tomcat:**

   ```bash
   sudo apt update
   sudo apt install -y tomcat9 tomcat9-admin
   ```

2. **Configure `tomcat-users.xml`:**

   ```xml
   <role rolename="manager-gui"/>
   <role rolename="manager-script"/>
   <user username="admin" password="admin" roles="manager-gui,manager-script"/>
   ```

3. **Restart Tomcat:**

   ```bash
   sudo service tomcat9 restart
   ```

4. **Access Tomcat:**

   ```
   http://<setm-public-ip>:8080
   ```

---

### 🔁 **Deploy WAR from Jenkins to SIT**

1. **Install "Deploy to container" plugin**

2. **Job → Configure → Post-build Actions:**

   * Select “Deploy WAR/EAR to a container”
   * Container: Tomcat 9.x
   * URL: `http://<setm-public-ip>:8080`
   * Credentials: admin/admin

3. **Run Build → WAR Deployed**

4. **Verify on browser:**

   ```
   http://<setm-public-ip>:8080/sit
   ```

---

### 🤖 **CI/CD Automation**

* **Enable SCM Polling / GitHub Webhook**
* Any change in the repo (`index.jsp`, etc.) triggers automatic:

  * Pull
  * WAR Build
  * Deployment to SIT
