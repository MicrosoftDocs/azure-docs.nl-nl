---
title: 'Snelstartgids: Azure-resources toevoegen aan uw IoT-oplossing'
description: In deze quickstart leert u hoe u uw end-to-end IoT-oplossing configureert met behulp van Azure Defender for IoT.
ms.topic: quickstart
ms.date: 01/25/2021
ms.openlocfilehash: 6a90e8c3007f7b3448fd3f1b6e4fa46ba861081b
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2021
ms.locfileid: "106384577"
---
# <a name="quickstart-configure-your-azure-defender-for-iot-solution"></a>Quickstart: Configureer uw Azure Defender for IoT

In dit artikel wordt uitgelegd hoe u een eerste configuratie van uw IoT-beveiligingsoplossing uitvoert met behulp van Defender for IoT.

## <a name="prerequisites"></a>Vereisten

- Geen

## <a name="what-is-defender-for-iot"></a>Wat is Defender for IoT?

Defender for IoT biedt uitgebreide end-to-end beveiliging voor Azure-gebaseerde IoT-oplossingen.

Met Defender voor IoT kunt u uw volledige IoT-oplossing in één dash board bewaken, halen al uw IoT-apparaten, IoT-platformen en back-end-resources in Azure.

Zodra Defender for IoT is ingeschakeld op uw IoT-hub, identificeert het automatisch andere Azure-services die ook verbonden zijn met uw IoT-hub en gerelateerd zijn aan uw IoT-oplossing.

Naast automatische relatiedetectie kunt u ook zelf kiezen welke andere Azure-resourcegroepen u wilt taggen als onderdeel van uw IoT-oplossing.

Uw selecties stellen u in staat hele abonnementen, resourcegroepen of afzonderlijke resources toe te voegen.

Nadat u alle resource relaties hebt gedefinieerd, gebruikt Defender voor IoT Defender om u te voorzien van beveiligings aanbevelingen en waarschuwingen voor deze resources.

## <a name="add-azure-resources-to-your-iot-solution"></a>Azure-resources toevoegen aan uw IoT-oplossing

Nieuwe resource toevoegen aan uw IoT-oplossing:

1. Open uw **IoT-hub** in de Azure-portal.

1. Onder **Beveiliging** selecteert u **Overzicht** gevolgd door **Instellingen**> Selecteer vervolgens **Bewaakte resources**.

1. Selecteer **Bewerken** en selecteer de bewaakte resources die bij uw IoT-oplossing horen.

1. Selecteer **Toevoegen**.

Gefeliciteerd. U hebt een nieuwe resourcegroep toegevoegd aan uw IoT-oplossing.

Defender for IoT bewaakt nu uw zojuist toegevoegde resourcegroepen en toont relevante beveiligingsaanbevelingen en -waarschuwingen als onderdeel van uw IoT-oplossing.

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel voor meer informatie over het maken van Defender-IoT-micro-agents...

> [!div class="nextstepaction"]
> [Defender-IoT-micro-agents maken](quickstart-create-security-twin.md)
