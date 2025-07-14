# NetworkSecurity-
A document with network security fixes for a specific organization that had network security issues.

These are the internal devices:
IP Address     | Role               | Services
--------------------------------------------------------
10.0.0.15      | Workstation (HR)   | FTP, Telnet
10.0.0.17      | Web Server         | HTTP
10.0.0.18      | Email Server       | HTTPS
10.0.0.25      | Domain Controller  | RPC, DNS
192.168.1.100  | IoT Camera         | FTP, Telnet
192.168.1.101  | Web Interface      | HTTP

Given the following scenario, I will be identifying anomalies, finding vulnerabilities and suggest fixes to secure the network.
You’re a junior network security analyst for a small company. Your manager gives you a network activity log and a list of internal machines. One of the machines might be infected with malware or misconfigured.  Your job is to investigate and secure the network. 

1. Identifying Anomalies:
*There are two “strange” IP addresses that are not on the internal device list. One being 10.0.0.1 which is known to be a private address, in the network log the email server is connected to it using port 443(HTTPS) this might look normal but let’s investigate it further to be sure.
*The other external IP address is 94.102.49.12, in the log an IoT camera is connected to this using port 6667 (an Internet Relay Chat port which is oftenly used by malware and botnets for command and control, this can mean the IoT camera is exposed)
*The HR workstation connects to the IoT camera repeatedly, 16 seconds apart firstly using FTP(21) and the second time using Telnet(23).

2.Spotting Vulnerabilities
*From the internal device list there are devices using insecure services which are FTP, Telnet, HTPP and 6667.
*The following insecure connections are shown and alternatives that should have been used for more secure connections.
[2025-04-22 08:12:43] 10.0.0.15 connected to 192.168.1.100:21 (FTP) 
[2025-04-22 08:12:50] 192.168.1.100 connected to 94.102.49.12:6667(IRC)
[2025-04-22 08:13:01] 10.0.0.15 connected to 192.168.1.100:23 (Telnet)
[2025-04-22 08:13:20] 10.0.0.17 connected to 192.168.1.101:80 (HTTP)
Alternatives to opt for:
FTP – SFTP
IRC - Encrypted  IRC via SSL/TLS
Telnet – SSH
HTTP – HTTPS
*Devices communicating externally:
[2025-04-22 08:12:50] 192.168.1.100 connected to 94.102.49.12:6667
-	Unusual port 6667 which is an insecure port, the IoT camera is exposed and is in risk of a malware infection. Usually, IoT devices use ports like SSH and never IRC.
[2025-04-22 08:14:01] 10.0.0.18 connected to 10.0.0.0.1:443 (HTTPS)
-	Since 10.0.0.1 is a private address, there might have been a misconfiguration that led to it being considered an external device.
-	The fact that this IP only has communication with the email server is a red flag.

3. Recommended Fixes
*Ports that should be blocked or restricted:
  - Block  port 6667 
  - Restrict outbound 443(HTTPS) only to known and approved Ips especially from email servers as malicious traffic can hide in payloads.
*Which services should be disabled or updated:
 - IRC: This is an outdated protocol and should be removed. The IoT camera is likely compromised. Disconnect it, reset and update it. 
-Update all services/ devices and ensure the latest patches are applied.
*Firewall Rule Change:
 - Block outbound port 6667 from all devices to prevent any IRC based command and control.


 

