---
title: Migreren naar MSAL.NET
titleSuffix: Microsoft identity platform
description: Meer informatie over de verschillen tussen de Microsoft Authentication Library voor .NET (MSAL.NET) en Azure AD Authentication Library for .NET (ADAL.NET) en hoe u migreert naar MSAL.NET.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/10/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: devx-track-csharp, aaddev
ms.openlocfilehash: 0e7dc3540dc54e0563a5ea416510bddb9a41fb65
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2021
ms.locfileid: "107861693"
---
# <a name="migrating-applications-to-msalnet"></a>Toepassingen migreren naar MSAL.NET

Zowel de Microsoft Authentication Library voor .NET (MSAL.NET) als de Azure AD Authentication Library voor .NET (ADAL.NET) worden gebruikt voor het verifiëren van Azure AD-entiteiten en aanvraagtokens van Azure AD. Tot nu toe hebben de meeste ontwikkelaars met het Azure AD for Developers-platform (v1.0) gewerkt om Azure AD-identiteiten (werk- en schoolaccounts) te verifiëren door tokens aan te vragen met behulp van Azure AD Authentication Library (ADAL). MSAL gebruiken:

- U kunt een bredere set Microsoft-identiteiten verifiëren (Azure AD-identiteiten en Microsoft-accounts, en sociale en lokale accounts via Azure AD B2C) omdat het gebruikmaakt van het Microsoft Identity Platform,
- uw gebruikers krijgen de beste ervaring voor een een aanmelding.
- uw toepassing kan incrementele toestemming inschakelen en het ondersteunen van voorwaardelijke toegang is eenvoudiger
- u profiteert van de innovatie.

**MSAL.NET of Microsoft.Identity.Web zijn** nu de aanbevolen auth-bibliotheken voor gebruik met het Microsoft Identity Platform. Er worden geen nieuwe functies geïmplementeerd op ADAL.NET. De inspanningen zijn gericht op het verbeteren van MSAL.

In dit artikel worden de verschillen beschreven tussen de Microsoft Authentication Library for .NET (MSAL.NET) en Azure AD Authentication Library for .NET (ADAL.NET) en helpt u bij het migreren naar MSAL.

## <a name="should-you-migrate-to-msalnet-or-to-microsoftidentityweb"></a>Moet u migreren naar MSAL.NET of naar Microsoft.Identity.Web

Voordat u de details van MSAL.NET versus ADAL.NET bekijkt, kunt u controleren of u MSAL.NET of een abstractie op een hoger niveau wilt gebruiken, zoals [Microsoft.Identity.Web](microsoft-identity-web.md)

Lees Voor meer informatie over de beslissingsstructuur hieronder, moet [ik alleen MSAL.NET gebruiken? of een abstractie op een hoger niveau?](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Is-MSAL.NET-right-for-me%3F)

:::image type="content" source="media/msal-net-migration/decision-diagram.png" alt-text="Blokdiagram waarin wordt uitgelegd hoe u kunt kiezen of u MSAL.NET en Microsoft.Identity.Web of beide moet gebruiken bij het migreren van ADAL.NET":::

## <a name="differences-between-adal-and-msal-apps"></a>Verschillen tussen ADAL- en MSAL-apps

In de meeste gevallen wilt u MSAL.NET microsoft identity platform gebruiken. Dit is de nieuwste generatie microsoft-verificatiebibliotheken. Met MSAL.NET verkrijgt u tokens voor gebruikers die zich aanmelden bij uw toepassing met Azure AD (werk- en schoolaccounts), Microsoft(personal) accounts (MSA) of Azure AD B2C.

Als u al bekend bent met het Azure AD-eindpunt voor ontwikkelaars (v1.0) (en ADAL.NET), kunt u lezen Wat is er anders aan het [Microsoft Identity Platform?](../azuread-dev/azure-ad-endpoint-comparison.md).

