# Building a SOC + Honeynet in Azure (Live Traffic)
![Screenshot 2024-01-18 191557](https://github.com/AnthonyRams/Azure-Honeyney-SOC/assets/156973794/cb7b04f9-d57a-4c25-a76e-6e16ceb5d3e7)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![OpenCloud](https://github.com/AnthonyRams/Azure-Honeyney-SOC/assets/156973794/1127c353-4a79-46eb-b282-6eea8a695fb6)

## Architecture After Hardening / Security Controls
![Closedcloud](https://github.com/AnthonyRams/Azure-Honeyney-SOC/assets/156973794/63f5bf82-38bf-451a-8092-5f394da6401c)

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
![nsg-malicious-allowed-in-before](https://github.com/AnthonyRams/Azure-Honeyney-SOC/assets/156973794/97d4b59b-36ef-419e-bde6-c2fd11a76846)
![linux-ssh-auth-fail-before](https://github.com/AnthonyRams/Azure-Honeyney-SOC/assets/156973794/425b967e-9652-439d-b13b-49d9c456e5fc)
![windows-rdp-auth-fail-before](https://github.com/AnthonyRams/Azure-Honeyney-SOC/assets/156973794/8c626e38-9627-448a-9949-2fda428ef04a)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 69309
| Syslog                   | 7253
| SecurityAlert            | 12
| SecurityIncident         | 107
| AzureNetworkAnalytics_CL | 4670

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
