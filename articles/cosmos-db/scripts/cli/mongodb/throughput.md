---
title: Azure CLI-scripts voor doorvoerbewerkingen (RU/s) voor Azure Cosmos DB API for MongoDB-resources
description: Azure CLI-scripts voor doorvoerbewerkingen (RU/s) voor Azure Cosmos DB API for MongoDB-resources
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
ms.date: 10/07/2020
ms.openlocfilehash: fc477fdd842025722a2bcfcd0172ab9d57fb0d4e
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/20/2021
ms.locfileid: "107790705"
---
# <a name="throughput-rus-operations-with-azure-cli-for-a-database-or-graph-for-azure-cosmos-db-api-for-mongodb"></a>Doorvoerbewerkingen (RU/s) met Azure CLI voor een database of grafiek voor Azure Cosmos DB API for MongoDB
[!INCLUDE[appliesto-mongodb-api](../../../includes/appliesto-mongodb-api.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- Voor dit artikel is versie 2.12.1 of hoger van Azure CLI vereist. Als u Azure Cloud Shell gebruikt, is de nieuwste versie al geïnstalleerd.

## <a name="sample-script"></a>Voorbeeldscript

Met dit script maakt u een MongoDB-database met gedeelde doorvoer en een MongoDB-verzameling met toegewezen doorvoer, waarna de doorvoer voor beiden wordt bijgewerkt. Met het script wordt de migratie van standaarddoorvoer naar doorvoer met automatische schaalaanpassing uitgevoerd en de waarde van doorvoer met automatische schaalaanpassing gelezen nadat de migratie is voltooid.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/mongodb/throughput.sh "Throughput operations for Azure Cosmos DB API for MongoDB.")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen.

```azurecli-interactive
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az cosmosdb create](/cli/azure/cosmosdb#az_cosmosdb_create) | Hiermee wordt een Azure Cosmos DB-account gemaakt. |
| [az cosmosdb mongodb database create](/cli/azure/cosmosdb/mongodb/database#az_cosmosdb_mongodb_database_create) | Hiermee maakt u een Azure Cosmos MongoDB API-database. |
| [az cosmosdb mongodb collection create](/cli/azure/cosmosdb/mongodb/collection#az_cosmosdb_mongodb_collection_create) | Hiermee maakt u een Azure Cosmos MongoDB API-verzameling. |
| [az cosmosdb mongodb database throughput update](/cli/azure/cosmosdb/mongodb/database/throughput#az_cosmosdb_mongodb_database_throughput_update) | RU's bijwerken voor een Azure Cosmos MongoDB API-database. |
| [az cosmosdb mongodb collection throughput update](/cli/azure/cosmosdb/mongodb/collection/throughput#az_cosmosdb_mongodb_collection_throughput_update) | RU's bijwerken voor een Azure Cosmos MongoDB API-verzameling. |
| [az cosmosdb mongodb database throughput migrate](/cli/azure/cosmosdb/mongodb/database/throughput#az_cosmosdb_mongodb_database_throughput_migrate) | Migratie doorvoer voor een database. |
| [az cosmosdb mongodb collection throughput migrate](/cli/azure/cosmosdb/mongodb/collection/throughput#az_cosmosdb_mongodb_collection_throughput-migrate) | Migratie doorvoer voor een verzameling. |
| [az group delete](/cli/azure/resource#az_resource_delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure Cosmos DB CLI](/cli/azure/cosmosdb) voor meer informatie over de Azure Cosmos DB CLI.

Alle Azure Cosmos DB CLI-voorbeeldscripts vindt u in de [documentatie van Azure Cosmos DB CLI GitHub-opslagplaats](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
