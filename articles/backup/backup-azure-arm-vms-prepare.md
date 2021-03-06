---
title: Fazer backup de máquinas virtuais do Azure em um cofre dos Serviços de Recuperação
description: Descreve como fazer backup de VMs do Azure em um cofre dos Serviços de Recuperação no Backup do Azure
ms.topic: conceptual
ms.date: 04/03/2019
ms.openlocfilehash: 88e7be7e2238637f1e6d5ac84abebdca0b9e1674
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86497923"
---
# <a name="back-up-azure-vms-in-a-recovery-services-vault"></a>Fazer backup de máquinas virtuais do Azure em um cofre dos Serviços de Recuperação

Este artigo descreve como fazer backup de VMs do Azure em um cofre dos Serviços de Recuperação, usando o serviço de [Backup do Azure](backup-overview.md).

Neste artigo, você aprenderá como:

> [!div class="checklist"]
>
> * Prepare VMs do Azure.
> * Crie um cofre.
> * Descubra VMs e configure uma política de backup.
> * Habilite o backup para VMs do Azure.
> * Executar o backup inicial.

> [!NOTE]
> Este artigo descreve como configurar um cofre e selecionar VMs para backup. Isso é útil se você quiser fazer backup de várias VMs. Como alternativa, você pode [fazer backup de uma única VM do Azure](backup-azure-vms-first-look-arm.md) diretamente das configurações da VM.

## <a name="before-you-start"></a>Antes de começar

