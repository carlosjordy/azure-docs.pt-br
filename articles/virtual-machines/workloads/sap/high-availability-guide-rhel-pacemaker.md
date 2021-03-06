---
title: Configurar o Pacemaker no RHEL no Azure | Microsoft Docs
description: Configurando o Pacemaker no Red Hat Enterprise Linux no Azure
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/24/2020
ms.author: radeltch
ms.openlocfilehash: eed53725507325351dcf51fbe368331c2a4fd2f8
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87065132"
---
# <a name="setting-up-pacemaker-on-red-hat-enterprise-linux-in-azure"></a>Configurando o Pacemaker no Red Hat Enterprise Linux no Azure

[planning-guide]:planning-guide.md
[deployment-guide]:deployment-guide.md
[dbms-guide]:dbms-guide.md
[sap-hana-ha]:sap-hana-high-availability.md
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2009879]:https://launchpad.support.sap.com/#/notes/2009879
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1999351]:https://launchpad.support.sap.com/#/notes/1999351

[virtual-machines-linux-maintenance]:../../maintenance-and-updates.md#maintenance-that-doesnt-require-a-reboot


Primeiro, leia os seguintes documentos e Notas SAP:

* A Nota SAP [1928533], que tem:
  * A lista de tamanhos de VM do Azure que têm suporte para a implantação de software SAP.
  * Informações importantes sobre capacidade para tamanhos de VM do Azure.
  * O software SAP e combinações de SO (sistema operacional) e banco de dados com suporte.
  * A versão do kernel do SAP necessária para Windows e Linux no Microsoft Azure.
