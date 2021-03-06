---
title: Informatie over het bijwerken van het apparaat voor IoT Hub importeren | Microsoft Docs
description: Belangrijkste concepten voor het importeren van een nieuwe update in de update van het apparaat voor IoT Hub.
author: andrewbrownmsft
ms.author: andbrown
ms.date: 2/10/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: 4cd5e0c016b98a3dc9336237a5c1b14e6b0f5789
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2021
ms.locfileid: "102040580"
---
# <a name="importing-updates-into-device-update-for-iot-hub"></a>Updates importeren in update voor het apparaat voor IoT Hub
Als u een update wilt implementeren op apparaten die moeten worden bijgewerkt voor IoT Hub, moet u eerst die update _importeren_ in de service voor het bijwerken van het apparaat. Hier volgt een overzicht van een aantal belang rijke concepten die u moet begrijpen wanneer het gaat om het importeren van updates.

## <a name="import-manifest"></a>Manifest importeren

Een import manifest is een JSON-bestand dat belang rijke informatie definieert over de update die u wilt importeren. Als onderdeel van het import proces verzendt u zowel het import manifest als de bijbehorende update bestanden (zoals een firmware-update pakket). De meta gegevens die in het import manifest zijn gedefinieerd, worden gebruikt om de update op te nemen. Sommige meta gegevens worden ook gebruikt tijdens de implementatie, bijvoorbeeld om te controleren of een update correct is geïnstalleerd.

Het import manifest bevat verschillende items die belang rijke update van het apparaat vertegenwoordigen voor IoT Hub concepten. Deze concepten worden hieronder beschreven.

### <a name="update-identity-update-id"></a>Update-identiteit (update-ID)

De update-identiteit vertegenwoordigt de unieke id van een update. Het definieert belang rijke eigenschappen over een update die wordt geïmporteerd. De update-identiteit bestaat uit drie delen:
* Provider: dit is de entiteit die verantwoordelijk is voor de update. Het is vaak een bedrijfs naam.
* Naam: een id voor een klasse van updates. De klasse kan alles zijn wat u kiest. Het is vaak een apparaat-of model naam.
* Versie: dit is een versie nummer waarmee deze update wordt onderscheiden van anderen die dezelfde provider en naam hebben. Deze versie wordt gebruikt door de update voor IoT Hub service van het apparaat en kan al dan niet overeenkomen met een versie van een afzonderlijk software onderdeel op het apparaat. 

### <a name="compatibility"></a>Compatibiliteit

Voor het vereenvoudigen van update-implementaties, vergelijkt de update van het apparaat voor IoT Hub compatibiliteits eigenschappen voor een update, die zijn gedefinieerd in het import manifest, met bijbehorende eigenschappen van het apparaat. Alleen updates met overeenkomende eigenschappen zullen worden geretourneerd en beschikbaar zijn voor implementatie.

### <a name="installedcriteria"></a>InstalledCriteria

De InstalledCriteria wordt door de Update Agent op een apparaat gebruikt om te bepalen of een update is geïnstalleerd.


## <a name="next-steps"></a>Volgende stappen

Als u klaar bent, kunt u de [hand leiding voor het importeren How-To](./import-update.md)uitproberen. hier vindt u stapsgewijze instructies voor het importeren van het proces.


