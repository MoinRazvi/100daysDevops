# 🚀 Day 11 of 100 | Tomcat Installation & Java App Deployment

## 🎯 Objective

Install and configure the **Apache Tomcat application server**, deploy a Java web application, and verify that it is accessible via a custom port — a **core real-world DevOps deployment task**.

---

## 🧩 Problem Statement

The Nautilus application development team has completed the beta version of a **Java-based web application**.
They decided to deploy it using **Tomcat** on **App Server 2** with the following requirements:

* Install Tomcat on App Server 2
* Configure Tomcat to run on **port 3004**
* Deploy `ROOT.war` provided on the Jump Host
* Ensure the application is accessible at the **base URL**:

  ```bash
  curl http://stapp02:3004
  ```

---

## 🛠️ Environment Details

| Component          | Value                       |
| ------------------ | --------------------------- |
| App Server         | App Server 2 (`stapp02`)    |
| Application Type   | Java Web Application        |
| Application Server | Apache Tomcat               |
| Custom Port        | `3004`                      |
| WAR File Location  | `/tmp/ROOT.war` (Jump Host) |

---

## 🛠️ Commands Used

* `yum`
* `rpm`
* `vi`
* `systemctl`
* `scp`
* `mv`
* `chown`
* `ss`
* `curl`

---

## 🧪 Hands-on Implementation

### ✅ Step 1: Login to App Server 2

From the Jump Host:

```bash
ssh steve@stapp02
```

---

### ✅ Step 2: Install Tomcat

```bash
sudo yum install -y tomcat tomcat-webapps tomcat-admin-webapps
```

Verify installation:

```bash
rpm -qa | grep tomcat
```

---

### ✅ Step 3: Configure Tomcat to Run on Port 3004

Edit the Tomcat configuration file:

```bash
sudo vi /etc/tomcat/server.xml
```

Locate the connector (default port `8080`) and update it:

```xml
<Connector port="3004"
           protocol="org.apache.coyote.http11.Http11NioProtocol"
           connectionTimeout="20000"
           redirectPort="8443" />
```

Save and exit.

---

### ✅ Step 4: Start and Enable Tomcat

```bash
sudo systemctl start tomcat
sudo systemctl enable tomcat
```

Verify service status:

```bash
sudo systemctl status tomcat
```

---

### ✅ Step 5: Copy Application WAR File from Jump Host

Exit App Server 2:

```bash
exit
```

From the Jump Host:

```bash
scp /tmp/ROOT.war steve@stapp02:/tmp/
```

---

### ✅ Step 6: Deploy the Application

Login back to App Server 2:

```bash
ssh steve@stapp02
```

Move the WAR file to Tomcat’s deployment directory:

```bash
sudo mv /tmp/ROOT.war /usr/share/tomcat/webapps/
```

Set correct ownership:

```bash
sudo chown tomcat:tomcat /usr/share/tomcat/webapps/ROOT.war
```

---

### ✅ Step 7: Restart Tomcat to Deploy Application

```bash
sudo systemctl restart tomcat
```

---

### ✅ Step 8: Verify Deployment

Check if Tomcat is listening on port **3004**:

```bash
ss -tulnp | grep 3004
```

Test the application:

```bash
curl http://stapp02:3004
```

✅ The application should load successfully from the **base URL**.

---

## 📌 Real-World Use Cases

* Deploying Java applications in enterprise environments
* Customizing application server ports
* Supporting legacy or port-specific deployments
* Validating application health via CLI tools
* Production troubleshooting for app servers

---

## 🧠 Key Learnings

* Apache Tomcat is a widely used **Java application server**
* `ROOT.war` deploys directly to the base URL
* Port configuration changes require Tomcat restart
* Correct file ownership is critical for deployment
* `curl` is a powerful tool for quick service validation
* This mirrors **real production deployment workflows**

