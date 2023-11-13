# Creating a SOC - Honeynet in Azure (Live Traffic)
![SOC - Honeynet in Azure](https://github.com/ctstephens/Azure-SOC-Honeynet/assets/150542854/84085c94-34cc-4cb1-9563-c53bfd60932a)

## Introduction

In this project, I built a Honeynet in Azure and ingest log sources from various resources into a Log Analytics Workspace, which is then used by Azure Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, applied some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics I will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our Honeynet)

## Architecture Before Hardening / Security Controls
![Open Public Network](https://github.com/ctstephens/Azure-SOC-Honeynet/assets/150542854/f355ddf5-4afc-4a2e-859c-8536d2cb056a)

For the "BEFORE" metrics, all resources were originally deployed exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with Public Endpoints visible to the Internet; aka, no use for Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![windows-rdp-auth-fail BEFORE](https://github.com/ctstephens/Azure-SOC-Honeynet/assets/150542854/28a3ebbb-0f01-4e1d-abd9-133f13612bc7)
![linux-ssh-auth-fail BEFORE](https://github.com/ctstephens/Azure-SOC-Honeynet/assets/150542854/077b7ae7-1c39-4254-b5a5-e203c5c07fed)
![mssql-auth-fail BEFORE](https://github.com/ctstephens/Azure-SOC-Honeynet/assets/150542854/c4c12377-3109-49cb-8ad2-baffa207e749)
![nsg-malicious-allowed-in BEFORE](https://github.com/ctstephens/Azure-SOC-Honeynet/assets/150542854/e4344cab-a7a0-4fc6-a8a4-cc9bb6edcd40)

## Metrics Before Hardening / Security Controls

The following table shows the metrics I measured in my insecure environment for 24 hours:
Start Time 2023-10-31 T12:23:23.
Stop Time 2023-11-01 T12:23:23.

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 15849
| Syslog                   | 3829
| SecurityAlert            | 2
| SecurityIncident         | 129
| AzureNetworkAnalytics_CL | 1018


## Architecture After Hardening / Security Controls
![Secure Network](https://github.com/ctstephens/Azure-SOC-Honeynet/assets/150542854/8b9d4027-15ad-417e-b1bc-d18c97ea7f38)

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my Admin Workstation, and all other resources were protected by their built-in Firewalls as well as Private Endpoints.

The architecture of the Honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 Windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Attack Maps Before Hardening / Security Controls
![windows-rdp-auth-fail AFTER](https://github.com/ctstephens/Azure-SOC-Honeynet/assets/150542854/0f37a026-067f-4977-bf97-7cd98ae324b3)
![linux-ssh-auth-fail AFTER](https://github.com/ctstephens/Azure-SOC-Honeynet/assets/150542854/e5276688-eb47-4e36-93b9-f94b1f11e282)
![mssql-auth-fail AFTER](https://github.com/ctstephens/Azure-SOC-Honeynet/assets/150542854/2e5679a5-ee3b-4d79-841d-2fcdd8f8c9fa)
![nsg-malicious-allowed-in AFTER](https://github.com/ctstephens/Azure-SOC-Honeynet/assets/150542854/f319056d-de21-4224-b11e-3d04b8af776b)

## Metrics After Hardening / Security Controls

The following table shows the metrics I measured in my environment for another 24 hours, but after we have applied security controls:
Start Time 2023-11-08 T20:25:33.
Stop Time	2023-11-09 T20:25:33.

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 0
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this Project, a Honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics Workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
