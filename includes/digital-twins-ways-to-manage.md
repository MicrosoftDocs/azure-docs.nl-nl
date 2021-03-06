---
author: baanders
description: include-bestand voor Azure Digital Apparaatdubbels-manieren om het exemplaar te beheren
ms.service: digital-twins
ms.topic: include
ms.date: 11/11/2020
ms.author: baanders
ms.openlocfilehash: 1f66101fc1231be2e5ce36dc348b1ca850113867
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/13/2021
ms.locfileid: "107303751"
---
In dit artikel wordt uitgelegd hoe u verschillende beheer bewerkingen kunt uitvoeren met behulp van de [Azure Digital apparaatdubbels .net (C#) **SDK**](/dotnet/api/overview/azure/digitaltwins/management). U kunt dezelfde beheer aanroepen ook maken met behulp van de andere taal-Sdk's die worden beschreven in [*How-to: gebruik de Azure Digital Apparaatdubbels api's en sdk's*](../articles/digital-twins/how-to-use-apis-sdks.md).

> [!TIP] 
> Houd er rekening mee dat alle SDK-methoden in synchrone en asynchrone versies worden geleverd. Voor wissel gesprekken retour neren de async-methoden `AsyncPageable<T>` wanneer de synchrone versies retour neren `Pageable<T>` .

Een andere beheer optie is om de Azure Digital Apparaatdubbels [**rest-api's**](/rest/api/azure-digitaltwins/) voor dit onderwerp rechtstreeks aan te roepen via een rest-client, zoals in het bericht. Zie [*How to: Requests with postman*](../articles/digital-twins/how-to-use-postman.md)voor instructies over hoe u dit doet.

Ten slotte kunt u dezelfde beheer bewerkingen uitvoeren met behulp van de Azure Digital Apparaatdubbels **cli**. Zie [*How-to: use the Azure Digital APPARAATDUBBELS cli*](../articles/digital-twins/how-to-use-cli.md)(Engelstalig) voor meer informatie over het gebruik van de cli.