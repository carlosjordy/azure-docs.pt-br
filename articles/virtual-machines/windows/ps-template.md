---
title: Criar uma VM do Windows a partir de um modelo no Azure
description: Use um modelo do Resource Manager e o PowerShell para criar facilmente uma nova VM do Windows.
author: cynthn
ms.service: virtual-machines-windows
ms.topic: article
ms.date: 03/22/2019
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 58b8a22ff36f85f25bb6a4b22617029fe3858660
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86998860"
---
# <a name="create-a-windows-virtual-machine-from-a-resource-manager-template"></a>Criar uma máquina virtual Windows usando um modelo do Resource Manager

Saiba como criar uma máquina virtual do Windows usando um modelo de Azure Resource Manager e Azure PowerShell do Azure cloud Shell. O modelo usado neste artigo implanta uma única máquina virtual executando o Windows Server em uma nova rede virtual com uma única sub-rede. Para criar uma máquina virtual do Linux, consulte [como criar uma máquina virtual Linux com modelos de Azure Resource Manager](../linux/create-ssh-secured-vm-from-template.md).

## <a name="create-a-virtual-machine"></a>Criar uma máquina virtual

A criação de uma máquina virtual do Azure geralmente inclui duas etapas:

- Crie um grupos de recursos. Um grupo de recursos do Azure é um contêiner lógico no qual os recursos do Azure são implantados e gerenciados. Você deve criar um grupo de recursos antes de criar uma máquina virtual.
- Crie uma máquina virtual.

O exemplo a seguir cria uma VM a partir de um [modelo de início rápido do Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json). Veja uma cópia do modelo:

[!code-json[create-windows-vm](~/quickstart-templates/101-vm-simple-windows/azuredeploy.json)]

Para executar o script do PowerShell, selecione **Experimente-** o para abrir o Azure cloud Shell. Para colar o script, clique com o botão direito do mouse no Shell e selecione **colar**:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the administrator username"
$adminPassword = Read-Host -Prompt "Enter the administrator password" -AsSecureString
$dnsLabelPrefix = Read-Host -Prompt "Enter an unique DNS name for the public IP"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json" `
    -adminUsername $adminUsername `
    -adminPassword $adminPassword `
    -dnsLabelPrefix $dnsLabelPrefix

 (Get-AzVm -ResourceGroupName $resourceGroupName).name

```

Se você optar por instalar e usar o PowerShell localmente em vez do Azure cloud Shell, este tutorial exigirá o módulo Azure PowerShell. Execute `Get-Module -ListAvailable Az` para encontrar a versão. Se você precisa atualizar, consulte [Instalar o módulo do Azure PowerShell](/powershell/azure/install-az-ps). Se você estiver executando o PowerShell localmente, também precisará executar o `Connect-AzAccount` para criar uma conexão com o Azure.

No exemplo anterior, você especificou um modelo armazenado no GitHub. Também é possível baixar ou criar um modelo e especificar o caminho local com o parâmetro `--template-file`.

Estes são alguns recursos adicionais:

- Para saber como desenvolver modelos do Resource Manager, confira a [Documentação do Azure Resource Manager](../../azure-resource-manager/index.yml).
- Para ver os esquemas de máquina virtual do Azure, consulte [referência de modelo do Azure](/azure/templates/microsoft.compute/allversions).
- Para ver mais exemplos de modelo de máquina virtual, consulte [modelos de início rápido do Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Compute&pageNumber=1&sort=Popular).

## <a name="connect-to-the-virtual-machine"></a>Conecte-se à máquina virtual

O último comando do PowerShell do script anterior mostra o nome da máquina virtual. Para se conectar à máquina virtual, consulte [como se conectar e entrar em uma máquina virtual do Azure que executa o Windows](./connect-logon.md).

## <a name="next-steps"></a>Próximas etapas

- Se houver problemas com a implantação, confira [Solução de erros de implantação comuns do Azure com o Azure Resource Manager](../../azure-resource-manager/templates/common-deployment-errors.md).
- Saiba como criar e gerenciar uma máquina virtual em [Criar e gerenciar VMs Windows com o módulo do Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Confira a sintaxe e as propriedades do JSON para os tipos de recursos que você implantou para saber mais sobre a criação de modelos:

- [Microsoft.Network/publicIPAddresses](/azure/templates/microsoft.network/publicipaddresses)
- [Microsoft.Network/virtualNetworks](/azure/templates/microsoft.network/virtualnetworks)
- [Microsoft.Network/networkInterfaces](/azure/templates/microsoft.network/networkinterfaces)
- [Microsoft.Compute/virtualMachines](/azure/templates/microsoft.compute/virtualmachines)