* [Examine](backup-architecture.md#architecture-built-in-azure-vm-backup) a arquitetura de backup de VM do Azure.
* [Saiba mais sobre](backup-azure-vms-introduction.md) backup de VM do Azure e a extensão de backup.
* [Examine a matriz de suporte](backup-support-matrix-iaas.md) antes de configurar o backup.

Além disso, há algumas ações que talvez você precise realizar em algumas circunstâncias:

* **Instale o agente de VM na VM**: O Backup do Azure faz backup de VMs do Azure instalando uma extensão para o agente de VM do Azure em execução no computador. Se a sua VM foi criada a partir de uma imagem do Azure Marketplace, o agente está instalado e em execução. Caso você crie uma VM personalizada ou migre um computador local, talvez seja necessário [instalar o agente manualmente](#install-the-vm-agent).

## <a name="create-a-vault"></a>Criar um cofre

 Um cofre armazena backups e pontos de recuperação criados ao longo do tempo e armazena as políticas de backup associadas às máquinas submetidas a backup. Crie um cofre da seguinte maneira:

1. Entre no [portal do Azure](https://portal.azure.com/).
2. Na pesquisa, digite **Serviços de Recuperação**. Em **Serviços**, clique em **Cofres dos Serviços de Recuperação**.

     ![Pesquisar cofres dos Serviços de Recuperação](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png)

3. No menu **Cofres dos Serviços de Recuperação**, clique em **+Adicionar**.

     ![Criar Cofre de Serviços de Recuperação - etapa 2](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

4. Em **Cofre dos Serviços de Recuperação**, insira um nome amigável para identificar o cofre.
    * O nome deve ser exclusivo para a assinatura do Azure.
    * Ele pode conter caracteres de 2 a 50.
    * Ele deve começar com uma letra e pode conter apenas letras, números e hifens.
5. Selecione a assinatura do Azure, o grupo de recursos e a região geográfica na qual o cofre deve ser criado. Em seguida, clique em **Criar**.
    * Pode levar algum tempo para que o cofre seja criado.
    * Monitore as notificações de status na área superior direita do portal.

Depois que o cofre é criado, ele aparece na lista de cofres dos Serviços de Recuperação. Se você não encontrar seu cofre, selecione **Atualizar**.

![Lista de cofres de backup](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

>[!NOTE]
> O Backup do Azure agora permite a personalização do nome do grupo de recursos criado pelo serviço de Backup do Azure. Para obter mais informações, confira [Grupo de recursos de Backup do Azure para Máquinas Virtuais](backup-during-vm-creation.md#azure-backup-resource-group-for-virtual-machines).

### <a name="modify-storage-replication"></a>Modificar a replicação de armazenamento

Por padrão, os cofres usam [GRS (armazenamento com redundância geográfica)](../storage/common/storage-redundancy.md).

* Se o cofre for seu mecanismo de backup primário, recomendamos que você use GRS.
* Você pode usar [LRS (armazenamento com redundância local)](../storage/common/storage-redundancy.md?toc=/azure/storage/blobs/toc.json) para ter uma opção mais barata.

Modifique o tipo de replicação de armazenamento da seguinte maneira:

1. No novo cofre, clique em **Propriedades** na seção **Configurações**.
2. Em **Propriedades**, em **Configuração de Backup**, clique em **Atualizar**.
3. Selecione o tipo de replicação de armazenamento e clique em **Salvar**.

      ![Definir a configuração de armazenamento para o novo cofre](./media/backup-try-azure-backup-in-10-mins/full-blade.png)

> [!NOTE]
   > Não é possível modificar o tipo de replicação de armazenamento depois que o cofre é configurado e contém itens de backup. Se desejar fazer isso, você precisará recriar o cofre.

## <a name="apply-a-backup-policy"></a>Aplicar uma política de backup

Configure uma política de backup para o cofre.

1. No cofre, clique em **+ Backup** na seção **Visão Geral**.

   ![Botão Backup](./media/backup-azure-arm-vms-prepare/backup-button.png)

2. Em **Meta de Backup** > **Onde sua carga de trabalho é executada?** , selecione **Azure**. Em **Do que você deseja fazer backup?** , selecione **Máquina virtual** >  **OK**. Isso registra a extensão da VM no cofre.

   ![Painéis Backup e Meta de Backup](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

3. Na **Política de Backup**, escolha a política que você deseja associar ao cofre.
    * A política padrão faz o backup da VM uma vez por dia. Os backups diários são mantidos por 30 dias. Instantâneos de recuperação instantânea são mantidos por dois dias.
    * Se você não quiser usar a política padrão, selecione **Criar** e crie uma política personalizada, conforme descrito no próximo procedimento.

      ![Política de backup padrão](./media/backup-azure-arm-vms-prepare/default-policy.png)

4. Em **Selecionar máquinas virtuais**, selecione as VMs das quais você deseja fazer backup usando a política. Em seguida, clique em **OK**.

   * As VMs selecionadas são validadas.
   * Você só pode escolher máquinas virtuais na mesma região que o cofre.
   * O backup das VMs só pode ser feito em um único cofre.

     ![Painel "Selecionar máquinas virtuais"](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

    >[!NOTE]
    > Somente as VMs na mesma região e assinatura do cofre estarão disponíveis para configurar o backup.

5. Em **Backup**, clique em **Habilitar backup**. Isso implantará a política no cofre e nas VMs e instalará a extensão de backup no agente da VM em execução na VM do Azure.

     ![Botão "Habilitar backup"](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Depois de habilitar o backup:

* O serviço de Backup instala a extensão de backup independentemente de a VM estar em execução.
* Um backup inicial será executado de acordo com seu agendamento de backup.
* Quando os backups forem executados, observe que:
  * Uma VM em execução tem a maior chance de capturar um ponto de recuperação consistente com o aplicativo.
  * No entanto, mesmo que a VM seja desativada, o backup será feito. Essa VM é conhecida como VM offline. Nesse caso, o ponto de recuperação será consistente com falhas.
* A conectividade de saída explícita não é necessária para permitir o backup de VMs do Azure.

### <a name="create-a-custom-policy"></a>Criar uma política personalizada

Se você optou por criar uma política de backup, preencha as configurações de política.

1. Em **Nome da política**, especifique um nome significativo.
2. Em **Agendamento de backup**, especifique quando os backups devem ser feitos. Você pode fazer backups diários ou semanais em VMs do Azure.
3. Em **Restauração Instantânea**, especifique por quanto tempo deseja manter os instantâneos localmente para restauração instantânea.
    * Quando você restaura, os discos da VM de backup são copiados do armazenamento, pela rede para o local de armazenamento de recuperação. Com a restauração instantânea, você pode aproveitar os instantâneos armazenados localmente feitos durante um trabalho de backup, sem esperar que os dados de backup sejam transferidos para o cofre.
    * Você pode reter instantâneos para restauração imediata entre um ou cinco dias. Dois dias é a configuração padrão.
4. Em **Período de retenção**, especifique por quanto tempo deseja manter seus pontos de backup diários ou semanais.
5. Em **Retenção do ponto de backup mensal**, especifique se deseja manter um backup mensal de seus backups diários ou semanais.
6. Clique em **OK** para salvar a política.

    ![Nova política de backup](./media/backup-azure-arm-vms-prepare/new-policy.png)

> [!NOTE]
   > O Backup do Azure não oferece suporte ao ajuste automático do relógio para alterações de horário de verão para backups de VMs do Azure. À medida que ocorrem alterações de tempo, modifique as políticas de backup manualmente, conforme necessário.

## <a name="trigger-the-initial-backup"></a>Disparar o backup inicial

O backup inicial será executado de acordo com o agendamento, mas você poderá executá-lo imediatamente da seguinte maneira:

1. No menu do cofre, clique em **Itens de backup**.
2. Em **Itens de Backup**, clique em **Máquina Virtual do Azure**.
3. Na lista **Itens de Backup**, clique nas reticências (...).
4. Clique em **Fazer backup agora**.
5. Em **Fazer Backup Agora**, use o controle de calendário para selecionar o último dia em que o ponto de recuperação deve ser mantido. Em seguida, clique em **OK**.
6. Monitorar as notificações do portal. Você pode monitorar o andamento do trabalho no painel do cofre > **Trabalhos de Backup** > **Em Andamento**. Dependendo do tamanho da VM, a criação do backup inicial pode demorar um pouco.

## <a name="verify-backup-job-status"></a>Verificar o status do trabalho de backup

Os detalhes do trabalho de backup para cada backup de VM consistem em duas fases, a fase de **Instantâneos** seguida pela fase **Transferir dados para o cofre**.<br/>
A fase de instantâneo garante a disponibilidade de um ponto de recuperação armazenado com os discos para **Restaurações Instantâneas** e estão disponíveis por um máximo de cinco dias, dependendo da retenção de instantâneo configurada pelo usuário. Transferir dados para o cofre cria um ponto de recuperação no cofre para retenção de longo prazo. A transferência de dados para o cofre só é iniciada depois que a fase de instantâneo é concluída.

  ![Status do trabalho de backup](./media/backup-azure-arm-vms-prepare/backup-job-status.png)

Há duas **Subtarefas** em execução no back-end, uma para o trabalho de backup de front-ends que pode ser verificado na folha de detalhes do **Trabalho de Backup**, conforme indicado abaixo:

  ![Status do trabalho de backup](./media/backup-azure-arm-vms-prepare/backup-job-phase.png)

A fase **Transferir dados para o cofre** pode levar vários dias para ser concluída, dependendo do tamanho dos discos, da rotatividade por disco e de vários outros fatores.

O status do trabalho pode variar dependendo dos seguintes cenários:

**Instantâneo** | **Transferir dados para o cofre** | **Status do Trabalho**
--- | --- | ---
Concluído | Em andamento | Em andamento
Concluído | Ignorado | Concluído
Concluído | Concluído | Concluído
Concluído | Falhou | Concluído com aviso
Falhou | Falhou | Falhou

Agora, com esse recurso, dois backups podem ser executados em paralelo para a mesma VM, porém, em qualquer fase (instantâneo, transferir dados para o cofre), apenas uma subtarefa pode estar em execução. Com essa funcionalidade de desacoplamento, pode-se evitar cenários em que o trabalho de backup em andamento resulta na falha do backup do dia seguinte. Os backups do dia seguinte poderão ter o instantâneo concluído enquanto **Transferir dados para o cofre** é ignorado se o trabalho de backup de um dia anterior estiver em estado de andamento.
O ponto de recuperação incremental criado no cofre capturará toda a rotatividade do último ponto de recuperação criado no cofre. Não há impacto no custo para o usuário.

## <a name="optional-steps"></a>Etapas opcionais

### <a name="install-the-vm-agent"></a>Instalar o agente de VM

O Backup do Azure faz backup de VMs do Azure instalando uma extensão para o agente de VM do Azure em execução no computador. Se a sua VM foi criada a partir de uma imagem do Azure Marketplace, o agente está instalado e em execução. Caso você crie uma VM personalizada ou migre um computador local, talvez seja necessário instalar o agente manualmente, conforme resumido na tabela.

**VM** | **Detalhes**
--- | ---
**Windows** | 1. [Baixe e instale](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) o arquivo MSI do agente.<br/><br/> 2. Instale com permissões de administrador no computador.<br/><br/> 3. Verifique a instalação. Em *C:\WindowsAzure\Packages* na VM, clique com o botão direito do mouse em **WaAppAgent.exe** > **Propriedades**. Na guia **Detalhes**, a **Versão do Produto** deve ser 2.6.1198.718 ou posterior.<br/><br/> Se você estiver atualizando o agente, verifique se nenhuma operação de backup está em execução e [reinstale o agente](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).
**Linux** | Instale usando um pacote RPM ou DEB do repositório de pacotes de sua distribuição. Esse é o método preferencial para instalar e atualizar o agente Linux do Azure. Todos os [provedores de distribuição aprovados](../virtual-machines/linux/endorsed-distros.md) integram o pacote do agente Linux do Azure em suas imagens e repositórios. O agente está disponível no [GitHub](https://github.com/Azure/WALinuxAgent), mas não recomendamos instalá-lo a partir daí.<br/><br/> Se você estiver atualizando o agente, verifique se nenhuma operação de backup está em execução e atualize os binários.

>[!NOTE]
> **O Backup do Azure agora dá suporte a backup e restauração de disco seletivo usando a solução de backup de Máquina Virtual do Azure.**
>
>Hoje, o Backup do Azure dá suporte ao backup de todos os discos (Sistema Operacional e dados) em uma VM usando a solução de backup de Máquina Virtual. Com a funcionalidade de exclusão de disco, você obtém uma opção para fazer backup de um ou alguns dos vários discos de dados em uma VM. Isso fornece uma solução eficiente e econômica para suas necessidades de backup e restauração. Cada ponto de recuperação contém dados dos discos incluídos na operação de backup, o que permite que você tenha um subconjunto de discos restaurados do ponto de recuperação fornecido durante a operação de restauração. Isso se aplica à restauração tanto do instantâneo quanto do cofre.
>
>**Para se inscrever na versão prévia, escreva-nos em AskAzureBackupTeam@microsoft.com**

## <a name="next-steps"></a>Próximas etapas

* Solucione quaisquer problemas com os[agentes de VM do Azure](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) ou o [backup de VM do Azure](backup-azure-vms-troubleshoot.md).
* [Restaure](backup-azure-arm-restore-vms.md) VMs do Azure.
