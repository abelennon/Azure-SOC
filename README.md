# Establishing a Security Operations Center (SOC) featuring a Honeypot on Microsoft Azure

## Introduction

This project involved me constructing a honeynet on Microsoft Azure and ingesting logs from different resources into a Log Analytics workspace. Subsequently, I used Microsoft Sentinel to generate attack maps, trigger alerts, and create incidents. I recorded certain security metrics in an unsecured environment for 24 hours, fortified the environment by implementing security controls, monitored metrics for an additional 24 hours, and presented the findings below. I utilized the metrics to create a geographical representation of the attackers' locations and provide a summary of the overall enhancement achieved by implementing security controls. Moreover, I performed a sequence of simulated attacks on the system.

## Architecture of the Lab

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

![Cloud Honeynet / SOC](https://i.imgur.com/qtY8Sey.png)

## Technologies Employed & Regulations Followed

- KQL Queries
- Microsoft Defender for Cloud
- Windows Remote Desktop
- Command Line Interface
- PowerShell
- NIST SP 800-53
- NIST SP 800-61

## Metrics Gathered

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/0fI2lto.png)

Concerning the "BEFORE" metrics, all resources were deployed initially in a way that made them accessible via the internet. This encompassed the Virtual Machines, which had their Network Security Groups and built-in firewalls configured to permit all traffic, and all other resources were deployed with public endpoints that were visible to the internet.

## Attack Maps Before Hardening / Security Controls
![MSSQL Auth Failures](https://i.imgur.com/SdtbwDI.png)<br>
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/ZsLVqF4.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/E9u8cbN.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/CASF2Rc.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics I measured in my insecure environment for 24 hours:<br>
Start Time 2023-04-29 22:27:31<br>
Stop Time 2023-04-30 22:27:31<br>

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 49065
| Syslog                   | 3740
| SecurityAlert            | 7
| SecurityIncident         | 151
| AzureNetworkAnalytics_CL | 1282

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/yo0TDsK.png)

Regarding the "AFTER" metrics, the Network Security Groups were fortified by blocking ALL traffic except for my admin workstation. Additionally, all other resources were safeguarded by both their built-in firewalls and Private Endpoint.

## Attack Maps After Hardening / Security Controls

```During the 24-hour period after fortification, none of the map queries yielded any outcomes because there were no instances of malicious activity.```

## Metrics After Hardening / Security Controls

The following table shows the metrics I measured in my environment for another 24 hours, but after I had applied security controls:<br>
Start Time 2023-05-01 23:50:58<br>
Stop Time	2023-05-02 23:50:58<br>

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 11640
| Syslog                   | 23
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Overall Improvement

| Metric                   | Change After Securing Environment
| ------------------------ | ---------------------------------
| SecurityEvent            | -76.28%
| Syslog                   | -99.39%
| SecurityAlert            | -100.00%
| SecurityIncident         | -100.00%
| AzureNetworkAnalytics_CL | -100.00%

## Conclusion

This project involved building a small honeynet on Microsoft Azure and integrating log sources into a Log Analytics workspace. Microsoft Sentinel was utilized to generate alerts and incidents based on the logs that were ingested. Moreover, metrics were recorded in an environment that lacked security controls, and then once again after security measures were implemented. Notably, the number of security events and incidents significantly decreased after the security controls were put in place, indicating their efficacy.

It's important to mention that in case the resources within the network were extensively used by regular users, there's a possibility that more security events and alerts would have been triggered during the 24-hour period after implementing the security controls.
