# Wazuh-SOC-Detection-Lab
A home SOC lab using 3 VMs (Kali Linux, Windows, Wazuh) focusing on Windows security monitoring and custom detection engineering.
## Project overview
This project is a home SOC lab. I built this to practise security monitoring, log analysis and detection engineering using Wazuh. This project also helped me to have a better understanding of how SOC analysts work and what they look for in security alerts, but also to enjoy myself and have a bit of fun experimenting with different operating systems and playing around with Wazuh.

-Kali linux - controlled security testing 
-Windows - monitored endpoint
-Wazuh - detections and alerts, security monitoring, collecting logs. The main aim of the project was to collect Windows security telemetry, investigate the events in Wazuh and create custom detection rules for different types of activity.

## Lab Architecture
To build this lab i used virtual box to creat my 3 VMs and connected them through a Host-Only network.
- Kali Linux -192.168.56.102
- Windows - 192.168.56.101
- Wazuh - 192.168.56.103

Kali Linux would be used to generate controlled security activity, while Windows would act as the target and monitored VM. The Wazuh Agent would collect the logs and security events from Windows, which would then be sent to the Wazuh Manager for analysis and storage. The Wazuh Dashboard would display the collected events and alerts so they could be investigated.

## Security Monitoring
Windows security telemetry was enabled and collected by the Wazuh Agent.
The project included monitoring for:

- File Integrity Monitoring
- Windows Firewall events
- Process creation
- Failed Windows logons
- Blocked SMB connections

## File integrity monitoring
Wazuh File Integrity Monitoring was configured on the Windows endpoint.
The Windows Startup folder and relevant registry locations were monitored for changes. I then created a test file in the Startup folder to confirm that Wazuh detected the changes. I also changed the contents of the test file to confirm that this would also be detected.

The resulting alerts showed that the file had been added and later modified, including changes to its file hash.

## custom detections
I created 4 customer wazuh rules which were all tested.

### Rule 100050 — Port Scan Detection
This rule detects repeated Windows Firewall blocks from the same source IP within 10 seconds.
The rule triggers after 5 matching events and is mapped to MITRE ATT&CK T1046 — Network Service Scanning.
The detection was tested using controlled Nmap scanning activity from Kali against Windows.

### Rule 100060 — Suspicious Process Detection
This rule detects the creation of commonly used Windows command and scripting interpreters.
The rule detects:
-PowerShell
-CMD
-WScript
-CScript
-MSHTA

The rule uses a PCRE2 regular expression to match the executable names regardless of capitalisation, for example PowerShell, powershell or POWERSHELL. It is mapped to MITRE ATT&CK T1059 — Command and Scripting Interpreter. The detection was tested by launching each of the monitored processes on the Windows VM.

### Rule 100070 — Brute-Force Detection
This rule detects repeated Windows logon failures against the same username. The rule triggers after 5 matching failures within 60 seconds. It is mapped to MITRE ATT&CK T1110 — Brute Force. The detection was tested using controlled failed logon attempts against the Windows account.

### Rule 100080 — Blocked SMB Connection Detection
This rule detects blocked Windows Firewall connections to TCP port 445. Port 445 is commonly used by SMB. The rule is mapped to MITRE ATT&CK T1021.002 — SMB/Windows Admin Shares. The detection was tested by generating a controlled connection attempt from Kali to port 445 on the Windows VM.


The custom rules were created using existing Wazuh rules as parent or matched rules. This allowed the custom detections to build on the Windows events already being processed by Wazuh.

## Trouble shooting and Investigations
The main source of learning for me in this project was for sure trouble shooting, coming up against a problem and finding a fix. 
Some of the custome rules did not initially trigger as expected so i had to invistaget why this was and then come up with a solution. For the port scan detetction, it didnt initally trigger so i investiagted the Windows Firewall and traced the wazuh rule heirachy to identify rule 60104 as the relvent parent rule. i used the wazuh alert generated from the Windows event to investigate  the existinf rules and trace the rule hierachy which allowed me to understand what i needed to change in my custom rule script.
My Proccess detection rule didnt work initially either so i investigated and founf that the field matching was the issue and that it needed to use the PCRE2 regex type so Wazuh would interpret the field as a regular expression. I updated the rule to use type ="pcre2", after which the dtection successfully matched the required process names.
My brute force rule i initally used the wrong parent, i discovered this beacuse wazuh would already detect a failed login however and i was building my brute force rule from that, i found that the windows event ID 4625 was being detected by rule 60122. i updated the custom rule to use 60122, after which the detction rule triggered succesfully.

## Skills Demonstrated

* Wazuh deployment and configuration
* Windows security monitoring
* Security event analysis
* File Integrity Monitoring
* Windows Firewall monitoring
* Process creation monitoring
* Detection engineering
* Wazuh rule development
* Regex and PCRE2 matching
* Rule hierarchy investigation
* Event correlation
* MITRE ATT&CK mapping
* Basic SOC investigation and troubleshooting
* Evidence collection and documentation

# project outcome
Honestly, as irritating as running into issues can be, finding a way to resolve them is always the best way to explore and learn new things. This project gave me practical experience building a small SOC environment from the ground up. I configured Windows telemetry, investigated Wazuh and how it processed events, developed custom detection rules, generated security activity using Linux and also used SSH to remotely access Wazuh from my host device, as I found it to run smoother and save me time.
I worked with different types of security activity throughout the project, including network enumeration, port scanning, SMB connection attempts, failed logon attempts and Windows process creation. I also configured and tested File Integrity Monitoring to understand how changes to files on a Windows endpoint could be detected and investigated.
A big part of the project was troubleshooting detections that did not work straight away. I investigated Wazuh alerts, traced existing rule hierarchies, identified the correct parent or matched rules and investigated event fields to understand why a detection was not triggering. This included fixing the process detection by using the PCRE2 regex type so the rule could correctly match the required process names.
Overall, this project gave me practical experience with security monitoring, log analysis, enumeration, Windows security events, detection engineering, Wazuh rule development, MITRE ATT&CK mapping and basic SOC investigation. More importantly, it showed me that problems are often where the most useful learning happens and gave me a solid foundation to take into a more advanced SOC project.

This was my first home lab, and I built it as a fun educational project. I plan to continue building more projects and aim to make each one more advanced than the last.
