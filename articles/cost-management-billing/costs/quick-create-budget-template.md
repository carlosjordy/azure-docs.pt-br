---
title: Início Rápido – Criar um orçamento com um modelo do Azure Resource Manager
description: Início Rápido mostrando como criar um orçamento com um modelo do Azure Resource Manager.
author: bandersmsft
ms.author: banders
tags: azure-resource-manager
ms.service: cost-management-billing
ms.topic: quickstart
ms.date: 06/10/2020
ms.custom: subject-armqs
ms.openlocfilehash: 5bff8e6057475701a2e78835fb5a950dcb8c8fcb
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86252429"
---
# <a name="quickstart-create-a-budget-with-an-arm-template"></a>Início Rápido: Criar um orçamento com um modelo do ARM

Orçamentos no Gerenciamento de Custos ajudam você a planejar e promover responsabilidade organizacional. Com orçamentos, você pode considerar os serviços do Azure que consome ou assina durante um período específico. Eles ajudam você a informar outras pessoas sobre seus gastos para gerenciar proativamente os custos e monitorar como os gastos evoluem ao longo do tempo. Quando os limites de orçamento que você criou são excedidos, são disparadas notificações. Nenhum de seus recursos é afetado e seu consumo não é interrompido. Você pode usar os orçamentos para comparar e controlar como analisar os custos de gastos. Este início rápido mostra como criar um orçamento usando um modelo do ARM (Azure Resource Manager).

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Se seu ambiente atender aos pré-requisitos e você estiver familiarizado com o uso de modelos ARM, selecione o botão **Implantar no Azure**. O modelo será aberto no portal do Azure.

