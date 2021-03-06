---
title: JavaScript-voorbeelden
titleSuffix: Azure Cognitive Search
description: Zoek Azure Cognitive Search Demo Java script-code voorbeelden die gebruikmaken van de Azure .NET SDK voor Java script.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/27/2021
ms.openlocfilehash: 85a4d6390087100d8d9521f6ac20dbace3a711eb
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2021
ms.locfileid: "104955938"
---
# <a name="javascript-code-samples-for-azure-cognitive-search"></a>Java script-code voorbeelden voor Azure Cognitive Search

Meer informatie over de Java script-code voorbeelden die de functionaliteit en werk stroom van een Azure Cognitive Search-oplossing demonstreren. In deze voor beelden wordt de [**azure Cognitive Search-client bibliotheek**](/javascript/api/overview/azure/search-documents-readme) voor de [**Azure SDK voor Java script**](/azure/developer/javascript/)gebruikt, die u via de volgende koppelingen kunt verkennen.

| Doel | Koppeling |
|--------|------|
| Pakket downloaden | [www.npmjs.com/package/@azure/search-documents](https://www.npmjs.com/package/@azure/search-documents) |
| API-verwijzing | [@azure/search-documents](/javascript/api/@azure/search-documents/)  |
| API-test cases | [github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/test](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/test) |
| Broncode | [github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents)  |

## <a name="sdk-samples"></a>SDK steekproeven

Code voorbeelden uit het ontwikkel team van Azure SDK illustreren het gebruik van de API. U kunt deze voor beelden vinden in [**Azure-SDK-voor-JS/tree/master/SDK/Search/Search-documenten/voor beelden**](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples) op github.

### <a name="javascript-sdk-samples"></a>Java script SDK-voor beelden

| Voorbeelden | Description |
|---------|-------------|
| [indexen](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/javascript/src/indexes) | Laat zien hoe [zoek indexen](search-what-is-an-index.md)maken, bijwerken, ophalen, weer geven en verwijderen. Deze voorbeeld categorie bevat ook een voor beeld van een service statistiek. |
| [dataSourceConnections (voor Indexeer functies)](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/javascript/src/dataSourceConnections) | Demonstreert hoe u Indexeer functie gegevens bronnen kunt maken, bijwerken, ophalen, weer geven en verwijderen. Dit is vereist voor indexering op basis van een indexer van [ondersteunde Azure-gegevens bronnen](search-indexer-overview.md#supported-data-sources). |
| [Indexeer functies](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/javascript/src/indexers) |  Demonstreert hoe [Indexeer functies](search-indexer-overview.md)maken, bijwerken, ophalen, weer geven, opnieuw instellen en verwijderen.|
| [Vaardig heden](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/javascript/src/skillSets) |   Demonstreert hoe u [vaardig heden](cognitive-search-working-with-skillsets.md) kunt maken, bijwerken, ophalen, weer geven en verwijderen die zijn gekoppeld aan Indexeer functies en die op AI gebaseerde verrijking uitvoeren tijdens het indexeren. |
| [synonymMaps](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/javascript/src/synonymMaps) | Laat zien hoe u [synoniemen kaarten](search-synonyms.md)kunt maken, bijwerken, ophalen, weer geven en verwijderen.  |

### <a name="typescript-samples"></a>TypeScript-voorbeelden

| Voorbeelden | Description |
|---------|-------------|
| [indexen](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/typescript/src/indexes) | Laat zien hoe [zoek indexen](search-what-is-an-index.md)maken, bijwerken, ophalen, weer geven en verwijderen. Deze voorbeeld categorie bevat ook een voor beeld van een service statistiek. |
| [dataSourceConnections (voor Indexeer functies)](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/typescript/src/dataSourceConnections) | Demonstreert hoe u Indexeer functie gegevens bronnen kunt maken, bijwerken, ophalen, weer geven en verwijderen. Dit is vereist voor indexering op basis van een indexer van [ondersteunde Azure-gegevens bronnen](search-indexer-overview.md#supported-data-sources). |
| [Indexeer functies](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/typescript/src/indexers) |  Demonstreert hoe [Indexeer functies](search-indexer-overview.md)maken, bijwerken, ophalen, weer geven, opnieuw instellen en verwijderen.|
| [Vaardig heden](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/typescript/src/skillSets) |   Demonstreert hoe u [vaardig heden](cognitive-search-working-with-skillsets.md) kunt maken, bijwerken, ophalen, weer geven en verwijderen die zijn gekoppeld aan Indexeer functies en die op AI gebaseerde verrijking uitvoeren tijdens het indexeren. |
| [synonymMaps](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/typescript/src/synonymMaps) | Laat zien hoe u [synoniemen kaarten](search-synonyms.md)kunt maken, bijwerken, ophalen, weer geven en verwijderen.  |

## <a name="doc-samples"></a>Doc-voor beelden

Code voorbeelden van het Cognitive Search team illustreren de functies en werk stromen. In veel van deze voor beelden wordt verwezen naar zelf studies, Quick starts en artikelen met procedures. U kunt deze voor beelden vinden in [**Azure-samples/Azure-Search-java script-voor beelden**](https://github.com/Azure-Samples/azure-search-javascript-samples) op github.

| Voorbeelden | Artikel |
|---------|---------|
| [Snelstartgids](https://github.com/Azure-Samples/azure-search-javascript-samples/tree/master/quickstart/v11) | Bron code voor [Quick Start: een zoek index maken in Java script](search-get-started-javascript.md). In dit artikel wordt de basis werk stroom beschreven voor het maken, laden en doorzoeken van een zoek index met voorbeeld gegevens. |

> [!Tip]
> Gebruik de voor beelden van de [browser](/samples/browse/?languages=javascript&products=azure-cognitive-search) om te zoeken naar micro soft-code voorbeelden in github, gefilterd op product, service en taal.

## <a name="other-samples"></a>Andere voor beelden

De volgende voor beelden worden ook gepubliceerd door het Cognitive Search-team, maar er wordt niet naar verwezen in de documentatie. Gekoppelde Leesmij-bestanden bieden gebruiks instructies.

| Voorbeelden | Description |
|---------|-------------|
| [Azure-zoeken-reageren-sjabloon](https://github.com/dereklegenzoff/azure-search-react-template) | Sjabloon reageren voor Azure Cognitive Search (github.com) |