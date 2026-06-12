SYSTEM ERROR TRIAGE REPORT

Alert ID: 52002
Date/Time: Jun 10, 2026 @ 16:41:55.622
Agent/Endpoint: ip-172-31-23-17 (Agent 000 — Wazuh Server, Ubuntu Linux)
Alert Discription: Apparmor DENIED
Security Level: 3 (low)

What Happened:
Wazuh detected multiple recurring Amppamor DENIED error events on Wazuh server itself. The Linux Apparmor security module blocked the built-in who command from reading a locale configuration file. The rule fired 8 times within the same window suggesting repeated attempts by the same process. 

Evidence Observed:
- Rule 52002 fired 8 times
- Operation: open - a file read was attempted
- Program: who (Linux system command)
- File blocked: /usr/share/coreutils/locales/uucore/en-US.ftl
- Action: read(r) was requested and denied
- process ID 3453
- Running as: root(fsuid=0)
- Source: /var/log/kern.log on Wazuh erver

Classification:
Benign True Positive

Analysis:
The who command is a standard Linux utlility used by Wazuh internally to monitor logged-in users. The Apparmor profile for the who command does not include read permission for the locale file path being accessed. This is a profile gap rather than malicious activity. The event originated on the Wazuh server itself not on a monitored endpoint. No attacker behaviour is indicated

Recommended response:
1. Confirm this is expected behaviour by checking Wazuh documentation for known Apparmor interactions.
2. Consider updating the Apparmor profile for the who command to include the locale file path.
3. If this fires repeatedly consider tuning rule 52002 to suppress known benign Apparmor denials
4. No escalation required

Analyst notes:
Alert originated on the SIEM server itself not on the Windows endpoint. Misidentifying the source agent is a common mistake - always check the agent name field before writing conclusions. The who command running as root is normal for Wazuh monitoring processes.

