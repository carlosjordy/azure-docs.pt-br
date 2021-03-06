---
title: Arquivos e diretórios no Lote do Azure
description: Saiba mais sobre arquivos e diretórios e como eles são usados em um fluxo de trabalho do Lote do Azure do ponto de vista de desenvolvimento.
ms.topic: conceptual
ms.date: 05/12/2020
ms.openlocfilehash: e7babb7e2cfdbbe78f61be766c549c1e80cacf98
ms.sourcegitcommit: a9784a3fd208f19c8814fe22da9e70fcf1da9c93
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83790863"
---
# <a name="files-and-directories-in-azure-batch"></a>Arquivos e diretórios no Lote do Azure

No Lote do Azure, cada tarefa tem um diretório de trabalho em que ela cria zero ou mais arquivos e diretórios. Esse diretório de trabalho pode ser usado para armazenar o programa executado pela tarefa, os dados que ele processa e a saída do processamento executado. Todos os arquivos e diretórios de uma tarefa são pertencentes ao usuário de tarefa.

O serviço de Lote exibe uma parte do sistema de arquivos em um nó como o *diretório-raiz*. As tarefas podem acessar esse diretório-raiz referenciando a variável de ambiente `AZ_BATCH_NODE_ROOT_DIR` . Para saber mais sobre como usar as variáveis de ambiente, consulte [Configurações de ambiente para tarefas](jobs-and-tasks.md#environment-settings-for-tasks).

O diretório raiz contém a seguinte estrutura de diretório:

![Compute node directory structure][media\files-and-directories\node-folder-structure.png]

- **aplicativos**: Contém informações sobre os detalhes dos pacotes de aplicativos instalados no nó de computação. As tarefas podem acessar esse diretório referenciando a variável de ambiente `AZ_BATCH_APP_PACKAGE` .

- **fsmounts**: O diretório contém todos os sistemas de arquivos montados em um nó de computação. As tarefas podem acessar esse diretório referenciando a variável de ambiente `AZ_BATCH_NODE_MOUNTS_DIR` . Para mais informações, consulte [Montar um sistema de arquivos virtual em um pool do Lote](virtual-file-mount.md).

- **compartilhado**: esse diretório fornece acesso de leitura/gravação a *todas* as tarefas executadas em um nó. Qualquer tarefa executada no nó pode criar, ler, atualizar e excluir arquivos nesse diretório. As tarefas podem acessar esse diretório referenciando a variável de ambiente `AZ_BATCH_NODE_SHARED_DIR` .

- **inicialização**: esse diretório é usado por uma tarefa inicial como seu diretório de trabalho. Todos os arquivos que são baixados para o nó pela tarefa de inicialização são armazenados aqui. A tarefa inicial pode criar, ler, atualizar e excluir arquivos nesse diretório. As tarefas podem acessar esse diretório referenciando a variável de ambiente `AZ_BATCH_NODE_STARTUP_DIR` .

- **volatile**: Este diretório é para fins internos. Não há garantia de que todos os arquivos nesse diretório ou que o diretório em si existam no futuro.

- **workitems**: Este diretório contém os diretórios para trabalhos e suas tarefas no nó de computação.

    No diretório **workitems**, um diretório **Tarefas** é criado para cada tarefa executada no nó. Esse diretório pode ser acessado ao fazer referência à variável de ambiente `AZ_BATCH_TASK_DIR`.
    
    Em cada diretório **Tarefas**, o serviço Lote cria um diretório de trabalho (`wd`) cujo caminho exclusivo é especificado pela variável de ambiente `AZ_BATCH_TASK_WORKING_DIR`. Esse diretório oferece acesso de leitura/gravação à tarefa. A tarefa pode criar, ler, atualizar e excluir arquivos contidos nesse diretório. Esse diretório é mantido com base na restrição *RetentionTime* especificada para a tarefa.

    Os arquivos `stdout.txt` e `stderr.txt` são gravados na pasta **Tarefas** durante a execução da tarefa.

> [!IMPORTANT]
> Quando um nó é removido do pool, todos os arquivos armazenados no nó são removidos.

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre [tratamento e detecção de erro](error-handling.md) no Lote do Azure.