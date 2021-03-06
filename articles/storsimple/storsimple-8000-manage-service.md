---
title: De StorSimple Apparaatbeheer-service in azure implementeren | Microsoft Docs
description: Meer informatie over de stappen die nodig zijn voor het maken, verwijderen, migreren van de service en het beheer van de service registratie sleutel.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: alkohli
ms.openlocfilehash: 85d66b5e4f7e95d114cbf0b4c681d73eebb93a67
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/31/2021
ms.locfileid: "106076561"
---
# <a name="deploy-the-storsimple-device-manager-service-for-storsimple-8000-series-devices"></a>De StorSimple Apparaatbeheer-service voor StorSimple 8000 Series-apparaten implementeren

[!INCLUDE [storsimple-8000-eol-banner](../../includes/storsimple-8000-eol-banner.md)]

## <a name="overview"></a>Overzicht

De StorSimple-Apparaatbeheer-service wordt uitgevoerd in Microsoft Azure en er wordt verbinding gemaakt met meerdere StorSimple-apparaten. Nadat u de service hebt gemaakt, kunt u deze gebruiken voor het beheren van alle apparaten die zijn verbonden met de StorSimple-Apparaatbeheer service vanaf één centrale locatie, waardoor de administratieve belasting wordt geminimaliseerd.

In deze zelf studie worden de stappen beschreven die nodig zijn voor het maken, verwijderen, migreren van de service en het beheer van de service registratie sleutel. De informatie in dit artikel is alleen van toepassing op apparaten uit de StorSimple 8000-serie. Ga voor meer informatie over virtuele StorSimple-matrices naar [Deploy a StorSimple Apparaatbeheer service voor uw StorSimple Virtual array](storsimple-virtual-array-manage-service.md).

> [!NOTE]
> -  Het Azure Portal ondersteunt apparaten met Update 5,0 of hoger. Als uw apparaat niet up-to-date is, installeert u update 5 onmiddellijk. Ga voor meer informatie naar [Update 5 installeren](storsimple-8000-install-update-5.md). 
> - Als u een StorSimple Cloud Appliance (8010/8020) gebruikt, kunt u een Cloud apparaat niet bijwerken. Gebruik de nieuwste versie van de software om een nieuw Cloud apparaat met Update 5,0 te maken en ga vervolgens naar het nieuwe Cloud apparaat dat is gemaakt. 
> - Op alle apparaten waarop update 4,0 of eerder wordt uitgevoerd, wordt de beheer functionaliteit gereduceerd. 

## <a name="create-a-service"></a>Een service maken
Als u een StorSimple Apparaatbeheer-service wilt maken, moet u het volgende hebben:

* Een abonnement met een Enterprise Overeenkomst
* Een actief Microsoft Azure Storage-account
* De facturerings gegevens die worden gebruikt voor toegangs beheer

Alleen de abonnementen met een Enterprise Overeenkomst zijn toegestaan. U kunt er ook voor kiezen om een standaard-opslag account te genereren wanneer u de service maakt.

Eén service kan meerdere apparaten beheren. Een apparaat kan echter niet meerdere services omvatten. Een grote onderneming kan meerdere service-exemplaren gebruiken om te werken met verschillende abonnementen, organisaties of zelfs implementatie locaties. 

> [!NOTE]
> U hebt afzonderlijke instanties van StorSimple Apparaatbeheer service nodig voor het beheren van StorSimple 8000 Series-apparaten en StorSimple virtuele matrices.

Voer de volgende stappen uit om een service te maken.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-8000-create-new-service.md)]


Voor elke StorSimple-Apparaatbeheer service bestaan de volgende kenmerken:

* **Naam** : de naam die is toegewezen aan uw StorSimple-Apparaatbeheer service toen deze werd gemaakt. **De service naam kan niet worden gewijzigd nadat de service is gemaakt. Dit geldt ook voor andere entiteiten, zoals apparaten, volumes, volume containers en back-upbeleiden waarvan de naam niet kan worden gewijzigd in de Azure Portal.**
* **Status** : de status van de service, die **actief**, **maken** of **online** kan zijn.
* **Locatie** : de geografische locatie waar het StorSimple-apparaat wordt geïmplementeerd.
* **Abonnement** : het facturerings abonnement dat is gekoppeld aan uw service.

