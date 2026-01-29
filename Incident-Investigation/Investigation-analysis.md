## DATA INGESTION INTO SPLUNK CLOUD
The OpenSSH.csv log file was successfully ingested into the Splunk Cloud environment. 

## Log File Analysis 
This report documents the initial observations made from the ingested log data in Splunk Cloud before detailed analysis commenced. The review focuses on the volume of ingested events and early indicators of suspicious activity.

Spl query (Total events captured)
source="OpenSSH. csv" index="main" index="main" sourcetype="csv"


## Event Summary
The Total Number of Events Indexed: 2,000 events
◦ This count reflects the volume of log entries successfully ingested and indexed by Splunk Cloud from the uploaded source.

Suspicious Notifications Identified
Before full analysis began, the following suspicious log entries were observed:
1.	Repeated Authentication Failures: Multiple entries were flagged with failed login attempts, suggesting potential brute-force or unauthorized access attempts.
2.	Unusual SSH Activity: Log lines referencing sshd with error codes and repeated connection attempts from similar IP addresses.
3.	Alert Keywords Detected: Terms such as “Failed password”, “Disconnected from” and “Invalid user” appeared frequently, indicating possible reconnaissance or credential stuffing behaviour.


## Log Analysis by “Authentication Failure”
To detect potential unauthorized access attempts and assess whether any suspicious or malicious activity was present and this returned 496 events. 

 Spl query
 source="OpenSSH. cv" host=*si-i-05abe@bb1fb2ffb74.prd-p-9jbli.splunkcloud.com" index="main" sourcetype='csv* "authentication failure"
I dedup _raw

## Authentication attempts by remote host(rhost)
The Splunk SPL was queried to identify remote IP addresses (rhost) with more than five authentication failure events.

Spl query
source="OpenSSH. csv" host="si-i-03b61620c7fa@d684.prd-p-zpb7y.splunkcloud.com" index="main" sourcetype="csv" "authentication failure"
| rex "rhost-(?<rhost>\d{1,3}(?: \. \d(1, 3})(3})" | stats count by rhost
I where count > 5
I sort -count


## Authentication Failure by Users
The Splunk SPL was queried to identify user accounts with the most authentication failure events. The goal is to detect which user are most frequently targeted or involved in failed login attempts, which may indicate brute-force attacks, misconfigurations, or unauthorized access attempts.

Spl query
source-"OpenSSH. csv" host="si-i-03b61620c7fa@d684.prd-p-zpb7y.splunkcloud.com" index-"main" sourcetype="csv" "authentication failure"
1 rex "user-f?<user>\w+)*
I stats count by user I sort -count


## Authentication Failure from Unusual Geographic region
The log was further queried to identify unusual login attempts originating from unexpected geographic locations.

Spl query
source="OpenSSH. csv" host="si-i-03b61620c7fa@d684.prd-p-zpb7y.splunkcloud.com" index-"main" sourcetype="csv" "authentication failure"
Time range: All time -
I rex "rhost=(?<rhost>\d(1,3)(?:\. \d(1,3)) (3))"
I iplocation rhost
I stats count by rhost Country City | sort -count



## Investigation into Failed Password Trend & Unusual IP addresses attempt Events.
The investigation focused on detecting failed login attempts and identifying unusual IP addresses that may indicate malicious activity.

Spl query for failed password
source="OpenSSH.csv" host="si-i-05abe@bb1fb2ffb74.prd-p-9jbli.splunkcloud.com" index-"main" sourcetype="csv" "Failed password"|rex "from (?<src_ip>\dt1.ldt\.ld
Time range: Last 24 hours v
+\. Id+)" I timechart span=1h count



Unusual IP Address Activity
The investigation reviewed disconnection events from IP addresses attempting to connect to the infrastructure. A Splunk query using rex was used to extract the source IP addresses, helping identify activity that may indicate failed logins

Spl query
source="OpenSSH. cv" host="si-i-05abe0bb1fb2ffb74.prd-p-9jbli.splunkcloud.com" index="main" sourcetype="csv" "Received disconnect from" | rex "disconnect from
(?<src_ip>\d+\. \d+\. \d+\. Id+)" | stats count BY src_ip | sort -count
































