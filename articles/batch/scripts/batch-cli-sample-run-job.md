---
title: Exemplo de Script da CLI do Azure – Execute um trabalho em lotes
description: Esse script cria um trabalho no Lote e adiciona uma série de tarefas ao trabalho. Ele também demonstra como monitorar um trabalho e suas tarefas.
ms.topic: sample
ms.date: 12/12/2019
ms.openlocfilehash: eefc6cfdc01ddf4b8fe05b3b52360994e5763013
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85957249"
---
# <a name="cli-example-run-a-job-and-tasks-with-azure-batch"></a>Exemplo de CLI: executar um trabalho e tarefas com o Lote do Azure

Esse script cria um trabalho no Lote e adiciona uma série de tarefas ao trabalho. Ele também demonstra como monitorar um trabalho e suas tarefas. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Caso opte por instalar e usar a CLI localmente, este artigo exige que seja executada a CLI do Azure versão 2.0.20 ou posterior. Execute `az --version` para encontrar a versão. Se você precisa instalar ou atualizar, consulte [Instalar a CLI do Azure](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/batch/run-job/run-job.sh "Run Job")]

## <a name="clean-up-deployment"></a>Limpar a implantação

Execute o comando a seguir para remover o grupo de recursos e todos os recursos associados a ele.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explicação sobre o script

Este script usa os comandos a seguir. Cada comando na tabela redireciona para a documentação específica do comando.

| Comando | Observações |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az batch account create](/cli/azure/batch/account#az-batch-account-create) | Cria a conta do Lote. |
| [az batch account login](/cli/azure/batch/account#az-batch-account-login) | Autentica na conta do Lote especificada para interação adicional com a CLI.  |
| [az batch pool create](/cli/azure/batch/pool#az-batch-pool-create) | Cria um pool de nós de computação.  |
| [az batch job create](/cli/azure/batch/job#az-batch-job-create) | Cria um trabalho do Lote.  |
| [az batch task create](/cli/azure/batch/task#az-batch-task-create) | Adiciona uma tarefa ao trabalho do Lote especificado.  |
| [az batch job set](/cli/azure/batch/job#az-batch-job-set) | Atualiza as propriedades de um trabalho do Lote.  |
| [az batch job show](/cli/azure/batch/job#az-batch-job-show) | Recupera detalhes de um trabalho especificado do Lote.  |
| [az batch task show](/cli/azure/batch/task#az-batch-task-show) | Recupera os detalhes de uma tarefa do trabalho do Lote especificado.  |
| [az group delete](/cli/azure/group#az-group-delete) | Exclui um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre a CLI do Azure, veja a [documentação da CLI do Azure](/cli/azure).
