# 🚀 Splunk Universal Forwarder Setup & Log Forwarding Guide

## 📌 Overview
This project demonstrates how to install, configure, and use the Splunk Universal Forwarder to send logs from a local machine (or server) to a Splunk indexer. This is a core skill in SOC environments where log collection from multiple endpoints is essential for monitoring and detection.

The goal is to forward system logs (Linux/Windows or custom log files) into Splunk for centralized analysis.

---

## 🧠 What is Splunk Universal Forwarder?
The Splunk Universal Forwarder is a lightweight agent installed on endpoint systems to collect and forward logs to a Splunk server (indexer or Splunk Cloud).

It does NOT analyze data locally — it only forwards logs.

---

## 🏗️ Architecture
Forwarder (Client Machine) ➜ Indexer (Splunk Server) ➜ Search Head (Splunk UI)

---

## ⚙️ Prerequisites
- Splunk Enterprise installed on server (indexer)
- Linux/Windows machine for forwarder
- Network connectivity between both systems
- Admin access

---

## 📥 Step 1: Download Splunk Universal Forwarder

Linux:
```bash
wget -O splunkforwarder.tgz "https://download.splunk.com/products/universalforwarder/releases/latest/linux/splunkforwarder.tgz"
````

Windows:
Download from official Splunk site:
[https://www.splunk.com/en_us/download/universal-forwarder.html](https://www.splunk.com/en_us/download/universal-forwarder.html)

---

## 📦 Step 2: Install Splunk Forwarder (Linux)

```bash
tar -xvf splunkforwarder.tgz
cd splunkforwarder/bin
```

Start first time setup:

```bash
./splunk start --accept-license
```

Set admin username/password during setup.

---

## 🔁 Step 3: Enable Splunk at Boot

```bash
./splunk enable boot-start
```

---

## 📡 Step 4: Configure Forwarding to Splunk Indexer

Replace `<INDEXER_IP>` with your Splunk server IP.

```bash
./splunk add forward-server <INDEXER_IP>:9997
```

Example:

```bash
./splunk add forward-server 192.168.1.10:9997
```

---

## 📂 Step 5: Add Log Source (Inputs)

### Add system logs:

```bash
./splunk add monitor /var/log
```

### Add specific file:

```bash
./splunk add monitor /var/log/auth.log
```

### Add custom directory:

```bash
./splunk add monitor /home/user/logs/
```

---

## 🔐 Step 6: Enable Receiving on Splunk Server (Indexer)

On Splunk Enterprise (server):

Go to CLI:

```bash
$SPLUNK_HOME/bin/splunk enable listen 9997
```

OR via UI:

* Settings → Forwarding and Receiving → Configure Receiving → Add port 9997

---

## 🔄 Step 7: Restart Splunk Forwarder

```bash
./splunk restart
```

---

## 📊 Step 8: Verify Data in Splunk UI

Search query:

```spl
index=main
```

Or:

```spl
index=main sourcetype=linux_secure
```

---

## 🧪 Step 9: Test Log Generation

Generate test login attempt:

```bash
ssh fakeuser@localhost
```

Check logs appear in Splunk search head.

---

## 🔍 Troubleshooting Commands

Check forwarder status:

```bash
./splunk list forward-server
```

Check inputs:

```bash
./splunk list monitor
```

Check logs:

```bash
tail -f splunkforwarder/var/log/splunk/splunkd.log
```

---

## 🚨 Common Issues

### ❌ No data in Splunk

* Check port 9997 is open
* Ensure indexer listening is enabled

### ❌ Forwarder not connecting

* Verify IP address
* Check firewall rules

### ❌ Permission issues

```bash
sudo chown -R splunk:splunk splunkforwarder
```

---

## 🛠️ Skills Demonstrated

* SIEM data ingestion
* Log forwarding architecture
* Splunk configuration (indexer + forwarder)
* Linux command-line usage
* SOC monitoring pipeline setup

---

## 📊 Why This Matters in SOC

In real SOC environments:

* Hundreds of endpoints send logs to SIEM
* Forwarders ensure centralized visibility
* Enables threat detection across infrastructure

---

## 🚀 Future Improvements

* Add Windows Event Log forwarding
* Configure multiple indexers (load balancing)
* Implement TLS encryption for forwarders
* Build dashboards for incoming logs

---

## 📁 Project Structure

```
splunk-forwarder-setup/
│── README.md
│── configs/
│     └── inputs.conf
│     └── outputs.conf
│── logs/
│     └── sample_auth.log
│── screenshots/
│     └── splunk_dashboard.png
```

---

## 🧾 Conclusion

This project demonstrates how to set up a Splunk Universal Forwarder and successfully stream logs to a central Splunk instance. It is a foundational SOC skill used in real-world security monitoring and incident detection environments.

---

⭐ If you found this useful, please star the repository!

```

