aSYSTEM ERROR TRIAGE REPORT

Alert ID: 61110
Date/Time: Jun 10, 2026 @ 15:45:43.722
Agent/Endpoint: WIN-D49VGC1NJJV (Agent 001, IP 192.168.64.2)
Alert Discription: Multiple System error events
Security Level: 10 (High)

What Happened:
Wazuh detected multiple recurring system error events on edpoint WIN-D49VGC1NJJV. The errors originate from the Trusted Platform Module driver which is reporting a non-recoverable hardware failure. The TPM driver fired Event ID 15 eight times within a short window, triggering the aggregation rule at level 10. The errors indicate that TPM dependent services including data encryption are unavailable.

Evidence Observed:
- Windows event id 15 fired 8 times within seconds
- Provider: TPM
- Error message: TPM driver encountered non-recoverable hardware error
- Channel: System
- Severity: Error
- Rule fired times: 2 aggregation events
- All errors originated from the same process ID 8864

Classification:
Benign True Positive

Analysis:
The TPM errors are expected behaviour in this environment. The endpoint WIN-D49VGC1NJJV is a Windows 11 ARM VM running under UTM on Apple Silicon Hardware. VMs do not have access to a physical TPM chip. Everytime Windows attempts to yse TPM services the driver fails becasue the hardware does not exist. This is not indicative of malicious activity. The high severity level is a result of the rule counting multiple occurances of the same error rather than the individual error being high risk

Recommended response:
1. Confirm the endpoint is a VM and document this in the asset register.
2. Tune Rule 61110 to suppress TPM errors specifically for known VM endpoints
3. Add an exception in the SIEM for this endpoint and error type to reduce alert noise
4. No further investigation required for this specific alert

Analyst notes:
This alert generating significant noise due to the VM environment. Rule tuning is recommended to prevent alrt fatigue. in a real enterprise environment if this error appreared on a physical machine it would warrant immediate investigation as TPM failure on real hardware could indicate tampering or a rootkit attempting to diable encryption services.

