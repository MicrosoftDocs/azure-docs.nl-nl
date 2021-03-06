---
title: Problemen met Azure route Server oplossen
description: Meer informatie over het oplossen van problemen met Azure route server.
services: route-server
author: duongau
ms.service: route-server
ms.topic: how-to
ms.date: 03/15/2021
ms.author: duau
ms.openlocfilehash: 83f1e83653c5674988cadcb5b54d3c675ae0b8b8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2021
ms.locfileid: "103489437"
---
# <a name="troubleshooting-azure-route-server-issues"></a>Problemen met Azure route Server oplossen

> [!IMPORTANT]
> Azure route server (preview) is momenteel beschikbaar als open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="connectivity-issues"></a>Connectiviteitsproblemen

### <a name="why-does-my-nva-lose-internet-connectivity-after-it-advertises-the-default-route-00000-to-azure-route-server"></a>Waarom verliest mijn NVA de Internet verbinding nadat de standaard route (0.0.0.0/0) is geadverteerd naar Azure route server?
Wanneer uw NVA de standaard route adverteert, stuurt Azure route server dit programma voor alle virtuele machines in het virtuele netwerk, met inbegrip van de NVA zelf. Deze standaard route stelt de NVA in als de volgende hop voor al het Internet-gebonden verkeer. Als uw NVA Internet verbinding nodig heeft, moet u een door de [gebruiker gedefinieerde route](../virtual-network/virtual-networks-udr-overview.md) configureren om deze standaard route van de NVA te overschrijven en de UDR te koppelen aan het subnet waar de NVA wordt gehost (Zie het onderstaande voor beeld). Anders zal de NVA-hostcomputer het Internet verkeer dat wordt verzonden door de NVA terug naar de NVA zelf verzenden.

| Route | Volgende hop |
|-------|----------|
| 0.0.0.0/0 | Internet |


### <a name="why-can-i-ping-from-my-nva-to-the-bgp-peer-ip-on-azure-route-server-but-after-i-set-up-the-bgp-peering-between-them-i-cant-ping-the-same-ip-anymore-why-does-the-bgp-peering-go-down"></a>Waarom kan ik pingen van mijn NVA naar het IP-adres van de BGP-peer op de Azure-route server, maar nadat ik de BGP-peering heb ingesteld, kan ik niet meer hetzelfde IP-adres pingen? Waarom gaat de BGP-peering omlaag?

In sommige NVA moet u een statische route toevoegen voor het Azure route server-subnet. Als bijvoorbeeld Azure route server zich in 10.0.255.0/27 bevindt en uw NVA zich in 10.0.1.0/24 bevindt, moet u de volgende route toevoegen aan de routerings tabel in de NVA:

| Route | Volgende hop |
|-------|----------|
| 10.0.255.0/27 | 10.0.1.1 |

10.0.1.1 is het standaard gateway-IP-adres in het subnet waar uw NVA (of meer nauw keuriger, een van de Nic's) wordt gehost.

### <a name="why-do-i-lose-connectivity-to-my-on-premises-network-over-expressroute-andor-azure-vpn-when-im-deploying-azure-route-server-to-a-virtual-network-that-already-has-expressroute-gateway-andor-azure-vpn-gateway"></a>Waarom kan ik geen verbinding meer maken met mijn on-premises netwerk via ExpressRoute en/of Azure VPN wanneer ik Azure route server Implementeer naar een virtueel netwerk dat al ExpressRoute gateway en/of Azure VPN-gateway heeft?
Wanneer u Azure route server implementeert in een virtueel netwerk, moeten we het besturings vlak tussen de gateways en het virtuele netwerk bijwerken. Tijdens deze update is er een periode waarin de Vm's in het virtuele netwerk geen verbinding met het on-premises netwerk kunnen hebben. We raden u ten zeerste aan een onderhoud te plannen voor het implementeren van Azure route server in uw productie omgeving.  

## <a name="control-plane-issues"></a>Problemen met beheer vlak

### <a name="why-is-the-bgp-peering-between-my-nva-and-the-azure-route-server-going-up-and-down-flapping"></a>Waarom is de BGP-peering tussen mijn NVA en de Azure route server actief of omlaag ("gaat en neer")?

De oorzaak van de gaat en neer kan zijn vanwege de BGP-timer instelling. De Keep-Alive timer op Azure route server is standaard ingesteld op 60 seconden en de timer voor vasthouden is 180 seconden.

### <a name="why-does-my-nva-not-receive-routes-from-azure-route-server-even-though-the-bgp-peering-is-up"></a>Waarom worden er door mijn NVA geen routes van de Azure route server ontvangen, zelfs als de BGP-peering actief is?

Het ASN dat door Azure route server wordt gebruikt, is 65515. Zorg ervoor dat u een andere ASN voor uw NVA configureert, zodat er een ' eBGP-sessie kan worden ingesteld tussen uw NVA-en Azure-route server, zodat route doorgifte automatisch kan plaatsvinden. Zorg ervoor dat u ' multi-hop ' inschakelt in de BGP-configuratie omdat uw NVA en Azure route server zich in verschillende subnetten in het virtuele netwerk bevinden.

### <a name="the-bgp-peering-between-my-nva-and-azure-route-server-is-up-i-can-see-routes-exchanged-correctly-between-them-why-arent-the-nva-routes-in-the-effective-routing-table-of-my-vm"></a>De BGP-peering tussen mijn NVA en Azure route server is actief. Ik kan tussen de routes die naar behoren zijn uitgewisseld. Waarom worden de NVA-routes niet vermeld in de doel matige routerings tabel van mijn VM? 

* Als uw virtuele machine zich in hetzelfde VNet bevindt als uw NVA en Azure route server:

     Azure route server biedt twee BGP-peer Ip's die worden gehost op twee virtuele machines die de verantwoordelijkheid voor het verzenden van de routes delen naar alle andere virtuele machines die in uw virtuele netwerk worden uitgevoerd. Elk van uw NVA moet twee identieke BGP-sessies instellen (bijvoorbeeld hetzelfde nummer gebruiken, hetzelfde pad en dezelfde set routes aankondigen) op de twee virtuele machines zodat uw Vm's in het virtuele netwerk consistente routerings gegevens kunnen verkrijgen van Azure route server. Zie het onderstaande diagram.

    ![Diagram waarin een virtueel netwerk apparaat met route server wordt weer gegeven.](./media/faq/network-virtual-appliances.png)

    Als u twee of meer exemplaren van de NVA hebt, *kunt* u verschillende NVA-instanties adverteren voor dezelfde route als u een NVA-exemplaar wilt aanwijzen als actief en het andere passief.

* Als uw virtuele machine zich in een ander virtueel netwerk bevindt dan de VM die als host fungeert voor uw NVA en Azure route server. Controleer of VNet-peering is ingeschakeld tussen de twee VNets *en* of externe route server gebruiken is ingeschakeld op het VNet van de virtuele machine.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [configureren van een Azure route server](quickstart-configure-route-server-powershell.md)
