---
title: Exemplos da CLI do Azure para a API do MongoDB do Azure Cosmos DB
description: Exemplos da CLI do Azure para a API do MongoDB do Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
ms.date: 06/03/2020
ms.author: mjbrown
ms.openlocfilehash: 7536a2e3fc32aeb330aad52625a5946e856d6646
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85556044"
---
# <a name="azure-cli-samples-for-azure-cosmos-db-mongodb-api"></a>Exemplos da CLI do Azure para a API do MongoDB do Azure Cosmos DB

A tabela a seguir inclui links para exemplos de scripts de CLI do Azure para a API do MongoDB do Azure Cosmos DB. As páginas de referência de todos os comandos do Azure Cosmos DB CLI estão disponíveis na [Referência de CLI do Azure](/cli/azure/cosmosdb). Todos os exemplos de scripts da CLI do Azure Cosmos DB podem ser encontrados no [Repositório GitHub da CLI do Azure Cosmos DB](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).

> [!NOTE]
> No momento, você só pode criar a versão 3.2 (ou seja, contas que usam o ponto de extremidade no formato `*.documents.azure.com`) da API do Azure Cosmos DB para as contas do MongoDB usando o PowerShell, a CLI e os modelos do Resource Manager. Para criar a versão 3.6 das contas, use portal do Azure.

|Tarefa | Descrição |
|---|---|
| [Criar uma conta, um banco de dados e uma coleção do Azure Cosmos](scripts/cli/mongodb/create.md?toc=%2fcli%2fazure%2ftoc.json)| Cria uma conta, um banco de dados e uma coleção do Azure Cosmos DB para API do MongoDB. |
| [Alterar taxa de transferência](scripts/cli/mongodb/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | Atualizar a RU/s em um banco de dados e em uma coleção.|
| [Adicionar ou fazer failover de regiões](scripts/cli/common/regions.md?toc=%2fcli%2fazure%2ftoc.json) | Adicione uma região, altere a prioridade do failover, dispare um failover manual.|
| [Chaves da conta e cadeias de conexão](scripts/cli/common/keys.md?toc=%2fcli%2fazure%2ftoc.json) | Liste chaves da conta, chaves somente leitura, regenerar chaves e listar cadeias de conexão.|
| [Proteger com o firewall do IP](scripts/cli/common/ipfirewall.md?toc=%2fcli%2fazure%2ftoc.json)| Crie uma conta do Cosmos com o firewall do IP configurado.|
| [Proteger a nova conta com pontos de extremidade de serviço](scripts/cli/common/service-endpoints.md?toc=%2fcli%2fazure%2ftoc.json)| Crie uma conta do Cosmos e proteja com pontos de extremidade de serviço.|
| [Proteger a conta existente com pontos de extremidade de serviço](scripts/cli/common/service-endpoints-ignore-missing-vnet.md?toc=%2fcli%2fazure%2ftoc.json)| Atualize uma conta do Cosmos para ser protegida com pontos de extremidade de serviço quando a sub-rede estiver finalmente configurada.|
| [Bloquear recursos da exclusão](scripts/cli/mongodb/lock.md?toc=%2fcli%2fazure%2ftoc.json)| Impedir que os recursos sejam excluídos com bloqueios de recursos.|
|||
