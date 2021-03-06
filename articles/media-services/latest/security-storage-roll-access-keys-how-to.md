---
title: Media Services v3 na de toegangs sleutels van de Rolling opslag bijwerken | Microsoft Docs
description: In dit artikel vindt u richt lijnen voor het bijwerken van Media Services v3 na de toegangs sleutels voor de Rolling opslag.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2021
ms.author: inhenkel
ms.openlocfilehash: 385e6109a80c8e1e455f98b5d02ca5762f8051c7
ms.sourcegitcommit: 02bc06155692213ef031f049f5dcf4c418e9f509
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2021
ms.locfileid: "106281847"
---
# <a name="update-media-services-v3-after-rolling-storage-access-keys"></a>Media Services v3 na de toegangs sleutels voor de Rolling opslag bijwerken

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

U wordt gevraagd om een Azure Storage account te selecteren wanneer u een nieuw Azure Media Services-account (AMS) maakt.  U kunt meer dan één opslag account toevoegen aan uw Media Services-account. In dit artikel wordt beschreven hoe u opslag sleutels kunt draaien. Ook wordt uitgelegd hoe u opslag accounts toevoegt aan een media account.

U moet [Azure Resource Manager-api's](/rest/api/media/operations/azure-media-services-rest-api-reference) en [Power shell](/powershell/module/az.media)gebruiken om de acties uit te voeren die in dit artikel worden beschreven.  Zie [Azure-resources beheren met Power shell en Resource Manager](../../azure-resource-manager/management/manage-resource-groups-powershell.md)voor meer informatie.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="storage-access-key-generation"></a>Toegangs sleutel voor opslag genereren

Wanneer een nieuw opslag account wordt gemaakt, genereert Azure 2 512-bits toegangs sleutels voor opslag, die worden gebruikt voor het verifiëren van toegang tot uw opslag account. Om uw opslag verbindingen veiliger te maken, moet u de toegangs sleutel voor opslag periodiek opnieuw genereren en draaien. Er zijn twee toegangs sleutels (primair en secundair) beschikbaar waarmee u verbindingen met het opslag account met behulp van één toegangs sleutel kunt onderhouden terwijl u de andere toegangs sleutel opnieuw genereert. Deze procedure wordt ook wel ' Rolling toegangs sleutels ' genoemd.

Media Services is afhankelijk van een opslag sleutel die hieraan is gegeven. Met name de Locators die worden gebruikt om uw assets te streamen of te downloaden, zijn afhankelijk van de opgegeven toegangs sleutel voor opslag. Wanneer er een AMS-account wordt gemaakt, neemt het standaard afhankelijk van de primaire toegangs sleutel voor de opslag. Als gebruiker kunt u echter de opslag sleutel bijwerken die AMS heeft. U moet Media Services weten welke sleutel moet worden gebruikt door de volgende stappen uit te voeren:

>[!NOTE]
> Als u meerdere opslag accounts hebt, voert u deze procedure uit met elk opslag account. De volg orde waarin u opslag sleutels roteert, is niet opgelost. U kunt de secundaire sleutel eerst draaien en vervolgens de primaire sleutel of andersom.
>
> Voordat u de stappen uitvoert op een productie account, moet u ervoor zorgen dat u deze test op een pre-productie rekening.
>

## <a name="steps-to-rotate-storage-keys"></a>Stappen voor het draaien van opslag sleutels
 
 1. Wijzig de primaire sleutel voor het opslag account via de Power shell-cmdlet of [Azure](https://portal.azure.com/) Portal.
 2. Roep de `Sync-AzMediaServiceStorageKeys` cmdlet aan met de juiste para meters om het Media account te dwingen om sleutels voor het opslag account op te halen
 
    In het volgende voor beeld ziet u hoe u sleutels synchroniseert met opslag accounts.
  
    `Sync-AzMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId`
  
 3. Wacht een uur. Controleer of de streaming-scenario's werken.
 4. Wijzig de secundaire sleutel voor het opslag account via de Power shell-cmdlet of de Azure Portal.
 5. Roep `Sync-AzMediaServiceStorageKeys` Power shell aan met de juiste para meters om ervoor te zorgen dat het Media-account nieuwe sleutels voor het opslag account ophaalt.
 6. Wacht een uur. Controleer of de streaming-scenario's werken.
 
### <a name="a-powershell-cmdlet-example"></a>Een voor beeld van een Power shell-cmdlet

In het volgende voor beeld ziet u hoe u het opslag account kunt ophalen en synchroniseren met het AMS-account.

```console
$regionName = "West US"
$resourceGroupName = "SkyMedia-USWest-App"
$mediaAccountName = "sky"
$storageAccountName = "skystorage"
$storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

Sync-AzMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId
```

## <a name="steps-to-add-storage-accounts-to-your-ams-account"></a>Stappen voor het toevoegen van opslag accounts aan uw AMS-account

In het volgende artikel ziet u hoe u opslag accounts toevoegt aan uw AMS-account: [meerdere opslag accounts koppelen aan een Media Services-account](storage-managing-multiple-storage-accounts-how-to.md).
