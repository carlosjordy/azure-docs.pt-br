---
title: incluir arquivo
description: incluir arquivo
services: backup
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: include
ms.date: 10/18/2018
ms.author: dacurwin
ms.custom: include file
ms.openlocfilehash: c0f717e26534151585a51badf4bb1fe1ba58eb00
ms.sourcegitcommit: f684589322633f1a0fafb627a03498b148b0d521
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85970511"
---
## <a name="create-a-recovery-services-vault"></a>Criar um cofre dos Serviços de Recuperação

Um cofre dos serviços de recuperação é uma entidade de gerenciamento que armazena pontos de recuperação criados ao longo do tempo e fornece uma interface para executar operações relacionadas ao backup. Isso inclui fazer backups sob demanda, executar restaurações e criar políticas de backup.

Para criar um cofre dos Serviços de Recuperação, siga estas etapas.

1. Entre na sua assinatura no [Portal do Azure](https://portal.azure.com/).

1. No menu esquerdo, selecione **Todos os serviços**.

    ![Selecionar Todos os serviços](./media/backup-create-rs-vault/click-all-services.png)

1. Na caixa de diálogo **Todos os serviços**, insira *Serviços de Recuperação*. A lista de recurso filtra de acordo com sua entrada. Na lista de recursos, selecione **Cofres dos Serviços de Recuperação**.

    ![Insira e escolha os cofres dos Serviços de Recuperação](./media/backup-create-rs-vault/all-services.png)

    A lista de cofres de Serviços de Recuperação na assinatura aparecerá.

1. No painel **Cofres dos Serviços de Recuperação**, selecione **Adicionar**.

    ![Adicionar um cofre dos Serviços de Recuperação](./media/backup-create-rs-vault/add-button-create-vault.png)

    A caixa de diálogo **Cofre dos Serviços de Recuperação** é aberta. Forneça valores para **Nome**, **Assinatura**, **Grupo de recursos** e **Local**.

    ![Configurar o cofre de Serviços de Recuperação](./media/backup-create-rs-vault/create-new-vault-dialog.png)

   - **Name**: Digite um nome amigável para identificar o cofre. O nome deve ser exclusivo para a assinatura do Azure. Especifique um nome que tenha, pelo menos, dois, porém, não mais de 50 caracteres. O nome deve começar com uma letra e consistir apenas em letras, números e hifens.
   - **Assinatura**: Escolha a assinatura a ser usada. Se você for um membro de apenas uma assinatura, verá esse nome. Se você não tem certeza de qual assinatura usar, utilize a assinatura padrão (sugerida). Só haverá múltiplas opções se a sua conta corporativa ou de estudante estiver associada a várias assinaturas do Azure.
   - **Grupo de recursos**: Use um grupo de recursos existente ou crie um novo. Para ver a lista de grupos de recursos disponíveis em sua assinatura, selecione **Usar existente** e, em seguida, selecione um recurso na lista suspensa. Para criar um novo grupo de recursos, selecione **Criar novo** e insira o nome. Para saber mais sobre os grupos de recursos, confira [Visão geral do Azure Resource Manager](../articles/azure-resource-manager/management/overview.md).
   - **Localização**: Selecione a região geográfica para o cofre. Para criar um cofre para proteger qualquer fonte de dados, o cofre *deve* estar na mesma região que a fonte de dados.

      > [!IMPORTANT]
      > Se você não tem certeza da localização da sua fonte de dados, feche a caixa de diálogo. Vá para a lista de seus recursos no portal. Se você tiver fontes de dados em várias regiões, crie um cofre dos Serviços de Recuperação para cada região. Crie o cofre na primeira localização antes de criar o cofre para outra. Não é necessário especificar contas de armazenamento para armazenar os dados de backup. O cofre dos Serviços de Recuperação e o Backup do Azure cuidam disso automaticamente.
      >
      >

1. Quando você estiver pronto para criar o cofre dos Serviços de Recuperação, clique em **Criar**.

    ![Criar o cofre dos Serviços de Recuperação](./media/backup-create-rs-vault/click-create-button.png)

    Pode levar um tempo para criar o cofre dos Serviços de Recuperação. Monitore as notificações de status na área **Notificações** no canto superior direito do portal. Depois que o cofre tiver sido criado, ele aparecerá na lista de cofres dos Serviços de Recuperação. Se você não encontrar seu cofre, selecione **Atualizar**.

     ![Atualizar a lista de cofres de backup](./media/backup-create-rs-vault/refresh-button.png)

>[!IMPORTANT]
> É altamente recomendável revisar as configurações padrão de **tipo de replicação de armazenamento** e configurações de **segurança** antes de configurar backups no cofre. Para obter mais informações, consulte a seção [definir redundância de armazenamento](https://docs.microsoft.com/azure/backup/backup-create-rs-vault#set-storage-redundancy) .
