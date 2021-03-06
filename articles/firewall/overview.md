---
title: O que é o Firewall do Azure?
description: Firewall do Azure é um serviço de segurança de rede gerenciado e baseado em nuvem que protege seus recursos de Rede Virtual do Azure.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: overview
ms.custom: mvc
ms.date: 06/18/2020
ms.author: victorh
Customer intent: As an administrator, I want to evaluate Azure Firewall so I can determine if I want to use it.
ms.openlocfilehash: 7a5b21551cd549f6a495f6cca7a8c5f96c72ddaa
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85081006"
---
# <a name="what-is-azure-firewall"></a>O que é o Firewall do Azure?

![Certificação ICSA](media/overview/icsa-cert-firewall-small.png)

Firewall do Azure é um serviço de segurança de rede gerenciado e baseado em nuvem que protege seus recursos de Rede Virtual do Azure. É um firewall como serviço totalmente com estado com alta disponibilidade interna e escalabilidade de nuvem irrestrita.

![Visão geral do firewall](media/overview/firewall-threat.png)

É possível criar, impor e registrar centralmente políticas de conectividade de rede e de aplicativo em assinaturas e redes virtuais. O Firewall do Azure usa um endereço IP público estático para seus recursos de rede virtual, permitindo que firewalls externos identifiquem o tráfego originário de sua rede virtual.  O serviço é totalmente integrado ao Azure Monitor para registro em log e análise.

## <a name="features"></a>Recursos

Para saber mais sobre os recursos do Firewall do Azure, confira [Recursos do Firewall do Azure](features.md).

## <a name="known-issues"></a>Problemas conhecidos

O Firewall do Azure tem os seguintes problemas conhecidos:

