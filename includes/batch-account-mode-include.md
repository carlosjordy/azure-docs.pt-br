---
title: incluir arquivo
description: incluir arquivo
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 05/04/2018
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 51b687dc2a6264eb12362616429746c9b182c3b1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "68323872"
---
> [!NOTE]
> Ao criar uma conta do Lote, você pode escolher entre dois modos de *alocação do pool*: **assinatura de usuário** e **serviço do Lote**. Na maioria dos casos, você deve usar o modo de serviço de Lote padrão, no qual os pools são alocados em segundo plano nas assinaturas gerenciadas do Azure. No modo de assinatura alternativo do usuário, as VMs do Lote e outros recursos são criados diretamente em sua assinatura, quando um pool é criado. Modo de assinatura de usuário é necessário se você quiser criar pools do Lote usando [Instâncias de VM Reservadas do Azure](https://azure.microsoft.com/pricing/reserved-vm-instances/). Para criar uma conta do Lote no modo de assinatura do usuário, você também deverá registrar sua assinatura com o Lote do Azure e associar a conta com um Azure Key Vault.