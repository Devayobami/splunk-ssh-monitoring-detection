## CONCLUSION
This report describes how centralized SSH monitoring was successfully set up in Splunk Cloud and how SSH security events were investigated. By collecting and analysing OpenSSH logs, the SOC gained clear visibility into all SSH login activity across the cloud environment.
The investigation found many failed SSH login attempts, with repeated attacks targeting privileged accounts such as the root user. Many of these attempts came from unusual geographic locations, and a short time window showed a sharp increase in failed logins, which is typical of automated brute-force attacks. Several external IP addresses were responsible for repeated connection and disconnection attempts, and threat intelligence checks confirmed these IPs are known to be malicious.
No successful SSH logins or system compromise were identified during the investigation. However, the activity observed shows ongoing attempts to gain unauthorized access. The creation of dashboards, field extractions, and automated alerts has improved the SOC’s ability to quickly detect, investigate, and respond to SSH-related threats in real time. 
Overall, this project improved security monitoring and incident response capabilities and provides a strong foundation for continued SSH threat detection and analysis.


## RECOMMENDATION
Based on the findings of this investigation and SOC best practices, the following recommendations are proposed to further reduce SSH attack surface and enhance security controls:
•	Implement Network-Level Access Controls: To apply IP allow-listing or firewall rules to restrict SSH access to only trusted management networks and known administrative IP ranges.
•	Integrate Threat Intelligence Feeds: To automate the enrichment of SSH events with external threat intelligence sources to enable faster identification and prioritization of known malicious IP addresses.
•	Enable Account Lockout and Rate Limiting: Configure SSH rate limiting or temporary account lockouts after multiple failed logins attempts to mitigate brute-force activity.
•	Regular Dashboard and Alert Review: Ensure to review dashboards, alert thresholds, and detection logic to ensure continued effectiveness as threat patterns evolve.
•	Develop and Maintain SSH Incident Runbooks: To formalize investigation and response procedures for SSH-related alerts to ensure consistent triage, escalation, and remediation by SOC analysts.


