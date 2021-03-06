---
title: Dv2- en DSv2-serie - Azure Virtual Machines
description: Specificaties voor de VM's uit de Dv2- en Dsv2-serie.
author: joelpelley
ms.service: virtual-machines
ms.subservice: vm-sizes-general
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: 71a4cebcc11657566f65f53508df18efe822c409
ms.sourcegitcommit: db925ea0af071d2c81b7f0ae89464214f8167505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/15/2021
ms.locfileid: "107514781"
---
# <a name="dv2-and-dsv2-series"></a>Dv2- en DSv2-serie

De Dv2- en DSv2-serie, een vervolg op de oorspronkelijke D-serie, hebben een krachtigere CPU en optimale configuratie van CPU naar geheugen, waardoor ze geschikt zijn voor de meeste productieworkloads. De Dv2-serie is ongeveer 35% sneller dan de D-serie. De Dv2-serie wordt uitgevoerd op Intel® Xeon® Graf 8272CL (Cascade Lake), Intel® Xeon® 8171M 2,1 GHz (Skylake), Intel® Xeon® E5-2673 v4 2,3 GHz (Broadwell) of de Intel® Xeon® E5-2673 v3 2,4 GHz (Haswell)-processors met intel Turbo Boost Technology 2.0. De Dv2-serie heeft dezelfde geheugen- en schijfconfiguraties als de D-serie.

## <a name="dv2-series"></a>Dv2-serie

Grootten uit de Dv2-serie worden uitgevoerd op Intel® Xeon® 8272CL (Cascade Lake), Intel® Xeon® 8171M 2,1 GHz (Skylake) of de Intel® Xeon ® E5-2673 v4 2,3 GHz (Broadwell) of de Intel® Xeon® E5-2673 v3 2,4 GHz (Haswell)-processors met Intel Turbo Boost Technology 2.0.

[ACU](acu.md): 210-250<br>
[Premium Storage:](premium-storage-performance.md)niet ondersteund<br>
[Premium Storage in de caching:](premium-storage-performance.md)niet ondersteund<br>
[Livemigratie:](maintenance-and-updates.md)ondersteund<br>
[Updates met geheugenbehoud:](maintenance-and-updates.md)ondersteund<br>
[VM-generatieondersteuning:](generation-2.md)generatie 1<br>
[Versneld netwerken:](../virtual-network/create-vm-accelerated-networking-cli.md)ondersteund (*vereist minimaal 2 vCPU's*)<br>
[Kortstondige besturingssysteemschijven:](ephemeral-os-disks.md)niet ondersteund <br>
<br>

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Maximale tijdelijke opslagdoorvoer: IOPS/Read MBps/Write MBps | Max. aantal gegevensschijven | Doorvoer: IOPS | Max. aantal NIC's | Verwachte netwerkbandbreedte (Mbps) |
|---|---|---|---|---|---|---|---|---|
| Standard_D1_v2 | 1  | 3,5 | 50  | 3000/46/23    | 4  | 4 x 500  | 2|750   |
| Standard_D2_v2 | 2  | 7   | 100 | 6000/93/46    | 8  | 8 x 500  | 2|1500  |
| Standard_D3_v2 | 4  | 14  | 200 | 12000/187/93  | 16 | 16 x 500 | 4|3000  |
| Standard_D4_v2 | 8  | 28  | 400 | 24000/375/187 | 32 | 32 x 500 | 8|6000  |
| Standard_D5_v2 | 16 | 56  | 800 | 48000/750/375 | 64 | 64x500 | 8|12000 |

## <a name="dsv2-series"></a>DSv2-serie

De DSv2-serie wordt uitgevoerd op Intel® Xeon® 8272CL (Cascade Lake), Intel® Xeon® 8171M 2,1GHz (Skylake) of intel® Xeon®® <2> <9> E5-2673 v4 2,3 GHz (Broadwell) of intel® Xeon® E5-2673 v3 2,4 GHz (Haswell)-processors met Intel Turbo Boost Technology 2.0 en maken gebruik van Premium Storage.

[ACU:](acu.md)210-250<br>
[Premium Storage:](premium-storage-performance.md)ondersteund<br>
[Premium Storage in deaching:](premium-storage-performance.md)ondersteund<br>
[Livemigratie:](maintenance-and-updates.md)ondersteund<br>
[Updates met geheugenbehoud:](maintenance-and-updates.md)ondersteund<br>
[VM-generatieondersteuning:](generation-2.md)generatie 1 en 2<br>
[Versneld netwerken:](../virtual-network/create-vm-accelerated-networking-cli.md)ondersteund *(vereist minimaal 2 vCPU's)*<br>
[Kortstondige besturingssysteemschijven:](ephemeral-os-disks.md)ondersteund <br>
<br>

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer in cache en tijdelijke opslag: IOPS/MBps (cachegrootte in GiB) | Maximale niet-gecachede schijfdoorvoer: IOPS/MBps | Max. aantal NIC's|Verwachte netwerkbandbreedte (Mbps) |
|---|---|---|---|---|---|---|---|---|
| Standard_DS1_v2 | 1  | 3,5 | 7   | 4  | 4000/32 (43)    | 3200/48   | 2|750   |
| Standard_DS2_v2 | 2  | 7   | 14  | 8  | 8000/64 (86)    | 6400/96   | 2|1500  |
| Standard_DS3_v2 | 4  | 14  | 28  | 16 | 16000/128 (172) | 12800/192 | 4|3000  |
| Standard_DS4_v2 | 8  | 28  | 56  | 32 | 32000/256 (344) | 25600/384 | 8|6000  |
| Standard_DS5_v2 | 16 | 56  | 112 | 64 | 64000/512 (688) | 51200/768 | 8|12000 |

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes-and-information"></a>Andere grootten en informatie

- [Algemeen gebruik](sizes-general.md)
- [Geoptimaliseerd voor geheugen](sizes-memory.md)
- [Geoptimaliseerd voor opslag](sizes-storage.md)
- [Geoptimaliseerde GPU](sizes-gpu.md)
- [Krachtig rekenvermogen](sizes-hpc.md)
- [Vorige generaties](sizes-previous-gen.md)

Prijscalculator: [Prijscalculator](https://azure.microsoft.com/pricing/calculator/)

Meer informatie over schijftypen: [schijftypen](./disks-types.md#ultra-disk)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [Azure Compute Units (ACU)](acu.md) u kan helpen bij het vergelijken van rekenprestaties in Azure-SKU's.
