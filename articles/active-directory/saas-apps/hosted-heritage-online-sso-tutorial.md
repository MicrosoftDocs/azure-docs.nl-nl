---
title: 'Zelfstudie: Eenmalige aanmelding van Azure Active Directory integreren met Hosted Heritage Online SSO | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Hosted Heritage Online SSO.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/17/2019
ms.author: jeedes
ms.openlocfilehash: fe09da61f4d87fbef4ce6c8739cd79c7bcda6796
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2021
ms.locfileid: "92442730"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-hosted-heritage-online-sso"></a>Zelfstudie: Eenmalige aanmelding van Azure Active Directory integreren met Hosted Heritage Online SSO

In deze zelfstudie leert u hoe u Hosted Heritage Online SSO kunt integreren met Azure Active Directory (Azure AD). Wanneer u Hosted Heritage Online SSO integreert met Azure AD, kunt u het volgende doen:

* In Azure Active Directory bepalen wie er toegang heeft tot Hosted Heritage Online SSO.
* Ervoor zorgen dat gebruikers zich automatisch met hun Azure AD-account kunnen aanmelden bij Hosted Heritage Online SSO.
* Uw accounts op een centrale locatie beheren: Azure Portal.

Zie [Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?](../manage-apps/what-is-single-sign-on.md) voor meer informatie over de integratie van SaaS-apps met Azure AD.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om aan de slag te gaan:

* Een Azure AD-abonnement Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/).
* Een abonnement op Hosted Heritage Online SSO waarvoor eenmalige aanmelding is ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Hosted Heritage Online SSO biedt ondersteuning voor met **SP** geïnitieerde SSO

## <a name="adding-hosted-heritage-online-sso-from-the-gallery"></a>Hosted Heritage Online SSO toevoegen vanuit de galerie

Voor het configureren van de integratie van Hosted Heritage Online SSO met Microsoft Azure Active Directory moet u Hosted Heritage Online SSO vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het linkernavigatiedeelvenster de service **Azure Active Directory**.
1. Ga naar **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.
1. Selecteer **Nieuwe toepassing** om een nieuwe toepassing toe te voegen.
1. Typ in de sectie **Toevoegen uit de galerie** **Hosted Heritage Online SSO** in het zoekvak.
1. Selecteer **Hosted Heritage Online SSO** in de resultaten en voeg vervolgens de app toe. Wacht enkele seconden tot de app is toegevoegd aan de tenant.

## <a name="configure-and-test-azure-ad-single-sign-on-for-hosted-heritage-online-sso"></a>Eenmalige aanmelding van Azure AD configureren en testen voor Hosted Heritage Online SSO

Configureer en test eenmalige aanmelding van Azure AD met Hosted Heritage Online SSO met behulp van een testgebruiker met de naam **B.Simon**. Eenmalige aanmelding werkt alleen als u een koppelingsrelatie tot stand brengt tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Hosted Heritage Online SSO.

Voltooi de volgende stappen om eenmalige aanmelding van Azure AD met Hosted Heritage Online SSO te configureren en testen:

