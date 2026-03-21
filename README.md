🛡️ Windows Event Log Analysis – RDP Attack Detection
1.  Objective

Detect and analyze failed logon attempts in a Windows Server environment using security event logs, focusing on identifying potential brute force attacks via Remote Desktop Protocol (RDP) and applying mitigation strategies.

2.  Environment

The lab environment was built using VMware Workstation to simulate a real-world Windows Server scenario.

🔧 Configuration
Virtualization Platform: VMware Workstation
<img width="1695" height="811" alt="image" src="https://github.com/user-attachments/assets/f62c73bd-ac46-49f0-b135-539d277dd88c" />

Operating System: Windows Server 2025
<img width="1526" height="840" alt="image" src="https://github.com/user-attachments/assets/e3cb630e-c17a-4f98-8b1e-2dccf3cbcc2c" />

Network Configuration: Bridged Network with DHCP enabled
<img width="1152" height="823" alt="image" src="https://github.com/user-attachments/assets/4347e4f7-7b5c-4b79-870a-a83b9aaec857" />

Remote Access: RDP enabled and exposed for testing
Logging: Windows Security Event Logs enabled via Event Viewer

This setup allowed the simulation and observation of failed logon attempts, particularly those related to remote access.

3.  Data Collection

Security event logs were collected using the Windows Event Viewer.
<img width="1744" height="309" alt="image" src="https://github.com/user-attachments/assets/5ac50313-c0f4-4f27-a8dc-a7aa98c6ae03" />

 Log Details
Tool Used: Event Viewer (Windows Logs → Security)

Event ID: 4625 (Failed Logon)

Log Source: Security Logs
 Fields Analyzed
Account Name (targeted user)
Source Network Address (origin IP)
Logon Type (authentication method)
Time Generated (timestamp)

Filters were applied to isolate failed authentication attempts and identify suspicious patterns.

## 4.  Analysis

The analysis of Event ID 4625 logs revealed multiple failed authentication attempts across different logon types.
<img width="1887" height="428" alt="image" src="https://github.com/user-attachments/assets/71dac9f2-ff25-4f99-b8e2-d0d05a6ee6a3" />
###  Logon Type Distribution
- Logon Type 3 (Network): 19 events  
- Logon Type 10 (RDP): 3 events  
- Logon Type 2 (Local): 5 events
A total of 27 failed login attempts were identified during the analysis period.
<img width="1494" height="340" alt="image" src="https://github.com/user-attachments/assets/849ab907-fd75-4031-8e76-1794112e10e9" />

<img width="1554" height="349" alt="image" src="https://github.com/user-attachments/assets/fa098f2b-5e40-4f9c-a542-7beddc67cc85" />
###  Observed Patterns

- Repeated login failures occurring within short time intervals  
- The same user account targeted multiple times  
- Presence of Logon Type 10 events, indicating RDP access attempts  
- Majority of attempts associated with Logon Type 3 (network-based authentication)  
###  Behavioral Analysis

- The high frequency of network-based failed logins (Logon Type 3) suggests possible automated or non-interactive authentication attempts  
- Repeated targeting of a single account indicates focused behavior rather than random user error  
- The presence of RDP-related failures confirms that remote access services are exposed and being tested  
- Source IP addresses identified as external/untrusted
<img width="1887" height="428" alt="image" src="https://github.com/user-attachments/assets/71dac9f2-ff25-4f99-b8e2-d0d05a6ee6a3" />

Overall, the activity indicates suspicious authentication behavior, with stronger evidence of network-based attempts and limited RDP involvement.

5.  Findings

The investigation identified multiple failed logon attempts consistent with suspicious authentication activity.

 Key Evidence
High frequency of failed logon events (Event ID 4625)
Repeated targeting of the same user account
Logon Type 10 confirming RDP access attempts
External source IP addresses involved
 Risk Assessment
- Repeated authentication attempts were observed from the same source IP address, indicating potential automated behavior

Risk Level: Medium to High

If not mitigated, this exposure could lead to unauthorized access, particularly in environments with weak credentials or no additional security controls.

6.  Mitigation

Based on the analysis, the following security measures are recommended:

6.1 Account Protection
Implement Account Lockout Policy
Enforce strong password policies.

6.2 Network Authentication Hardening (High Priority)
Restrict access to network services (SMB, RPC) via firewall
Allow connections only from trusted IP ranges
Disable unnecessary services.

6.3 RDP Security
Restrict RDP access by IP
Enable Network Level Authentication (NLA)
Disable RDP when not required.

6.4 Monitoring
Monitor Event ID 4625 continuously
Create alerts for:
High number of failed logins
Repeated attempts from a single IP
Integrate logs with SIEM (e.g., Wazuh, Splunk).

6.5 Additional Hardening
Rename or disable default accounts (e.g., Administrator)
Apply least privilege principle.

7.  Future Improvements
Automate detection using PowerShell scripts
Correlate failed (4625) and successful (4624) logins
Implement alerting and response automation

8.  Attack Indicators

The following indicators were identified during analysis:

- Multiple failed login attempts within short intervals  
- Repeated attempts targeting a single account  
- Authentication attempts from external IP addresses  
- Presence of both network and RDP-based logon attempts  



9.  Conclusion

This project demonstrated how to simulate and analyze failed authentication attempts in a Windows environment, replicating patterns commonly associated with brute force attacks.

The observed behavior highlights how exposed services like RDP can become targets and reinforces the importance of continuous monitoring and log analysis.

Additionally, the implementation of mitigation strategies emphasizes the concept of defense in depth, where multiple security layers are required to effectively reduce risk.

This project contributes to the development of practical skills in threat detection, analysis, and response within Windows-based environments.
