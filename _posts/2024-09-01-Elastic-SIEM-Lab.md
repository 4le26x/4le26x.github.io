---
layout: single
title: <span style="color:#F04E98">Elastic</span><span style="color:#FFD35D"> SIEM </span><span style="color:#00BFB3">Lab</span><span style="color:#B3DA1C"> Setup</span>
excerpt: "This post outlines a project to deepen understanding of Elastic SIEM by setting up a basic Security Information and Event Management (SIEM) environment. The lab includes setting up Elastic SIEM, installing an agent on a Kali VM, generating security events, and creating visualizations and alerts."
date: 2024-08-24
classes: wide
header:
  teaser: /assets/images/ElasticSIEM/ElasticSIEM.png
  teaser_home_page: true
  icon: /assets/images/ElasticSIEM/ElasticSIEM.png
categories:
  - Elastic SIEM
  - Medium
  - EN
tags:
  - SIEM Setup
  - Elastic Agent
  - Security Monitoring
  - Log Analysis
---

## DESCRIPTION

The following post outlines a project I undertook to deepen my understanding of how to use Elastic SIEM. This project guides you through the process of setting up a basic Security Information and Event Management (SIEM) environment using Elastic SIEM, part of the Elastic Stack. The primary goal is to provide hands-on experience in security monitoring, event detection, and incident response by collecting, analyzing, and visualizing security events. The lab leverages a Kali Linux virtual machine to generate various security events, which are then captured and analyzed within Elastic SIEM.

This lab is ideal for those looking to practice and enhance their skills in using SIEM tools, understand the mechanics of security event analysis, and apply these concepts in a real-world-like environment. It serves as a straightforward introduction to the function of SIEMs and their crucial role in modern cybersecurity operations.

<!-- ![Elastic SIEM Description](/assets/images/ElasticSIEM/ElasticSIEM_Description.JPG)-->

