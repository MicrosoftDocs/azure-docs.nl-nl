---
title: Azure Table Storage-| Microsoft Docs
description: Meer informatie over het bewaken van de prestaties en beschikbaarheid van Azure Table Storage. Azure Table Storage-gegevens bewaken, meer informatie over configuratie en metrische gegevens en logboekgegevens analyseren.
author: normesta
services: storage
ms.service: storage
ms.topic: conceptual
ms.date: 10/26/2020
ms.author: normesta
ms.reviewer: fryu
ms.custom: monitoring, devx-track-csharp, devx-track-azurecli
ms.openlocfilehash: 11439afbe7bceb17dff5393d248c6c9f5d3f6b53
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/20/2021
ms.locfileid: "107767831"
---
# <a name="monitoring-azure-table-storage"></a>Azure Table Storage bewaken

Wanneer u kritieke toepassingen en bedrijfsprocessen hebt die afhankelijk zijn van Azure-resources, wilt u deze resources controleren op beschikbaarheid, prestaties en werking. In dit artikel worden de bewakingsgegevens beschreven die worden gegenereerd door Azure Table Storage en hoe u de functies van Azure Monitor kunt gebruiken om waarschuwingen over deze gegevens te analyseren.

> [!NOTE]
> Azure Storage logboeken in Azure Monitor is in openbare preview en is beschikbaar voor preview-tests in alle regio's van de openbare cloud. Deze preview maakt logboeken mogelijk voor blobs (waaronder Azure Data Lake Storage Gen2), bestanden, wachtrijen en tabellen. Deze functie is beschikbaar voor alle opslagaccounts die zijn gemaakt met het Azure Resource Manager implementatiemodel. Zie [Overzicht van opslagaccounts.](../common/storage-account-overview.md)

## <a name="monitor-overview"></a>Overzicht van bewaken

De **pagina** Overzicht in de Azure Portal voor elke Tabelopslagresource bevat een korte weergave van het resourcegebruik, zoals aanvragen en facturering per uur. Deze informatie is nuttig, maar er is slechts een kleine hoeveelheid bewakingsgegevens beschikbaar. Sommige van deze gegevens worden automatisch verzameld en zijn beschikbaar voor analyse zodra u de resource maakt. U kunt aanvullende typen gegevensverzameling inschakelen met enige configuratie.

## <a name="what-is-azure-monitor"></a>Wat is Azure Monitor?
Azure Table Storage maakt bewakingsgegevens [met behulp Azure Monitor](../../azure-monitor/overview.md), een volledige stack-bewakingsservice in Azure. Azure Monitor biedt een volledige set functies voor het bewaken van uw Azure-resources en -resources in andere clouds en on-premises. 

Begin met het artikel [Azure-resources bewaken met Azure Monitor](../../azure-monitor/essentials/monitor-azure-resource.md) waarin het volgende wordt beschreven:

- Wat is Azure Monitor?
- Kosten die zijn gekoppeld aan bewaking
- Bewakingsgegevens die zijn verzameld in Azure
- Gegevensverzameling configureren
- Standaardhulpprogramma's in Azure voor het analyseren en waarschuwen van bewakingsgegevens

De volgende secties zijn gebaseerd op dit artikel door een beschrijving te geven van de specifieke gegevens die zijn verzameld van Azure Storage. Voorbeelden laten zien hoe u gegevensverzameling configureert en deze gegevens analyseert met Azure-hulpprogramma's.

## <a name="monitoring-data"></a>Bewakingsgegevens

