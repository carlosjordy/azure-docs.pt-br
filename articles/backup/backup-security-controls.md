---
title: Controles de segurança
description: Saiba mais sobre os controles de segurança usados no serviço de backup do Azure. Esses controles ajudam o serviço a prevenir, detectar e responder a vulnerabilidades de segurança.
ms.topic: conceptual
ms.date: 09/23/2019
ms.openlocfilehash: c5a324ac3738abfcf1a51442d7acd78a01e5717e
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86513533"
---
# <a name="security-controls-for-azure-backup"></a>Controles de segurança para o backup do Azure

Este artigo documenta os controles de segurança incorporados ao backup do Azure.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Rede

| Controle de segurança | Sim/Não | Observações | Documentação
|---|---|--|--|
| Suporte ao ponto de extremidade de serviço| Não |  |  |
| Suporte à injeção de VNet| Não |  |  |
| Isolamento de rede e suporte de firewall| Sim | Há suporte para túnel forçado no backup de VM. Não há suporte para túnel forçado em cargas de trabalho em execução dentro de VMs. |  |
| Suporte a túnel forçado| Não |  |  |

## <a name="monitoring--logging"></a>Monitorando & log

| Controle de segurança | Sim/Não | Observações| Documentação
|---|---|--|--|
| Suporte ao monitoramento do Azure (log Analytics, app insights, etc.)| Sim | Há suporte para Log Analytics por meio de logs de recursos. Para obter mais informações, consulte [monitorar cargas de trabalho protegidas de backup do Azure usando o log Analytics](https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/). |  |
| Registro e auditoria do plano de gerenciamento e controle| Sim | Todas as ações disparadas pelo cliente no portal do Azure são registradas em logs de atividades. |  |
| Log e auditoria do plano de dados| Não | O plano de dados do Backup do Azure não pode ser acessado diretamente.  |  |

## <a name="identity"></a>Identidade

| Controle de segurança | Sim/Não | Observações| Documentação
|---|---|--|--|
| Autenticação| Sim | A autenticação é feita pelo Azure Active Directory. |  |
| Autorização| Sim | São usadas regras de RBAC internas e criadas pelo cliente. Para obter mais informações, consulte [usar o controle de acesso baseado em função para gerenciar pontos de recuperação do backup do Azure](./backup-rbac-rs-vault.md). |  |

## <a name="data-protection"></a>Proteção de dados

| Controle de segurança | Sim/Não | Observações | Documentação
|---|---|--|--|
| Criptografia no lado do servidor em repouso: chaves gerenciadas pela Microsoft | Sim | Usar a criptografia do serviço de armazenamento para contas de armazenamento. |  |
| Criptografia no lado do servidor em repouso: chaves gerenciadas pelo cliente (BYOK) | Não |  |  |
| Criptografia em nível de coluna (serviços de dados do Azure)| Não |  |  |
| Criptografia em trânsito (como criptografia de ExpressRoute, criptografia de vnet e criptografia vnet)| Não | Usar HTTPS. |  |
| Chamadas criptografadas à API| Sim |  |  |

## <a name="configuration-management"></a>Gerenciamento de configuração

| Controle de segurança | Sim/Não | Observações| Documentação
|---|---|--|--|
| Suporte ao gerenciamento de configuração (controle de versão de configuração, etc.)| Sim|  |  |

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre os [controles de segurança internos nos serviços do Azure](../security/fundamentals/security-controls.md).
