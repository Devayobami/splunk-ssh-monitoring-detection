

💻# Centralized SSH Monitoring and Detection using Splunk Cloud

## Overview
This project demonstrates the design and implementation of centralized SSH monitoring and threat detection using Splunk Cloud. The objective was to identify, investigate, and respond to suspicious SSH activity such as brute-force attacks, authentication failures, and connections from unusual geolocations.

## Objectives
- Detect anomalous SSH authentication behaviour in real time
- Identify brute-force and credential-stuffing attempts
- Correlate SSH activity by IP address, user, time, and geolocation
- Build SOC-ready dashboards and automated alerts
- Improve incident response efficiency through visibility and standardisation

## Data Source
- OpenSSH authentication logs (OpenSSh.csv)
- Total events analysed: 2,000

## Key Findings
- 496 SSH authentication failure events identified
- Root account was the most targeted user
- Significant brute-force spike detected between 09:00–11:00
- Multiple malicious IPs identified and validated via threat intelligence
- No successful compromise observed

🛠️ ## Tools & Technologies
- Splunk Cloud
- SPL (Search Processing Language)
- Regex
- Threat Intelligence (AbuseIPDB)

📬 ## Deliverables
- SSH monitoring dashboard
- Automated alerts for suspicious IP activity
- Field extractions for source IP analysis
- Documented investigation and response workflow

## Outcome
This project enhanced SOC visibility into SSH threats, enabling faster detection, investigation, and escalation of suspicious activity in line with SOC best practices.


### STEPS TAKEN:


## The OpenSSH.csv log file was successfully ingested into the Splunk Cloud environment.

<img width="452" height="174" alt="image" src="https://github.com/user-attachments/assets/0f7761b5-0195-4bad-b514-f5eb30e17ffe" />

## Upon initial inspection of the ingested log file, the following suspicious terms were identified prior to conducting a full analysis: "Several Authentication failures", "Several failed password", "Invalid user"

<img width="451" height="228" alt="image" src="https://github.com/user-attachments/assets/8708208a-567c-4927-9ecb-1029dc15e627" />

## An SPL query was then executed to identify potential unauthorized access attempts and evaluate any indicators of suspicious or malicious activity.

<img width="452" height="180" alt="image" src="https://github.com/user-attachments/assets/c6dc6679-776e-48a5-8d69-e84cfe0194b7" />

## Further investigation into authentication attempts from remote hosts (rhost) involved to identify any IP addresses with more than five authentication failures.

<img width="452" height="202" alt="image" src="https://github.com/user-attachments/assets/c6d7af62-11ea-4312-b97e-b77872c0cd02" />

## An additional SPL query was used to identify user accounts with repeated authentication failures, helping highlight accounts that may be targeted or involved in potential brute‑force attempts, misconfigurations, or unauthorized access activity.

<img width="452" height="177" alt="image" src="https://github.com/user-attachments/assets/bc2bd5ab-07aa-4c91-8636-f7a25984595b" />


## The authentication failure logs were reviewed to identify login attempts originating from unusual or unexpected geographic locations.

<img width="452" height="266" alt="image" src="https://github.com/user-attachments/assets/26da4faa-7e5a-4226-9213-f0d7307e93f8" />

## These investigation examined patterns of failed password attempts and flagged unusual IP addresses that could indicate malicious activity.

<img width="451" height="184" alt="image" src="https://github.com/user-attachments/assets/ed347545-9bdc-4d59-b46a-b21e4ae5e50c" />

<img width="452" height="204" alt="image" src="https://github.com/user-attachments/assets/f9dd26d3-87a6-43da-a5bd-03fe4cd398fe" />

The Analysis revealed a significant spike in failed login attempts between 09:00 and 11:00 on December 5, 2025, peaking at 171 failures at 10:00. This sustained pattern, alongside activity from unusual IP addresses, is consistent with brute‑force, credential‑stuffing, or scanning behaviour. Further review of disconnection events showed repeated connection attempts from several IPs, with 183.62.140.253 generating 285 disconnects and two additional IPs (187.141.143.180 and 103.99.0.122) exhibiting similar probing activity. Threat‑intelligence checks via AbuseIPDB confirmed all three as malicious, linked to brute‑force and DDoS activity.


## DASHBOARD CREATION

A dedicated Splunk SSH dashboard was developed to enhance centralized visibility into authentication activity across the environment. The dashboard transforms raw log data into actionable security insights, supporting real‑time monitoring, cross‑system correlation, and incident response workflows. It consolidates key SSH security indicators, including total event volumes, authentication failures originating from atypical geographic regions, failed‑password trends, suspicious or high‑risk IP activity, and disconnection patterns associated with known malicious sources.

<img width="452" height="200" alt="image" src="https://github.com/user-attachments/assets/3520b5c5-a9c4-4f8e-9029-985b2d38d10b" />

<img width="452" height="200" alt="image" src="https://github.com/user-attachments/assets/85b0f376-4f82-4638-9c4e-28ab8d55931e" />

## SPLUNK DASHBOARD PANELS AND KEY INSIGHTS

## Total number of events: Splunk dashboard Query: 
source="OpenSSH.csv" host="si-i-03b61620c7fa0d684.prd-p-zpb7y.splunkcloud.com" sourcetype="csv" | stats count

## Authentication Failure from Unusual Geolocation: This panel highlights the geographic locations associated with authentication failures, helping attribute suspicious activity to specific regions and strengthening geo‑based threat intelligence. 
      Splunk dashboard Query:
