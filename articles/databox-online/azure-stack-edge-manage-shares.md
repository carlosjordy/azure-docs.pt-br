---
title: Gerenciamento de compartilhamento do Edge Azure Stack | Microsoft Docs
description: Descreve como usar o portal do Azure para gerenciar compartilhamentos no seu Azure Stack Edge.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 03/25/2019
ms.author: alkohli
ms.openlocfilehash: 1aceb9d2fb1d9b5890bc0859d432bc1c5e7e4db4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84339831"
---
# <a name="use-the-azure-portal-to-manage-shares-on-your-azure-stack-edge"></a>Use o portal do Azure para gerenciar compartilhamentos no seu Azure Stack Edge

Este artigo descreve como gerenciar compartilhamentos em seu Azure Stack Edge. Você pode gerenciar o Azure Stack Edge por meio da portal do Azure ou por meio da interface do usuário da Web local. Use o portal do Azure para adicionar, excluir, atualizar compartilhamentos ou sincronizar a chave de armazenamento para a conta de armazenamento associada aos compartilhamentos.

## <a name="about-shares"></a>Sobre compartilhamentos

Para transferir dados para o Azure, você precisa criar compartilhamentos em seu Azure Stack Edge. Os compartilhamentos que você adiciona no dispositivo Azure Stack Edge podem ser compartilhamentos locais ou compartilhamentos que enviam dados por push para a nuvem.

 - **Compartilhamentos locais**: Use esses compartilhamentos quando desejar que os dados sejam processados localmente no dispositivo.
 - **Compartilhamentos**: Use esses compartilhamentos quando desejar que os dados do dispositivo sejam enviados automaticamente para sua conta de armazenamento na nuvem. Todas as funções de nuvem, como **Atualizar** e **Sincronizar chaves de armazenamento**, se aplicam a esses compartilhamentos.

Neste artigo, você aprenderá como:

> [!div class="checklist"]
>
> * Adicionar um compartilhamento
> * Excluir um compartilhamento
> * Atualizar compartilhamentos
> * Sincronizar chave de armazenamento

## <a name="add-a-share"></a>Adicionar um compartilhamento

Execute as etapas a seguir no portal do Azure para criar um compartilhamento.

1. Na portal do Azure, vá para o recurso de borda do Azure Stack e vá para **Gateway > compartilhamentos**. Selecione **+ Adicionar compartilhamento** na barra de comandos.

    ![Selecionar Adicionar compartilhamento](media/azure-stack-edge-manage-shares/add-share-1.png)

2. Em **Adicionar compartilhamento**, especifique as configurações de compartilhamento. Forneça um nome exclusivo para seu compartilhamento.
    
    Os nomes de compartilhamentos só podem conter números, letras minúsculas e hifens. O nome do compartilhamento deve ter entre 3 e 63 caracteres e começar com uma letra ou um número. Cada hífen deve ser precedido e seguido por um caractere que não seja um hífen.

3. Escolha um **Tipo** para o compartilhamento. O tipo pode ser **SMB** ou **NFS**, sendo SMB o padrão. SMB é o padrão para clientes do Windows e NFS é usado para clientes Linux. Dependendo da escolha entre compartilhamentos SMB ou NFS, as opções apresentadas serão ligeiramente diferentes.

4. Forneça um **Conta de armazenamento** na qual o compartilhamento resida. Um contêiner será criado na conta de armazenamento com o nome do compartilhamento se o contêiner já não existir. Se o contêiner já existir, ele será usado.