|Problema  |Descrição  |Atenuação  |
|---------|---------|---------|
As regras de filtragem de rede para protocolos não TCP/UDP (por exemplo, ICMP) não funcionam para o tráfego vinculado à Internet|As regras de filtragem de rede para protocolos não TCP/UDP não funcionam com o SNAT para seu endereço IP público. Protocolos não TCP/UDP têm suporte entre VNets e sub-redes spoke.|O Firewall do Azure usa o Standard Load Balancer [que não dá suporte a SNAT para protocolos IP](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview). Estamos explorando as opções para dar suporte a esse cenário em uma versão futura.|
|Suporte do PowerShell e da CLI ausente para ICMP|O Azure PowerShell e a CLI não dão suporte ao ICMP como um protocolo válido nas regras de rede.|Ainda é possível usar o ICMP como protocolo por meio do portal e da API REST. Estamos trabalhando para adicionar o ICMP no PowerShell e na CLI em breve.|
|As marcas de FQDN requerem que um protocolo:porta seja definido|As regras de aplicativo com marcas de FQDN exigem a definição de um protocolo:porta.|Você pode usar **HTTPS** como o valor de porta:protocolo. Estamos trabalhando para tornar esse campo opcional quando marcas de FQDN são usadas.|
|Não há suporte para a movimentação de um firewall para um grupo de recursos ou uma assinatura diferente|Não há suporte para a movimentação de um firewall para um grupo de recursos ou uma assinatura diferente.|O suporte a essa funcionalidade está em nosso roteiro. Para mover um firewall para um grupo de recursos ou uma assinatura diferente, você precisa excluir a instância atual e recriá-la no novo grupo de recursos ou na nova assinatura.|
|Alertas de inteligência de ameaças podem ser mascarados|As regras de rede com destino 80/443 para filtragem de saída mascaram os alertas de inteligência de ameaças quando configuradas para o modo somente alerta.|Crie a filtragem de saída para 80/443 usando regras de aplicativo. Ou, alterar o modo de inteligência contra ameaças para **Alertar e negar**.|
|O Firewall do Azure usa apenas DNS do Azure para resolução de nome|O Firewall do Azure resolve FQDNs usando apenas o DNS do Azure. Não há suporte para um servidor DNS personalizado. Não há nenhum impacto à resolução de DNS em outras sub-redes.|Estamos trabalhando para afrouxar essa limitação.|
|O DNAT de Firewall do Azure não funciona para destinos de IP privados|O suporte para DNAT do Firewall do Azure é limitado à saída/entrada da Internet. No momento, o DNAT não funciona para destinos de IP privados. Por exemplo, spoke para spoke.|Esta é uma limitação atual.|
|Não é possível remover a primeira configuração de IP público|Cada endereço IP público do Firewall do Azure é atribuído a uma *configuração de IP*.  A primeira configuração de IP é atribuída durante a implantação do firewall e geralmente também contém uma referência à sub-rede do firewall (a menos que configurado de maneira explicitamente diferente por meio de uma implantação de modelo). Não é possível excluir essa configuração de IP, pois ele desalocaria o firewall. Você ainda poderá alterar ou remover o endereço IP público associado a essa configuração de IP se o firewall tiver pelo menos um outro endereço IP público disponível para uso.|Isso ocorre por design.|
|As Zonas de disponibilidade só podem ser configuradas durante a implantação.|As Zonas de disponibilidade só podem ser configuradas durante a implantação. Você não pode configurar Zonas de Disponibilidade após a implantação de um firewall.|Isso ocorre por design.|
|SNAT em conexões de entrada|Além de DNAT, as conexões via o endereço IP público do firewall (entrada) estão no modo SNAT para um dos IPs privados do firewall. Esse requisito hoje (e também para NVAs ativa/ativa) garante o roteamento simétrico.|Para preservar a fonte original para HTTP/S, use os cabeçalhos [XFF](https://en.wikipedia.org/wiki/X-Forwarded-For). Por exemplo, use um serviço como o [Azure Front Door](../frontdoor/front-door-http-headers-protocol.md#front-door-to-backend) ou o [Gateway de Aplicativo do Azure](../application-gateway/rewrite-http-headers.md) na frente do firewall. Você também pode adicionar o WAF como parte Porta da frente do Azure e encadear ao firewall.
|Suporte para filtragem de FQDN do SQL apenas no modo de proxy (porta 1433)|Para o Banco de Dados SQL do Azure, o SQL Data Warehouse do Azure e a Instância Gerenciada do SQL do Azure:<br><br>Durante a versão prévia, a filtragem de FQDN do SQL tem suporte apenas no modo de proxy (a porta 1433).<br><br>Para IaaS do SQL do Azure:<br><br>Se estiver usando portas não padrão, você poderá especificar essas portas nas regras do aplicativo.|Para o SQL no modo de redirecionamento (o padrão ao se conectar de dentro do Azure), você pode filtrar o acesso usando a tag de serviço do SQL como parte das regras de rede do Firewall do Azure.
|O tráfego de saída na porta TCP 25 não é permitido| As conexões SMTP de saída que usam a porta TCP 25 foram bloqueadas. A porta 25 é usada principalmente para entrega de email não autenticado. Esse é o comportamento de plataforma padrão para máquinas virtuais. Para saber mais, confira [Solucionar problemas de conectividade de SMTP de saída no Azure](../virtual-network/troubleshoot-outbound-smtp-connectivity.md). No entanto, ao contrário de máquinas virtuais, no momento, não é possível habilitar essa funcionalidade no Firewall do Azure. Observação: para permitir o SMTP autenticado (porta 587) ou o SMTP por uma porta diferente de 25, configure uma regra de rede e não uma regra de aplicativo, porque não há suporte para a inspeção de SMTP no momento.|Siga o método recomendado para enviar email conforme documentado no artigo de solução de problemas de SMTP. Ou exclua a máquina virtual que precisa de acesso SMTP de saída da rota padrão para o firewall. Em vez disso, configure o acesso de saída diretamente para a Internet.
|Não há suporte para FTP Ativo|O FTP Ativo é desabilitado no Firewall do Azure para se proteger contra ataques de retorno de FTP usando o comando FTP PORT.|Em vez disso, você pode usar o FTP Passivo. Você ainda deve abrir explicitamente as portas TCP 20 e 21 no firewall.
|A métrica de utilização da porta SNAT mostra 0%|A métrica de utilização da porta SNAT do Firewall do Azure pode mostrar o uso de 0% mesmo quando as portas SNAT são usadas. Nesse caso, o uso da métrica como parte da métrica de integridade do firewall fornece um resultado incorreto.|Esse problema foi corrigido e a distribuição para produção é destinada a maio de 2020. Em alguns casos, a reimplantação do firewall resolve o problema, mas não é consistente. Como uma solução alternativa intermediária, use apenas o estado de integridade do firewall para procurar *status = degradado*, não para *status = não íntegro*. O esgotamento de porta será exibido como *degradado*. *Não íntegro* é reservado para uso futuro quando mais métricas afetam a integridade do firewall.
|O DNAT não é compatível com o Túnel Forçado habilitado|Os firewalls implantados com o Túnel Forçado habilitado não são compatíveis com acesso de entrada proveniente da Internet devido ao roteamento assimétrico.|Isso ocorre por design devido ao roteamento assimétrico. O caminho de retorno para conexões de entrada passa pelo firewall local, que não viu a conexão estabelecida.
|O FTP Passivo de Saída não funciona para Firewalls com vários endereços IP públicos|O FTP Passivo estabelece conexões diferentes para canais de controle e de dados. Quando um Firewall com vários endereços IP públicos envia dados de saída, ele seleciona aleatoriamente um de seus endereços IP públicos para o endereço IP de origem. O FTP falha quando os canais de controle e de dados usam endereços IP de origem diferentes.|Uma configuração SNAT explícita está em planejamento. Enquanto isso, considere o uso de um só endereço IP nessa situação.|
|A métrica NetworkRuleHit não tem uma dimensão de protocolo|A métrica ApplicationRuleHit permite o protocolo baseado em filtragem, mas essa funcionalidade está ausente na métrica NetworkRuleHit correspondente.|Uma correção está sendo investigada.|
|Não há suporte para regras NAT com portas entre 64000 e 65535|O Firewall do Azure permite qualquer porta no intervalo de 1 a 65535 nas regras de rede e de aplicativo, no entanto, as regras de NAT dão suporte apenas a portas no intervalo de 1 a 63999.|Esta é uma limitação atual.
|As atualizações de configuração podem levar cinco minutos em média|Uma atualização de configuração do Firewall do Azure pode levar de três a cinco minutos em média e não há suporte para atualizações paralelas.|Uma correção está sendo investigada.|
|O Firewall do Azure usa cabeçalhos de TLS SNI para filtrar tráfego HTTPS e MSSQL|Se o software do navegador ou para servidores não for compatível com a extensão SNI (Indicação de Nome do Servidor), você não poderá se conectar por meio do Firewall do Azure.|Se o software do navegador ou para servidores não for compatível com a SNI, você poderá controlar a conexão usando uma regra de rede em vez de uma regra de aplicativo. Consulte [Indicação de Nome de Servidor](https://wikipedia.org/wiki/Server_Name_Indication) para conhecer software que seja compatível com a SNI.

## <a name="next-steps"></a>Próximas etapas

- [Tutorial: Implantar e configurar o Firewall do Azure usando o portal do Azure](tutorial-firewall-deploy-portal.md)
- [Implantar Firewall do Azure usando um modelo](deploy-template.md)
- [Criar um ambiente de teste do Firewall do Azure](scripts/sample-create-firewall-test.md)
