# Splunk + Sysmon Lab Setup

This document details the full process of setting up **Splunk Enterprise on Ubuntu**, configuring **Sysmon on Windows**, and forwarding logs using the **Splunk Universal Forwarder**.

---

## 1. Install Splunk Enterprise on Ubuntu VM

First, download Splunk Enterprise on your **Ubuntu VM** from the [official Splunk website](https://www.splunk.com).  
Then extract and install it into `/opt` using:

```bash
sudo tar -xvzf splunk-10.0.0-e8eb0c4654f8-linux-amd64.tgz -C /opt
cd /opt/splunk/bin
sudo ./splunk enable boot-start
```

You will then be prompted to create a **username and password**.

<img src="https://github.com/user-attachments/assets/d98a0208-8448-4105-a37e-8676ef3c5a33" width="700">

---

## 2. Access Splunk Enterprise

Once Splunk is running, go to your **Windows VM** browser and type in the **Ubuntu VM IP** (from `enp0s8` adapter).  
This lets you access Splunk Enterprise through port `8000`.

<img src="https://github.com/user-attachments/assets/a8f68c29-b2d7-4f02-becf-5b8051aa751a" width="700">

Enable receiving data on port **9997** (this is where logs will be forwarded).

---

## 3. Install Sysmon on Windows VM

Download Sysmon from [Microsoft Sysinternals](https://learn.microsoft.com/en-us/sysinternals).  
Since Sysmon is noisy by default, download a **community Sysmon config** (XML file) to filter logs.

Unzip Sysmon, place the config file in the same folder, and run in **PowerShell (Admin):**

```powershell
.\Sysmon64.exe -accepteula -i sysmonconfig.xml
```

<img src="https://github.com/user-attachments/assets/23d9f523-3984-4eb9-bc56-97fe39881f72" width="700">
<img src="https://github.com/user-attachments/assets/81a3a842-f9ca-4be0-9ffd-55311cc901b9" width="700">


Check Event Viewer under **Applications and Services Logs → Microsoft → Windows → Sysmon → Operational** to confirm events are being logged.

---

## 4. Install Splunk Universal Forwarder on Windows VM

Next, download the **Splunk Universal Forwarder** (Windows 64-bit MSI) and install it.  
During setup, choose **"On-premises Splunk Enterprise instance"** and provide the **Ubuntu VM IP** and port `9997`.

<img src="https://github.com/user-attachments/assets/56ecdde9-f0c2-4545-a897-c589d89254ff" width="700">
<img src="https://github.com/user-attachments/assets/55d8c741-ec67-4dcf-847e-ca6c95b0a313" width="700">
<img src="https://github.com/user-attachments/assets/05d50e77-e955-4fc5-9ac1-e63959d657cd" width="700">

---

## 5. Create Sysmon Index on Splunk (Ubuntu VM)

Inside Splunk Enterprise (Ubuntu VM), create a new index called `sysmon`.  
This is where all forwarded Sysmon logs will be stored.

<img src="https://github.com/user-attachments/assets/70c6a16b-1bb4-4a55-9e6b-b8456181fbf4" width="700">

---

## 6. Configure Splunk Universal Forwarder (Windows VM)

Navigate to the Universal Forwarder config folder:

```
C:\Program Files\SplunkUniversalForwarder\etc\system\local
```

Create or edit `inputs.conf` (Run Notepad as Admin) and add:

```conf
[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = 0
renderXml = true
index = sysmon
```

This tells the forwarder to:
- Collect Sysmon logs (`WinEventLog://...`)
- Ensure logging is **enabled**
- Forward logs in structured XML
- Send to the `sysmon` index on Ubuntu Splunk

<img src="https://github.com/user-attachments/assets/1c8b2a89-f82e-42c3-8760-2fbdb2d9bbb0" width="700">

Restart the Splunk Forwarder service in PowerShell:

```powershell
Restart-Service -Name SplunkForwarder
```

If you get **Access Denied** errors for Sysmon, open `services.msc`, edit SplunkForwarder service, and run it under **Local System** privileges.

<img src="https://github.com/user-attachments/assets/eaed0f51-33ab-4744-86f9-245c0790db78" width="700">

---

## 7. Verify Splunk Receives Sysmon Logs

Go back to Splunk Enterprise (Ubuntu VM), open **Search & Reporting**, and query:

```spl
index=sysmon | stats count by EventCode
```

Now you should see Sysmon logs successfully arriving from your Windows VM.

<img src="https://github.com/user-attachments/assets/af62dd64-9926-4a34-ad50-1e1c94ceafc4" width="700">
<img src="https://github.com/user-attachments/assets/865d90ec-cf71-48d2-9769-75d0efbed2e0" width="700">

---

# ✅ Final Result

You now have a working lab setup where:
- **Sysmon** collects security logs (Windows VM).  
- **Splunk Universal Forwarder** ships them to Ubuntu.  
- **Splunk Enterprise** ingests, stores, and visualizes the data.  

This is the same setup SOC teams use for **threat hunting, log aggregation, and incident response**.

Download Splunk Ad-on for Sysmon because Splunk doesn't automatically parse raw XML data 

<img width="1444" height="444" alt="image" src="https://github.com/user-attachments/assets/629543c8-8e65-4e87-9374-dbb41e4c7eba" />

Proceed to paste this code in Ubuntu VM to extract archive add-on

cd /opt/splunk/etc/apps/
sudo tar -xvzf ~/Downloads/splunk-add-on-for-sysmon_*.tgz










