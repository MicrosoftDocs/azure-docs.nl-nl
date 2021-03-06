---
author: PatrickFarley
ms.author: pafarley
ms.service: cognitive-services
ms.date: 10/25/2020
ms.openlocfilehash: d73fef916c2652f1fa4b98fd84173c1ec0abb263
ms.sourcegitcommit: 6f1aa680588f5db41ed7fc78c934452d468ddb84
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2021
ms.locfileid: "107725146"
---
Aan de slag met de Custom Vision-clientbibliotheek voor Python. Volg deze stappen om het pakket te installeren en de voorbeeldcode voor het bouwen van een objectdetectiemodel uit te proberen. U maakt een project, voegt tags toe, traint het project en gebruikt de voorspellingseindpunt-URL van het project om het programmatisch te testen. Gebruik dit voorbeeld als een sjabloon om uw eigen beeldherkennings-app te maken.

> [!NOTE]
> Als u een objectdetectiemodel wilt bouwen en trainen _zonder_ code te schrijven, raadpleegt u de [handleiding voor browsers](../../get-started-build-detector.md).

Gebruik de Custom Vision-clientbibliotheek voor Python voor het volgende:

* Een nieuw project maken in de Custom Vision-service
* Label aan het project toevoegen
* Afbeeldingen uploaden en labelen
* Het project trainen
* De huidige herhaling publiceren
* Voorspellingseindpunt testen

