
# Log Monitoring with Filebeat, Elasticsearch, and Kibana

This project demonstrates how to set up a simple log monitoring system using Filebeat, Elasticsearch, and Kibana. The guide covers creating a custom log file, configuring Filebeat to monitor it, sending the logs to Elasticsearch, and analyzing them in Kibana.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Step 1: Create a Custom Log File](#step-1-create-a-custom-log-file)
- [Step 2: Configure Filebeat](#step-2-configure-filebeat)
- [Step 3: Start Filebeat](#step-3-start-filebeat)
- [Step 4: Verify Elasticsearch](#step-4-verify-elasticsearch)
- [Step 5: Analyze Logs in Kibana](#step-5-analyze-logs-in-kibana)
- [Visualizations and Dashboards (Optional)](#visualizations-and-dashboards-optional)

## Prerequisites

- **Kali Linux** or another Linux distribution
- **Elasticsearch** and **Kibana** installed and running on localhost
- **Filebeat** installed and configured on your system

## Step 1: Create a Custom Log File

- Create a new log file to store custom log messages:
  ```bash
  sudo nano /var/log/new_custom_log.log

Add sample log entries to the file:

[INFO] 2024-08-16 15:00:00 - Application started
[ERROR] 2024-08-16 15:01:00 - Failed to open file
[WARN] 2024-08-16 15:02:00 - Low disk space

Save and exit the file.

# Step 2: Configure Filebeat

- Open the Filebeat configuration file:


sudo nano /etc/filebeat/filebeat.yml


Add the following input configuration:

filebeat.inputs:
- type: log
  paths:
    - /var/log/new_custom_log.log
  fields:
    log_type: new_custom_log

Ensure the output is set to Elasticsearch:

output.elasticsearch:
  hosts: ["localhost:9200"]

Save and exit the configuration file.

# Step 3: Start Filebeat
Restart the Filebeat service to apply the new configuration:

sudo systemctl restart filebeat

Check the status of the service to ensure it's running:

sudo systemctl status filebeat

# Step 4: Verify Elasticsearch
Check Elasticsearch to verify that logs are being indexed:

List Elasticsearch indices:
curl -X GET "localhost:9200/_cat/indices?v"

Search for the new logs:
curl -X GET "localhost:9200/filebeat-*/_search?pretty&q=log_type:new_custom_log"

# Step 5: Analyze Logs in Kibana

Access Kibana by navigating to http://localhost:5601 in your browser.
Create an Index Pattern

* Go to Stack Management > Index Patterns.
* Click Create index pattern.
* Enter filebeat-* as the index pattern.
* Select @timestamp as the time filter field.
* Click Create index pattern.

View Logs in the Discover Tab

Go to Discover in Kibana.
Select the filebeat-* index pattern.
Use the search bar to filter logs by type or other criteria:
fields.log_type: "new_custom_log"

# Visualizations and Dashboards (Optional)

Create Visualizations
* Navigate to Visualize Library.
* Create a new visualization using the filebeat-* index pattern.

Build Dashboards
* Go to Dashboard in Kibana.
* Add your visualizations to create a dashboard.

Conclusion
This guide provides a complete process for setting up a log monitoring system using Filebeat, Elasticsearch, and Kibana. You can expand on this setup by adding more log sources, creating complex visualizations, and setting up alerts.

License
This project is licensed under the MIT License - see the LICENSE file for details.




