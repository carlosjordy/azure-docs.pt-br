---
title: Latência de conexão de usuário da área de trabalho virtual do Windows – Azure
description: Latência de conexão para usuários da área de trabalho virtual do Windows.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 8a60779fb045aa612a6ba0988c4635752f973f60
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82607394"
---
# <a name="determine-user-connection-latency-in-windows-virtual-desktop"></a>Determinar a latência de conexão de usuário na área de trabalho virtual do Windows

A área de trabalho virtual do Windows está disponível globalmente. Os administradores podem criar máquinas virtuais (VMs) em qualquer região do Azure que desejarem. A latência de conexão irá variar dependendo do local dos usuários e das máquinas virtuais. Os serviços de área de trabalho virtual do Windows serão continuamente distribuídos para novas regiões para melhorar a latência. 
 
A [ferramenta estimador de experiência de área de trabalho virtual do Windows](https://azure.microsoft.com/services/virtual-desktop/assessment/) pode ajudá-lo a determinar o melhor local para otimizar a latência de suas VMs. Recomendamos que você use a ferramenta a cada dois ou três meses para garantir que o local ideal não seja alterado à medida que a área de trabalho virtual do Windows se acumula em novas áreas. 

## <a name="azure-traffic-manager"></a>Gerenciador de Tráfego do Azure

A área de trabalho virtual do Windows usa o Gerenciador de tráfego do Azure, que verifica o local do servidor DNS do usuário para encontrar a instância mais próxima do serviço de área de trabalho virtual do Windows. Recomendamos que os administradores examinem o local do servidor DNS do usuário antes de escolher o local para as VMs.

## <a name="next-steps"></a>Próximas etapas

- Para verificar o melhor local para obter a latência ideal, consulte a [ferramenta estimador de experiência de área de trabalho virtual do Windows](https://azure.microsoft.com/services/virtual-desktop/assessment/).
- Para os planos de preços, consulte [preços da área de trabalho virtual do Windows](https://azure.microsoft.com/pricing/details/virtual-desktop/).
- Para começar a usar sua implantação de área de trabalho virtual do Windows, confira [nosso tutorial](./virtual-desktop-fall-2019/tenant-setup-azure-active-directory.md).