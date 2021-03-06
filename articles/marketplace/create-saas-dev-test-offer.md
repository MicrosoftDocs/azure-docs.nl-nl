---
title: Een test aanbieding maken
description: Een afzonderlijke ontwikkelings aanbieding maken voor het testen van uw productie aanbod in het Commercial Marketplace-programma in micro soft Partner Center.
author: mingshen-ms
ms.author: mingshen
ms.reviewer: dannyevers
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
ms.date: 03/25/2021
ms.openlocfilehash: 893d38d7dcf2ef0910bc46d3e9bfd168c2a89162
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2021
ms.locfileid: "105543360"
---
# <a name="create-a-test-offer"></a>Een test aanbieding maken

Als u vanuit uw productie aanbieding een afzonderlijke omgeving wilt ontwikkelen, maakt u een afzonderlijke aanbieding voor testen en ontwikkeling (ontwikkel aars) en een afzonderlijke productie-aanbieding (PROD). Zie [een SaaS-aanbieding plannen](plan-saas-offer.md#test-offer)voor meer informatie over de voor delen van het gebruik van een afzonderlijke ontwikkel aanbieding.

U configureert de meeste instellingen in de ontwikkel-en productie aanbiedingen. De officiële marketing taal en-assets, zoals scherm afbeeldingen en logo's, moeten bijvoorbeeld hetzelfde zijn. In de gevallen waarin de configuratie hetzelfde is, kunt u velden kopiëren en plakken van de plannen in de ontwikkel aanbieding naar de plannen in de productie-aanbieding.

In de volgende secties worden de configuratie verschillen tussen de ontwikkel-en productie aanbiedingen beschreven.

## <a name="offer-setup-page"></a>Pagina aanbieding instellen

U kunt het beste dezelfde alias gebruiken in het vak **alias** van beide aanbiedingen, maar u kunt ' _test ' toevoegen aan de alias van de ontwikkelings aanbieding. Als de alias van uw productie-aanbieding bijvoorbeeld ' contososolution ' is, moet de alias van de ontwikkel aanbieding ' contososolution_test ' zijn. Op deze manier kunt u gemakkelijk identificeren welk ontwikkel aanbod van uw productie aanbod is.

Gebruik in de sectie **klant leads** een Azure-tabel of een test-CRM-omgeving voor de ontwikkelings aanbieding. Gebruik het beoogde beheer systeem voor leads voor de productie-aanbieding.

## <a name="properties-page"></a>De pagina Eigenschappen

Configureer deze pagina hetzelfde in de aanbiedingen DEV en PROD.

## <a name="offer-listing-page"></a>Aanbiedings pagina

Configureer deze pagina hetzelfde in de aanbiedingen DEV en PROD.

## <a name="preview-audience"></a>Voor beeld van doel groep

Neem in de ontwikkel aanbieding de Azure Active Directory (AAD)-e-mail adressen van gebruikers principals of Microsoft-account (MSA) op van ontwikkel aars en testers, met inbegrip van uzelf. De user principal name van een gebruiker op AAD kan verschillen van het e-mail adres van die gebruiker. jane.doe@contoso.comWerkt bijvoorbeeld niet, maar wel janedoe@contoso.com . De gebruikers die u opgeeft, hebben toegang tot de ontwikkel aanbieding wanneer u de **Preview** -koppeling tijdens de ontwikkelings-en test fase deelt.

Neem in de aanbieding voor de productie order de Azure AD-user principal name of het micro soft-account-e-mail van de gebruikers op die de aanbieding valideren voordat u de **knop Live go** selecteert om de Live-aanbieding te publiceren.

## <a name="technical-configuration-page"></a>Pagina technische configuratie

In deze tabel worden de verschillen beschreven tussen de instellingen voor ontwikkel aanbiedingen en productie aanbiedingen.

***Tabel 1: verschillen in de technische configuratie***

| Instelling | Ontwikkel aanbieding | PRODUCTIE aanbod |
| ------------ | ------------- | ------------- |
| URL van landings pagina | Voer het dev/test-eind punt in. | Voer uw productie-eind punt in. |
| Verbindings-webhook | Voer het dev/test-eind punt in. | Voer uw productie-eind punt in. |
| Tenant-ID Azure Active Directory | Voer de Tenant-ID voor de registratie van de test-app (AAD-Directory-ID) in. | Voer uw Tenant-ID voor de registratie van de productie-app in. |
| Azure Active Directory-toepassings-id | Voer de toepassings-ID voor de registratie van de test-app (client-ID) in. | Voer de toepassings-ID voor de registratie van uw productie-app in. |
||||

## <a name="plan-overview-page"></a>Overzichts pagina voor het abonnement

Wanneer u uw plannen maakt, raden we u aan dezelfde _plan-id_ en _plan naam_ te gebruiken in de ontwikkel-en productie aanbiedingen, met uitzonde ring van de plan-id in de ontwikkel aanbieding met **_test**. Als de plan-ID in de productie-aanbieding bijvoorbeeld ' onderneming ' is, moet de plan-ID in het ontwikkel aanbod ' enterprise_test ' zijn. Op deze manier kunt u gemakkelijk identificeren welk ontwikkel aanbod van uw productie aanbod is. U maakt plannen in de productie-aanbieding met de prijs modellen en prijzen die u kiest voor uw aanbieding.

### <a name="plan-listing"></a>Aanbieding plannen

Voer op het tabblad plan **Overview**  >  **plan** overzicht dezelfde plan beschrijving in de ontwikkel-en productie plannen in.

### <a name="pricing-and-availability-page"></a>Pagina prijs informatie en beschik baarheid

Deze sectie bevat richt lijnen voor het volt ooien van de **overzichts**  >  **prijs en de beschikbaarheids** pagina van het plan.

#### <a name="markets"></a>Landen

Selecteer dezelfde markten voor de ontwikkel-en productie aanbiedingen.

#### <a name="pricing"></a>Prijzen

Gebruik de ONTWIKKELINGs aanbieding om te experimenteren met prijs modellen. Nadat u hebt gecontroleerd welk prijs model of welke modellen het beste werken, maakt u de plannen in de productie-aanbieding met de gewenste prijs modellen en prijzen.

De ONTWIKKELINGs aanbieding moet plannen hebben met nul of lage prijzen in de plannen. De productie-aanbieding heeft de prijzen die u aan klanten wilt kosten.

> [!IMPORTANT]
> Aankopen die in de preview-versie zijn gemaakt, worden verwerkt voor ontwikkel-en productie aanbiedingen. Als een aanbieding een prijs van $100/mo heeft, wordt $100 in rekening gebracht voor uw bedrijf. Als dit het geval is, kunt u een [ondersteunings ticket](support.md) openen. er wordt een uitbetaling voor het volledige bedrag uitgegeven (en er worden geen Store-service kosten in rekening gebracht).

#### <a name="pricing-model"></a>Prijsmodel

Gebruik hetzelfde prijs model in de plannen van de ontwikkel-en productie aanbiedingen. Als het plan in de productie-aanbieding bijvoorbeeld vast tarief is, met een maandelijkse facturerings termijn, configureert u het plan in de ontwikkel aanbieding met hetzelfde model.

Voor het verlagen van de kosten voor het testen van de prijs modellen, inclusief aangepaste meet waarde voor Marketplace, raden we u aan de **prijs** sectie van het tabblad **prijzen en beschik baarheid** te configureren in de ontwikkelings aanbieding met lagere prijzen dan de productie-aanbieding. Hieronder vindt u enkele richt lijnen die u kunt volgen bij het instellen van prijzen voor plannen in de ONTWIKKELINGs aanbieding.

***Tabel 2: richt lijnen voor prijzen***

| Prijs | Opmerking |
| ------------ | ------------- |
| $0,00 | Stel een totale transactie kosten in op nul om geen financiële gevolgen te ondervinden. Gebruik deze prijs bij het aanroepen van de meter-Api's of voor het testen van inkoop plannen in uw aanbieding tijdens het ontwikkelen van uw oplossing. |
| $0,01-$49,99 | Gebruik dit prijs bereik om analyse, rapportage en het aankoop proces te testen. |
| $50,00 en hoger | Gebruik dit prijs bereik om de uitbetaling te testen. Zie voor meer informatie over ons betalings schema [uitbetalings schema's en-processen](/partner-center/payout-policy-details). |
|||

Open een [ondersteunings ticket](support.md)om te voor komen dat u rekening moet houden met de kosten van een Store-service voor uw test.

#### <a name="free-trial"></a>Gratis proefversie

Schakel geen gratis proef versie in voor de ontwikkel aanbieding.

## <a name="co-sell-with-microsoft-page"></a>Samen verkopen met micro soft-pagina

Configureer het tabblad **samen met micro soft** niet voor de ontwikkelings aanbieding.

## <a name="resell-through-csps"></a>Doorverkopen via CSP's

Configureer het tabblad **verkopen via csp's** van de ontwikkelings aanbieding niet.

## <a name="next-steps"></a>Volgende stappen

- [Een SaaS-aanbieding testen en publiceren](test-publish-saas-offer.md)
