---
title: 'Patroon: Logische operators in een beleidsdefinitie'
description: Dit Azure Policy-patroon biedt voorbeelden van het gebruik van logische operators in een beleidsdefinitie.
ms.date: 03/31/2021
ms.topic: sample
ms.openlocfilehash: feb9e50b0c73c19027b747cf0f95fa1cb6fbd47c
ms.sourcegitcommit: 99fc6ced979d780f773d73ec01bf651d18e89b93
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/31/2021
ms.locfileid: "106093347"
---
# <a name="azure-policy-pattern-logical-operators"></a>Azure Policy-patroon: logische operators

Een beleidsdefinitie kan verschillende voorwaardelijke instructies bevatten. Mogelijk moeten alle instructies waar zijn of is dit slechts voor een aantal instructies vereist. Ter ondersteuning van deze behoeften heeft de taal [logische operators](../concepts/definition-structure.md#logical-operators) voor **not**, **allOf** en **anyOf**. Deze operators zijn optioneel en kunnen worden genest om complexe scenario's te maken.

## <a name="sample-1-one-logical-operator"></a>Voorbeeld 1: Eén logische operator

Met deze beleids definitie worden [Azure Cosmos DB](../../../cosmos-db/introduction.md) -accounts geëvalueerd om te zien of automatische failovers en meerdere schrijf locaties zijn geconfigureerd. Wanneer dit niet het geval is, wordt de [audit](../concepts/effects.md#audit) geactiveerd en wordt er een logboekvermelding gemaakt wanneer de niet-compatibele resource wordt gemaakt of bijgewerkt.

:::code language="json" source="~/policy-templates/patterns/pattern-logical-operators-1.json":::

### <a name="sample-1-explanation"></a>Voorbeeld 1: Uitleg

:::code language="json" source="~/policy-templates/patterns/pattern-logical-operators-1.json" range="6-22" highlight="3":::

In het blok **policyRule.if** wordt één **allOf** gebruikt om ervoor te zorgen dat aan alle drie de voorwaarden wordt voldaan.
Alleen wanneer aan al deze voorwaarden wordt voldaan, wordt het effect **audit** geactiveerd.

## <a name="sample-2-multiple-logical-operators"></a>Voorbeeld 2: Meerdere logische operators

Deze beleidsdefinitie evalueert resources op een naamgevingspatroon. Als een resource niet voldoet aan het patroon, wordt deze geweigerd met het effect [deny](../concepts/effects.md#deny).

:::code language="json" source="~/policy-templates/patterns/pattern-logical-operators-2.json":::

### <a name="sample-2-explanation"></a>Voorbeeld 2: Uitleg

:::code language="json" source="~/policy-templates/patterns/pattern-logical-operators-2.json" range="7-21" highlight="2,3,9":::

Het blok **policyRule.if** bevat ook één **allOf**, maar elke voorwaarde bevat ook de logische operator **not**. De voorwaarde in de logische operator **not** wordt eerst geëvalueerd en daarna wordt **not** geëvalueerd om te bepalen of de volledige component waar of niet waar is. Als beide logische operators **not** worden geëvalueerd als waar, wordt het beleidseffect geactiveerd.

## <a name="sample-3-combining-logical-operators"></a>Voorbeeld 3: Logische operators combineren

Met deze beleids definitie wordt een [lente van Azure](/azure/developer/java/spring-framework) -accounts geëvalueerd om te zien of tracering niet is ingeschakeld of dat tracering niet is geslaagd.

:::code language="json" source="~/policy-templates/patterns/pattern-logical-operators-3.json":::

### <a name="sample-3-explanation"></a>Voorbeeld 3: Uitleg

:::code language="json" source="~/policy-templates/patterns/pattern-logical-operators-3.json" range="6-28" highlight="3,8":::

Dit blok **policyRule.if** bevat zowel de logische operator **allOf** als **anyOf**. De logische operator **anyOf** levert waar op als ten minste aan één voorwaarde wordt voldaan. Aangezien het _type_ essentieel is voor **allOf**, moet deze altijd waar opleveren. Als het _type_ en een van de voorwaarden in **anyOf** waar zijn, wordt het beleidseffect geactiveerd.

## <a name="next-steps"></a>Volgende stappen

- Bekijk andere [patronen en ingebouwde definities](./index.md).
- Lees over de [structuur van Azure Policy-definities](../concepts/definition-structure.md).
- Lees [Informatie over de effecten van het beleid](../concepts/effects.md).