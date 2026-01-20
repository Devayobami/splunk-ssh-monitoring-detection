

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