5. Na lista suspensa, escolha o **Serviço de armazenamento** entre blob de blocos, blobs de páginas ou arquivos. O tipo do serviço escolhido depende do formato escolhido para os dados que residirão no Azure. Por exemplo, nessa instância, queremos que os dados residam como blobs de blocos no Azure, portanto, selecionamos **Blob de Blocos**. Se você escolher **Blob de Páginas**, precisará garantir que os dados sejam alinhados com 512 bytes. Use **Blob de páginas** para VHDs ou VHDX que são sempre alinhados com 512 bytes.

   > [!IMPORTANT]
   > Certifique-se de que a conta de armazenamento do Azure que você usa não tenha políticas de imutabilidade definidas nela se você a estiver usando com um Azure Stack borda ou Gateway do Data Box dispositivo. Para obter mais informações, consulte [definir e gerenciar políticas de imutabilidade para o armazenamento de BLOBs](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutability-policies-manage).

6. Esta etapa depende do tipo de compartilhamento criado, SMB ou NFS.
   - **Se estiver criando compartilhamento SMB**: no campo **Todos os usuários locais com privilégios**, escolha **Criar novo** ou **Usar existente**. Se a criação for de um novo usuário local, forneça o **nome de usuário**, a **senha** e então confirme a senha. Isso atribui as permissões ao usuário local. Depois de atribuir as permissões aqui, você pode usar o Gerenciador de Arquivos para modificar essas permissões.

      ![Adicionar compartilhamento SMB](media/azure-stack-edge-manage-shares/add-smb-share.png)

        Se você marcar a opção Permitir apenas operações de leitura para esses compartilhamento de dados, poderá especificar usuários somente leitura.
   - **Se a criação for de um compartilhamento NFS**: você precisará fornecer os **endereços IP dos clientes permitidos** que podem acessar o compartilhamento.

      ![Adicionar compartilhamento NFS](media/azure-stack-edge-manage-shares/add-nfs-share.png)

7. Para acessar com facilidade os compartilhamentos por meio dos módulos de computação de borda, use o ponto de montagem local. Selecione **Usar o compartilhamento com a Computação de borda** para que o compartilhamento seja montado automaticamente depois que ele é criado. Quando essa opção é selecionada, o módulo de borda também pode usar a computação com o ponto de montagem local.

8. Clique em **Criar** para criar o compartilhamento. Você será notificado de que a criação do compartilhamento está em andamento. Depois que o compartilhamento for criado com as configurações especificadas, a folha **Compartilhamentos** será atualizada para refletir o novo compartilhamento.

## <a name="add-a-local-share"></a>Adicionar um compartilhamento local

1. Na portal do Azure, vá para o recurso de borda do Azure Stack e vá para **Gateway > compartilhamentos**. Selecione **+ Adicionar compartilhamento** na barra de comandos.

   ![Selecionar Adicionar compartilhamento](media/azure-stack-edge-manage-shares/add-local-share-1.png)

2. Em **Adicionar compartilhamento**, especifique as configurações de compartilhamento. Forneça um nome exclusivo para seu compartilhamento.
    
    Os nomes de compartilhamentos só podem conter números, letras minúsculas e hifens. O nome do compartilhamento deve ter entre 3 e 63 caracteres e começar com uma letra ou um número. Cada hífen deve ser precedido e seguido por um caractere que não seja um hífen.

3. Escolha um **Tipo** para o compartilhamento. O tipo pode ser **SMB** ou **NFS**, sendo SMB o padrão. SMB é o padrão para clientes do Windows e NFS é usado para clientes Linux. Dependendo da escolha entre compartilhamentos SMB ou NFS, as opções apresentadas serão ligeiramente diferentes.

4. Para acessar com facilidade os compartilhamentos por meio dos módulos de computação de borda, use o ponto de montagem local. Selecione **Usar o compartilhamento com a computação de borda** para que o módulo do Edge possa usar a computação com o ponto de montagem local.

5. Selecione **Configurar como compartilhamentos locais do Edge**. Os dados em compartilhamentos locais permanecerão localmente no dispositivo. Você pode processar esses dados localmente.

6. No campo **Todos os usuários locais com privilégios**, escolha **Criar** ou **Usar existente**.

