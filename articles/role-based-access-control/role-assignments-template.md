---
title: Adicionar atribuições de função do Azure usando modelos do Azure Resource Manager – Azure RBAC
description: Saiba como conceder acesso aos recursos do Azure para usuários, grupos, entidades de serviço ou identidades gerenciadas usando modelos do Azure Resource Manager e o Azure RBAC (controle de acesso baseado em função do Azure).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/26/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: e26f2ed498b8bfcf6b1518ea34815efb75a8eabe
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85392447"
---
# <a name="add-azure-role-assignments-using-azure-resource-manager-templates"></a>Adicionar atribuições de função do Azure usando modelos do Azure Resource Manager

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)] Além de usar o Azure PowerShell ou a CLI do Azure, você pode atribuir funções usando [Modelos do Azure Resource Manager](../azure-resource-manager/templates/template-syntax.md). Os modelos poderão ser úteis se você precisar implantar recursos de maneira consistente e repetida. Este artigo descreve como atribuir funções usando modelos.

## <a name="get-object-ids"></a>Obter IDs de objeto

Para atribuir uma função, você precisa especificar a ID do usuário, grupo ou aplicativo ao qual deseja atribuir a função. A ID tem o formato: `11111111-1111-1111-1111-111111111111`. Você pode obter a ID usando o portal do Azure, o Azure PowerShell ou a CLI do Azure.

### <a name="user"></a>Usuário

Para obter a ID de um usuário, você pode usar os comandos [Get-AzADUser](/powershell/module/az.resources/get-azaduser) ou [az ad user show](/cli/azure/ad/user#az-ad-user-show).

```azurepowershell
$objectid = (Get-AzADUser -DisplayName "{name}").id
```

```azurecli
objectid=$(az ad user show --id "{email}" --query objectId --output tsv)
```

### <a name="group"></a>Agrupar

Para obter a ID de um grupo, você pode usar os comandos [Get-AzADGroup](/powershell/module/az.resources/get-azadgroup) ou [az ad group show](/cli/azure/ad/group#az-ad-group-show).

```azurepowershell
$objectid = (Get-AzADGroup -DisplayName "{name}").id
```

```azurecli
objectid=$(az ad group show --group "{name}" --query objectId --output tsv)
```

### <a name="application"></a>Aplicativo

Para obter a ID de uma entidade de serviço (identidade usada por um aplicativo), você pode usar os comandos [Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal) ou [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list). Para uma entidade de serviço, use a ID de objeto, e **não** a ID do aplicativo.

```azurepowershell
$objectid = (Get-AzADServicePrincipal -DisplayName "{name}").id
```

```azurecli
objectid=$(az ad sp list --display-name "{name}" --query [].objectId --output tsv)
```

## <a name="add-a-role-assignment"></a>Adicionar uma atribuição de função

Para conceder acesso no Azure RBAC, adicione uma atribuição de função.

### <a name="resource-group-scope-without-parameters"></a>Escopo do grupo de recursos (sem parâmetros)

O modelo a seguir mostra uma maneira básica de adicionar uma atribuição de função. Alguns valores são especificados no modelo. O modelo a seguir demonstra:

-  Como atribuir a função de [Leitor](built-in-roles.md#reader) a um usuário, grupo ou aplicativo em um escopo de grupo de recursos

Para usar o modelo, você deve fazer o seguinte:

- Criar um arquivo JSON e copiar o modelo
- Substituir `<your-principal-id>` pela ID de um usuário, grupo ou aplicativo ao qual atribuir a função

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[guid(resourceGroup().id)]",
            "properties": {
                "roleDefinitionId": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
                "principalId": "<your-principal-id>"
            }
        }
    ]
}
```

Aqui estão exemplos dos comandos [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) e [az group deployment create](/cli/azure/group/deployment#az-group-deployment-create) para saber como iniciar a implantação em um grupo de recursos chamado ExampleGroup.

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-test.json
```

```azurecli
az group deployment create --resource-group ExampleGroup --template-file rbac-test.json
```

Confira a seguir um exemplo de atribuição de função de Leitor a um usuário em um grupo de recursos após a implantação do modelo.

![Atribuição de função no escopo do grupo de recursos](./media/role-assignments-template/role-assignment-template.png)

### <a name="resource-group-or-subscription-scope"></a>Escopo do grupo de recursos ou da assinatura

O modelo anterior não é muito flexível. O modelo a seguir usa parâmetros e pode ser usado em diferentes escopos. O modelo a seguir demonstra:

