---
title: Naslag informatie over het Azure Sentinel data source-schema
description: Dit artikel bevat informatie over Azure en gegevens bron schema's van derden die worden ondersteund door Azure Sentinel, met koppelingen naar de bijbehorende referentie documentatie.
author: batamig
ms.author: bagol
manager: rkarlin
ms.assetid: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: reference
ms.custom: ''
ms.date: 01/14/2021
ms.openlocfilehash: 4601f2d6eddbbe8809dfd46a7e0cc5aa3c40c722
ms.sourcegitcommit: c3739cb161a6f39a9c3d1666ba5ee946e62a7ac3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2021
ms.locfileid: "107209567"
---
# <a name="data-source-schema-reference"></a>Verwijzing naar gegevens bron schema

Dit artikel bevat een lijst met ondersteunde Azure-en gegevens bron schema's van derden, met koppelingen naar de bijbehorende referentie documentatie.

## <a name="azure-data-sources"></a>Azure-gegevensbronnen

| Type                             | Gegevensbron             | Tabel naam Log Analytics | Schema verwijzing |
| -------------------------------- | ---------------------- | ---------------------- | ---------------- |
| **Azure**                            | Azure Active Directory | SigninEvents           | [Aanmeld eigenschappen van Azure AD-activiteiten rapporten](/graph/api/resources/signin#properties) |
| **Azure**                            | Azure Active Directory | Audit logs bevat              | [Naslag informatie over Azure Monitor audit logs bevat](/azure/azure-monitor/reference/tables/auditlogs) |
| **Azure**                            | Azure Active Directory | AzureActivity          | [Naslag informatie over Azure Monitor AzureActivity](/azure/azure-monitor/reference/tables/azureactivity) |
| **Azure**                            | Office                 | OfficeActivity         | Office 365 Management Activity API-schema's: <br>- [Algemeen schema ](/office/office-365-management-api/office-365-management-activity-api-schema#common-schema)   <br>- [Exchange-beheer schema ](/office/office-365-management-api/office-365-management-activity-api-schema#exchange-admin-schema) <br>- [Exchange-postvak schema](/office/office-365-management-api/office-365-management-activity-api-schema#exchange-mailbox-schema)  <br>- [Share point-basis schema](/office/office-365-management-api/office-365-management-activity-api-schema#sharepoint-base-schema)   <br>- [Share point-Bestands bewerkingen](/office/office-365-management-api/office-365-management-activity-api-schema#sharepoint-file-operations) |
| **Azure**                            | Azure Key Vault         | AzureDiagnostics       | [Naslag informatie over Azure Monitor AzureDiagnostics](/azure/azure-monitor/reference/tables/azurediagnostics) |
| **Host**                             | Linux                  | Syslog                 | [Azure Monitor syslog-referentie](/azure/azure-monitor/reference/tables/syslog) |
| **Netwerk**                          | IIS-logboeken               | W3CIISLog              | [Naslag informatie over Azure Monitor W3CIISLog](/azure/azure-monitor/reference/tables/w3ciislog) |
| **Netwerk**                          | VMinsights             | VMConnection           | [Naslag informatie over Azure Monitor VMConnection](/azure/azure-monitor/reference/tables/vmconnection) |
| **Netwerk**                          | Wire data-oplossing     | WireData               | [Naslag informatie over Azure Monitor WireData](/azure/azure-monitor/reference/tables/wiredata) |
| **Netwerk**                          | NSG-stroom logboeken          | AzureNetworkAnalytics  | [Schema's en gegevens aggregatie in Traffic Analytics](../network-watcher/traffic-analytics-schema.md) |
| | | | |

> [!NOTE]
> Zie voor meer informatie de volledige [Azure monitor gegevens referentie](/azure/azure-monitor/reference/).
>
## <a name="3rd-party-vendor-data-sources"></a>gegevens bronnen van derden

De volgende tabel bevat een lijst met ondersteunde externe leveranciers en hun syslog-of common Event Format (CEF)-toewijzings documentatie voor verschillende ondersteunde logboek typen, die CEF veld Toewijzingen en voorbeeld logboeken voor elk categorie type bevatten.

| Type |    Leverancier |    Product | Tabel naam Log Analytics | CEF veld-toewijzings verwijzing  |
| ----- | ----- | ----- | ----- |----- |
| **Netwerk** | Palo Alto   | BESTURINGS SYSTEEM PANNEN    | CommonSecurityLog |   [Pan-OS 9,0 algemene hand leiding](https://docs.paloaltonetworks.com/content/dam/techdocs/en_US/pdf/cef/pan-os-90-cef-configuration-guide.pdf) voor de integratie van de gebeurtenis indeling (zoek naar *CEF-stijl logboek indelingen*) |
| **Netwerk** | Check Point  |ALL   | CommonSecurityLog | [Beschrijving van logboek velden](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk109795)       |
| **Netwerk** | FortiGate   | ALL   | CommonSecurityLog | [Schema structuur van logboek](https://docs.fortinet.com/document/fortigate/6.2.3/fortios-log-message-reference/738142/log-schema-structure)         |
| **Netwerk** | Barracuda | Web Application Firewall |  CommonSecurityLog   | [Syslog en andere logboeken configureren](https://campus.barracuda.com/product/webapplicationfirewall/doc/4259935/how-to-configure-syslog-and-other-logs/)  |
| **Netwerk** | Cisco | ASA | CommonSecurityLog | [Berichten van de Cisco ASA-serie syslog](https://www.cisco.com/c/en/us/td/docs/security/asa/syslog/b_syslog/about.html)    |
| **Netwerk** | Cisco | Firepower   | CommonSecurityLog | [Cisco Firepower Threat verdediging syslog-berichten](https://www.cisco.com/c/en/us/td/docs/security/firepower/Syslogs/b_fptd_syslog_guide.html)    |
| **Netwerk** | Cisco   | Paraplu  | Tabel met aangepaste logboeken  | [Logboek indelingen en versie beheer](https://docs.umbrella.com/deployment-umbrella/docs/log-formats-and-versioning)   |
| **Netwerk**   | Cisco | Meraki    | CommonSecurityLog |   [Gebeurtenis typen en logboek voorbeelden van syslog](https://documentation.meraki.com/zGeneral_Administration/Monitoring_and_Reporting/Syslog_Event_Types_and_Log_Samples)    |
| **Netwerk**   | Zscaler | Nano streaming-service (NSS)|   CommonSecurityLog | [Nss-feeds Format teren](https://help.zscaler.com/zia/documentation-knowledgebase/analytics/nss/nss-feeds/formatting-nss-feeds) (alleen web-, firewall-, DNS-en tunnel Logboeken) |
| **Netwerk**   |F5 | BigIP LTM|    CommonSecurityLog|  [Gebeurtenis berichten en typen aanvallen](https://techdocs.f5.com/kb/en-us/products/big-ip_ltm/manuals/product/bigip-external-monitoring-implementations-13-0-0/15.html)  |
| **Netwerk** | F5  | BigIP ASM|    CommonSecurityLog|  [Logboek registratie van toepassings beveiligings gebeurtenissen](https://techdocs.f5.com/kb/en-us/products/big-ip_asm/manuals/product/asm-implementations-13-1-0/14.html)                                                           |
| **Netwerk** | Citrix  |Web App-Firewall   | CommonSecurityLog|    [Ondersteuning voor logboek registratie van algemene gebeurtenis indelingen (CEF) in de toepassings firewall](https://support.citrix.com/article/CTX136146) <br>  [Verwijzing naar syslog-bericht voor NetScaler 12,0](https://developer-docs.citrix.com/projects/netscaler-syslog-message-reference/en/12.0/)   |
|**Host** |Symantec | Symantec Endpoint Protection-beheerder (SEPM) | CommonSecurityLog|[Instellingen voor externe logboek registratie en ernst van logboek gebeurtenissen voor Endpoint Protection-beheerder](https://support.symantec.com/us/en/article.tech171741.html)|
|**Host** |Trend Micro |Alles |CommonSecurityLog | [Syslog-inhouds toewijzing-CEF](https://docs.trendmicro.com/en-us/enterprise/control-manager-70/appendices/syslog-mapping-cef.aspx) |
| | | | | |

> [!NOTE]
> Zie ook [veld toewijzing CEF en CommonSecurityLog](cef-name-mapping.md)voor meer informatie.
> 
## <a name="next-steps"></a>Volgende stappen

Meer informatie over de ondersteunde Azure Sentinel-connectors, zoals CEF, syslog, direct, agent en aangepaste connectors:

- [Verbinding maken met gegevensbronnen](connect-data-sources.md)

- [Azure Sentinel syslog, CEF en andere connectors van derden](https://techcommunity.microsoft.com/t5/azure-sentinel/azure-sentinel-syslog-cef-and-other-3rd-party-connectors-grand/ba-p/803891)
