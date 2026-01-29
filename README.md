


 💻 # SSH Brute-Force Detection & Investigation using Splunk Cloud

## Overview
This project documents a SOC-led investigation into SSH brute-force activity observed within a cloud environment. 
The investigation focused on detecting, analyzing, and responding to repeated SSH authentication failures using Splunk Cloud.

The work was carried out collaboratively with a SOC analyst team and demonstrates real-world SOC workflows including:
- Centralized log ingestion
- Threat detection using SPL
- Dashboard development
- Alert tuning
- Threat intelligence correlation

## Objectives
- Detect anomalous SSH authentication behavior in near real time
- Identify brute-force patterns targeting privileged accounts
- Correlate suspicious IP addresses with threat intelligence sources
- Improve SOC visibility through dashboards and automated alerts
- Improve incident response efficiency through visibility and standardisation
  
## Tools & Technologies
- Splunk Cloud
- Regex
- SPL (Search Processing Language)
- AbuseIPDB (Threat Intelligence)
- SOC investigation methodology

## My Role & Collaboration
I collaborated with members of the SOC team throughout this project. My contributions included:
- Analyzing SSH authentication failure events
- Writing and refining SPL queries for brute-force detection
- Investigating suspicious source IP addresses
- Assisting in dashboard logic and alert refinement
- Supporting documentation of findings and recommendations

This project reflects a team-based SOC investigation rather than an individual lab exercise.

## Key Findings
- 496 SSH authentication failures were identified
- Root account was the most targeted user
- Significant brute-force spike detected within a short time window
- Multiple source IPs were confirmed malicious via threat intelligence
- No successful compromise was observed

## Outcome
The project improved SSH threat visibility and strengthened incident response capability through dashboards, alerts, and standardized investigation procedures.


