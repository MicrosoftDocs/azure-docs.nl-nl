---
title: De door Azure Automation account beheerde identiteit uitschakelen (preview)
description: In dit artikel wordt uitgelegd hoe u een beheerde identiteit voor een Azure Automation-account kunt uitschakelen Azure Automation verwijderen.
services: automation
ms.subservice: process-automation
ms.date: 04/14/2021
ms.topic: conceptual
ms.openlocfilehash: e17e1afda50d9a0263067a77bf26435f53b4f237
ms.sourcegitcommit: db925ea0af071d2c81b7f0ae89464214f8167505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/15/2021
ms.locfileid: "107519269"
---
# <a name="disable-your-azure-automation-account-managed-identity-preview"></a>De door Azure Automation account beheerde identiteit uitschakelen (preview)

Er zijn twee manieren om een door het systeem toegewezen identiteit uit te schakelen in Azure Automation. U kunt deze taak uitvoeren vanuit de Azure Portal of met behulp van een ARM Azure Resource Manager sjabloon ( Azure Resource Manager).

## <a name="disable-managed-identity-in-the-azure-portal"></a>Beheerde identiteit uitschakelen in de Azure Portal

U kunt de beheerde identiteit uitschakelen vanuit de Azure Portal ongeacht hoe de beheerde identiteit oorspronkelijk is ingesteld.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Navigeer naar uw Automation-account en selecteer **Identiteit** onder **Accountinstellingen.**

1. Stel de **optie Systeem toegewezen** in op **Uit** en druk op **Opslaan.** Wanneer u wordt gevraagd om te bevestigen, drukt u op **Ja.**

De beheerde identiteit wordt verwijderd en heeft geen toegang meer tot de doelresource.

## <a name="disable-using-azure-resource-manager-template"></a>Uitschakelen met behulp Azure Resource Manager sjabloon

Als u de beheerde identiteit voor uw Automation-account hebt gemaakt met behulp van een Azure Resource Manager-sjabloon, kunt u de beheerde identiteit uitschakelen door die sjabloon opnieuw te gebruiken en de instellingen ervan te wijzigen. Stel het type van de onderliggende eigenschap van het identiteitsobject in op **Geen,** zoals wordt weergegeven in het volgende voorbeeld, en voer de sjabloon vervolgens opnieuw uit.

```json
"identity": { 
   "type": "None" 
} 
```

Als u een door het systeem toegewezen identiteit verwijdert met behulp van deze methode, wordt deze ook verwijderd uit Azure AD. Door het systeem toegewezen identiteiten worden ook automatisch verwijderd uit Azure AD wanneer de app-resource aan wie ze zijn toegewezen, wordt verwijderd.

## <a name="next-steps"></a>Volgende stappen

- Zie Enable [and use managed identity for Automation (preview)](enable-managed-identity-for-automation.md)(Beheerde identiteit inschakelen en gebruiken voor Automation (preview) voor meer informatie over het inschakelen van beheerde identiteiten in Azure Automation.

- Zie Overzicht van Automation-accountverificatie voor een overzicht van de beveiliging van [Automation-account.](automation-security-overview.md)
