---
title: De Azure-portal gebruiken om een Service Bus-wachtrij te maken
description: In deze quickstart leert u hoe u een Service Bus-naamruimte en een wachtrij in de naamruimte maakt met behulp van Azure Portal.
author: spelluru
ms.author: spelluru
ms.date: 08/12/2020
ms.topic: quickstart
ms.custom:
- mode-portal
ms.openlocfilehash: e38d32c93453737060f654add58f09902b05ee45
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2021
ms.locfileid: "107537198"
---
# <a name="use-azure-portal-to-create-a-service-bus-namespace-and-a-queue"></a>Azure Portal gebruiken om een Service Bus-naamruimte en -wachtrij te maken
In deze quickstart wordt beschreven hoe u een Service Bus-naamruimte en -wachtrij maakt met behulp van [Azure Portal][Azure portal]. Er wordt ook beschreven hoe u autorisatiereferenties ophaalt die een client-toepassing kan gebruiken voor het verzenden/ontvangen van berichten naar/van de wachtrij. 

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over een Azure-abonnement beschikt om deze quickstart uit te voeren. Als u nog geen abonnement op Azure hebt, kunt u een [gratis account][] maken voordat u begint.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u een Service Bus-naamruimte en een wachtrij in de naamruimte gemaakt. Raadpleeg een van de volgende quickstarts in het gedeelte **Berichten verzenden en ontvangen** om te zien hoe berichten worden verzonden/ontvangen naar/van de wachtrij. 

- [.NET](service-bus-dotnet-get-started-with-queues.md)
- [Java](service-bus-java-how-to-use-queues.md)
- [JavaScript](service-bus-nodejs-how-to-use-queues.md)
- [Python](service-bus-python-how-to-use-queues.md)
- [PHP](service-bus-php-how-to-use-queues.md)
- [Ruby](service-bus-ruby-how-to-use-queues.md)

[gratis account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[Azure portal]: https://portal.azure.com/

[service-bus-flow]: ./media/service-bus-quickstart-portal/service-bus-flow.png
