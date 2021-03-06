---
title: Visão geral das máquinas virtuais
titleSuffix: Azure VMware Solution by CloudSimple
description: Saiba mais sobre as máquinas virtuais CloudSimple e seus benefícios.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 436a5ab6fb286ce206f981487b6a29d774ecd1a1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77024901"
---
# <a name="cloudsimple-virtual-machines-overview"></a>Visão geral das máquinas virtuais CloudSimple

O CloudSimple permite que você gerencie VMs (máquinas virtuais) VMware do portal do Azure.  Um cluster ou um pool de recursos do seu cluster vSphere é gerenciado por meio do Azure, mapeando-o para sua assinatura.

Para criar uma VM CloudSimple do Azure, um modelo de VM deve existir em seu vCenter de nuvem privada.  O modelo é usado para personalizar o sistema operacional e os aplicativos.  A VM de modelo pode ser protegida para atender às políticas de segurança corporativa.  Você pode usar o modelo para criar VMs e, em seguida, consumi-las do portal do Azure usando um modelo de autoatendimento.

## <a name="benefits"></a>Benefícios

As máquinas virtuais CloudSimple da portal do Azure fornecem um mecanismo de autoatendimento para que os usuários criem e gerenciem máquinas virtuais VMware.

* Criar uma VM CloudSimple em sua nuvem privada vCenter
* Gerenciar Propriedades da VM
  * Adicionar/remover discos
  * Adicionar/remover NICs
* Operações de energia de sua VM CloudSimple
  * Ligar e desligar
  * Reiniciar VM
* Excluir VM

## <a name="next-steps"></a>Próximas etapas

* Saiba como [consumir VMs VMware no Azure](quickstart-create-vmware-virtual-machine.md)
* Saiba como [mapear sua assinatura do Azure](azure-subscription-mapping.md)