1. **[Eenmalige aanmelding van Azure AD configureren](#configure-azure-ad-sso)** : zodat uw gebruikers deze functie kunnen gebruiken.
    1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : om eenmalige aanmelding van Azure AD te testen met B.Simon.
    1. **[De Azure AD-testgebruiker toewijzen](#assign-the-azure-ad-test-user)** zodat B.Simon eenmalige aanmelding van Azure AD kan gebruiken.
1. **[Eenmalige aanmelding configureren in Hosted Heritage Online SSO](#configure-hosted-heritage-online-sso-sso)** : als u de instellingen voor eenmalige aanmelding aan de clientzijde wilt configureren.
    1. **[Testgebruiker voor Hosted Heritage Online SSO maken](#create-hosted-heritage-online-sso-test-user)** : als u een tegenhanger van B.Simon in Hosted Heritage Online SSO wilt hebben die is gekoppeld aan de Azure Active Directory-weergave van de gebruiker.
1. **[Eenmalige aanmelding testen](#test-sso)** : om te controleren of de configuratie werkt.

## <a name="configure-azure-ad-sso"></a>Eenmalige aanmelding van Azure AD configureren

Volg deze stappen om eenmalige aanmelding van Azure AD in te schakelen in Azure Portal.

1. Ga in de [Azure-portal](https://portal.azure.com/) op de integratiepagina van de toepassing **Hosted Heritage Online SSO** naar de sectie **Beheren** en selecteer **Eenmalige aanmelding**.
1. Selecteer **SAML** op de pagina **Selecteer een methode voor eenmalige aanmelding**.
1. Op de pagina **Eenmalige aanmelding instellen met SAML** klikt u op het bewerkings-/penpictogram voor **Standaard-SAML-configuratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. In de sectie **Standaard-SAML-configuratie** voert u de waarden in voor de volgende velden:

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<SUBDOMAIN>.cirqahosting.com/Shibboleth.sso/Login`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<SUBDOMAIN>.cirqahosting.com/shibboleth`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en id. Neem contact op met het [klantenondersteuningsteam van Hosted Heritage Online SSO](mailto:support@isoxford.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. Op de pagina **Eenmalige aanmelding instellen met SAML** klikt u in de sectie **SAML-handtekeningcertificaat** op de kopieerknop om de **URL voor federatieve metagegevens van de app** te kopiëren en slaat u deze op uw computer op.

    ![De link om het certificaat te downloaden](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie gaat u een testgebruiker met de naam B.Simon maken in Azure Portal.

1. Selecteer in het linkerdeelvenster van Azure Portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. Volg de volgende stappen bij de eigenschappen voor **Gebruiker**:
   1. Voer in het veld **Naam**`B.Simon` in.  
   1. Voer username@companydomain.extension in het veld **Gebruikersnaam** in. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie geeft u B. Simon toestemming om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen tot Hosted Heritage Online SSO.

1. Selecteer in Azure Portal de optie **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.
1. Selecteer **Hosted Heritage Online SSO** in de lijst met toepassingen.
1. Zoek op de overzichtspagina van de app de sectie **Beheren** en selecteer **Gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![De koppeling Gebruiker toevoegen](common/add-assign-user.png)

1. Selecteer in het dialoogvenster **Gebruikers en groepen** de optie **B.Simon** in de lijst Gebruikers. Klik vervolgens op de knop **Selecteren** onderaan het scherm.
1. Als u een waarde voor een rol verwacht in de SAML-assertie, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren. Klik vervolgens op de knop **Selecteren** onderaan het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

## <a name="configure-hosted-heritage-online-sso-sso"></a>Eenmalige aanmelding configureren in Hosted Heritage Online SSO

Als u eenmalige aanmelding wilt configureren in **Hosted Heritage Online SSO**, moet u de **app-URL voor federatieve metagegevens** verzenden naar het [ondersteuningsteam van Hosted Heritage Online SSO](mailto:support@isoxford.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-hosted-heritage-online-sso-test-user"></a>Testgebruiker maken voor Hosted Heritage Online SSO

In deze sectie maakt u een gebruiker met de naam B.Simon in Hosted Heritage Online SSO. Neem contact op met het [ondersteuningsteam van Hosted Heritage Online SSO](mailto:support@isoxford.com) om de gebruikers toe te voegen in het platform van Hosted Heritage Online SSO. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

## <a name="test-sso"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u in het toegangsvenster op de tegel Hosted Heritage Online SSO klikt, wordt u automatisch aangemeld bij de instantie van Hosted Heritage Online SSO waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](../user-help/my-apps-portal-end-user-access.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende bronnen

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](./tutorial-list.md) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](../manage-apps/what-is-single-sign-on.md) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](../conditional-access/overview.md)

- [Hosted Heritage Online SSO uitproberen met Azure AD](https://aad.portal.azure.com/)