U moet echter nog steeds ADAL.NET als uw toepassing gebruikers met eerdere versies van Active Directory Federation Services [(ADFS) moet aanmelden.](/windows-server/identity/active-directory-federation-services) Zie [ADFS-ondersteuning voor meer informatie.](https://aka.ms/msal-net-adfs-support)

In de volgende afbeelding ziet u enkele van de verschillen tussen ADAL.NET en MSAL.NET voor een openbare clienttoepassing Naast ![ elkaar code](./media/msal-compare-msaldotnet-and-adaldotnet/differences.png)

### <a name="nuget-packages-and-namespaces"></a>NuGet-pakketten en -naamruimten

ADAL.NET wordt gebruikt vanuit het [NuGet-pakket Microsoft.IdentityModel.Clients.ActiveDirectory.](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) de naamruimte die moet worden gebruikt, is `Microsoft.IdentityModel.Clients.ActiveDirectory` .

Als u MSAL.NET, moet u het NuGet-pakket [Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client) toevoegen en de `Microsoft.Identity.Client` naamruimte gebruiken

### <a name="scopes-not-resources"></a>Scopes, geen resources

ADAL.NET verkrijgt tokens voor *resources*, maar MSAL.NET haalt tokens voor *scopes op.* Voor een aantal MSAL.NET AcquireToken-overschrijvingen is een parameter met de naam scopes( `IEnumerable<string> scopes` vereist. Deze parameter is een eenvoudige lijst met tekenreeksen die de gewenste machtigingen en resources declareeren die worden aangevraagd. Bekende scopes zijn de Microsoft Graph van de Microsoft Graph van [de toepassing.](/graph/permissions-reference)

Het is ook mogelijk om in MSAL.NET toegang te krijgen tot v1.0-resources. Zie De details in [Scopes for a v1.0 application (Scopes voor een v1.0-toepassing).](#scopes-for-a-web-api-accepting-v10-tokens)

### <a name="core-classes"></a>Kernklassen

- ADAL.NET [gebruikt AuthenticationContext](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/AuthenticationContext:-the-connection-to-Azure-AD) als de weergave van uw verbinding met de STS (Security Token Service) of autorisatieserver, via een instantie. Daarentegen is MSAL.NET ontworpen rond [clienttoepassingen.](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Client-Applications) Het biedt twee afzonderlijke klassen: `PublicClientApplication` en `ConfidentialClientApplication`

- Tokens verkrijgen: ADAL.NET en MSAL.NET dezelfde verificatie-aanroepen ( en voor ADAL.NET, en in MSAL.NET), maar met verschillende `AcquireTokenAsync` `AcquireTokenSilentAsync` parameters `AcquireTokenInteractive` `AcquireTokenSilent` vereist. Een verschil is dat u, in MSAL.NET, niet meer de van uw toepassing hoeft door te geven bij elke `ClientID` AcquireTokenXX-aanroep. Inderdaad, de `ClientID` wordt slechts één keer ingesteld bij het bouwen van de ( of `IPublicClientApplication` `IConfidentialClientApplication` ).

### <a name="iaccount-not-iuser"></a>IAccount niet IUser

ADAL.NET gemanipuleerde gebruikers. Een gebruiker is echter een persoon of een softwareagent, maar kan eigenaar/eigenaar/verantwoordelijk zijn voor een of meer accounts in het Microsoft-identiteitssysteem (verschillende Azure AD-accounts, Azure AD B2C, persoonlijke Microsoft-accounts).

MSAL.NET 2.x definieert nu het concept van Account (via de IAccount-interface). Deze wijziging die een wijziging doorbreekt, biedt de juiste semantiek: het feit dat dezelfde gebruiker meerdere accounts kan hebben, in verschillende Azure AD-directorieën. Ook MSAL.NET betere informatie in gastscenario's, omdat er informatie over het thuisaccount wordt verstrekt.

Zie voor meer informatie over de verschillen tussen IUser en IAccount [MSAL.NET 2.x.](https://aka.ms/msal-net-2-released)

### <a name="exceptions"></a>Uitzonderingen

#### <a name="interaction-required-exceptions"></a>Vereiste uitzonderingen voor interactie

MSAL.NET heeft meer expliciete uitzonderingen. Als bijvoorbeeld stille verificatie mislukt in ADAL, is de procedure om de uitzondering te ondervangen en te zoeken naar `user_interaction_required` de foutcode:

```csharp
catch(AdalException exception)
{
 if (exception.ErrorCode == "user_interaction_required")
 {
  try
  {“try to authenticate interactively”}}
 }
}
```

Zie Details in [Het aanbevolen patroon voor het verkrijgen van een token](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/AcquireTokenSilentAsync-using-a-cached-token#recommended-pattern-to-acquire-a-token) met ADAL.NET

Met MSAL.NET vangt u `MsalUiRequiredException` op zoals beschreven in [AcquireTokenSilent.](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AcquireTokenSilentAsync-using-a-cached-token)

```csharp
catch(MsalUiRequiredException exception)
{
 try {“try to authenticate interactively”}
}
```

#### <a name="handling-claim-challenge-exceptions"></a>Uitzonderingen voor claim-uitdaging afhandelen

In ADAL.NET worden claim-vraag-uitzonderingen op de volgende manier verwerkt:

- `AdalClaimChallengeException` is een uitzondering (afgeleid van ) die door de service wordt veroorzaakt voor het geval een resource meer claims van de gebruiker vereist (bijvoorbeeld verificatie `AdalServiceException` met twee factoren). Het `Claims` lid bevat een JSON-fragment met de claims, die worden verwacht.
- In ADAL.NET moet de openbare clienttoepassing die deze uitzondering ontvangt, de overschrijven aanroepen met `AcquireTokenInteractive` een claimparameter. Met deze overschrijven van `AcquireTokenInteractive` wordt niet eens geprobeerd de cache te raken, omdat dit niet nodig is. De reden hiervoor is dat het token in de cache niet de juiste claims heeft (anders zou er `AdalClaimChallengeException` geen zijn geworpen). Daarom is het niet nodig om naar de cache te kijken. Houd er rekening mee dat de kan worden ontvangen in een WebAPI die OBO doet, terwijl de moet worden aangeroepen in een openbare clienttoepassing die `ClaimChallengeException` `AcquireTokenInteractive` deze web-API aanroept.
- zie Handling [AdalClaimChallengeException (AdalClaimChallengeException verwerken) voor meer informatie, inclusief voorbeelden](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Exceptions-in-ADAL.NET#handling-adalclaimchallengeexception)

In MSAL.NET worden claim-vraag-uitzonderingen op de volgende manier verwerkt:

- De `Claims` worden aan de oppervlakte genomen in de `MsalServiceException` .
- Er is een `.WithClaim(claims)` methode die kan worden toegepast op de `AcquireTokenInteractive` opbouwer.

### <a name="supported-grants"></a>Ondersteunde toekenningen

Niet alle toekenningen worden nog ondersteund in MSAL.NET en het v2.0-eindpunt. Hier volgt een samenvatting waarin de ADAL.NET en MSAL worden vergeleken. Door NET ondersteunde toekenningen.

#### <a name="public-client-applications"></a>Openbare clienttoepassingen

Hier worden de toekenningen ondersteund in ADAL.NET en MSAL.NET desktop- en mobiele toepassingen

Verlenen | ADAL.NET | MSAL.NET
----- |----- | -----
Interactief | [Interactieve auth](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Acquiring-tokens-interactively---Public-client-application-flows) | [Interactief tokens verkrijgen in MSAL.NET](scenario-desktop-acquire-token.md?tabs=dotnet#acquire-a-token-interactively)
Geïntegreerde Windows-authenticatie | [Geïntegreerde verificatie in Windows (Kerberos)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/AcquireTokenSilentAsync-using-Integrated-authentication-on-Windows-(Kerberos)) | [Geïntegreerde Windows-verificatie](scenario-desktop-acquire-token.md?tabs=dotnet#integrated-windows-authentication)
Gebruikersnaam en wachtwoord | [Tokens ophalen met gebruikersnaam en wachtwoord](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Acquiring-tokens-with-username-and-password)| [Verificatie van gebruikersnaamwachtwoord](scenario-desktop-acquire-token.md?tabs=dotnet#username-and-password)
Stroom voor apparaatcode | [Apparaatprofiel voor apparaten zonder webbrowsers](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Device-profile-for-devices-without-web-browsers) | [Stroom apparaatcode](scenario-desktop-acquire-token.md?tabs=dotnet#command-line-tool-without-a-web-browser)

#### <a name="confidential-client-applications"></a>Vertrouwelijke clienttoepassingen

Hier vindt u de toekenningen die worden ondersteund in ADAL.NET, MSAL.NET en Microsoft.Identity.Web voor webtoepassingen, web-API's en daemontoepassingen:

Type of App | Verlenen | ADAL.NET | MSAL.NET
----- | ----- | ----- | -----
Web-app, web-API, daemon | Clientreferenties | [Clientreferentiestromen in ADAL.NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Client-credential-flows) | [Clientreferentiestromen in MSAL.NET](scenario-daemon-acquire-token.md?tabs=dotnet#acquiretokenforclient-api)
Web-API | Namens | [Service-naar-service-aanroepen namens de gebruiker met ADAL.NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Service-to-service-calls-on-behalf-of-the-user) | [Namens in MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/on-behalf-of)
Web-app | Auth Code | [Tokens met autorisatiecodes verkrijgen voor web-apps met ADAL.NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Acquiring-tokens-with-authorization-codes-on-web-apps) | [Tokens met autorisatiecodes verkrijgen voor web-apps met A MSAL.NET](scenario-web-app-call-api-acquire-token.md?tabs=aspnetcore)

### <a name="cache-persistence"></a>Cache-persistentie

ADAL.NET kunt u de klasse uitbreiden om de gewenste persistentiefunctionaliteit te implementeren op platforms zonder een beveiligde opslag (.NET Framework en .NET Core) met behulp van `TokenCache` de methoden , en `BeforeAccess` `BeforeWrite` . Zie Serialisatie [van tokencache in ADAL.NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization).

MSAL.NET maakt van de tokencache een verzegelde klasse, waardoor het niet meer mogelijk is om het uit te breiden. Daarom moet uw implementatie van tokencache-persistentie de vorm hebben van een helperklasse die communiceert met de verzegelde tokencache. Deze interactie wordt beschreven in [Serialisatie van tokencache in MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/token-cache-serialization).

## <a name="signification-of-the-common-authority"></a>Signification of the common authority (Aanmelding van de gemeenschappelijke autoriteit)

Als u in v1.0 de instantie gebruikt, kunnen gebruikers zich aanmelden met een `https://login.microsoftonline.com/common` AAD-account (voor elke organisatie). Zie [Validatie van autoriteit in ADAL.NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/AuthenticationContext:-the-connection-to-Azure-AD#authority-validation)

Als u de instantie in v2.0 gebruikt, kunnen gebruikers zich aanmelden met een AAD-organisatie of een persoonlijk `https://login.microsoftonline.com/common` Microsoft-account (MSA). Als MSAL.NET aanmelding wilt beperken tot een AAD-account (hetzelfde gedrag als bij ADAL.NET), gebruikt u `https://login.microsoftonline.com/organizations` . Zie de parameter in openbare `authority` clienttoepassing voor [meer informatie.](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Client-Applications#publicclientapplication)

## <a name="v10-and-v20-tokens"></a>v1.0- en v2.0-tokens

Er zijn twee versies van tokens:
- v1.0-tokens
- v2.0-tokens

Het v1.0-eindpunt (gebruikt door ADAL) stuurt alleen v1.0-tokens.

Het v2.0-eindpunt (gebruikt door MSAL) stuurt echter de versie van het token dat de web-API accepteert. Een eigenschap van het toepassingsmanifest van de web-API stelt ontwikkelaars in staat om te kiezen welke versie van het token wordt geaccepteerd. Zie `accessTokenAcceptedVersion` de [naslagdocumentatie over het toepassingsmanifest.](reference-app-manifest.md)

Zie toegangstokens voor meer informatie over v1.0- en [v2.0 Azure Active Directory tokens](access-tokens.md)

## <a name="scopes-for-a-web-api-accepting-v10-tokens"></a>Scopes voor een web-API die v1.0-tokens accepteert

OAuth2-machtigingen zijn machtigingsbereiken die een v1.0-web-API-toepassing (resource) beschikbaar maakt voor clienttoepassingen. Deze machtigingsbereiken kunnen worden verleend aan clienttoepassingen tijdens de toestemming. Zie de sectie over oauth2Permissions in [Azure Active Directory toepassingsmanifest](./reference-app-manifest.md).

### <a name="scopes-to-request-access-to-specific-oauth2-permissions-of-a-v10-application"></a>Bereiken voor het aanvragen van toegang tot specifieke OAuth2-machtigingen van een v1.0-toepassing

Als u tokens wilt verkrijgen voor een toepassing die v1.0-tokens accepteert (bijvoorbeeld de Microsoft Graph-API, dat wil zeggen , moet u maken door een gewenste resource-id samen tevoegen met een gewenste https://graph.microsoft.com) `scopes` OAuth2-machtiging voor die resource.

Als u bijvoorbeeld in de naam van de gebruiker een v1.0-web-API wilt openen die de URI van de app-id is, wilt u `ResourceId` het volgende gebruiken:

```csharp
var scopes = new [] { ResourceId+"/user_impersonation" };
```

Als u wilt lezen en schrijven met MSAL.NET Azure Active Directory met behulp van de Microsoft Graph API ( , maakt u een lijst met https://graph.microsoft.com/) scopes, zoals in het volgende fragment:

```csharp
string ResourceId = "https://graph.microsoft.com/"; 
string[] scopes = { ResourceId + "Directory.Read", ResourceId + "Directory.Write" }
```

#### <a name="warning-should-you-have-one-or-two-slashes-in-the-scope-corresponding-to-a-v10-web-api"></a>Waarschuwing: Moet u een of twee slashes in het bereik hebben die overeenkomen met een v1.0-web-API

Als u het bereik wilt schrijven dat overeenkomt met de Azure Resource Manager API ( , vraagt u het volgende https://management.core.windows.net/) bereik aan (let op de twee slashes).

```csharp
var scopes = new[] {"https://management.core.windows.net//user_impersonation"};
var result = await app.AcquireTokenInteractive(scopes).ExecuteAsync();

// then call the API: https://management.azure.com/subscriptions?api-version=2016-09-01
```

Dit komt doordat de Resource Manager-API een slash verwacht in de doelgroepclaim ( ), en er vervolgens een slash is om de API-naam van het bereik `aud` te scheiden.

De logica die door Azure AD wordt gebruikt, is als volgt:
- Voor ADAL-eindpunt (v1.0) met een v1.0-toegangsteken (de enige mogelijke), aud=resource
- Voor MSAL (v2.0-eindpunt) waarin een toegangstoken wordt gevraagd voor een resource die v2.0-tokens accepteert, aud=resource. Appid
- Voor MSAL (v2.0-eindpunt) waarbij een toegangsteken wordt gevraagd voor een resource die een v1.0-toegangsteken accepteert (dit is het bovenstaande geval), parseert Azure AD de gewenste doelgroep uit het aangevraagde bereik door alles vóór de laatste slash te nemen en dit te gebruiken als de resource-id. Dus als https: /database.windows.net een doelgroep verwacht van " ", moet u een bereik van \/ https://database.windows.net/ https: \/ /database.windows.net//.default. Zie ook probleem #[747:](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747)De slash van de resource-URL wordt weggelaten, waardoor de sql-auth-fout is #747


### <a name="scopes-to-request-access-to-all-the-permissions-of-a-v10-application"></a>Bereiken voor het aanvragen van toegang tot alle machtigingen van een v1.0-toepassing

Als u bijvoorbeeld een token wilt verkrijgen voor alle statische scopes van een v1.0-toepassing, moet u

```csharp
ResourceId = "someAppIDURI";
var scopes = new [] { ResourceId+"/.default" };
```

### <a name="scopes-to-request-in-the-case-of-client-credential-flow--daemon-app"></a>Te aanvragen scopes in het geval van clientreferentiestroom/daemon-app

In het geval van clientreferentiestroom is het bereik dat moet worden doorgeven ook `/.default` . Dit bereik vertelt Aan Azure AD: 'alle machtigingen op app-niveau waar de beheerder toestemming voor heeft gegeven in de registratie van de toepassing.

## <a name="adal-to-msal-migration"></a>Migratie van ADAL naar MSAL

In ADAL.NET v2. X, de vernieuwingstokens zijn beschikbaar gemaakt, zodat u oplossingen kunt ontwikkelen voor het gebruik van deze tokens door ze in de caching op te nemen en de methoden van `AcquireTokenByRefreshToken` ADAL 2.x te gebruiken.
Sommige van deze oplossingen zijn gebruikt in scenario's zoals:
* Langlopende services die acties uitvoeren, waaronder het vernieuwen van dashboards namens de gebruikers, terwijl de gebruikers niet meer verbonden zijn.
* WebFarm-scenario's voor het inschakelen van de client om de RT naar de webservice te brengen (caching wordt uitgevoerd aan clientzijde, versleutelde cookie en niet serverzijde)

MSAL.NET geeft geen vernieuwingstokens weer, uit veiligheidsoverwegingen: MSAL verwerkt het vernieuwen van tokens voor u.

Gelukkig beschikt MSAL.NET nu over een API waarmee u uw vorige vernieuwingstokens (verkregen met ADAL) kunt migreren naar `IConfidentialClientApplication` de :

```csharp
/// <summary>
/// Acquires an access token from an existing refresh token and stores it and the refresh token into
/// the application user token cache, where it will be available for further AcquireTokenSilent calls.
/// This method can be used in migration to MSAL from ADAL v2 and in various integration
/// scenarios where you have a RefreshToken available.
/// (see https://aka.ms/msal-net-migration-adal2-msal2)
/// </summary>
/// <param name="scopes">Scope to request from the token endpoint.
/// Setting this to null or empty will request an access token, refresh token and ID token with default scopes</param>
/// <param name="refreshToken">The refresh token from ADAL 2.x</param>
IByRefreshToken.AcquireTokenByRefreshToken(IEnumerable<string> scopes, string refreshToken);
```

Met deze methode kunt u het eerder gebruikte vernieuwings token samen met de wenste scopes (resources) leveren. Het vernieuwings token wordt uitgewisseld voor een nieuw token en in de cache van uw toepassing opgeslagen.

Omdat deze methode is bedoeld voor scenario's die niet gebruikelijk zijn, is deze niet direct toegankelijk met de zonder eerst `IConfidentialClientApplication` te casten naar `IByRefreshToken` .

Dit codefragment toont een deel van de migratiecode in een vertrouwelijke clienttoepassing. `GetCachedRefreshTokenForSignedInUser` het vernieuwingstoken ophalen dat is opgeslagen in een opslagruimte door een eerdere versie van de toepassing die werd gebruikt om gebruik te maken van ADAL 2.x. `GetTokenCacheForSignedInUser` deserialiseert een cache voor de aangemelde gebruiker (aangezien vertrouwelijke clienttoepassingen één cache per gebruiker moeten hebben).

```csharp
TokenCache userCache = GetTokenCacheForSignedInUser();
string rt = GetCachedRefreshTokenForSignedInUser();

IConfidentialClientApplication app;
app = ConfidentialClientApplicationBuilder.Create(clientId)
 .WithAuthority(Authority)
 .WithRedirectUri(RedirectUri)
 .WithClientSecret(ClientSecret)
 .Build();
IByRefreshToken appRt = app as IByRefreshToken;

AuthenticationResult result = await appRt.AcquireTokenByRefreshToken(null, rt)
                                         .ExecuteAsync()
                                         .ConfigureAwait(false);
```

Er worden een toegangsteken en id-token geretourneerd in uw AuthenticationResult terwijl uw nieuwe vernieuwingstoken wordt opgeslagen in de cache.

U kunt deze methode ook gebruiken voor verschillende integratiescenario's waarin u een vernieuwings token beschikbaar hebt.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de scopes vindt u in [Scopes, permissions, and consent (Scopes, machtigingen en toestemming) in het Microsoft Identity Platform](v2-permissions-and-consent.md)
