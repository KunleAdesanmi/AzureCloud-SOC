# Building a SOC + Honeynet in Azure (Live Traffic)
<img width="900" alt="Cloud_Honeynet_SOC" src="https://github.com/KunleAdesanmi/AzureCloud-SOC/assets/96393713/0a224482-1084-4448-bfad-04b84ea56984">

## Introduction

In this project, I built a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
<img width="900" alt="Architecture_Before_Hardening" src="https://github.com/KunleAdesanmi/AzureCloud-SOC/assets/96393713/07a3dbfe-383e-433a-b092-f7e500e60f9e">

## Architecture After Hardening / Security Controls
<img width="900" alt="Architecture_After_Hardening" src="https://github.com/KunleAdesanmi/AzureCloud-SOC/assets/96393713/e8ec14e4-723f-4c63-8683-f96ff02a37bf">

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

<img width="1000" alt="nsg-malicious-allowed-in_before" src="https://github.com/KunleAdesanmi/AzureCloud-SOC/assets/96393713/07fa8b72-12a9-485e-aec6-879d551a6693">


<img width="1000" alt="linux-ssh-auth-fail_before" src="https://github.com/KunleAdesanmi/AzureCloud-SOC/assets/96393713/cea03c47-c536-4c58-8abf-d2a5ace6f57c">


<img width="1000" alt="windows-rdp-auth-fail_before" src="https://github.com/KunleAdesanmi/AzureCloud-SOC/assets/96393713/7e17f89e-0fb7-49c5-90a5-1c14116a8ebf">


<img width="1000" alt="mssql-auth-fail_before" src="https://github.com/KunleAdesanmi/AzureCloud-SOC/assets/96393713/57f1156b-6799-442d-84b6-b655151510d9">


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-11-05T10:50:04.4144907Z
Stop Time 2023-11-06T10:50:04.4144907Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 43684
| Syslog                   | 2442
| SecurityAlert            | 5
| SecurityIncident         | 565
| AzureNetworkAnalytics_CL | 3563

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-11-07T13:30:37.4876815Z
Stop Time	2023-11-08T13:30:37.4876815Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8937
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
