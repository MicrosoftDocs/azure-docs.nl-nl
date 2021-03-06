---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 04/05/2021
ms.author: mimart
ms.openlocfilehash: 5a2382146cd8b85b8eef54b924a206dda7107b0f
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2021
ms.locfileid: "106382454"
---
#### <a name="app-registrations"></a>[App-registraties](#tab/app-reg-ga/) 

1. Selecteer onder **Beheren** de optie **API-machtigingen**.
1. Selecteer onder **Geconfigureerde machtigingen** de optie **Een machtiging toevoegen**.
1. Selecteer het tabblad **micro soft api's** en selecteer vervolgens **Microsoft Graph**.
1. Selecteer **Toepassingsmachtigingen**.
1. Vouw de juiste machtigings groep uit en schakel het selectie vakje in van de machtiging om aan uw beheer toepassing toe te kennen. Bijvoorbeeld:
    * **Gebruiker**  >  **User. readwrite. all**: voor gebruikers migratie of gebruikers beheer scenario's.
    * **Groep**  >  **Group. readwrite. all**: voor het maken van groepen, het lezen en bijwerken van groepslid maatschappen en het verwijderen van groepen.
    * **AuditLog**  >  **AuditLog. Read. all**: voor het lezen van de controle logboeken van de map.
    * **Beleid**  >  **Policy. readwrite. TrustFramework**: voor continue integratie/continue levering (CI/cd)-scenario's. Bijvoorbeeld aangepaste beleids implementatie met Azure-pijp lijnen.
1. Selecteer **Machtigingen toevoegen**. Wacht, zoals aangegeven, een paar minuten voordat u verdergaat met de volgende stap.
1. Selecteer **Beheerderstoestemming verlenen voor (naam van uw tenant)** .
1. Als u momenteel niet bent aangemeld met het account van de globale beheerder, meldt u zich aan met een account in uw Azure AD B2C-Tenant waaraan ten minste de rol van *Cloud toepassings beheerder* is toegewezen en selecteert u **beheerder toestemming geven voor (uw Tenant naam)**.
1. Selecteer **vernieuwen** en controleer vervolgens of ' verleend voor... ' wordt weer gegeven onder **status**. Het kan enkele minuten duren voordat de machtigingen zijn doorgegeven.

#### <a name="applications-legacy"></a>[Toepassingen (verouderd)](#tab/applications-legacy/)

1. Op de overzichts pagina van de **geregistreerde app** selecteert u **instellingen**.
1. Selecteer onder **API-toegang** de optie **vereiste machtigingen**.
1. Selecteer **Microsoft Graph**.
1. Schakel onder **toepassings machtigingen** het selectie vakje in van de machtiging om aan uw beheer toepassing toe te kennen. Bijvoorbeeld:
    * **Alle audit logboek gegevens lezen**: Selecteer deze machtiging om de controle logboeken van de map te lezen.
    * **Directory gegevens lezen en schrijven**: Selecteer deze machtiging voor gebruikers migratie of scenario's voor gebruikers beheer.
    * **Het vertrouwens raamwerk beleid van uw organisatie lezen en schrijven**: Selecteer deze machtiging voor continue integratie/doorlopende levering (CI/cd)-scenario's. Bijvoorbeeld aangepaste beleids implementatie met Azure-pijp lijnen.
1. Selecteer **Opslaan**.
1. Selecteer **machtigingen verlenen** en selecteer vervolgens **Ja**. Het kan enkele minuten duren voordat de machtigingen volledig zijn door gegeven.
