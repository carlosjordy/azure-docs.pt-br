---
title: Configurar um ponto de extremidade público-SQL do Azure Instância Gerenciada
description: Saiba como configurar um ponto de extremidade público para o Azure SQL Instância Gerenciada
services: sql-database
ms.service: sql-managed-instance
ms.subservice: security
ms.custom: sqldbrb=1
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto, carlrab
ms.date: 05/07/2019
ms.openlocfilehash: 1c2dd3f93abf6418b99bf28d11f2df254b024971
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84708614"
---
# <a name="configure-public-endpoint-in-azure-sql-managed-instance"></a>Configurar um ponto de extremidade público na Instância Gerenciada de SQL do Azure
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

O ponto de extremidade público para uma [instância gerenciada](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index) permite o acesso a dados para sua instância gerenciada de fora da [rede virtual](../../virtual-network/virtual-networks-overview.md). Você pode acessar sua instância gerenciada de serviços multilocatários do Azure, como Power BI, serviço de Azure App ou uma rede local. Usando o ponto de extremidade público em uma instância gerenciada, você não precisa usar uma VPN, o que pode ajudar a evitar problemas de taxa de transferência de VPN.

Neste artigo, você aprenderá a:

> [!div class="checklist"]
>
> - Habilite o ponto de extremidade público para sua instância gerenciada no portal do Azure
> - Habilitar o ponto de extremidade público para sua instância gerenciada usando o PowerShell
> - Configurar o grupo de segurança de rede da instância gerenciada para permitir o tráfego para o ponto de extremidade público da instância gerenciada
> - Obter a cadeia de conexão de ponto de extremidade público da instância gerenciada

## <a name="permissions"></a>Permissões

Devido à sensibilidade dos dados que estão em uma instância gerenciada, a configuração para habilitar o ponto de extremidade público da instância gerenciada requer um processo de duas etapas. Essa medida de segurança segue a separação de tarefas (SoD):

