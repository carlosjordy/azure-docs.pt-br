---
title: Limitações de pools de nó do Windows Server
titleSuffix: Azure Kubernetes Service
description: Saiba mais sobre as limitações conhecidas ao executar pools de nós do Windows Server e cargas de trabalho de aplicativo no serviço kubernetes do Azure (AKS)
services: container-service
ms.topic: article
ms.date: 05/28/2020
ms.openlocfilehash: a86d6f0fe942a72a96c504a61d5030624f161cd5
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86507005"
---
# <a name="current-limitations-for-windows-server-node-pools-and-application-workloads-in-azure-kubernetes-service-aks"></a>Limitações atuais para pools de nós do Windows Server e cargas de trabalho de aplicativo no serviço kubernetes do Azure (AKS)

No serviço de kubernetes do Azure (AKS), você pode criar um pool de nós que executa o Windows Server como o sistema operacional convidado nos nós. Esses nós podem executar aplicativos de contêiner do Windows nativos, como os criados no .NET Framework. Como há diferenças importantes em como o sistema operacional Linux e Windows fornece suporte a contêineres, alguns recursos comuns relacionados a kubernetes e Pod não estão disponíveis atualmente para pools de nós do Windows.

Este artigo descreve algumas das limitações e os conceitos de sistema operacional para nós do Windows Server no AKS.

## <a name="which-windows-operating-systems-are-supported"></a>Quais sistemas operacionais Windows têm suporte?

O AKS usa o Windows Server 2019 como a versão do sistema operacional do host e só dá suporte ao isolamento do processo. Não há suporte para imagens de contêiner criadas usando outras versões do Windows Server. [Compatibilidade de versão do contêiner do Windows][windows-container-compat]

## <a name="is-kubernetes-different-on-windows-and-linux"></a>O kubernetes é diferente no Windows e no Linux?

O suporte ao pool de nós do servidor de janelas inclui algumas limitações que fazem parte do Windows Server upstream no projeto kubernetes. Essas limitações não são específicas do AKS. Para obter mais informações sobre esse suporte upstream para o Windows Server no kubernetes, consulte a seção [funcionalidade e limitações com suporte][upstream-limitations] do [introdução ao suporte do Windows no documento kubernetes][intro-windows] , do projeto kubernetes.

O kubernetes é historicamente focado em Linux. Muitos exemplos usados no site do upstream [kubernetes.Io][kubernetes] são destinados para uso em nós do Linux. Quando você cria implantações que usam contêineres do Windows Server, as seguintes considerações no nível do sistema operacional se aplicam:

- **Identidade** – o Linux identifica um usuário por um UID (identificador de usuário inteiro). Um usuário também tem um nome de usuário alfanumérico para fazer logon, que o Linux traduz para a UID do usuário. Da mesma forma, o Linux identifica um grupo de usuários por um GID (identificador de grupo inteiro) e converte um nome de grupo em seu GID correspondente.
    - O Windows Server usa um SID (identificador de segurança binário) maior que é armazenado no banco de dados SAM (Gerenciador de acesso à segurança do Windows). Esse banco de dados não é compartilhado entre o host e os contêineres ou entre contêineres.
- **Permissões de arquivo** -o Windows Server usa uma lista de controle de acesso baseada em SIDs, em vez de um bitmask de permissões e uid + gid
- **Caminhos de arquivo** -a Convenção no Windows Server é usar \ em vez de/.
    - Em especificações de Pod que montam volumes, especifique o caminho corretamente para contêineres do Windows Server. Por exemplo, em vez de um ponto de montagem de */mnt/volume* em um contêiner do Linux, especifique uma letra de unidade e um local como */K/volume* para montar como a unidade *K:* .

## <a name="what-kind-of-disks-are-supported-for-windows"></a>Que tipos de discos têm suporte para o Windows?

Os discos do Azure e os arquivos do Azure são os tipos de volumes com suporte, acessados como volumes NTFS no contêiner do Windows Server.

## <a name="can-i-run-windows-only-clusters-in-aks"></a>Posso executar clusters somente do Windows no AKS?

Os nós mestres (o plano de controle) em um cluster AKS são hospedados pelo AKS do serviço, você não será exposto ao sistema operacional dos nós que hospedam os componentes mestres. Todos os clusters AKS são criados com um pool de primeiro nó padrão, que é baseado em Linux. Esse pool de nós contém serviços do sistema, que são necessários para que o cluster funcione. É recomendável executar pelo menos dois nós no primeiro pool de nós para garantir a confiabilidade do cluster e a capacidade de realizar operações de cluster. O primeiro pool de nós baseado em Linux não pode ser excluído, a menos que o próprio cluster AKS seja excluído.