* A Nota SAP [2015553] lista pré-requisitos para implantações de software SAP com suporte do SAP no Azure.
* Nota SAP [2002167] recomendou configurações do sistema operacional Red Hat Enterprise Linux
* Nota SAP [2009879] tem diretrizes SAP HANA para Red Hat Enterprise Linux
* A Nota SAP [2178632] contém informações detalhadas sobre todas as métricas de monitoramentos relatadas para o SAP no Azure.
* A Nota SAP [2191498] tem a versão necessária do SAP Host Agent para Linux no Azure.
* A Nota SAP [2243692] tem informações sobre o licenciamento do SAP no Linux no Azure.
* A Nota SAP [1999351] tem informações de solução de problemas adicionais para a Extensão de Monitoramento Avançado do Azure para SAP.
* [WIKI da comunidade do SAP](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) tem todas as Notas SAP necessárias para Linux.
* [Planejamento e implementação de Máquinas Virtuais do Azure para SAP no Linux][planning-guide]
* [Implantação de Máquinas Virtuais do Azure para SAP no Linux (este artigo)][deployment-guide]
* [Implantação de Máquinas Virtuais do Azure do DBMS para SAP no Linux][dbms-guide]
* [Replicação do sistema SAP HANA no cluster de marca-passo](https://access.redhat.com/articles/3004101)
* Documentação geral do RHEL
  * [Visão geral do complemento de alta disponibilidade](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_overview/index)
  * [Administração de complemento de alta disponibilidade](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_administration/index)
  * [Referência de complemento de alta disponibilidade](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/index)
  * [Políticas de suporte para clusters de alta disponibilidade do RHEL – sbd e fence_sbd](https://access.redhat.com/articles/2800691)
* Documentação do RHEL específica do Azure:
  * [Políticas de suporte para clusters de alta disponibilidade do RHEL - máquinas virtuais do Microsoft Azure como membros de cluster](https://access.redhat.com/articles/3131341)
  * [Instalando e configurando um Cluster de alta disponibilidade do Red Hat Enterprise Linux 7.4 (e posterior) no Microsoft Azure](https://access.redhat.com/articles/3252491)
  * [Configurar o SAP S/4HANA ASCS/ERS com o ENSA2 (Servidor de Enfileiramento Autônomo 2) no Pacemaker no RHEL 7.6](https://access.redhat.com/articles/3974941)

## <a name="cluster-installation"></a>Instalação do Cluster

![Pacemaker na visão geral do RHEL](./media/high-availability-guide-rhel-pacemaker/pacemaker-rhel.png)

> [!NOTE]
> A Red Hat não dá suporte ao watchdog emulado por software. A Red Hat não dá suporte a SBD em plataformas de nuvem. Para obter detalhes, confira [Políticas de suporte para clusters de alta disponibilidade do RHEL – sbd e fence_sbd](https://access.redhat.com/articles/2800691).
> O único mecanismo de isolamento com suporte para clusters do Red Hat Enterprise Linux do Pacemaker no Azure é o agente de limite do Azure.  

Os itens a seguir são prefixados com **[A]** – aplicável a todos os nós, **[1]** – aplicável somente ao nó 1 ou **[2]** – aplicável somente ao nó 2.

1. **[A]**  Registrar

   Registre suas máquinas virtuais e anexe-as a um pool que contenha repositórios para o RHEL 7.

   <pre><code>sudo subscription-manager register
   # List the available pools
   sudo subscription-manager list --available --matches '*SAP*'
   sudo subscription-manager attach --pool=&lt;pool id&gt;
   </code></pre>

   Observe que, ao anexar um pool a uma imagem do RHEL PAYG do Azure Marketplace, você será efetivamente cobrado duas vezes pelo uso do RHEL: uma vez para a imagem PAYG e uma vez para o direito do RHEL no pool anexado. Para minimizar isso, o Azure agora fornece imagens do RHEL BYOS. Mais informação estão disponíveis [aqui](../redhat/byos.md).

1. **[A]**  RHEL habilitar para os repositórios do SAP

   Para instalar os pacotes necessários, habilite os seguintes repositórios.

   <pre><code>sudo subscription-manager repos --disable "*"
   sudo subscription-manager repos --enable=rhel-7-server-rpms
   sudo subscription-manager repos --enable=rhel-ha-for-rhel-7-server-rpms
   sudo subscription-manager repos --enable=rhel-sap-for-rhel-7-server-rpms
   sudo subscription-manager repos --enable=rhel-ha-for-rhel-7-server-eus-rpms
   </code></pre>

1. **[A]**  Instalar o complemento de alta disponibilidade do RHEL

   <pre><code>sudo yum install -y pcs pacemaker fence-agents-azure-arm nmap-ncat
   </code></pre>

   > [!IMPORTANT]
   > Recomendamos as seguintes versões do agente do Azure Fence (ou posterior) para que os clientes se beneficiem de um tempo de failover mais rápido se uma interrupção de recurso falhar ou se os nós de cluster não conseguirem se comunicar mais entre si:  
   > RHEL 7.6: fence-agents-4.2.1-11.el7_6.8  
   > RHEL 7.5: fence-agents-4.0.11-86.el7_5.8  
   > RHEL 7.4: fence-agents-4.0.11-66.el7_4.12  
   > Para obter mais informações, confira [A VM do Azure em execução como um membro de cluster de alta disponibilidade do RHEL leva muito tempo para ser protegida ou o isolamento falha/atinge o tempo limite antes que a VM seja desligada](https://access.redhat.com/solutions/3408711).

   Verifique a versão do agente de isolamento do Azure. Se necessário, atualize-a para uma versão igual ou posterior à especificada acima.

   <pre><code># Check the version of the Azure Fence Agent
    sudo yum info fence-agents-azure-arm
   </code></pre>

   > [!IMPORTANT]
   > Se você precisar atualizar o agente do Azure Fence e se estiver usando uma função personalizada, atualize-a para incluir a ação **powerOff**. Para obter detalhes, confira [Criar uma função personalizada para o agente de isolamento](#1-create-a-custom-role-for-the-fence-agent).  

1. **[A]** Configurar a resolução de nome do host

   Você pode usar um servidor DNS ou modificar /etc/hosts em todos os nós. Este exemplo mostra como usar o arquivo /etc/hosts.
   Substitua o endereço IP e o nome do host nos comandos a seguir. A vantagem de usar /etc/hosts é que o cluster se torna independente do DNS, o que também pode ser um ponto único de falhas.

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   Insira as seguintes linhas para /etc/hosts. Altere o endereço IP e o nome do host para corresponder ao seu ambiente

   <pre><code># IP address of the first cluster node
   <b>10.0.0.6 prod-cl1-0</b>
   # IP address of the second cluster node
   <b>10.0.0.7 prod-cl1-1</b>
   </code></pre>

1. **[A]** Alterar a senha hacluster para a mesma senha

   <pre><code>sudo passwd hacluster
   </code></pre>

1. **[A]** Adicione regras de firewall para o marcapasso

   Adicione as seguintes regras de firewall para todas as comunicações de cluster entre os nós do cluster.

   <pre><code>sudo firewall-cmd --add-service=high-availability --permanent
   sudo firewall-cmd --add-service=high-availability
   </code></pre>

1. **[A]** Ativar serviços básicos de cluster

   Execute os seguintes comandos para habilitar o serviço Pacemaker e iniciá-lo.

   <pre><code>sudo systemctl start pcsd.service
   sudo systemctl enable pcsd.service
   </code></pre>

1. **[1]** Criar cluster do marcapasso

   Execute os seguintes comandos para autenticar os nós e criar o cluster. Defina o token como 30000 para permitir a manutenção da memória. Para obter mais informações, confira [este artigo para Linux][virtual-machines-linux-maintenance].

   <pre><code>sudo pcs cluster auth <b>prod-cl1-0</b> <b>prod-cl1-1</b> -u hacluster
   sudo pcs cluster setup --name <b>nw1-azr</b> <b>prod-cl1-0</b> <b>prod-cl1-1</b> --token 30000
   sudo pcs cluster start --all

   # Run the following command until the status of both nodes is online
   sudo pcs status

   # Cluster name: nw1-azr
   # WARNING: no stonith devices and stonith-enabled is not false
   # Stack: corosync
   # Current DC: <b>prod-cl1-1</b> (version 1.1.18-11.el7_5.3-2b07d5c5a9) - partition with quorum
   # Last updated: Fri Aug 17 09:18:24 2018
   # Last change: Fri Aug 17 09:17:46 2018 by hacluster via crmd on <b>prod-cl1-1</b>
   #
   # 2 nodes configured
   # 0 resources configured
   #
   # Online: [ <b>prod-cl1-0</b> <b>prod-cl1-1</b> ]
   #
   # No resources
   #
   #
   # Daemon Status:
   #   corosync: active/disabled
   #   pacemaker: active/disabled
   #   pcsd: active/enabled
   </code></pre>

1. **[A]** Conjunto esperado de votos

   <pre><code>sudo pcs quorum expected-votes 2
   </code></pre>

1. **[1]** permitir ações simultâneas de isolamento

   <pre><code>sudo pcs property set concurrent-fencing=true
   </code></pre>

## <a name="create-stonith-device"></a>Criar dispositivo STONITH

O dispositivo STONITH usa uma Entidade de Serviço para autorização no Microsoft Azure. Siga estas etapas para criar uma Entidade de Serviço.

1. Acesse <https://portal.azure.com>
1. Abra a folha Azure Active Directory  
   Vá para Propriedades e anote a ID do Diretório. Essa é a **ID de locatário**.
1. Clique em Registros do Aplicativo
1. Clique em Novo registro
1. Insira um Nome, selecione "Contas apenas neste diretório de organização" 
2. Selecione um Tipo de Aplicativo "Web", insira uma URL de logon (por exemplo http:\//localhost) e clique em Adicionar  
   A URL de logon não é usada e pode ser qualquer URL válida
1. Selecione Certificados e Segredos e clique em Novo segredo do cliente
1. Insira uma descrição para uma nova chave, selecione "Nunca expira" e clique em Adicionar
1. Anote o Valor. Ele é usado como **senha** da Entidade de Serviço
1. Selecione Visão geral. Anote a ID do Aplicativo. Ela é usada como nome de usuário (**ID de logon** nas etapas abaixo) da Entidade de Serviço

### <a name="1-create-a-custom-role-for-the-fence-agent"></a>**[1]**  Criar uma função personalizada para o agente de isolamento

A Entidade de Serviço não tem permissões para acessar os recursos do Azure por padrão. Você precisa fornecer as permissões da Entidade de Serviço para iniciar e parar (power-off) todas as máquinas virtuais do cluster. Se você ainda não tiver criado a função personalizada, você pode criá-la usando o [PowerShell](../../../role-based-access-control/role-assignments-powershell.md) ou [CLI do Azure](../../../role-based-access-control/role-assignments-cli.md)

Use o seguinte conteúdo para o arquivo de entrada. Você precisa adaptar o conteúdo às suas assinaturas, ou seja, substitua c276fc76-9cd4-44c9-99a7-4fd71546436e e e91d47c4-76f3-4271-a796-21b4ecfe3624 pelas IDs da sua assinatura. Se você tiver apenas uma assinatura, remova a segunda entrada em AssignableScopes.

```json
{
    "properties": {
        "roleName": "Linux Fence Agent Role",
        "description": "Allows to power-off and start virtual machines",
        "assignableScopes": [
            "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
            "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
        ],
        "permissions": [
            {
                "actions": [
                    "Microsoft.Compute/*/read",
                    "Microsoft.Compute/virtualMachines/powerOff/action",
                    "Microsoft.Compute/virtualMachines/start/action"
                ],
                "notActions": [],
                "dataActions": [],
                "notDataActions": []
            }
        ]
    }
}
```

### <a name="a-assign-the-custom-role-to-the-service-principal"></a>**[A]** Atribuir a função personalizada à Entidade de Serviço

Atribua a função personalizada “Função do Agente de Isolamento Linux" que foi criada no último capítulo para a Entidade de Serviço. Não mais use a função de proprietário!

1. Acesse https://portal.azure.com
1. Abra a folha Todos os recursos
1. Selecione a máquina virtual do primeiro nó do cluster
1. Clique em Controle de acesso (IAM)
1. Clique em Adicionar atribuição de função
1. Selecione a função "Função do Agente de Isolamento do Linux”
1. Digite o nome do aplicativo criado acima
1. Clique em Salvar

Repita as etapas acima para o segundo nó do cluster.

### <a name="1-create-the-stonith-devices"></a>**[1]** Criar os dispositivos STONITH

Depois de editar as permissões das máquinas virtuais, você pode configurar os dispositivos STONITH no cluster.

<pre><code>
sudo pcs property set stonith-timeout=900
</code></pre>

Use o seguinte comando para configurar o dispositivo fence.

> [!NOTE]
> A opção 'pcmk_host_map' é SOMENTE requerida no comando, se os nomes de host do RHEL e os nomes de nós do Azure NÃO forem idênticos. Consulte a seção em negrito no comando.

<pre><code>sudo pcs stonith create rsc_st_azure fence_azure_arm login="<b>login ID</b>" passwd="<b>password</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant ID</b>" subscriptionId="<b>subscription id</b>" <b>pcmk_host_map="prod-cl1-0:10.0.0.6;prod-cl1-1:10.0.0.7"</b> \
power_timeout=240 pcmk_reboot_timeout=900 pcmk_monitor_timeout=120 pcmk_monitor_retries=4 pcmk_action_limit=3 \
op monitor interval=3600
</code></pre>

> [!IMPORTANT]
> As operações de monitoramento e isolamento são desserializadas. Como resultado, se houver uma operação de monitoramento em execução mais longa e um evento de isolamento simultâneo, não haverá atraso para o failover de cluster, devido à operação de monitoramento já em execução.  

### <a name="1-enable-the-use-of-a-stonith-device"></a>**[1]** Habilitar o uso de um dispositivo STONITH

<pre><code>sudo pcs property set stonith-enabled=true
</code></pre>

> [!TIP]
>O agente do Azure Fence requer conectividade de saída para pontos de extremidade públicos, conforme documentado, juntamente com soluções possíveis, em [Conectividade de ponto de extremidade pública para VMs usando o ILB padrão](./high-availability-guide-standard-load-balancer-outbound-connections.md).  

## <a name="next-steps"></a>Próximas etapas

* [Planejamento e implementação de Máquinas Virtuais do Azure para o SAP][planning-guide]
* [Implantação de Máquinas Virtuais do Azure para SAP][deployment-guide]
* [Implantação do DBMS de Máquinas Virtuais do Azure para SAP][dbms-guide]
* Para saber como estabelecer a alta disponibilidade e o plano de recuperação de desastre do SAP HANA em VMs do Azure, confira [Alta disponibilidade do SAP HANA em VMs (Máquinas Virtuais) do Azure][sap-hana-ha]
