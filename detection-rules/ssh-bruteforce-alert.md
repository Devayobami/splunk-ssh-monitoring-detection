
## REFINING ALERT CREATION IN SPLUNK CLOUD

This report outlines the creation and configuration of an automated alert in Splunk Cloud to monitor SSH disconnection events originating from the previously identified suspicious IP address. The alert was configured to run on a scheduled interval and is triggered when the volume of SSH disconnection events exceeds a predefined threshold.
The purpose of this alert is to provide early visibility into potentially unauthorized access attempts or other security-related anomalies associated with this external host, enabling the SOC to quickly investigate and respond to suspicious SSH activity.


## Alert Query: 
source="OpenSSH.csv" host="si-i-03b61620c7fa0d684.prd-p-zpb7y.splunkcloud.com" index="main" sourcetype="csv" "Received disconnect from 183.62.140.253"