## <a name="what-network-plug-ins-are-supported"></a>Quais plug-ins de rede têm suporte?

Clusters AKS com pools de nós do Windows devem usar o modelo de rede CNI do Azure (avançado). Não há suporte para a rede Kubenet (básica). Para obter mais informações sobre as diferenças em modelos de rede, consulte [conceitos de rede para aplicativos no AKs][azure-network-models]. O modelo de rede CNI do Azure requer planejamento e considerações adicionais para o gerenciamento de endereços IP. Para obter mais informações sobre como planejar e implementar o Azure CNI, consulte [Configurar a rede CNI do Azure no AKs][configure-azure-cni].

## <a name="is-preserving-the-client-source-ip-supported"></a>Há suporte para a preservação do IP de origem do cliente?

Neste momento, não há suporte para a [preservação do IP de origem do cliente][client-source-ip] com nós do Windows.

## <a name="can-i-change-the-max--of-pods-per-node"></a>Posso alterar o máximo. n º de pods por nó?

Sim. Para as implicações e opções disponíveis, consulte [número máximo de pods][maximum-number-of-pods].

## <a name="how-do-patch-my-windows-nodes"></a>Como corrigir meus nós do Windows?

Os nós do Windows Server no AKS devem ser *atualizados* para obter as correções e atualizações mais recentes do patch. As atualizações do Windows não estão habilitadas em nós no AKS. O AKS lança novas imagens do pool de nós assim que os patches estão disponíveis, é responsabilidade dos clientes atualizarem pools de nós para permanecerem atualizados sobre patches e hotfix. Isso também é verdadeiro para a versão kubernetes que está sendo usada. As notas de versão do AKS indicarão quando novas versões estão disponíveis. Para obter mais informações sobre como atualizar um pool de nós do Windows Server, consulte [atualizar um pool de nós no AKs][nodepool-upgrade].

> [!NOTE]
> A imagem atualizada do Windows Server será usada somente se uma atualização de cluster (atualização do plano de controle) tiver sido executada antes da atualização do pool de nós
>

## <a name="why-am-i-seeing-an-error-when-i-try-to-create-a-new-windows-agent-pool"></a>Por que estou vendo um erro ao tentar criar um novo pool do agente do Windows?

Se você criou o cluster antes de fevereiro de 2020 e nunca fez nenhuma operação de atualização de cluster, o cluster ainda usa uma imagem antiga do Windows. Você pode ter visto um erro semelhante a:

"A seguinte lista de imagens referenciadas do modelo de implantação não foi encontrada: Editor: MicrosoftWindowsServer, oferta: WindowsServer, SKU: 2019-datacenter-Core-smalldisk-2004, versão: mais recente. Consulte https://docs.microsoft.com/azure/virtual-machines/windows/cli-ps-findimage para obter instruções sobre como localizar imagens disponíveis. "

Para corrigir isso:

1. Atualize o [plano de controle de cluster][upgrade-cluster-cp]. Isso atualizará a oferta de imagem e o Publicador.
1. Crie novos pools do agente do Windows.
1. Mova os pods do Windows de pools existentes do agente do Windows para novos pools do agente do Windows.
1. Exclua pools antigos do agente do Windows.

## <a name="how-do-i-rotate-the-service-principal-for-my-windows-node-pool"></a>Como fazer girar a entidade de serviço para meu pool de nós do Windows?

Os pools de nós do Windows não dão suporte à rotação de entidade de serviço. Para atualizar a entidade de serviço, crie um novo pool de nós do Windows e migre o pods do pool mais antigo para o novo. Quando isso for concluído, exclua o pool de nós mais antigo.

## <a name="how-many-node-pools-can-i-create"></a>Quantos pools de nós posso criar?

O cluster AKS pode ter um máximo de dez pools de nós. Você pode ter um máximo de 1000 nós entre esses pools de nós. [Limitações do pool de nós][nodepool-limitations].

## <a name="what-can-i-name-my-windows-node-pools"></a>O que posso nomear meus pools de nós do Windows?

Você precisa manter o nome em um máximo de 6 (seis) caracteres. Essa é uma limitação atual do AKS.

## <a name="are-all-features-supported-with-windows-nodes"></a>Todos os recursos têm suporte com nós do Windows?

As políticas de rede e kubenet não têm suporte no momento com nós do Windows.

