---
title: Executar comandos do PowerShell com as credenciais do Azure AD para acessar dados de BLOB ou fila
titleSuffix: Azure Storage
description: O PowerShell dá suporte à entrada com credenciais do Azure AD para executar comandos no blob de armazenamento do Azure e dados de filas. Um token de acesso é fornecido para a sessão e usado para autorizar operações de chamada. As permissões dependem da função RBAC atribuída à entidade de segurança do Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/30/2019
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: 80ca5b63a91da31a5b226a589e15fb202eabd4ad
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84805770"
---
# <a name="run-powershell-commands-with-azure-ad-credentials-to-access-blob-or-queue-data"></a>Executar comandos do PowerShell com as credenciais do Azure AD para acessar dados de BLOB ou fila

O armazenamento do Azure fornece extensões para o PowerShell que permitem que você entre e execute comandos de script com as credenciais do Azure Active Directory (AD do Azure). Quando você entra no PowerShell com credenciais do Azure AD, um token de acesso OAuth 2,0 é retornado. Esse token é usado automaticamente pelo PowerShell para autorizar operações de dados subsequentes no armazenamento de BLOBs ou filas. Para operações com suporte, você não precisa passar uma chave de conta ou token SAS com o comando.

Você pode atribuir permissões a dados de BLOB e de fila a uma entidade de segurança do Azure AD por meio do RBAC (controle de acesso baseado em função). Para obter mais informações sobre as funções RBAC no armazenamento do Azure, consulte [gerenciar direitos de acesso aos dados do armazenamento do Azure com o RBAC](storage-auth-aad-rbac.md).

## <a name="supported-operations"></a>Operações com suporte

As extensões de armazenamento do Azure têm suporte para operações em dados de BLOB e de fila. As operações que você pode chamar dependem das permissões concedidas à entidade de segurança do Azure AD com a qual você entra no PowerShell. As permissões para contêineres ou filas de armazenamento do Azure são atribuídas por meio de RBAC. Por exemplo, se você tiver atribuído a função de **leitor de dados de blob** , poderá executar comandos de script que lêem dados de um contêiner ou fila. Se você tiver atribuído a função **colaborador de dados de blob** , poderá executar comandos de script que lêem, gravam ou excluem um contêiner ou uma fila ou os dados que eles contêm.

Para obter detalhes sobre as permissões necessárias para cada operação de armazenamento do Azure em um contêiner ou fila, consulte [chamar operações de armazenamento com tokens OAuth](/rest/api/storageservices/authorize-with-azure-active-directory#call-storage-operations-with-oauth-tokens).  

## <a name="call-powershell-commands-using-azure-ad-credentials"></a>Chamar comandos do PowerShell usando credenciais do Azure AD

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Para usar Azure PowerShell para entrar e executar operações subsequentes no armazenamento do Azure usando as credenciais do Azure AD, crie um contexto de armazenamento para fazer referência à conta de armazenamento e inclua o `-UseConnectedAccount` parâmetro.

O exemplo a seguir mostra como criar um contêiner em uma nova conta de armazenamento de Azure PowerShell usando suas credenciais do Azure AD. Lembre-se de substituir os valores dos espaços reservados entre colchetes angulares pelos seus próprios valores:

1. Entre em sua conta do Azure com o comando [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) :

    ```powershell
    Connect-AzAccount
    ```

    Para obter mais informações sobre como entrar no Azure com o PowerShell, consulte [entrar com Azure PowerShell](/powershell/azure/authenticate-azureps).

1. Crie um grupo de recursos do Azure chamando [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). 

    ```powershell
    $resourceGroup = "sample-resource-group-ps"
    $location = "eastus"
    New-AzResourceGroup -Name $resourceGroup -Location $location
    ```

1. Crie uma conta de armazenamento chamando [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount).

    ```powershell
    $storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
      -Name "<storage-account>" `
      -SkuName Standard_LRS `
      -Location $location `
    ```

1. Obtenha o contexto da conta de armazenamento que especifica a nova conta de armazenamento chamando [New-AzStorageContext](/powershell/module/az.storage/new-azstoragecontext). Ao atuar em uma conta de armazenamento, você pode fazer referência ao contexto em vez de passar repetidamente as credenciais. Inclua o `-UseConnectedAccount` parâmetro para chamar quaisquer operações de dados subsequentes usando suas credenciais do Azure AD:

    ```powershell
    $ctx = New-AzStorageContext -StorageAccountName "<storage-account>" -UseConnectedAccount
    ```

1. Antes de criar o contêiner, atribua a função [Colaborador de Dados do Blob de Armazenamento](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) a si mesmo. Embora você seja o proprietário da conta, você precisa de permissões explícitas para executar operações de dados na conta de armazenamento. Para obter mais informações sobre como atribuir funções RBAC, consulte [conceder acesso ao blob do Azure e dados de fila com RBAC no portal do Azure](storage-auth-aad-rbac.md).

    > [!IMPORTANT]
    > As atribuições de função do RBAC podem levar alguns minutos para serem propagadas.

1. Crie um contêiner chamando [New-AzStorageContainer](/powershell/module/az.storage/new-azstoragecontainer). Como essa chamada usa o contexto criado nas etapas anteriores, o contêiner é criado usando suas credenciais do Azure AD.

    ```powershell
    $containerName = "sample-container"
    New-AzStorageContainer -Name $containerName -Context $ctx
    ```

## <a name="next-steps"></a>Próximas etapas

- [Usar o PowerShell para atribuir uma função de RBAC para acesso a dados de BLOB e de fila](storage-auth-aad-rbac-powershell.md)
- [Autorizar o acesso a dados de BLOB e de fila com identidades gerenciadas para recursos do Azure](storage-auth-aad-msi.md)
