---
title: Perguntas frequentes-Azure Monitor para soluções SAP | Microsoft Docs
description: Este artigo fornece respostas para as perguntas frequentes sobre o Azure monitor para soluções SAP
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/30/2020
ms.author: radeltch
ms.openlocfilehash: cf0366300c4fab18a0f6231a97ca050eddd50132
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85852229"
---
# <a name="azure-monitor-for-sap-solutions-faq-preview"></a>Perguntas frequentes sobre o Azure monitor para soluções SAP (versão prévia)
## <a name="frequently-asked-questions"></a>Perguntas frequentes

Este artigo fornece respostas para perguntas frequentes sobre o Azure monitor para soluções SAP.  

 - **Tenho que pagar por Azure Monitor para soluções SAP?**  
Não há nenhuma taxa de licenciamento para Azure Monitor para soluções SAP.  
No entanto, os clientes são responsáveis por arcar com os custos dos componentes do grupo de recursos gerenciados.  

 - **Em quais regiões esse serviço está disponível para visualização pública?**  
Para a visualização pública, esse serviço estará disponível no leste dos EUA 2, oeste dos EUA 2, leste dos EUA e Europa Ocidental.  

 - **Preciso fornecer permissões para permitir a implantação do grupo de recursos gerenciado em minha assinatura?**  
Não, não são necessárias permissões explícitas.  

 - **Onde reside a VM do coletor?**  
No momento da implantação de Azure Monitor para recursos de soluções SAP, recomendamos que os clientes escolham a mesma VNET para o recurso de monitoramento como servidor SAP HANA. Portanto, a VM do coletor é recomendada para residir na mesma VNET que o servidor SAP HANA. Se os clientes estiverem usando um banco de dados não HANA, a VM do coletor residirá na mesma VNET que o banco de dados não HANA.  

 - **Quais versões do HANA têm suporte?**  
HANA 1,0 SPS 12 (Rev. 120 ou superior) e HANA 2,0 SPS03 ou superior  

 - **Quais configurações de implantação do HANA têm suporte?**  
Há suporte para as seguintes configurações:
   - Nó único (expansão) e vários nós (expansão)  
   - Contêiner de banco de dados individual (HANA 1,0 SPS 12) e vários contêineres de banco de dados (HANA 1,0 SPS 12 ou HANA 2,0)  
   - Failover de host automático (n + 1) e HSR  

 - **Quais versões do SQL Server são suportadas?**  
SQL Server 2012 SP4 ou superior.  

 - **Quais configurações de SQL Server têm suporte?**  
Há suporte para as seguintes configurações:
   - Instâncias autônomas padrão ou nomeadas em uma máquina virtual  
   - Instâncias ou instâncias clusterizadas em uma configuração AlwaysOn ao usar o nome virtual do recurso clusterizado ou o nome do ouvinte AlwaysOn. No momento, nenhuma métrica específica de cluster ou AlwaysOn é coletada    
   - Não há suporte para o banco de dados SQL do Azure (PAAS) no momento  

 - **O que acontecerá se eu excluir acidentalmente o grupo de recursos gerenciado?**  
O grupo de recursos gerenciados é bloqueado por padrão. Portanto, as chances de exclusão acidental do grupo de recursos gerenciado pelos clientes são minúsculos.  
Se um cliente excluir o grupo de recursos gerenciado, Azure Monitor para soluções SAP deixará de funcionar. O cliente precisará implantar um novo Azure Monitor para recursos de soluções SAP e começar novamente.  

 - **Quais funções eu preciso em minha assinatura do Azure para implantar Azure Monitor para recursos de soluções SAP?**  
Função de colaborador.  

 - **Qual é o SLA deste produto?**  
As visualizações são excluídas dos contratos de nível de serviço. Leia o termo de licença completa por meio de Azure Monitor para imagem do Marketplace de soluções SAP.  

 - **Posso monitorar meu cenário inteiro por meio dessa solução?**  
Atualmente, você pode monitorar o banco de dados HANA, a infraestrutura subjacente, o cluster de alta disponibilidade e o Microsoft SQL Server em visualização pública.  

 - **Este serviço substitui o SAP Solution Manager?**  
Não. Os clientes ainda podem usar o SAP Solution Manager para monitoramento de processos comerciais.  

 - **Qual é o valor desse serviço em soluções tradicionais como SAP HANA cockpit/Studio?**  
O Azure Monitor para soluções SAP não é específico ao banco de dados HANA. Azure Monitor para soluções SAP também dá suporte ao AnyDB.  

## <a name="next-steps"></a>Próximas etapas

- Crie seu primeiro Azure Monitor para o recurso de soluções SAP.