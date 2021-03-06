---
title: Azure Blob Storage met behulp van het NFS 3.0-protocol (preview) | Microsoft Docs
description: Leer hoe u een container in Blob Storage kunt monteren vanaf een virtuele Azure-machine (VM) of een client die on-premises wordt uitgevoerd met behulp van het NFS 3.0-protocol.
author: normesta
ms.subservice: blobs
ms.service: storage
ms.topic: conceptual
ms.date: 08/04/2020
ms.author: normesta
ms.reviewer: yzheng
ms.custom: references_regions
ms.openlocfilehash: 1c71c6b55049d81d5c1ff3e26cba3436f0e2dd23
ms.sourcegitcommit: 5ce88326f2b02fda54dad05df94cf0b440da284b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2021
ms.locfileid: "107890740"
---
# <a name="mount-blob-storage-by-using-the-network-file-system-nfs-30-protocol-preview"></a>Blob-opslag met behulp van het NFS-protocol (Network File System) 3.0 (preview)

U kunt een container in Blob Storage vanuit een Virtuele Linux-machine (VM) van Azure of een Linux-systeem dat on-premises wordt uitgevoerd, met behulp van het NFS 3.0-protocol. Dit artikel bevat stapsgewijs richtlijnen. Zie Protocolondersteuning voor Network [File System (NFS) 3.0 in Azure Blob Storage (preview) voor meer informatie over NFS 3.0-protocolondersteuning in](network-file-system-protocol-support.md)Blob Storage.

## <a name="step-1-register-the-nfs-30-protocol-feature-with-your-subscription"></a>Stap 1: registreer de NFS 3.0-protocolfunctie bij uw abonnement

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. Open een PowerShell-opdrachtvenster. 

2. Meld u aan bij uw Azure-abonnement met de opdracht `Connect-AzAccount` en volg de instructies op het scherm.

   ```powershell
   Connect-AzAccount
   ```

3. Als uw identiteit is gekoppeld aan meer dan één abonnement, stelt u uw actieve abonnement in.

   ```powershell
   $context = Get-AzSubscription -SubscriptionId <subscription-id>
   Set-AzContext $context
   ```
   
   Vervang de `<subscription-id>` waarde van de tijdelijke aanduiding door de id van uw abonnement.

4. Registreer de `AllowNFSV3` functie met behulp van de volgende opdracht.

   ```powershell
   Register-AzProviderFeature -FeatureName AllowNFSV3 -ProviderNamespace Microsoft.Storage 
   ```

5. Registreer de resourceprovider met behulp van de volgende opdracht.
    
   ```powershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.Storage   
   ```
   
# <a name="azure-cli"></a>[Azure-CLI](#tab/azure-cli)

1. Open een Terminal-venster.

2. Meld u aan bij uw Azure-abonnement met de opdracht `az login` en volg de instructies op het scherm.

   ```azurecli-interactive
   az login
   ```
   
3. Registreer de `AllowNFSV3` functie met behulp van de volgende opdracht.

   ```azurecli-interactive
   az feature register --namespace Microsoft.Storage --name AllowNFSV3 --subscription <subscription-id>
   ```

   Vervang de `<subscription-id>` waarde van de tijdelijke aanduiding door de id van uw abonnement.

4. Registreer de resourceprovider met behulp van de volgende opdracht.
    
   ```azurecli-interactive
   az provider register -n Microsoft.Storage --subscription <subscription-id>
   ```

   Vervang de `<subscription-id>` waarde van de tijdelijke aanduiding door de id van uw abonnement.

---

## <a name="step-2-verify-that-the-feature-is-registered"></a>Stap 2: controleren of de functie is geregistreerd 

