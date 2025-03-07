# Tracing the Attack: Data Breach Analysis at Premium House Lights  

## Project Overview  
This project is a **digital forensics investigation** into a data breach incident at **Premium House Lights Inc.** The analysis uncovers how an attacker **gained unauthorized access, escalated privileges, exfiltrated sensitive data, and demanded ransom.** This investigation follows an incident response workflow and applies forensic techniques using **Autopsy, Volatility, Wireshark, and log analysis** to identify the attack path, exploit vectors, and recommendations for remediation.

## Objectives  
- Investigate a **targeted cyberattack** leading to data exfiltration.  
- Perform **log analysis, network forensics, and memory forensics** to trace the attacker’s activities.  
- Identify **Tactics, Techniques, and Procedures (TTPs)** used in the attack.  
- Provide **actionable recommendations** to strengthen cybersecurity defenses.  

## Tools & Technologies Used  
- **Autopsy** - File system forensics  
- **Volatility** - Memory analysis  
- **Wireshark** - Network packet analysis  
- **Linux CLI & Log Analysis** - Investigating access logs  
- **SIEM Queries** - Log correlation and event detection  

## Attack Timeline  
The attacker followed the **cyber kill chain** to compromise the system:  
1. **Reconnaissance** - Scanned for vulnerabilities in the web application.  
2. **Exploitation** - Uploaded a **reverse shell** via an **insecure file upload endpoint** (`upload.php`).  
3. **Privilege Escalation** - Gained **root access** to the database server.  
4. **Data Exfiltration** - Extracted customer data using **mysqldump** and transferred it to an external server.  
5. **Extortion** - Sent a **ransom demand for 10 BTC** to prevent public exposure.  

## Technical Analysis  

### Evidence Collection  
- **Webserver logs** showed repeated unauthorized access attempts.  
- **Wireshark captures** confirmed outbound traffic to an **attacker-controlled IP**.  
- **Volatility analysis** identified suspicious **process execution** (reverse shell activity).  
- **Autopsy analysis** revealed unauthorized modifications to system files.  

### Attack Execution Breakdown  
| Attack Stage          | Evidence Collected  |
|----------------------|------------------|
| **Initial Compromise** | Log files show repeated GET/POST requests to `/upload.php` |
| **Reverse Shell Execution** | Wireshark shows shell communication with attacker's IP **(138.68.92.163:4444)** |
| **Privilege Escalation** | `sudo mysql -u root -p` executed for database access |
| **Data Exfiltration** | SCP transfer to external IP **(178.62.228.28)** |

### Weaknesses Exploited  
1. **Insecure File Upload** - No validation, allowing a **malicious PHP shell upload**.  
2. **Weak Network Segmentation** - Allowed **lateral movement** from webserver to database.  
3. **Poor Privilege Management** - Root access via SSH was enabled without restrictions.  
4. **Lack of Monitoring** - No **SIEM or IDS/IPS** detected the abnormal activity in real time.  

## Incident Response & Containment  
- **Isolated compromised systems** and disabled vulnerable endpoints.  
- **Blocked malicious IPs** and reset administrative credentials.  
- **Analyzed logs & memory dumps** to reconstruct the attack sequence.  
- **Deployed patches** for file upload security and network segmentation.  
- **Strengthened privilege access controls** and implemented **SIEM-based monitoring**.  

## Security Recommendations  
✔️ **Fix Web Upload Vulnerabilities** – Implement **input validation, restrict file types, and sanitize inputs**.  
✔️ **Enforce Network Segmentation** – Isolate **web and database servers** to **prevent lateral movement**.  
✔️ **Harden Access Controls** – Remove **root SSH access**, enforce **multi-factor authentication (MFA)**.  
✔️ **Deploy SIEM & IDS** – Implement **real-time monitoring** for **anomalous network traffic & privilege escalation**.  
✔️ **Regular Security Testing** – Conduct **penetration testing and vulnerability assessments**.  
✔️ **Incident Response Readiness** – Establish a **post-incident review policy** and train teams on IR workflows.  

## Key Takeaways  
- **Forensic techniques** revealed the full attack chain, emphasizing the importance of **log monitoring and access controls**.  
- **Network segmentation and SIEM implementation** could have detected and prevented the attack earlier.  
- **Proactive security measures**, such as **regular vulnerability scanning and least privilege enforcement**, are critical to reducing attack surfaces.  

## Supporting Files  
**PCAP captures, forensic logs, and attack evidence** are stored in this repository for reference
