---
title: 'Quickstart: Text Analytics-clientbibliotheek voor C# | Microsoft Docs'
description: Aan de slag met de Text Analytics-clientbibliotheek voor C#
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 04/19/2021
ms.author: aahi
ms.reviewer: assafi
ms.openlocfilehash: 1fd102f0f94f1ce53bebfba94d4f4c1a1f9e3812
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/20/2021
ms.locfileid: "107765090"
---
<a name="HOLTop"></a>

# <a name="version-31-preview"></a>[Versie 3.1: preview](#tab/version-3-1)

[v3.1-referentiedocumentatie](/dotnet/api/azure.ai.textanalytics?preserve-view=true&view=azure-dotnet-preview) | [Broncode voor v3.1-bibliotheek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics) | [v3.1-pakket (NuGet)](https://www.nuget.org/packages/Azure.AI.TextAnalytics/5.1.0-beta.5) | [v3.1-voorbeelden](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics/samples)

# <a name="version-30"></a>[Versie 3.0](#tab/version-3)

[v3-referentiedocumentatie](/dotnet/api/azure.ai.textanalytics) | [Broncode voor v3-bibliotheek](https://github.com/Azure/azure-sdk-for-net/tree/Azure.AI.TextAnalytics_5.0.0/sdk/textanalytics/Azure.AI.TextAnalytics) | [v3-pakket (NuGet)](https://www.nuget.org/packages/Azure.AI.TextAnalytics) | [v3-voorbeelden](https://github.com/Azure/azure-sdk-for-net/tree/Azure.AI.TextAnalytics_5.0.0/sdk/textanalytics/Azure.AI.TextAnalytics/samples)

---

## <a name="prerequisites"></a>Vereisten

* Azure-abonnement: [Krijg een gratis abonnement](https://azure.microsoft.com/free/cognitive-services)
* De [Visual Studio IDE](https://visualstudio.microsoft.com/vs/)
* Zodra u een Azure-abonnement hebt, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="Een Text Analytics-resource maken"  target="_blank">maakt u een Text Analytics-resource </a> in de Azure-portal om uw sleutel en eindpunt op te halen.  Nadat de app is geïmplementeerd, klikt u op **Ga naar resource**.
    * U hebt de sleutel en het eindpunt nodig van de resource die u maakt, om de toepassing te verbinden met de Text Analytics-API. Later in de quickstart plakt u uw sleutel en eindpunt in de onderstaande code.
    * U kunt de gratis prijscategorie (`F0`) gebruiken om de service uit te proberen, en later upgraden naar een betaalde laag voor productie.
* Als u de functie Analyseren wilt gebruiken, hebt u een Text Analytics-resource in de prijscategorie Standard (S) nodig.

## <a name="setting-up"></a>Instellen

### <a name="create-a-new-net-core-application"></a>Een nieuwe .NET Core-app maken

Maak een nieuwe console-app in .NET Core met behulp van de Visual Studio IDE. Hiermee wordt een Hello World-project gemaakt met één bronbestand van C#: *program.cs*.

# <a name="version-31-preview"></a>[Versie 3.1: preview](#tab/version-3-1)

Installeer de clientbibliotheek door met de rechtermuisknop op de oplossing te klikken in **Solution Explorer** en **NuGet-pakketten beheren** te selecteren. Selecteer in Package Manager dat wordt geopend de optie **Bladeren** en zoek naar `Azure.AI.TextAnalytics`. Schakel het vakje **prerelease toevoegen** in, selecteer versie `5.1.0-beta.5` en selecteer **Installeren**. U kunt ook de [Package Manager-console](/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package) gebruiken.

# <a name="version-30"></a>[Versie 3.0](#tab/version-3)

Installeer de clientbibliotheek door met de rechtermuisknop op de oplossing te klikken in **Solution Explorer** en **NuGet-pakketten beheren** te selecteren. Selecteer in Package Manager dat wordt geopend de optie **Bladeren** en zoek naar `Azure.AI.TextAnalytics`. Selecteer versie `5.0.0` en vervolgens **Installeren**. U kunt ook de [Package Manager-console](/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package) gebruiken.


> [!TIP]
> Wilt u het codebestand voor de quickstart in één keer weergeven? U kunt het vinden [op GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/TextAnalytics/program.cs), dat de codevoorbeelden in deze quickstart bevat. 

---

# <a name="version-31-preview"></a>[Versie 3.1: preview](#tab/version-3-1)

Open het bestand *program.cs* en voeg de volgende `using`-instructies toe:

```csharp
using Azure;
using System;
using System.Globalization;
using Azure.AI.TextAnalytics;
```

Maak in de klasse `Program` van de toepassing variabelen voor de sleutel en het eindpunt van uw resource.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly AzureKeyCredential credentials = new AzureKeyCredential("<replace-with-your-text-analytics-key-here>");
private static readonly Uri endpoint = new Uri("<replace-with-your-text-analytics-endpoint-here>");
```

Vervang de methode `Main` van de toepassing. U definieert later de methoden die hier worden aangeroepen.

```csharp
static void Main(string[] args)
{
    var client = new TextAnalyticsClient(endpoint, credentials);
    // You will implement these methods later in the quickstart.
    SentimentAnalysisExample(client);
    SentimentAnalysisWithOpinionMiningExample(client);
    LanguageDetectionExample(client);
    EntityRecognitionExample(client);
    EntityLinkingExample(client);
    RecognizePIIExample(client);
    KeyPhraseExtractionExample(client);

    Console.Write("Press any key to exit.");
    Console.ReadKey();
}
```

# <a name="version-30"></a>[Versie 3.0](#tab/version-3)

Open het bestand *program.cs* en voeg de volgende `using`-instructies toe:

```csharp
using Azure;
using System;
using System.Globalization;
using Azure.AI.TextAnalytics;
```

Maak in de klasse `Program` van de toepassing variabelen voor de sleutel en het eindpunt van uw resource.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly AzureKeyCredential credentials = new AzureKeyCredential("<replace-with-your-text-analytics-key-here>");
private static readonly Uri endpoint = new Uri("<replace-with-your-text-analytics-endpoint-here>");
```

Vervang de methode `Main` van de toepassing. U definieert later de methoden die hier worden aangeroepen.

```csharp
static void Main(string[] args)
{
    var client = new TextAnalyticsClient(endpoint, credentials);
    // You will implement these methods later in the quickstart.
    SentimentAnalysisExample(client);
    LanguageDetectionExample(client);
    EntityRecognitionExample(client);
    EntityLinkingExample(client);
    KeyPhraseExtractionExample(client);

    Console.Write("Press any key to exit.");
    Console.ReadKey();
}
```

---

## <a name="object-model"></a>Objectmodel

De Text Analytics-client is een `TextAnalyticsClient`-object dat wordt geverifieerd bij Azure met behulp van uw sleutel. De client biedt functies om tekst als afzonderlijke tekenreeksen of als een batch te accepteren. U kunt tekst synchroon of asynchroon naar de API verzenden. Het responsobject bevat de analyse-informatie voor elk document dat u verzendt. 

Als u versie `3.x` van de service gebruikt, kunt u een optioneel exemplaar van `TextAnalyticsClientOptions` gebruiken om de client te initialiseren met verschillende standaardinstellingen (zoals een standaardtaal voor het land of de regio). U kunt ook verifiëren met een Azure Active Directory-token. 

## <a name="code-examples"></a>Codevoorbeelden

* [Sentimentanalyse](#sentiment-analysis)
* [Meninganalyse](#opinion-mining)
* [Taaldetectie](#language-detection)
* [Herkenning van benoemde entiteiten](#named-entity-recognition-ner)
* [Entiteiten koppelen](#entity-linking)
* [Sleuteltermextractie](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>De client verifiëren

# <a name="version-31-preview"></a>[Versie 3.1: preview](#tab/version-3-1)

Zorg ervoor dat met uw eerdere main-methode een nieuw clientobject wordt gemaakt met uw eindpunt en referenties.

```csharp
var client = new TextAnalyticsClient(endpoint, credentials);
```

# <a name="version-30"></a>[Versie 3.0](#tab/version-3)

Zorg ervoor dat met uw eerdere main-methode een nieuw clientobject wordt gemaakt met uw eindpunt en referenties.

```csharp
var client = new TextAnalyticsClient(endpoint, credentials);
```

---

## <a name="sentiment-analysis"></a>Sentimentanalyse

# <a name="version-31-preview"></a>[Versie 3.1: preview](#tab/version-3-1)

Maak een nieuwe functie met de naam `SentimentAnalysisExample()` waarvoor de client wordt gebruikt die u eerder hebt gemaakt en roep de bijbehorende functie `AnalyzeSentiment()` aan. Het geretourneerde `Response<DocumentSentiment>`-object bevat het sentimentlabel en de score van het volledige invoerdocument, evenals een sentimentanalyse voor elke zin als dit is gelukt. Als er een fout optreedt, wordt er een `RequestFailedException` gegenereerd.

```csharp
static void SentimentAnalysisExample(TextAnalyticsClient client)
{
    string inputText = "I had the best day of my life. I wish you were there with me.";
    DocumentSentiment documentSentiment = client.AnalyzeSentiment(inputText);
    Console.WriteLine($"Document sentiment: {documentSentiment.Sentiment}\n");

    foreach (var sentence in documentSentiment.Sentences)
    {
        Console.WriteLine($"\tText: \"{sentence.Text}\"");
        Console.WriteLine($"\tSentence sentiment: {sentence.Sentiment}");
        Console.WriteLine($"\tPositive score: {sentence.ConfidenceScores.Positive:0.00}");
        Console.WriteLine($"\tNegative score: {sentence.ConfidenceScores.Negative:0.00}");
        Console.WriteLine($"\tNeutral score: {sentence.ConfidenceScores.Neutral:0.00}\n");
    }
}
```

### <a name="output"></a>Uitvoer

```console
Document sentiment: Positive

        Text: "I had the best day of my life."
        Sentence sentiment: Positive
        Positive score: 1.00
        Negative score: 0.00
        Neutral score: 0.00

        Text: "I wish you were there with me."
        Sentence sentiment: Neutral
        Positive score: 0.21
        Negative score: 0.02
        Neutral score: 0.77
```

### <a name="opinion-mining"></a>Meninganalyse

Maak een nieuwe functie met de naam die de client gebruikt die u eerder hebt gemaakt en roep de functie aan `SentimentAnalysisWithOpinionMiningExample()` met de optie in de `AnalyzeSentimentBatch()` `IncludeOpinionMining` `AnalyzeSentimentOptions` zak. Het geretourneerde `AnalyzeSentimentResultCollection`-object bevat de verzameling `AnalyzeSentimentResult` waarin `Response<DocumentSentiment>` voorkomt. Het verschil tussen en is dat de laatste in elke zin bevat, waarin een geanalyseerd doel en `SentimentAnalysis()` `SentimentAnalysisWithOpinionMiningExample()` de gerelateerde `SentenceOpinion` evaluatie(s) worden weer te zien. Als er een fout optreedt, wordt er een `RequestFailedException` gegenereerd.

```csharp
static void SentimentAnalysisWithOpinionMiningExample(TextAnalyticsClient client)
{
    var documents = new List<string>
    {
        "The food and service were unacceptable, but the concierge were nice."
    };

    AnalyzeSentimentResultCollection reviews = client.AnalyzeSentimentBatch(documents, options: new AnalyzeSentimentOptions()
    {
        IncludeOpinionMining = true
    });

    foreach (AnalyzeSentimentResult review in reviews)
    {
        Console.WriteLine($"Document sentiment: {review.DocumentSentiment.Sentiment}\n");
        Console.WriteLine($"\tPositive score: {review.DocumentSentiment.ConfidenceScores.Positive:0.00}");
        Console.WriteLine($"\tNegative score: {review.DocumentSentiment.ConfidenceScores.Negative:0.00}");
        Console.WriteLine($"\tNeutral score: {review.DocumentSentiment.ConfidenceScores.Neutral:0.00}\n");
        foreach (SentenceSentiment sentence in review.DocumentSentiment.Sentences)
        {
            Console.WriteLine($"\tText: \"{sentence.Text}\"");
            Console.WriteLine($"\tSentence sentiment: {sentence.Sentiment}");
            Console.WriteLine($"\tSentence positive score: {sentence.ConfidenceScores.Positive:0.00}");
            Console.WriteLine($"\tSentence negative score: {sentence.ConfidenceScores.Negative:0.00}");
            Console.WriteLine($"\tSentence neutral score: {sentence.ConfidenceScores.Neutral:0.00}\n");

            foreach (SentenceOpinion sentenceOpinion in sentence.Opinions)
            {
                Console.WriteLine($"\tTarget: {sentenceOpinion.Target.Text}, Value: {sentenceOpinion.Target.Sentiment}");
                Console.WriteLine($"\tTarget positive score: {sentenceOpinion.Target.ConfidenceScores.Positive:0.00}");
                Console.WriteLine($"\tTarget negative score: {sentenceOpinion.Target.ConfidenceScores.Negative:0.00}");
                foreach (AssessmentSentiment assessment in sentenceOpinion.Assessments)
                {
                    Console.WriteLine($"\t\tRelated Assessment: {assessment.Text}, Value: {assessment.Sentiment}");
                    Console.WriteLine($"\t\tRelated Assessment positive score: {assessment.ConfidenceScores.Positive:0.00}");
                    Console.WriteLine($"\t\tRelated Assessment negative score: {assessment.ConfidenceScores.Negative:0.00}");
                }
            }
        }
        Console.WriteLine($"\n");
    }
}
```

### <a name="output"></a>Uitvoer

```console
Document sentiment: Positive

        Positive score: 0.84
        Negative score: 0.16
        Neutral score: 0.00

        Text: "The food and service were unacceptable, but the concierge were nice."
        Sentence sentiment: Positive
        Sentence positive score: 0.84
        Sentence negative score: 0.16
        Sentence neutral score: 0.00

        Target: food, Value: Negative
        Target positive score: 0.01
        Target negative score: 0.99
                Related Assessment: unacceptable, Value: Negative
                Related Assessment positive score: 0.01
                Related Assessment negative score: 0.99
        Target: service, Value: Negative
        Target positive score: 0.01
        Target negative score: 0.99
                Related Assessment: unacceptable, Value: Negative
                Related Assessment positive score: 0.01
                Related Assessment negative score: 0.99
        Target: concierge, Value: Positive
        Target positive score: 1.00
        Target negative score: 0.00
                Related Assessment: nice, Value: Positive
                Related Assessment positive score: 1.00
                Related Assessment negative score: 0.00

Press any key to exit.
```

# <a name="version-30"></a>[Versie 3.0](#tab/version-3)

Maak een nieuwe functie met de naam `SentimentAnalysisExample()` waarvoor de client wordt gebruikt die u eerder hebt gemaakt en roep de bijbehorende functie `AnalyzeSentiment()` aan. Het geretourneerde `Response<DocumentSentiment>`-object bevat het sentimentlabel en de score van het volledige invoerdocument, evenals een sentimentanalyse voor elke zin als dit is gelukt. Als er een fout optreedt, wordt er een `RequestFailedException` gegenereerd.

```csharp
static void SentimentAnalysisExample(TextAnalyticsClient client)
{
    string inputText = "I had the best day of my life. I wish you were there with me.";
    DocumentSentiment documentSentiment = client.AnalyzeSentiment(inputText);
    Console.WriteLine($"Document sentiment: {documentSentiment.Sentiment}\n");

    foreach (var sentence in documentSentiment.Sentences)
    {
        Console.WriteLine($"\tText: \"{sentence.Text}\"");
        Console.WriteLine($"\tSentence sentiment: {sentence.Sentiment}");
        Console.WriteLine($"\tPositive score: {sentence.ConfidenceScores.Positive:0.00}");
        Console.WriteLine($"\tNegative score: {sentence.ConfidenceScores.Negative:0.00}");
        Console.WriteLine($"\tNeutral score: {sentence.ConfidenceScores.Neutral:0.00}\n");
    }
}
```

### <a name="output"></a>Uitvoer

```console
Document sentiment: Positive

        Text: "I had the best day of my life."
        Sentence sentiment: Positive
        Positive score: 1.00
        Negative score: 0.00
        Neutral score: 0.00

        Text: "I wish you were there with me."
        Sentence sentiment: Neutral
        Positive score: 0.21
        Negative score: 0.02
        Neutral score: 0.77
```

---

## <a name="language-detection"></a>Taaldetectie

# <a name="version-31-preview"></a>[Versie 3.1: preview](#tab/version-3-1)


Maak een nieuwe functie met de naam `LanguageDetectionExample()` waarvoor de client wordt gebruikt die u eerder hebt gemaakt en roep de bijbehorende functie `DetectLanguage()` aan. Het geretourneerde `Response<DetectedLanguage>`-object bevat de gedetecteerde taal, samen met de naam en ISO 6391-code van de taal. Als er een fout optreedt, wordt er een `RequestFailedException` gegenereerd.

> [!Tip]
> In sommige gevallen kan het lastig zijn om talen ondubbelzinnig te karakteriseren op basis van de invoer. U kunt parameter `countryHint` gebruiken om een land- of regionummer van twee letters op te geven. Standaard gebruikt de API de standaard-countryHint US. Als u dit gedrag wilt verwijderen, kunt u deze parameter opnieuw instellen door deze waarde in te stellen op de lege tekenreeks `countryHint = ""`. Als u een andere standaardwaarde wilt instellen, stelt u eigenschap `TextAnalyticsClientOptions.DefaultCountryHint` in en geeft u deze door tijdens de initialisatie van de client.

```csharp
static void LanguageDetectionExample(TextAnalyticsClient client)
{
    DetectedLanguage detectedLanguage = client.DetectLanguage("Ce document est rédigé en Français.");
    Console.WriteLine("Language:");
    Console.WriteLine($"\t{detectedLanguage.Name},\tISO-6391: {detectedLanguage.Iso6391Name}\n");
}
```

### <a name="output"></a>Uitvoer

```console
Language:
        French, ISO-6391: fr
```

# <a name="version-30"></a>[Versie 3.0](#tab/version-3)


Maak een nieuwe functie met de naam `LanguageDetectionExample()` waarvoor de client wordt gebruikt die u eerder hebt gemaakt en roep de bijbehorende functie `DetectLanguage()` aan. Het geretourneerde `Response<DetectedLanguage>`-object bevat de gedetecteerde taal, samen met de naam en ISO 6391-code van de taal. Als er een fout optreedt, wordt er een `RequestFailedException` gegenereerd.

> [!Tip]
> In sommige gevallen kan het lastig zijn om talen ondubbelzinnig te karakteriseren op basis van de invoer. U kunt parameter `countryHint` gebruiken om een land- of regionummer van twee letters op te geven. Standaard gebruikt de API de standaard-countryHint US. Als u dit gedrag wilt verwijderen, kunt u deze parameter opnieuw instellen door deze waarde in te stellen op de lege tekenreeks `countryHint = ""`. Als u een andere standaardwaarde wilt instellen, stelt u eigenschap `TextAnalyticsClientOptions.DefaultCountryHint` in en geeft u deze door tijdens de initialisatie van de client.

```csharp
static void LanguageDetectionExample(TextAnalyticsClient client)
{
    DetectedLanguage detectedLanguage = client.DetectLanguage("Ce document est rédigé en Français.");
    Console.WriteLine("Language:");
    Console.WriteLine($"\t{detectedLanguage.Name},\tISO-6391: {detectedLanguage.Iso6391Name}\n");
}
```

### <a name="output"></a>Uitvoer

```console
Language:
        French, ISO-6391: fr
```


---

## <a name="named-entity-recognition-ner"></a>NER (Herkenning van benoemde entiteiten)

# <a name="version-31-preview"></a>[Versie 3.1: preview](#tab/version-3-1)


Maak een nieuwe functie met de naam `EntityRecognitionExample()` waarvoor de client wordt gebruikt die u eerder hebt gemaakt, roep de functie `RecognizeEntities()` aan van de nieuwe functie en herhaal de resultaten. Het geretourneerde `Response<CategorizedEntityCollection>`-object bevat de verzameling met gedetecteerde entiteiten `CategorizedEntity`. Als er een fout optreedt, wordt er een `RequestFailedException` gegenereerd.

```csharp
static void EntityRecognitionExample(TextAnalyticsClient client)
{
    var response = client.RecognizeEntities("I had a wonderful trip to Seattle last week.");
    Console.WriteLine("Named Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tText: {entity.Text},\tCategory: {entity.Category},\tSub-Category: {entity.SubCategory}");
        Console.WriteLine($"\t\tScore: {entity.ConfidenceScore:F2},\tLength: {entity.Length},\tOffset: {entity.Offset}\n");
    }
}
```

### <a name="output"></a>Uitvoer

```console
Named Entities:
        Text: trip,     Category: Event,        Sub-Category:
                Score: 0.61,    Length: 4,      Offset: 18

        Text: Seattle,  Category: Location,     Sub-Category: GPE
                Score: 0.82,    Length: 7,      Offset: 26

        Text: last week,        Category: DateTime,     Sub-Category: DateRange
                Score: 0.80,    Length: 9,      Offset: 34
```

### <a name="entity-linking"></a>Entiteiten koppelen

Maak een nieuwe functie met de naam `EntityLinkingExample()` waarvoor de client wordt gebruikt die u eerder hebt gemaakt, roep de functie `RecognizeLinkedEntities()` aan van de nieuwe functie en herhaal de resultaten. Het geretourneerde `Response<LinkedEntityCollection>`-object bevat de verzameling met gedetecteerde entiteiten `LinkedEntity`. Als er een fout optreedt, wordt er een `RequestFailedException` gegenereerd. Aangezien gekoppelde entiteiten uniek worden geïdentificeerd, worden exemplaren van dezelfde entiteit gegroepeerd onder een `LinkedEntity`-object als een lijst met `LinkedEntityMatch`-objecten.

```csharp
static void EntityLinkingExample(TextAnalyticsClient client)
{
    var response = client.RecognizeLinkedEntities(
        "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, " +
        "to develop and sell BASIC interpreters for the Altair 8800. " +
        "During his career at Microsoft, Gates held the positions of chairman, " +
        "chief executive officer, president and chief software architect, " +
        "while also being the largest individual shareholder until May 2014.");
    Console.WriteLine("Linked Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tName: {entity.Name},\tID: {entity.DataSourceEntityId},\tURL: {entity.Url}\tData Source: {entity.DataSource}");
        Console.WriteLine("\tMatches:");
        foreach (var match in entity.Matches)
        {
            Console.WriteLine($"\t\tText: {match.Text}");
            Console.WriteLine($"\t\tScore: {match.ConfidenceScore:F2}");
            Console.WriteLine($"\t\tLength: {match.Length}");
            Console.WriteLine($"\t\tOffset: {match.Offset}\n");
        }
    }
}
```

### <a name="output"></a>Uitvoer

```console
Linked Entities:
        Name: Microsoft,        ID: Microsoft,  URL: https://en.wikipedia.org/wiki/Microsoft    Data Source: Wikipedia
        Matches:
                Text: Microsoft
                Score: 0.55
                Length: 9
                Offset: 0

                Text: Microsoft
                Score: 0.55
                Length: 9
                Offset: 150

        Name: Bill Gates,       ID: Bill Gates, URL: https://en.wikipedia.org/wiki/Bill_Gates   Data Source: Wikipedia
        Matches:
                Text: Bill Gates
                Score: 0.63
                Length: 10
                Offset: 25

                Text: Gates
                Score: 0.63
                Length: 5
                Offset: 161

        Name: Paul Allen,       ID: Paul Allen, URL: https://en.wikipedia.org/wiki/Paul_Allen   Data Source: Wikipedia
        Matches:
                Text: Paul Allen
                Score: 0.60
                Length: 10
                Offset: 40

        Name: April 4,  ID: April 4,    URL: https://en.wikipedia.org/wiki/April_4      Data Source: Wikipedia
        Matches:
                Text: April 4
                Score: 0.32
                Length: 7
                Offset: 54

        Name: BASIC,    ID: BASIC,      URL: https://en.wikipedia.org/wiki/BASIC        Data Source: Wikipedia
        Matches:
                Text: BASIC
                Score: 0.33
                Length: 5
                Offset: 89

        Name: Altair 8800,      ID: Altair 8800,        URL: https://en.wikipedia.org/wiki/Altair_8800  Data Source: Wikipedia
        Matches:
                Text: Altair 8800
                Score: 0.88
                Length: 11
                Offset: 116
```

### <a name="personally-identifiable-information-recognition"></a>Herkenning van persoonlijke gegevens

Maak een nieuwe functie met de naam `RecognizePIIExample()` waarvoor de client wordt gebruikt die u eerder hebt gemaakt, roep de functie `RecognizePiiEntities()` aan van de nieuwe functie en herhaal de resultaten. Het geretourneerde `PiiEntityCollection`-object vertegenwoordigt de lijst met gedetecteerde PII-entiteiten. Als er een fout optreedt, wordt er een `RequestFailedException` gegenereerd.

```csharp
static void RecognizePIIExample(TextAnalyticsClient client)
{
    string document = "A developer with SSN 859-98-0987 whose phone number is 800-102-1100 is building tools with our APIs.";

    PiiEntityCollection entities = client.RecognizePiiEntities(document).Value;

    Console.WriteLine($"Redacted Text: {entities.RedactedText}");
    if (entities.Count > 0)
    {
        Console.WriteLine($"Recognized {entities.Count} PII entit{(entities.Count > 1 ? "ies" : "y")}:");
        foreach (PiiEntity entity in entities)
        {
            Console.WriteLine($"Text: {entity.Text}, Category: {entity.Category}, SubCategory: {entity.SubCategory}, Confidence score: {entity.ConfidenceScore}");
        }
    }
    else
    {
        Console.WriteLine("No entities were found.");
    }
}
```

### <a name="output"></a>Uitvoer

```console
Redacted Text: A developer with SSN *********** whose phone number is ************ is building tools with our APIs.
Recognized 2 PII entities:
Text: 859-98-0987, Category: U.S. Social Security Number (SSN), SubCategory: , Confidence score: 0.65
Text: 800-102-1100, Category: Phone Number, SubCategory: , Confidence score: 0.8
```

# <a name="version-30"></a>[Versie 3.0](#tab/version-3)


> [!NOTE]
> Nieuw in versie `3.0`:
> * Entiteitskoppeling is nu gescheiden van entiteitsherkenning.


Maak een nieuwe functie met de naam `EntityRecognitionExample()` waarvoor de client wordt gebruikt die u eerder hebt gemaakt, roep de functie `RecognizeEntities()` aan van de nieuwe functie en herhaal de resultaten. Het geretourneerde `Response<IReadOnlyCollection<CategorizedEntity>>`-object bevat de lijst met gedetecteerde entiteiten. Als er een fout optreedt, wordt er een `RequestFailedException` gegenereerd.

```csharp
static void EntityRecognitionExample(TextAnalyticsClient client)
{
    var response = client.RecognizeEntities("I had a wonderful trip to Seattle last week.");
    Console.WriteLine("Named Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tText: {entity.Text},\tCategory: {entity.Category},\tSub-Category: {entity.SubCategory}");
        Console.WriteLine($"\t\tScore: {entity.ConfidenceScore:F2}\n");
    }
}
```

### <a name="output"></a>Uitvoer

```console
Named Entities:
        Text: trip,     Category: Event,        Sub-Category:
                Score: 0.61

        Text: Seattle,  Category: Location,     Sub-Category: GPE
                Score: 0.82

        Text: last week,        Category: DateTime,     Sub-Category: DateRange
                Score: 0.80
```

### <a name="entity-linking"></a>Entiteiten koppelen

Maak een nieuwe functie met de naam `EntityLinkingExample()` waarvoor de client wordt gebruikt die u eerder hebt gemaakt, roep de functie `RecognizeLinkedEntities()` aan van de nieuwe functie en herhaal de resultaten. Het geretourneerde object `Response<IReadOnlyCollection<LinkedEntity>>` vertegenwoordigt de lijst met gedetecteerde entiteiten. Als er een fout optreedt, wordt er een `RequestFailedException` gegenereerd. Aangezien gekoppelde entiteiten uniek worden geïdentificeerd, worden exemplaren van dezelfde entiteit gegroepeerd onder een `LinkedEntity`-object als een lijst met `LinkedEntityMatch`-objecten.

```csharp
static void EntityLinkingExample(TextAnalyticsClient client)
{
    var response = client.RecognizeLinkedEntities(
        "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, " +
        "to develop and sell BASIC interpreters for the Altair 8800. " +
        "During his career at Microsoft, Gates held the positions of chairman, " +
        "chief executive officer, president and chief software architect, " +
        "while also being the largest individual shareholder until May 2014.");
    Console.WriteLine("Linked Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tName: {entity.Name},\tID: {entity.DataSourceEntityId},\tURL: {entity.Url}\tData Source: {entity.DataSource}");
        Console.WriteLine("\tMatches:");
        foreach (var match in entity.Matches)
        {
            Console.WriteLine($"\t\tText: {match.Text}");
            Console.WriteLine($"\t\tScore: {match.ConfidenceScore:F2}\n");
        }
    }
}
```

### <a name="output"></a>Uitvoer

```console
Linked Entities:
        Name: Altair 8800,      ID: Altair 8800,        URL: https://en.wikipedia.org/wiki/Altair_8800  Data Source: Wikipedia
        Matches:
                Text: Altair 8800
                Score: 0.88

        Name: Bill Gates,       ID: Bill Gates, URL: https://en.wikipedia.org/wiki/Bill_Gates   Data Source: Wikipedia
        Matches:
                Text: Bill Gates
                Score: 0.63

                Text: Gates
                Score: 0.63

        Name: Paul Allen,       ID: Paul Allen, URL: https://en.wikipedia.org/wiki/Paul_Allen   Data Source: Wikipedia
        Matches:
                Text: Paul Allen
                Score: 0.60

        Name: Microsoft,        ID: Microsoft,  URL: https://en.wikipedia.org/wiki/Microsoft    Data Source: Wikipedia
        Matches:
                Text: Microsoft
                Score: 0.55

                Text: Microsoft
                Score: 0.55

        Name: April 4,  ID: April 4,    URL: https://en.wikipedia.org/wiki/April_4      Data Source: Wikipedia
        Matches:
                Text: April 4
                Score: 0.32

        Name: BASIC,    ID: BASIC,      URL: https://en.wikipedia.org/wiki/BASIC        Data Source: Wikipedia
        Matches:
                Text: BASIC
                Score: 0.33
```

--- 


### <a name="key-phrase-extraction"></a>Sleuteltermextractie

# <a name="version-31-preview"></a>[Versie 3.1: preview](#tab/version-3-1)

Maak een nieuwe functie met de naam `KeyPhraseExtractionExample()` waarvoor de client wordt gebruikt die u eerder hebt gemaakt en roep de bijbehorende functie `ExtractKeyPhrases()` aan. Het geretourneerde object `<Response<KeyPhraseCollection>` bevat de lijst met gedetecteerde sleuteltermen. Als er een fout optreedt, wordt er een `RequestFailedException` gegenereerd.

```csharp
static void KeyPhraseExtractionExample(TextAnalyticsClient client)
{
    var response = client.ExtractKeyPhrases("My cat might need to see a veterinarian.");

    // Printing key phrases
    Console.WriteLine("Key phrases:");

    foreach (string keyphrase in response.Value)
    {
        Console.WriteLine($"\t{keyphrase}");
    }
}
```

### <a name="output"></a>Uitvoer

```console
Key phrases:
    cat
    veterinarian
```

# <a name="version-30"></a>[Versie 3.0](#tab/version-3)

Maak een nieuwe functie met de naam `KeyPhraseExtractionExample()` waarvoor de client wordt gebruikt die u eerder hebt gemaakt en roep de bijbehorende functie `ExtractKeyPhrases()` aan. Het geretourneerde object `<Response<IReadOnlyCollection<string>>` bevat de lijst met gedetecteerde sleuteltermen. Als er een fout optreedt, wordt er een `RequestFailedException` gegenereerd.

```csharp
static void KeyPhraseExtractionExample(TextAnalyticsClient client)
{
    var response = client.ExtractKeyPhrases("My cat might need to see a veterinarian.");

    // Printing key phrases
    Console.WriteLine("Key phrases:");

    foreach (string keyphrase in response.Value)
    {
        Console.WriteLine($"\t{keyphrase}");
    }
}
```

### <a name="output"></a>Uitvoer

```console
Key phrases:
    cat
    veterinarian
```

---

## <a name="use-the-api-asynchronously-with-the-analyze-operation"></a>De API asynchroon gebruiken met de analysebewerking

# <a name="version-31-preview"></a>[Versie 3.1: preview](#tab/version-3-1)

[!INCLUDE [Analyze operation pricing](../analyze-operation-pricing-caution.md)]

Maak een nieuwe functie met de naam `AnalyzeOperationExample()` waarvoor de client wordt gebruikt die u eerder hebt gemaakt en roep de bijbehorende functie `StartAnalyzeBatchActionsAsync()` aan. Het `AnalyzeBatchActionsOperation` geretourneerde object bevat het `Operation` interfaceobject. Aangezien het een langdurige bewerking is, moet `await` op `operation.WaitForCompletionAsync()` voor de waarde worden bijgewerkt. Zodra `WaitForCompletionAsync()` is voltooid, moet de verzameling worden bijgewerkt in `operation.Value`. Als er een fout optreedt, wordt er een `RequestFailedException` gegenereerd.


```csharp
static async Task AnalyzeOperationExample(TextAnalyticsClient client)
{
    string inputText = "Microsoft was founded by Bill Gates and Paul Allen.";

    var batchDocuments = new List<string> { inputText };


    TextAnalyticsActions actions = new TextAnalyticsActions()
    {
        RecognizeEntitiesOptions = new List<RecognizeEntitiesOptions>() { new RecognizeEntitiesOptions() },
        DisplayName = "Analyze Operation Quick Start Example"
    };

    AnalyzeBatchActionsOperation operation = await client.StartAnalyzeBatchActionsAsync(batchDocuments, actions);

    await operation.WaitForCompletionAsync();

    Console.WriteLine($"Status: {operation.Status}");
    Console.WriteLine($"Created On: {operation.CreatedOn}");
    Console.WriteLine($"Expires On: {operation.ExpiresOn}");
    Console.WriteLine($"Last modified: {operation.LastModified}");
    if (!string.IsNullOrEmpty(operation.DisplayName))
        Console.WriteLine($"Display name: {operation.DisplayName}");
    Console.WriteLine($"Total actions: {operation.TotalActions}");
    Console.WriteLine($"  Succeeded actions: {operation.ActionsSucceeded}");
    Console.WriteLine($"  Failed actions: {operation.ActionsFailed}");
    Console.WriteLine($"  In progress actions: {operation.ActionsInProgress}");

    await foreach (AnalyzeBatchActionsResult documentsInPage in operation.Value)
    {
        RecognizeEntitiesResultCollection entitiesResult = documentsInPage.RecognizeEntitiesActionsResults.FirstOrDefault().Result;

        Console.WriteLine("Recognized Entities");

        foreach (RecognizeEntitiesResult result in entitiesResult)
        {
            Console.WriteLine($"  Recognized the following {result.Entities.Count} entities:");

            foreach (CategorizedEntity entity in result.Entities)
            {
                Console.WriteLine($"  Entity: {entity.Text}");
                Console.WriteLine($"  Category: {entity.Category}");
                Console.WriteLine($"  Offset: {entity.Offset}");
                Console.WriteLine($"  Length: {entity.Length}");
                Console.WriteLine($"  ConfidenceScore: {entity.ConfidenceScore}");
                Console.WriteLine($"  SubCategory: {entity.SubCategory}");
            }
            Console.WriteLine("");
        }
    }
}
```

Nadat u dit voorbeeld aan uw toepassing hebt toegevoegd, roept u de methode `main()` aan met behulp van `await`.

```csharp
await AnalyzeOperationExample(client).ConfigureAwait(false);
```
### <a name="output"></a>Uitvoer

```console
Status: succeeded
Created On: 3/10/2021 2:25:01 AM +00:00
Expires On: 3/11/2021 2:25:01 AM +00:00
Last modified: 3/10/2021 2:25:05 AM +00:00
Display name: Analyze Operation Quick Start Example
Total actions: 1
  Succeeded actions: 1
  Failed actions: 0
  In progress actions: 0
Recognized Entities
    Recognized the following 3 entities:
    Entity: Microsoft
    Category: Organization
    Offset: 0
    ConfidenceScore: 0.83
    SubCategory: 
    Entity: Bill Gates
    Category: Person
    Offset: 25
    ConfidenceScore: 0.85
    SubCategory: 
    Entity: Paul Allen
    Category: Person
    Offset: 40
    ConfidenceScore: 0.9
    SubCategory: 
```

U kunt ook de bewerking Analyseren ook gebruiken om persoonsgegevens en extractie van sleutelwoorden te detecteren. Zie het [analysevoorbeeld](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics/samples) (Engelstalig) op GitHub.

# <a name="version-30"></a>[Versie 3.0](#tab/version-3)

Deze functie is niet beschikbaar in versie 3.0.

---