7. Selecione **Criar**. 

   ![Criar um compartilhamento local](media/azure-stack-edge-manage-shares/add-local-share-2.png)

    Você verá uma notificação indicando que a criação do compartilhamento está em andamento. Depois que o compartilhamento for criado com as configurações especificadas, a folha **Compartilhamentos** será atualizada para refletir o novo compartilhamento.

   ![Exibir atualizações da folha Compartilhamentos](media/azure-stack-edge-manage-shares/add-local-share-3.png)
    
    Selecione o compartilhamento para exibir o ponto de montagem local para os módulos de computação de borda para esse compartilhamento.

   ![Exibir detalhes do compartilhamento local](media/azure-stack-edge-manage-shares/add-local-share-4.png)

## <a name="mount-a-share"></a>Montar um compartilhamento

Se você criou um compartilhamento antes de configurar a computação no dispositivo Azure Stack Edge, será necessário montar o compartilhamento. Siga as etapas a seguir para montar um compartilhamento.

1. Na portal do Azure, vá para o recurso de borda do Azure Stack e vá para **Gateway > compartilhamentos**. Na lista de compartilhamentos, selecione o compartilhamento que você deseja montar. A coluna **Usados para computação** mostrará o status como **Desabilitado** para o compartilhamento selecionado.

   ![Selecionar compartilhamento](media/azure-stack-edge-manage-shares/select-share-mount.png)

2. Selecione **Montar**.

   ![Selecione montar](media/azure-stack-edge-manage-shares/select-mount.png)

3. Quando solicitado a confirmar, selecione **Sim**. Isso montará o compartilhamento.

   ![Confirme a montagem](media/azure-stack-edge-manage-shares/confirm-mount.png)

4. Depois que o compartilhamento estiver montado, vá para a lista de compartilhamentos. Você verá que a coluna **Usados para computação** mostra o status do compartilhamento como **Habilitado**.

   ![Compartilhamento montado](media/azure-stack-edge-manage-shares/share-mounted.png)

5. Selecione o compartilhamento novamente para exibir o ponto de montagem local para o compartilhamento. O módulo de computação de Borda usa esse ponto de montagem local para o compartilhamento.

   ![Ponto de montagem local para o compartilhamento](media/azure-stack-edge-manage-shares/share-mountpoint.png)

## <a name="unmount-a-share"></a>Desmontar um compartilhamento

Execute as etapas a seguir no portal do Azure para desmontar um compartilhamento.

1. Na portal do Azure, vá para o recurso de borda do Azure Stack e vá para **Gateway > compartilhamentos**.

   ![Selecionar compartilhamento](media/azure-stack-edge-manage-shares/select-share-unmount.png)

2. Na lista de compartilhamentos, selecione o compartilhamento que você deseja desmontar. Você deseja certificar-se de que o compartilhamento que você desmontar não é usado por todos os módulos. Se o compartilhamento for usado por um módulo, então você verá problemas com o módulo correspondente. Selecione **Desmontar**.

   ![Selecione desmontar](media/azure-stack-edge-manage-shares/select-unmount.png)

3. Quando solicitado a confirmar, selecione **Sim**. Isso desmontará o compartilhamento.

   ![Confirmar desmontagem](media/azure-stack-edge-manage-shares/confirm-unmount.png)

4. Depois que o compartilhamento estiver desmontado, vá para a lista de compartilhamentos. Você verá que a coluna **Usados para computação** mostra o status do compartilhamento como **Desabilitado**.

   ![Compartilhamento desmontado](media/azure-stack-edge-manage-shares/share-unmounted.png)

## <a name="delete-a-share"></a>Excluir um compartilhamento

Para excluir um compartilhamento, siga estas etapas no portal do Azure.

1. Na lista de compartilhamentos, selecione e clique no compartilhamento que você deseja excluir.

   ![Selecionar compartilhamento](media/azure-stack-edge-manage-shares/delete-share-1.png)

2. Clique em **Excluir**.

   ![Clique em Excluir](media/azure-stack-edge-manage-shares/delete-share-2.png)

