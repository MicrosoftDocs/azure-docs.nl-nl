---
title: bestand opnemen
description: bestand opnemen
services: azure-communication-services
author: mikben
manager: mikben
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 03/10/2021
ms.topic: include
ms.custom: include file
ms.author: mikben
ms.openlocfilehash: 24a5c92164e0eace41224edfd2153c6142f7ea49
ms.sourcegitcommit: b28e9f4d34abcb6f5ccbf112206926d5434bd0da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/09/2021
ms.locfileid: "107251379"
---
[!INCLUDE [Public Preview Notice](../../../includes/public-preview-include-chat.md)]

## <a name="prerequisites"></a>Vereisten
Voordat u aan de slag gaat, moet u het volgende doen:

- Maak een Azure-account met een actief abonnement. Zie [Gratis een account maken](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voor meer informatie. 
- Installeer [Xcode](https://developer.apple.com/xcode/) en [CocoaPods](https://cocoapods.org/). U gebruikt Xcode om een iOS-toepassing te maken voor de Snelstartgids en CocoaPods om afhankelijkheden te installeren.
- Maak een Azure Communication Services-resource. Zie [Quick Start: bronnen voor communicatie Services maken en beheren](../../create-communication-resource.md)voor meer informatie. Voor deze Quick start moet u het resource-eind punt vastleggen.
- Maak twee gebruikers in azure Communication Services en geef ze een [token voor gebruikers toegang](../../access-tokens.md). Zorg ervoor dat u het bereik instelt op `chat` en noteer de `token` teken reeks en de `userId` teken reeks. In deze Quick Start maakt u een thread met een eerste deel nemer en voegt u vervolgens een tweede deel nemer aan de thread toe.

## <a name="setting-up"></a>Instellen

### <a name="create-a-new-ios-application"></a>Een nieuwe iOS-toepassing maken

Open Xcode en selecteer **Een nieuw Xcode-project maken**. Selecteer vervolgens **IOS** als het platform en de **app** voor de sjabloon.

Voer voor de project naam **ChatQuickstart** in. Selecteer vervolgens **Story Board** als de interface, **UIKit app Delegate** als de levens cyclus en **Swift** als de taal.

Selecteer **volgende** en kies de map waarin u het project wilt maken.

### <a name="install-the-libraries"></a>De bibliotheken installeren

Gebruik CocoaPods om de benodigde communicatie Services-afhankelijkheden te installeren.

Ga vanaf de opdracht regel in de hoofdmap van het IOS- `ChatQuickstart` project. Maak een Podfile met de volgende opdracht: `pod init` .

Open de Podfile en voeg de volgende afhankelijkheden toe aan het `ChatQuickstart` doel:

```
pod 'AzureCommunication', '~> 1.0.0-beta.11'
pod 'AzureCommunicationChat', '~> 1.0.0-beta.11'
```

Installeer de afhankelijkheden met de volgende opdracht: `pod install` . Houd er rekening mee dat hiermee ook een Xcode-werk ruimte wordt gemaakt.

Nadat `pod install` het project is uitgevoerd, opent u het opnieuw in Xcode door de zojuist gemaakte opdracht te selecteren `.xcworkspace` .

### <a name="set-up-the-placeholders"></a>De tijdelijke aanduidingen instellen

Open de werk ruimte `ChatQuickstart.xcworkspace` in Xcode en open `ViewController.swift` .

In deze Quick Start voegt u uw code toe aan `viewController` en bekijkt u de uitvoer in de Xcode-console. In deze Quick Start wordt het bouwen van een gebruikers interface in iOS niet geadresseerd. 

Importeer boven aan de `viewController.swift` `AzureCommunication` `AzureCommunicatonChat` bibliotheken en:

```
import AzureCommunication
import AzureCommunicationChat
```

Kopieer de volgende code in de `viewDidLoad()` methode van `ViewController` :

```
override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.

        let semaphore = DispatchSemaphore(value: 0)
        DispatchQueue.global(qos: .background).async {
            do {
                // <CREATE A CHAT CLIENT>
                
                // <CREATE A CHAT THREAD>

                // <LIST ALL CHAT THREADS>

                // <GET A CHAT THREAD CLIENT>

                // <SEND A MESSAGE>

                // <SEND A READ RECEIPT >

                // <RECEIVE MESSAGES>

                // <ADD A USER>
                
                // <LIST USERS>
            } catch {
                print("Quickstart failed: \(error.localizedDescription)")
            }
        }
    }
```

Voor demonstratie doeleinden gebruiken we een semafoor om uw code te synchroniseren. In de volgende stappen vervangt u de tijdelijke aanduidingen door voorbeeld code met behulp van de Azure Communication Services-chat bibliotheek.


### <a name="create-a-chat-client"></a>Een chat-client maken

Vervang de opmerking `<CREATE A CHAT CLIENT>` door de volgende code:

```
let endpoint = "<ACS_RESOURCE_ENDPOINT>"
let credential =
try CommunicationTokenCredential(
    token: "<ACCESS_TOKEN>"
)
let options = AzureCommunicationChatClientOptions()

let chatClient = try ChatClient(
    endpoint: endpoint,
    credential: credential,
    withOptions: options
)
```

Vervang door `<ACS_RESOURCE_ENDPOINT>` het eind punt van uw Azure Communication Services-resource. Vervang door `<ACCESS_TOKEN>` een geldig toegangs token voor communicatie Services.

Deze Snelstartgids heeft geen betrekking op het maken van een servicelaag voor het beheren van tokens voor uw chat toepassing, maar dat wordt aanbevolen. Zie de sectie ' chat Architecture ' van [Chat concepten](../../../concepts/chat/concepts.md)voor meer informatie.

Voor meer informatie over tokens voor gebruikers toegang, Zie [Quick Start: toegangs tokens maken en beheren](../../access-tokens.md).

## <a name="object-model"></a>Objectmodel 

De volgende klassen en interfaces verwerken enkele van de belangrijkste functies van de Azure Communication Services chat SDK voor Java script.

| Naam                                   | Beschrijving                                                                                                                                                                           |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ChatClient` | Deze klasse is vereist voor de chat functionaliteit. U maakt de app met uw abonnements gegevens en gebruikt deze om threads te maken, op te halen en te verwijderen. |
| `ChatThreadClient` | Deze klasse is vereist voor de functionaliteit van de chat-thread. U kunt een exemplaar verkrijgen via `ChatClient` en dit gebruiken om berichten te verzenden, te ontvangen, bij te werken en te verwijderen. U kunt dit ook gebruiken om gebruikers toe te voegen, te verwijderen en te verkrijgen, getypte meldingen en lees bevestigingen te verzenden en u te abonneren op chat gebeurtenissen. |

## <a name="start-a-chat-thread"></a>Een chat-thread starten

Nu gebruikt u uw `ChatClient` om een nieuwe thread te maken met een eerste gebruiker.

Vervang de opmerking `<CREATE A CHAT THREAD>` door de volgende code:

```
let request = CreateChatThreadRequest(
    topic: "Quickstart",
    participants: [
        ChatParticipant(
            id: CommunicationUserIdentifier("<USER_ID>"),
            displayName: "Jack"
        )
    ]
)

var threadId: String?
chatClient.create(thread: request) { result, _ in
    switch result {
    case let .success(result):
        threadId = result.chatThread?.id

    case .failure:
        fatalError("Failed to create thread.")
    }
    semaphore.signal()
}
semaphore.wait()
```

Vervang door `<USER_ID>` een geldige gebruikers-id voor communicatie Services.

U gebruikt hier een semafoor om te wachten op de voltooiings-handler voordat u doorgaat. In latere stappen gebruikt u de `threadId` van de reactie die wordt geretourneerd naar de voltooiings-handler.

## <a name="list-all-chat-threads"></a>Alle chat-threads weer geven

Nadat u een chat-thread hebt gemaakt, kunt u alle chat-threads weer geven door de- `listChatThreads` methode aan te roepen `ChatClient` . Vervang de opmerking `<LIST ALL CHAT THREADS>` door de volgende code:

```
chatClient.listThreads { result, _ in
    switch result {
    case let .success(chatThreadItems):
        var iterator = chatThreadItems.syncIterator
            while let chatThreadItem = iterator.next() {
                print("Thread id: \(chatThreadItem.id)")
            }
    case .failure:
        print("Failed to list threads")
    }
    semaphore.signal()
}
semaphore.wait()
```

## <a name="get-a-chat-thread-client"></a>Een chat-thread-client ophalen

Nu u een chat-thread hebt gemaakt, kunt u een `ChatThreadClient` voor het uitvoeren van bewerkingen in de thread verkrijgen.

Vervang de opmerking `<GET A CHAT THREAD CLIENT>` door de volgende code:

```
let chatThreadClient = try chatClient.createClient(forThread: threadId!)
```

## <a name="send-a-message-to-a-chat-thread"></a>Een bericht verzenden naar een chat-thread

Vervang de opmerking `<SEND A MESSAGE>` door de volgende code:

```
let message = SendChatMessageRequest(
    content: "Hello!",
    senderDisplayName: "Jack"
)

var messageId: String?

chatThreadClient.send(message: message) { result, _ in
    switch result {
    case let .success(result):
        print("Message sent, message id: \(result.id)")
        messageId = result.id
    case .failure:
        print("Failed to send message")
    }
    semaphore.signal()
}
semaphore.wait()
```

Eerst bouwt u de `SendChatMessageRequest` , die de inhoud en de weergave naam van de afzender bevat. Deze aanvraag kan ook de geschiedenis tijd delen bevatten als u deze wilt opnemen. Het antwoord dat wordt geretourneerd naar de voltooiings-handler bevat de ID van het bericht dat is verzonden.


## <a name="send-a-read-receipt"></a>Een lees bevestiging verzenden

U kunt een lees bevestiging verzenden voor een bepaald bericht door het aanroepen van de `ChatThreadClients` `sendReadReceipt` methode. Vervang de opmerking `<SEND A READ RECEIPT>` door de volgende code:

```
if let id = messageId {
    chatThreadClient.sendReadReceipt(forMessage: id) { result, _ in
        switch result {
        case .success:
            print("Read receipt sent")
        case .failure:
            print("Failed to send read receipt")
        }
        semaphore.signal()
    }
    semaphore.wait()
} else {
    print("Cannot send read receipt without a message id")
}
```

## <a name="receive-chat-messages-from-a-chat-thread"></a>Chatberichten ontvangen van een chat-thread

U kunt berichten van een chat-thread ontvangen door de `listMessages()` methode vanuit te roepen `ChatThreadClient` . Berichten weer geven bevatten systeem berichten en verzonden berichten van de gebruiker. Zie [bericht typen](https://docs.microsoft.com/azure/communication-services/concepts/chat/concepts#message-types) voor meer informatie over de typen berichten die u kunt ontvangen

Vervang de opmerking `<RECEIVE MESSAGES>` door de volgende code:

```
chatThreadClient.listMessages { result, _ in
    switch result {
    case let .success(messages):
        var iterator = messages.syncIterator
        while let message = iterator.next() {
            print("Received message of type \(message.type)")
        }

    case .failure:
        print("Failed to receive messages")
    }
    semaphore.signal()
}
semaphore.wait()
```

## <a name="add-a-user-as-a-participant-to-the-chat-thread"></a>Een gebruiker toevoegen als deel nemer aan de chat thread

Vervang de opmerking `<ADD A USER>` door de volgende code:

```
let user = ChatParticipant(
    id: CommunicationUserIdentifier("<USER_ID>"),
    displayName: "Jane"
)

chatThreadClient.add(participants: [user]) { result, _ in
    switch result {
    case let .success(result):
        (result.invalidParticipants != nil) ? print("Added participant") : print("Error adding participant")
    case .failure:
        print("Failed to add the participant")
    }
    semaphore.signal()
}
semaphore.wait()
```

Vervang door `<USER_ID>` de gebruikers-id van de communicatie services van de gebruiker die u wilt toevoegen.

Wanneer u een deel nemer aan een thread toevoegt, kan het geretourneerde antwoord fouten bevatten. Deze fouten duiden op een fout bij het toevoegen van bepaalde deel nemers.

## <a name="list-users-in-a-thread"></a>Gebruikers in een thread weer geven

Vervang de opmerking `<LIST USERS>` door de volgende code:

```
chatThreadClient.listParticipants { result, _ in
    switch result {
    case let .success(participants):
        var iterator = participants.syncIterator
        while let participant = iterator.next() {
            let user = participant.id as! CommunicationUserIdentifier
            print("User with id: \(user.identifier)")
        }
    case .failure:
        print("Failed to list participants")
    }
    semaphore.signal()
}
semaphore.wait()
```

## <a name="run-the-code"></a>De code uitvoeren

Klik in Xcode op de knop uitvoeren om het project te bouwen en uit te voeren. In de-console kunt u de uitvoer van de code en de logboek uitvoer van de ChatClient weer geven.

