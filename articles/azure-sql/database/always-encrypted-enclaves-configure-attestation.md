---
title: Azure Attestation configureren voor uw logische Azure SQL-Server
description: Azure Attestation configureren voor Always Encrypted met beveiligde enclaves in Azure SQL Database.
keywords: versleutelen van gegevens, SQL-versleuteling, database versleuteling, gevoelige gegevens, Always Encrypted, beveiligde enclaves, SGX, Attestation
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: how-to
author: jaszymas
ms.author: jaszymas
ms.reviwer: vanto
ms.date: 01/15/2021
ms.openlocfilehash: a51aa15e1338380d4b4179e7fb8899273750c374
ms.sourcegitcommit: 5fd1f72a96f4f343543072eadd7cdec52e86511e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2021
ms.locfileid: "106107177"
---
# <a name="configure-azure-attestation-for-your-azure-sql-logical-server"></a>Azure Attestation configureren voor uw logische Azure SQL-Server

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Always Encrypted met beveiligde enclaves voor Azure SQL Database is momenteel beschikbaar als **open bare preview**.

[Microsoft Azure Attestation](../../attestation/overview.md) is een oplossing voor het TEEs (Trusted Execution Environment), met inbegrip van Intel-software Guard Extensions (Intel SGX) enclaves. 

Als u Azure-Attestation wilt gebruiken voor de verificatie van Intel SGX-enclaves die wordt gebruikt voor [Always encrypted met Secure enclaves](/sql/relational-databases/security/encryption/always-encrypted-enclaves) in Azure SQL database, moet u het volgende doen:

