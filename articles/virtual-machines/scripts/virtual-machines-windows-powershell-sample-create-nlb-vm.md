---
title: Amostra de script do Azure PowerShell – Criar uma VM NBL Windows
description: Amostra de script do Azure PowerShell – Criar uma VM NBL Windows
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 4b726c125c6a1d5cd5a96428f3125167efba18af
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86500966"
---
# <a name="load-balance-traffic-between-highly-available-virtual-machines"></a>Como balancear a carga de tráfego entre máquinas virtuais altamente disponíveis

Este exemplo de script cria todos os componentes necessários para executar várias máquinas virtuais do Windows Server 2016 configuradas em uma alta disponibilidade e configuração de balanceamento de carga. Após a execução do script, você terá três máquinas virtuais, associadas a um Conjunto de Disponibilidade do Azure, e acessíveis por meio de um Azure Load Balancer.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Exemplo de script

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.ps1 "Create VM NLB")]

## <a name="clean-up-deployment"></a>Limpar a implantação

Execute o comando a seguir para remover o grupo de recursos, a VM e todos os recursos relacionados.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Explicação sobre o script

Esse script usa os seguintes comandos para criar a implantação. Cada item em que a tabela contém links para a documentação específica do comando.

| Comando | Observações |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Cria uma configuração de sub-rede. Essa configuração é usada com o processo de criação de rede virtual. |
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Cria uma rede virtual. |
| [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Cria um endereço IP público. |
| [New-AzLoadBalancerFrontendIpConfig](/powershell/module/az.network/new-azloadbalancerfrontendipconfig) | Cria uma configuração de IP front-end para um balanceador de carga. |
| [New-AzLoadBalancerBackendAddressPoolConfig](/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig) | Cria uma configuração de pool de endereços de back-end para um balanceador de carga. |
| [New-AzLoadBalancerProbeConfig](/powershell/module/az.network/new-azloadbalancerprobeconfig) | Cria uma configuração de investigação para um balanceador de carga. |
| [New-AzLoadBalancerRuleConfig](/powershell/module/az.network/new-azloadbalancerruleconfig) | Cria uma configuração de regra para um balanceador de carga. |
| [New-AzLoadBalancerInboundNatRuleConfig](/powershell/module/az.network/new-azloadbalancerinboundnatruleconfig) | Cria uma configuração de regra NAT de entrada para um balanceador de carga. |
| [New-AzLoadBalancer](/powershell/module/az.network/new-azloadbalancer) | Cria um balanceador de carga. |
| [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig) | Cria uma configuração de regra de grupo de segurança de rede. Essa configuração é usada para criar uma regra NSG quando o NSG é criado. |
| [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) | Cria um grupo de segurança de rede. |
| [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig) | Obtém informações de sub-rede. Essas informações são usadas ao criar um adaptador de rede. |
| [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) | Cria um adaptador de rede. |
| [New-AzVMConfig](/powershell/module/az.compute/new-azvmconfig) | Cria uma configuração de VM. Essa configuração inclui informações como nome da VM, sistema operacional e credenciais administrativas. A configuração é usada durante a criação da VM. |
| [New-AzVM](/powershell/module/az.compute/new-azvm) | Crie uma máquina virtual. |
|[Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Remove um grupo de recursos e todos os recursos contidos nele. |

Você também pode criar VMs usando sua própria imagem gerenciada personalizada. Na configuração da VM, para `Set-AzVMSourceImage` use os parâmetros `-Id` e `-VM` em vez de `-PublisherName`, `-Offer`, `-Skus` e `-Version`.

Por exemplo, criar a configuração da VM seria:

```powershell
$vmConfig = New-AzVMConfig -VMName 'myVM3' -VMSize Standard_DS1_v2 -AvailabilitySetId $as.Id | `
  Set-AzVMOperatingSystem -Windows -ComputerName 'myVM3' -Credential $cred | `
  Set-AzVMSourceImage -Id <Image.ID of the custom managed image> | Add-AzVMNetworkInterface -Id $nicVM3.Id
 ```

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre o módulo do Azure PowerShell, confira [Documentação do Azure PowerShell](/powershell/azure/overview).

Amostras de script do PowerShell da máquina virtual adicionais podem ser encontrados na [documentação da VM Windows do Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