source="OpenSSH.csv" host="si-i-03b61620c7fa0d684.prd-p-zpb7y.splunkcloud.com" index="main" sourcetype="csv" "authentication failure" | rex "rhost=(?<rhost>\d+\.\d+\.\d+\.\d+)"
| iplocation rhost
| geostats count by Country

## Failed Password Trend: This visualizes the temporal pattern of failed SSH login attempts. A noticeable spike around 10:00 suggests a concentrated brute-force attempt during that hour.    Splunk dashboard Query:
source="OpenSSH.csv" host="si-i-03b61620c7fa0d684.prd-p-zpb7y.splunkcloud.com" index="main" sourcetype="csv" "Failed password" | rex "from (?<src_ip>\d+\.\d+\.\d+\.\d+)" | timechart span=1h count

### Top Suspicious IP Trend: This panel identifies the source IP addresses generating the highest volumes of suspicious login activity.
    Splunk dashboard Query:
source="OpenSSH.csv" host="si-i-03b61620c7fa0d684.prd-p-zpb7y.splunkcloud.com" index="main" sourcetype="csv" "Received disconnect from" | rex "disconnect from (?<src_ip>\d+\.\d+\.\d+\.\d+)" | stats count BY src_ip | sort -count

## Failed Password Attempts (520): This metric shows the total volume of failed login attempts, helping gauge potential brute‑force activity and prioritize related alerts.
      Splunk dashboard Query:
source="OpenSSH.csv" host="si-i-03b61620c7fa0d684.prd-p-zpb7y.splunkcloud.com" index="main" sourcetype="csv" "failed password" | stats count

## Disconnected Sessions: IPs 183.62.140.253 (285 events) and 103.99.0.122 (45 events) generated unusually high volumes of session disconnects, indicating potential forced terminations, session hijacking attempts, or other abnormal activity.
    Splunk dashboard Query: 
source="OpenSSH.csv" host="si-i-03b61620c7fa0d684.prd-p-zpb7y.splunkcloud.com" index="main" sourcetype="csv" "Received disconnect from 183.62.140.253" | stats count
			                                                  AND
source="OpenSSH.csv" host="si-i-03b61620c7fa0d684.prd-p-zpb7y.splunkcloud.com" index="main" sourcetype="csv" "Received disconnect from 103.99.0.122" | stats count

### ENHANCING SSH DISCONNECTION ALERT CONFIGURATION IN SPLUNK CLOUD
This section documents the configuration of an automated Splunk Cloud alert designed to monitor SSH disconnection events associated with the previously identified suspicious IP address 183.62.140.253. The alert runs on a scheduled interval and triggers when disconnection activity surpasses a defined threshold. Its purpose is to provide early detection of potential unauthorized access attempts or related security anomalies, enabling timely SOC investigation and response.

<img width="452" height="98" alt="image" src="https://github.com/user-attachments/assets/c81687de-85a5-4f91-92b6-8a4e8547aa5e" />

## Field Extraction Setup
This section documents the creation of a custom Splunk field extraction designed to isolate source IP addresses from the OpenSSH events. Using Splunk’s built‑in field extractor and delimiter‑based parsing, a new field (src_ip) was generated to provide clear visibility into originating IP addresses and to support deeper security analysis across the dataset.

<img width="452" height="171" alt="image" src="https://github.com/user-attachments/assets/ad17021b-ad42-4989-99ec-71a59f845d5a" />

<img width="452" height="256" alt="image" src="https://github.com/user-attachments/assets/1cc770c5-f2fc-4d8b-b6ae-c3ccaf870f77" />

### CONCLUSION
The report demonstrates the successful implementation of centralized SSH monitoring within Splunk Cloud and the subsequent investigation of SSH‑related security events. Analysis of OpenSSH logs provided the SOC with comprehensive visibility into authentication activity across the cloud environment.
The investigation identified a high volume of failed SSH login attempts, including repeated attempts against privileged accounts such as root. Many originated from atypical geographic regions, and a concentrated spike in failures indicated behaviour consistent with automated brute‑force activity. Multiple external IP addresses were linked to persistent connection and disconnection attempts, and threat‑intelligence validation confirmed these hosts as malicious.
No successful unauthorized logins or signs of compromise were detected. However, the observed activity reflects ongoing attempts to breach the environment. The development of dashboards, field extractions, and automated alerts has strengthened the SOC’s ability to detect, analyse, and respond to SSH‑based threats in real time.
Overall, this initiative has enhanced security monitoring posture and established a solid foundation for continued SSH threat detection and incident response.

### RECOMMENDATION
The investigation’s findings, combined with SOC best practices, support several measures to strengthen SSH security and reduce exposure to attack:
• Apply Network‑Level Access Controls: Restrict SSH access through IP allow‑listing or firewall rules to limit connectivity to trusted management networks and approved administrative sources.
• Integrate Threat Intelligence: Automate enrichment of SSH events with external threat‑intelligence feeds to quickly identify and prioritise activity from known malicious IP addresses.
• Enforce Rate Limiting and Account Lockouts: Configure SSH rate‑limiting or temporary lockouts after repeated failed login attempts to reduce the effectiveness of brute‑force attacks.
• Review Dashboards and Alerts Regularly: Continuously evaluate dashboard metrics, alert thresholds, and detection logic to maintain effectiveness as threat patterns evolve.
• Maintain SSH Incident Response Runbooks: Establish and update formal runbooks for SSH‑related investigations to ensure consistent triage, escalation, and remediation across SOC operations.