[![Implantar no Azure](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fcreate-budget%2Fazuredeploy.json)

## <a name="prerequisites"></a>Pré-requisitos

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

O modelo do ARM dá suporte apenas a assinaturas do Azure para EA (Contratos Enterprise). Outros tipos de assinatura não são compatíveis com o modelo.

Para criar e gerenciar orçamentos, você deve ter permissão de colaborador. Você pode criar orçamentos individuais para grupos de recursos e assinaturas de EA. No entanto, não é possível criar os orçamentos para contas de cobrança de EA. Para assinatura do Azure EA, você deve ter acesso de leitura para exibir orçamentos.

Após a criação de um orçamento, você precisa pelo menos de acesso de leitura à sua conta do Azure para ver orçamentos.

Se você tiver uma nova assinatura, não poderá criar um orçamento imediatamente nem usar outros recursos do Gerenciamento de Custos. Poderá levar até 48 horas para você poder usar todos os recursos do Gerenciamento de Custos.

Há suporte para as permissões ou escopos a seguir do Azure por assinatura para orçamentos por usuário e grupo. Para obter mais informações sobre escopos, consulte [Entender e trabalhar com escopos](understand-work-scopes.md).

- Proprietário – pode criar, modificar ou excluir os orçamentos para uma assinatura.
- Colaborador e contribuidor do Gerenciamento de Custos - pode criar, modificar ou excluir seus próprios orçamentos. Pode modificar o valor do orçamento para orçamentos criados por outras pessoas.
- Leitor e leitor do Gerenciamento de Custos – pode exibir os orçamentos para os quais ele têm permissão.

Para obter mais informações sobre como atribuir permissões aos dados de Gerenciamento de Custos, consulte [Atribuir acesso aos dados de Gerenciamento de Custos](assign-access-acm-data.md).

## <a name="review-the-template"></a>Examinar o modelo

O modelo usado neste início rápido é proveniente dos [Modelos de Início Rápido do Azure](https://azure.microsoft.com/resources/templates/create-budget).

:::code language="json" source="~/quickstart-templates/create-budget/azuredeploy.json" range="1-146" highlight="110-139":::

Um recurso do Azure é definido no modelo:

* [Microsoft.Consumption/budgets](/azure/templates/microsoft.consumption/budgets): criar um orçamento do Azure.

## <a name="deploy-the-template"></a>Implantar o modelo

1. Selecione a imagem a seguir para entrar no Azure e abrir um modelo. O modelo cria um orçamento.

   [![Implantar no Azure](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fcreate-budget%2Fazuredeploy.json)

2. Selecione ou insira os seguintes valores.

   [![Modelo do Resource Manager, Criar orçamento, portal de implantação](./media/quick-create-budget-template/create-budget-using-template-portal.png)](./media/quick-create-budget-template/create-budget-using-template-portal.png#lightbox)

    * **Assinatura**: selecione uma assinatura do Azure.
    * **Grupo de recursos**: selecione **Criar** e insira um nome exclusivo para o grupo de recursos e clique em **OK** ou selecione um grupo de recursos existente.
    * **Local**: selecione um local. Por exemplo, **Centro dos EUA**.
    * **Nome do Orçamento**: insira um nome para o orçamento. Ele deve ser exclusivo em um grupo de recursos. Somente caracteres alfanuméricos, sublinhados e hifens são permitidos.
    * **Quantidade**: insira a quantidade total de custo ou uso a ser controlada com o orçamento.
    * **Categoria do Orçamento**: selecione a categoria do orçamento, se ele controla o **Custo** ou o **Uso**.
    * **Intervalo de agregação**: insira o tempo coberto por um orçamento. Os valores permitidos são mensais, trimestrais ou anuais. O orçamento é redefinido ao final do intervalo de agregação.
    * **Data de Início**: insira a data de início com o primeiro dia do mês no formato AAAA-MM-DD. Uma data de início futura não deve ser a mais de três meses do dia de hoje. Você pode especificar uma data de início anterior com o período de Intervalo de Agregação.
    * **Data de Término**: insira a data de término do orçamento no formato AAAA-MM-DD. Se não for fornecida, o padrão será definido como 10 anos a partir da data de início.
    * **Operador**: selecione um operador de comparação. Os valores possíveis são EqualTo, GreaterThan ou GreaterThanOrEqualTo.
    * **Limite**: insira um valor de limite para a notificação. Uma notificação é enviada quando o custo excede o limite. É sempre percentual e deve estar entre 0 e 1000.
    * **Emails de Contato**: insira uma lista de endereços de email para os quais enviar a notificação de orçamento quando o limite for excedido. O formato esperado é `["user1@domain.com","user2@domain.com"]`.
    * **Funções de Contato**: insira a lista de funções de contato para as quais enviar a notificação de orçamento quando o limite for excedido. Os valores padrão são Proprietário, Colaborador e Leitor. O formato esperado é `["Owner","Contributor","Reader"]`.
    * **Grupos de Contato**: insira uma lista de IDs de recurso do grupo de ações, como URIs de recurso completo, para os quais enviar a notificação de orçamento quando o limite for excedido. Ela aceita uma matriz de cadeias de caracteres. O formato esperado é `["action group resource ID1","action group resource ID2"]`. Se não desejar usar grupos de ação, insira `[]`.
    * **Filtro de Recursos**: insira uma lista de filtros para recursos. O formato esperado é `["Resource Filter Name1","Resource Filter Name2"]`. Se não desejar aplicar um filtro, insira `[]`. Se você inserir um filtro de recurso, também deverá inserir valores de **filtros de medidores**.
    * **Filtro de Medidores**: insira uma lista de filtros em medidores, obrigatórios para orçamentos com a categoria de orçamento **Uso**. O formato esperado é `["Meter Filter Name1","Meter Filter Name2"]`. Se você não inseriu um **filtro de recursos**, insira `[]`.
    * **Concordo com os termos e condições declarados acima**: selecione.

3. Selecione **Comprar**. Depois que o orçamento for implantado com êxito, você receberá uma notificação:

   ![Modelo do Resource Manager, orçamento, notificação de implantação do portal](./media/quick-create-budget-template/resource-manager-template-portal-deployment-notification.png)

O portal do Azure é usado para implantar o modelo. Além do portal do Azure, você também pode usar o Azure PowerShell, a CLI do Azure e a API REST. Para saber mais sobre outros métodos de implantação, confira [Implantar modelos](../../azure-resource-manager/templates/deploy-powershell.md).

## <a name="validate-the-deployment"></a>Validar a implantação

Você pode usar o portal do Azure para verificar se o orçamento foi criado navegando até **Cobrança + Gerenciamento de Custos** > selecione um escopo > **Orçamentos**. Ou use os scripts da CLI do Azure ou do Azure PowerShell a seguir para ver o orçamento.

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
az consumption budget list
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
Get-AzConsumptionBudget
```

---

## <a name="clean-up-resources"></a>Limpar os recursos

Quando você não precisar mais de um orçamento, exclua-o usando um dos seguintes métodos:

### <a name="azure-portal"></a>Portal do Azure

Navegue até **Gerenciamento de Custos + Cobrança** > selecione um escopo do orçamento > **Orçamentos** > selecione um orçamento >, em seguida, selecione **Excluir orçamento**.

### <a name="command-line"></a>Linha de comando

Você pode remover o orçamento usando a CLI do Azure ou o Azure PowerShell.

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
echo "Enter the budget name:" &&
read budgetName &&
az consumption budget delete --budget-name $budgetName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$budgetName = Read-Host -Prompt "Enter the budget name"
Remove-AzConsumptionBudget -Name $budgetName
Write-Host "Press [ENTER] to continue..."
```

---

## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você criou um orçamento do Azure para a implantação. Para saber mais sobre a Cobrança e o Gerenciamento de Custos do Azure e o Azure Resource Manager, continue lendo os artigos abaixo.

- Leia a visão geral de [Cobrança e Gerenciamento de Custos](../cost-management-billing-overview.md)
- [Crie orçamentos](tutorial-acm-create-budgets.md) no portal do Azure
- Saiba mais sobre o [Azure Resource Manager](../../azure-resource-manager/management/overview.md)
