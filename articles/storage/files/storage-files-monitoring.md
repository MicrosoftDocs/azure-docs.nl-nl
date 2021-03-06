---
title: Bewakings- Azure Files | Microsoft Docs
description: Meer informatie over het bewaken van de prestaties en beschikbaarheid van Azure Files. Be Azure Files gegevens, meer informatie over configuratie en het analyseren van metrische gegevens en logboekgegevens.
author: normesta
services: storage
ms.service: storage
ms.subservice: files
ms.topic: conceptual
ms.date: 3/02/2021
ms.author: normesta
ms.reviewer: fryu
ms.custom: monitoring, devx-track-csharp, devx-track-azurecli
ms.openlocfilehash: 620ee3bc5978da4b274aed9a412679ae0835f0b9
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/20/2021
ms.locfileid: "107759823"
---
# <a name="monitoring-azure-files"></a>Bewakings- Azure Files

Wanneer u kritieke toepassingen en bedrijfsprocessen hebt die afhankelijk zijn van Azure-resources, wilt u deze resources controleren op beschikbaarheid, prestaties en werking. In dit artikel worden de bewakingsgegevens beschreven die door Azure Files worden gegenereerd en hoe u de functies van Azure Monitor kunt gebruiken om waarschuwingen over deze gegevens te analyseren.

## <a name="monitor-overview"></a>Overzicht van bewaken

De **pagina** Overzicht in de Azure Portal voor elke Azure Files resource bevat een korte weergave van het resourcegebruik, zoals aanvragen en facturering per uur. Deze informatie is nuttig, maar er is slechts een kleine hoeveelheid bewakingsgegevens beschikbaar. Sommige van deze gegevens worden automatisch verzameld en zijn beschikbaar voor analyse zodra u de resource maakt. U kunt aanvullende typen gegevensverzameling inschakelen met enige configuratie.

## <a name="what-is-azure-monitor"></a>Wat is Azure Monitor?
Azure Files maakt bewakingsgegevens met behulp [van Azure Monitor](../../azure-monitor/overview.md), een volledige stack-bewakingsservice in Azure. Azure Monitor biedt een volledige set functies voor het bewaken van uw Azure-resources en -resources in andere clouds en on-premises. 

Begin met het artikel [Azure-resources bewaken met Azure Monitor](../../azure-monitor/essentials/monitor-azure-resource.md), waarin het volgende wordt beschreven:

- Wat is Azure Monitor?
- Kosten die zijn gekoppeld aan bewaking
- Bewakingsgegevens die zijn verzameld in Azure
- Gegevensverzameling configureren
- Standaardhulpprogramma's in Azure voor het analyseren en waarschuwen van bewakingsgegevens

De volgende secties zijn gebaseerd op dit artikel door de specifieke gegevens te beschrijven die zijn verzameld van Azure Files. Voorbeelden laten zien hoe u gegevensverzameling configureert en deze gegevens analyseert met Azure-hulpprogramma's.

## <a name="monitoring-data"></a>Bewakingsgegevens

