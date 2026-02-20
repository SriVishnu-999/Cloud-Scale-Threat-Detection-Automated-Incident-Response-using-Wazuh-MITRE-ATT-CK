# 🚨 Wazuh SOC Detection Engineering Lab

**Author:** Vishnu  
**Role Focus:** SOC Analyst | Detection Engineer | Security Operations  
**Environment:** Ubuntu Server + Windows 10  
**Platform:** Wazuh 4.14.2  
**Framework Mapping:** MITRE ATT&CK  

---

## 📌 Project Summary

This project demonstrates the deployment and configuration of a Security Information and Event Management (SIEM) lab using Wazuh.  

The lab simulates real-world SOC operations including:

- Endpoint onboarding
- Log ingestion & analysis
- Custom detection rule engineering
- MITRE ATT&CK mapping
- Attack simulation
- Alert validation
- Active response automation

The goal was to build a production-style detection environment and validate detections using real attack scenarios.

---

## 🏗️ Lab Architecture

```
Ubuntu Server (Wazuh Manager + Indexer + Dashboard)
        |
        | Encrypted Agent Communication
        |
Windows 10 (Wazuh Agent)
```

---

## ⚙️ Phase 1 — SIEM Deployment

Installed on Ubuntu:

- Wazuh Manager
- Wazuh Indexer (OpenSearch)
- Wazuh Dashboard

Service validation:

```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard
```

---

## 🖥️ Phase 2 — Windows Agent Integration

Steps:

1. Installed Wazuh Agent on Windows 10
2. Registered agent with manager
3. Verified agent connectivity

```bash
sudo /var/ossec/bin/agent_control -l
```

Successfully onboarded Windows endpoint into the SIEM.

---

## 🎯 Phase 3 — Custom Detection Engineering

Custom rules were created inside:

```
/var/ossec/etc/rules/local_rules.xml
```

---

### 🔐 Rule 100500 — Brute Force Detection  
**MITRE Technique:** T1110 (Brute Force)

Detects 5 failed logins within 120 seconds.

```xml
<group name="windows,custom,">
  <rule id="100500" level="12" frequency="5" timeframe="120">
    <if_matched_sid>60122</if_matched_sid>
    <description>Windows Multiple Failed Logins - Brute Force Detected</description>
    <mitre>
      <id>T1110</id>
    </mitre>
  </rule>
</group>
```

---

### 🛡 Rule 100510 — Privilege Escalation  
**MITRE Technique:** T1078 (Valid Accounts)

```xml
<rule id="100510" level="10">
  <if_sid>60103</if_sid>
  <description>Windows Privilege Escalation Detected</description>
  <mitre>
    <id>T1078</id>
  </mitre>
</rule>
```

---

### ⚡ Rule 100520 — PowerShell Execution  
**MITRE Technique:** T1059.001 (PowerShell)

```xml
<rule id="100520" level="13">
  <if_group>windows</if_group>
  <field name="win.system.providerName">Microsoft-Windows-PowerShell</field>
  <description>PowerShell Execution Detected - Possible Abuse</description>
  <mitre>
    <id>T1059.001</id>
  </mitre>
</rule>
```

---

## 🧪 Phase 4 — Attack Simulation

Simulated the following:

- Multiple failed login attempts
- PowerShell execution
- Privilege escalation activity

Validated alerts using:

```bash
sudo /var/ossec/bin/wazuh-logtest
```

Dashboard queries used:

```
rule.id:100500
rule.id:100510
rule.id:100520
```

---

## 🔥 Active Response Automation

Created a bash script to block attacking IPs using iptables when brute-force rule fires.

Script location:

```
/var/ossec/active-response/bin/block_windows_ip.sh
```

Script contents:

```bash
#!/bin/bash
IP=$1
iptables -I INPUT -s $IP -j DROP
```

This enables automatic containment upon detection.

---

## 📊 Security Configuration Assessment (SCA)

Executed CIS Windows 10 Benchmark policy.

Results:

- 424 checks evaluated
- 29% compliance score
- Multiple password & lockout misconfigurations identified

Identified risk → Simulated attack → Detected via custom rule → Automatically blocked.

---

## 🧠 Skills Demonstrated

- SIEM deployment
- Endpoint log ingestion
- XML rule engineering
- MITRE ATT&CK mapping
- Windows Event Log analysis
- Threat simulation
- Linux troubleshooting
- Active response automation
- SOC investigation workflow

---

## 📁 Repository Structure

```
Wazuh-SOC-Lab/
│
├── screenshots/
├── rules/
├── active-response/
├── attack-simulation/
└── README.md
```

---

## 🚀 Future Improvements

- Add Sysmon-based detections
- Add credential dumping detection (T1003)
- Create SOAR-style automation
- Expand to Linux & Cloud agents
- Integrate threat intelligence feeds

---

## 🎯 Conclusion

This project demonstrates the complete SOC lifecycle:

