# Home SIEM Lab: Wazuh + AWS + Windows Endpoint

## Project Summary
I built a home Security Operations Centre lab where a Windows endpoint is monitored by a cloud-hosted SIEM. I deployed Wazuh on an AWS EC2 instance as the central monitoring platform, connected a Windows 11 virtual machine as a monitored endpoint using the Wazuh agent, and simulated real attack scenarios including brute force attempts and reconnaissance activity. I then triaged the resulting alerts and documented my findings in professional investigation reports.

## Lab Architecture  

The Technologies used for the project consisted of AWS EC2 instance where the Wazuh server is deployed. Windows 11 VM through UTM on Mac which has the Wazuh agent setup and finally the Wazuh Dashboard is accessed through the browser.

Windows 11 VM - UTM on Mac
        
        | Wazuh Agent (port 1514/1515)
        
Wazuh Server - AWS EC2 Sydney
        
        | HTTPS (port 443)
        
Wazuh Dashboard - Browser

## Tools and Technologies
| Tool | Version | Purpose |
|---|---|---|
| Wazuh | 4.7.5 | SIEM platform |
| AWS EC2 | c7i-flex.large | Cloud server hosting |
| Ubuntu | 26.04 LTS | Wazuh server OS |
| Windows 11 ARM | Build 26200 | Monitored endpoint |
| UTM | Latest | Virtualisation on Apple Silicon |
| PowerShell | 5.1 | Attack simulation |
| MITRE ATT&CK | v14 | Threat classification |

## What I Built
- Deployed Wazuh 4.7.5 on an AWS EC2 Ubuntu instance in the Sydney 
  region, configured with appropriate security group rules to allow 
  agent communication on ports 1514 and 1515 and dashboard access on 
  port 443
- Provisioned a Windows 11 ARM virtual machine using UTM on Apple 
  Silicon, installed and registered the Wazuh agent, and verified active 
  agent status in the Wazuh dashboard
- Assigned a static Elastic IP to the AWS instance to ensure consistent 
  agent connectivity across server restarts

## Attack Simulations and Findings
- I simuluated a **brute force attack** where multiple attempts were made to sign into the system with incorrect credentials. This was logged by the Wazuh SIEM as a 'Logon failure' with an event ID of 60122. The recommended steps were to verify if this user existed in the system through Active Directory, block further attempts, contact the endpoint owner, isolate the endpoint, reset credentials and escalate to L2 analyst.
- Reviewed a **TPM Hardware Error** (Benign True Positive), here the SIEM had detected multiple system error events simultaneously which is why it labeled the severity as a 10/12. The issue was to do with Trusted Platform Module driver which was not responding. This issue was expected from the Virtual Machine as it did not have a physical TPM chip so there was no malicious activity. The recommended steps were to tune rule 61110 to suppress the TPM errors specifically for known VM endpoints and add an exception in the SIEM for this endpoint and error type to reduce alert noise.
- Reviewed an **Apparmor DENIED Event** (Benign True Positive), here the SIEM detected multiple recurring Apparmor DENIED error events on Wazuh server itself. The Linux Apparmor security module blocked the built-in who command from reading a locale configuration file. The who command is a standard Linux utility used by Wazuh internally to monitor logged-in users. The Apparmor profile for the who command does not include read permission for the locale file path being accessed. This is a profile gap rather than malicious activity. The recommended steps were to consider updating the Apparmor profile for the who command to include the locale file path.

## Key Learnings
- Deployed Wazuh SIEM on AWS EC2 including cloud infrastructure 
  configuration, firewall rules, and SSH access management
- Connected a Windows 11 ARM virtual machine as a monitored endpoint 
  using the Wazuh Agent, including agent registration and service configuration
- Simulated real attack techniques including brute force authentication 
  attacks and system reconnaissance
- Applied the SOC analyst triage workflow to classify and investigate 
  security alerts using MITRE ATT&CK framework context
- Wrote professional triage investigation reports documenting evidence, 
  classification, analysis and recommended response for each alert
- Identified and documented the importance of SIEM rule tuning to reduce 
  alert fatigue from known benign events




