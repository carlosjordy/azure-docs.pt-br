---
title: Reiniciar o servidor-portal do Azure-banco de dados do Azure para MySQL
description: Este artigo descreve como você pode reiniciar um servidor de banco de dados do Azure para MySQL usando o portal do Azure.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 3/18/2020
ms.openlocfilehash: aa79d1ba2cba86bf3f381ff78fd630ce455128d9
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86107862"
---
# <a name="restart-azure-database-for-mysql-server-using-azure-portal"></a>Reiniciar o servidor Banco de Dados do Azure para MySQL usando o portal do Azure
Este tópico descreve como você pode reiniciar um servidor do Banco de Dados do Azure para MySQL. Você talvez precise reiniciar o servidor por razões de manutenção, o que causa uma breve interrupção, conforme o servidor executa a operação.

A reinicialização do servidor será bloqueada se o serviço estiver ocupado. Por exemplo, o serviço pode estar processando uma operação solicitada anteriormente como o dimensionamento vCores.

O tempo necessário para concluir uma reinicialização depende do processo de recuperação do MySQL. Para diminuir o tempo de reinicialização, é recomendável que você minimize a quantidade de atividade que ocorre no servidor antes da reinicialização.

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este guia de instruções, você precisa:
- Um [banco de dados do Azure para servidor MySQL](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="perform-server-restart"></a>Realizar a reinicialização do servidor

As etapas a seguir reiniciam o servidor MySQL:

1. No Portal do Azure, selecione o servidor do Banco de Dados do Azure para MySQL.

2. Na barra de ferramentas da página de **Visão Geral** do servidor, clique em **Reiniciar**.

   ![Banco de Dados do Azure para MySQL - Visão geral - botão Reiniciar](./media/howto-restart-server-portal/2-server.png)

3. Clique em **Sim** para confirmar a reinicialização do servidor.

   ![Banco de Dados do Azure para MySQL - Restaurar confirmação](./media/howto-restart-server-portal/3-restart-confirm.png)

4. Observe que o status do servidor muda para "Reiniciando".

   ![Banco de Dados do Azure para MySQL - Reiniciar status](./media/howto-restart-server-portal/4-restarting-status.png)

5. Confirme se a reinicialização do servidor foi bem-sucedida.

   ![Banco de Dados do Azure para MySQL - Reiniciar status](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Próximas etapas

[Início rápido: criar e gerenciar um servidor de Banco de Dados do Azure para MySQL usando o Portal do Azure](./quickstart-create-mysql-server-database-using-azure-portal.md)
