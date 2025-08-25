🎯 Objective

To investigate Linux authentication logs for signs of unauthorized access attempts, identify suspicious users, and detect abnormal login behavior.

🛠️ Setup

* **Dataset used**: [`ssh_logs.json`](./logs/ssh_logs_UAI.json) 

Index created: main

Method: Uploaded the dataset into Splunk and used SPL queries to extract insights.

📊 Key SPL Queries

Total Success Events

index=main source="Linux_UnAuthorized_Auditd_logs.json" status="success"
| stats count as "Total Success Events"




Most Common Event

index=main source="Linux_UnAuthorized_Auditd_logs.json"
| stats count by event
| sort - count
| head 1


Path Accessed by User jaimin_pathak (uid=1010, accessed twice)

index=main source="Linux_UnAuthorized_Auditd_logs.json" uid=1010 user="jaimin_pathak"
| stats count by path
| where count=2

📌 Insights

Parsed Linux Auditd logs to detect successful and failed authentication events.

Identified the most frequent event type in the dataset.

Investigated activity of user jaimin_pathak (uid=1010) and found repeated access attempts to the same file path.

Learned how to correlate user, UID, status, and path fields to detect abnormal login patterns.