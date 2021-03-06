---
title: Een video bijsnijden bij het coderen met Media Services
description: In dit onderwerp wordt beschreven hoe u een video bijsnijden bij het coderen met Azure Media Services met behulp van .NET SDK
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: how-to
ms.date: 06/09/2019
ms.author: inhenkel
ms.custom: devx-track-csharp
ms.openlocfilehash: 333d9d7e23182a98e9b8ad5bcc9c2a63362b2293
ms.sourcegitcommit: bfa7d6ac93afe5f039d68c0ac389f06257223b42
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2021
ms.locfileid: "106492355"
---
# <a name="subclip-a-video-when-encoding-with-media-services---net"></a>Een video afspelen tijdens het coderen met Media Services-.NET

U kunt een video knippen of subfragmenteren wanneer u deze codeert met behulp van een [taak](/rest/api/media/jobs). Deze functionaliteit werkt met elke [trans formatie](/rest/api/media/transforms) die is gebouwd met behulp van de [BuiltInStandardEncoderPreset](/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) -voor instellingen of de [StandardEncoderPreset](/rest/api/media/transforms/createorupdate#standardencoderpreset) -voor waarden.

In het volgende C#-voor beeld wordt een taak gemaakt die een video in een activum bijsnijdt bij het verzenden van een coderings taak. 

## <a name="prerequisites"></a>Vereisten

Voor het uitvoeren van de stappen die in dit onderwerp worden beschreven, moet u het volgende doen:

- [Een Azure Media Services-account maken](./account-create-how-to.md)
- Maak een trans formatie en een invoer-en uitvoer activa. U kunt zien hoe u een trans formatie-en invoer-en uitvoer assets maakt in de zelf studie [video uploaden, coderen en streamen met behulp van .net](stream-files-tutorial-with-api.md) .
- Bekijk het [Codeer concept](encode-concept.md) onderwerp.

## <a name="example"></a>Voorbeeld

```csharp
/// <summary>
/// Submits a request to Media Services to apply the specified Transform to a given input video.
/// </summary>
/// <param name="client">The Media Services client.</param>
/// <param name="resourceGroupName">The name of the resource group within the Azure subscription.</param>
/// <param name="accountName"> The Media Services account name.</param>
/// <param name="transformName">The name of the transform.</param>
/// <param name="jobName">The (unique) name of the job.</param>
/// <param name="inputAssetName">The name of the input asset.</param>
/// <param name="outputAssetName">The (unique) name of the  output asset that will store the result of the encoding job. </param>
// <SubmitJob>
private static async Task<Job> JobWithBuiltInStandardEncoderWithSingleClipAsync(
    IAzureMediaServicesClient client,
    string resourceGroupName,
    string accountName,
    string transformName,
    string jobName,
    string inputAssetName,
    string outputAssetName)
{
    var jobOutputs = new List<JobOutputAsset>
    {
        new JobOutputAsset(state: JobState.Queued, progress: 0, assetName: outputAssetName)
    };

    var clipStart = new AbsoluteClipTime()
    {
        Time = new TimeSpan(0, 0, 20)
    };

    var clipEnd = new AbsoluteClipTime()
    {
        Time = new TimeSpan(0, 0, 30)
    };

    var jobInput = new JobInputAsset(assetName: inputAssetName, start: clipStart, end: clipEnd);

    Job job = await client.Jobs.CreateAsync(
        resourceGroupName,
        accountName,
        transformName,
        jobName,
        new Job(input: jobInput, outputs: jobOutputs.ToArray(), name: jobName)
        {
            Description = $"A Job with transform {transformName} and single clip.",
            Priority = Priority.Normal,
        });

    return job;
}
```

## <a name="next-steps"></a>Volgende stappen

[Coderen met een aangepaste trans formatie](transform-custom-presets-how-to.md) 