- Como atribuir uma função a um usuário, grupo ou aplicativo em um escopo de grupo de recursos ou assinatura
- Como especificar as funções de Proprietário, Colaborador e Leitor como um parâmetro

Para usar o modelo, você deve especificar as seguintes entradas:

- A ID de um usuário, grupo ou aplicativo ao qual atribuir a função
- Uma ID exclusiva que será usada para a atribuição de função, ou você poderá usar a ID padrão

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "principalId": {
            "type": "string",
            "metadata": {
                "description": "The principal to assign the role to"
            }
        },
        "builtInRoleType": {
            "type": "string",
            "allowedValues": [
                "Owner",
                "Contributor",
                "Reader"
            ],
            "metadata": {
                "description": "Built-in role to assign"
            }
        },
        "roleNameGuid": {
            "type": "string",
            "defaultValue": "[newGuid()]",
            "metadata": {
                "description": "A new GUID used to identify the role assignment"
            }
        }
    },
    "variables": {
        "Owner": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
        "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
        "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]"
    },
    "resources": [
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[parameters('roleNameGuid')]",
            "properties": {
                "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
                "principalId": "[parameters('principalId')]"
            }
        }
    ]
}
```

> [!NOTE]
> Esse modelo não é idempotente, a menos que o mesmo valor de `roleNameGuid` seja fornecido como um parâmetro para cada implantação do modelo. Se nenhum `roleNameGuid` for fornecido, por padrão, um novo GUID será gerado em cada implantação, e implantações subsequentes falharão com um erro `Conflict: RoleAssignmentExists`.

O escopo da atribuição de função é determinado no nível da implantação. Aqui estão exemplos dos comandos [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) e [az group deployment create](/cli/azure/group/deployment#az-group-deployment-create) para saber como iniciar a implantação em um escopo do grupo de recursos.

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-test.json -principalId $objectid -builtInRoleType Reader
```

```azurecli
az group deployment create --resource-group ExampleGroup --template-file rbac-test.json --parameters principalId=$objectid builtInRoleType=Reader
```

