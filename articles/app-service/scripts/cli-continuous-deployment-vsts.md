---
title: Continue implementatie vanuit een Azure-opslagplaatsen
description: Meer informatie over het gebruik van de Azure CLI voor het automatiseren van de implementatie en het beheer van uw App Service-app. In dit voorbeeld ziet u hoe u CI/CD instelt vanuit Azure opslagplaatsen.
author: msangapu-msft
tags: azure-service-management
ms.assetid: 389d3bd3-cd8e-4715-a3a1-031ec061d385
ms.devlang: azurecli
ms.topic: sample
ms.date: 12/11/2017
ms.author: msangapu
ms.custom: mvc, seodec18, devx-track-azurecli
ms.openlocfilehash: cfc9ffff21db92eef4a3a544cb042394078a4e6d
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/20/2021
ms.locfileid: "107788021"
---
# <a name="create-an-app-service-app-with-continuous-deployment-using-azure-cli"></a>Een App Service-app maken met continue implementatie met behulp van Azure CLI

In dit voorbeeldscript wordt een app gemaakt in App Service met de bijbehorende resources en wordt er vervolgens continue implementatie vanuit een Azure DevOps-opslagplaats ingesteld. Voor dit voorbeeld hebt u het volgende nodig:

* Een Azure DevOps-opslagplaats met toepassingscode, waarvoor u beheerdersbevoegdheden hebt.
* Een [persoonlijk toegangstoken (PAT)](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate) voor uw Azure DevOps-organisatie.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Voor deze zelfstudie is versie 2.0 of hoger van Azure CLI vereist. Als u Azure Cloud Shell gebruikt, is de nieuwste versie al geïnstalleerd.

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-vsts-continuous/deploy-vsts-continuous.sh?highlight=3-4 "Create an app with continuous deployment from Azure DevOps")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [`az group create`](/cli/azure/group#az_group_create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [`az appservice plan create`](/cli/azure/appservice/plan#az_appservice_plan_create) | Hiermee maakt u een App Service-plan. |
| [`az webapp create`](/cli/azure/webapp#az_webapp_create) | Hiermee maakt u een App Service-app. |
| [`az webapp deployment source config`](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_config) | Hiermee koppelt u een App Service-app aan een Git- of Mercurial-opslagplaats. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.

Meer voorbeelden van App Service CLI-scripts vindt u in de [documentatie van Azure App Service](../samples-cli.md).
