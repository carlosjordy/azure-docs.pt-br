---
title: Exemplos do Azure PowerShell – Anexar e usar discos de dados
description: Esse script cria um conjunto de dimensionamento de máquinas virtuais do Azure e anexa e prepara os discos de dados usando o PowerShell.
author: mimckitt
ms.author: mimckitt
ms.topic: sample
ms.service: virtual-machine-scale-sets
ms.subservice: disks
ms.date: 06/25/2020
ms.reviewer: jushiman
ms.custom: mimckitt
ms.openlocfilehash: 05d5d901a6ed908dc4ff766a440d5f3e05cb7db3
ms.sourcegitcommit: dfa5f7f7d2881a37572160a70bac8ed1e03990ad
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85373787"
---
# <a name="attach-and-use-data-disks-with-a-virtual-machine-scale-set-with-powershell"></a>Anexar e usar discos de dados com um conjunto de dimensionamento de máquinas virtuais com o PowerShell
Esse script cria um conjunto de dimensionamento de máquinas virtuais e anexa e prepara os discos de dados.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>Exemplo de script

[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/use-data-disks/use-data-disks.ps1 "Create a virtual machine scale set with data disks")]

## <a name="clean-up-deployment"></a>Limpar a implantação
Execute o comando a seguir para remover o grupo de recursos, o conjunto de dimensionamento e todos os recursos relacionados.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Explicação sobre o script
Esse script usa os seguintes comandos para criar a implantação. Cada item em que a tabela contém links para a documentação específica do comando.

| Comando | Observações |
|---|---|
| [New-AzVmss](/powershell/module/az.compute/new-azvmss) | Cria o conjunto de dimensionamento de máquinas virtuais e todos os recursos de suporte, incluindo a rede virtual, o balanceador de carga e as regras de NAT. |
| [Get-AzVmss](/powershell/module/az.compute/get-azvmss) | Obtém informações sobre um conjunto de dimensionamento de máquinas virtuais. |
| [Add-AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) | Adiciona uma extensão de VM para um Script personalizado para instalar um aplicativo Web básico. |
| [Update-AzVmss](/powershell/module/az.compute/update-azvmss) | Atualiza o modelo de conjunto de dimensionamento de máquinas virtuais para aplicar a extensão de VM. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Remove um grupo de recursos e todos os recursos contidos nele. |

## <a name="next-steps"></a>Próximas etapas
Para obter mais informações sobre o módulo do Azure PowerShell, confira [Documentação do Azure PowerShell](/powershell/azure/overview).
