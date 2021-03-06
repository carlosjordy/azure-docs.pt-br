---
title: Funções personalizadas do Azure – RBAC do Azure
description: Saiba como criar funções personalizadas do Azure com o Azure RBAC (controle de acesso baseado em função) para o gerenciamento de acesso refinado de recursos do Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: e4206ea9-52c3-47ee-af29-f6eef7566fa5
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fd737a22a37d6edc47c2769a470af00537d720eb
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87124146"
---
# <a name="azure-custom-roles"></a>Funções personalizadas do Azure

> [!IMPORTANT]
> A adição de um grupo de gerenciamento ao `AssignableScopes` está em visualização no momento.
> Essa versão prévia é fornecida sem um contrato de nível de serviço e não é recomendada para cargas de trabalho de produção. Alguns recursos podem não ter suporte ou podem ter restrição de recursos.
> Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Se as [funções internas do Azure](built-in-roles.md) não atenderem às necessidades específicas de sua organização, você poderá criar funções personalizadas próprias. Assim como as funções internas, você pode atribuir funções personalizadas a usuários, grupos e entidades de serviço em escopos grupo de gerenciamento, assinatura e grupo de recursos.

As funções personalizadas podem ser compartilhadas entre assinaturas que confiam no mesmo diretório do Azure AD. Há um limite de **5.000** funções personalizadas por diretório. (Para Azure Alemanha e Azure China 21Vianet, o limite é de 2.000 funções personalizadas.) As funções personalizadas podem ser criadas usando o portal do Azure, Azure PowerShell, CLI do Azure ou a API REST.

## <a name="custom-role-example"></a>Exemplo de função personalizada

O exemplo a seguir mostra a aparência de uma função personalizada, conforme exibido usando Azure PowerShell no formato JSON. Essa função personalizada pode ser usada para monitorar e reiniciar máquinas virtuais.

```json
{
  "Name": "Virtual Machine Operator",
  "Id": "88888888-8888-8888-8888-888888888888",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.ResourceHealth/availabilityStatuses/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
    "/subscriptions/{subscriptionId1}",
    "/subscriptions/{subscriptionId2}",
    "/providers/Microsoft.Management/managementGroups/{groupId1}"
  ]
}
```

O seguinte mostra a mesma função personalizada que é exibida usando CLI do Azure.

