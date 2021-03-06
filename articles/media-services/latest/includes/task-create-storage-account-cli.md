---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI, devx-track-azurecli
ms.openlocfilehash: 44c349afe4bee4762b6dc2564c200f68f3296cc4
ms.sourcegitcommit: 5ce88326f2b02fda54dad05df94cf0b440da284b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2021
ms.locfileid: "107893132"
---
<!-- ### Create a storage account -->

Als u een Media Services-account gaat maken, moet u de naam van een Azure Storage-accountresource opgeven. Het opgegeven opslagaccount wordt gekoppeld aan uw Media Services-account. Zie [Storage-accounts](../storage-account-concept.md) voor meer informatie over het gebruik van Storage-accounts in Media Services.

U moet één primair **opslagaccount** hebben en  u kunt een aantal secundaire opslagaccounts koppelen aan uw Media Services account. Media Services ondersteunt **GPv2**-accounts (General-purpose v2) of **GPv1**-accounts (General-purpose v1). Blob-accounts kunt u niet instellen als **primaire** account. Zie [Opties voor Azure Storage-account](../../../storage/common/storage-account-overview.md) voor meer informatie over opslagaccounts. 

In dit voorbeeld maakt u een Standard LRS-account voor algemeen gebruik (v2). Als u wilt experimenteren met opslagaccounts, gebruikt u `--sku Standard_LRS`. Als u echter een SKU voor productie selecteert, kunt u overwegen om `--sku Standard_RAGRS` te gebruiken. Deze biedt geografische replicatie voor bedrijfscontinuïteit. Zie [Opslagaccounts](/cli/azure/storage/account) voor meer informatie.

Met de volgende opdracht maakt u een Storage-account die wordt gekoppeld aan de Media Services-account. Vervang in het onderstaande script door uw eigen naam voor het uitschrijven van gegevens met een lengte van minder dan `storageaccountforams` 24 tekens. `amsResourceGroup` moet overeenkomen met de waarde die u in de vorige stap hebt gegeven voor de resourcegroep.

```azurecli
az storage account create --name storageaccountforams --kind StorageV2 --sku Standard_LRS -l westus2 -g amsResourceGroup
```