Deployment → Log Ingestion → Detection Engineering → Attack Simulation → Alert Validation → Automated Response

It reflects real-world detection engineering and SOC analyst capabilities suitable for enterprise environments.

---# 🚨 Wazuh SOC Detection Engineering Lab

**Author:** Vishnu  
**Role Focus:** SOC Analyst | Detection Engineer | Security Operations  
**Environment:** Ubuntu Server + Windows 10  
**Platform:** Wazuh 4.14.2  
**Framework Mapping:** MITRE ATT&CK  

---

## 📌 Project Summary

This project demonstrates the deployment and configuration of a Security Information and Event Management (SIEM) lab using Wazuh.  

The lab simulates real-world SOC operations including:

- Endpoint onboarding
- Log ingestion & analysis
- Custom detection rule engineering
- MITRE ATT&CK mapping
- Attack simulation
- Alert validation
- Active response automation

The goal was to build a production-style detection environment and validate detections using real attack scenarios.

---

## 🏗️ Lab Architecture

```
Ubuntu Server (Wazuh Manager + Indexer + Dashboard)
        |
        | Encrypted Agent Communication
        |
Windows 10 (Wazuh Agent)
```

---

## ⚙️ Phase 1 — SIEM Deployment

Installed on Ubuntu:

- Wazuh Manager
- Wazuh Indexer (OpenSearch)
- Wazuh Dashboard

Service validation:

```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard
```

---

## 🖥️ Phase 2 — Windows Agent Integration

Steps:

1. Installed Wazuh Agent on Windows 10
2. Registered agent with manager
3. Verified agent connectivity

```bash
sudo /var/ossec/bin/agent_control -l
```

Successfully onboarded Windows endpoint into the SIEM.

---

## 🎯 Phase 3 — Custom Detection Engineering

Custom rules were created inside:

```
/var/ossec/etc/rules/local_rules.xml
```

---

### 🔐 Rule 100500 — Brute Force Detection  
**MITRE Technique:** T1110 (Brute Force)

Detects 5 failed logins within 120 seconds.

```xml
<group name="windows,custom,">
  <rule id="100500" level="12" frequency="5" timeframe="120">
    <if_matched_sid>60122</if_matched_sid>
    <description>Windows Multiple Failed Logins - Brute Force Detected</description>
    <mitre>
      <id>T1110</id>
    </mitre>
  </rule>
</group>
```

---

### 🛡 Rule 100510 — Privilege Escalation  
**MITRE Technique:** T1078 (Valid Accounts)

```xml
<rule id="100510" level="10">
  <if_sid>60103</if_sid>
  <description>Windows Privilege Escalation Detected</description>
  <mitre>
    <id>T1078</id>
  </mitre>
</rule>
```

---

### ⚡ Rule 100520 — PowerShell Execution  
**MITRE Technique:** T1059.001 (PowerShell)

```xml
<rule id="100520" level="13">
  <if_group>windows</if_group>
  <field name="win.system.providerName">Microsoft-Windows-PowerShell</field>
  <description>PowerShell Execution Detected - Possible Abuse</description>
  <mitre>
    <id>T1059.001</id>
  </mitre>
</rule>
```

---

## 🧪 Phase 4 — Attack Simulation

Simulated the following:

- Multiple failed login attempts
- PowerShell execution
- Privilege escalation activity

Validated alerts using:

```bash
sudo /var/ossec/bin/wazuh-logtest
```

Dashboard queries used:

```
rule.id:100500
rule.id:100510
rule.id:100520
```

---

## 🔥 Active Response Automation

Created a bash script to block attacking IPs using iptables when brute-force rule fires.

Script location:

```
/var/ossec/active-response/bin/block_windows_ip.sh
```

Script contents:

```bash
#!/bin/bash
IP=$1
iptables -I INPUT -s $IP -j DROP
```

This enables automatic containment upon detection.

---

## 📊 Security Configuration Assessment (SCA)

Executed CIS Windows 10 Benchmark policy.

Results:

- 424 checks evaluated
- 29% compliance score
- Multiple password & lockout misconfigurations identified

Identified risk → Simulated attack → Detected via custom rule → Automatically blocked.

---

## 🧠 Skills Demonstrated

- SIEM deployment
- Endpoint log ingestion
- XML rule engineering
- MITRE ATT&CK mapping
- Windows Event Log analysis
- Threat simulation
- Linux troubleshooting
- Active response automation
- SOC investigation workflow

---

## 🚀 Future Improvements

- Add Sysmon-based detections
- Add credential dumping detection (T1003)
- Create SOAR-style automation
- Expand to Linux & Cloud agents
- Integrate threat intelligence feeds

---

## 🎯 Conclusion

This project demonstrates the complete SOC lifecycle:

Deployment → Log Ingestion → Detection Engineering → Attack Simulation → Alert Validation → Automated Response

It reflects real-world detection engineering and SOC analyst capabilities suitable for enterprise environments.

---
