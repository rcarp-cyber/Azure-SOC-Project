# Azure-SOC-Project# Building a SOC + Honeynet in Azure (Live Traffic)
Cloud Honeynet / SOC![image](https://github.com/user-attachments/assets/a7997374-4ca3-40e9-bbf9-af24482ae76a)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
Architecture Diagram![image](https://github.com/user-attachments/assets/0fd6ddcc-5bbd-4f29-b645-7aa141386f18)


## Architecture After Hardening / Security Controls
Architecture Diagram![image](https://github.com/user-attachments/assets/90ee891c-aedc-4dd9-a235-9686109ccfff)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
NSG Allowed Inbound Malicious Flows![image](https://github.com/user-attachments/assets/62c1fc85-61df-494d-92e3-ff335e74a481)<br>
Linux Syslog Auth Failures![image](https://github.com/user-attachments/assets/c46d9e21-da4f-4f7c-8d6e-94d6937b1b29)<br>
Windows RDP/SMB Auth Failures![image](https://github.com/user-attachments/assets/02cb4fba-85a6-4dd7-ad95-294920cb99f4)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2025-02-27 03:12
Stop Time 2025-02-28 03:12

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 110083
| Syslog                   | 16013
| SecurityAlert            | 0
| SecurityIncident         | 301
| AzureNetworkAnalytics_CL | 2060

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2025-03-13 21:03
Stop Time	2025-03-14 21:03

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 633
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
