---
title: Criar um conjunto de dimensionamento que usa VMs do Azure Spot
description: Saiba como criar conjuntos de dimensionamento de máquinas virtuais do Azure que usam VMs do Spot para economizar nos custos.
author: cynthn
ms.author: cynthn
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: spot
ms.date: 03/25/2020
ms.reviewer: jagaveer
ms.custom: jagaveer
ms.openlocfilehash: 70d7eb000ed2d50bc22bb005621ee7515e5a2a61
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86527448"
---
# <a name="azure-spot-vms-for-virtual-machine-scale-sets"></a>VMs do Azure Spot para conjuntos de dimensionamento de máquinas virtuais 

O uso do Azure Spot em conjuntos de dimensionamento permite tirar proveito da capacidade não usada com uma economia de custos significativa. A qualquer momento que o Azure precisar da capacidade de volta, a infraestrutura do Azure removerá as instâncias do Spot. Portanto, as instâncias do Spot são ótimas para cargas de trabalho que podem lidar com interrupções, como trabalhos de processamento em lotes, ambientes de desenvolvimento/teste, grandes cargas de trabalho de computação e etc.

A quantidade de capacidade disponível pode variar com base no tamanho, região, hora do dia e etc. Durante a implantação de instâncias do Spot em conjuntos de dimensionamento, o Azure alocará a instância apenas se houver capacidade disponível, mas não há SLA para essas instâncias. Um conjunto de dimensionamento do Spot é implantado em um domínio de falha único e não oferece garantias de alta disponibilidade.


## <a name="pricing"></a>Preços

O preço para instâncias do Spot varia com base na região e SKU. Para obter mais informações, consulte os preços para [Linux](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) e [Windows](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/windows/). 


Como o preço é variável, você tem a opção de definir um preço máximo, em dólares americanos (USD), usando até 5 casas decimais. Por exemplo, o valor `0.98765`seria um preço máximo de $0,98765 USD por hora. Se você definir o preço máximo como `-1`, a instância não será removida com base no preço. O preço da instância será o preço atual para o Spot ou o preço de uma instância padrão, o que for menor, desde que haja capacidade e cota disponíveis.

## <a name="eviction-policy"></a>Política de remoção

Durante a criação de conjuntos de dimensionamento do Spot, é possível definir a política de remoção para *Desalocar* (padrão) ou *Excluir*. 

A política *Desalocar* move suas instâncias removidas para o estado de parado desalocado permitindo que você reimplante instâncias removidas. No entanto, não há nenhuma garantia de que a alocação terá êxito. As VMs deslocadas afetarão sua cota de instância do conjunto de dimensionamento e você será cobrado pelos discos subjacentes. 

Se você quiser que suas instâncias em seu conjunto de dimensionamento do Spot sejam excluídas quando forem removidas, você pode definir a política de remoção para *excluir*. Com a política de remoção definida para excluir, você pode criar novas VMs, aumentando a propriedade de contagem de instância do conjunto de dimensionamento. As VMs removidas são excluídas junto com seus discos subjacentes e, portanto, você não será cobrado pelo armazenamento. Você também pode usar o recurso de dimensionamento automático dos conjuntos de dimensionamento para tentar e compensar automaticamente as VMs removidas, mas não há nenhuma garantia de que a alocação terá êxito. É recomendável que você só use o recurso de dimensionamento automático em conjuntos de dimensionamento do Spot quando você definir a política de remoção para excluir, evitando, assim, que o custo de seus discos atinjam os limites de cota. 

Os usuários podem optar por receber notificações na VM por meio dos [Eventos Agendados do Azure](../virtual-machines/linux/scheduled-events.md). Isso notificará você se suas VMs estiverem sendo removidas e você terá 30 segundos para concluir todos os trabalhos e realizar tarefas de desligamento antes da remoção. 


## <a name="deploying-spot-vms-in-scale-sets"></a>Implantar VMs do Spot em conjuntos de dimensionamento

