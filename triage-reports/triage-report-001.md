ALERT TRIAGE REPORT

Alert ID: 60122
Date/Time: 2026-06-09 21:18:58 UTC
Agent/Endpoint: WIN-D49VGC1NJJV (Agent 001, IP 192.168.64.2)
Alert Discription: Logon failure — Unknown user or bad password
Security Level: 5 (medium)

What happened: 
Wazuh detected multiple failed authentication attempts against the local machine
WIN-D49VGC1NJJV. The Attempts used the username FakeAdmin which does not exist on the
system. The authentication method was NTLM over a network logon (Type 3), originating from
the loopback address indicating the attempts were made locally.

Evidence Observed:
- Windows event ID 4625 (Authenticaiton failure) fired twice
- targetUserName: FakeAdmin (non-existent account)
- logonType: 3 (Network Logon)
- authenticaitonPackageName: NTLM
- sourceIpAddress: ::1(localhost loopback)
- statusCode: 0xc000006d (Unknown username or bad password)
- MITRE ATT&CK: T1078 Valid Accounts, T1531 Account Access Removal

Classification
True Positive

Analysis:
The pattern of repeated failed authentication attempts against a non-existent account is
consistent with a brute force or credential stuffing attack. The use of NTLM and a network
logon type suggests an automated tool or script was used. The source address beig localhost
is notable and may indicate an internal threat or a compromised process running on the
endpoint itself.

Recommended reponse:
1. Verify whether FakeAdmin exists in Active Directory or local user accounts 
2. Block Further authentication attempts from the source if external
3. Contact the endpoint owner to confirm whether the ativity was authorised
4. If unauthorised, isolate the endpoint pending further investigation
5. reset any potential compromised credentials
6. Escalate to L2 analyst per brute force response playbook

Analyst Notes:
Source IP was the loopback address (::1) suggesting the authentication attempt originatd
from the machine itself rather than an external source. This warrants further investigation
into what process initiated the requests.


