---
title: Provedores de recursos e tipos de recursos
description: Descreve os provedores de recursos que oferecem suporte ao Gerenciador de Recursos, aos respectivos esquemas e versões de API disponíveis, bem como às regiões que podem hospedar os recursos.
ms.topic: conceptual
ms.date: 08/29/2019
ms.openlocfilehash: 82b8251006a1a2d4edd198eca843489d3720f8d6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84702764"
---
# <a name="azure-resource-providers-and-types"></a>Provedores e tipos de recursos do Azure

Ao implantar recursos, com frequência você precisa recuperar informações sobre os provedores e tipos de recursos. Por exemplo, se você quer armazenar chaves e segredos, trabalhe com o provedor de recursos Microsoft.KeyVault . Este provedor de recursos oferece um tipo de recurso denominado cofres para criar o cofre da chave.

O nome de um tipo de recurso está no formato: **{provedor-de-recursos}/{tipo-de-recurso}**. O tipo de recurso de um cofre de chaves é **Microsoft.KeyVault/vaults**.

Neste artigo, você aprenderá como:

* Exibir todos os provedores de recursos no Azure
* Verificar o status do registro de um provedor de recursos
* Registrar um provedor de recursos
* Exibir os tipos de recurso para um provedor de recursos
* Exibir localizações válidas para um tipo de recurso
* Exibir versões de API válidas para um tipo de recurso

Você pode executar essas etapas por meio do portal do Azure, Azure PowerShell ou CLI do Azure.

Para obter uma lista que mapeia os provedores de recursos para os serviços do Azure, confira [Provedores de recursos para os serviços do Azure](azure-services-resource-providers.md).

## <a name="azure-portal"></a>Portal do Azure

Para ver todos os provedores de recursos e o status do registro para a sua assinatura:

