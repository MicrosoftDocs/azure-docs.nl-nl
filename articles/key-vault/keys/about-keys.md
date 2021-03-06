---
title: Over sleutels - Azure Key Vault
description: Overzicht van REST-interface van Azure Key Vault en detailinformatie over sleutels voor ontwikkelaars.
services: key-vault
author: amitbapat
manager: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: overview
ms.date: 02/17/2021
ms.author: ambapat
ms.openlocfilehash: b9565095a40052a940d7a7b31f0fd3a27e0e75c2
ms.sourcegitcommit: 260a2541e5e0e7327a445e1ee1be3ad20122b37e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/21/2021
ms.locfileid: "107814992"
---
# <a name="about-keys"></a>Over sleutels

Azure Key Vault biedt twee typen resources voor het opslaan en beheren van cryptografische sleutels. Kluizen bieden ondersteuning voor met software beveiligde en met HSM beveiligde sleutels (Hardware Security Module). Beheerde HSM's ondersteunen alleen met HSM beveiligde sleutels. 

|Resourcetype|Methoden voor sleutelbeveiliging|Basis-URL van het eindpunt van het gegevensvlak|
|--|--|--|
| **Kluizen** | Met software beveiligd<br/><br/>en<br/><br/>met HSM beveiligd (met Premium SKU)</li></ul> | https://{vault-name}.vault.azure.net |
| **Beheerde HMS's** | HSM-beveiligd | https://{hsm-name}.managedhsm.azure.net |
||||

- **Kluizen** - Kluizen bieden een goedkope, eenvoudig te implementeren, multi-tenant, zone-flexibele (indien beschikbaar), maximaal beschikbare oplossing voor sleutelbeheer die geschikt is voor de meeste veelvoorkomende scenario's voor cloudtoepassingen.
- **Beheerde HSM's** - Beheerde HSM biedt één tenant, zone-flexibele (indien beschikbaar), maximaal beschikbare HSM's voor het opslaan en beheren van uw cryptografische sleutels. Het meest geschikt voor toepassingen en gebruiksscenario's die hoogwaardige sleutels verwerken. Helpt u ook voldoen aan de strengste vereisten voor beveiliging, naleving en regelgeving. 

> [!NOTE]
> Met kluizen kunt u naast cryptografische sleutels ook verschillende typen objecten opslaan en beheren, zoals geheimen, certificaten en sleutels voor opslagaccounts.

Cryptografische sleutels in Key Vault worden weergegeven als JWK-objecten (JSON Web Key). De specificaties voor JavaScript Object Notation (JSON) en JavaScript Object Signing and Encryption (JOSE) zijn:

-   [JSON Web Key (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key)  
-   [JSON Web Encryption (JWE)](http://tools.ietf.org/html/draft-ietf-jose-json-web-encryption)  
-   [JSON Web Algorithms (JWA)](http://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms)  
-   [JSON Web Signature (JWS)](https://tools.ietf.org/html/draft-ietf-jose-json-web-signature) 

De basisspecificaties van JWK/JWA worden ook uitgebreid om sleuteltypen mogelijk te maken die uniek zijn voor de Azure Key Vault-implementatie en beheerde HSM-implementatie. 

Met HSM-beveiligde sleutels (ook wel HSM-sleutels genoemd) worden verwerkt in een HSM (Hardware Security Module) en behouden altijd de grens van de HSM-beveiliging. 

- Kluizen gebruiken **FIPS 140-2 Level 2** gevalideerde HSM's om HSM-sleutels in gedeelde HSM-backend-infrastructuur te beschermen. 
- Beheerde HSM maakt gebruik **van met FIPS 140-2 Level 3** gevalideerde HSM-modules om uw sleutels te beveiligen. Elke HSM-pool is een geïsoleerd exemplaar [](../managed-hsm/security-domain.md) met één tenant met een eigen beveiligingsdomein dat volledige cryptografische isolatie biedt van alle andere HSM's die dezelfde hardware-infrastructuur delen.

Deze sleutels worden beveiligd in HSM-groepen van één tenant. U een RSA-, EC- en symmetrische sleutel importeren, in zachte vorm of door te exporteren vanaf een ondersteund HSM-apparaat. U kunt ook sleutels genereren in HSM-pools. Wanneer u HSM-sleutels importeert met behulp van de methode die wordt beschreven in de specificatie [BYOK (uw eigen sleutel gebruiken)](../keys/byok-specification.md), wordt het sleutelmateriaal voor beveiligd transport in beheerde HSM-pools ingeschakeld. 

Zie [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/privacy/) voor meer informatie over geografische grenzen

## <a name="key-types-and-protection-methods"></a>Sleuteltypen en beveiligingsmethoden

Key Vault ondersteunt RSA- en EC-sleutels. Beheerde HSM ondersteunt RSA, EC en symmetrische sleutels. 

### <a name="hsm-protected-keys"></a>HSM-beveiligde sleutels

|Type sleutel|Kluizen (alleen premium SKU)|Beheerde HMS's|
|--|--|--|
|**EC-HSM**: Elliptic Curve-sleutel | Ondersteund | Ondersteund|
|**RSA-HSM**: RSA-sleutel|Ondersteund|Ondersteund|
|**oct-HSM:** Symmetrische sleutel|Niet ondersteund|Ondersteund|
|||

### <a name="software-protected-keys"></a>Met software beveiligde sleutels

|Type sleutel|Kluizen|Beheerde HMS's|
|--|--|--|
**RSA**: Met software beveiligde RSA-sleutel|Ondersteund|Niet ondersteund
**EC**: Met software beveiligde Elliptic Curve-sleutel|Ondersteund|Niet ondersteund
|||

### <a name="compliance"></a>Naleving

|Sleuteltype en doel|Naleving|
|---|---|
|Met software beveiligde sleutels in kluizen (Premium & Standard-SKU's) | FIPS 140-2 Level 1|
|Met HSM beveiligde sleutels in kluizen (Premium SKU)| FIPS 140-2 Level 2|
|Met HSM beveiligde sleutels in beheerde HSM|FIPS 140-2 Level 3|
|||



Zie [Sleuteltypen, algoritmen en bewerkingen](about-keys-details.md) voor meer informatie over elk sleuteltype, algoritmen, bewerkingen, kenmerken en tags.

## <a name="next-steps"></a>Volgende stappen
- [Informatie over Key Vault](../general/overview.md)
- [Informatie over beheerde HSM](../managed-hsm/overview.md)
- [Over geheimen](../secrets/about-secrets.md)
- [Over certificaten](../certificates/about-certificates.md)
- [Overzicht REST-API voor Key Vault](../general/about-keys-secrets-certificates.md)
- [Verificatie, aanvragen en antwoorden](../general/authentication-requests-and-responses.md)
- [Gids voor Key Vault-ontwikkelaars](../general/developers-guide.md)
