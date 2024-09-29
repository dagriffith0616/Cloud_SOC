# Cloud_SOC# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

I leveraged Azure cloud services to deploy multiple Virtual Machines and Virtual Networks in this project. I then configured agents on the VMs and network groups to forward logs into Log Analytics Workspace. I then configure Microsoft Sentinel SIEM to build attack maps, trigger alerts, and automatically create incidents from the alerts. I configured the environment to be accessible from the internet and observed the attacks over a 24-hour period. I investigated the various alerts using Microsoft Sentinel and applied network security controls. After hardening the networks, I monitored the environment for 24 hours to compare the metrics. The captured metrics were: 

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel (SIEM)

## Attack Maps Before Hardening / Security Controls

-- Network Security Groups- Malicious flows allowed in:
![image](https://github.com/user-attachments/assets/54c266f9-09a8-4eb2-85ba-513d84ab7a8d)
-- Linux Syslog Authorization Failures
![image](https://github.com/user-attachments/assets/3d3ee4f0-be97-4477-9e3d-260d30366f24)
-- Windows RDP/SMB Authorization Failures
![image](https://github.com/user-attachments/assets/431cce26-2089-4b57-b1dd-3ab41e0ec3b4)

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment over a 24-hour period:

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 57882
| Syslog                   | 9390
| SecurityAlert            | 39
| SecurityIncident         | 247
| AzureNetworkAnalytics_CL | 3506


## Metrics After Hardening / Security Controls

The following table shows the metrics observed over a 24-hour period after responding to incidents and applying NIST 800-53 controls.

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8928
| Syslog                   | 4
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Reduction In Incidents

![image](https://github.com/user-attachments/assets/afc97fa9-d97b-4f38-8cbb-d8bc20ec93b9)


## Conclusion

In this project, the honeynet experienced nearly constant brute-force attempts to log in to the Windows and Linux VM. This resulted in 74 medium to high incidents being triggered for investigation in Microsoft Sentinel. After restricting network access using firewall rules, all recorded metrics improved drastically. Of particular note, attempts to brute-force login to the Windows and Linux VMs were reduced by 84.58% and 99.96%, respectively.  

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
