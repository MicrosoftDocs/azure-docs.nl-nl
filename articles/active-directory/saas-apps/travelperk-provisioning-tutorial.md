---
title: 'Zelf studie: TravelPerk configureren voor het automatisch inrichten van gebruikers met Azure Active Directory | Microsoft Docs'
description: Meer informatie over het automatisch inrichten en ongedaan maken van de inrichting van gebruikers accounts van Azure AD naar TravelPerk.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: 3e40f87d-8624-4b14-b098-80ff916103c3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2021
ms.author: Zhchia
ms.openlocfilehash: 19436b7faef081757e4500c76e7537ee78081bfa
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2021
ms.locfileid: "104950377"
---
# <a name="tutorial-configure-travelperk-for-automatic-user-provisioning"></a>Zelf studie: TravelPerk configureren voor automatische gebruikers inrichting

In deze zelf studie worden de stappen beschreven die u moet uitvoeren in zowel TravelPerk als Azure Active Directory (Azure AD) voor het configureren van automatische gebruikers inrichting. Wanneer de configuratie is geconfigureerd, worden gebruikers en groepen door Azure AD automatisch ingericht en ongedaan gemaakt op [TravelPerk](https://www.travelperk.com/) met behulp van de Azure AD-inrichtings service. Zie voor belangrijke details over wat deze service doet, hoe het werkt en veelgestelde vragen [Inrichting en ongedaan maken van inrichting van gebruikers automatiseren naar SaaS-toepassingen met Azure Active Directory](../app-provisioning/user-provisioning.md).

## <a name="capabilities-supported"></a>Ondersteunde mogelijkheden

> [!div class="checklist"]
>
> - Gebruikers maken in TravelPerk
> - Gebruikers in TravelPerk verwijderen wanneer ze niet meer toegang nodig hebben
> - Gebruikers kenmerken gesynchroniseerd laten tussen Azure AD en TravelPerk
> - [Eenmalige aanmelding](./travelperk-tutorial.md) bij TravelPerk (aanbevolen)

## <a name="prerequisites"></a>Vereisten

In het scenario dat in deze zelfstudie wordt beschreven, wordt ervan uitgegaan dat u al beschikt over de volgende vereisten:

- [Een Azure AD-tenant](../develop/quickstart-create-new-tenant.md).
- Een gebruikersaccount in Azure AD met [machtigingen](../roles/permissions-reference.md) voor het configureren van de inrichting (bijvoorbeeld toepassingsbeheerder, cloud-toepassingsbeheerder, toepassingseigenaar of globale beheerder).
- Een actief [TravelPerk](https://app.travelperk.com/signup) -beheerders account.
- Een Premium/Pro- [abonnement](https://www.travelperk.com/pricing/).


## <a name="step-1-plan-your-provisioning-deployment"></a>Stap 1. Implementatie van de inrichting plannen

1. Lees [hoe de inrichtingsservice werkt](../app-provisioning/user-provisioning.md).
2. Bepaal wie u wilt opnemen in het [bereik voor inrichting](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
3. Bepaal welke gegevens moeten worden [toegewezen tussen Azure AD en TravelPerk](../app-provisioning/customize-application-attributes.md).

## <a name="step-2-configure-travelperk-to-support-provisioning-with-azure-ad"></a>Stap 2. TravelPerk configureren voor ondersteuning bij het inrichten met Azure AD

1. Meld u aan bij de [TravelPerk](https://app.travelperk.com/company/integrations/scim) -toepassing met uw beheerders account.

2. Navigeren naar de  >  **scim integraties**  >   van bedrijfs instellingen

3. Klik op **scim-API inschakelen**

   ![Inschakelen](./media/travelperk-provisioning-tutorial/configuration.png)

4. U kunt ook goed keuringen inschakelen via SCIM. Met goed keuringen kunt u extra governance instellen door de opgegeven goed keurders eerst te laten bepalen dat de TRIPS worden goedgekeurd. Meer informatie hierover vindt u [hier](https://support.travelperk.com/hc/en-us/articles/360044168971-How-do-approval-processes-work-).

5. U kunt opgeven of u wilt dat de Manager van elke persoon automatisch de gebruiker wordt die verantwoordelijk is voor de goed keuring van reizen. Daarom wordt er een fiatteur toegewezen in het bijbehorende automatische goedkeurings proces. TravelPerk wijst de **Manager** -waarde van Azure toe aan de gewenste fiatteur van de gebruiker. De gebruiker moet bestaan op het platform voordat de ingerichte gebruikers fiatteur wordt.
Goed keurders worden niet gemaakt als ze niet juist zijn geconfigureerd op TravelPerk.

6. Automatisch goedkeurings proces maken is beschikbaar in de **scim-instellingen** nadat scim is ingeschakeld op de pagina integraties. Als u deze wilt inschakelen, selecteert u **een id-provider** en schakelt u de wissel knop **automatisch goedkeurings proces maken** in.

7. Klik op **wijzigingen opslaan** zodra het vereiste goedkeurings proces is geconfigureerd.

   ![Automatiseren](./media/travelperk-provisioning-tutorial/approval.png)

## <a name="step-3-add-travelperk-from-the-azure-ad-application-gallery"></a>Stap 3. TravelPerk toevoegen vanuit de Azure AD-toepassings galerie

Voeg TravelPerk toe vanuit de Azure AD-toepassings galerie om het beheren van de inrichting van TravelPerk te starten. Als u eerder TravelPerk voor SSO hebt ingesteld, kunt u dezelfde toepassing gebruiken. U wordt echter aangeraden een afzonderlijke app te maken wanneer u de integratie voor het eerst test. Klik [hier](../manage-apps/add-application-portal.md) voor meer informatie over het toevoegen van een toepassing uit de galerie.

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Stap 4. Definiëren wie u wilt opnemen in het bereik voor inrichting

Met de Azure AD-inrichtingsservice kunt u bepalen wie worden ingericht op basis van toewijzing aan de toepassing en/of op basis van kenmerken van de gebruiker/groep. Als u ervoor kiest om te bepalen wie wordt ingericht voor uw app op basis van toewijzing, kunt u de volgende [stappen](../manage-apps/assign-user-or-group-access-portal.md) gebruiken om gebruikers en groepen aan de toepassing toe te wijzen. Als u ervoor kiest om uitsluitend te bepalen wie wordt ingericht op basis van kenmerken van de gebruiker of groep, kunt u een bereikfilter gebruiken zoals [hier](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) wordt beschreven.

- Wanneer u gebruikers toewijst aan TravelPerk, moet u een andere rol dan **standaard toegang** selecteren. Gebruikers met de rol Standaardtoegang worden uitgesloten van inrichting en worden gemarkeerd als niet-effectief gerechtigd in de inrichtingslogboeken. Als Standaardtoegang de enige beschikbare rol voor de toepassing is, kunt u [het manifest van de toepassing bijwerken](../develop/howto-add-app-roles-in-azure-ad-apps.md) om extra rollen toe te voegen.

- Begin klein. Test de toepassing met een kleine set gebruikers en groepen voordat u de toepassing naar iedereen uitrolt. Wanneer het bereik voor inrichting is ingesteld op toegewezen gebruikers en groepen, kunt u dit beheren door een of twee gebruikers of groepen aan de app toe te wijzen. Wanneer het bereik is ingesteld op alle gebruikers en groepen, kunt u een [bereikfilter op basis van kenmerken](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) opgeven.

## <a name="step-5-configure-automatic-user-provisioning-to-travelperk"></a>Stap 5. Automatische gebruikers inrichting configureren voor TravelPerk

In deze sectie wordt u begeleid bij de stappen voor het configureren van de Azure AD-inrichtingsservice om gebruikers en/of groepen in TestApp te maken, bij te werken en uit te schakelen op basis van gebruikers- en/of groepstoewijzingen in Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-travelperk-in-azure-ad"></a>Automatische gebruikers inrichting configureren voor TravelPerk in azure AD:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com). Selecteer **Bedrijfstoepassingen** en vervolgens **Alle toepassingen**.

   ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **TravelPerk** in de lijst met apps.

   ![De koppeling TravelPerk in de lijst met toepassingen](common/all-applications.png)

3. Selecteer het tabblad **Inrichten**.

   ![Tabblad Inrichting](common/provisioning.png)

4. Stel de **Inrichtingsmodus** in op **Automatisch**.

   ![De inrichtingsmodus ingesteld op Automatisch](common/provisioning-automatic.png)

5. Klik onder de sectie **Referenties voor beheerder** op **Autoriseren**. U wordt omgeleid naar de aanmeldings pagina van **TravelPerk**. Voer uw **gebruikers naam** en **wacht woord** in en klik op de knop **Aanmelden** . Klik op **app autoriseren** op de pagina autorisatie. Klik op **verbinding testen** om te controleren of Azure AD verbinding kan maken met TravelPerk. Als de verbinding mislukt, zorg er dan voor dat uw SecureLogin-account beheerders machtigingen heeft en probeer het opnieuw.

   ![Referenties voor beheerder](./media/travelperk-provisioning-tutorial/authorize.png)

   ![Welkom](./media/travelperk-provisioning-tutorial/login.png)

   ![Access](./media/travelperk-provisioning-tutorial/authorization.png)

6. Voer in het veld **E-mailadres voor meldingen** het e-mailadres in van een persoon of groep die de inrichtingsfoutmeldingen zou moeten ontvangen en schakel het selectievakje **Een e-mailmelding verzenden als een fout optreedt** in.

   ![E-mailadres voor meldingen](common/provisioning-notification-email.png)

7. Selecteer **Opslaan**.

8. Selecteer in de sectie **toewijzingen** de optie **Azure Active Directory gebruikers synchroniseren met TravelPerk**.

9. Controleer de gebruikers kenmerken die zijn gesynchroniseerd vanuit Azure AD naar TravelPerk in de sectie **kenmerk toewijzing** . De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt om te voldoen aan de gebruikers accounts in TravelPerk voor bijwerk bewerkingen. Als u ervoor kiest om het [overeenkomende doel kenmerk](../app-provisioning/customize-application-attributes.md)te wijzigen, moet u ervoor zorgen dat de TRAVELPERK-API het filteren van gebruikers op basis van dat kenmerk ondersteunt. Selecteer de knop **Opslaan** om eventuele wijzigingen door te voeren.

   | Kenmerk                                                                         | Type      | Ondersteund voor filteren |
   | --------------------------------------------------------------------------------- | --------- | ----------------------- |
   | userName                                                                          | Tekenreeks    | &check;                 |
   | externalId                                                                        | Tekenreeks    |
   | actief                                                                            | Booleaans   |
   | name.honorificPrefix                                                              | Tekenreeks    |
   | name.familyName                                                                   | Tekenreeks    |
   | name.givenName                                                                    | Tekenreeks    |
   | name.middleName                                                                   | Tekenreeks    |
   | preferredLanguage                                                                 | Tekenreeks    |
   | landinstelling                                                                            | Tekenreeks    |
   | phoneNumbers[type eq "work"].value                                                | Tekenreeks    |
   | externalId                                                                        | Tekenreeks    |
   | title                                                                             | Tekenreeks    |
   | urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:costCenter             | Tekenreeks    |
   | urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager                | Naslaginformatie |
   | urn: IETF: params: scim: schemas: extension: travelperk: 2.0: User: gender                 | Tekenreeks    |
   | urn: IETF: params: scim: schemas: extensie: travelperk: 2.0: gebruiker: dateOfBirth            | Tekenreeks    |
   | urn: IETF: params: scim: schemas: extensie: travelperk: 2.0: gebruiker: invoiceProfiles        | Matrix     |
   | urn:IETF:params:scim:schemas:extension:travelperk: 2.0:User:emergencyContact. name  | Tekenreeks    |
   | urn: IETF: params: scim: schemas: extension: travelperk: 2.0: gebruiker: emergencyContact. Phone | Tekenreeks    |
   | urn: IETF: params: scim: schemas: extensie: travelperk: 2.0: gebruiker: travelPolicy           | Tekenreeks    |

10. Als u bereikfilters wilt configureren, raadpleegt u de volgende instructies in de [zelfstudie Bereikfilter](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

11. Als u de Azure AD-inrichtings service voor **TravelPerk wilt inschakelen, wijzigt u de** **inrichtings status** in in het gedeelte **instellingen** .

    ![Inrichtingsstatus ingeschakeld](common/provisioning-toggle-on.png)

12. Definieer de gebruikers en/of groepen die u wilt inrichten voor TravelPerk door de gewenste waarden in het **bereik** te kiezen in de sectie **instellingen** .

    ![Inrichtingsbereik](common/provisioning-scope.png)

13. Wanneer u klaar bent om in te richten, klikt u op **Opslaan**.

    ![Inrichtingsconfiguratie opslaan](common/provisioning-configuration-save.png)

Met deze bewerking wordt de eerste synchronisatiecyclus gestart van alle gebruikers en groepen die zijn gedefinieerd onder **Bereik** in de sectie **Instellingen**. De initiële cyclus duurt langer dan volgende cycli, die ongeveer om de 40 minuten plaatsvinden zolang de Azure AD-inrichtingsservice wordt uitgevoerd.

## <a name="step-6-monitor-your-deployment"></a>Stap 6. Uw implementatie bewaken

Nadat u het inrichten hebt geconfigureerd, gebruikt u de volgende resources om uw implementatie te bewaken:

1. Gebruik de [inrichtingslogboeken](../reports-monitoring/concept-provisioning-logs.md) om te bepalen welke gebruikers al dan niet met succes zijn ingericht
2. Controleer de [voortgangsbalk](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) om de status van de inrichtingscyclus weer te geven en te zien of deze al bijna is voltooid
3. Als het configureren van de inrichting een foutieve status lijkt te hebben, wordt de toepassing in quarantaine geplaatst. [Klik hier](../app-provisioning/application-provisioning-quarantine-status.md) voor meer informatie over quarantainestatussen.

## <a name="additional-resources"></a>Aanvullende resources

- [Gebruikersaccountinrichting voor zakelijke apps beheren](../app-provisioning/configure-automatic-user-provisioning-portal.md)
- [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over het controleren van logboeken en het ophalen van rapporten over de inrichtingsactiviteit](../app-provisioning/check-status-user-account-provisioning.md)