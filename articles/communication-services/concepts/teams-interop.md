---
title: Interoperabiliteit van Teams-vergadering
titleSuffix: An Azure Communication Services concept document
description: Deelnemen aan Teams-vergaderingen
author: chpalm
manager: chpalm
services: azure-communication-services
ms.author: chpalm
ms.date: 03/10/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: cf6553cd7c59febd19f9654e31188f127b8eb065
ms.sourcegitcommit: 02bc06155692213ef031f049f5dcf4c418e9f509
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2021
ms.locfileid: "106276753"
---
# <a name="teams-interoperability"></a>Interoperabiliteit met Teams

[!INCLUDE [Public Preview](../includes/public-preview-include-document.md)]

> [!IMPORTANT]
> Vul [dit formulier](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR21ouQM6BHtHiripswZoZsdURDQ5SUNQTElKR0VZU0VUU1hMOTBBMVhESS4u)in om [teams Tenant interoperabiliteit](../concepts/teams-interop.md)in of uit te scha kelen.

Azure Communication Services kan worden gebruikt voor het bouwen van aangepaste vergaderervaringen die communiceren met Microsoft Teams. Gebruikers van uw oplossingen voor communicatie Services kunnen communiceren met teams deel nemers over spraak, video, chatten en het delen van het scherm.

Met de interoperabiliteit van teams kunt u aangepaste toepassingen maken waarmee gebruikers verbinding maken met teams vergaderingen. Gebruikers van uw aangepaste toepassingen hoeven geen Azure Active Directory-identiteiten of Teams-licenties te hebben om deze functie te kunnen ervaren. Dit is ideaal voor het samenbrengen van werknemers (die mogelijk bekend zijn met Teams) en externe gebruikers (met behulp van een aangepaste toepassingservaring) in een naadloze vergaderervaring. Bijvoorbeeld:

1. Werknemers gebruiken Teams om een vergadering te plannen 
1. Vergaderings gegevens worden gedeeld met externe gebruikers via uw aangepaste toepassing.
   * **Graph API gebruiken** Uw aangepaste communicatie Services-toepassing maakt gebruik van de Microsoft Graph-Api's om toegang te krijgen tot de vergaderings gegevens die moeten worden gedeeld. 
   * **Andere opties gebruiken** Uw Vergader koppeling kan bijvoorbeeld worden gekopieerd uit uw agenda in micro soft teams.
1. Externe gebruikers gebruiken uw aangepaste toepassing om deel te nemen aan de team vergadering (via de communicatie Services en de chat-Sdk's)

De architectuur op hoog niveau voor deze gebruikers-case ziet er als volgt uit: 

![Architectuur voor Teams-interop](./media/call-flows/teams-interop.png)

Hoewel bepaalde teams functies als verhoogde hand, samen modus en groepen alleen beschikbaar zijn voor teams gebruikers, heeft uw aangepaste toepassing toegang tot de kern functionaliteit van audio, video, chatten en scherm delen van de vergadering. De chat functie voor vergaderingen is toegankelijk voor uw aangepaste toepassings gebruiker terwijl deze wordt aangeroepen. Het is niet mogelijk om berichten te verzenden of te ontvangen voordat u deelneemt of nadat u de oproep hebt verlaten. 

Wanneer een communicatie Services-gebruiker deelneemt aan de vergadering van de teams, wordt de weergave naam die via de aanroepende SDK is verschaft, weer gegeven voor teams gebruikers. De Communication Services-gebruiker wordt anders behandeld als een anonieme gebruiker in Teams.  Uw aangepaste toepassing moet rekening houden met gebruikersverificatie en andere veiligheidsmaatregelen om Teams vergaderingen te beveiligen. Houd rekening met de beveiligingsimplicaties van het deel laten nemen van anonieme gebruikers aan vergaderingen, en gebruik de [Beveiligingshandleiding van Teams](/microsoftteams/teams-security-guide#addressing-threats-to-teams-meetings) om de mogelijkheden te configureren die beschikbaar zijn voor anonieme gebruikers.

Communication Services teams Interop bevindt zich momenteel in een persoonlijke preview. Wanneer het algemeen beschikbaar is, worden communicatie Services-gebruikers behandeld als gebruikers van externe toegang. Meer informatie over externe toegang in [gesprek, chat en samen werking met mensen buiten uw organisatie in micro soft teams](/microsoftteams/communicate-with-users-from-other-organizations).

Gebruikers van Communication Services kunnen deelnemen aan geplande Teams-vergaderingen, zolang anonieme deelnames zijn ingeschakeld in de [instellingen van vergaderingen](/microsoftteams/meeting-settings-in-teams). Als de vergadering is gepland voor een kanaal, kunnen communicatie Services gebruikers niet deel nemen aan de chat sessie of berichten verzenden en ontvangen.

## <a name="teams-in-government-clouds-gcc"></a>Teams in Government Clouds (GCC)
Azure Communication Services-interoperabiliteit is op dit moment niet compatibel met teams implementaties met behulp van [Microsoft 365 Government Clouds (gcc)](/MicrosoftTeams/plan-for-government-gcc) . 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Voeg u aanroepende app toe aan een Teams-meeting](../quickstarts/voice-video-calling/get-started-teams-interop.md)