## TABLE OF CONTENTS
- [Installation](#installation)
- [Generating Security Events](#generating-events)
- [Dashboard Creation](#dashboard-creation)
- [Alert Setup](#alert-setup)
- [References](#references)

## INSTALLATION

### Step 1: Set up a Free Trial Elastic Account
Start by creating a free Elastic account. Visit the Elastic registration page, sign up, and verify your email. Once verified, log in to the Elastic Cloud console. Begin your free trial and create an Elasticsearch deployment by choosing a cloud provider, region, and deployment size—default settings are usually fine for this lab. After setting up the deployment, install Elastic's prebuilt rules from the Security section under the Detections tab to start monitoring for security events.

![Elastic SIEM Description](/assets/images/ElasticSIEM/step_1_1.png)

![Elastic SIEM Description](/assets/images/ElasticSIEM/step_1_2.png)
### Step 2: Setting up the Agent to Collect Logs

> An agent is a software program installed on a device, such as a server or endpoint, to collect and send data to a centralized system for analysis and monitoring. In the context of Elastic SIEM, the agent is crucial for collecting and forwarding security-related events from your endpoints to your Elastic SIEM instance.

- [More Information about the role of agents in Elastic SIEM](https://alejandros-organization-8.gitbook.io/active/v/untitled/installation#:~:text=More%20Information%20about%20the%20role%20of%20agents%20in%20Elastic%20SIEM)

To set up the agent on your Kali Linux virtual machine, first, log in to your Elastic SIEM instance. Once logged in, navigate to the Integrations page by clicking on the Kibana main menu bar at the top left and selecting “Integrations” from the dropdown menu. In the search bar on the Integrations page, type “Elastic Defend” and select it from the results. This integration is specifically designed to collect and manage security data.

![Elastic SIEM Description](/assets/images/ElasticSIEM/step_2_1.png)

Click on “Install Elastic Defend” and follow the instructions provided to install the agent on your Kali VM. The installation process involves downloading the Elastic Agent package and enrolling it with your Elastic deployment. During enrollment, you’ll need to generate an enrollment token from the Elastic Cloud console, which will link the agent to your specific deployment. After completing these steps, the agent will be installed and configured to start collecting logs and forwarding them to your SIEM instance.

- To ensure the agent is functioning correctly, open a terminal on your Kali VM and run the command:

```bash
sudo systemctl status elastic-agent.service
```

![Elastic SIEM Description](/assets/images/ElasticSIEM/step_2_2.png)
> Note: You should see a status message indicating that the service is active and running. This confirms that the agent is properly installed and operational, ready to begin sending data to Elastic SIEM.

![Elastic SIEM Description](/assets/images/ElasticSIEM/step_2_3.png)
## Generating Security Events

With your Elastic environment set up and the agent installed, it’s time to generate security events. Use Nmap, a powerful network scanning tool, to create logs that Elastic SIEM will capture. On Kali Linux, Nmap is pre-installed; on other distributions, install it via your package manager (e.g., `sudo apt-get install nmap` for Ubuntu/Debian).

To generate events, run a full port scan on your Kali VM’s IP with `nmap -A -p- 192.168.0.27`. This will produce network activity that the Elastic Agent will forward to your SIEM for analysis. Remember, Nmap can be disruptive, so review its legal and ethical implications before use.

After the scan, check the Logs tab in the Observability section of the Elastic Cloud console. Here, you’ll find the logs generated by your Nmap scan, allowing you to see how Elastic SIEM captures and analyzes security events in real time.

![Elastic SIEM Description](/assets/images/ElasticSIEM/event_1.png)

![Elastic SIEM Description](/assets/images/ElasticSIEM/event_2.png)
## Dashboard Creation

To better understand and analyze the security events captured by your SIEM, you can create a dashboard in Elastic that visualizes the data. Start by logging into your Elastic Cloud account and navigating to the Dashboards section under the Analytics menu. Here, click the “Create dashboard” button to start a new dashboard.

![Elastic SIEM Description](/assets/images/ElasticSIEM/dashboard_1.png)

![Elastic SIEM Description](/assets/images/ElasticSIEM/dashboard_2.png)

You’ll then add visualizations to your dashboard. Click “Create Visualization” and select a type of chart that best represents your data, such as an area or line chart. In the visualization editor, set “Count” as the metric for the vertical axis to show the number of events, and use “Timestamp” for the horizontal axis to display when these events occurred. This setup will create a time-based visualization that tracks security events as they happen.

<p align="center">
  <img src="/assets/images/ElasticSIEM/dashboard_4.png" alt="Elastic SIEM Description" style="width:45%; margin-right:10px;" />
  <img src="/assets/images/ElasticSIEM/dashboard_5.png" alt="Elastic SIEM Description" style="width:45%;" />
</p>

Once you’ve configured your visualizations, save your dashboard. This dashboard will now provide a real-time view of security events, making it easier to spot patterns or anomalies in your logs. You can customize the dashboard further by adding more visualizations or rearranging the layout to suit your needs.

![Elastic SIEM Description](/assets/images/ElasticSIEM/dashboard_6.png)
## Alert Setup

The final step in this project is to set up an alert that will notify you if an Nmap scan is detected in your environment. In the Elastic SIEM, navigate to the Detections tab and click on “Create new rule.” You’ll create a custom query rule specifically designed to detect Nmap scan events.

![Elastic SIEM Description](/assets/images/ElasticSIEM/alert_1.png)

In the rule creation interface, define your rule by setting the query to something like `event.action: "nmap_scan"`. This query will trigger whenever the SIEM detects an event that matches the action “nmap\_scan.” Name your rule appropriately (e.g., “Nmap Scan Detection”) and assign it a severity level. You can keep the default settings for how often the rule checks for new events or adjust it based on your needs

![Elastic SIEM Description](/assets/images/ElasticSIEM/alert_2.png)

Next, decide what action should be taken when this rule is triggered. Options include sending an email, posting a message to a Slack channel, or triggering a webhook that integrates with other security tools. Once you’ve configured the action, finalize the alert by clicking “Create and enable rule.”

![Elastic SIEM Description](/assets/images/ElasticSIEM/aler_3.png)

Your alert is now active and will continuously monitor for Nmap scan events. If such an event is detected, the alert will trigger, and the action you specified will be executed. You can manage and review these alerts in the Alerts section under Security in Elastic, ensuring you stay on top of potential security incidents.

![Elastic SIEM Description](/assets/images/ElasticSIEM/alert_4.png)

## References

1. [A Simple Elastic SIEM Lab by Aali23](https://medium.com/@aali23/a-simple-elastic-siem-lab-6765159ee2b2)
2. [Setting Up a Home Lab for Elastic SIEM: A Step-by-Step Guide by Christoff Elce](https://medium.com/@christoff.elce/setting-up-a-home-lab-for-elastic-siem-a-step-by-step-guide-e85f3750eb25)

