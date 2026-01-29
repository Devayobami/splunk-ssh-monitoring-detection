
## Dashboard Panels and Insights

## Total number of events (2,000): The dashboard tells us the total number of events which includes authentication attempts, disconnection events, and failure attempts logs. 

Splunk dashboard Query: 
source="OpenSSH.csv" host="si-i-03b61620c7fa0d684.prd-p-zpb7y.splunkcloud.com" sourcetype="csv" | stats count



## Authentication Failure from Unusual Geolocation: This highlight the geographic markers indicating where several authentication failures occurred. This helps attribute suspicious activity to specific regions, supporting geo-based threat intelligence. 

Splunk dashboard Query:
source="OpenSSH.csv" host="si-i-03b61620c7fa0d684.prd-p-zpb7y.splunkcloud.com" index="main" sourcetype="csv" "authentication failure" | rex "rhost=(?<rhost>\d+\.\d+\.\d+\.\d+)"
| iplocation rhost
| geostats count by Country



## Failed Password Trend: This visualizes the temporal pattern of failed SSH login attempts. A noticeable spike around 10:00 suggests a concentrated brute-force attempt during that hour.  

Splunk dashboard Query:
source="OpenSSH.csv" host="si-i-03b61620c7fa0d684.prd-p-zpb7y.splunkcloud.com" index="main" sourcetype="csv" "Failed password" | rex "from (?<src_ip>\d+\.\d+\.\d+\.\d+)" | timechart span=1h count



## Top Suspicious IP Trend: The chart highlights the source IPs responsible for high volumes of suspicious login activity.

Splunk dashboard Query:
source="OpenSSH.csv" host="si-i-03b61620c7fa0d684.prd-p-zpb7y.splunkcloud.com" index="main" sourcetype="csv" "Received disconnect from" | rex "disconnect from (?<src_ip>\d+\.\d+\.\d+\.\d+)" | stats count BY src_ip | sort -count



## Number of Failed Password Attempts – 520: This visualizes the total number of failed password attempts detected in the logs. This metric helps assess the scale of brute-force activity and supports alert prioritization.

Splunk dashboard Query:
source="OpenSSH.csv" host="si-i-03b61620c7fa0d684.prd-p-zpb7y.splunkcloud.com" index="main" sourcetype="csv" "failed password" | stats count



## Disconnected Session from 183.62.140.253 (285 events) & 103.99.0.122 (45 events): This indicates a high number of disconnected sessions from these IPs, suggesting possible session hijacking, forced terminations, or abnormal behaviour from this source.

Splunk dashboard Query: 
source="OpenSSH.csv" host="si-i-03b61620c7fa0d684.prd-p-zpb7y.splunkcloud.com" index="main" sourcetype="csv" "Received disconnect from 183.62.140.253" | stats count

      
AND


source="OpenSSH.csv" host="si-i-03b61620c7fa0d684.prd-p-zpb7y.splunkcloud.com" index="main" sourcetype="csv" "Received disconnect from 103.99.0.122" | stats count