## <a name="delete-a-service"></a>Een service verwijderen

Voordat u een service verwijdert, moet u ervoor zorgen dat er geen aangesloten apparaten worden gebruikt. Als de service wordt gebruikt, deactiveert u de verbonden apparaten. Met de bewerking deactiveren wordt de verbinding tussen het apparaat en de service verbroken, maar blijven de apparaatgegevens in de Cloud behouden.

> [!IMPORTANT]
> De bewerking kan niet ongedaan worden gemaakt nadat een service is verwijderd. Elk apparaat dat de service gebruikt, moet opnieuw worden ingesteld op de fabrieks instellingen voordat het kan worden gebruikt met een andere service. In dit scenario gaan de lokale gegevens op het apparaat, evenals de configuratie, verloren.

Voer de volgende stappen uit om een service te verwijderen.

### <a name="to-delete-a-service"></a>Een service verwijderen

1. Zoek de service die u wilt verwijderen. Klik op het pictogram **resources** en voer vervolgens de juiste voor waarden in om te zoeken. Klik in de zoek resultaten op de service die u wilt verwijderen.

    ![Zoek service die u wilt verwijderen](./media/storsimple-8000-manage-service/deletessdevman1.png)

2. Hiermee gaat u naar de Blade StorSimple Apparaatbeheer service. Klik op **Verwijderen**.

    ![Service verwijderen](./media/storsimple-8000-manage-service/deletessdevman2.png)

3. Klik op **Ja** in het bevestigings bericht. Het kan enkele minuten duren voordat de service is verwijderd.

    ![Verwijdering bevestigen](./media/storsimple-8000-manage-service/deletessdevman3.png)

## <a name="get-the-service-registration-key"></a>De serviceregistratiesleutel ophalen

Nadat u een service hebt gemaakt, moet u uw StorSimple-apparaat registreren bij de service. Als u uw eerste StorSimple-apparaat wilt registreren, hebt u de service registratie sleutel nodig. Als u extra apparaten wilt registreren met een bestaande StorSimple-service, hebt u zowel de registratie sleutel als de versleutelings sleutel voor service gegevens nodig (die tijdens de registratie wordt gegenereerd op het eerste apparaat). Zie [StorSimple Security](storsimple-8000-security.md)(Engelstalig) voor meer informatie over de versleutelings sleutel voor service gegevens. U kunt de registratie sleutel ophalen met behulp van **sleutels** op uw StorSimple Apparaatbeheer-Blade.

Voer de volgende stappen uit om de service registratie sleutel op te halen.

[!INCLUDE [storsimple-8000-get-service-registration-key](../../includes/storsimple-8000-get-service-registration-key.md)]

Behoud de service registratie sleutel op een veilige locatie. U hebt deze sleutel nodig, evenals de versleutelings sleutel voor service gegevens, om extra apparaten te registreren bij deze service. Nadat u de service registratie sleutel hebt verkregen, moet u uw apparaat configureren via de Windows PowerShell voor StorSimple-interface.

