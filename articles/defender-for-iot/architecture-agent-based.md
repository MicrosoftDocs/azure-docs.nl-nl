---
title: Wat is een oplossings architectuur op basis van een agent
description: Meer informatie over de architectuur en informatie stroom van Azure Defender voor IoT-agents.
ms.topic: overview
ms.date: 1/25/2021
ms.openlocfilehash: e08c3f151c3d3bc4fe08e61d960874b07f5b63e8
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2021
ms.locfileid: "106383521"
---
# <a name="what-is-agent-based-solution-for-device-builders"></a>Wat is oplossing op basis van een agent voor apparaten bouwers

In dit artikel wordt de functionele systeem architectuur van de Defender voor IoT-oplossing op basis van een agent beschreven. Azure Defender voor IoT biedt twee mogelijkheden voor het aanpassen van de behoeften van uw omgeving, de oplossing zonder agent voor organisaties en oplossingen op basis van agents voor apparaten.

## <a name="iot-hub-built-in-security"></a>Ingebouwde beveiliging van IoT hub

Defender voor IoT is standaard ingeschakeld in elke nieuwe IoT Hub die wordt gemaakt. Defender voor IoT biedt real-time bewaking, aanbevelingen en waarschuwingen, zonder dat de installatie van de agent op een apparaat hoeft te worden geïnstalleerd en maakt gebruik van geavanceerde analyses op geregistreerde IoT Hub meta gegevens om uw veld apparaten en IoT-hubs te analyseren en te beveiligen. 

## <a name="defender-for-iot-micro-agent"></a>Defender voor IoT micro agent 

Defender voor IoT micro agent biedt uitgebreide beveiligings beveiliging en inzicht in het gedrag van apparaten. Hiermee worden onbewerkte beveiligings gebeurtenissen van uw apparaten verzameld, geaggregeerd en geanalyseerd. Onbewerkte beveiligings gebeurtenissen kunnen IP-verbindingen, het maken van processen, gebruikers aanmeldingen en andere informatie over beveiliging zijn. Defender voor IoT-apparaat-agents verwerken ook gebeurtenissen aggregatie om te voor komen dat een hoge netwerk doorvoer. De agents zijn zeer aanpasbaar, zodat u ze kunt gebruiken voor specifieke taken, zoals het verzenden van belang rijke informatie op de snelste SLA, of voor het samen voegen van uitgebreide beveiligings informatie en-context in grotere segmenten, waardoor er hogere service kosten worden bespaard.

Apparaat-agents en andere toepassingen gebruiken de Azure-SDK voor het **verzenden van beveiligings berichten** om beveiligings gegevens te verzenden naar Azure IOT hub. IoT hub haalt deze informatie op en stuurt deze door naar de Defender voor IoT-service.

Zodra de Defender voor IoT-service is ingeschakeld, worden door IoT hub naast de doorgestuurde gegevens ook alle interne gegevens verzonden voor analyse door Defender voor IoT. Deze gegevens omvatten onder andere de bewerkingen voor het bewerkings logboek van de Cloud, apparaat-id's en de configuratie van de hub. Al deze informatie helpt u bij het maken van de Defender voor IoT Analytics-pijp lijn.

Defender voor IoT Analytics-pijp lijn ontvangt ook andere Threat Intelligence-streams van verschillende bronnen binnen micro soft-en micro soft-partners. De volledige analyse pijplijn van Defender voor IoT werkt met elke klant configuratie die is gemaakt voor de service (zoals aangepaste waarschuwingen en het gebruik van de SDK voor het verzenden van beveiligings berichten).

Met behulp van de analytische pijp lijn combineert Defender voor IoT alle gegevens stromen om aanbevelingen en waarschuwingen te genereren die actie kunnen ondernemen. De pijp lijn bevat zowel aangepaste regels die zijn gemaakt door veiligheids onderzoekers als experts, en machine learning modellen zoekt naar afwijkingen vanuit het standaard gedrag van apparaten en risico analyse.

Defender voor IoT-aanbevelingen en-waarschuwingen (analyse pijplijn uitvoer) worden geschreven naar de Log Analytics werk ruimte van elke klant. Met inbegrip van de onbewerkte gebeurtenissen in de werk ruimte en met de waarschuwingen en aanbevelingen kunnen grondige onderzoeken en query's worden uitgevoerd op basis van de exacte details van de gedetecteerde verdachte activiteiten.

:::image type="content" source="media/architecture/micro-agent-architecture.png" alt-text="De architectuur van micro agent.":::

## <a name="next-steps"></a>Volgende stappen

[Veelgestelde vragen over Defender voor IoT](resources-frequently-asked-questions.md)

[Systeemvereisten](quickstart-system-prerequisites.md)
