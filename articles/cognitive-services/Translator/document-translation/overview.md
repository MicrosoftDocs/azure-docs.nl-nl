---
title: Wat is document vertalingen?
description: Een overzicht van de service en het proces voor het vertalen van batch documenten in de Cloud.
ms.topic: overview
manager: nitinme
ms.author: lajanuar
author: laujan
ms.date: 02/11/2021
ms.openlocfilehash: 0d9ef13de29ac140d94e9e4c05b14f35b9e5834c
ms.sourcegitcommit: f5448fe5b24c67e24aea769e1ab438a465dfe037
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2021
ms.locfileid: "105968166"
---
# <a name="what-is-document-translation-preview"></a>Wat is document vertalingen (preview)?

Document vertalingen is een Cloud functie van de [Azure Translator](../translator-info-overview.md) -service en maakt deel uit van de Azure cognitieve service-familie van rest-api's. Met de API voor document vertalingen worden documenten geconverteerd naar en van 90 talen en dialecten, terwijl de document structuur en de gegevens indeling behouden blijven.

Deze documentatie bevat de volgende artikel typen:  

* [**Quick**](get-started-with-document-translation.md) starts zijn aan de slag-instructies die u helpen bij het maken van aanvragen voor de service.
* [**Hand leidingen**](create-sas-tokens.md) bevatten instructies voor het gebruik van de functie op meer specifieke of aangepaste manieren.  

## <a name="document-translation-key-features"></a>Sleutel functies voor document vertalingen

| Functie | Beschrijving |
| ---------| -------------|
| **Grote bestanden vertalen**| Vertaal hele documenten asynchroon.|
|**Een groot aantal bestanden vertalen**|Vertaal meerdere bestanden tot en met 90 talen en dialecten.|
|**Presentatie van bron bestand behouden**| Vertaal bestanden en behoud de oorspronkelijke indeling en notatie.|
|**Aangepaste vertaling Toep assen**| Vertaal documenten met behulp van algemene en [aangepaste Vertaal](../customization.md#custom-translator) modellen.|
|**Aangepaste Glossaries Toep assen**|Vertaal documenten met aangepaste Glossaries.|

## <a name="how-to-get-started"></a>Aan de slag

In onze hand leiding leert u hoe u snel aan de slag kunt gaan met behulp van document Translator. Als u wilt beginnen, hebt u een actief [Azure-account](https://azure.microsoft.com/free/cognitive-services/)nodig.  Als u er nog geen hebt, kunt u [een gratis account maken](https://azure.microsoft.com/free).

> [!div class="nextstepaction"]
> [Aan de slag](get-started-with-document-translation.md)

## <a name="supported-document-formats"></a>Ondersteunde documentindelingen

De volgende document bestands typen worden ondersteund door document vertalingen:

| Bestands type| Bestandsextensie|Description|
|---|---|--|
|Adobe PDF|.pdf|Adobe Acrobat Portable Document Format|
|HTML|.html|Taal van de Hyper tekst opmaak.|
|Lokalisatie van Interchange File-indeling|.xlf. , XLIFF| Een parallelle document indeling, export van Vertaal geheugen systemen. De gebruikte talen worden in het bestand gedefinieerd.|
|Microsoft Excel|.xlsx|Een werkblad bestand voor gegevens analyse en documentatie.|
|Microsoft Outlook|. msg|Een e-mail bericht dat is gemaakt of opgeslagen in micro soft Outlook.|
|Microsoft PowerPoint|.pptx| Een presentatie bestand dat wordt gebruikt om inhoud in een diapresentatie-indeling weer te geven.|
|Microsoft Word|.docx| Een tekst document bestand.|
|Door tabs gescheiden waarden/tabblad|. TSV/. tab| Een door tabs gescheiden bestand met ruwe gegevens dat wordt gebruikt door spreadsheetprogram ma's.|
|Tekst|.txt| Een niet-opgemaakt tekst document.|

## <a name="supported-glossary-formats"></a>Ondersteunde indelingen van woorden lijst

De volgende woordenlijst bestands typen worden ondersteund door document vertalingen:

| Bestands type| Bestandsextensie|Description|
|---|---|--|
|Lokalisatie van Interchange File-indeling|.xlf. , XLIFF| Een parallelle document indeling, export van Vertaal geheugen systemen. De gebruikte talen worden in het bestand gedefinieerd.|
|Door tabs gescheiden waarden/tabblad|. TSV/. tab| Een door tabs gescheiden bestand met ruwe gegevens dat wordt gebruikt door spreadsheetprogram ma's.|

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aan de slag met document vertalingen](get-started-with-document-translation.md)
