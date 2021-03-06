---
title: Criar instância do DMS (modelo do Azure Resource Manager)
description: Saiba como criar um Serviço de Migração de Banco de Dados usando o modelo do Azure Resource Manager.
author: MashaMSFT
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: mathoma
ms.date: 06/29/2020
ms.service: dms
ms.openlocfilehash: 9e0d537a948e960c90737cc3f367940a0ffca74c
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85852448"
---
# <a name="create-instance-of-azure-database-migration-service-azure-resource-manager-template"></a>Criar instância do Serviço de Migração de Banco de Dados do Azure (modelo do Azure Resource Manager)

Use esse modelo do Azure Resource Manager para implantar uma instância do Serviço de Migração de Banco de Dados do Azure. 

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Pré-requisitos

O modelo do ARM do Serviço de Migração de Banco de Dados do Azure requer o seguinte: 

- A versão mais recente da [CLI do Azure](/cli/azure/install-azure-cli?view=azure-cli-latest) e/ou do [PowerShell](/powershell/scripting/install/installing-powershell?view=powershell-7). 
- Uma assinatura do Azure. Se você não tiver uma, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="review-the-template"></a>Examinar o modelo

O modelo usado neste início rápido é proveniente dos [Modelos de Início Rápido do Azure](https://azure.microsoft.com/resources/templates/101-azure-database-migration-simple-deploy/).

:::code language="json" source="~/quickstart-templates/101-azure-database-migration-simple-deploy/azuredeploy.json" highlight="33-75":::

Há três recursos do Azure definidos no modelo: 

- [Microsoft.Network/virtualNetworks](/azure/templates/microsoft.network/virtualnetworks): Cria a rede virtual. 
- [Microsoft.Network/virtualNetworks/subnets](/azure/templates/microsoft.network/virtualnetworks/subnets): Cria a sub-rede. 
- [Microsoft.DataMigration/services](/azure/templates/microsoft.datamigration/services): Implanta uma instância do Serviço de Migração de Banco de Dados do Azure. 

Mais modelos de Serviços de Migração de Banco de Dados do Azure podem ser encontrados na [galeria de modelos de início rápido](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Datamigration).


## <a name="deploy-the-template"></a>Implantar o modelo

1. Selecione a imagem a seguir para entrar no Azure e abrir um modelo. O modelo cria uma instância do Serviço de Migração de Banco de Dados do Azure. 

   [![Implantar no Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-azure-database-migration-simple-deploy%2fazuredeploy.json)

2. Selecione ou insira os seguintes valores.

    * **Assinatura**: Selecione uma assinatura do Azure.
    * **Grupo de recursos**: Selecione um grupo de recursos existente na lista suspensa ou selecione **Criar** para criar um grupo de recursos. 
    * **Região**: Localização em que os recursos serão implantados.
    * **Nome do serviço**: nome do novo serviço de migração.
    * **Localização**: A localização do grupo de recursos, deixe como o padrão de `[resourceGroup().location]`.
    * **Nome da VNet**: nome da nova rede virtual.
    * **Nome da Sub-rede**: nome da nova sub-rede associada à rede virtual.



3. Selecione **Examinar + criar**. Depois que a instância do Serviço de Migração de Banco de Dados do Azure tiver sido implantada com êxito, você receberá uma notificação. 


O portal do Azure é usado para implantar o modelo. Além do portal do Azure, você também pode usar o Azure PowerShell, a CLI do Azure e a API REST. Para saber mais sobre outros métodos de implantação, confira [Implantar modelos](../azure-resource-manager/templates/deploy-powershell.md).

## <a name="review-deployed-resources"></a>Examinar os recursos implantados

Você pode usar a CLI do Azure para verificar os recursos implantados. 


```azurecli-interactive
echo "Enter the resource group where your SQL Server VM exists:" &&
read resourcegroupName &&
az resource list --resource-group $resourcegroupName 
```


## <a name="clean-up-resources"></a>Limpar os recursos

Quando você não precisar mais dele, exclua o grupo de recursos usando a CLI do Azure ou o Azure PowerShell:

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

---

## <a name="next-steps"></a>Próximas etapas

Para obter um tutorial passo a passo que orienta você durante o processo de criação de um modelo, confira:

> [!div class="nextstepaction"]
> [ Tutorial: Criar e implantar seu primeiro modelo do Azure Resource Manager](/azure/azure-resource-manager/templates/template-tutorial-create-first-template)

Para outras maneiras de implantar o Serviço de Migração de Banco de Dados do Azure, confira: 
- [Azure portal](quickstart-create-data-migration-service-portal.md)

Para saber mais, confira [uma visão geral do Serviço de Migração de Banco de Dados do Azure](dms-overview.md)