Windows Event Log Analysis - RDP Attack Detection

1. Objective
Detect and analyze failed logon attempts in a Windows Server environment using security event logs, with a focus on identifying potential brute force attacks via Remote Desktop Protocol (RDP) and applying basic mitigation strategies.

2. Environment (VMware + Windows Server)
The lab environment was built using VMware Workstation to simulate a real-world Windows Server scenario.

- Virtualization Platform: VMware Workstation
- Operating System: Windows Server 2022
- Network Configuration: NAT with manually assigned static IP address
- Remote Access: RDP (Remote Desktop Protocol) enabled and exposed for testing
- Logging: Windows Security Event Logs enabled (Event Viewer)

This setup allowed the simulation and observation of failed logon attempts, particularly those related to RDP access.

3. Data Collection (Event Viewer - ID 4625)
Security event logs were collected using the Windows Event Viewer, focusing on failed logon attempts.

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
<img width="1744" height="309" alt="a7be3eba-44a3-4daf-a916-1ad873d06ad3" src="https://github.com/user-attachments/assets/896bc7db-5c82-4414-8eea-92d15814f165" />
   
4. Analysis
The analysis of Event ID 4625 logs revealed a high frequency of failed logon attempts within short time intervals, indicating a non-human pattern of activity.

Multiple failed attempts were observed targeting the same user account, suggesting a possible brute force attack.

Key observations:

- Repeated login failures occurred within seconds
- The same account was targeted multiple times
- Logon Type 10 was identified in several events, indicating attempts via Remote Desktop Protocol (RDP)
- Source Network Address showed external IP addresses, not associated with the internal network

The pattern of behavior strongly suggests automated login attempts rather than manual user error.

This activity is consistent with brute force or credential stuffing attacks commonly observed on exposed RDP services.

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

<img width="786" height="119" alt="image" src="https://github.com/user-attachments/assets/6b47127e-906b-441b-9c22-c010350ea90a" />


10. Mitigation
To mitigate brute force attacks targeting RDP services, the following security measures are recommended:

- Restrict RDP access using firewall rules or VPN instead of exposing it directly to the internet
- Implement account lockout policies to prevent repeated login attempts
- Enforce strong password policies to reduce the risk of credential compromise
- Limit RDP access to authorized users only (principle of least privilege)
- Enable monitoring of failed logon attempts (Event ID 4625) for early detection
- Apply Multi-Factor Authentication (MFA) where possible
- Optionally change the default RDP port to reduce automated scanning attempts

These measures significantly reduce the attack surface and improve the overall security posture of the system.

12. Conclusion
Through this project, it was possible to simulate and analyze failed authentication attempts in a Windows environment, replicating patterns commonly associated with brute force attacks.

Although the activity originated from a controlled internal source, the observed behavior closely mirrors real-world attack scenarios, demonstrating how easily exposed services like RDP can become targets.

This exercise emphasized the importance of log analysis as a critical component of security monitoring and highlighted the risks of inadequate access controls.

The implementation of mitigation strategies further reinforced the concept of defense in depth, showing that effective security relies on multiple layers of protection rather than a single control.

This project contributes to the development of practical skills in threat detection, analysis, and response within Windows-based environments.

