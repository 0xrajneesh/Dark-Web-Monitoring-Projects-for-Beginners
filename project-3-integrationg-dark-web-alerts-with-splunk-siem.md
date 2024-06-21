# Integrating Dark Web Alerts with SIEM Systems

## Introduction
In this project, you will learn how to integrate dark web monitoring alerts with a Security Information and Event Management (SIEM) system. SIEM systems help in aggregating and analyzing security data from various sources, enabling real-time threat detection and response. You will use SpiderFoot for dark web monitoring and integrate its alerts with a popular SIEM system such as Splunk.

## Lab Setup
- **Operating System**: Linux (Ubuntu 20.04 recommended)
- **Tools Required**: SpiderFoot, Splunk

### Tool Installation

#### 1. Installing SpiderFoot
- Install SpiderFoot by following these steps:

```bash
sudo apt update
sudo apt install git python3 python3-pip
git clone https://github.com/smicallef/spiderfoot.git
cd spiderfoot
pip3 install -r requirements.txt
```
- Run SpiderFoot:

```bash
python3 sf.py
```
- Access SpiderFoot via a web browser at `http://localhost:5001`

2. Installing Splunk
- Download Splunk from the official website: Splunk Downloads

```bash
wget -O splunk-8.2.2-linux-2.6-amd64.deb 'https://download.splunk.com/products/splunk/releases/8.2.2/linux/splunk-8.2.2-linux-2.6-amd64.deb'
sudo dpkg -i splunk-8.2.2-linux-2.6-amd64.deb
```
- Start and enable Splunk:

```bash
sudo /opt/splunk/bin/splunk start --accept-license
sudo /opt/splunk/bin/splunk enable boot-start
```
## Exercises

### Exercise 1: Configuring SpiderFoot for Dark Web Monitoring
1. Open SpiderFoot: Navigate to `http://localhost:5001` in your web browser.
2. Create a New Scan: Click on "New Scan".
3. Set Scan Target: Enter a target domain or keyword related to your dark web interest.
4. Configure Modules: Enable modules related to the dark web. For instance, enable the tor module.
5. Start Scan: Click "Run Scan" and monitor the progress.

**Expected Output**
- You should see a list of discovered data related to your target, including links and content from dark web sites.

### Exercise 2: Exporting SpiderFoot Scan Results
1. View Scan Results: After the scan completes, click on the scan ID to view results.
2. Export Results: Export the scan results in CSV format.
3. Go to the "Export" tab in the scan results page.
4. Choose CSV format and click "Export".

**Expected Output**
- A CSV file containing the scan results will be downloaded to your local machine.

### Exercise 3: Setting Up Splunk for Dark Web Alert Integration
1. Log in to Splunk: Navigate to `http://localhost:8000` and log in with your credentials.
2. Add Data Input: Go to "Settings" > "Add Data" > "Upload" and upload the CSV file exported from SpiderFoot.
3. Configure Data Input: Follow the prompts to configure the data input and index it appropriately.

**Expected Output**
- The SpiderFoot scan results will be indexed in Splunk, ready for search and analysis.

### Exercise 4: Creating Alerts in Splunk
1. Search Indexed Data: Use the Splunk search bar to query the indexed SpiderFoot data. For example:
```spl
index=spiderfoot sourcetype=csv
```
2. Create an Alert:
3. Click on "Save As" > "Alert".
4. Configure the alert conditions, such as triggering when specific keywords or patterns are found in the data.
5. Set alert actions, such as sending an email notification.

**Expected Output**
- An alert will be created in Splunk that triggers based on the specified conditions and sends notifications accordingly.

### Exercise 5: Automating SpiderFoot Data Ingestion into Splunk
1. Schedule SpiderFoot Scans: Use SpiderFoot's scheduling feature to run scans at regular intervals.
2. Automate Data Export and Import:
3. Write a script to export SpiderFoot scan results periodically.
4. Use Splunk's REST API to automate the ingestion of these results into Splunk. Example Script: 
```bash
#!/bin/bash
# Schedule this script with crontab

# Run SpiderFoot scan
cd /path/to/spiderfoot
python3 sf.py --scan-target example.com --scan-config darkweb_config > results.csv

# Upload to Splunk
curl -k -u username:password https://localhost:8089/services/receivers/simple?index=spiderfoot -d @results.csv
```

**Expected Output**
- SpiderFoot scan results will be automatically ingested into Splunk at regular intervals, keeping your SIEM system updated with the latest dark web monitoring data.

## Conclusion
By completing these exercises, you have learned how to integrate dark web monitoring alerts with a SIEM system using SpiderFoot and Splunk. You configured SpiderFoot for dark web monitoring, exported scan results, set up data inputs and alerts in Splunk, and automated the data ingestion process. This hands-on experience equips you with practical skills in dark web intelligence gathering and integration with SIEM systems.