## <a name="can-i-run-ingress-controllers-on-windows-nodes"></a>Posso executar controladores de entrada em nós do Windows?

Sim, um controlador de entrada que dá suporte a contêineres do Windows Server pode ser executado em nós do Windows no AKS.

## <a name="can-i-use-azure-dev-spaces-with-windows-nodes"></a>Posso usar Azure Dev Spaces com nós do Windows?

Atualmente, o Azure Dev Spaces está disponível apenas para pools de nós baseados em Linux.

## <a name="can-my-windows-server-containers-use-gmsa"></a>Meus contêineres do Windows Server podem usar o gMSA?

O suporte ao grupo de contas de serviço gerenciado (gMSA) não está disponível no momento no AKS.

## <a name="can-i-use-azure-monitor-for-containers-with-windows-nodes-and-containers"></a>Posso usar Azure Monitor para contêineres com nós e contêineres do Windows?

Sim, você pode, no entanto, Azure Monitor está em visualização pública para coletar logs (stdout, stderr) e métricas de contêineres do Windows. Você também pode anexar à transmissão ao vivo de logs stdout de um contêiner do Windows.

## <a name="are-there-any-limitations-on-the-number-of-services-on-a-cluster-with-windows-nodes"></a>Há limitações quanto ao número de serviços em um cluster com nós do Windows?

Um cluster com nós do Windows pode ter aproximadamente 500 serviços antes de encontrar a exaustão da porta.

## <a name="can-i-use-the-kubernetes-web-dashboard-with-windows-containers"></a>Posso usar o painel da Web do kubernetes com contêineres do Windows?

Sim, você pode usar o [painel da Web do kubernetes][kubernetes-dashboard] para acessar informações sobre contêineres do Windows, mas, neste momento, não é possível executar o *kubectl exec* em um contêiner do Windows em execução diretamente do painel da Web do kubernetes. Para obter mais detalhes sobre como se conectar ao seu contêiner do Windows em execução, consulte [conectar-se com o RDP para os nós do Windows Server do cluster do AKS (serviço kubernetes do Azure) para manutenção ou solução de problemas][windows-rdp].

## <a name="what-if-i-need-a-feature-which-is-not-supported"></a>E se eu precisar de um recurso que não tenha suporte?

Trabalhamos muito para reunir todos os recursos de que você precisa para o Windows no AKS, mas se você encontrar lacunas, o projeto de [AKs-Engine][aks-engine] de software livre fornecerá uma maneira fácil e totalmente personalizável de executar o kubernetes no Azure, incluindo o suporte do Windows. Certifique-se de conferir nosso roteiro de recursos que estão chegando ao [roteiro do AKS][aks-roadmap].

## <a name="next-steps"></a>Próximas etapas

Para começar a usar contêineres do Windows Server no AKS, [crie um pool de nós que executa o Windows Server em AKs][windows-node-cli].

<!-- LINKS - external -->
[kubernetes]: https://kubernetes.io
[aks-engine]: https://github.com/azure/aks-engine
[upstream-limitations]: https://kubernetes.io/docs/setup/production-environment/windows/intro-windows-in-kubernetes/#supported-functionality-and-limitations
[intro-windows]: https://kubernetes.io/docs/setup/production-environment/windows/intro-windows-in-kubernetes/
[aks-roadmap]: https://github.com/Azure/AKS/projects/1

<!-- LINKS - internal -->
[azure-network-models]: concepts-network.md#azure-virtual-networks
[configure-azure-cni]: configure-azure-cni.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[windows-node-cli]: windows-container-cli.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[upgrade-cluster]: upgrade-cluster.md
[upgrade-cluster-cp]: use-multiple-node-pools.md#upgrade-a-cluster-control-plane-with-multiple-node-pools
[azure-outbound-traffic]: ../load-balancer/load-balancer-outbound-connections.md#defaultsnat
[nodepool-limitations]: use-multiple-node-pools.md#limitations
[windows-container-compat]: /virtualization/windowscontainers/deploy-containers/version-compatibility?tabs=windows-server-2019%2Cwindows-10-1909
[maximum-number-of-pods]: configure-azure-cni.md#maximum-pods-per-node
[azure-monitor]: ../azure-monitor/insights/container-insights-overview.md#what-does-azure-monitor-for-containers-provide
[client-source-ip]: concepts-network.md#ingress-controllers
[kubernetes-dashboard]: kubernetes-dashboard.md
[windows-rdp]: rdp.md
