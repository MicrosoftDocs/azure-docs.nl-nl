---
title: 'PowerShell-voorbeeld: gebruikers en groepen voor toepassingsproxy-app vermelden'
description: PowerShell-voorbeeld waarin alle gebruikers en groepen worden vermeld die aan een bepaalde Azure Active Directory (Azure AD)-toepassingsproxytoepassing zijn gekoppeld.
services: active-directory
author: kenwith
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: sample
ms.date: 12/05/2019
ms.author: kenwith
ms.reviewer: japere
ms.openlocfilehash: 0fdbf2ab7d33978a72e7455f3e2f5ba591cc4ab7
ms.sourcegitcommit: 2654d8d7490720a05e5304bc9a7c2b41eb4ae007
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/13/2021
ms.locfileid: "107375488"
---
# <a name="display-users-and-groups-assigned-to-an-application-proxy-application"></a>Gebruikers en groepen weergeven die zijn toegewezen aan een toepassingsproxytoepassing

In dit PowerShell-voorbeeld worden alle gebruikers en groepen vermeld die aan een bepaalde Azure Active Directory (Azure AD)-toepassingsproxytoepassing zijn gekoppeld.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Voor dit voorbeeld is de [AzureAD V2 PowerShell voor Graph-module](/powershell/azure/active-directory/install-adv2) (AzureAD) of de [AzureAD V2 PowerShell voor Graph-module (preview)](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview&preserve-view=true) (AzureADPreview) vereist.

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/display-users-group-of-an-app.ps1 "Display users and groups assigned to an Application Proxy application")]

## <a name="script-explanation"></a>Uitleg van het script

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureADUser](/powershell/module/AzureAD/get-azureaduser)| Hiermee wordt een gebruiker opgehaald. |
| [Get-AzureADGroup](/powershell/module/AzureAD/get-azureadgroup)| Hiermee wordt een groep opgehaald. |
| [Get-AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal) | Hiermee wordt een service-principal opgehaald. |
| [Get-AzureADUserAppRoleAssignment](/powershell/module/AzureAD/get-azureaduserapproleassignment) | Hiermee wordt een roltoewijzing voor een gebruikerstoepassing opgehaald. |
| [Get-AzureADGroupAppRoleAssignment](/powershell/module/AzureAD/get-azureadgroupapproleassignment) | Hiermee wordt een roltoewijzing voor een groepstoepassing opgehaald. |

## <a name="next-steps"></a>Volgende stappen

Zie het [overzicht van de Azure PowerShell-module](/powershell/azure/active-directory/overview) voor meer informatie over de Azure AD PowerShell-module.

Zie [Azure AD PowerShell-voorbeelden voor de Azure AD-toepassingsproxy](../application-proxy-powershell-samples.md) voor andere PowerShell-voorbeelden voor de toepassingsproxy.