Azure Table Storage verzamelt dezelfde soorten bewakingsgegevens als andere Azure-resources, die worden beschreven in [Gegevens van Azure-resources bewaken.](../../azure-monitor/essentials/monitor-azure-resource.md#monitoring-data) 

Zie [Naslaginformatie over bewakingsgegevens van Azure Table Storage](monitor-table-storage-reference.md) voor gedetailleerde informatie over de metrische gegevens en logboekgegevens die zijn gemaakt door Azure Table Storage.

Metrische gegevens en logboeken in Azure Monitor bieden alleen ondersteuning Azure Resource Manager opslagaccounts. Azure Monitor biedt geen ondersteuning voor klassieke opslagaccounts. Als u metrische gegevens of logboeken wilt gebruiken in een klassiek opslagaccount, moet u migreren naar een Azure Resource Manager opslagaccount. Zie [Migreren naar Azure Resource Manager](../../virtual-machines/migration-classic-resource-manager-overview.md).

U kunt de klassieke metrische gegevens en logboeken blijven gebruiken als u dat wilt. Klassieke metrische gegevens en logboeken zijn beschikbaar in parallel met metrische gegevens en logboeken in Azure Monitor. De ondersteuning blijft van pas als Azure Storage service beëindigt op verouderde metrische gegevens en logboeken.

## <a name="collection-and-routing"></a>Verzameling en routering

Metrische gegevens van het platform en het activiteitenlogboek worden automatisch verzameld, maar kunnen worden doorgeleid naar andere locaties met behulp van een diagnostische instelling. 

Als u resourcelogboeken wilt verzamelen, moet u een diagnostische instelling maken. Wanneer u de instelling maakt, kiest **u tabel** als het type opslag waarop u logboeken wilt inschakelen. Geef vervolgens een van de volgende categorieën bewerkingen op waarvoor u logboeken wilt verzamelen. 

| Categorie | Beschrijving |
|:---|:---|
| StorageRead | Leesbewerkingen op objecten. |
| StorageWrite | Schrijfbewerkingen op objecten. |
| StorageDelete | Verwijder bewerkingen op objecten. |

## <a name="creating-a-diagnostic-setting"></a>Een diagnostische instelling maken

U kunt een diagnostische instelling maken met behulp van de Azure Portal, PowerShell, de Azure CLI of een Azure Resource Manager sjabloon. 

Zie Diagnostische instelling maken voor het verzamelen van platformlogboeken en metrische gegevens in Azure voor algemene [richtlijnen.](../../azure-monitor/essentials/diagnostic-settings.md)

> [!NOTE]
> Azure Storage logboeken in Azure Monitor is in openbare preview en is beschikbaar voor preview-tests in alle openbare cloudregio's. In deze preview worden logboeken voor blobs (waaronder Azure Data Lake Storage Gen2), bestanden, wachtrijen en tabellen. Deze functie is beschikbaar voor alle opslagaccounts die zijn gemaakt met het Azure Resource Manager implementatiemodel. Zie [Overzicht van opslagaccounts.](../common/storage-account-overview.md)

### <a name="azure-portal"></a>[Azure-portal](#tab/azure-portal)

1. Meld u aan bij Azure Portal.

2. Ga naar uw opslagaccount.

3. Klik in **de sectie** Bewaking op **Diagnostische instellingen (preview)**.

   > [!div class="mx-imgBorder"]
   > ![portal - Diagnostische logboeken](media/monitor-table-storage/diagnostic-logs-settings-pane.png)   

4. Kies **tabel** als het type opslag waar u logboeken voor wilt inschakelen.

5. Klik op **Diagnostische instelling toevoegen**.

   > [!div class="mx-imgBorder"]
   > ![portal - Resourcelogboeken - diagnostische instelling toevoegen](media/monitor-table-storage/diagnostic-logs-settings-pane-2.png)

   De **pagina Diagnostische** instellingen wordt weergegeven.

   > [!div class="mx-imgBorder"]
   > ![Pagina Resourcelogboeken](media/monitor-table-storage/diagnostic-logs-page.png)

6. Voer in **het veld** Naam van de pagina een naam in voor deze instelling voor resourcelogboek. Selecteer vervolgens welke bewerkingen u wilt geregistreerd (lees-, schrijf- en verwijderbewerkingen) en waar de logboeken moeten worden verzonden.

#### <a name="archive-logs-to-a-storage-account"></a>Logboeken archiveren in een opslagaccount

Als u ervoor kiest om uw logboeken te archiveren in een opslagaccount, betaalt u voor het volume aan logboeken dat naar het opslagaccount wordt verzonden. Zie de sectie **Platformlogboeken** van de pagina met Azure Monitor prijzen voor [specifieke](https://azure.microsoft.com/pricing/details/monitor/#platform-logs) prijzen.

1. Schakel het **selectievakje Archiveren naar een opslagaccount** in en klik vervolgens op de **knop Configureren.**

   > [!div class="mx-imgBorder"]   
   > ![Archiefopslag op de pagina Diagnostische instellingen](media/monitor-table-storage/diagnostic-logs-settings-pane-archive-storage.png)

2. Selecteer in **de vervolgkeuzelijst** Opslagaccount het opslagaccount waarin u uw logboeken wilt archiveren, klik op de knop **OK** en klik vervolgens op **de knop** Opslaan.

   [!INCLUDE [no retention policy](../../../includes/azure-storage-logs-retention-policy.md)]

   > [!NOTE]
   > Zie [Azure-resourcelogboeken](../../azure-monitor/essentials/resource-logs.md#send-to-azure-storage) archiveren voor meer informatie over de vereisten voor het opslagaccount voordat u een opslagaccount als exportbestemming kiest.

#### <a name="stream-logs-to-azure-event-hubs"></a>Logboeken streamen naar Azure Event Hubs

Als u ervoor kiest om uw logboeken naar een Event Hub te streamen, betaalt u voor het volume aan logboeken dat naar de Event Hub wordt verzonden. Zie de sectie **Platformlogboeken** van de pagina met Azure Monitor prijzen voor [specifieke](https://azure.microsoft.com/pricing/details/monitor/#platform-logs) prijzen.

1. Schakel het **selectievakje Streamen naar een Event Hub** in en klik vervolgens op de **knop** Configureren.

2. Kies in **het deelvenster Een Event Hub** selecteren de naamruimte, naam en beleidsnaam van de Event Hub waar u uw logboeken naar wilt streamen. 

   > [!div class="mx-imgBorder"]
   > ![Event Hub voor de pagina Diagnostische instellingen](media/monitor-table-storage/diagnostic-logs-settings-pane-event-hub.png)

3. Klik op **de knop OK** en klik vervolgens op de **knop** Opslaan.

#### <a name="send-logs-to-azure-log-analytics"></a>Logboeken verzenden naar Azure Log Analytics

1. Schakel het **selectievakje Verzenden naar Log Analytics** in, selecteer een Log Analytics-werkruimte en klik vervolgens op de knop **Opslaan.**

   > [!div class="mx-imgBorder"]   
   > ![Pagina Met diagnostische instellingen logboekanalyse](media/monitor-table-storage/diagnostic-logs-settings-pane-log-analytics.png)

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

Als u ervoor kiest om uw logboeken te archiveren naar een opslagaccount, betaalt u voor het volume aan logboeken dat naar het opslagaccount wordt verzonden. Zie de sectie **Platformlogboeken** van de pagina met Azure Monitor prijzen voor [specifieke](https://azure.microsoft.com/pricing/details/monitor/#platform-logs) prijzen.

Schakel logboeken in met [behulp van de PowerShell-cmdlet Set-AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting) en de `StorageAccountId` parameter .

```powershell
Set-AzDiagnosticSetting -ResourceId <storage-service-resource-id> -StorageAccountId <storage-account-resource-id> -Enabled $true -Category <operations-to-log>
```

Vervang de `<storage-service-resource--id>` tijdelijke aanduiding in dit fragment door de resource-id van de tabelservice. U vindt de resource-id in de Azure Portal door de pagina **Eigenschappen van** uw opslagaccount te openen.

U kunt `StorageRead` , en gebruiken voor de waarde van de parameter `StorageWrite` `StorageDelete` **Categorie.**

[!INCLUDE [no retention policy](../../../includes/azure-storage-logs-retention-policy.md)]

Hier volgt een voorbeeld:

`Set-AzDiagnosticSetting -ResourceId /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/tableServices/default -StorageAccountId /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/myloggingstorageaccount -Enabled $true -Category StorageWrite,StorageDelete`

Zie Azure-resourcelogboeken voor meer informatie Azure Storage het archiveren [van resourcelogboeken naar een Azure Storage.](../../azure-monitor/essentials/resource-logs.md#send-to-azure-storage)

#### <a name="stream-logs-to-an-event-hub"></a>Logboeken streamen naar een Event Hub

Als u ervoor kiest om uw logboeken naar een Event Hub te streamen, betaalt u voor het volume aan logboeken dat naar de Event Hub wordt verzonden. Zie de sectie **Platformlogboeken** van de pagina met Azure Monitor prijzen voor [specifieke](https://azure.microsoft.com/pricing/details/monitor/#platform-logs) prijzen.

Schakel logboeken in met behulp [van de PowerShell-cmdlet Set-AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting) met de `EventHubAuthorizationRuleId` parameter .

```powershell
Set-AzDiagnosticSetting -ResourceId <storage-service-resource-id> -EventHubAuthorizationRuleId <event-hub-namespace-and-key-name> -Enabled $true -Category <operations-to-log> -RetentionEnabled <retention-bool> -RetentionInDays <number-of-days>
```

Hier volgt een voorbeeld:

`Set-AzDiagnosticSetting -ResourceId /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/tableServices/default -EventHubAuthorizationRuleId /subscriptions/20884142-a14v3-4234-5450-08b10c09f4/resourceGroups/myresourcegroup/providers/Microsoft.EventHub/namespaces/myeventhubnamespace/authorizationrules/RootManageSharedAccessKey -Enabled $true -Category StorageDelete`

Zie Azure-resourcelogboeken voor meer informatie over het verzenden van resourcelogboeken [naar Event Hubs.](../../azure-monitor/essentials/resource-logs.md#send-to-azure-event-hubs)

#### <a name="send-logs-to-log-analytics"></a>Logboeken naar Log Analytics verzenden

Schakel logboeken in met [behulp van de PowerShell-cmdlet Set-AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting) met de `WorkspaceId` parameter .

```powershell
Set-AzDiagnosticSetting -ResourceId <storage-service-resource-id> -WorkspaceId <log-analytics-workspace-resource-id> -Enabled $true -Category <operations-to-log> -RetentionEnabled <retention-bool> -RetentionInDays <number-of-days>
```

Hier volgt een voorbeeld:

`Set-AzDiagnosticSetting -ResourceId /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/tableServices/default -WorkspaceId /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.OperationalInsights/workspaces/my-analytic-workspace -Enabled $true -Category StorageDelete`

Zie Azure-resourcelogboeken [streamen naar Log Analytics-werkruimte in Azure Monitor](../../azure-monitor/essentials/resource-logs.md#send-to-log-analytics-workspace)voor meer Azure Monitor.

### <a name="azure-cli"></a>[Azure-CLI](#tab/azure-cli)

1. Open eerst de [Azure Cloud Shell](../../cloud-shell/overview.md)of als u [](/cli/azure/install-azure-cli) de Azure CLI lokaal hebt geïnstalleerd, opent u een opdrachtconsoletoepassing zoals Windows PowerShell.

2. Als uw identiteit is gekoppeld aan meer dan één abonnement, stelt u uw actieve abonnement in op het abonnement van het opslagaccount waar u logboeken voor wilt inschakelen.

   ```azurecli-interactive
   az account set --subscription <subscription-id>
   ```

   Vervang de `<subscription-id>` waarde van de tijdelijke aanduiding door de id van uw abonnement.

#### <a name="archive-logs-to-a-storage-account"></a>Logboeken archiveren in een opslagaccount

Als u ervoor kiest om uw logboeken te archiveren naar een opslagaccount, betaalt u voor het volume aan logboeken dat naar het opslagaccount wordt verzonden. Zie de sectie **Platformlogboeken** van de pagina met Azure Monitor prijzen voor [specifieke](https://azure.microsoft.com/pricing/details/monitor/#platform-logs) prijzen.

Schakel logboeken in met behulp van [de opdracht az monitor diagnostic-settings create.](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_create)

```azurecli-interactive
az monitor diagnostic-settings create --name <setting-name> --storage-account <storage-account-name> --resource <storage-service-resource-id> --resource-group <resource-group> --logs '[{"category": <operations>, "enabled": true "retentionPolicy": {"days": <number-days>, "enabled": <retention-bool}}]'
```

Vervang de `<storage-service-resource--id>` tijdelijke aanduiding in dit fragment door de resource-id Table Storage-service. U vindt de resource-id in de Azure Portal door de pagina **Eigenschappen van** uw opslagaccount te openen.

U kunt `StorageRead` , en gebruiken voor de waarde van de `StorageWrite` `StorageDelete` **categorieparameter.**

[!INCLUDE [no retention policy](../../../includes/azure-storage-logs-retention-policy.md)]

Hier volgt een voorbeeld:

`az monitor diagnostic-settings create --name setting1 --storage-account mystorageaccount --resource /subscriptions/938841be-a40c-4bf4-9210-08bcf06c09f9/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/myloggingstorageaccount/tableServices/default --resource-group myresourcegroup --logs '[{"category": StorageWrite, "enabled": true}]'`

#### <a name="stream-logs-to-an-event-hub"></a>Logboeken streamen naar een Event Hub

Als u ervoor kiest om uw logboeken naar een Event Hub te streamen, betaalt u voor het volume aan logboeken dat naar de Event Hub wordt verzonden. Zie de sectie **Platformlogboeken** van de pagina met Azure Monitor prijzen voor [specifieke](https://azure.microsoft.com/pricing/details/monitor/#platform-logs) prijzen.

Schakel logboeken in met behulp van [de opdracht az monitor diagnostic-settings create.](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_create)

```azurecli-interactive
az monitor diagnostic-settings create --name <setting-name> --event-hub <event-hub-name> --event-hub-rule <event-hub-namespace-and-key-name> --resource <storage-account-resource-id> --logs '[{"category": <operations>, "enabled": true "retentionPolicy": {"days": <number-days>, "enabled": <retention-bool}}]'
```

Hier volgt een voorbeeld:

`az monitor diagnostic-settings create --name setting1 --event-hub myeventhub --event-hub-rule /subscriptions/938841be-a40c-4bf4-9210-08bcf06c09f9/resourceGroups/myresourcegroup/providers/Microsoft.EventHub/namespaces/myeventhubnamespace/authorizationrules/RootManageSharedAccessKey --resource /subscriptions/938841be-a40c-4bf4-9210-08bcf06c09f9/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/myloggingstorageaccount/tableServices/default --logs '[{"category": StorageDelete, "enabled": true }]'`

#### <a name="send-logs-to-log-analytics"></a>Logboeken naar Log Analytics verzenden

Schakel logboeken in met behulp van [de opdracht az monitor diagnostic-settings create.](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_create)

```azurecli-interactive
az monitor diagnostic-settings create --name <setting-name> --workspace <log-analytics-workspace-resource-id> --resource <storage-account-resource-id> --logs '[{"category": <category name>, "enabled": true "retentionPolicy": {"days": <days>, "enabled": <retention-bool}}]'
```

Hier volgt een voorbeeld:

`az monitor diagnostic-settings create --name setting1 --workspace /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.OperationalInsights/workspaces/my-analytic-workspace --resource /subscriptions/938841be-a40c-4bf4-9210-08bcf06c09f9/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/myloggingstorageaccount/tableServices/default --logs '[{"category": StorageDelete, "enabled": true ]'`

 Zie Azure-resourcelogboeken [streamen naar een Log Analytics-werkruimte in Azure Monitor.](../../azure-monitor/essentials/resource-logs.md#send-to-log-analytics-workspace)

### <a name="template"></a>[Sjabloon](#tab/template)

Zie Diagnostische instelling Azure Resource Manager voor een sjabloon voor het maken van een [diagnostische Azure Storage.](../../azure-monitor/essentials/resource-manager-diagnostic-settings.md#diagnostic-setting-for-azure-storage)

---

## <a name="analyzing-metrics"></a>Metrische gegevens analyseren

U kunt metrische gegevens voor Azure Storage met metrische gegevens van andere Azure-services analyseren met behulp van Metrics Explorer. Open Metrics Explorer door Metrische **gegevens te kiezen** in **Azure Monitor** menu. Zie Aan de slag met Azure Metrics Explorer voor meer informatie over het gebruik [van Metrics Explorer.](../../azure-monitor/essentials/metrics-getting-started.md) 

In dit voorbeeld ziet u hoe u **transacties op** accountniveau kunt weergeven.

![Schermopname van toegang tot metrische gegevens in de Azure Portal](./media/monitor-table-storage/access-metrics-portal.png)

Voor metrische gegevens die dimensies ondersteunen, kunt u de metrische gegevens filteren met de gewenste dimensiewaarde. In dit voorbeeld ziet u hoe u **transacties op** accountniveau kunt weergeven voor een specifieke bewerking door waarden te selecteren voor de **dimensie API-naam.**

![Schermopname van toegang tot metrische gegevens met dimensie in Azure Portal](./media/monitor-table-storage/access-metrics-portal-with-dimension.png)

Zie Dimensies voor metrische gegevens voor een volledige Azure Storage [dimensies.](monitor-table-storage-reference.md#metrics-dimensions)

Metrische gegevens voor Azure Table Storage staan in deze naamruimten: 

- Microsoft.Storage/storageAccounts
- Microsoft.Storage/storageAccounts/tableServices

Zie Ondersteunde metrische Azure Monitor voor een lijst met alle ondersteunde Azure Monitor, waaronder Azure Table [Storage.](../../azure-monitor/essentials/metrics-supported.md)


### <a name="accessing-metrics"></a>Toegang tot metrische gegevens

> [!TIP]
> Als u Azure CLI- of .NET-voorbeelden wilt bekijken, kiest u de bijbehorende tabbladen die hier worden vermeld.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

#### <a name="list-the-metric-definition"></a>De definitie van de metrische gegevens in een lijst zetten

U kunt de metrische definitie van uw opslagaccount of de Table Storage-service opnabelen. Gebruik de cmdlet [Get-AzMetricDefinition.](/powershell/module/az.monitor/get-azmetricdefinition)

In dit voorbeeld vervangt u de tijdelijke aanduiding door de resource-id van het hele opslagaccount of de `<resource-ID>` resource-id van de Table Storage-service.  U vindt deze resource-ID's op **de pagina Eigenschappen** van uw opslagaccount in Azure Portal.

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetricDefinition -ResourceId $resourceId
```

#### <a name="reading-metric-values"></a>Metrische waarden lezen

U kunt metrische waarden op accountniveau van uw opslagaccount of de Table Storage-service lezen. Gebruik de [cmdlet Get-AzMetric.](/powershell/module/Az.Monitor/Get-AzMetric)

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetric -ResourceId $resourceId -MetricNames "UsedCapacity" -TimeGrain 01:00:00
```

### <a name="azure-cli"></a>[Azure-CLI](#tab/azure-cli)

#### <a name="list-the-account-level-metric-definition"></a>De metrische definitie op accountniveau in een lijst zetten

U kunt de definitie van metrische gegevens van uw opslagaccount of de Table Storage-service gebruiken. Gebruik de [opdracht az monitor metrics list-definitions.](/cli/azure/monitor/metrics#az_monitor_metrics_list_definitions)
 
In dit voorbeeld vervangt u de tijdelijke aanduiding door de resource-id van het hele opslagaccount of de `<resource-ID>` resource-id van de Table Storage-service. U vindt deze resource-ID's op **de pagina Eigenschappen** van uw opslagaccount in Azure Portal.

```azurecli-interactive
   az monitor metrics list-definitions --resource <resource-ID>
```

#### <a name="read-account-level-metric-values"></a>Metrische waarden op accountniveau lezen

U kunt de metrische waarden van uw opslagaccount of de Table Storage-service lezen. Gebruik de [opdracht az monitor metrics list.](/cli/azure/monitor/metrics#az_monitor_metrics_list)

```azurecli-interactive
   az monitor metrics list --resource <resource-ID> --metric "UsedCapacity" --interval PT1H
```

### <a name="net"></a>[.NET](#tab/azure-portal)

Azure Monitor biedt de [.NET SDK voor het](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/) lezen van de definitie en waarden van metrische gegevens. De [voorbeeldcode laat](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/) zien hoe u de SDK gebruikt met verschillende parameters. U moet of een nieuwere `0.18.0-preview` versie gebruiken voor metrische opslaggegevens.
 
Vervang in deze voorbeelden de tijdelijke aanduiding `<resource-ID>` door de resource-id van het hele opslagaccount of de Table Storage-service. U vindt deze resource-ID's op de **pagina Eigenschappen** van uw opslagaccount in Azure Portal.

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
        // Resource ID for table storage
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/tableServices/default";
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

### <a name="template"></a>[Sjabloon](#tab/template)

N.v.t.

---

## <a name="analyzing-logs"></a>Logboeken analyseren

U hebt toegang tot resourcelogboeken als een blob in een opslagaccount, als gebeurtenisgegevens of via Log Analytic-query's.

Zie Azure Table Storage monitoring data reference (Referentie voor bewakingsgegevens van Azure Table Storage) voor gedetailleerde naslaginformatie over de velden die in deze [logboeken worden weergegeven.](monitor-table-storage-reference.md)

> [!NOTE]
> Azure Storage logboeken in Azure Monitor is in openbare preview en is beschikbaar voor preview-tests in alle openbare cloudregio's. Deze preview maakt logboeken mogelijk voor blobs (waaronder Azure Data Lake Storage Gen2), bestanden, wachtrijen, tabellen, Premium Storage-accounts in algemene v1- en v2-opslagaccounts voor algemeen gebruik. Klassieke opslagaccounts worden niet ondersteund.

Logboekgegevens worden alleen gemaakt als er aanvragen worden gedaan voor het service-eindpunt. Als een opslagaccount bijvoorbeeld activiteit heeft in het eindpunt van de tabel, maar niet in de blob- of wachtrij-eindpunten, worden alleen logboeken gemaakt die betrekking hebben op de tabelservice. Azure Storage logboeken bevatten gedetailleerde informatie over geslaagde en mislukte aanvragen voor een opslagservice. Deze informatie kan worden gebruikt voor het bewaken van afzonderlijke aanvragen en voor het vaststellen van problemen met een opslagservice. Aanvragen worden geregistreerd op basis van best-effort.

### <a name="log-authenticated-requests"></a>Geverifieerde aanvragen in logboeken

 De volgende typen geverifieerde aanvragen worden geregistreerd:

- Geslaagde aanvragen
- Mislukte aanvragen, inclusief time-out-, beperkings-, netwerk-, autorisatiefouten en overige fouten
- Aanvragen die gebruikmaken van een Shared Access Signature (SAS) of OAuth, inclusief mislukte en geslaagde aanvragen
- Aanvragen voor analysegegevens (klassieke logboekgegevens in de **$logs** container en metrische klassegegevens in de **$metric** tabellen)

Aanvragen die worden gedaan door de Table Storage-service zelf, zoals het maken of verwijderen van logboeken, worden niet geregistreerd. Zie Storage [logged operations and status messages](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) and Storage log format (Logboekindeling voor opslag) voor een volledige lijst met de [vastgelegde gegevens.](monitor-table-storage-reference.md)

### <a name="log-anonymous-requests"></a>Anonieme aanvragen in een logboek

 De volgende typen anonieme aanvragen worden geregistreerd:

- Geslaagde aanvragen
- Serverfouten
- Time-outfouten voor client en server
- Mislukte GET-aanvragen met foutcode 304 (niet gewijzigd)

Alle andere mislukte anonieme aanvragen worden niet geregistreerd. Zie Storage [logged operations and status messages](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) and Storage log format (Logboekindeling voor opslag) voor een volledige lijst met de [vastgelegde gegevens.](monitor-table-storage-reference.md)

### <a name="accessing-logs-in-a-storage-account"></a>Logboeken openen in een opslagaccount

Logboeken worden weergegeven als blobs die zijn opgeslagen in een container in het doelopslagaccount. Gegevens worden verzameld en opgeslagen in één blob als een JSON-nettolading met regel scheidingstekens. De naam van de blob volgt deze naamconventie:

`https://<destination-storage-account>.blob.core.windows.net/insights-logs-<storage-operation>/resourceId=/subscriptions/<subscription-ID>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<source-storage-account>/tableServices/default/y=<year>/m=<month>/d=<day>/h=<hour>/m=<minute>/PT1H.json`

Hier volgt een voorbeeld:

`https://mylogstorageaccount.blob.core.windows.net/insights-logs-storagewrite/resourceId=/subscriptions/`<br>`208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/tableServices/default/y=2019/m=07/d=30/h=23/m=12/PT1H.json`

### <a name="accessing-logs-in-an-event-hub"></a>Logboeken openen in een Event Hub

Logboeken die naar een Event Hub worden verzonden, worden niet opgeslagen als een bestand, maar u kunt controleren of de Event Hub de logboekgegevens heeft ontvangen. Ga in Azure Portal naar uw Event Hub en  controleer of het aantal binnenkomende berichten groter is dan nul. 

![Auditlogboeken](media/monitor-table-storage/event-hub-log.png)

U kunt logboekgegevens die naar uw Event Hub worden verzonden, openen en lezen met behulp van beveiligingsgegevens en hulpprogramma's voor gebeurtenisbeheer en bewaking. Zie [Azure-resourcelogboeken voor meer informatie.](../../azure-monitor/essentials/resource-logs.md#send-to-azure-event-hubs)

### <a name="accessing-logs-in-a-log-analytics-workspace"></a>Logboeken openen in een Log Analytics-werkruimte

U kunt logboeken die naar een Log Analytics-werkruimte worden verzonden, openen met behulp Azure Monitor logboekquery's.

Zie Azure-bewakingsgegevens [streamen naar Event Hub en externe partners voor meer informatie.](../../azure-monitor/essentials/stream-monitoring-data-event-hubs.md)

Gegevens worden opgeslagen in de **tabel StorageTableLogs.** 

#### <a name="sample-kusto-queries"></a>Kusto-voorbeeldquery's

Hier volgen enkele query's die u kunt invoeren in de **zoekbalk** voor logboeken om u te helpen uw Tabelopslag te bewaken. Deze query's werken met de [nieuwe taal](../../azure-monitor/logs/log-query-overview.md).

> [!IMPORTANT]
> Wanneer u Logboeken **selecteert** in het menu van de resourcegroep van het opslagaccount, wordt Log Analytics geopend met het querybereik ingesteld op de huidige resourcegroep. Dit betekent dat logboekquery's alleen gegevens uit die resourcegroep bevatten. Als u een query wilt uitvoeren die gegevens uit andere resources of gegevens van andere Azure-services bevat, selecteert u **Logboeken** in **het Azure Monitor** menu. Zie [Logboekquerybereik en tijdsbereik in Azure Monitor Log Analytics voor](../../azure-monitor/logs/scope.md) meer informatie.

Gebruik deze query's om uw Azure Storage controleren:

* De tien meest voorkomende fouten in de afgelopen drie dagen op te sneren.

    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d) and StatusText !contains "Success"
    | summarize count() by StatusText
    | top 10 by count_ desc
    ```
* De top 10 van bewerkingen die de meeste fouten in de afgelopen drie dagen hebben veroorzaakt, op een lijst zetten.

    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d) and StatusText !contains "Success"
    | summarize count() by OperationName
    | top 10 by count_ desc
    ```
* De top 10 van bewerkingen met de langste end-to-end-latentie in de afgelopen drie dagen.

    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d)
    | top 10 by DurationMs desc
    | project TimeGenerated, OperationName, DurationMs, ServerLatencyMs, ClientLatencyMs = DurationMs - ServerLatencyMs
    ```
* Om een lijst weer te maken van alle bewerkingen die in de afgelopen drie dagen beperkingsfouten aan de serverzijde hebben veroorzaakt.

    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d) and StatusText contains "ServerBusy"
    | project TimeGenerated, OperationName, StatusCode, StatusText
    ```
* Om alle aanvragen met anonieme toegang in de afgelopen drie dagen weer te geven.

    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d) and AuthenticationType == "Anonymous"
    | project TimeGenerated, OperationName, AuthenticationType, Uri
    ```
* Een cirkeldiagram maken van bewerkingen die in de afgelopen drie dagen zijn gebruikt.
    ```Kusto
    StorageTableLogs
    | where TimeGenerated > ago(3d)
    | summarize count() by OperationName
    | sort by count_ desc 
    | render piechart
    ```
## <a name="faq"></a>Veelgestelde vragen

**Ondersteunt Azure Storage metrische gegevens voor Managed Disks of onmanaged schijven?**

Nee. Azure Compute ondersteunt de metrische gegevens op schijven. Zie Metrische gegevens per schijf voor [beheerde en niet-beheerde schijven voor meer informatie.](https://azure.microsoft.com/blog/per-disk-metrics-managed-disks/)

## <a name="next-steps"></a>Volgende stappen

- Zie Azure Table Storage monitoring data reference (Referentie voor bewakingsgegevens van Azure Table Storage) voor een verwijzing naar de logboeken en metrische gegevens die zijn gemaakt [door Azure Table Storage.](monitor-table-storage-reference.md)
- Zie Azure-resources bewaken met azure-resources met Azure Monitor voor meer [Azure Monitor.](../../azure-monitor/essentials/monitor-azure-resource.md)
- Zie migratie van metrische gegevens voor meer [Azure Storage migratie van metrische gegevens.](../common/storage-metrics-migration.md)