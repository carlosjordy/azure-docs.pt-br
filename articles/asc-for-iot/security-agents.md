---
title: Visão geral do agente de segurança
description: Comece a entender, configurar, implantar e usar a central de segurança do Azure para agentes de serviço de segurança de IoT em seus dispositivos IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 55c8d3b6-3126-4246-8d07-ef88fe5ea84f
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 0d4d21db5185e4564666e526b3edb5ca6d3451e0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81310641"
---
# <a name="get-started-with-azure-security-center-for-iot-device-security-agents"></a>Introdução à central de segurança do Azure para agentes de segurança do dispositivo IoT

A central de segurança do Azure para agentes de segurança de IoT oferece recursos de segurança aprimorados, como monitoramento de conexões remotas, aplicativos ativos, eventos de logon e práticas recomendadas de configuração do sistema operacional. Assuma o controle do seu campo de dispositivo proteção contra ameaças e postura de segurança com um único serviço.

A arquitetura de referência para agentes de segurança do Linux e do Windows, tanto em C# quanto em C, são fornecidas.

A central de segurança do Azure para agentes de segurança de IoT lida com a coleta de eventos brutos do sistema operacional do dispositivo, agregação de eventos para reduzir o custo e a configuração por meio de um módulo de dispositivo. As mensagens de segurança são enviadas por meio do Hub IoT para a central de segurança do Azure para serviços de análise de IoT.

Use o seguinte fluxo de trabalho para implantar e testar a central de segurança do Azure para agentes de segurança de IoT:

1. [Habilitar a central de segurança do Azure para o serviço de IoT para o Hub IoT](quickstart-onboard-iot-hub.md)
1. Se o Hub IoT não tiver nenhum dispositivo registrado, [registre um novo dispositivo](https://docs.microsoft.com/azure/iot-accelerators/quickstart-device-simulation-deploy).
1. [Crie um módulo de segurança do azureiotsecurity](quickstart-create-security-twin.md) para seus dispositivos.
1. Para instalar o agente em um dispositivo simulado do Azure em vez de instalar em um dispositivo real, [crie uma nova VM (máquina virtual) do Azure](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal) em uma zona disponível.
1. [Implante uma central de segurança do Azure para o agente de segurança do IOT](how-to-deploy-linux-cs.md) em seu dispositivo IOT ou nova VM.
1. Siga as instruções para [trigger_events](https://aka.ms/iot-security-github-trigger-events) executar uma simulação inofensiva de um ataque.
1. Verifique a central de segurança do Azure para alertas de IoT em resposta ao ataque simulado na etapa anterior. Inicie a verificação cinco minutos depois de executar o script.
1. Explore [alertas](concept-security-alerts.md), [recomendações](concept-recommendations.md)e aprofunde-se [usando log Analytics](how-to-security-data-access.md) usando o Hub IOT.

## <a name="next-steps"></a>Próximas etapas

- Configurar sua [solução](quickstart-configure-your-solution.md)
- [Criar Módulos de Segurança](quickstart-create-security-twin.md)
- Configurar [alertas personalizados](quickstart-create-custom-alerts.md)
- [Implantar um agente de segurança](how-to-deploy-agent.md)
