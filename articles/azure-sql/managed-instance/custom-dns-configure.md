---
title: DNS Personalizado
titleSuffix: Azure SQL Managed Instance
description: Este tópico descreve as opções de configuração para um DNS personalizado com o Azure SQL Instância Gerenciada.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 07/17/2019
ms.openlocfilehash: 2ba5794ba647c28cde3b54a1afdfbd0201b23e8e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84706147"
---
# <a name="configure-a-custom-dns-for-azure-sql-managed-instance"></a>Configurar um DNS personalizado para o Azure SQL Instância Gerenciada
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

O Azure SQL Instância Gerenciada deve ser implantado em uma [VNet (rede virtual)](../../virtual-network/virtual-networks-overview.md)do Azure. Há alguns cenários (por exemplo, db mail, servidores vinculados a outras instâncias do SQL Server em seu ambiente de nuvem ou híbrido) que exigem nomes de hosts privados para serem resolvidos na Instância Gerenciada de SQL. Nesse caso, você precisa configurar um DNS personalizado dentro do Azure. 

Como o SQL Instância Gerenciada usa o mesmo DNS para seus trabalhos internos, configure o servidor DNS personalizado para que ele possa resolver nomes de domínio públicos.

> [!IMPORTANT]
> Sempre use um FQDN (nome de domínio totalmente qualificado) para o servidor de email, para a instância do SQL Server e para outros serviços, mesmo que eles estejam dentro de sua zona DNS privada. Por exemplo, use `smtp.contoso.com` para seu servidor de email porque o `smtp` não resolverá corretamente. Criar um servidor vinculado ou replicação que referencie SQL Server VMs dentro da mesma rede virtual também exige um FQDN e um sufixo DNS padrão. Por exemplo, `SQLVM.internal.cloudapp.net`. Para obter mais informações, consulte [resolução de nomes que usa seu próprio servidor DNS](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server).

> [!IMPORTANT]
> A atualização de servidores DNS da rede virtual não afetará o SQL Instância Gerenciada imediatamente. A configuração de DNS do SQL Instância Gerenciada é atualizada após a concessão do DHCP expirar ou após a atualização da plataforma, o que ocorrer primeiro. **Os usuários são aconselhados a definir sua configuração de DNS de rede virtual antes de criar sua primeira instância gerenciada.**

## <a name="next-steps"></a>Próximas etapas

- Para obter uma visão geral, consulte [o que é o Azure SQL instância gerenciada?](sql-managed-instance-paas-overview.md).
- Para obter um tutorial mostrando como criar uma nova instância gerenciada, consulte [criar uma instância gerenciada](instance-create-quickstart.md).
- Para obter informações sobre como configurar uma VNet para uma instância gerenciada, consulte [configuração de vnet para instâncias gerenciadas](connectivity-architecture-overview.md).
