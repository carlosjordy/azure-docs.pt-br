---
title: Autenticar com identidades gerenciadas
description: Acessar recursos protegidos por Azure Active Directory sem entrar com credenciais ou segredos usando uma identidade gerenciada
services: logic-apps
ms.suite: integration
ms.reviewer: jonfan, logicappspm
ms.topic: article
ms.date: 02/10/2020
ms.openlocfilehash: 06c10cffcfa5c68b1da8ba366ca270f1c2fa6ea4
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87060972"
---
# <a name="authenticate-access-to-azure-resources-by-using-managed-identities-in-azure-logic-apps"></a>Autenticar o acesso a recursos do Azure usando identidades gerenciadas em Aplicativos Lógicos do Azure

Para acessar facilmente outros recursos protegidos pelo Azure AD (Azure Active Directory) e autenticar sua identidade sem precisar entrar, o aplicativo lógico pode usar uma [identidade gerenciada](../active-directory/managed-identities-azure-resources/overview.md) (anteriormente conhecida como Identidade de Serviço Gerenciada ou MSI), em vez de credenciais ou segredos. O Azure gerencia essa identidade para você e ajuda a proteger suas credenciais, porque você não precisa fornecer ou trocar segredos.

Os Aplicativos Lógicos do Azure dão suporte a identidades gerenciadas [*atribuídas pelo sistema*](../active-directory/managed-identities-azure-resources/overview.md) e [*atribuídas pelo usuário*](../active-directory/managed-identities-azure-resources/overview.md). Seu aplicativo lógico pode usar a identidade atribuída ao sistema ou uma *única* identidade atribuída ao usuário, que você pode compartilhar em um grupo de aplicativos lógicos, mas não ambas. No momento, somente [gatilhos e ações internos específicos](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound) dão suporte a identidades gerenciadas, não conectores ou conexões gerenciadas, por exemplo:

* HTTP
* Funções do Azure
* Gerenciamento de API do Azure
* Serviços de Aplicativo do Azure

Este artigo mostra como configurar os dois tipos de identidades gerenciadas para seu aplicativo lógico. Para saber mais, consulte esses tópicos:

