---
title: 'Snelstart: resources maken en beheren in Azure Communication Services'
titleSuffix: An Azure Communication Services quickstart
description: In deze Snelstart leert u hoe u uw eerste Azure Communication Services-resource kunt maken en beheren.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2021
ms.topic: overview
ms.service: azure-communication-services
zone_pivot_groups: acs-plat-azp-azcli-net-ps
ms.openlocfilehash: aabb8bdf4105702aa623c45bc291770b05b8279e
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2021
ms.locfileid: "105726768"
---
# <a name="quickstart-create-and-manage-communication-services-resources"></a>Quickstart: Communication Services-resources maken en beheren

Ga aan de slag met Azure Communication Services door uw eerste Communication Services-resource te provisioneren. Communicatie services-resources kunnen worden ingericht via de [Azure Portal](https://portal.azure.com) of met de .net Management SDK. Met de Management SDK en de Azure Portal kunt u uw resources en interface maken, configureren, bijwerken en verwijderen met [Azure Resource Manager](../../azure-resource-manager/management/overview.md), de implementatie-en beheer service van Azure. Alle beschik bare functies in de Sdk's zijn beschikbaar in de Azure Portal. 


> [!WARNING]
> Houd er rekening mee dat terwijl communicatie services beschikbaar is in meerdere geographs, om een telefoon nummer op te halen waarvoor de resource een gegevens locatie moet hebben ingesteld op ' US '. Houd er ook rekening mee dat communicatie bronnen niet kunnen worden overgedragen naar een ander abonnement tijdens de openbare preview.

::: zone pivot="platform-azp"
[!INCLUDE [Azure portal](./includes/create-resource-azp.md)]
::: zone-end

::: zone pivot="platform-azcli"
[!INCLUDE [Azure CLI](./includes/create-resource-azcli.md)]
::: zone-end

::: zone pivot="platform-net"
[!INCLUDE [.NET](./includes/create-resource-net.md)]
::: zone-end

::: zone pivot="platform-powershell"
[!INCLUDE [PowerShell](./includes/create-resource-powershell.md)]
::: zone-end


## <a name="access-your-connection-strings-and-service-endpoints"></a>Toegang tot uw verbindingsreeksen en service-eindpunten

Met verbindings reeksen kunnen de Sdk's van de communicatie Services verbinding maken en verifiëren met Azure. U hebt toegang tot uw verbindingsreeksen voor Communication Services en service-eindpunten van de Azure Portal of programmatisch met Azure Resource Manager-API‘s

Nadat u naar uw communicatie Services-resource hebt genavigeerd, selecteert u **sleutels** in het navigatie menu en kopieert u de **verbindings reeks** of **eind punt** waarden voor gebruik door de sdk's van de communicatie Services. Houd er rekening mee dat u toegang hebt tot de primaire en secundaire sleutels. Dit kan handig zijn in scenario's waarin u tijdelijke toegang tot uw Communication Services-resources wilt verlenen aan een derde partij of staging-omgeving.

:::image type="content" source="./media/key.png" alt-text="Schermafbeelding van de sleutelpagina van de Communication Services.":::

U hebt ook toegang tot belang rijke informatie met behulp van Azure CLI, zoals uw resource groep of de sleutels voor een specifieke resource. 

Installeer [Azure cli](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?tabs=azure-cli) en gebruik de volgende opdracht om u aan te melden. U moet uw referenties opgeven om verbinding te maken met uw Azure-account.
```azurecli
az login
```

U hebt nu toegang tot belang rijke informatie over uw resources.
```azurecli
az communication list --resource-group "<resourceGroup>"

az communication list-key --name "<communicationName>" --resource-group "<resourceGroup>"
```

Als u een specifiek abonnement wilt selecteren, kunt u ook de vlag opgeven ```--subscription``` en de abonnements-id opgeven.
```
az communication list --resource-group  "resourceGroup>"  --subscription "<subscriptionID>"

az communication list-key --name "<communicationName>" --resource-group "resourceGroup>" --subscription "<subscriptionID>"
```

## <a name="store-your-connection-string"></a>Uw verbindingsreeks bijwerken

Sdk's voor communicatie Services gebruiken verbindings reeksen om aanvragen voor communicatie services te autoriseren. U hebt verschillende mogelijkheden om uw verbindingsreeks op te slaan:

* Een toepassing die wordt uitgevoerd op het bureaublad of op een apparaat kan de verbindingsreeks opslaan in een bestand **app.config** of **web.config**. Voeg de verbindingsreeks toe aan de sectie **AppSettings** in deze bestanden.
* Een toepassing die wordt uitgevoerd in een Azure App Service kan de verbindingsreeks opslaan in de [App Service-toepassingsinstellingen](../../app-service/configure-common.md). Voeg de verbindingsreeks toe aan de sectie **Verbindingsreeksen** van het tabblad Toepassingsinstellingen in de portal.
* U kunt uw verbindingsreeks opslaan in [Azure Key Vault](../../data-factory/store-credentials-in-key-vault.md).
* Als u uw toepassing lokaal uitvoert, wilt u mogelijk uw verbindingsreeks opslaan in een omgevingsvariabele.

### <a name="store-your-connection-string-in-an-environment-variable"></a>De verbindingsreeks naar een omgevingsvariabele schrijven

Als u een omgevingsvariabele wilt configureren, opent u een consolevenster en selecteert u het besturingssysteem op de onderstaande tabbladen. Vervang `<yourconnectionstring>` door de feitelijke verbindingsreeks.

#### <a name="windows"></a>[Windows](#tab/windows)

Open een consolevenster en voer de volgende opdracht in:

```console
setx COMMUNICATION_SERVICES_CONNECTION_STRING "<yourconnectionstring>"
```

Nadat u de omgevingsvariabele hebt toegevoegd, moet u actieve programma's die de omgevingsvariabele moeten lezen, opnieuw starten. Start ook het consolevenster opnieuw. Als u bijvoorbeeld Visual Studio als editor gebruikt, start u Visual Studio opnieuw voordat u het voorbeeld uitvoert.

#### <a name="macos"></a>[MacOS](#tab/unix)

Bewerk uw **.zshrc** en voeg de omgevingsvariabele toe:

```bash
export COMMUNICATION_SERVICES_CONNECTION_STRING="<yourconnectionstring>"
```

Nadat u de omgevingsvariabele toevoegt, voert u `source ~/.zshrc` uit vanuit het consolevenster om de wijzigingen van kracht te laten worden. Als u de omgevingsvariabele hebt gemaakt met uw IDE open, moet u de editor, IDE of shell mogelijk sluiten en opnieuw openen om toegang te krijgen tot de variabele.

#### <a name="linux"></a>[Linux](#tab/linux)

Bewerk uw **.bash_profile** en voeg de omgevingsvariabele toe:

```bash
export COMMUNICATION_SERVICES_CONNECTION_STRING="<yourconnectionstring>"
```

Nadat u de omgevingsvariabele toevoegt, voert u `source ~/.bash_profile` uit vanuit het consolevenster om de wijzigingen van kracht te laten worden. Als u de omgevingsvariabele hebt gemaakt met uw IDE open, moet u de editor, IDE of shell mogelijk sluiten en opnieuw openen om toegang te krijgen tot de variabele.

---

## <a name="clean-up-resources"></a>Resources opschonen

Als u een Communication Services-abonnement wilt opschonen en verwijderen, kunt u de resource of resourcegroep verwijderen. Als u de resourcegroep verwijdert, worden ook alle bijbehorende resources verwijderd.

Als er telefoonnummers zijn toegewezen aan uw resource bij het verwijderen van resources, worden de telefoonnummers automatisch van uw resource vrijgegeven.

## <a name="next-steps"></a>Volgende stappen

In deze Snelstart hebt u de volgende zaken geleerd:

> [!div class="checklist"]
> * Een Communication Services-resource aanmaken.
> * Resourcegeografie en -tags configureren
> * Toegang tot de sleutels voor die resource
> * De resource verwijderen

> [!div class="nextstepaction"]
> [Uw eerste gebruikers-toegangstokens maken](access-tokens.md)