1. Entre no [portal do Azure](https://portal.azure.com).
2. No menu do portal do Azure, selecione **Todos os serviços**.

    ![selecionar assinaturas](./media/resource-providers-and-types/select-all-services.png)

3. Na caixa **Todos os serviços** , digite **assinatura** e, em seguida, selecione **Assinaturas**.
4. Selecione a assinatura na lista de assinaturas para visualizá-la.
5. Selecione **Provedores de recursos** e exiba a lista de provedores de recursos disponíveis.

    ![mostrar provedores de recursos](./media/resource-providers-and-types/show-resource-providers.png)

6. O registro de um provedor de recursos configura sua assinatura para trabalhar com o provedor de recursos. O escopo de registro é sempre a assinatura. Por padrão, muitos provedores de recursos são automaticamente registrados. No entanto, talvez seja necessário registrar manualmente alguns provedores de recursos. Para registrar um provedor de recursos, você deve ter permissão para realizar a `/register/action` operação para o provedor de recursos. Esta operação está incluída nas funções de Colaborador e de Proprietário. Para registrar um provedor de recursos, selecione **Registrar**. Na captura de tela anterior, o link **Registrar** é destacado para **Microsoft.Blueprint**.

    Não é possível cancelar o registro de um provedor de recursos quando você ainda tem tipos de recursos desse provedor de recursos em sua assinatura.

Para obter informações para um provedor de recursos específico:

1. Entre no [portal do Azure](https://portal.azure.com).
2. No menu do portal do Azure, selecione **Todos os serviços**.
3. Na caixa **Todos os serviços**, digite **gerenciador de recursos** e, em seguida, selecione **Gerenciador de Recursos**.

    ![selecionar Todos os serviços](./media/resource-providers-and-types/select-resource-explorer.png)

4. Expanda **provedores** selecionando a seta para a direita.

    ![Selecionar provedores](./media/resource-providers-and-types/select-providers.png)

5. Expanda o provedor de recursos e o tipo de recurso que você deseja exibir.

    ![Selecionar tipo de recurso](./media/resource-providers-and-types/select-resource-type.png)

6. O Gerenciador de Recursos tem suporte em todas as regiões, mas os recursos que você implanta talvez não tenham suporte em todas as regiões. Além disso, pode haver limitações em sua assinatura que impedem o uso de algumas regiões que oferecem suporte aos recursos. O Resource Explorer mostra localizações válidas para o tipo de recurso.

    ![Mostrar localizações](./media/resource-providers-and-types/show-locations.png)

7. A versão disponível da API corresponde a uma versão das operações da API REST lançadas pelo provedor de recursos. Conforme um provedor de recursos habilita novos recursos, ele lança uma nova versão da API REST. O Resource Explorer mostra versões de API válidas para o tipo de recurso.

    ![Mostrar versões de API](./media/resource-providers-and-types/show-api-versions.png)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Para ver todos os provedores de recursos no Azure e o status do registro para a sua assinatura, use:

```azurepowershell-interactive
Get-AzResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

Que retorna resultados semelhantes a:

```output
ProviderNamespace                RegistrationState
-------------------------------- ------------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

O registro de um provedor de recursos configura sua assinatura para trabalhar com o provedor de recursos. O escopo de registro é sempre a assinatura. Por padrão, muitos provedores de recursos são automaticamente registrados. No entanto, talvez seja necessário registrar manualmente alguns provedores de recursos. Para registrar um provedor de recursos, você deve ter permissão para realizar a `/register/action` operação para o provedor de recursos. Esta operação está incluída nas funções de Colaborador e de Proprietário.

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
```

Que retorna resultados semelhantes a:

```output
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {West Europe, East US, East US 2, West US...}
```

Não é possível cancelar o registro de um provedor de recursos quando você ainda tem tipos de recursos desse provedor de recursos em sua assinatura.

Para obter informações para um provedor de recursos específico, use:

```azurepowershell-interactive
Get-AzResourceProvider -ProviderNamespace Microsoft.Batch
```

Que retorna resultados semelhantes a:

```output
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {West Europe, East US, East US 2, West US...}

...
```

Para ver os tipos de recurso para um provedor de recursos, use:

```azurepowershell-interactive
(Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

Que retorna:

```output
batchAccounts
operations
locations
locations/quotas
```

A versão disponível da API corresponde a uma versão das operações da API REST lançadas pelo provedor de recursos. Conforme um provedor de recursos habilita novos recursos, ele lança uma nova versão da API REST.

Para obter versões de API disponíveis para um tipo de recurso, use:

```azurepowershell-interactive
((Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

Que retorna:

```output
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

O Gerenciador de Recursos tem suporte em todas as regiões, mas os recursos que você implanta talvez não tenham suporte em todas as regiões. Além disso, pode haver limitações em sua assinatura que impedem o uso de algumas regiões que oferecem suporte aos recursos.

Para obter as localizações com suporte para um tipo de recurso, use.

```azurepowershell-interactive
((Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

Que retorna:

```output
West Europe
East US
East US 2
West US
...
```

## <a name="azure-cli"></a>CLI do Azure

Para ver todos os provedores de recursos no Azure e o status do registro para a sua assinatura, use:

```azurecli
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

Que retorna resultados semelhantes a:

```output
Provider                         Status
-------------------------------- ----------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

O registro de um provedor de recursos configura sua assinatura para trabalhar com o provedor de recursos. O escopo de registro é sempre a assinatura. Por padrão, muitos provedores de recursos são automaticamente registrados. No entanto, talvez seja necessário registrar manualmente alguns provedores de recursos. Para registrar um provedor de recursos, você deve ter permissão para realizar a `/register/action` operação para o provedor de recursos. Esta operação está incluída nas funções de Colaborador e de Proprietário.

```azurecli
az provider register --namespace Microsoft.Batch
```

Que retorna uma mensagem de que o registro está em andamento.

Não é possível cancelar o registro de um provedor de recursos quando você ainda tem tipos de recursos desse provedor de recursos em sua assinatura.

Para obter informações para um provedor de recursos específico, use:

```azurecli
az provider show --namespace Microsoft.Batch
```

Que retorna resultados semelhantes a:

```output
{
    "id": "/subscriptions/####-####/providers/Microsoft.Batch",
    "namespace": "Microsoft.Batch",
    "registrationsState": "Registering",
    "resourceTypes:" [
        ...
    ]
}
```

Para ver os tipos de recurso para um provedor de recursos, use:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

Que retorna:

```output
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

A versão disponível da API corresponde a uma versão das operações da API REST lançadas pelo provedor de recursos. Conforme um provedor de recursos habilita novos recursos, ele lança uma nova versão da API REST.

Para obter versões de API disponíveis para um tipo de recurso, use:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

Que retorna:

```output
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

O Gerenciador de Recursos tem suporte em todas as regiões, mas os recursos que você implanta talvez não tenham suporte em todas as regiões. Além disso, pode haver limitações em sua assinatura que impedem o uso de algumas regiões que oferecem suporte aos recursos.

Para obter as localizações com suporte para um tipo de recurso, use.

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

Que retorna:

```output
Result
---------------
West Europe
East US
East US 2
West US
...
```

## <a name="next-steps"></a>Próximas etapas

* Para saber mais sobre como criar modelos do Resource Manager, confira [criação de modelos de Azure Resource Manager](../templates/template-syntax.md). 
* Para exibir os esquemas de modelo de provedor de recursos, confira [Referência de modelo](/azure/templates/).
* Para obter uma lista que mapeia os provedores de recursos para os serviços do Azure, confira [Provedores de recursos para os serviços do Azure](azure-services-resource-providers.md).
* Para exibir as operações para um provedor de recursos, consulte [API REST do Azure](/rest/api/).
