---
title: Melhorar o excellency operacional com o Advisor
description: Use o Azure Advisor para otimizar e amadurecer sua excelência operacional para suas assinaturas do Azure.
ms.topic: article
ms.date: 10/24/2019
ms.openlocfilehash: 2b4c4726400134e4eec3868e155da47cb8c515b5
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87057641"
---
# <a name="achieve-operational-excellence-by-using-azure-advisor"></a>Obtenha excelência operacional usando o Azure Advisor

As recomendações de excelência operacional no Azure Advisor podem ajudá-lo com: 
- Eficiência do processo e do fluxo de trabalho.
- Capacidade de gerenciamento de recursos.
- Práticas recomendadas de implantação. 

Você pode obter essas recomendações na guia **excelência operacional** do painel do Advisor.

## <a name="create-azure-service-health-alerts-to-be-notified-when-azure-problems-affect-you"></a>Crie alertas de integridade do serviço do Azure para ser notificado quando os problemas do Azure afetarem você

Recomendamos que você configure alertas de integridade do serviço do Azure para que você seja notificado quando os problemas de serviço do Azure afetarem você. A [integridade do serviço do Azure](https://azure.microsoft.com/features/service-health/) é um serviço gratuito que fornece orientação e suporte personalizados quando você é afetado por um problema de serviço do Azure. O Advisor identifica as assinaturas que não têm alertas configurados e recomenda configurá-los.


## <a name="design-your-storage-accounts-to-prevent-reaching-the-maximum-subscription-limit"></a>Projete suas contas de armazenamento para evitar atingir o limite máximo de assinaturas

Uma região do Azure pode dar suporte a um máximo de 250 contas de armazenamento por assinatura. Depois de atingir esse limite, você não poderá criar contas de armazenamento nessa combinação de região/assinatura. O Advisor verifica suas assinaturas e fornece recomendações para você projetar para menos contas de armazenamento para qualquer região/assinatura que esteja perto de atingir o limite.

## <a name="ensure-you-have-access-to-azure-cloud-experts-when-you-need-it"></a>Certifique-se de que você terá acesso aos especialistas de nuvem do Azure quando precisar

Ao executar uma carga de trabalho comercialmente crítica, é importante ter acesso ao suporte técnico quando necessário. O Advisor identifica as possíveis assinaturas críticas para os negócios que não têm suporte técnico incluído em seu plano de suporte. Ele recomenda a atualização para uma opção que inclui suporte técnico.

## <a name="delete-and-re-create-your-pool-to-remove-a-deprecated-internal-component"></a>Excluir e recriar o pool para remover um componente interno preterido

Se o seu pool estiver usando um componente interno preterido, exclua e recrie o pool para melhorar a estabilidade e o desempenho.

## <a name="repair-invalid-log-alert-rules"></a>Reparar regras de alerta de log inválidas

O Azure Advisor detecta regras de alerta que têm consultas inválidas especificadas na seção condição. Você pode criar regras de alerta de log no Azure Monitor e usá-las para executar consultas de análise em intervalos especificados. Os resultados da consulta determinarão se um alerta precisar ser disparado. As consultas de análise podem se tornar inválidas ao longo do tempo devido a alterações em recursos, tabelas ou comandos referenciados. O Advisor recomenda que você corrija a consulta na regra de alerta para impedir que ela seja automaticamente desativada e garantir a cobertura de monitoramento de seus recursos no Azure. [Saiba mais sobre como solucionar problemas de regras de alerta.](https://aka.ms/aa_logalerts_queryrepair)

## <a name="use-azure-policy-recommendations"></a>Usar recomendações de Azure Policy

Azure Policy é um serviço no Azure que você pode usar para criar, atribuir e gerenciar políticas. Essas políticas impõem regras e efeitos em seus recursos. As recomendações de Azure Policy a seguir podem ajudá-lo a obter o excellency operacional: 

**Gerenciar marcas.** Essa política adiciona ou substitui a tag e o valor especificados quando qualquer recurso é criado ou atualizado. Você pode corrigir os recursos existentes disparando uma tarefa de correção. Essa política não modifica marcas em grupos de recursos.

**Impor os requisitos de conformidade geográfica.** Esta política permite que você restrinja os locais que sua organização pode especificar ao implantar recursos. 

**Especifique SKUs de máquina virtual permitidas para implantações.** Esta política permite especificar um conjunto de SKUs de máquina virtual que sua organização pode implantar.

**Impor as *VMs de auditoria que não usam discos gerenciados*.**

**Habilitar *herdar uma marca de grupos de recursos*.** Essa política adiciona ou substitui a tag e o valor especificados do grupo de recursos pai quando qualquer recurso é criado ou atualizado. Você pode corrigir os recursos existentes disparando uma tarefa de correção.

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre as recomendações do Assistente, consulte:
* [Introdução ao Advisor](advisor-overview.md)
* [Introdução](advisor-get-started.md)
* [Recomendações de custo do Advisor](advisor-cost-recommendations.md)
* [Recomendações de desempenho do Advisor](advisor-performance-recommendations.md)
* [Recomendações de confiabilidade do Advisor](advisor-high-availability-recommendations.md)
* [Recomendações de segurança do Advisor](advisor-security-recommendations.md)
* [API REST do Advisor](/rest/api/advisor/)