Zie voor meer informatie over het gebruik van deze registratie sleutel [stap 3: het apparaat configureren en registreren via Windows PowerShell voor StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-the-service-registration-key"></a>De service registratie sleutel opnieuw genereren
Als u een sleutel rotatie moet uitvoeren of als de lijst met service beheerders is gewijzigd, moet u een service registratie sleutel opnieuw genereren. Wanneer u de sleutel opnieuw genereert, wordt de nieuwe sleutel alleen gebruikt voor het registreren van volgende apparaten. De apparaten die al zijn geregistreerd, worden niet beïnvloed door dit proces.

Voer de volgende stappen uit om een service registratie sleutel opnieuw te genereren.

### <a name="to-regenerate-the-service-registration-key"></a>De service registratie sleutel opnieuw genereren
1. Ga op de Blade **StorSimple Apparaatbeheer** naar **beheer &gt;** **sleutels**.
    
    ![Ga naar de Blade sleutels](./media/storsimple-8000-manage-service/regenregkey2.png)

2. Klik op de Blade **sleutels** op **opnieuw genereren**.

    ![Klik op opnieuw genereren](./media/storsimple-8000-manage-service/regenregkey3.png)
3. Controleer op de Blade **service registratie sleutel opnieuw genereren** de actie die vereist is wanneer de sleutels opnieuw worden gegenereerd. Op alle volgende apparaten die zijn geregistreerd bij deze service wordt de nieuwe registratie sleutel gebruikt. Klik op **opnieuw genereren** om te bevestigen. U wordt gewaarschuwd nadat het opnieuw genereren is voltooid.

    ![Opnieuw genereren bevestigen](./media/storsimple-8000-manage-service/regenregkey4.png)

4. Er wordt een nieuwe service registratie sleutel weer gegeven.

5. Kopieer deze sleutel en sla deze op voor het registreren van nieuwe apparaten bij deze service.



## <a name="change-the-service-data-encryption-key"></a>De versleutelings sleutel voor service gegevens wijzigen
[!INCLUDE [storage-files-migration-generate-key](../../includes/storage-files-migration-generate-key.md)]

## <a name="supported-operations-on-devices-running-versions-prior-to-update-50"></a>Ondersteunde bewerkingen op apparaten met versies voorafgaand aan update 5,0
In de Azure Portal worden alleen de StorSimple-apparaten met Update 5,0 en hoger ondersteund. De apparaten waarop oudere versies worden uitgevoerd, hebben beperkte ondersteuning. Nadat u naar de Azure Portal hebt gemigreerd, gebruikt u de volgende tabel om te begrijpen welke bewerkingen worden ondersteund op apparaten met versies voorafgaand aan update 5,0.

| Bewerking                                                                                                                       | Ondersteund      |
|---------------------------------------------------------------------------------------------------------------------------------|----------------|
| Een apparaat registreren                                                                                                               | Yes            |
| Apparaatinstellingen configureren, zoals algemeen, netwerk en beveiliging                                                                | Yes            |
| Updates scannen, downloaden en installeren                                                                                             | Yes            |
| Apparaat deactiveren                                                                                                               | Yes            |
| Apparaat verwijderen                                                                                                                   | Yes            |
| Een volume container maken, wijzigen en verwijderen                                                                                   | No             |
| Een volume maken, wijzigen en verwijderen                                                                                             | No             |
| Een back-upbeleid maken, wijzigen en verwijderen                                                                                      | No             |
| Een hand matige back-up maken                                                                                                            | No             |
| Een geplande back-up maken                                                                                                         | Niet van toepassing |
| Herstellen vanuit een back-upset                                                                                                        | No             |
| Klonen naar een apparaat met update 3,0 en hoger <br> Op het bron apparaat wordt versie uitgevoerd vóór het bijwerken van 3,0.                                | Yes            |
| Klonen naar een apparaat met versies voorafgaand aan update 3,0                                                                          | No             |
| Failover als bron apparaat <br> (vanaf een apparaat met versie vóór het bijwerken van 3,0 naar een apparaat met update 3,0 of hoger)                                                               | Yes            |
| Failover als doel apparaat <br> (op een apparaat met software versie vóór update 3,0)                                                                                   | No             |
| Een waarschuwing wissen                                                                                                                  | Yes            |
| Back-upbeleid, back-upcatalogus, volumes, volume containers, bewakings grafieken, taken en waarschuwingen die zijn gemaakt in de klassieke portal weer geven | Yes            |
| Apparaat-controllers in-of uitschakelen                                                                                              | Ja            |


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [implementatie proces van StorSimple](storsimple-8000-deployment-walkthrough-u2.md).
* Meer informatie over [het beheren van uw StorSimple-opslag account](storsimple-8000-manage-storage-accounts.md).
* Meer informatie over [het gebruik van de StorSimple Apparaatbeheer-service voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).