3. Quando solicitada a confirmação, clique em **Sim**.

   ![Confirmar exclusão](media/azure-stack-edge-manage-shares/delete-share-3.png)

A lista de compartilhamentos é atualizada para refletir a exclusão.

## <a name="refresh-shares"></a>Atualizar compartilhamentos

O recurso de atualização permite atualizar o conteúdo de um compartilhamento. Quando você atualiza um compartilhamento, uma pesquisa é iniciada para localizar todos os objetos do Azure, incluindo os blobs e arquivos adicionados à nuvem desde a última atualização. Esses arquivos adicionais são então baixados para atualizar o conteúdo do compartilhamento no dispositivo.

> [!IMPORTANT]
>
> - Não é possível atualizar compartilhamentos locais.
> - As permissões e ACLs (listas de controle de acesso) não são preservadas em uma operação de atualização. 

Para atualizar um compartilhamento, siga estas etapas no portal do Azure.

1. No portal do Azure, acesse **Compartilhamentos**. Selecione e clique no compartilhamento que você deseja atualizar.

   ![Selecionar compartilhamento](media/azure-stack-edge-manage-shares/refresh-share-1.png)

2. Clique em **Atualizar**.

   ![Clique em atualizar](media/azure-stack-edge-manage-shares/refresh-share-2.png)
 
3. Quando solicitada a confirmação, clique em **Sim**. Um trabalho começa a atualizar o conteúdo do compartilhamento local.

   ![Confirmar atualização](media/azure-stack-edge-manage-shares/refresh-share-3.png)

4. Enquanto a atualização está em andamento, a opção de atualização fica esmaecida no menu de contexto. Clique na notificação de trabalho para exibir o status do trabalho de atualização.

5. O tempo para atualização depende do número de arquivos no contêiner do Azure, bem como dos arquivos no dispositivo. Depois que a atualização tiver sido concluída com êxito, o carimbo de data/hora de compartilhamento será atualizado. Mesmo que a atualização tenha falhas parciais, a operação será considerada bem-sucedida e o carimbo de data/hora será atualizado. Os logs de erros de atualização também são atualizados.

   ![Carimbo de data/hora atualizado](media/azure-stack-edge-manage-shares/refresh-share-4.png)
 
Se houver uma falha, um alerta será gerado. O alerta detalha a causa e a recomendação para corrigir o problema. O alerta também fornece links para um arquivo que tem o resumo completo das falhas, incluindo os arquivos que falharam em ser atualizados ou excluídos.

## <a name="sync-storage-keys"></a>Sincronizar chaves de armazenamento

Se tiver sido feita a rotação das chaves da conta de armazenamento, você precisará sincronizar as chaves de acesso de armazenamento. A sincronização ajuda o dispositivo obter as chaves mais recente para sua conta de armazenamento.

Execute as seguintes etapas no portal do Azure para sincronizar sua chave de acesso de armazenamento.

1. Vá para **Visão Geral** em seu recurso. Na lista de compartilhamentos, escolha e clique em um compartilhamento associado com a conta de armazenamento que você precisa sincronizar.

    ![Selecione o compartilhamento com a conta de armazenamento relevante](media/azure-stack-edge-manage-shares/sync-storage-key-1.png)

2. Clique em **Sincronizar chave de armazenamento**. Clique em **Sim** quando for solicitada a confirmação.

     ![Selecionar Sincronizar chave de armazenamento](media/azure-stack-edge-manage-shares/sync-storage-key-2.png)

3. Saia da caixa de diálogo depois que a sincronização tiver sido concluída.

>[!NOTE]
> Você precisa fazer isso apenas uma vez para uma conta de armazenamento específica. Você não precisa repetir essa ação para todos os compartilhamentos associados com a mesma conta de armazenamento.

## <a name="next-steps"></a>Próximas etapas

- Saiba como [Gerenciar usuários usando o portal do Azure](azure-stack-edge-manage-users.md).