```json
[
  {
    "assignableScopes": [
      "/subscriptions/{subscriptionId1}",
      "/subscriptions/{subscriptionId2}",
      "/providers/Microsoft.Management/managementGroups/{groupId1}"
    ],
    "description": "Can monitor and restart virtual machines.",
    "id": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleDefinitions/88888888-8888-8888-8888-888888888888",
    "name": "88888888-8888-8888-8888-888888888888",
    "permissions": [
      {
        "actions": [
          "Microsoft.Storage/*/read",
          "Microsoft.Network/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action",
          "Microsoft.Authorization/*/read",
          "Microsoft.ResourceHealth/availabilityStatuses/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Insights/diagnosticSettings/*",
          "Microsoft.Support/*"
        ],
        "dataActions": [],
        "notActions": [],
        "notDataActions": []
      }
    ],
    "roleName": "Virtual Machine Operator",
    "roleType": "CustomRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

Quando você cria uma função personalizada, ela aparece no portal do Azure com um ícone de recurso laranja.

![Ícone personalizado da função](./media/custom-roles/roles-custom-role-icon.png)

## <a name="custom-role-properties"></a>Propriedades da função personalizada

A tabela a seguir descreve o significado das propriedades da função personalizada.

| Propriedade | Obrigatório | Type | Descrição |
| --- | --- | --- | --- |
| `Name`</br>`roleName` | Sim | Cadeia de caracteres | O nome de exibição da função personalizada. Embora uma definição de função seja um grupo de gerenciamento ou recurso de nível de assinatura, uma definição de função pode ser usada em várias assinaturas que compartilham o mesmo diretório do AD do Azure. Esse nome de exibição precisa ser exclusivo no escopo do diretório do Azure AD. Pode incluir letras, números, espaços e caracteres especiais. O número máximo de caracteres é 128. |
| `Id`</br>`name` | Sim | Cadeia de caracteres | A ID exclusiva da função personalizada. Para o Azure PowerShell e a CLI do Azure, essa ID é gerada automaticamente ao criar uma nova função. |
| `IsCustom`</br>`roleType` | Sim | Cadeia de caracteres | Indica se esta é uma função personalizada. Defina como `true` ou `CustomRole` para funções personalizadas. Defina como `false` ou `BuiltInRole` para funções internas. |
| `Description`</br>`description` | Sim | Cadeia de caracteres | A descrição da função personalizada. Pode incluir letras, números, espaços e caracteres especiais. O número máximo de caracteres é 1024. |
| `Actions`</br>`actions` | Sim | String[] | Uma matriz de cadeias de caracteres que especifica as operações de gerenciamento permitidas pela função. Para obter mais informações, consulte [Ações](role-definitions.md#actions). |
| `NotActions`</br>`notActions` | Não | String[] | Uma matriz de cadeias de caracteres que especifica as operações de gerenciamento que são excluídas do `Actions` permitido. Para obter mais informações, consulte [NotActions](role-definitions.md#notactions). |
| `DataActions`</br>`dataActions` | Não | String[] | Uma matriz de cadeias de caracteres que especifica as operações de dados permitidas pela função em seus dados dentro desse objeto. Se você criar uma função personalizada com `DataActions` , essa função não poderá ser atribuída ao escopo do grupo de gerenciamento. Para obter mais informações, consulte [Dataactions](role-definitions.md#dataactions). |
| `NotDataActions`</br>`notDataActions` | Não | String[] | Uma matriz de cadeias de caracteres que especifica as operações de dados excluídas do `DataActions` permitido. Para obter mais informações, consulte [NotDataActions](role-definitions.md#notdataactions). |
| `AssignableScopes`</br>`assignableScopes` | Sim | String[] | Uma matriz de cadeias de caracteres que especifica os escopos para os quais a função personalizada está disponível para atribuição. Você só pode definir um grupo de gerenciamento em `AssignableScopes` uma função personalizada. A adição de um grupo de gerenciamento ao `AssignableScopes` está em visualização no momento. Para obter mais informações, consulte [AssignableScopes](role-definitions.md#assignablescopes). |

## <a name="wildcard-permissions"></a>Permissões de curinga

`Actions`, `NotActions` , `DataActions` e `NotDataActions` suportam curingas ( `*` ) para definir permissões. Um curinga ( `*` ) estende uma permissão para tudo que corresponde à cadeia de caracteres de ação que você fornece. Por exemplo, suponha que você quisesse adicionar todas as permissões relacionadas ao gerenciamento de custos e às exportações do Azure. Você pode adicionar todas essas cadeias de caracteres de ação:

```
Microsoft.CostManagement/exports/action
Microsoft.CostManagement/exports/read
Microsoft.CostManagement/exports/write
Microsoft.CostManagement/exports/delete
Microsoft.CostManagement/exports/run/action
```

Em vez de adicionar todas essas cadeias de caracteres, basta adicionar uma cadeia de caracteres curinga. Por exemplo, a cadeia curinga a seguir é equivalente às cinco cadeias de caracteres anteriores. Isso também incluiria permissões de exportação futuras que possam ser adicionadas.

```
Microsoft.CostManagement/exports/*
```

Você também pode ter vários curingas em uma cadeia de caracteres. Por exemplo, a cadeia de caracteres a seguir representa todas as permissões de consulta para o gerenciamento de custos.

```
Microsoft.CostManagement/*/query/*
```

## <a name="steps-to-create-a-custom-role"></a>Etapas para criar uma função personalizada

Para criar uma função personalizada, aqui estão as etapas básicas que você deve seguir.

1. Decida como você deseja criar a função personalizada.

    Você pode criar funções personalizadas usando portal do Azure, Azure PowerShell, CLI do Azure ou a API REST.

1. Determine as permissões necessárias.

    Ao criar uma função personalizada, você precisa saber as operações que estão disponíveis para definir suas permissões. Para exibir a lista de operações, consulte o [Azure Resource Manager operações do provedor de recursos](resource-provider-operations.md). Você adicionará as operações às `Actions` Propriedades ou `NotActions` da definição de [função](role-definitions.md). Se houver operações de dados, você as adicionará às `DataActions` Propriedades ou `NotDataActions` .

1. Crie a função personalizada.

    Normalmente, você começa com uma função interna existente e depois a modifica conforme suas necessidades. A maneira mais fácil é usar o portal do Azure. Para obter as etapas sobre como criar uma função personalizada usando o portal do Azure, consulte [criar ou atualizar funções personalizadas do Azure usando o portal do Azure](custom-roles-portal.md).

1. Teste a função personalizada.

    Quando tiver sua função personalizada, será necessário testá-la para verificar se funciona como o esperado. Se você precisar fazer ajustes mais tarde, você pode atualizar a função personalizada.

## <a name="who-can-create-delete-update-or-view-a-custom-role"></a>Quem pode criar, excluir, atualizar ou exibir uma função personalizada

Assim como funções internas, a propriedade `AssignableScopes` especifica os escopos para os quais a função está disponível para atribuição. A propriedade `AssignableScopes` de uma função personalizada também controla quem pode criar, excluir, atualizar ou exibir a função personalizada.

| Tarefa | Operação | Descrição |
| --- | --- | --- |
| Criar/excluir uma função personalizada | `Microsoft.Authorization/ roleDefinitions/write` | Os usuários que recebem essa operação em todos os `AssignableScopes` da função personalizada podem criar (ou excluir) funções personalizadas para uso nesses escopos. Por exemplo, [proprietários](built-in-roles.md#owner) e [Administradores de acesso de usuário](built-in-roles.md#user-access-administrator) de grupos de gerenciamento, assinaturas e grupos de recursos. |
| Atualizar uma função personalizada | `Microsoft.Authorization/ roleDefinitions/write` | Os usuários que recebem essa operação em todos os `AssignableScopes` da função personalizada podem atualizar as funções personalizadas nesses escopos. Por exemplo, [proprietários](built-in-roles.md#owner) e [Administradores de acesso de usuário](built-in-roles.md#user-access-administrator) de grupos de gerenciamento, assinaturas e grupos de recursos. |
| Exibir uma função personalizada | `Microsoft.Authorization/ roleDefinitions/read` | Os usuários que recebem essa operação em um escopo podem exibir as funções personalizadas que estão disponíveis para atribuição nesse escopo. Todas as funções internas permitem que as funções personalizadas sejam disponibilizadas para atribuição. |

## <a name="custom-role-limits"></a>Limites de função personalizada

A lista a seguir descreve os limites para funções personalizadas.

- Cada diretório pode ter até **5000** funções personalizadas.
- O Azure Alemanha e o Azure China 21Vianet podem ter até 2000 funções personalizadas para cada diretório.
- Não é possível definir `AssignableScopes` como o escopo raiz ( `"/"` ).
- Você só pode definir um grupo de gerenciamento em `AssignableScopes` uma função personalizada. A adição de um grupo de gerenciamento ao `AssignableScopes` está em visualização no momento.
- Funções personalizadas com `DataActions` não podem ser atribuídas no escopo do grupo de gerenciamento.
- Azure Resource Manager não valida a existência do grupo de gerenciamento no escopo atribuível da definição de função.

Para obter mais informações sobre funções personalizadas e grupos de gerenciamento, consulte [organizar seus recursos com grupos de gerenciamento do Azure](../governance/management-groups/overview.md#azure-custom-role-definition-and-assignment).

## <a name="input-and-output-formats"></a>Formatos de entrada e saída

Para criar uma função personalizada usando a linha de comando, você normalmente usa o JSON para especificar as propriedades desejadas para a função personalizada. Dependendo das ferramentas usadas, os formatos de entrada e saída terão uma aparência ligeiramente diferente. Esta seção lista os formatos de entrada e saída, dependendo da ferramenta.

### <a name="azure-powershell"></a>Azure PowerShell

Para criar uma função personalizada usando Azure PowerShell, você deve fornecer a entrada a seguir.

```json
{
  "Name": "",
  "Description": "",
  "Actions": [],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": []
}
```

Para atualizar uma função personalizada usando Azure PowerShell, você deve fornecer a entrada a seguir. Observe que a `Id` propriedade foi adicionada. 

```json
{
  "Name": "",
  "Id": "",
  "Description": "",
  "Actions": [],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": []
}
```

Veja a seguir um exemplo de saída quando você lista uma função personalizada usando Azure PowerShell e o comando [ConvertTo-JSON](/powershell/module/microsoft.powershell.utility/convertto-json) . 

```json
{
  "Name": "",
  "Id": "",
  "IsCustom": true,
  "Description": "",
  "Actions": [],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": []
}
```

### <a name="azure-cli"></a>CLI do Azure

Para criar ou atualizar uma função personalizada usando CLI do Azure, você deve fornecer a entrada a seguir. Esse formato é o mesmo formato quando você cria uma função personalizada usando Azure PowerShell.

```json
{
  "Name": "",
  "Description": "",
  "Actions": [],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": []
}
```

Veja a seguir um exemplo de saída ao listar uma função personalizada usando CLI do Azure.

```json
[
  {
    "assignableScopes": [],
    "description": "",
    "id": "",
    "name": "",
    "permissions": [
      {
        "actions": [],
        "dataActions": [],
        "notActions": [],
        "notDataActions": []
      }
    ],
    "roleName": "",
    "roleType": "CustomRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

### <a name="rest-api"></a>API REST

Para criar ou atualizar uma função personalizada usando a API REST, você deve fornecer a entrada a seguir. Esse formato é o mesmo formato que é gerado quando você cria uma função personalizada usando o portal do Azure.

```json
{
  "properties": {
    "roleName": "",
    "description": "",
    "assignableScopes": [],
    "permissions": [
      {
        "actions": [],
        "notActions": [],
        "dataActions": [],
        "notDataActions": []
      }
    ]
  }
}
```

Veja a seguir um exemplo de saída quando você lista uma função personalizada usando a API REST.

```json
{
    "properties": {
        "roleName": "",
        "type": "CustomRole",
        "description": "",
        "assignableScopes": [],
        "permissions": [
            {
                "actions": [],
                "notActions": [],
                "dataActions": [],
                "notDataActions": []
            }
        ],
        "createdOn": "",
        "updatedOn": "",
        "createdBy": "",
        "updatedBy": ""
    },
    "id": "",
    "type": "Microsoft.Authorization/roleDefinitions",
    "name": ""
}
```

## <a name="next-steps"></a>Próximas etapas

- [Tutorial: Criar uma função personalizada do Azure usando o Azure PowerShell](tutorial-custom-role-powershell.md)
- [Tutorial: Criar uma função personalizada do Azure usando a CLI do Azure](tutorial-custom-role-cli.md)
- [Entender as definições de função do Azure](role-definitions.md)
- [Solucionar problemas do RBAC do Azure](troubleshooting.md)