- A habilitação do ponto de extremidade público em uma instância gerenciada precisa ser feita pelo administrador da instância gerenciada. O administrador da instância gerenciada pode ser encontrado na página **visão geral** do recurso de instância gerenciada.
- Permitir o tráfego usando um grupo de segurança de rede que precisa ser feito por um administrador de rede. Para obter mais informações, consulte [permissões de grupo de segurança de rede](../../virtual-network/manage-network-security-group.md#permissions).

## <a name="enabling-public-endpoint-for-a-managed-instance-in-the-azure-portal"></a>Habilitando o ponto de extremidade público para uma instância gerenciada no portal do Azure

1. Inicie o portal do Azure em<https://portal.azure.com/.>
1. Abra o grupo de recursos com a instância gerenciada e selecione a **instância gerenciada do SQL** na qual você deseja configurar o ponto de extremidade público.
1. Nas configurações de **segurança** , selecione a guia **rede virtual** .
1. Na página configuração de rede virtual, selecione **habilitar** e, em seguida, o ícone **salvar** para atualizar a configuração.

![mi-vnet-config.png](./media/public-endpoint-configure/mi-vnet-config.png)

## <a name="enabling-public-endpoint-for-a-managed-instance-using-powershell"></a>Habilitando o ponto de extremidade público para uma instância gerenciada usando o PowerShell

### <a name="enable-public-endpoint"></a>Habilitar o ponto de extremidade público

Execute os comandos do PowerShell a seguir. Substitua **Subscription-ID** pela sua ID de assinatura. Também substitua **RG-Name** pelo grupo de recursos da instância gerenciada e substitua **mi** pelo nome da instância gerenciada.

```powershell
Install-Module -Name Az

Import-Module Az.Accounts
Import-Module Az.Sql

Connect-AzAccount

# Use your subscription ID in place of subscription-id below

Select-AzSubscription -SubscriptionId {subscription-id}

# Replace rg-name with the resource group for your managed instance, and replace mi-name with the name of your managed instance

$mi = Get-AzSqlInstance -ResourceGroupName {rg-name} -Name {mi-name}

$mi = $mi | Set-AzSqlInstance -PublicDataEndpointEnabled $true -force
```

### <a name="disable-public-endpoint"></a>Desabilitar ponto de extremidade público

Para desabilitar o ponto de extremidade público usando o PowerShell, execute o seguinte comando (e também não se esqueça de fechar o NSG da porta de entrada 3342 se você o tiver configurado):

```powershell
Set-AzSqlInstance -PublicDataEndpointEnabled $false -force
```

## <a name="allow-public-endpoint-traffic-on-the-network-security-group"></a>Permitir tráfego de ponto de extremidade público no grupo de segurança de rede

1. Se você tiver a página de configuração da instância gerenciada ainda aberta, navegue até a guia **visão geral** . caso contrário, volte para o recurso de **instância gerenciada do SQL** . Selecione o link **rede virtual/sub-rede** , que levará você para a página de configuração de rede virtual.

    ![mi-overview.png](./media/public-endpoint-configure/mi-overview.png)

1. Selecione a guia **sub-redes** no painel de configuração à esquerda da sua rede virtual e anote o grupo de **segurança** para sua instância gerenciada.

    ![mi-vnet-subnet.png](./media/public-endpoint-configure/mi-vnet-subnet.png)

1. Volte para o grupo de recursos que contém a instância gerenciada. Você deve ver o nome do **grupo de segurança de rede** indicado acima. Selecione o nome para acessar a página de configuração do grupo de segurança de rede.

1. Selecione a guia **regras de segurança de entrada** e **adicione** uma regra com prioridade mais alta do que a regra de **deny_all_inbound** com as seguintes configurações: </br> </br>

    |Configuração  |Valor sugerido  |Descrição  |
    |---------|---------|---------|
    |**Origem**     |Qualquer endereço IP ou marca de serviço         |<ul><li>Para serviços do Azure como Power BI, selecione a marca de serviço de nuvem do Azure</li> <li>Para seu computador ou máquina virtual do Azure, use o endereço IP de NAT</li></ul> |
    |**Intervalos de portas de origem**     |* |Deixe isso para * (qualquer) como as portas de origem geralmente são alocadas dinamicamente e, como tal, imprevisíveis |
    |**Destino**     |Qualquer         |Deixando o destino como qualquer para permitir o tráfego na sub-rede da instância gerenciada |
    |**Intervalos de portas de destino**     |3342         |Porta de destino do escopo para 3342, que é o ponto de extremidade TDS público da instância gerenciada |
    |**Protocolo**     |TCP         |O SQL Instância Gerenciada usa o protocolo TCP para TDS |
    |**Ação**     |Allow         |Permitir o tráfego de entrada para a instância gerenciada por meio do ponto de extremidade público |
    |**Priority**     |1300         |Verifique se essa regra é de prioridade mais alta do que a regra de **deny_all_inbound** |

    ![mi-nsg-rules.png](./media/public-endpoint-configure/mi-nsg-rules.png)

    > [!NOTE]
    > A porta 3342 é usada para conexões de ponto de extremidade público para instância gerenciada e não pode ser alterada neste ponto.

## <a name="obtaining-the-managed-instance-public-endpoint-connection-string"></a>Obtendo a cadeia de conexão de ponto de extremidade público da instância gerenciada

1. Navegue até a página de configuração de instância gerenciada que foi habilitada para o ponto de extremidade público. Selecione a guia **cadeias de conexão** na configuração **configurações** .
1. Observe que o nome do host do ponto de extremidade público é fornecido no formato <mi_name>. **Public**. <dns_zone>. Database.Windows.net e que a porta usada para a conexão é 3342.

    ![mi-public-endpoint-conn-string.png](./media/public-endpoint-configure/mi-public-endpoint-conn-string.png)

## <a name="next-steps"></a>Próximas etapas

Saiba como usar o [Azure SQL instância gerenciada com segurança com o ponto de extremidade público](public-endpoint-overview.md).