[Referentiedocumentatie](/python/api/overview/azure/cognitiveservices/customvision) | [Broncode bibliotheek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-vision-customvision/azure/cognitiveservices/vision/customvision) | [Package (PyPI)](https://pypi.org/project/azure-cognitiveservices-vision-customvision/) | [Voorbeelden](/samples/browse/?languages=python&products=azure&term=vision&terms=vision)

## <a name="prerequisites"></a>Vereisten

* Azure-abonnement: [Krijg een gratis abonnement](https://azure.microsoft.com/free/cognitive-services/)
* [Python 3.x](https://www.python.org/)
  * Uw Python-installatie moet [pip bevatten.](https://pip.pypa.io/en/stable/) U kunt controleren of pip is geïnstalleerd door uit te voeren `pip --version` op de opdrachtregel. Haal pip op door de nieuwste versie van Python te installeren.
* Zodra u een Azure-abonnement hebt, maakt u een Custom Vision-resource en maakt u een Custom Vision-resource in de Azure Portal om een trainings- en voorspellingsresource te maken en uw sleutels en eindpunt op te <a href="https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision"  title=" "  target="_blank"> </a> halen. Wacht tot deze is geïmplementeerd en klik op de knop **Naar de resource gaan**.
    * U hebt de sleutel en het eindpunt nodig van de resource die u maakt, om de toepassing te verbinden met Custom Vision. Later in de quickstart plakt u uw sleutels en eindpunt in de onderstaande code.
    * U kunt de gratis prijscategorie (`F0`) gebruiken om de service uit te proberen, en later upgraden naar een betaalde laag voor productie.

## <a name="setting-up"></a>Instellen

### <a name="install-the-client-library"></a>De clientbibliotheek installeren

Als u een beeldanalyse-app wilt schrijven met Custom Vision voor Python, hebt u de Custom Vision-clientbibliotheek nodig. Nadat u Python hebt geïnstalleerd, voert u de volgende opdracht uit in PowerShell of een consolevenster:

```powershell
pip install azure-cognitiveservices-vision-customvision
```

### <a name="create-a-new-python-application"></a>Een nieuwe Python-toepassing maken

Maak een nieuw Python-bestand en importeer de volgende bibliotheken.

[!code-python[](~/cognitive-services-quickstart-code/python/CustomVision/ObjectDetection/CustomVisionQuickstart.py?name=snippet_imports)]

> [!TIP]
> Wilt u het codebestand voor de quickstart in één keer weergeven? Die is te vinden op [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/CustomVision/ObjectDetection/CustomVisionQuickstart.py), waar de codevoorbeelden uit deze quickstart zich bevinden.

Maak variabelen voor het Azure-eindpunt en de abonnementssleutels van uw resource.

[!code-python[](~/cognitive-services-quickstart-code/python/CustomVision/ObjectDetection/CustomVisionQuickstart.py?name=snippet_creds)]


> [!IMPORTANT]
> Ga naar Azure Portal. Als de Custom Vision-resources die u in de sectie **Vereisten** hebt gemaakt, zijn geïmplementeerd, klikt u onder **Volgende stappen** op de knop **Ga naar resource**. U vindt de sleutels en het eindpunt op de pagina's over **sleutel en eindpunt** van de resources, onder **Resourcebeheer**. U moet de sleutels voor zowel uw trainings- als voorspellingsresources ophalen, samen met het API-eindpunt voor uw trainingsresource.
>
> U kunt de resource-id van de voorspelling vinden op het tabblad **Overzicht**, vermeld als **Abonnements-id**.
>
> Vergeet niet de sleutels uit uw code te verwijderen wanneer u klaar bent, en maak deze sleutels nooit openbaar. Overweeg om voor productie een veilige manier te gebruiken voor het opslaan en openen van uw referenties. Zie het artikel over [Cognitive Services-beveiliging voor meer](../../../cognitive-services-security.md) informatie.

## <a name="object-model"></a>Objectmodel

|Naam|Beschrijving|
|---|---|
|[CustomVisionTrainingClient](/python/api/azure-cognitiveservices-vision-customvision/azure.cognitiveservices.vision.customvision.training.customvisiontrainingclient) | Deze klasse behandelt het maken, trainen en publiceren van uw modellen. |
|[CustomVisionPredictionClient](/python/api/azure-cognitiveservices-vision-customvision/azure.cognitiveservices.vision.customvision.prediction.customvisionpredictionclient)| Deze klasse verwerkt de query op uw modellen voor objectdetectievoorspellingen.|
|[ImagePrediction](/python/api/azure-cognitiveservices-vision-customvision/azure.cognitiveservices.vision.customvision.prediction.models.imageprediction)| Deze klasse definieert een voorspelling van één object op één afbeelding. Het bevat eigenschappen voor de object-id en -naam, de locatie van het begrenzingsvak van het object en een betrouwbaarheidsscore.|

## <a name="code-examples"></a>Codevoorbeelden

Deze codefragmenten laten zien hoe u het volgende kunt uitvoeren met de Custom Vision-clientbibliotheek voor Python:

* [De client verifiëren](#authenticate-the-client)
* [Een nieuw project maken in de Custom Vision-service](#create-a-new-custom-vision-project)
* [Label aan het project toevoegen](#add-tags-to-the-project)
* [Afbeeldingen uploaden en labelen](#upload-and-tag-images)
* [Het project trainen](#train-the-project)
* [De huidige herhaling publiceren](#publish-the-current-iteration)
* [Voorspellingseindpunt testen](#test-the-prediction-endpoint)

## <a name="authenticate-the-client"></a>De client verifiëren

Instantieer een exemplaar van een trainings- en voorspellingsclient met uw eindpunt en sleutels. Maak **ApiKeyServiceClientCredentials**-objecten met uw sleutels en gebruik deze met uw eindpunt om een [CustomVisionTrainingClient](/python/api/azure-cognitiveservices-vision-customvision/azure.cognitiveservices.vision.customvision.training.customvisiontrainingclient)- en [CustomVisionPredictionClient](/python/api/azure-cognitiveservices-vision-customvision/azure.cognitiveservices.vision.customvision.prediction.customvisionpredictionclient)-object te maken.

[!code-python[](~/cognitive-services-quickstart-code/python/CustomVision/ObjectDetection/CustomVisionQuickstart.py?name=snippet_auth)]


## <a name="create-a-new-custom-vision-project"></a>Een nieuw project maken in de Custom Vision-service

Als u een nieuw Custom Vision Service-project wilt maken, voegt u de volgende code aan uw script toe. 

Raadpleeg de [create_project](/python/api/azure-cognitiveservices-vision-customvision/azure.cognitiveservices.vision.customvision.training.operations.customvisiontrainingclientoperationsmixin#create-project-name--description-none--domain-id-none--classification-type-none--target-export-platforms-none--custom-headers-none--raw-false----operation-config-&preserve-view=true)-methode om andere opties op te geven wanneer u uw project maakt (uitgelegd in de webportalgids [Een detector maken](../../get-started-build-detector.md)).  

[!code-python[](~/cognitive-services-quickstart-code/python/CustomVision/ObjectDetection/CustomVisionQuickstart.py?name=snippet_create)]


## <a name="add-tags-to-the-project"></a>Label aan het project toevoegen

Voeg de volgende code toe aan uw project om objecttags in uw project te maken:

[!code-python[](~/cognitive-services-quickstart-code/python/CustomVision/ObjectDetection/CustomVisionQuickstart.py?name=snippet_tags)]


## <a name="upload-and-tag-images"></a>Afbeeldingen uploaden en labelen

Download eerst de voorbeeldafbeeldingen voor dit project. Sla de inhoud van de [map Voorbeeldafbeeldingen](https://github.com/Azure-Samples/cognitive-services-sample-data-files/tree/master/CustomVision/ObjectDetection/Images) op uw lokale apparaat op.

> [!NOTE]
> Hebt u een bredere set afbeeldingen nodig om uw training te voltooien? Met Trove, een Microsoft Garage-project, kunt u sets afbeeldingen verzamelen en aanschaffen voor trainingsdoeleinden. Wanneer u uw afbeeldingen hebt verzameld kunt u ze downloaden en importeren naar uw Custom Vision-project. Ga naar de pagina [Trove](https://www.microsoft.com/ai/trove?activetab=pivot1:primaryr3) voor meer informatie.

Als u afbeeldingen labelt in objectdetectieprojecten, dient u de regio van elk gelabeld object op te geven met behulp van genormaliseerde coördinaten. Met de volgende code wordt elk voorbeeld van een afbeelding aan de bijbehorende gelabelde regio gekoppeld. Door de regio's wordt het begrenzingsvak opgegeven in genormaliseerde coördinaten. De coördinaten worden gegeven in de volgorde links, boven, breedte, hoogte.

[!code-python[](~/cognitive-services-quickstart-code/python/CustomVision/ObjectDetection/CustomVisionQuickstart.py?name=snippet_tagging)]

> [!NOTE]
> Als u geen hulpprogramma voor klikken en slepen hebt om de coördinaten van regio's te markeren, kunt u de Web-UI op [Customvision.ai](https://www.customvision.ai/) gebruiken. In dit voorbeeld zijn de coördinaten al opgenomen.

Gebruik vervolgens deze kaart met koppelingen om elke voorbeeldafbeelding met de bijbehorende regiocoördinaten te uploaden (u kunt maximaal 64 afbeeldingen in één batch uploaden). Voeg de volgende code toe.


[!code-python[](~/cognitive-services-quickstart-code/python/CustomVision/ObjectDetection/CustomVisionQuickstart.py?name=snippet_upload)]

> [!NOTE]
> U moet het pad naar de afbeeldingen wijzigen, gebaseerd op waar u de opslagplaats Cognitive Services Python-SDK-voorbeelden eerder hebt gedownload.

## <a name="train-the-project"></a>Het project trainen

Met deze code wordt de eerste iteratie van het voorspellingsmodel gemaakt. 

[!code-python[](~/cognitive-services-quickstart-code/python/CustomVision/ObjectDetection/CustomVisionQuickstart.py?name=snippet_train)]

> [!TIP]
> Trainen met geselecteerde tags
>
> Indien gewenst kun u alleen trainen op een subset van de toegepaste tags. U kunt dit doen als u bepaalde tags nog niet vaak genoeg hebt toegepast, maar u wel voldoende andere codes hebt toegepast. Stel in de **[train_project](/python/api/azure-cognitiveservices-vision-customvision/azure.cognitiveservices.vision.customvision.training.operations.customvisiontrainingclientoperationsmixin#train-project-project-id--training-type-none--reserved-budget-in-hours-0--force-train-false--notification-email-address-none--selected-tags-none--custom-headers-none--raw-false----operation-config-&preserve-view=true)** -aanroep de optionele parameter *selected_tags* in op een lijst met de ID-tekenreeksen van de tags die u wilt gebruiken. Het model wordt getraind om alleen de tags in de lijst te herkennen.

## <a name="publish-the-current-iteration"></a>De huidige herhaling publiceren

Er is pas na publicatie een iteratie beschikbaar in het voorspellingseindpunt. Met de volgende code wordt de huidige iteratie van het model beschikbaar gemaakt voor het uitvoeren van query's. 

[!code-python[](~/cognitive-services-quickstart-code/python/CustomVision/ObjectDetection/CustomVisionQuickstart.py?name=snippet_publish)]

## <a name="test-the-prediction-endpoint"></a>Voorspellingseindpunt testen

Als u een afbeelding naar het voorspellingseindpunt wilt verzenden en de voorspelling wilt ophalen, voegt u de volgende code toe aan het einde van het bestand:

[!code-python[](~/cognitive-services-quickstart-code/python/CustomVision/ObjectDetection/CustomVisionQuickstart.py?name=snippet_test)]


## <a name="run-the-application"></a>De toepassing uitvoeren

Voer *CustomVisionQuickstart.py* uit.

```powershell
python CustomVisionQuickstart.py
```

De uitvoer van de toepassing moet in de console worden weergegeven. Vervolgens kunt u controleren of de testafbeelding (gevonden in **<base_image_location>/images/Test**) op de juiste wijze wordt gelabeld en of de detectieregio juist is. U kunt altijd teruggaan naar de [Custom Vision-website](https://customvision.ai) en de huidige status bekijken van het nieuwe project dat u hebt gemaakt.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [clean-od-project](../../includes/clean-od-project.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt nu elke stap van het objectdetectieproces in code uitgevoerd. Met dit voorbeeld wordt één trainingsiteratie uitgevoerd, maar vaak zult u uw model meerdere keren willen trainen en testen om het nauwkeuriger te maken. In de volgende handleiding wordt classificatie van afbeeldingen behandeld. De principes zijn soortgelijk aan die van objectdetectie.

> [!div class="nextstepaction"]
> [Een model testen en opnieuw trainen](../../test-your-model.md)

* Wat is Custom Vision?
* De broncode voor dit voorbeeld is te vinden [op GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/CustomVision/ObjectDetection/CustomVisionQuickstart.py)
* [SDK-naslagdocumentatie](/python/api/overview/azure/cognitiveservices/customvision)
