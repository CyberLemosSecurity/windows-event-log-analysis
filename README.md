Windows Event Log Analysis - RDP Attack Detection

1. Objective
Detect and analyze failed logon attempts in a Windows Server environment using security event logs, with a focus on identifying potential brute force attacks via Remote Desktop Protocol (RDP) and applying basic mitigation strategies.

2. Environment (VMware + Windows Server)
The lab environment was built using VMware Workstation to simulate a real-world Windows Server scenario.

- Virtualization Platform: VMware Workstation
![Untitled](https://github.com/user-attachments/assets/c8e9ffac-2c81-4eec-b8e1-41a79f02c29e)

- Operating System: Windows Server 2025
<img width="1526" height="840" alt="image" src="https://github.com/user-attachments/assets/0a47713d-78b0-490d-9540-86e129883581" />
- Network Configuration: Bridge with DHCP Activate.
<img width="1152" height="823" alt="image" src="https://github.com/user-attachments/assets/72aa2b72-8b5d-4f5d-bb72-597e240e6275" />

- Remote Access: RDP (Remote Desktop Protocol) enabled and exposed for testing
- Logging: Windows Security Event Logs enabled (Event Viewer)

This setup allowed the simulation and observation of failed logon attempts, particularly those related to RDP access.

3. Data Collection (Event Viewer - ID 4625)

Security event logs were collected using the Windows Event Viewer, focusing on failed logon attempts.
<img width="1744" height="309" alt="a7be3eba-44a3-4daf-a916-1ad873d06ad3" src="https://github.com/user-attachments/assets/896bc7db-5c82-4414-8eea-92d15814f165" />
- Tool Used: Event Viewer (Windows Logs > Security)
- Event ID: 4625 (Failed Logon)
- Log Source: Security Logs

Filters were applied to isolate Event ID 4625 entries in order to identify unsuccessful authentication attempts.

The following key fields were analyzed:
- Account Name (targeted user)
- Source Network Address (origin of the attempt)
- Logon Type (method of access, e.g., RDP)
- Time Generated (timestamp of each attempt)

This approach allowed the identification of repeated failed logon patterns and potential unauthorized access attempts.
   
4. Analysis
The analysis of Event ID 4625 logs revealed a high frequency of failed logon attempts within short time intervals, indicating a non-human pattern of activity.

Multiple failed attempts were observed targeting the same user account, suggesting a possible brute force attack.

Key observations:

- Repeated login failures occurred within seconds
<img width="1494" height="340" alt="image" src="https://github.com/user-attachments/assets/b06a5bac-cbd0-4e40-bf6f-a7fa865744f3" />

- The same account was targeted multiple times
- Logon Type 10 was identified in several events, indicating attempts via Remote Desktop Protocol (RDP)
- Source Network Address showed external IP addresses, not associated with the internal network
<img width="1887" height="428" alt="image" src="https://github.com/user-attachments/assets/5ca4573e-b734-471d-82fc-a49c446d56ff" />

The pattern of behavior strongly suggests automated login attempts rather than manual user error.

This activity is consistent with brute force or credential stuffing attacks commonly observed on exposed RDP services.
<img width="1554" height="349" alt="image" src="https://github.com/user-attachments/assets/f71c4115-d745-458f-96b6-85d18babb42f" />

5. Findings
The investigation confirmed the presence of multiple failed logon attempts consistent with a brute force attack targeting the RDP service.

Evidence supporting this conclusion includes:

- High frequency of failed logon events (Event ID 4625) within short time intervals
- Repeated targeting of the same user account
- Logon Type 10 identified, indicating Remote Desktop (RDP) access attempts
- Source Network Addresses originating from external, non-trusted IPs

This behavior is characteristic of automated attack tools attempting to gain unauthorized access.

Risk Level: Medium to High

If left unmitigated, this exposure could lead to successful unauthorized access, especially if weak credentials are in use.

This suggests either:
- Incorrect credential usage from a legitimate internal system
- Misconfigured services attempting authentication
- Or potential unauthorized lateral movement within the network

Although not confirmed as malicious, the behavior is considered suspicious and requires further investigation.

6. Mitigation
Based on the analysis of failed login attempts, the following measures are recommended:

### 1. Account Protection
- Implement **Account Lockout Policy** to block repeated failed attempts  
- Enforce strong password policies (complexity and length)  

### 2. Network Authentication Hardening (High Priority)
- Restrict access to network services (SMB, RPC) using firewall rules  
- Limit inbound connections to trusted IP addresses only  
- Disable unnecessary network services to reduce attack surface  

### 3. RDP Security (Secondary Risk)
- Restrict RDP access to specific IP ranges  
- Enable **Network Level Authentication (NLA)**  
- Disable RDP if not required  

### 4. Monitoring and Detection
- Continuously monitor **Event ID 4625** for abnormal patterns  
- Create alerts for:
  - High number of failed logins in short time  
  - Repeated attempts from a single IP  
- Integrate logs with a SIEM (e.g., Wazuh, Splunk)  

### 4. Additional Hardening
- Rename or disable default accounts (e.g., Administrator)  
- Use least privilege principle for user accounts  
 ### 6. Future Improvement
- Implement automated detection using PowerShell scripts  
- Correlate failed (4625) and successful (4624) logins  
- Block IPs dynamically after threshold violations  

7. Conclusion
Through this project, it was possible to simulate and analyze failed authentication attempts in a Windows environment, replicating patterns commonly associated with brute force attacks.

Although the activity originated from a controlled internal source, the observed behavior closely mirrors real-world attack scenarios, demonstrating how easily exposed services like RDP can become targets.

This exercise emphasized the importance of log analysis as a critical component of security monitoring and highlighted the risks of inadequate access controls.

The implementation of mitigation strategies further reinforced the concept of defense in depth, showing that effective security relies on multiple layers of protection rather than a single control.

This project contributes to the development of practical skills in threat detection, analysis, and response within Windows-based environments.