* [Gatilhos e ações que dão suporte a identidades gerenciadas](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)
* [Tipos de autenticação com suporte em chamadas de saída](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)
* [Limites de identidade gerenciada para aplicativos lógicos](../logic-apps/logic-apps-limits-and-config.md#managed-identity)
* [Serviços do Azure que dão suporte à autenticação do Azure AD com identidades gerenciadas](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)

## <a name="prerequisites"></a>Pré-requisitos

* Uma assinatura do Azure. Se você não tem uma assinatura, [inscreva-se em uma conta gratuita do Azure](https://azure.microsoft.com/free/). A identidade gerenciada e o recurso do Azure de destino em que você precisa de acesso devem usar a mesma assinatura do Azure.

* Para conceder a uma identidade gerenciada acesso a um recurso do Azure, você precisa adicionar uma função ao recurso de destino para essa identidade. Para adicionar funções, você precisa de [permissões de administrador do Azure AD](../active-directory/users-groups-roles/directory-assign-admin-roles.md) que podem atribuir funções a identidades no locatário do Azure AD correspondente.

* O recurso do Azure de destino que você deseja acessar. Nesse recurso, você adicionará uma função à identidade gerenciada, que ajuda o aplicativo lógico a autenticar o acesso ao recurso de destino.

* O aplicativo lógico no qual você deseja usar o [gatilho ou ações que dão suporte a identidades gerenciadas](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)

## <a name="enable-managed-identity"></a>Habilitar identidade gerenciada

Para configurar a identidade gerenciada que você deseja usar, siga o link para essa identidade:

* [Identidade atribuída ao sistema](#system-assigned)
* [Identidade atribuída ao usuário](#user-assigned)

<a name="system-assigned"></a>

### <a name="enable-system-assigned-identity"></a>Habilitar identidade atribuída ao sistema

Diferentemente de identidades atribuídas pelo sistema, você não precisa criar manualmente a identidade atribuída ao sistema. Para configurar a identidade atribuída ao sistema para seu aplicativo lógico, estão são as opções que você pode usar:

* [Azure portal](#azure-portal-system-logic-app)
* [Modelos do Gerenciador de Recursos do Azure](#template-system-logic-app)

<a name="azure-portal-system-logic-app"></a>

#### <a name="enable-system-assigned-identity-in-azure-portal"></a>Habilitar a identidade atribuída ao sistema no portal do Azure

1. No [Portal do Azure](https://portal.azure.com), abra o aplicativo lógico no Designer do Aplicativo Lógico.

1. No menu do aplicativo lógico, em **Configurações**, selecione **Identidade**. Selecione **Atribuída ao sistema** > **Ativada** > **Salvar**. Quando o Azure solicitar que você confirme, selecione **Sim**.

   ![Habilitar a identidade atribuída ao sistema](./media/create-managed-service-identity/enable-system-assigned-identity.png)

   > [!NOTE]
   > Se você receber um erro de que você pode ter apenas uma identidade gerenciada, seu aplicativo lógico já estará associado à identidade atribuída ao usuário. Para adicionar a identidade atribuída ao sistema, primeiro *remova* a identidade atribuída ao usuário do seu aplicativo lógico.

   Seu aplicativo lógico agora pode usar a identidade atribuída ao sistema, que é registrada no Azure Active Directory e representada por uma ID de objeto.

   ![ID de objeto da identidade atribuída ao sistema](./media/create-managed-service-identity/object-id-system-assigned-identity.png)

   | Propriedade | Valor | Descrição |
   |----------|-------|-------------|
   | **ID do objeto** | <*identity-resource-ID*> | Um GUID (identificador global exclusivo) que representa a identidade atribuída ao sistema para o seu aplicativo lógico em um locatário do Azure AD |
   ||||

1. Agora siga as [etapas que fornecem a essa identidade acesso ao recurso](#access-other-resources) mais adiante neste tópico.

<a name="template-system-logic-app"></a>

#### <a name="enable-system-assigned-identity-in-azure-resource-manager-template"></a>Habilitar identidade atribuída ao sistema no modelo do Azure Resource Manager

Para automatizar a criação e a implantação de recursos do Azure, como os aplicativos lógicos, você pode usar [modelos do Azure Resource Manager](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md). Para habilitar a identidade gerenciada atribuída ao sistema para seu aplicativo lógico no modelo, adicione o objeto `identity` e a propriedade filho `type` à definição de recurso do aplicativo lógico no modelo, por exemplo:

```json
{
   "apiVersion": "2016-06-01",
   "type": "Microsoft.logic/workflows",
   "name": "[variables('logicappName')]",
   "location": "[resourceGroup().location]",
   "identity": {
      "type": "SystemAssigned"
   },
   "properties": {
      "definition": {
         "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
         "actions": {},
         "parameters": {},
         "triggers": {},
         "contentVersion": "1.0.0.0",
         "outputs": {}
   },
   "parameters": {},
   "dependsOn": []
}
```

Quando o Azure cria sua definição de recurso de aplicativo lógico, o objeto `identity` obtém estas propriedades adicionais:

```json
"identity": {
   "type": "SystemAssigned",
   "principalId": "<principal-ID>",
   "tenantId": "<Azure-AD-tenant-ID>"
}
```

| Propriedade (JSON) | Valor | Descrição |
|-----------------|-------|-------------|
| `principalId` | <*principal-ID*> | O GUID (identificador global exclusivo) do objeto de entidade de serviço para a identidade gerenciada que representa seu aplicativo lógico no locatário do Azure AD. Esse GUID, às vezes, é exibido como uma "ID de objeto" ou `objectID`. |
| `tenantId` | <*Azure-AD-tenant-ID*> | O GUID (identificador global exclusivo) que representa o locatário do Azure AD do qual o aplicativo lógico agora é membro. Dentro do locatário do Azure AD, a entidade de serviço tem o mesmo nome que a instância do aplicativo lógico. |
||||

<a name="user-assigned"></a>

### <a name="enable-user-assigned-identity"></a>Habilitar identidade atribuída ao usuário

Para configurar uma identidade gerenciada atribuída ao usuário para seu aplicativo lógico, primeiro você deve criar essa identidade como um recurso autônomo do Azure separado. Estas são as opções que você pode usar:

* [Azure portal](#azure-portal-user-identity)
* [Modelos do Gerenciador de Recursos do Azure](#template-user-identity)
* Azure PowerShell
  * [Criar identidade atribuída ao usuário](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md)
  * [Adicionar atribuição de função](../active-directory/managed-identities-azure-resources/howto-assign-access-powershell.md)
* CLI do Azure
  * [Criar identidade atribuída ao usuário](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)
  * [Adicionar atribuição de função](../active-directory/managed-identities-azure-resources/howto-assign-access-cli.md)
* API REST do Azure
  * [Criar identidade atribuída ao usuário](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-rest.md)
  * [Adicionar atribuição de função](../role-based-access-control/role-assignments-rest.md)

<a name="azure-portal-user-identity"></a>

#### <a name="create-user-assigned-identity-in-the-azure-portal"></a>Criar identidade atribuída ao usuário no portal do Azure

1. No [portal do Azure](https://portal.azure.com), na caixa de pesquisa em qualquer página, insira `managed identities` e selecione **Identidades gerenciadas**.

   ![Localizar e selecionar "Identidades Gerenciadas"](./media/create-managed-service-identity/find-select-managed-identities.png)

1. Em **identidades Gerenciadas**, selecione **Adicionar**.

   ![Adicionar nova identidade gerenciada](./media/create-managed-service-identity/add-user-assigned-identity.png)

1. Forneça informações sobre sua identidade gerenciada e, em seguida, selecione **Criar**, por exemplo:

   ![Criar identidade gerenciada atribuída ao usuário](./media/create-managed-service-identity/create-user-assigned-identity.png)

   | Propriedade | Obrigatório | Valor | Descrição |
   |----------|----------|-------|-------------|
   | **Nome do recurso** | Sim | <*user-assigned-identity-name*> | O nome a ser dado à sua identidade atribuída ao usuário. Este exemplo usa "Fabrikam-user-assigned-identity". |
   | **Assinatura** | Sim | <*Azure-subscription-name*> | O nome da assinatura do Azure a ser usado |
   | **Grupo de recursos** | Sim | <*Azure-resource-group-name*> | O nome do grupo de recursos a ser usado. Crie um grupo ou selecione um existente. Este exemplo cria um grupo chamado "fabrikam-managed-identities-RG". |
   | **Localidade** | Sim | <*Azure-region*> | A região do Azure na qual armazenar informações sobre seu recurso. Este exemplo usa "Oeste dos EUA". |
   |||||

   Agora você pode adicionar a identidade atribuída ao usuário ao seu aplicativo lógico. Você não pode adicionar mais de uma identidade atribuída ao usuário ao seu aplicativo lógico.

1. No portal do Azure, encontre e abra seu aplicativo lógico no Designer de Aplicativo Lógico.

1. No menu do aplicativo lógico, em **Configurações**, selecione **Identidade** e, em seguida, **Atribuída ao usuário** > **Adicionar**.

   ![Adicionar identidade gerenciada atribuída ao usuário](./media/create-managed-service-identity/add-user-assigned-identity-logic-app.png)

1. No painel **Adicionar identidade gerenciada atribuída ao usuário**, na lista **Assinatura**, selecione sua assinatura do Azure, se ainda não tiver selecionado. Na lista que mostra *todas* as identidades gerenciadas nessa assinatura, localize e selecione a identidade atribuída ao usuário desejada. Para filtrar a lista, na caixa de pesquisa **Identidades gerenciadas atribuídas ao usuário**, insira o nome da identidade ou do grupo de recursos. Quando terminar, selecione **Adicionar**.

   ![Selecione a identidade atribuída ao usuário a ser usada](./media/create-managed-service-identity/select-user-assigned-identity.png)

   > [!NOTE]
   > Se você receber um erro de que você pode ter apenas uma identidade gerenciada, seu aplicativo lógico já estará associado à identidade atribuída ao sistema. Para adicionar a identidade atribuída ao usuário, primeiro desabilite a identidade atribuída ao sistema em seu aplicativo lógico.

   Seu aplicativo lógico agora está associado à identidade gerenciada atribuída ao usuário.

   ![Associação com a identidade atribuída ao usuário](./media/create-managed-service-identity/added-user-assigned-identity.png)

1. Agora siga as [etapas que fornecem a essa identidade acesso ao recurso](#access-other-resources) mais adiante neste tópico.

<a name="template-user-identity"></a>

#### <a name="create-user-assigned-identity-in-an-azure-resource-manager-template"></a>Criar identidade atribuída ao usuário em um modelo do Azure Resource Manager

Para automatizar a criação e a implantação de recursos do Azure como aplicativos lógicos, você pode usar [modelos do Azure Resource Manager](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), que dão suporte a [identidades atribuídas ao usuário para autenticação](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md). Na seção `resources` do seu modelo, a definição de recurso do aplicativo lógico requer estes itens:

* Um objeto `identity` com a propriedade `type` definida como `UserAssigned`

* Um objeto filho `userAssignedIdentities` que especifica a ID de recurso da identidade, que é outro objeto filho que tem as propriedades `principalId` e `clientId`

Este exemplo mostra uma definição de recurso de aplicativo lógico para uma solicitação HTTP PUT e inclui um objeto `identity` não parametrizado. A resposta à solicitação PUT e a operação GET subsequente também têm este objeto `identity`:

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {<template-parameters>},
   "resources": [
      {
         "apiVersion": "2016-06-01",
         "type": "Microsoft.logic/workflows",
         "name": "[variables('logicappName')]",
         "location": "[resourceGroup().location]",
         "identity": {
            "type": "UserAssigned",
            "userAssignedIdentities": {
               "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<user-assigned-identity-name>": {
                  "principalId": "<principal-ID>",
                  "clientId": "<client-ID>"
               }
            }
         },
         "properties": {
            "definition": {<logic-app-workflow-definition>}
         },
         "parameters": {},
         "dependsOn": []
      },
   ],
   "outputs": {}
}
```

| Propriedade (JSON) | Valor | Descrição |
|-----------------|-------|-------------|
| `principalId` | <*principal-ID*> | O GUID (identificador global exclusivo) para a identidade gerenciada atribuída ao usuário no locatário do Azure AD |
| `clientId` | <*client-ID*> | Um GUID (identificador global exclusivo) para a nova identidade do seu aplicativo lógico que é usada para chamadas durante o runtime |
||||

Se o modelo também incluir a definição de recurso da identidade gerenciada, você poderá parametrizar o objeto `identity`. Este exemplo mostra como o objeto filho `userAssignedIdentities` faz referência a uma variável `userAssignedIdentity` que você define na seção `variables` do seu modelo. Essa variável referencia a ID de recurso para sua identidade atribuída ao usuário.

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {
      "Template_LogicAppName": {
         "type": "string"
      },
      "Template_UserAssignedIdentityName": {
         "type": "securestring"
      }
   },
   "variables": {
      "logicAppName": "[parameters(`Template_LogicAppName')]",
      "userAssignedIdentityName": "[parameters('Template_UserAssignedIdentityName')]"
   },
   "resources": [
      {
         "apiVersion": "2016-06-01",
         "type": "Microsoft.logic/workflows",
         "name": "[variables('logicAppName')]",
         "location": "[resourceGroup().location]",
         "identity": {
            "type": "UserAssigned",
            "userAssignedIdentities": {
               "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities/', variables('userAssignedIdentityName'))]": {}
            }
         },
         "properties": {
            "definition": {<logic-app-workflow-definition>}
         },
         "parameters": {},
         "dependsOn": [
            "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities/', variables('userAssignedIdentityName'))]"
         ]
      },
      {
         "apiVersion": "2018-11-30",
         "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
         "name": "[parameters('Template_UserAssignedIdentityName')]",
         "location": "[resourceGroup().location]",
         "properties": {
            "tenantId": "<tenant-ID>",
            "principalId": "<principal-ID>",
            "clientId": "<client-ID>"
         }
      }
  ]
}
```

| Propriedade (JSON) | Valor | Descrição |
|-----------------|-------|-------------|
| `tenantId` | <*Azure-AD-tenant-ID*> | O GUID (identificador global exclusivo) que representa o locatário do Azure AD do qual a identidade atribuída ao usuário agora é membro. Dentro do locatário do Azure AD, a entidade de serviço tem o mesmo nome que a identidade atribuída ao usuário. |
| `principalId` | <*principal-ID*> | O GUID (identificador global exclusivo) para a identidade gerenciada atribuída ao usuário no locatário do Azure AD |
| `clientId` | <*client-ID*> | Um GUID (identificador global exclusivo) para a nova identidade do seu aplicativo lógico que é usada para chamadas durante o runtime |
||||

<a name="access-other-resources"></a>

## <a name="give-identity-access-to-resources"></a>Conceder à identidade acesso aos recursos

Para usar a identidade gerenciada do aplicativo lógico para autenticação, configure o acesso para essa identidade no recurso do Azure em que você planeja usar a identidade. Para concluir essa tarefa, atribua a função apropriada a essa identidade no recurso do Azure de destino. Estas são as opções que você pode usar:

* [Azure portal](#azure-portal-assign-access)
* [Modelo do Azure Resource Manager](../role-based-access-control/role-assignments-template.md)
* O Azure PowerShell ([New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment)) – para obter mais informações, confira [Adicionar atribuição de função usando o Azure RBAC e o Azure PowerShell](../role-based-access-control/role-assignments-powershell.md).
* CLI do Azure ([az role assignment create](/cli/azure/role/assignment?view=azure-cli-latest#az-role-assignment-create)) – para obter mais informações, confira [Adicionar atribuição de função usando o Azure RBAC e a CLI do Azure](../role-based-access-control/role-assignments-cli.md).
* [API REST do Azure](../role-based-access-control/role-assignments-rest.md)

<a name="azure-portal-assign-access"></a>

### <a name="assign-access-in-the-azure-portal"></a>Atribuir acesso no portal do Azure

1. No [portal do Azure](https://portal.azure.com), acesse o recurso do Azure ao qual você deseja que sua identidade gerenciada tenha acesso.

1. No menu do recurso, selecione **Controle de acesso (IAM)**  > **Atribuições de função** em que você pode examinar as atribuições de função atuais para esse recurso. Na barra de ferramentas, selecione **Adicionar** > **Adicionar atribuição de função**.

   ![Selecione “Adicionar” > “Adicionar atribuição de função”](./media/create-managed-service-identity/add-role-to-resource.png)

   > [!TIP]
   > Se a opção **Adicionar atribuição de função** estiver desabilitada, provavelmente você não terá permissões. Para obter mais informações sobre as permissões com as quais você pode gerenciar funções para recursos, confira [Permissões da função Administrador no Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

1. Em **Adicionar atribuição de função**, selecione uma **Função** que forneça à sua identidade o acesso necessário ao recurso de destino.

   Para o exemplo deste tópico, sua identidade precisa de uma [função que possa acessar o blob em um contêiner do Armazenamento do Azure](../storage/common/storage-auth-aad.md#assign-rbac-roles-for-access-rights).

   ![Selecione a função "Colaborador de dados do Blob de Armazenamento"](./media/create-managed-service-identity/select-role-for-identity.png)

1. Siga estas etapas para sua identidade gerenciada:

   * **Identidade atribuída ao sistema**

     1. Na caixa **Atribuir acesso a**, selecione **Aplicativo Lógico**. Quando a propriedade **Assinatura** for exibida, selecione a assinatura do Azure associada à sua identidade.

        ![Selecione o acesso para a identidade atribuída ao sistema](./media/create-managed-service-identity/assign-access-system.png)

     1. Na caixa **Selecionar**, selecione seu aplicativo lógico na lista. Se a lista for muito longa, use a caixa **Selecionar** para filtrar a lista.

        ![Selecione o aplicativo lógico para identidade atribuída ao sistema](./media/create-managed-service-identity/add-permissions-select-logic-app.png)

   * **Identidade atribuída ao usuário**

     1. Na caixa **Atribuir acesso a**, selecione **Identidade gerenciada atribuída ao usuário**. Quando a propriedade **Assinatura** for exibida, selecione a assinatura do Azure associada à sua identidade.

        ![Selecione o acesso para a identidade atribuída ao usuário](./media/create-managed-service-identity/assign-access-user.png)

     1. Na caixa **Selecionar**, selecione sua identidade na lista. Se a lista for muito longa, use a caixa **Selecionar** para filtrar a lista.

        ![Selecione sua identidade atribuída ao usuário](./media/create-managed-service-identity/add-permissions-select-user-assigned-identity.png)

1. Quando terminar, selecione **Salvar**.

   A lista de atribuições de função do recurso de destino agora mostra a função e a identidade gerenciada selecionadas. Este exemplo mostra como você pode usar a identidade atribuída ao sistema para um aplicativo lógico e uma identidade atribuída ao usuário para um grupo de outros aplicativos lógicos.

   ![Adicionar identidades gerenciadas e funções ao recurso de destino](./media/create-managed-service-identity/added-roles-for-identities.png)

   Para obter mais informações, confira [Atribuir um acesso de identidade gerenciada a um recurso usando o portal do Azure](../active-directory/managed-identities-azure-resources/howto-assign-access-portal.md).

1. Agora siga as [etapas para autenticar o acesso com a identidade](#authenticate-access-with-identity) em um gatilho ou uma ação que dá suporte a identidades gerenciadas.

<a name="authenticate-access-with-identity"></a>

## <a name="authenticate-access-with-managed-identity"></a>Autenticar o acesso com a identidade gerenciada

Depois de [habilitar a identidade gerenciada para seu aplicativo lógico](#azure-portal-system-logic-app) e [conceder a essa identidade acesso ao recurso ou à entidade de destino](#access-other-resources), você poderá usar essa identidade em [gatilhos e ações que dão suporte a identidades gerenciadas](logic-apps-securing-a-logic-app.md#managed-identity-authentication).

> [!IMPORTANT]
> Se você tiver uma função do Azure em que deseja usar a identidade atribuída pelo sistema, primeiro [habilite a autenticação para Azure Functions](../logic-apps/logic-apps-azure-functions.md#enable-authentication-for-azure-functions).

Estas etapas mostram como usar a identidade gerenciada com um gatilho ou uma ação por meio do portal do Azure. Para especificar a identidade gerenciada em uma definição de JSON subjacente de um gatilho ou uma ação, confira [Autenticação de identidade gerenciada](../logic-apps/logic-apps-securing-a-logic-app.md#managed-identity-authentication).

1. No [portal do Azure](https://portal.azure.com), abra o aplicativo lógico no Designer do aplicativo lógico.

1. Se você ainda não tiver feito isso, adicione [o gatilho ou a ação que dá suporte a identidades gerenciadas](logic-apps-securing-a-logic-app.md#managed-identity-authentication).

   Por exemplo, o gatilho ou a ação HTTP pode usar a identidade atribuída ao sistema que você habilitou para seu aplicativo lógico. Em geral, o gatilho ou a ação HTTP usa essas propriedades para especificar o recurso ou a entidade que você deseja acessar:

   | Propriedade | Obrigatório | Descrição |
   |----------|----------|-------------|
   | **Método** | Sim | O método HTTP usado pela operação que você deseja executar |
   | **URI** | Sim | A URL do ponto de extremidade para acessar a entidade ou o recurso do Azure de destino. A sintaxe do URI geralmente inclui a [ID do recurso](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication) para o recurso ou serviço do Azure. |
   | **Cabeçalhos** | Não | Todos os valores de cabeçalho que você precisa ou deseja incluir na solicitação de saída, como o tipo de conteúdo |
   | **Consultas** | Não | Todos os parâmetros de consulta que você precisa ou deseja incluir na solicitação, como o parâmetro para uma operação específica ou a versão da API para a operação que você deseja executar |
   | **Autenticação** | Sim | O tipo de autenticação a ser usado para autenticar o acesso ao recurso ou à entidade de destino |
   ||||

   Como um exemplo específico, suponha que você queira executar a [operação Blob de Instantâneo](/rest/api/storageservices/snapshot-blob) em um blob na conta de Armazenamento do Azure em que você configurou o acesso para sua identidade anteriormente. No entanto, o [conector do Armazenamento de Blobs do Azure](/connectors/azureblob/) não oferece essa operação no momento. Em vez disso, você pode executar essa operação usando a [ação HTTP](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action) ou [outra operação da API REST de Serviço Blob](/rest/api/storageservices/operations-on-blobs).

   > [!IMPORTANT]
   > Para acessar as contas de armazenamento do Azure por trás de firewalls usando solicitações HTTP e identidades gerenciadas, verifique se você também configurou sua conta de armazenamento com a [exceção que permite o acesso por serviços confiáveis da Microsoft](../connectors/connectors-create-api-azureblobstorage.md#access-trusted-service).

   Para executar a [operação Blob de Instantâneo](/rest/api/storageservices/snapshot-blob), a ação HTTP especifica estas propriedades:

   | Propriedade | Obrigatório | Valor de exemplo | Descrição |
   |----------|----------|---------------|-------------|
   | **Método** | Sim | `PUT`| O método HTTP usado pela operação Blob de Instantâneo |
   | **URI** | Sim | `https://{storage-account-name}.blob.core.windows.net/{blob-container-name}/{folder-name-if-any}/{blob-file-name-with-extension}` | A ID de recurso para um arquivo do Armazenamento de Blobs do Azure no ambiente do Azure Global (público), que usa essa sintaxe |
   | **Cabeçalhos** | Sim, para o Armazenamento do Azure | `x-ms-blob-type` = `BlockBlob` <p>`x-ms-version` = `2019-02-02` | Os valores de cabeçalho `x-ms-blob-type` e `x-ms-version` necessários para operações do Armazenamento do Azure. <p><p>**Importante**: no gatilho HTTP de saída e nas solicitações de ação para o Armazenamento do Azure, o cabeçalho requer a propriedade `x-ms-version` e a versão da API para a operação que você deseja executar. <p>Para saber mais, consulte esses tópicos: <p><p>- [Cabeçalhos de solicitação – Blob de Instantâneo](/rest/api/storageservices/snapshot-blob#request) <br>- [Controle de versão para serviços de Armazenamento do Azure](/rest/api/storageservices/versioning-for-the-azure-storage-services#specifying-service-versions-in-requests) |
   | **Consultas** | Sim, para esta operação | `comp` = `snapshot` | O nome e o valor do parâmetro de consulta para a operação Blob de Instantâneo. |
   |||||

   Este é o a ação HTTP de exemplo que mostra todos esses valores de propriedade:

   ![Adicionar uma ação HTTP para acessar um recurso do Azure](./media/create-managed-service-identity/http-action-example.png)

1. Agora adicione a propriedade **Autenticação** à ação HTTP. Na lista **Adicionar novo parâmetro**, selecione **Autenticação**.

   ![Adicione a propriedade “Autenticação” à ação HTTP](./media/create-managed-service-identity/add-authentication-property.png)

   > [!NOTE]
   > Nem todos os gatilhos e ações dão suporte para permitir que você adicione um tipo de autenticação. Para obter mais informações, confira [Adicionar autenticação a chamadas de saída](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound).

1. Na lista **Tipo de autenticação**, selecione **Identidade Gerenciada**.

   ![Para “Autenticação”, selecione “Identidade Gerenciada”](./media/create-managed-service-identity/select-managed-identity.png)

1. Na lista de identidades gerenciadas, selecione entre as opções disponíveis com base em seu cenário.

   * Se você configurou a identidade atribuída ao sistema, selecione **Identidade Gerenciada Atribuída ao Sistema**, se ainda não tiver selecionado.

     ![Selecione “Identidade Gerenciada Atribuída ao Sistema”](./media/create-managed-service-identity/select-system-assigned-identity-for-action.png)

   * Se você configurou uma identidade atribuída ao usuário, selecione essa identidade, se ainda não tiver selecionado.

     ![Selecionar a identidade atribuída ao usuário](./media/create-managed-service-identity/select-user-assigned-identity-for-action.png)

   Este exemplo continua com a **Identidade Gerenciada Atribuída ao Sistema**.

1. Em alguns gatilhos e ações, a propriedade **Público-alvo** também é exibida para que você defina a ID do recurso de destino. Defina a propriedade **Público-alvo** como a [ID de recurso para o recurso ou serviço de destino](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication). Caso contrário, por padrão, a propriedade **Público-alvo** usa a ID de recurso `https://management.azure.com/`, que é a ID de recurso para o Azure Resource Manager.

   > [!IMPORTANT]
   > Verifique se essa ID de recurso de destino *corresponde exatamente* ao valor que o Azure AD (Active Directory) espera, incluindo barras à direita necessárias. Por exemplo, a ID de recurso para todas as contas de Armazenamento de Blobs do Azure requer uma barra à direita. No entanto, a ID de recurso para uma conta de armazenamento específica não requer uma barra à direita. Confira as [IDs de recurso para os serviços do Azure que dão suporte ao Azure AD](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication).

   Este exemplo define a propriedade **Público-alvo** como `https://storage.azure.com/` para que os tokens de acesso usados para autenticação sejam válidos para todas as contas de armazenamento. No entanto, você também pode especificar uma URL de serviço raiz, `https://fabrikamstorageaccount.blob.core.windows.net`, para uma conta de armazenamento específica.

   ![Definir a propriedade “Público-alvo” como a ID de recurso de destino](./media/create-managed-service-identity/specify-audience-url-target-resource.png)

   Para obter mais informações sobre como autorizar o acesso com o Azure AD para o Armazenamento do Azure, confira estes tópicos:

   * [Autorizar o acesso a blobs e filas do Azure usando o Azure Active Directory](../storage/common/storage-auth-aad.md)
   * [Autorizar o acesso ao Armazenamento do Azure com o Azure Active Directory](/rest/api/storageservices/authorize-with-azure-active-directory#use-oauth-access-tokens-for-authentication)

1. Continue criando o aplicativo lógico da maneira desejada.

<a name="remove-identity"></a>

## <a name="disable-managed-identity"></a>Desabilitar identidade gerenciada

Para parar de usar uma identidade gerenciada para seu aplicativo lógico, você tem estas opções:

* [Azure portal](#azure-portal-disable)
* [Modelos do Gerenciador de Recursos do Azure](#template-disable)
* Azure PowerShell
  * [Remover atribuição de função](../role-based-access-control/role-assignments-powershell.md)
  * [Excluir identidade atribuída ao usuário](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md)
* CLI do Azure
  * [Remover atribuição de função](../role-based-access-control/role-assignments-cli.md)
  * [Excluir identidade atribuída ao usuário](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)
* API REST do Azure
  * [Remover atribuição de função](../role-based-access-control/role-assignments-rest.md)
  * [Excluir identidade atribuída ao usuário](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-rest.md)

Se você exclui seu aplicativo lógico, o Azure remove automaticamente a identidade gerenciada do Azure AD.

<a name="azure-portal-disable"></a>

### <a name="disable-managed-identity-in-the-azure-portal"></a>Desabilitar a identidade gerenciada no portal do Azure

No portal do Azure, primeiro remova o acesso da identidade ao [seu recurso de destino](#disable-identity-target-resource). Em seguida, desligue a identidade atribuída ao sistema ou remova a identidade atribuída ao usuário do [seu aplicativo lógico](#disable-identity-logic-app).

<a name="disable-identity-target-resource"></a>

#### <a name="remove-identity-access-from-resources"></a>Remover o acesso à identidade dos recursos

1. No [portal do Azure](https://portal.azure.com), acesse o recurso do Azure de destino em que você deseja remover o acesso para a identidade gerenciada.

1. No menu do recurso de destino, selecione **Controle de acesso (IAM)** . Na barra de ferramentas, selecione **Atribuições de função**.

1. Na lista de funções, selecione as identidades gerenciadas que você deseja remover. Na barra de ferramentas, selecione **Remover**.

   > [!TIP]
   > Se a opção **Remover** estiver desabilitada, provavelmente você não terá permissões. Para obter mais informações sobre as permissões com as quais você pode gerenciar funções para recursos, confira [Permissões da função Administrador no Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

A identidade gerenciada agora é removida e não tem mais acesso ao recurso de destino.

<a name="disable-identity-logic-app"></a>

#### <a name="disable-managed-identity-on-logic-app"></a>Desabilitar a identidade gerenciada no aplicativo lógico

1. No [Portal do Azure](https://portal.azure.com), abra o aplicativo lógico no Designer do Aplicativo Lógico.

1. No menu do aplicativo lógico, em **Configurações**, selecione **Identidade** e, em seguida, siga as etapas para sua identidade:

   * Selecione **Atribuída ao sistema** > **Ativada** > **Salvar**. Quando o Azure solicitar que você confirme, selecione **Sim**.

     ![Desabilitar a identidade atribuída ao sistema](./media/create-managed-service-identity/disable-system-assigned-identity.png)

   * Selecione **Atribuído ao usuário** e a identidade gerenciada e, em seguida, selecione **Remover**. Quando o Azure solicitar que você confirme, selecione **Sim**.

     ![Remover a identidade atribuída ao usuário](./media/create-managed-service-identity/remove-user-assigned-identity.png)

A identidade gerenciada agora está desabilitada em seu aplicativo lógico.

<a name="template-disable"></a>

### <a name="disable-managed-identity-in-azure-resource-manager-template"></a>Desabilitar a identidade gerenciada no modelo do Azure Resource Manager

Se você tiver criado a identidade gerenciada do aplicativo lógico usando um modelo do Azure Resource Manager, defina a propriedade filho `type` do objeto `identity` como `None`. Para a identidade gerenciada pelo sistema, essa ação também exclui a ID da entidade de segurança do Azure AD.

```json
"identity": {
   "type": "None"
}
```

## <a name="next-steps"></a>Próximas etapas

* [Proteger o acesso e os dados nos Aplicativos Lógicos do Azure](../logic-apps/logic-apps-securing-a-logic-app.md)
