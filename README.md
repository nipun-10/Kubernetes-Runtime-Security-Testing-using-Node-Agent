
# 🚀 Kubernetes Runtime Security Testing using Node Agent 

## 📌 Project Overview

This project demonstrates a **real-world Kubernetes runtime security validation setup** where a vulnerable application is deployed and multiple attack scenarios are simulated to evaluate the effectiveness of a **Node Agent (runtime security monitor)**.

The goal is to **test detection and prevention capabilities** by analyzing whether attacks are successfully blocked or exploited. This setup closely mimics **real-world DevSecOps and cloud security testing environments**, helping identify gaps in runtime protection.

---

## 🧠 S – Situation

Modern Kubernetes environments run multiple workloads, but **runtime threats like SQL injection, command injection, and XSS attacks often go undetected** if proper monitoring is not in place.

To understand how runtime security works in practice, I decided to simulate real attacks on a vulnerable application and validate how effectively a **Node Agent detects or blocks them**.

---

## 🎯 T – Task

My objective was to:

* Deploy a vulnerable application inside a Kubernetes cluster
* Simulate multiple real-world attack vectors
* Monitor Node Agent logs in real time
* Identify:

  * Which attacks are **blocked (detected)**
  * Which attacks are **successful (missed detection)**

---

## ⚙️ A – Action

I created a dedicated testing environment inside Kubernetes and performed the following:

* Deployed **DVWA (Damn Vulnerable Web Application)** in a separate namespace

* Verified that the **Node Agent was already running on cluster nodes**

* Used tools like:

  * Burp Suite / OWASP ZAP
  * sqlmap
  * Manual payload injection

* Simulated multiple attacks:

  * SQL Injection
  * Command Injection
  * Stored XSS
  * DOM-based XSS
  * File Inclusion (LFI)
  * Brute Force attacks

* Monitored logs using:

```bash
kubectl logs -n node-agent <pod-name> --follow
```

* Compared HTTP responses:

  * `200 OK` → Attack successful
  * `403 Forbidden` → Attack blocked

---

## 📊 R – Result

The project successfully demonstrated:

* Real-time visibility of runtime attacks
* Clear differentiation between **detected vs missed threats**
* Understanding of how Node Agent enforces security policies
* Identification of **gaps in detection mechanisms**

This project significantly improved my understanding of:

* Kubernetes runtime security
* Attack patterns and exploitation techniques
* Log-based security validation

---

## 🏗️ Architecture Overview

![Architecture Diagram](Arch%20Diagram.png)
```
Kubernetes Cluster
│
├── Node 1 (Node Agent running)
│   ├── DVWA Pod (dvwa-test namespace)
│
├── Node 2 (Node Agent running)
│   ├── Other workloads
│
User → Deploys DVWA
Kubernetes → Schedules Pod on Node
Node Agent → Monitors activity on node
```

### 🔍 Key Insight

* Node Agent works at **node level (not pod level)**
* Any deployed workload is **automatically monitored**
* No additional integration required for each application

---

## 🔧 Environment Setup

### 1️⃣ Create Namespace

```bash
kubectl create namespace dvwa-test
kubectl config set-context --current --namespace=dvwa-test
```

---

### 2️⃣ Deploy DVWA

```bash
kubectl apply -f dvwa-deployment.yaml
```

---

### 3️⃣ Access Application

**Option 1: NodePort**

```
http://<NODE_IP>:30080
```

**Option 2: Port Forward**

```bash
kubectl port-forward svc/dvwa 8080:80 -n dvwa-test
```

```
http://localhost:8080
```

---

### 4️⃣ DVWA Setup

* Username: `admin`
* Password: `password`
* Set security level → **Low**

---

## ⚔️ Attack Simulation

### 🧨 SQL Injection

```
' OR '1'='1
```

---

### 💻 Command Injection

```
127.0.0.1; whoami
127.0.0.1; cat /etc/passwd
```

---

### 🧬 Stored XSS

```html
<script>alert('stored-xss')</script>
```

---

### 🌐 DOM XSS

```html
<script>alert(1)</script>
<img src=x onerror=alert(1)>
```

---

### 📂 File Inclusion (LFI)

```
?page=../../etc/passwd
```

---

### 🔐 Brute Force

Multiple login attempts using wrong credentials followed by correct password.

---

## 🧪 Validation Method

| HTTP Code | Meaning          |
| --------- | ---------------- |
| 200       | Attack Exploited |
| 403       | Attack Blocked   |

---

## 📘 What I Learned

* How runtime security works in Kubernetes
* Importance of node-level monitoring
* Real-world attack execution techniques
* Log-based threat validation
* Security gaps in detection systems

---

## 🔐 Security Insights

* Not all attacks are detected → need better rule tuning
* Runtime visibility is critical for production systems
* Logs are the **primary source of truth** for validation
* Defense requires both **prevention + detection**

---

## 📚 Key Learnings

* Deep understanding of Kubernetes security
* Hands-on experience with attack simulation
* Practical exposure to DevSecOps workflows
* Real-world debugging of security issues

---

## 🧠 Use Cases

* Kubernetes security testing
* DevSecOps pipelines validation
* Runtime threat detection analysis
* Security research & POCs

---

## 🚀 Future Enhancements

* Integrate Falco for rule-based detection
* Automate attack simulation pipeline
* Add alerting (Slack / Email)
* Use ML models for false positive reduction
* Integrate into CI/CD security workflows

---

## 🧑‍💻 Author

**Nipun Bhardwaj**
DevSecOps & Cloud Enthusiast

📌 GitHub: [https://github.com/nipun-10](https://github.com/nipun-10)

---

⭐ If you found this project helpful, consider giving it a star!