Registratiegoedkeuring kan maximaal een uur duren. Gebruik de volgende opdrachten om te controleren of de registratie is voltooid.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage -FeatureName AllowNFSV3
```

# <a name="azure-cli"></a>[Azure-CLI](#tab/azure-cli)

```azurecli-interactive
az feature show --namespace Microsoft.Storage --name AllowNFSV3 --subscription <subscription-id>
```

Vervang de `<subscription-id>` waarde van de tijdelijke aanduiding door de id van uw abonnement.

---

## <a name="step-3-create-an-azure-virtual-network-vnet"></a>Stap 3: Een Azure Virtual Network (VNet) maken

Uw opslagaccount moet zijn opgenomen in een VNet. Met een VNet kunnen clients veilig verbinding maken met uw opslagaccount. Zie voor meer informatie over VNet en het maken van een VNet de [Virtual Network documentatie.](../../virtual-network/index.yml)

> [!NOTE]
> Clients in hetzelfde VNet kunnen containers aan uw account toevoegen. U kunt ook een container koppelen vanaf een client die wordt uitgevoerd in een on-premises netwerk, maar u moet eerst uw on-premises netwerk verbinden met uw VNet. Zie [Ondersteunde netwerkverbindingen.](network-file-system-protocol-support.md#supported-network-connections)

## <a name="step-4-configure-network-security"></a>Stap 4: netwerkbeveiliging configureren

De enige manier om de gegevens in uw account te beveiligen, is door een VNet en andere netwerkbeveiligingsinstellingen te gebruiken. Andere hulpprogramma's die worden gebruikt voor het beveiligen van gegevens, waaronder autorisatie van accountsleutels, Azure Active Directory-beveiliging (AD) en toegangsbeheerlijsten (ACL's), worden nog niet ondersteund in accounts waarbij de NFS 3.0-protocolondersteuning is ingeschakeld.

Als u de gegevens in uw account wilt beveiligen, bekijkt u deze aanbevelingen: Aanbevelingen voor [netwerkbeveiliging voor Blob Storage.](security-recommendations.md#networking)

## <a name="step-5-create-and-configure-a-storage-account"></a>Stap 5: een opslagaccount maken en configureren

Als u een container wilt toevoegen met behulp van NFS  3.0, moet u een opslagaccount maken nadat u de functie bij uw abonnement hebt geregistreerd. U kunt geen accounts inschakelen die bestonden voordat u de functie registreerde.

In de preview-versie van deze functie wordt het NFS 3.0-protocol ondersteund voor standaard v2-opslagaccounts voor algemeen gebruik en voor premium blok-blobopslagaccounts. Zie Overzicht van opslagaccounts voor meer informatie over [deze typen opslagaccounts.](../common/storage-account-overview.md)

Kies bij het configureren van het account de volgende waarden:

|Instelling | Premium-prestaties | Standaardprestaties  
|----|---|---|
|Locatie|Alle beschikbare regio's |Een van de volgende regio's: Australië - oost, Korea - centraal, VS - oost en VS - zuid-centraal   
|Prestaties|Premium| Standard
|Soort account|BlockBlobStorage| Algemeen v2
|Replicatie|Lokaal redundante opslag (LRS)| Lokaal redundante opslag (LRS)
|Verbindingsmethode|Openbaar eindpunt (geselecteerde netwerken) of privé-eindpunt |Openbaar eindpunt (geselecteerde netwerken) of privé-eindpunt
|Veilige overdracht vereist|Uitgeschakeld|Uitgeschakeld
|Hiërarchische naamruimte|Ingeschakeld|Ingeschakeld
|NFS V3|Ingeschakeld |Ingeschakeld 

U kunt de standaardwaarden voor alle andere instellingen accepteren. 

## <a name="step-6-create-a-container"></a>Stap 6: een container maken

Maak een container in uw opslagaccount met behulp van een van deze hulpprogramma's of SDK's:

|Hulpprogramma's|SDK's|
|---|---|
|[Azure-portal](https://portal.azure.com)|[.NET](data-lake-storage-directory-file-acl-dotnet.md#create-a-container)|
|[AzCopy](../common/storage-use-azcopy-v10.md#transfer-data)|[Java](data-lake-storage-directory-file-acl-java.md)|
|[PowerShell](data-lake-storage-directory-file-acl-powershell.md#create-a-container)|[Python](data-lake-storage-directory-file-acl-python.md#create-a-container)|
|[Azure-CLI](data-lake-storage-directory-file-acl-cli.md#create-a-container)|[JavaScript](data-lake-storage-directory-file-acl-javascript.md)|
||[REST](/rest/api/storageservices/create-container)|

## <a name="step-7-mount-the-container"></a>Stap 7: de container monteren

Maak een map op uw Linux-systeem en vervolgens een container in het opslagaccount.

1. Maak een map op een Linux-systeem.

   ```
   mkdir -p /mnt/test
   ```

2. Voer de volgende opdracht uit om een container te maken.

   ```
   mount -o sec=sys,vers=3,nolock,proto=tcp <storage-account-name>.blob.core.windows.net:/<storage-account-name>/<container-name>  /mnt/test
   ```

   - Vervang de `<storage-account-name>` tijdelijke aanduiding die wordt weergegeven in deze opdracht door de naam van uw opslagaccount.  

   - Vervang de `<container-name>` tijdelijke aanduiding door de naam van uw container.

---

## <a name="resolve-common-issues"></a>Veelvoorkomende problemen oplossen

|Probleem/fout | Oplossing|
|---|---|
|`Access denied by server while mounting`|Zorg ervoor dat de client wordt uitgevoerd in een ondersteund subnet. Zie [Ondersteunde netwerklocaties.](network-file-system-protocol-support.md#supported-network-connections)|
|`No such file or directory`| Zorg ervoor dat de container die u koppelt, is gemaakt nadat u hebt gecontroleerd of de functie is geregistreerd. Zie [Stap 2: controleren of de functie is geregistreerd.](#step-2-verify-that-the-feature-is-registered) Zorg er ook voor dat u de opdracht voor het monteren typt en dat de parameters rechtstreeks in de terminal staan. Als u een deel van deze opdracht vanuit een andere toepassing in de terminal kopieert, kunnen verborgen tekens in de gekopieerde gegevens deze fout veroorzaken.|

## <a name="see-also"></a>Zie ook

[Protocolondersteuning voor Network File System (NFS) 3.0 in Azure Blob Storage (preview)](network-file-system-protocol-support.md)
