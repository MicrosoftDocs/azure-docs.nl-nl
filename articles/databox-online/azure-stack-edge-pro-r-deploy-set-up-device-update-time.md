---
title: Zelfstudie voor het configureren van tijd-. apparaat- en update-instellingen voor een Azure Stack Edge Pro R-apparaat in Azure Portal
description: In deze zelfstudie voor het implementeren van Azure Stack Edge Pro R krijgt u instructies om apparaat-, update-, en tijdinstellingen voor uw fysieke apparaat te configureren.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/18/2020
ms.author: alkohli
ms.openlocfilehash: 94abfd704d000b907158b2559a268718046b6661
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/31/2021
ms.locfileid: "106059612"
---
# <a name="tutorial-configure-the-device-settings-for-azure-stack-edge-pro-r"></a>Zelfstudie: Apparaatinstellingen configureren voor Azure Stack Edge Pro R

In deze zelfstudie wordt beschreven hoe u apparaatinstellingen voor uw Azure Stack Edge Pro R-apparaat kunt configureren. U kunt de apparaatnaam, update-server en tijdserver instellen via de lokale webinterface.

Het kan 5-7 minuten duren voordat de apparaatinstellingen zijn voltooid.

In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]
>
> * Vereisten
> * Apparaatinstellingen configureren
> * Update configureren 
> * Tijd configureren

## <a name="prerequisites"></a>Vereisten

Voordat u de apparaatinstellingen op uw Azure Stack Edge Pro R-apparaat gaat configureren, dient u het volgende te controleren:

* Voor uw fysieke apparaat:

    - U hebt het fysieke apparaat geïnstalleerd zoals beschreven in [Azure Stack Edge Pro R installeren](azure-stack-edge-pro-r-deploy-install.md).
    - U hebt het netwerk geconfigureerd en het rekennetwerk op uw apparaat geconfigureerd, zoals wordt beschreven in [Zelfstudie: Netwerk configureren voor Azure Stack Edge Pro R ](azure-stack-edge-pro-r-deploy-configure-network-compute-web-proxy.md).


## <a name="configure-device-settings"></a>Apparaatinstellingen configureren

Volg deze stappen voor het configureren van apparaatinstellingen:

1. Voer op de pagina **Apparaat** de volgende stappen uit:

    1. Voer een beschrijvende naam voor het apparaat in. De beschrijvende naam moet tussen 1 en 13 tekens lang zijn en mag alleen letters, cijfers en afbreekstreepjes bevatten.

    2. Geef een **DNS-domein** voor uw apparaat op. Dit domein wordt gebruikt om het apparaat in te stellen als een bestandsserver.

    3. Selecteer **Toepassen** om de geconfigureerde apparaatinstellingen te valideren en toe te passen.

        ![Pagina Apparaat van de lokale webinterface 1](./media/azure-stack-edge-pro-r-deploy-set-up-device-update-time/device-2.png)

        Als u de naam van het apparaat en het DNS-domein hebt gewijzigd, werken de automatisch gegenereerde zelfondertekende certificaten op het apparaat niet. Kies een van de volgende opties wanneer u certificaten configureert: 
        
        - Apparaatcertificaten genereren en downloaden. 
        - Uw eigen certificaten voor het apparaat gebruiken, inclusief de ondertekeningsketen.
    

        ![Pagina Apparaat van de lokale webinterface 2](./media/azure-stack-edge-pro-r-deploy-set-up-device-update-time/device-3.png)

    4. Wanneer de apparaatnaam en het DNS-domein worden gewijzigd, wordt het SMB-eindpunt gemaakt.  

    5. Nadat de instellingen zijn toegepast, selecteert u **Volgende: Server bijwerken**.

        ![Pagina 'Apparaat' van de lokale webinterface 3](./media/azure-stack-edge-pro-r-deploy-set-up-device-update-time/device-4.png)

## <a name="configure-update"></a>Update configureren

1. U kunt nu op de pagina **Bijwerken** de locatie configureren waar u de updates voor uw apparaat kunt downloaden.  

    - U kunt de updates rechtstreeks van de **Microsoft Update-server** ophalen.

        ![Pagina Update-server van lokale webinterface](./media/azure-stack-edge-pro-r-deploy-set-up-device-update-time/update-2.png)

        U kunt er ook voor kiezen om updates te implementeren via **Windows Server Update Services** (WSUS). Geef het pad naar de WSUS-server op.
        
        ![Pagina Update-server van lokale webinterface 2](./media/azure-stack-edge-pro-r-deploy-set-up-device-update-time/update-3.png)

        > [!NOTE] 
        > Als er een afzonderlijke Windows Update-server is geconfigureerd en u ervoor kiest om verbinding te maken via *https* (in plaats van *http*), zijn er certificaten voor de ondertekeningsketen nodig, die vereist zijn om verbinding te maken met de update-server. Ga naar [Certificaten beheren](azure-stack-edge-gpu-manage-certificates.md) voor meer informatie over het maken en uploaden van certificaten.         
        > Als u een niet-verbonden modus wilt gebruiken, bijvoorbeeld wanneer uw Azure Stack Edge-apparaatlagen in combinatie met MDC (Modular Data Center) worden gebruikt, schakelt u de optie WSUS in. Tijdens de activering wordt door het apparaat gescand op updates. Als de server niet is ingesteld, mislukt de activering. 


2. Selecteer **Toepassen**.
3. Nadat de update-server is geconfigureerd, selecteert u **Volgende: Tijd**.
    

## <a name="configure-time"></a>Tijd configureren

Volg deze stappen voor het configureren van tijdinstellingen op uw apparaat. 

> [!IMPORTANT]
> Hoewel de tijdinstellingen optioneel zijn, raden we u ten zeerste aan dat u een primaire NTP-server en een secundaire NTP-server op het lokale netwerk voor uw apparaat configureert. Als de lokale server niet beschikbaar is, kunnen openbare NTP-servers worden geconfigureerd.

NTP-servers zijn vereist, omdat uw apparaat de tijd moet synchroniseren voor verificatie met uw cloudserviceproviders.

1. Op de pagina **Tijd** kunt u de tijdzone en de primaire en secundaire NTP-servers voor uw apparaat selecteren.  
    
    1. Selecteer in de vervolgkeuzelijst **Tijdzone** de tijdzone die overeenkomt met de geografische locatie waar het apparaat wordt geïmplementeerd.
        De standaard tijdzone voor uw apparaat is PST. Het apparaat zal deze tijdzone gebruiken voor alle geplande bewerkingen.

    2. Voer in het vak **Primaire NTP-server** de primaire server voor uw apparaat in of accepteer de standaardwaarde van time.windows.com.  
        Zorg ervoor dat in uw netwerk NTP-verkeer kan worden doorgegeven van uw datacenter naar internet.

    3. Voer desgewenst in het vak **secundaire NTP-server** een secundaire server in voor uw apparaat.

    4. Selecteer **Toepassen** om de geconfigureerde tijdinstellingen te valideren en toe te passen.

        ![Pagina Tijd van lokale webinterface](./media/azure-stack-edge-pro-r-deploy-set-up-device-update-time/time-2.png)

2. Nadat de instellingen zijn toegepast, selecteert u **Volgende: Certificaten**.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]
>
> * Vereisten
> * Apparaatinstellingen configureren
> * Update configureren 
> * Tijd configureren

Als u wilt weten hoe u certificaten voor uw Azure Stack Edge Pro R-apparaat kunt configureren, raadpleegt u:

> [!div class="nextstepaction"]
> [Certificaten configureren](./azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption.md)