Para implantar VMs do Spot em conjuntos de dimensionamento, você poderá definir o novo sinalizador de *Prioridade* como *Spot*. Todas as VMs no conjunto de dimensionamento serão definidas como Spot. Para criar um conjunto de dimensionamento com VMs do Spot, use um dos seguintes métodos:
- [Azure portal](#portal)
- [CLI do Azure](#azure-cli)
- [PowerShell do Azure](#powershell)
- [Modelos do Gerenciador de Recursos do Azure](#resource-manager-templates)

## <a name="portal"></a>Portal

O processo para criar um conjunto de dimensionamento que usa VMs do Spot é o mesmo detalhado no [artigo de introdução](quick-create-portal.md). Quando estiver implantando um conjunto de dimensionamento, você pode optar por definir o sinalizador de Spot e a política de remoção: ![Criar um conjunto de dimensionamento com VMs do Spot](media/virtual-machine-scale-sets-use-spot/vmss-spot-portal-max-price.png)


## <a name="azure-cli"></a>CLI do Azure

O processo para criar um conjunto de dimensionamento com VMs do Spot é o mesmo detalhado no [artigo de introdução](quick-create-cli.md). Basta adicionar '--Priority Spot', e adicionar `--max-price`. Neste exemplo, usamos `-1` para `--max-price` para que a instância não seja removida com base no preço.

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --priority Spot \
    --max-price -1 
```

## <a name="powershell"></a>PowerShell

O processo para criar um conjunto de dimensionamento com VMs do Spot é o mesmo detalhado no [artigo de introdução](quick-create-powershell.md).
Basta adicionar '-Priority Spot', e fornecer um `-max-price` para [New-AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig).

```powershell
$vmssConfig = New-AzVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Priority "Spot" `
    --max-price -1
```

## <a name="resource-manager-templates"></a>Modelos do Gerenciador de Recursos

O processo para criar um conjunto de dimensionamento que use VMs do Spot é o mesmo detalhado no artigo de introdução para [Linux](quick-create-template-linux.md) ou [Windows](quick-create-template-windows.md). 

Para implantações de modelo do Spot, use`"apiVersion": "2019-03-01"` ou posterior. Adicione as propriedades `priority`, `evictionPolicy` e `billingProfile` à seção `"virtualMachineProfile":` em seu modelo: 

```json
                "priority": "Spot",
                "evictionPolicy": "Deallocate",
                "billingProfile": {
                    "maxPrice": -1
                }
```

Para excluir a instância depois que ela tiver sido removida, altere o parâmetro `evictionPolicy` para `Delete`.

## <a name="faq"></a>Perguntas frequentes

**P:** Uma vez criada, uma instância do Spot é igual à instância padrão?

**R:** Sim, exceto que não há SLA para VMs do Spot e elas podem ser removidas a qualquer momento.


**P:** O que fazer quando você é removido, mas ainda precisa de capacidade?

**R:** Recomendamos que você use VMs padrão em vez de VMs do Spot se precisar de capacidade imediatamente.


**P:** Como a cota é gerenciada para o Spot?

**R:** As instâncias do Spot e as instâncias padrão terão pools de cotas separados. A cota do Spot será compartilhada entre as VMs e as instâncias do conjunto de dimensionamento. Para saber mais, confira [Assinatura e limites de serviço, cotas e restrições do Azure](../azure-resource-manager/management/azure-subscription-service-limits.md).


**P:** Posso solicitar uma cota adicional para o Spot?

**R:** Sim, você poderá enviar a solicitação para aumentar sua cota de VMs do Spot por meio do [processo de solicitação de cota padrão](../azure-portal/supportability/per-vm-quota-requests.md).


**P:** É possível converter os conjuntos de dimensionamento existentes para conjuntos de dimensionamento do Spot?

**R:** Não, a definição do sinalizador `Spot` só tem suporte no momento da criação.


**P:** Se eu estivesse usando `low` para conjuntos de dimensionamento de baixa prioridade, preciso começar a usar `Spot` em vez disso?

**R:** Por enquanto, `low` e `Spot` funcionarão, mas você deve começar a fazer a transição para usar `Spot`.


**P:** É possível criar um conjunto de dimensionamento com VMs regulares e VMs do Spot?

**R:** Não, um conjunto de escala não oferece suporte a mais de um tipo de prioridade.


**P:**  É possível usar o dimensionamento automático com conjuntos de dimensionamento do Spot?

**R:** Sim, é possível definir regras de dimensionamento automático em seu conjunto de dimensionamento do Spot. Se suas VMs forem removidas, o dimensionamento automático pode tentar criar novas VMs do Spot. Lembre-se de que essa capacidade não é garantida. 


**P:**  O dimensionamento automático funciona com as políticas de remoção (deslocar e excluir)?

**R:** Recomenda-se definir sua política de remoção para excluir ao usar o dimensionamento automático. Isso ocorre porque instâncias desalocadas são contadas em relação a sua capacidade de contagem no conjunto de dimensionamento. Ao usar o dimensionamento automático, você provavelmente atingirá sua contagem de instâncias de destino rapidamente devido a instâncias desalocadas, removidas. 


**P:** Quais canais dão suporte a VMs do Spot?

**R:** Consulte a tabela abaixo para encontrar a disponibilidade da VM do Spot.

<a name="channel"></a>

| Canais do Azure               | Disponibilidade de VMs do Azure Spot       |
|------------------------------|-----------------------------------|
| Contrato Enterprise         | Sim                               |
| Pré-pago                | Sim                               |
| Provedor de Serviços de Nuvem (CSP) | [Entre em contato com seu parceiro](/partner-center/azure-plan-get-started) |
| Benefícios                     | Não disponível                     |
| Patrocinado                    | Sim                               |
| Avaliação gratuita                   | Não disponível                     |


**P:** Onde posso postar perguntas?

**R:** Você pode postar e marcar sua pergunta com `azure-spot` em [Perguntas e respostas](/answers/topics/azure-spot.html). 

## <a name="next-steps"></a>Próximas etapas

Confira a [página de preços de conjunto de dimensionamento de máquinas virtuais](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) para obter detalhes sobre preços.