1. Maak een [Attestation-provider](../../attestation/basic-concepts.md#attestation-provider) en configureer deze met het aanbevolen Attestation-beleid.

2. Verleen uw logische Azure SQL-Server toegang tot uw Attestation-provider.

> [!NOTE]
> Het configureren van Attestation is de verantwoordelijkheid van de Attestation-beheerder. Zie [rollen en verantwoordelijkheden bij het configureren van SGX-enclaves en-Attestation](always-encrypted-enclaves-plan.md#roles-and-responsibilities-when-configuring-sgx-enclaves-and-attestation).

## <a name="requirements"></a>Vereisten

De logische Azure SQL-Server en de Attestation-provider moeten deel uitmaken van dezelfde Azure Active Directory Tenant. Cross-Tenant interacties worden niet ondersteund. 

Er moet een Azure AD-identiteit worden toegewezen aan de logische Azure SQL-Server. Als Attestation-beheerder moet u de Azure AD-identiteit van de server verkrijgen van de Azure SQL Database beheerder voor die server. U gaat de identiteit gebruiken om de server toegang te verlenen tot de Attestation-provider. 

Zie [een Azure AD-identiteit toewijzen aan uw server](transparent-data-encryption-byok-configure.md#assign-an-azure-active-directory-azure-ad-identity-to-your-server)voor instructies over het maken van een server met een identiteit of het toewijzen van een identiteit aan een bestaande server met behulp van Power shell en Azure cli.

## <a name="create-and-configure-an-attestation-provider"></a>Een Attestation-provider maken en configureren

Een [Attestation-provider](../../attestation/basic-concepts.md#attestation-provider) is een bron in azure Attestation waarbij [Attestation-aanvragen](../../attestation/basic-concepts.md#attestation-request) worden geëvalueerd tegen [attest beleid](../../attestation/basic-concepts.md#attestation-request) en [Attestation-tokens](../../attestation/basic-concepts.md#attestation-token). 

Attestation-beleid wordt opgegeven met behulp van de grammatica van de [claim regel](../../attestation/claim-rule-grammar.md).

Micro soft raadt het volgende beleid aan voor de attesting van Intel SGX enclaves die wordt gebruikt voor Always Encrypted in Azure SQL Database:

```output
version= 1.0;
authorizationrules 
{
       [ type=="x-ms-sgx-is-debuggable", value==false ]
        && [ type=="x-ms-sgx-product-id", value==4639 ]
        && [ type=="x-ms-sgx-svn", value>= 0 ]
        && [ type=="x-ms-sgx-mrsigner", value=="e31c9e505f37a58de09335075fc8591254313eb20bb1a27e5443cc450b6e33e5"] 
    => permit();
};
```

Met het bovenstaande beleid wordt gecontroleerd:

- De enclave in Azure SQL Database biedt geen ondersteuning voor fout opsporing. 
  > Enclaves kan worden geladen met fout opsporing uitgeschakeld of ingeschakeld. De ondersteuning voor fout opsporing is ontworpen om ontwikkel aars in staat te stellen de code die wordt uitgevoerd in een enclave op te lossen. In een productie systeem kan fout opsporing een beheerder in staat stellen om de inhoud van de enclave te onderzoeken, waardoor het beveiligings niveau dat door de enclave wordt geboden, wordt verminderd. Met het aanbevolen beleid wordt fout opsporing uitgeschakeld om er zeker van te zijn dat als een kwaadwillende beheerder probeert fout opsporing in te scha kelen door over te nemen van de enclave computer, de Attestation mislukt. 
- De product-ID van de enclave komt overeen met de product-ID die is toegewezen aan Always Encrypted met beveiligde enclaves.
  > Elke enclave heeft een unieke product-ID die de enclave van andere enclaves onderscheidt. De product-ID die is toegewezen aan de Always Encrypted enclave is 4639. 
- Het nummer van de beveiligings versie (SVN) van de bibliotheek is groter dan 0.
  > Met de SVN kan micro soft reageren op mogelijke beveiligings fouten die zijn geïdentificeerd in de enclave-code. Als er een beveiligings probleem wordt dicovered en opgelost, implementeert micro soft een nieuwe versie van de enclave met een nieuwe (incremented) SVN. Het hierboven aanbevolen beleid wordt bijgewerkt om de nieuwe SVN weer te geven. Als u uw beleid bijwerkt zodat dit overeenkomt met het aanbevolen beleid, kunt u ervoor zorgen dat als een kwaadwillende beheerder een oudere en onveilige enclave probeert te laden, de Attestation mislukt.
- De bibliotheek in het enclave is ondertekend met behulp van de micro soft-handtekening sleutel (de waarde van de x-MS-SGX-mrsigner-claim is de hash van de ondertekeningssleutel).
  > Een van de belangrijkste doel stellingen is het overtuigen van clients dat de binaire waarde die in de enclave wordt uitgevoerd, het binaire bestand is dat moet worden uitgevoerd. Attestation-beleid voorziet in twee mechanismen voor dit doel. Een is de **mrenclave** -claim die de hash is van het binaire bestand dat moet worden uitgevoerd in een enclave. Het probleem met de **mrenclave** is dat de binaire hash zelfs wordt gewijzigd met trivial wijzigingen in de code, waardoor het lastig is om de code die in de enclave wordt uitgevoerd, ongedaan te maken. Daarom raden wij het gebruik van de **mrsigner** aan. Dit is een hash van een sleutel die wordt gebruikt om de binaire enclave te ondertekenen. Wanneer micro soft Revs de enclave, blijft de **mrsigner** hetzelfde zolang de handtekening sleutel niet verandert. Op deze manier wordt het haalbaar om bijgewerkte binaire bestanden te implementeren zonder de toepassingen van klanten te verbreken. 

> [!IMPORTANT]
> Er wordt een Attestation-provider gemaakt met het standaard beleid voor Intel SGX enclaves, waarmee de code die niet binnen de enclave wordt uitgevoerd, niet wordt gevalideerd. Micro soft adviseert u ten zeerste het aanbevolen beleid in te stellen en het standaard beleid niet te gebruiken voor Always Encrypted met beveiligde enclaves.

Voor instructies voor het maken van een Attestation-provider en het configureren van een Attestation-beleid met behulp van:

- [Snelstartgids: Azure Attestation met Azure Portal instellen](../../attestation/quickstart-portal.md)
    > [!IMPORTANT]
    > Wanneer u uw Attestation-beleid configureert met Azure Portal, stelt u Attestation-type in op `SGX-IntelSDK` .
- [Quickstart: Azure Attestation instellen met behulp van Microsoft Azure PowerShell](../../attestation/quickstart-powershell.md)
    > [!IMPORTANT]
    > Wanneer u uw Attestation-beleid configureert met Azure PowerShell, stelt u de `Tee` para meter in op `SgxEnclave` .
- [Quickstart: Azure Attestation instellen met Azure CLI](../../attestation/quickstart-azure-cli.md)
    > [!IMPORTANT]
    > Wanneer u uw Attestation-beleid configureert met Azure CLI, stelt u de `attestation-type` para meter in op `SGX-IntelSDK` .

## <a name="determine-the-attestation-url-for-your-attestation-policy"></a>De Attestation-URL voor het Attestation-beleid bepalen

Nadat u een Attestation-beleid hebt geconfigureerd, moet u de URL van de Attestation delen, die verwijst naar het beleid, Administrators van toepassingen die gebruikmaken van Always Encrypted met beveiligde enclaves in Azure SQL Database. Toepassings beheerders of/en toepassings gebruikers moeten hun apps configureren met de Attestation-URL, zodat ze instructies kunnen uitvoeren die gebruikmaken van beveiligde enclaves.

### <a name="use-powershell-to-determine-the-attestation-url"></a>Power shell gebruiken om de Attestation-URL te bepalen

Gebruik het volgende script om uw Attestation-URL te bepalen:

```powershell
$attestationProvider = Get-AzAttestation -Name $attestationProviderName -ResourceGroupName $attestationResourceGroupName 
$attestationUrl = $attestationProvider.AttestUri + "/attest/SgxEnclave"
Write-Host "Your attestation URL is: " $attestationUrl 
```

### <a name="use-azure-portal-to-determine-the-attestation-url"></a>Azure Portal gebruiken om de Attestation-URL te bepalen

1. Kopieer in het deel venster Overzicht voor uw Attestation-provider de waarde van de eigenschap Attestation URI naar het klem bord. Een Attestation-URI moet er als volgt uitzien: `https://MyAttestationProvider.us.attest.azure.net` .

2. Voeg het volgende toe aan de Attestation-URI: `/attest/SgxEnclave` . 

De resulterende Attestation-URL moet er als volgt uitzien: `https://MyAttestationProvider.us.attest.azure.net/attest/SgxEnclave`

## <a name="grant-your-azure-sql-logical-server-access-to-your-attestation-provider"></a>De logische Azure SQL-Server toegang verlenen tot uw Attestation-provider

Tijdens de Attestation-werk stroom roept de logische Azure SQL-Server met uw data base de Attestation-provider aan om een Attestation-aanvraag in te dienen. Voor de logische Azure SQL-Server om Attestation-aanvragen te kunnen indienen, moet de server een machtiging hebben voor de `Microsoft.Attestation/attestationProviders/attestation/read` actie op de Attestation-provider. De aanbevolen manier om de machtiging te verlenen is de beheerder van de Attestation-provider om de Azure AD-identiteit van de server toe te wijzen aan de rol van de Attestation-provider of de bijbehorende resource groep.

### <a name="use-azure-portal-to-assign-permission"></a>Azure Portal gebruiken om machtigingen toe te wijzen

Volg de algemene instructies in [Azure roles toewijzen met behulp van de Azure Portal](../../role-based-access-control/role-assignments-portal.md)om de identiteit van een Azure SQL-Server toe te wijzen aan de Attestation Reader-rol voor een Attestation-provider. Als u zich in het deel venster **toewijzing van rol toevoegen** bevindt:

1. Selecteer in de vervolg keuzelijst **rol** de rol **Attestation-lezer** .
1. Voer in het veld **selecteren** de naam van uw Azure SQL-Server in om ernaar te zoeken.

Zie de onderstaande scherm afbeelding voor een voor beeld.

![roltoewijzing van Attestation-lezer](./media/always-encrypted-enclaves/attestation-provider-role-assigment.png)

> [!NOTE]
> Een server kan alleen worden weer gegeven in het deel venster **toewijzing van rol toevoegen** als aan de server een Azure AD-identiteit is toegewezen. Zie [vereisten](#requirements).

### <a name="use-powershell-to-assign-permission"></a>Power shell gebruiken om machtigingen toe te wijzen

1. Zoek uw logische Azure SQL-Server.

```powershell
$serverResourceGroupName = "<server resource group name>"
$serverName = "<server name>" 
$server = Get-AzSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName 
```
 
2. Wijs de server toe aan de rol van de Attestation-lezer voor de resource groep met uw Attestation-provider.

```powershell
$attestationResourceGroupName = "<attestation provider resource group name>"
New-AzRoleAssignment -ObjectId $server.Identity.PrincipalId -RoleDefinitionName "Attestation Reader" -ResourceGroupName $attestationResourceGroupName
```

Zie [Azure-rollen toewijzen met behulp van Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md#assign-role-examples)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- [Manage keys for Always Encrypted with secure enclaves](/sql/relational-databases/security/encryption/always-encrypted-enclaves-manage-keys) (Sleutels beheren voor Always Encrypted met beveiligde enclaves)

## <a name="see-also"></a>Zie ook

- [Zelf studie: aan de slag met Always Encrypted met beveiligde enclaves in Azure SQL Database](always-encrypted-enclaves-getting-started.md)