Aqui estão exemplos dos comandos [New-AzDeployment](/powershell/module/az.resources/new-azdeployment) and [az deployment create](/cli/azure/deployment#az-deployment-create) para saber como iniciar a implantação em um escopo de assinatura e especificar o local.

```azurepowershell
New-AzDeployment -Location centralus -TemplateFile rbac-test.json -principalId $objectid -builtInRoleType Reader
```

```azurecli
az deployment create --location centralus --template-file rbac-test.json --parameters principalId=$objectid builtInRoleType=Reader
```

### <a name="resource-scope"></a>Escopo do recurso

Se você precisar adicionar uma atribuição de função no nível de um recurso, o formato da atribuição de função será diferente. Forneça o namespace do provedor de recursos e o tipo do recurso ao qual atribuir a função. Inclua também o nome do recurso no nome da atribuição de função.

Para o tipo e o nome da atribuição de função, use o seguinte formato:

```json
"type": "{resource-provider-namespace}/{resource-type}/providers/roleAssignments",
"name": "{resource-name}/Microsoft.Authorization/{role-assign-GUID}"
```

O modelo a seguir demonstra:

- Como criar uma nova conta de armazenamento
- Como atribuir uma função a um usuário, grupo ou aplicativo no escopo da conta de armazenamento
- Como especificar as funções de Proprietário, Colaborador e Leitor como um parâmetro

Para usar o modelo, você deve especificar as seguintes entradas:

- A ID de um usuário, grupo ou aplicativo ao qual atribuir a função

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "principalId": {
            "type": "string",
            "metadata": {
                "description": "The principal to assign the role to"
            }
        },
        "builtInRoleType": {
            "type": "string",
            "allowedValues": [
                "Owner",
                "Contributor",
                "Reader"
            ],
            "metadata": {
                "description": "Built-in role to assign"
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {
        "Owner": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
        "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
        "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
        "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageName')]",
            "location": "[parameters('location')]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        },
        {
            "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[concat(variables('storageName'), '/Microsoft.Authorization/', guid(uniqueString(variables('storageName'))))]",
            "dependsOn": [
                "[variables('storageName')]"
            ],
            "properties": {
                "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
                "principalId": "[parameters('principalId')]"
            }
        }
    ]
}
```

Para implantar o modelo anterior, use os comandos de grupo de recursos. Aqui estão exemplos dos comandos [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) e [az group deployment create](/cli/azure/group/deployment#az-group-deployment-create) para saber como iniciar a implantação em um escopo de recurso.

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-test.json -principalId $objectid -builtInRoleType Contributor
```

```azurecli
az group deployment create --resource-group ExampleGroup --template-file rbac-test.json --parameters principalId=$objectid builtInRoleType=Contributor
```

Confira a seguir um exemplo de atribuição de função de Colaborador a um usuário para uma conta de armazenamento depois de implantar o modelo.

![Atribuição de função no escopo de recurso](./media/role-assignments-template/role-assignment-template-resource.png)

### <a name="new-service-principal"></a>Nova entidade de serviço

Se você criar uma entidade de serviço e tentar atribuir uma função imediatamente a essa entidade de serviço, essa atribuição de função poderá falhar em alguns casos. Por exemplo, se você criar uma identidade gerenciada e, em seguida, tentar atribuir uma função a essa entidade no mesmo modelo do Azure Resource Manager, a atribuição de função poderá falhar. Provavelmente, o motivo dessa falha é um atraso de replicação. A entidade de serviço é criada em uma região; no entanto, a atribuição de função pode ocorrer em uma região diferente que ainda não tenha replicado a entidade de serviço. Para resolver esse cenário, você deve definir a propriedade `principalType` como `ServicePrincipal` ao criar a atribuição de função.

O modelo a seguir demonstra:

- Como criar uma entidade de serviço de identidade gerenciada
- Como especificar o `principalType`
- Como atribuir a função de Colaborador a essa entidade de serviço em um escopo de grupo de recursos

Para usar o modelo, você deve especificar as seguintes entradas:

- O nome de base da identidade gerenciada, ou você pode usar a cadeia de caracteres padrão

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "baseName": {
            "type": "string",
            "defaultValue": "msi-test"
        }
    },
    "variables": {
        "identityName": "[concat(parameters('baseName'), '-bootstrap')]",
        "bootstrapRoleAssignmentId": "[guid(concat(resourceGroup().id, 'contributor'))]",
        "contributorRoleDefinitionId": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]"
    },
    "resources": [
        {
            "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
            "name": "[variables('identityName')]",
            "apiVersion": "2018-11-30",
            "location": "[resourceGroup().location]"
        },
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[variables('bootstrapRoleAssignmentId')]",
            "dependsOn": [
                "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', variables('identityName'))]"
            ],
            "properties": {
                "roleDefinitionId": "[variables('contributorRoleDefinitionId')]",
                "principalId": "[reference(resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', variables('identityName')), '2018-11-30').principalId]",
                "scope": "[resourceGroup().id]",
                "principalType": "ServicePrincipal"
            }
        }
    ]
}
```

Aqui estão exemplos dos comandos [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) e [az group deployment create](/cli/azure/group/deployment#az-group-deployment-create) para saber como iniciar a implantação em um escopo do grupo de recursos.

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup2 -TemplateFile rbac-test.json
```

```azurecli
az group deployment create --resource-group ExampleGroup2 --template-file rbac-test.json
```

Confira a seguir um exemplo da atribuição de função de Colaborador para uma nova entidade de serviço de identidade gerenciada após a implantação do modelo.

![Atribuição de função para uma nova entidade de serviço de identidade gerenciada](./media/role-assignments-template/role-assignment-template-msi.png)

## <a name="remove-a-role-assignment"></a>Excluir uma atribuição de função

No Azure RBAC, para remover o acesso a um recurso do Azure, remova a atribuição de função. Não há uma maneira de remover uma atribuição de função usando um modelo. Para remover uma atribuição de função, você deve usar outras ferramentas, como:

- [Azure portal](role-assignments-portal.md#remove-a-role-assignment)
- [PowerShell do Azure](role-assignments-powershell.md#remove-a-role-assignment)
- [CLI do Azure](role-assignments-cli.md#remove-a-role-assignment)
- [REST API](role-assignments-rest.md#remove-a-role-assignment)

## <a name="next-steps"></a>Próximas etapas

- [Início Rápido: Criar e implantar modelos do Azure Resource Manager usando o portal do Azure](../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md)
- [Noções básicas de estrutura e sintaxe dos modelos do Azure Resource Manager](../azure-resource-manager/templates/template-syntax.md)
- [Criar grupos de recursos e recursos no nível da assinatura](../azure-resource-manager/templates/deploy-to-subscription.md)
- [Modelos de Início Rápido do Azure](https://azure.microsoft.com/resources/templates/?term=rbac)