Azure Files verzamelt dezelfde soorten bewakingsgegevens als andere Azure-resources, die worden beschreven in [Gegevens van Azure-resources bewaken.](../../azure-monitor/essentials/monitor-azure-resource.md#monitoring-data) 

Zie [Naslaginformatie over bewakingsgegevens van Azure File](storage-files-monitoring-reference.md) voor gedetailleerde informatie over de metrische gegevens en logboekgegevens die zijn gemaakt door Azure Files.

Metrische gegevens en logboeken in Azure Monitor bieden alleen ondersteuning Azure Resource Manager opslagaccounts. Azure Monitor biedt geen ondersteuning voor klassieke opslagaccounts. Als u metrische gegevens of logboeken wilt gebruiken in een klassiek opslagaccount, moet u migreren naar een Azure Resource Manager opslagaccount. Zie [Migreren naar Azure Resource Manager](../../virtual-machines/migration-classic-resource-manager-overview.md).

## <a name="collection-and-routing"></a>Verzameling en routering

Metrische gegevens van het platform en het activiteitenlogboek worden automatisch verzameld, maar kunnen worden doorgeleid naar andere locaties met behulp van een diagnostische instelling. 

Als u resourcelogboeken wilt verzamelen, moet u een diagnostische instelling maken. Wanneer u de instelling maakt, kiest u **bestand** als het type opslag waarop u logboeken wilt inschakelen. Geef vervolgens een van de volgende categorieën bewerkingen op waarvoor u logboeken wilt verzamelen. 

| Categorie | Beschrijving |
|:---|:---|
| StorageRead | Leesbewerkingen op objecten. |
| StorageWrite | Schrijfbewerkingen op objecten. |
| StorageDelete | Verwijder bewerkingen op objecten. |

Zie Voor de lijst met SMB- en REST-bewerkingen die zijn geregistreerd, zie [Storage logged operations](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) and status messages (Opslag vastgelegde bewerkingen en statusberichten) en Azure Files [bewakingsgegevensreferentie](storage-files-monitoring-reference.md).

## <a name="creating-a-diagnostic-setting"></a>Een diagnostische instelling maken

U kunt een diagnostische instelling maken met behulp van de Azure Portal, PowerShell, de Azure CLI of een Azure Resource Manager sjabloon.

> [!NOTE]
> Azure Storage logboeken in Azure Monitor is in openbare preview en is beschikbaar voor preview-tests in alle regio's van de openbare cloud. Deze preview maakt logboeken mogelijk voor blobs (waaronder Azure Data Lake Storage Gen2), bestanden, wachtrijen en tabellen. Deze functie is beschikbaar voor alle opslagaccounts die zijn gemaakt met het Azure Resource Manager implementatiemodel. Zie [Overzicht van opslagaccounts.](../common/storage-account-overview.md)

Zie Diagnostische instelling maken voor het verzamelen van platformlogboeken en metrische gegevens in Azure voor algemene [richtlijnen.](../../azure-monitor/essentials/diagnostic-settings.md)

### <a name="azure-portal"></a>[Azure-portal](#tab/azure-portal)

1. Meld u aan bij Azure Portal.

2. Ga naar uw opslagaccount.

3. Klik in **de sectie** Bewaking op **Diagnostische instellingen (preview)**.

   > [!div class="mx-imgBorder"]
   > ![portal - Diagnostische logboeken](media/storage-files-monitoring/diagnostic-logs-settings-pane.png)   

4. Kies **bestand** als het type opslag waar u logboeken voor wilt inschakelen.

5. Klik op **Diagnostische instelling toevoegen**.

   > [!div class="mx-imgBorder"]
   > ![portal - Resourcelogboeken - diagnostische instelling toevoegen](media/storage-files-monitoring/diagnostic-logs-settings-pane-2.png)

   De **pagina Diagnostische** instellingen wordt weergegeven.

   > [!div class="mx-imgBorder"]
   > ![Pagina resourcelogboeken](media/storage-files-monitoring/diagnostic-logs-page.png)

6. Voer in **het** veld Naam van de pagina een naam in voor deze instelling voor resourcelogboek. Selecteer vervolgens welke bewerkingen u wilt geregistreerd (lees-, schrijf- en verwijderbewerkingen) en waar u wilt dat de logboeken worden verzonden.

#### <a name="archive-logs-to-a-storage-account"></a>Logboeken archiveren in een opslagaccount

Als u ervoor kiest om uw logboeken te archiveren naar een opslagaccount, betaalt u voor het volume aan logboeken dat naar het opslagaccount wordt verzonden. Zie de sectie **Platformlogboeken** van de pagina met Azure Monitor [prijzen voor specifieke](https://azure.microsoft.com/pricing/details/monitor/#platform-logs) prijzen.

1. Schakel het **selectievakje Archiveren naar een opslagaccount** in en klik vervolgens op de **knop Configureren.**

   > [!div class="mx-imgBorder"]   
   > ![Archiefopslag van pagina Diagnostische instellingen](media/storage-files-monitoring/diagnostic-logs-settings-pane-archive-storage.png)

2. Selecteer in **de vervolgkeuzelijst** Opslagaccount het opslagaccount waarin u uw logboeken wilt archiveren, klik op de knop **OK** en klik vervolgens op **de knop** Opslaan.

   [!INCLUDE [no retention policy](../../../includes/azure-storage-logs-retention-policy.md)]

   > [!NOTE]
   > Zie [Azure-resourcelogboeken](../../azure-monitor/essentials/resource-logs.md#send-to-azure-storage) archiveren voor meer informatie over de vereisten voor het opslagaccount voordat u een opslagaccount als exportbestemming kiest.

#### <a name="stream-logs-to-azure-event-hubs"></a>Logboeken streamen naar Azure Event Hubs

Als u ervoor kiest om uw logboeken naar een Event Hub te streamen, betaalt u voor het volume aan logboeken dat naar de Event Hub wordt verzonden. Zie de sectie **Platformlogboeken** van de pagina met Azure Monitor prijzen voor [specifieke](https://azure.microsoft.com/pricing/details/monitor/#platform-logs) prijzen.

1. Schakel het **selectievakje Streamen naar een Event Hub** in en klik vervolgens op de **knop** Configureren.

2. Kies in **het deelvenster Een Event Hub** selecteren de naamruimte, naam en beleidsnaam van de Event Hub waar u uw logboeken naar wilt streamen. 

   > [!div class="mx-imgBorder"]
   > ![Event Hub voor de pagina Diagnostische instellingen](media/storage-files-monitoring/diagnostic-logs-settings-pane-event-hub.png)

3. Klik op **de knop OK** en klik vervolgens op de **knop** Opslaan.

#### <a name="send-logs-to-azure-log-analytics"></a>Logboeken verzenden naar Azure Log Analytics

1. Schakel het **selectievakje Verzenden naar Log Analytics** in, selecteer een Log Analytics-werkruimte en klik vervolgens op de knop **Opslaan.**

   > [!div class="mx-imgBorder"]   
   > ![Pagina Met diagnostische instellingen logboekanalyse](media/storage-files-monitoring/diagnostic-logs-settings-pane-log-analytics.png)

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. Open een Windows PowerShell opdrachtvenster en meld u aan bij uw Azure-abonnement met behulp van de `Connect-AzAccount` opdracht . Volg vervolgens de instructies op het scherm.

   ```powershell
   Connect-AzAccount
   ```

2. Stel uw actieve abonnement in op het abonnement van het opslagaccount waar u logboekregistratie voor wilt inschakelen.

   ```powershell
   Set-AzContext -SubscriptionId <subscription-id>
   ```

#### <a name="archive-logs-to-a-storage-account"></a>Logboeken archiveren in een opslagaccount

Als u ervoor kiest om uw logboeken te archiveren in een opslagaccount, betaalt u voor het volume aan logboeken dat naar het opslagaccount wordt verzonden. Zie de sectie **Platformlogboeken** van de pagina met Azure Monitor prijzen voor [specifieke](https://azure.microsoft.com/pricing/details/monitor/#platform-logs) prijzen.

Schakel logboeken in met behulp van de PowerShell-cmdlet [Set-AzDiagnosticSetting,](/powershell/module/az.monitor/set-azdiagnosticsetting) samen met de `StorageAccountId` parameter .

```powershell
Set-AzDiagnosticSetting -ResourceId <storage-service-resource-id> -StorageAccountId <storage-account-resource-id> -Enabled $true -Category <operations-to-log> 
```

Vervang de `<storage-service-resource--id>` tijdelijke aanduiding in dit fragment door de resource-id van de Azure File-service. U vindt de resource-id in de Azure Portal door de pagina **Eigenschappen van** uw opslagaccount te openen.

U kunt `StorageRead` , en gebruiken voor de waarde van de parameter `StorageWrite` `StorageDelete` **Category.**

[!INCLUDE [no retention policy](../../../includes/azure-storage-logs-retention-policy.md)]

Hier volgt een voorbeeld:

`Set-AzDiagnosticSetting -ResourceId /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/fileServices/default -StorageAccountId /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/myloggingstorageaccount -Enabled $true -Category StorageWrite,StorageDelete`

Zie Azure-resourcelogboeken archiveren via Azure PowerShell voor een [beschrijving van elke parameter.](../../azure-monitor/essentials/resource-logs.md#send-to-azure-storage)

#### <a name="stream-logs-to-an-event-hub"></a>Logboeken streamen naar een Event Hub

Als u ervoor kiest om uw logboeken naar een Event Hub te streamen, betaalt u voor het volume aan logboeken dat naar de Event Hub wordt verzonden. Zie de sectie **Platformlogboeken** van de pagina met Azure Monitor prijzen voor [specifieke](https://azure.microsoft.com/pricing/details/monitor/#platform-logs) prijzen.

Schakel logboeken in met behulp [van de PowerShell-cmdlet Set-AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting) met de `EventHubAuthorizationRuleId` parameter .

```powershell
Set-AzDiagnosticSetting -ResourceId <storage-service-resource-id> -EventHubAuthorizationRuleId <event-hub-namespace-and-key-name> -Enabled $true -Category <operations-to-log> -RetentionEnabled <retention-bool> -RetentionInDays <number-of-days>
```

Hier volgt een voorbeeld:

`Set-AzDiagnosticSetting -ResourceId /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/fileServices/default -EventHubAuthorizationRuleId /subscriptions/20884142-a14v3-4234-5450-08b10c09f4/resourceGroups/myresourcegroup/providers/Microsoft.EventHub/namespaces/myeventhubnamespace/authorizationrules/RootManageSharedAccessKey -Enabled $true -Category StorageDelete`

Zie Stream Data to Event Hubs via PowerShell cmdlets (Gegevens streamen naar een Event Hubs [via PowerShell-cmdlets) voor](../../azure-monitor/essentials/resource-logs.md#send-to-azure-event-hubs)een beschrijving van elke parameter.

#### <a name="send-logs-to-log-analytics"></a>Logboeken naar Log Analytics verzenden

Schakel logboeken in met behulp [van de PowerShell-cmdlet Set-AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting) met de `WorkspaceId` parameter .

```powershell
Set-AzDiagnosticSetting -ResourceId <storage-service-resource-id> -WorkspaceId <log-analytics-workspace-resource-id> -Enabled $true -Category <operations-to-log> -RetentionEnabled <retention-bool> -RetentionInDays <number-of-days>
```

Hier volgt een voorbeeld:

`Set-AzDiagnosticSetting -ResourceId /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/fileServices/default -WorkspaceId /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.OperationalInsights/workspaces/my-analytic-workspace -Enabled $true -Category StorageDelete`

Zie Azure-resourcelogboeken [streamen naar Log Analytics-werkruimte in Azure Monitor voor meer Azure Monitor.](../../azure-monitor/essentials/resource-logs.md#send-to-log-analytics-workspace)

### <a name="azure-cli"></a>[Azure-CLI](#tab/azure-cli)

1. Open eerst de [Azure Cloud Shell](../../cloud-shell/overview.md)of als u [](/cli/azure/install-azure-cli) de Azure CLI lokaal hebt geïnstalleerd, opent u een opdrachtconsoletoepassing zoals Windows PowerShell.

2. Als uw identiteit is gekoppeld aan meer dan één abonnement, stelt u uw actieve abonnement in op het abonnement van het opslagaccount waar u logboeken voor wilt inschakelen.

   ```azurecli-interactive
   az account set --subscription <subscription-id>
   ```

   Vervang de `<subscription-id>` waarde van de tijdelijke aanduiding door de id van uw abonnement.

#### <a name="archive-logs-to-a-storage-account"></a>Logboeken archiveren in een opslagaccount

Als u ervoor kiest om uw logboeken te archiveren naar een opslagaccount, betaalt u voor het volume aan logboeken dat naar het opslagaccount wordt verzonden. Zie de sectie **Platformlogboeken** van de pagina met Azure Monitor prijzen voor [specifieke](https://azure.microsoft.com/pricing/details/monitor/#platform-logs) prijzen.

Schakel logboeken in met behulp [van de opdracht az monitor diagnostic-settings create.](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_create)

```azurecli-interactive
az monitor diagnostic-settings create --name <setting-name> --storage-account <storage-account-name> --resource <storage-service-resource-id> --resource-group <resource-group> --logs '[{"category": <operations>, "enabled": true}]'
```

Vervang de `<storage-service-resource--id>` tijdelijke aanduiding in dit fragment door de resource-id Blob Storage-service. U vindt de resource-id in de Azure Portal door de pagina **Eigenschappen van** uw opslagaccount te openen.

U kunt `StorageRead` , en gebruiken voor de waarde van de `StorageWrite` `StorageDelete` **categorieparameter.**

[!INCLUDE [no retention policy](../../../includes/azure-storage-logs-retention-policy.md)]

Hier volgt een voorbeeld:

`az monitor diagnostic-settings create --name setting1 --storage-account mystorageaccount --resource /subscriptions/938841be-a40c-4bf4-9210-08bcf06c09f9/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/myloggingstorageaccount/fileServices/default --resource-group myresourcegroup --logs '[{"category": StorageWrite, "enabled": true}]'`

Zie de logboeken voor archiveringsresources via de Azure CLI voor een [beschrijving van elke parameter.](../../azure-monitor/essentials/resource-logs.md#send-to-azure-storage)

#### <a name="stream-logs-to-an-event-hub"></a>Logboeken streamen naar een Event Hub

Als u ervoor kiest om uw logboeken naar een Event Hub te streamen, betaalt u voor het aantal logboeken dat naar de Event Hub wordt verzonden. Zie de sectie **Platformlogboeken** van de pagina met Azure Monitor prijzen voor [specifieke](https://azure.microsoft.com/pricing/details/monitor/#platform-logs) prijzen.

Schakel logboeken in met behulp van [de opdracht az monitor diagnostic-settings create.](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_create)

```azurecli-interactive
az monitor diagnostic-settings create --name <setting-name> --event-hub <event-hub-name> --event-hub-rule <event-hub-namespace-and-key-name> --resource <storage-account-resource-id> --logs '[{"category": <operations>, "enabled": true "retentionPolicy": {"days": <number-days>, "enabled": <retention-bool}}]'
```

Hier volgt een voorbeeld:

`az monitor diagnostic-settings create --name setting1 --event-hub myeventhub --event-hub-rule /subscriptions/938841be-a40c-4bf4-9210-08bcf06c09f9/resourceGroups/myresourcegroup/providers/Microsoft.EventHub/namespaces/myeventhubnamespace/authorizationrules/RootManageSharedAccessKey --resource /subscriptions/938841be-a40c-4bf4-9210-08bcf06c09f9/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/myloggingstorageaccount/fileServices/default --logs '[{"category": StorageDelete, "enabled": true }]'`

Zie Gegevens streamen naar een Event Hubs [via Azure CLI voor een beschrijving van elke parameter.](../../azure-monitor/essentials/resource-logs.md#send-to-azure-event-hubs)

#### <a name="send-logs-to-log-analytics"></a>Logboeken naar Log Analytics verzenden

Schakel logboeken in met behulp van [de opdracht az monitor diagnostic-settings create.](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_create)

```azurecli-interactive
az monitor diagnostic-settings create --name <setting-name> --workspace <log-analytics-workspace-resource-id> --resource <storage-account-resource-id> --logs '[{"category": <category name>, "enabled": true "retentionPolicy": {"days": <days>, "enabled": <retention-bool}}]'
```

Hier volgt een voorbeeld:

`az monitor diagnostic-settings create --name setting1 --workspace /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.OperationalInsights/workspaces/my-analytic-workspace --resource /subscriptions/938841be-a40c-4bf4-9210-08bcf06c09f9/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/myloggingstorageaccount/fileServices/default --logs '[{"category": StorageDelete, "enabled": true ]'`

 Zie Azure-resourcelogboeken [streamen naar een Log Analytics-werkruimte in Azure Monitor.](../../azure-monitor/essentials/resource-logs.md#send-to-log-analytics-workspace)

### <a name="template"></a>[Sjabloon](#tab/template)

Zie Diagnostische instelling Azure Resource Manager voor een sjabloon voor het maken van een [diagnostische Azure Storage.](../../azure-monitor/essentials/resource-manager-diagnostic-settings.md#diagnostic-setting-for-azure-storage)

---

## <a name="analyzing-metrics"></a>Metrische gegevens analyseren

U kunt metrische gegevens voor Azure Storage met metrische gegevens van andere Azure-services analyseren met behulp van Metrics Explorer. Open Metrics Explorer door Metrische **gegevens te kiezen** in **Azure Monitor** menu. Zie Aan de slag met Azure Metrics Explorer voor meer informatie over het gebruik [van Metrics Explorer.](../../azure-monitor/essentials/metrics-getting-started.md) 

Voor metrische gegevens die dimensies ondersteunen, kunt u de metrische gegevens filteren met de gewenste dimensiewaarde.  Zie Dimensies voor metrische gegevens voor een volledige Azure Storage [dimensies.](storage-files-monitoring-reference.md#metrics-dimensions) Metrische gegevens Azure Files zijn in deze naamruimten: 

- Microsoft.Storage/storageAccounts
- Microsoft.Storage/storageAccounts/fileServices

Zie ondersteunde metrische Azure Monitor voor een lijst Azure Files alle [ondersteunde Azure Monitor.](../../azure-monitor/essentials/metrics-supported.md#microsoftstoragestorageaccountsfileservices)

### <a name="accessing-metrics"></a>Toegang tot metrische gegevens

> [!TIP]
> Als u Azure CLI- of .NET-voorbeelden wilt bekijken, kiest u de bijbehorende tabbladen die hier worden vermeld.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

#### <a name="list-the-metric-definition"></a>De metrische definitie opsnr.

U kunt de metrische definitie van uw opslagaccount of de Azure Files service. Gebruik de cmdlet [Get-AzMetricDefinition.](/powershell/module/az.monitor/get-azmetricdefinition)

Vervang in dit voorbeeld de tijdelijke aanduiding door de resource-id van het hele opslagaccount of de `<resource-ID>` resource-id van de Azure Files service.  U vindt deze resource-ID's op de **pagina Eigenschappen** van uw opslagaccount in Azure Portal.

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetricDefinition -ResourceId $resourceId
```

#### <a name="reading-metric-values"></a>Metrische waarden lezen

U kunt metrische waarden op accountniveau van uw opslagaccount of de Azure Files lezen. Gebruik de [cmdlet Get-AzMetric.](/powershell/module/Az.Monitor/Get-AzMetric)

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetric -ResourceId $resourceId -MetricNames "UsedCapacity" -TimeGrain 01:00:00
```

### <a name="azure-cli"></a>[Azure-CLI](#tab/azure-cli)

#### <a name="list-the-account-level-metric-definition"></a>De metrische definitie op accountniveau in een lijst zetten

U kunt de metrische definitie van uw opslagaccount of de Azure Files service. Gebruik de [opdracht az monitor metrics list-definitions.](/cli/azure/monitor/metrics#az_monitor_metrics_list_definitions)
 
Vervang in dit voorbeeld de tijdelijke aanduiding door de resource-id van het hele opslagaccount of de `<resource-ID>` resource-id van de Azure Files service. U vindt deze resource-ID's op **de pagina Eigenschappen** van uw opslagaccount in Azure Portal.

```azurecli-interactive
   az monitor metrics list-definitions --resource <resource-ID>
```

#### <a name="read-account-level-metric-values"></a>Metrische waarden op accountniveau lezen

U kunt de metrische waarden van uw opslagaccount of de Azure Files lezen. Gebruik de [opdracht az monitor metrics list.](/cli/azure/monitor/metrics#az_monitor_metrics_list)

```azurecli-interactive
   az monitor metrics list --resource <resource-ID> --metric "UsedCapacity" --interval PT1H
```

### <a name="net"></a>[.NET](#tab/azure-portal)

Azure Monitor biedt de [.NET SDK voor het](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/) lezen van de definitie en waarden van metrische gegevens. De [voorbeeldcode](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/) laat zien hoe u de SDK gebruikt met verschillende parameters. U moet of een `0.18.0-preview` nieuwere versie gebruiken voor metrische opslaggegevens.
 
Vervang in deze voorbeelden de `<resource-ID>` tijdelijke aanduiding door de resource-id van het hele opslagaccount of de Azure Files service. U vindt deze resource-ID's op **de pagina Eigenschappen** van uw opslagaccount in Azure Portal.

Vervang de `<subscription-ID>` variabele door de id van uw abonnement. Zie De portal gebruiken om een Azure AD-toepassing en `<tenant-ID>` `<application-ID>` `<AccessKey>` [service-principal](../../active-directory/develop/howto-create-service-principal-portal.md)te maken die toegang hebben tot resources voor hulp bij het verkrijgen van waarden voor , en . 

#### <a name="list-the-account-level-metric-definition"></a>De metrische definitie op accountniveau in een lijst zetten

In het volgende voorbeeld ziet u hoe u een metrische definitie op accountniveau welijst:

```csharp
    public static async Task ListStorageMetricDefinition()
    {
        var resourceId = "<resource-ID>";
        var subscriptionId = "<subscription-ID>";
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";


        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;
        IEnumerable<MetricDefinition> metricDefinitions = await readOnlyClient.MetricDefinitions.ListAsync(resourceUri: resourceId, cancellationToken: new CancellationToken());

        foreach (var metricDefinition in metricDefinitions)
        {
            // Enumrate metric definition:
            //    Id
            //    ResourceId
            //    Name
            //    Unit
            //    MetricAvailabilities
            //    PrimaryAggregationType
            //    Dimensions
            //    IsDimensionRequired
        }
    }

```

#### <a name="reading-account-level-metric-values"></a>Metrische waarden op accountniveau lezen

In het volgende voorbeeld ziet u hoe u `UsedCapacity` gegevens op accountniveau kunt lezen:

```csharp
    public static async Task ReadStorageMetricValue()
    {
        var resourceId = "<resource-ID>";
        var subscriptionId = "<subscription-ID>";
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";

        MonitorClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;

        Response = await readOnlyClient.Metrics.ListAsync(
            resourceUri: resourceId,
            timespan: timeSpan,
            interval: System.TimeSpan.FromHours(1),
            metricnames: "UsedCapacity",

            aggregation: "Average",
            resultType: ResultType.Data,
            cancellationToken: CancellationToken.None);

        foreach (var metric in Response.Value)
        {
            // Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

#### <a name="reading-multidimensional-metric-values"></a>Multidimensionale metrische waarden lezen

Voor multidimensionale metrische gegevens moet u metagegevensfilters definiëren als u metrische gegevens wilt lezen over specifieke dimensiewaarden.

In het volgende voorbeeld ziet u hoe u metrische gegevens kunt lezen over de metrische gegevens die multidimensionaal ondersteunen:

```csharp
    public static async Task ReadStorageMetricValueTest()
    {
        // Resource ID for Azure Files
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/fileServices/default";
        var subscriptionId = "<subscription-ID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";

        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;
        // It's applicable to define meta data filter when a metric support dimension
        // More conditions can be added with the 'or' and 'and' operators, example: BlobType eq 'BlockBlob' or BlobType eq 'PageBlob'
        ODataQuery<MetadataValue> odataFilterMetrics = new ODataQuery<MetadataValue>(
            string.Format("BlobType eq '{0}'", "BlockBlob"));

        Response = readOnlyClient.Metrics.List(
                        resourceUri: resourceId,
                        timespan: timeSpan,
                        interval: System.TimeSpan.FromHours(1),
                        metricnames: "BlobCapacity",
                        odataQuery: odataFilterMetrics,
                        aggregation: "Average",
                        resultType: ResultType.Data);

        foreach (var metric in Response.Value)
        {
            //Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

# <a name="template"></a>[Sjabloon](#tab/template)

N.v.t.

---

## <a name="analyzing-logs"></a>Logboeken analyseren

U hebt toegang tot resourcelogboeken als een blob in een opslagaccount, als gebeurtenisgegevens of via Log Analytic-query's.

Zie Voor de lijst met SMB- en REST-bewerkingen die zijn geregistreerd, zie [Storage logged operations](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) and status messages (Opslag vastgelegde bewerkingen en statusberichten) en Azure Files [bewakingsgegevensreferentie](storage-files-monitoring-reference.md).

> [!NOTE]
> Azure Storage logboeken in Azure Monitor is in openbare preview en is beschikbaar voor preview-tests in alle openbare cloudregio's. Deze preview maakt logboeken mogelijk voor blobs (waaronder Azure Data Lake Storage Gen2), bestanden, wachtrijen, tabellen, Premium Storage-accounts in algemeen v1- en v2-opslagaccounts voor algemeen gebruik. Klassieke opslagaccounts worden niet ondersteund.

Logboekgegevens worden alleen gemaakt als er aanvragen worden gedaan voor het service-eindpunt. Als een opslagaccount bijvoorbeeld activiteit heeft in het bestands-eindpunt, maar niet in de tabel- of wachtrij-eindpunten, worden alleen logboeken gemaakt die betrekking hebben op de Azure File-service. Azure Storage logboeken bevatten gedetailleerde informatie over geslaagde en mislukte aanvragen voor een opslagservice. Deze informatie kan worden gebruikt voor het bewaken van afzonderlijke aanvragen en voor het vaststellen van problemen met een opslagservice. Aanvragen worden op basis van 'best effort' geregistreerd.

### <a name="log-authenticated-requests"></a>Geverifieerde aanvragen in logboeken

 De volgende typen geverifieerde aanvragen worden geregistreerd:

- Geslaagde aanvragen
- Mislukte aanvragen, inclusief time-out-, beperkings-, netwerk-, autorisatiefouten en overige fouten
- Aanvragen die gebruikmaken van Kerberos, NTLM of Shared Access Signature (SAS), inclusief mislukte en geslaagde aanvragen
- Aanvragen voor analysegegevens (klassieke logboekgegevens in de **$logs** container en klassieke metrische gegevens in **$metric** tabellen)

Aanvragen die door de Azure Files service zelf worden gedaan, zoals het maken of verwijderen van logboeken, worden niet geregistreerd. Zie Storage [logged operations](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) and status messages (Opslagbewerkingen en statusberichten) en Azure Files naslaginformatie over [bewakingsgegevens](storage-files-monitoring-reference.md)voor een volledige lijst van de SMB- en REST-aanvragen die worden geregistreerd.

### <a name="accessing-logs-in-a-storage-account"></a>Logboeken openen in een opslagaccount

Logboeken worden weergegeven als blobs die zijn opgeslagen in een container in het doelopslagaccount. Gegevens worden verzameld en opgeslagen in één blob als een JSON-nettolading met regel scheidingstekens. De naam van de blob volgt deze naamconventie:

`https://<destination-storage-account>.blob.core.windows.net/insights-logs-<storage-operation>/resourceId=/subscriptions/<subscription-ID>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<source-storage-account>/fileServices/default/y=<year>/m=<month>/d=<day>/h=<hour>/m=<minute>/PT1H.json`

Hier volgt een voorbeeld:

`https://mylogstorageaccount.blob.core.windows.net/insights-logs-storagewrite/resourceId=/subscriptions/`<br>`208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/fileServices/default/y=2019/m=07/d=30/h=23/m=12/PT1H.json`

### <a name="accessing-logs-in-an-event-hub"></a>Logboeken openen in een Event Hub

Logboeken die naar een Event Hub worden verzonden, worden niet opgeslagen als een bestand, maar u kunt controleren of de Event Hub de logboekgegevens heeft ontvangen. Ga in Azure Portal naar uw Event Hub en controleer of het aantal **binnenkomende** berichten groter is dan nul. 

![Auditlogboeken](media/storage-files-monitoring/event-hub-log.png)

U kunt logboekgegevens die naar uw Event Hub worden verzonden, openen en lezen met behulp van beveiligingsgegevens en hulpprogramma's voor gebeurtenisbeheer en bewaking. Zie Wat kan ik doen met de bewakingsgegevens die naar mijn [Event Hub worden verzonden? voor meer informatie.](../../azure-monitor/essentials/stream-monitoring-data-event-hubs.md)

### <a name="accessing-logs-in-a-log-analytics-workspace"></a>Logboeken openen in een Log Analytics-werkruimte

U kunt logboeken die naar een Log Analytics-werkruimte worden verzonden, openen met behulp Azure Monitor logboekquery's. Gegevens worden opgeslagen in de **tabel StorageFileLogs.** 

Zie Log Analytics-zelfstudie [voor meer informatie.](../../azure-monitor/logs/log-analytics-tutorial.md)

#### <a name="sample-kusto-queries"></a>Kusto-voorbeeldquery's

Hier volgen enkele query's die u kunt invoeren in de **zoekbalk** voor logboeken om u te helpen uw Azure Files. Deze query's werken met de [nieuwe taal](../../azure-monitor/logs/log-query-overview.md).

> [!IMPORTANT]
> Wanneer u Logboeken **selecteert** in het menu van de resourcegroep van het opslagaccount, wordt Log Analytics geopend met het querybereik ingesteld op de huidige resourcegroep. Dit betekent dat logboekquery's alleen gegevens uit die resourcegroep bevatten. Als u een query wilt uitvoeren die gegevens uit andere resources of gegevens van andere Azure-services bevat, selecteert u **Logboeken** in **het Azure Monitor** menu. Zie [Logboekquerybereik en tijdsbereik in Azure Monitor Log Analytics voor](../../azure-monitor/logs/scope.md) meer informatie.

Gebruik deze query's om uw Azure-bestands shares te bewaken:

- SMB-fouten weergeven in de afgelopen week

```Kusto
StorageFileLogs
| where Protocol == "SMB" and TimeGenerated >= ago(7d) and StatusCode contains "-"
| sort by StatusCode
```
- Een cirkeldiagram maken van SMB-bewerkingen in de afgelopen week

```Kusto
StorageFileLogs
| where Protocol == "SMB" and TimeGenerated >= ago(7d) 
| summarize count() by OperationName
| sort by count_ desc
| render piechart
```

- REST-fouten weergeven in de afgelopen week

```Kusto
StorageFileLogs
| where Protocol == "HTTPS" and TimeGenerated >= ago(7d) and StatusText !contains "Success"
| sort by StatusText asc
```

- Een cirkeldiagram maken van REST-bewerkingen in de afgelopen week

```Kusto
StorageFileLogs
| where Protocol == "HTTPS" and TimeGenerated >= ago(7d) 
| summarize count() by OperationName
| sort by count_ desc
| render piechart
```

Zie [StorageFileLogs](/azure/azure-monitor/reference/tables/storagefilelogs)als u de lijst met kolomnamen en beschrijvingen voor Azure Files wilt weergeven.

Zie Log Analytics-zelfstudie voor meer informatie over het schrijven [van query's.](../../azure-monitor/logs/log-analytics-tutorial.md)

## <a name="alerts"></a>Waarschuwingen

Azure Monitor waarschuwingen worden u proactief op de hoogte gesteld wanneer er belangrijke voorwaarden in uw bewakingsgegevens worden gevonden. Ze bieden u de mogelijkheid om problemen in uw systeem te identificeren en op te lossen voordat uw klanten deze merken. U kunt waarschuwingen instellen voor [metrische](../../azure-monitor/alerts/alerts-metric-overview.md)gegevens, [logboeken](../../azure-monitor/alerts/alerts-unified-log.md)en het [activiteitenlogboek.](../../azure-monitor/alerts/activity-log-alerts.md) 

De volgende tabel bevat enkele voorbeeldscenario's om te controleren en de juiste metrische gegevens voor de waarschuwing te gebruiken:

| Scenario | Te gebruiken metrische gegevens voor waarschuwing |
|-|-|
| De bestands share wordt beperkt. | Metrische gegevens: transacties<br>Dimensienaam: Antwoordtype <br>Dimensienaam: Bestandsshare (alleen Premium-bestandsshare) |
| De bestandsgrootte is 80% van de capaciteit. | Metrische gegevens: Bestandscapaciteit<br>Dimensienaam: Bestandsshare (alleen Premium-bestandsshare) |
| Het egressiebestandsbestand heeft in één dag meer dan 500 GiB overschreden. | Metrische gegevens: Egress<br>Dimensienaam: Bestandsshare (alleen Premium-bestandsshare) |

### <a name="how-to-create-alerts-for-azure-files"></a>Waarschuwingen maken voor Azure Files

1. Ga naar uw **opslagaccount** in **Azure Portal**. 

2. Klik **op Waarschuwingen** en klik vervolgens op + Nieuwe **waarschuwingsregel.**

3. Klik **op Resource bewerken,** selecteer het **resourcetype Bestand** en klik vervolgens op **Klaar.** 

4. Klik **op Voorwaarde toevoegen** en geef de volgende informatie op voor de waarschuwing: 

    - **Meting**
    - **Dimensienaam**
    - **Waarschuwingslogica**

5. Klik **op Actiegroepen toevoegen** en voeg een actiegroep (e-mail, sms, enzovoort) toe aan de waarschuwing door een bestaande actiegroep te selecteren of een nieuwe actiegroep te maken.

6. Vul de **waarschuwingsdetails** in, zoals de naam van **de waarschuwingsregel,** **Beschrijving** en **Ernst.**

7. Klik **op Waarschuwingsregel maken** om de waarschuwing te maken.

> [!NOTE]  
> Als u een waarschuwing maakt en deze te veel ruis maakt, past u de drempelwaarde en waarschuwingslogica aan.

### <a name="how-to-create-an-alert-if-a-file-share-is-throttled"></a>Een waarschuwing maken als een bestands share wordt beperkt

1. Ga naar uw **opslagaccount** in **Azure Portal**.
2. Klik in **de sectie** Bewaking op **Waarschuwingen** en klik vervolgens op + **Nieuwe waarschuwingsregel.**
3. Klik **op Resource bewerken,** selecteer het **resourcetype Bestand** voor het opslagaccount en klik vervolgens op **Klaar.** Als de naam van het opslagaccount bijvoorbeeld `contoso` is, selecteert u de `contoso/file` resource.
4. Klik **op Voorwaarde toevoegen** om een voorwaarde toe te voegen.
5. U ziet een lijst met signalen die worden ondersteund voor het opslagaccount. Selecteer de **metrische gegevens voor** Transacties.
6. Klik op de blade **Signaallogica** configureren op de **vervolgkeuzepagina Dimensienaam** en selecteer **Antwoordtype.**
7. Klik op **de vervolgkeuzepagina** Dimensiewaarden en selecteer de juiste antwoordtypen voor uw bestands share.

    Selecteer voor standaardbestands shares de volgende antwoordtypen:

    - SuccessWithShareIopsThrottling
    - SuccessWithThrottling
    - ClientShareIopsThrottlingError

    Selecteer voor Premium-bestands shares de volgende antwoordtypen:

    - SuccessWithShareEgressThrottling
    - SuccessWithShareIngressThrottling
    - SuccessWithShareIopsThrottling
    - ClientShareEgressThrottlingError
    - ClientShareIngressThrottlingError
    - ClientShareIopsThrottlingError

   > [!NOTE]
   > Als de antwoordtypen niet worden  vermeld in de vervolgkeuzelijst Dimensiewaarden, betekent dit dat de resource niet is beperkt. Als u de dimensiewaarden  wilt toevoegen, selecteert u naast de vervolgkeuzelijst Dimensiewaarden de optie Aangepaste waarde **toevoegen,** voert u het type respone in (bijvoorbeeld **SuccessWithThrottling**), selecteert u **OK** en herhaalt u deze stappen om alle toepasselijke antwoordtypen toe te voegen voor uw bestands share.

8. Voor **Premium-bestands shares** klikt u op **de vervolgkeuzepagina** Dimensienaam en **selecteert u Bestands share**. Ga **voor standaardbestands shares** naar stap **#10**.

   > [!NOTE]
   > Als de bestands share een  standaardbestands share is, worden de bestands share(s) niet weergegeven in de bestands share(s) omdat er geen metrische gegevens per share beschikbaar zijn voor standaardbestands shares. Beperkingswaarschuwingen voor standaardbestands shares worden geactiveerd als een bestands share binnen het opslagaccount wordt beperkt en de waarschuwing niet identificeert welke bestands share is beperkt. Aangezien metrische gegevens per share niet beschikbaar zijn voor standaardbestands shares, is het aanbeveling om één bestands share per opslagaccount te hebben.

9. Klik op **de vervolgkeuzepagina** Dimensiewaarden en selecteer de bestands share(s) die u wilt waarschuwen.
10. Definieer **de waarschuwingsparameters** (drempelwaarde, operator, aggregatiegranulariteit en frequentie van evaluatie) en klik op **Done**.

    > [!TIP]
    > Als u een statische drempel gebruikt, kan de grafiek met metrische gegevens helpen om een redelijke drempelwaarde te bepalen als de bestands share momenteel wordt beperkt. Als u een dynamische drempelwaarde gebruikt, worden in de grafiek met metrische gegevens de berekende drempelwaarden weergegeven op basis van recente gegevens.

11. Klik **op Actiegroepen toevoegen** om een actiegroep (e-mail, sms, enzovoort) aan de waarschuwing toe te voegen door een bestaande actiegroep te selecteren of een nieuwe actiegroep te maken. 
12. Vul de **waarschuwingsdetails** **in, zoals de naam** van de waarschuwingsregel, **Beschrijving** en **Ernst.**
13. Klik **op Waarschuwingsregel maken** om de waarschuwing te maken.

### <a name="how-to-create-an-alert-if-the-azure-file-share-size-is-80-of-capacity"></a>Een waarschuwing maken als de grootte van de Azure-bestands share 80% van de capaciteit is

1. Ga naar uw **opslagaccount** in **Azure Portal**.
2. Klik in **de sectie** Bewaking op **Waarschuwingen** en klik vervolgens op + **Nieuwe waarschuwingsregel.**
3. Klik **op Resource bewerken,** selecteer **het resourcetype Bestand** voor het opslagaccount en klik vervolgens op **Klaar.** Als de naam van het opslagaccount bijvoorbeeld `contoso` is, selecteert u de `contoso/file` resource.
4. Klik **op Voorwaarde toevoegen** om een voorwaarde toe te voegen.
5. U ziet een lijst met signalen die worden ondersteund voor het opslagaccount. Selecteer de **metrische gegevens** bestandscapaciteit.
6. Voor **Premium-bestands shares** klikt u op **de vervolgkeuzepagina** Dimensienaam en **selecteert u Bestands share.** Voor **standaardbestands shares** gaat u verder **met stap #8**.

   > [!NOTE]
   > Als de bestands share een  standaardbestands share is, worden in de bestandsdimensie de bestands share(s) niet vermeld, omdat metrische gegevens per share niet beschikbaar zijn voor standaardbestands shares. Waarschuwingen voor standaardbestands shares zijn gebaseerd op alle bestands shares in het opslagaccount. Omdat metrische gegevens per share niet beschikbaar zijn voor standaardbestands shares, wordt aanbevolen één bestands share per opslagaccount te hebben.

7. Klik op **de vervolgkeuzepagina** Dimensiewaarden en selecteer de bestands share(s) die u wilt waarschuwen.
8. Voer de **drempelwaarde** in bytes in. Als de bestands sharegrootte bijvoorbeeld 100 TiB is en u een waarschuwing wilt ontvangen wanneer de grootte van de bestands share 80% van de capaciteit is, is de drempelwaarde in bytes 87960930222080.
9. Definieer de rest van **de waarschuwingsparameters** (aggregatiegranulatie en evaluatiefrequentie) en klik op **Done.**
10. Klik **op Actiegroepen toevoegen** om een actiegroep (e-mail, sms, enzovoort) aan de waarschuwing toe te voegen door een bestaande actiegroep te selecteren of een nieuwe actiegroep te maken. 
11. Vul de details **van de waarschuwing in,** zoals de naam van **de waarschuwingsregel,** **Beschrijving** en **Ernst.**
12. Klik **op Waarschuwingsregel maken** om de waarschuwing te maken.

### <a name="how-to-create-an-alert-if-the-azure-file-share-egress-has-exceeded-500-gib-in-a-day"></a>Een waarschuwing maken als het egressie-gebruik van de Azure-bestands share 500 GiB per dag heeft overschreden

1. Ga naar uw **opslagaccount** in **Azure Portal**.
2. Klik in de sectie Bewaking op **Waarschuwingen** en klik vervolgens op **+ Nieuwe waarschuwingsregel.**
3. Klik **op Resource bewerken,** selecteer **het bestandsresourcetype** voor het opslagaccount en klik vervolgens op **Klaar.** Als de naam van het opslagaccount bijvoorbeeld contoso is, selecteert u de resource contoso/file.
4. Klik **op Voorwaarde toevoegen** om een voorwaarde toe te voegen.
5. U ziet een lijst met signalen die worden ondersteund voor het opslagaccount. Selecteer de **metrische gegevens voor** het egress-gebruik.
6. Voor **Premium-bestands shares** klikt u op **de vervolgkeuzepagina** Dimensienaam en **selecteert u Bestands share**. Ga **voor standaardbestands shares** naar stap **#8**.

   > [!NOTE]
   > Als de bestands share een  standaardbestands share is, worden de bestands share(s) niet weergegeven in de bestands share(s) omdat er geen metrische gegevens per share beschikbaar zijn voor standaardbestands shares. Waarschuwingen voor standaardbestands shares zijn gebaseerd op alle bestands shares in het opslagaccount. Aangezien metrische gegevens per share niet beschikbaar zijn voor standaardbestands shares, is het aanbeveling om één bestands share per opslagaccount te hebben.

7. Klik op **de vervolgkeuzepagina** Dimensiewaarden en selecteer de bestands share(s) die u wilt waarschuwen.
8. Voer **536870912000** bytes in bij Drempelwaarde. 
9. Klik op **de vervolgkeuzepagina Aggregatiegranulatie** en selecteer **24 uur.**
10. Selecteer de **Frequentie van de evaluatie** en klik op **Klaar.**
11. Klik **op Actiegroepen toevoegen** om een actiegroep (e-mail, sms, enzovoort) aan de waarschuwing toe te voegen door een bestaande actiegroep te selecteren of een nieuwe actiegroep te maken. 
12. Vul de **waarschuwingsdetails** in, zoals de naam van **de waarschuwingsregel,** **Beschrijving** en **Ernst.**
13. Klik **op Waarschuwingsregel maken** om de waarschuwing te maken.

## <a name="next-steps"></a>Volgende stappen

- [Azure Files naslaginformatie over bewakingsgegevens](storage-files-monitoring-reference.md)
- [Azure-resources bewaken met Azure Monitor](../../azure-monitor/essentials/monitor-azure-resource.md)
- [Azure Storage migratie van metrische gegevens](../common/storage-metrics-migration.md)
- [Een Azure Files-implementatie plannen](./storage-files-planning.md)
- [Azure Files implementeren](./storage-how-to-create-file-share.md)
- [Problemen met Azure Files in Windows oplossen](./storage-troubleshoot-windows-file-connection-problems.md)
- [Problemen met Azure Files in Linux oplossen](./storage-troubleshoot-linux-file-connection-problems.md)
