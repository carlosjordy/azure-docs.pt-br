---
title: Criar um balanceador de carga interno - Modelo do Azure
titleSuffix: Azure Load Balancer
description: Saiba como criar um balanceador de carga interno no Gerenciador de Recursos usando um modelo
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: how-to
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: allensu
ms.openlocfilehash: 785054222dc3281b11b5e6abe1b82f67e2fc5ace
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87066655"
---
# <a name="create-an-internal-load-balancer-using-a-template"></a>Criar um balanceador de carga interno usando um modelo

> [!div class="op_single_selector"]
> * [Portal do Azure](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [CLI do Azure](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Modelo](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Implantar o modelo usando o clique para implantar

O modelo de exemplo disponível no repositório público usa um arquivo de parâmetro que contém os valores padrão usados para gerar o cenário descrito acima. Para implantar esse modelo usando a opção de clique para implantar, acesse [este link](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), clique em **Implantar no Azure**, substitua os valores de parâmetro padrão, se necessário, e siga as instruções no portal.

## <a name="deploy-the-template-by-using-powershell"></a>Implantar o modelo usando o PowerShell

Para implantar o modelo baixado usando o PowerShell, faça o seguinte.

1. Se você nunca usou o Azure PowerShell, consulte [Como instalar e configurar o Azure PowerShell](/powershell/azure/) e siga as instruções até o fim para entrar no Azure e selecionar sua assinatura.
2. Baixe o arquivo de parâmetros no disco local.
3. Edite o arquivo e salve-o.
4. Execute o cmdlet **New-AzResourceGroupDeployment** para criar um grupo de recursos usando o modelo.

    ```azurepowershell-interactive
    New-AzResourceGroupDeployment -Name TestRG -Location westus `
        -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
        -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'
    ```

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Implantar o modelo usando a CLI do Microsoft Azure

Para implantar o modelo usando a CLI do Microsoft Azure, faça o seguinte:

1. Se você nunca usou CLI do Azure, consulte [instalar e configurar o CLI do Azure](../cli-install-nodejs.md) e siga as instruções até o ponto em que você seleciona sua conta e assinatura do Azure.
2. Acesse [https://shell.azure.com](https://shell.azure.com) para abrir o Cloud Shell no navegador. Execute o comando **azure config mode** para alternar para o modo do Gerenciador de Recursos, como mostrado abaixo.

    ```console
    azure config mode arm
    ```

    Este é o resultado esperado para o comando descrito acima:

    ```output
    info:    New mode is arm
    ```

3. Abra o arquivo de parâmetro, selecione o seu conteúdo e salve-o em um arquivo em seu computador. Para este exemplo, salvamos o arquivo de parâmetros em *parameters.json*.
4. Execute o comando **azure group deployment create** para implantar o novo balanceador de carga interno usando o modelo e os arquivos de parâmetro baixados e modificados acima. A lista exibida após a saída explicar os parâmetros usados.

    ```console
    azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json
    ```

## <a name="next-steps"></a>Próximas etapas

[Configurar um modo de distribuição do balanceador de carga usando a afinidade de IP de origem](load-balancer-distribution-mode.md)

[Definir configurações de tempo limite de TCP ocioso para o balanceador de carga](load-balancer-tcp-idle-timeout.md)

Saiba mais sobre a sintaxe e as propriedades JSON de um balanceador de carga em um modelo em [Microsoft.Network/loadBalancers](/azure/templates/microsoft.network/loadbalancers).