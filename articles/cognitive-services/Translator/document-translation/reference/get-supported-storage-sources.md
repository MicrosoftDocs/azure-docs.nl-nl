---
title: Methode ondersteunde opslagbronnen op halen
titleSuffix: Azure Cognitive Services
description: De methode get supported storage sources retourneert een lijst met ondersteunde opslagbronnen.
services: cognitive-services
author: jann-skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 04/21/2021
ms.author: v-jansk
ms.openlocfilehash: 03203206da6ae3ea9a1174aebafda0b58e22ea41
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2021
ms.locfileid: "107864843"
---
# <a name="get-supported-storage-sources"></a>Ondersteunde opslagbronnen op halen

De methode Ondersteunde opslagbronnen op halen retourneert een lijst met opslagbronnen/-opties die worden ondersteund door de documentvertalingsservice.

## <a name="request-url"></a>Aanvraag-URL

Verzend een `GET` aanvraag naar:
```HTTP
GET https://<NAME-OF-YOUR-RESOURCE>.cognitiveservices.azure.com/translator/text/batch/v1.0-preview.1/storagesources
```

Meer informatie over het vinden [van uw aangepaste domeinnaam](../get-started-with-document-translation.md#find-your-custom-domain-name).

> [!IMPORTANT]
>
> * **Voor alle API-aanvragen voor de documentvertalingsservice is een aangepast domein-eindpunt vereist.**
> * U kunt het eindpunt op de pagina sleutels  en eindpunt van uw Azure Portal-resource, noch het globale translator-eindpunt, , gebruiken om HTTP-aanvragen voor documentvertaling `api.cognitive.microsofttranslator.com` te maken.

## <a name="request-headers"></a>Aanvraagheaders

Aanvraagheaders zijn:

|Kopteksten|Beschrijving|
|--- |--- |
|Ocp-Apim-Subscription-Key|Vereiste aanvraagheader|

## <a name="response-status-codes"></a>Antwoordstatuscodes

Hier volgen de mogelijke HTTP-statuscodes die een aanvraag retourneert.

|Statuscode|Description|
|--- |--- |
|200|OK. Geslaagde aanvraag en retourneert de lijst met opslagbronnen.|
|500|Interne serverfout.|
|Andere statuscodes|<ul><li>Te veel aanvragen</li><li>Server tijdelijk niet beschikbaar</li></ul>|

## <a name="get-supported-storage-sources-response"></a>Antwoord van ondersteunde opslagbronnen op halen

### <a name="successful-get-supported-storage-sources-response"></a>Geslaagd antwoord van ondersteunde opslagbronnen
Basistype voor lijst retourneren in de API Ondersteunde opslagbronnen op halen.

|Naam|Type|Description|
|--- |--- |--- |
|waarde|tekenreeks []|Lijst met objecten.|


### <a name="error-response"></a>Foutbericht

|Naam|Type|Description|
|--- |--- |--- |
|code|tekenreeks|Enums met foutcodes op hoog niveau. Mogelijke waarden:<br/><ul><li>InternalServerError</li><li>InvalidArgument</li><li>InvalidRequest</li><li>RequestRateTooHigh</li><li>ResourceNotFound</li><li>ServiceUnavailable</li><li>Niet geautoriseerd</li></ul>|
|message|tekenreeks|Haalt een foutbericht op hoog niveau op.|
|innerError|InnerErrorV2|Nieuwe interne foutindeling, die voldoet aan Cognitive Services API-richtlijnen. Het bevat de vereiste eigenschappen ErrorCode, bericht en optionele eigenschappen doel, details(sleutel-waardepaar), interne fout (dit kan worden genest).|
|innerError.code|tekenreeks|Haalt de codefoutreeks op.|
|innerError.message|tekenreeks|Haalt een foutbericht op hoog niveau op.|

## <a name="examples"></a>Voorbeelden

### <a name="example-successful-response"></a>Voorbeeld van geslaagd antwoord

Hier volgt een voorbeeld van een geslaagd antwoord.

```JSON
{
  "value": [
    "AzureBlob"
  ]
}
```

### <a name="example-error-response"></a>Voorbeeld van een foutbericht
Hier volgt een voorbeeld van een foutbericht. Het schema voor andere foutcodes is hetzelfde.

Statuscode: 500

```JSON
{
  "error": {
    "code": "InternalServerError",
    "message": "Internal Server Error",
    "innerError": {
      "code": "InternalServerError",
      "message": "Unexpected internal server error has occurred"
    }
  }
}
```

## <a name="next-steps"></a>Volgende stappen

Volg onze quickstart voor meer informatie over het gebruik van Documentvertaling en de clientbibliotheek.

> [!div class="nextstepaction"]
> [Aan de slag met documentvertaling](../get-started-with-document-translation.md)