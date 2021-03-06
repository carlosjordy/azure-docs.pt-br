---
title: Configurações de armazenamento de máquina virtual do SAP HANA no Azure | Microsoft Docs
description: Recomendações de armazenamento para VMs que têm o SAP HANA implantado nelas.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/30/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c1e0efc2c64a1cbdcc2c83c019f7743406054afe
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87074022"
---
# <a name="sap-hana-azure-virtual-machine-storage-configurations"></a>Configurações de armazenamento de máquina virtual do SAP HANA no Azure

O Azure fornece diferentes tipos de armazenamento principais adequados a máquinas virtuais do Azure que executam o SAP HANA. Os **tipos de armazenamento do Azure certificados pelo SAP HANA** que podem ser considerados para implantações do SAP HANA são listados como: 

- Armazenamento Premium ou SSD do Azure Premium 
- [Disco Ultra](../../linux/disks-enable-ultra-ssd.md)
- [Azure NetApp Files](https://azure.microsoft.com/services/netapp/) 

Para saber mais sobre esses tipos de disco, confira o artigo [tipos de armazenamento do Azure para carga de trabalho do SAP](./planning-guide-storage.md) e [Selecione um tipo de disco](../../linux/disks-types.md)

O Azure oferece dois métodos de implantação para VHDs no armazenamento Standard e Premium do Azure. Esperamos que você aproveite o [disco gerenciado do Azure](https://azure.microsoft.com/services/managed-disks/) para implantações de armazenamento em bloco do Azure. 

Para obter uma lista de tipos de armazenamento e os respectivos SLAs em IOPS e taxa de transferência de armazenamento, revise a [documentação do Azure para discos gerenciado](https://azure.microsoft.com/pricing/details/managed-disks/).

> [!IMPORTANT]
> Independentemente do tipo de armazenamento do Azure escolhido, o sistema de arquivos usado nesse armazenamento precisa ter suporte do SAP para o sistema operacional específico e o DBMS. [Nota de suporte nº 405827 do SAP](https://launchpad.support.sap.com/#/notes/405827) lista os sistemas de arquivos com suporte para diferentes sistemas operacionais e bancos de dados, incluindo o SAP HANA. Isso se aplica a todos os volumes que o SAP HANA pode acessar para leitura e gravação de qualquer tarefa. Especificamente usando o NFS no Azure para o SAP HANA, restrições adicionais de versões do NFS se aplicam conforme mencionado mais adiante neste artigo 


As condições mínimas certificadas pelo SAP HANA para os diferentes tipos de armazenamento são: 

- O armazenamento Premium do Azure- **/Hana/log** precisa ter suporte do Azure [acelerador de gravação](../../linux/how-to-enable-write-accelerator.md). O volume **/Hana/data** pode ser colocado no armazenamento Premium sem o Azure acelerador de gravação ou no ultra Disk
- Ultra Disk do Azure pelo menos para o volume **/Hana/log** . O volume **/Hana/data** pode ser colocado em um armazenamento Premium sem o Azure acelerador de gravação ou para obter tempos de reinicialização mais rápidos
- Volumes **NFS v 4.1** sobre Azure NetApp Files para **/Hana/log e/Hana/data**. O volume de/Hana/Shared pode usar o protocolo NFS v3 ou NFS v 4.1

Alguns tipos de armazenamento podem ser combinados. Por exemplo, é possível colocar **/Hana/data** no armazenamento Premium e **/Hana/log** pode ser colocado em armazenamento de ultra disco para obter a baixa latência necessária. Se você usar um volume baseado em seja para **/Hana/data**, o **/Hana/log** volume também precisará ser baseado em NFS na parte superior do seja. **Não há suporte**para o uso de NFS sobre seja para um dos volumes (como/Hana/Data) e armazenamento Premium do Azure ou ultra Disk para o outro volume (como **/Hana/log**).

No mundo local, você raramente precisava preocupar-se com os subsistemas de E/S e as funcionalidades. O motivo era que o fornecedor do dispositivo precisava certificar-se de que os requisitos mínimos de armazenamento fossem atendidos para o SAP HANA. Ao criar a infraestrutura do Azure por conta própria, você deve estar ciente de alguns desses requisitos emitidos pelo SAP. Algumas das características mínimas de taxa de transferência que o SAP recomenda, são:

- Leitura/gravação em **/Hana/log** de 250 MB/s com tamanhos de e/s de 1 MB
- Leia a atividade de pelo menos 400 MB/s para **/Hana/data** para os tamanhos de e/s de 16 mb e 64 MB
- Atividade de gravação de pelo menos 250 MB/s para **/Hana/data** com tamanhos de e/s de 16 mb e 64 MB

Uma vez que a baixa latência de armazenamento é crítica aos sistemas DBMS, mesmo que o DBMS, como SAP HANA, mantenha os dados na memória. O caminho crítico no armazenamento é geralmente em torno das gravações de log de transações dos sistemas DBMS. Mas também operações como gravar pontos de salvamento ou carregar dados na memória após a recuperação de falhas poderão ser críticas. Portanto, é **obrigatório** aproveitar o armazenamento Premium do Azure, ultra Disk ou seja para volumes **/Hana/data** e **/Hana/log** . 


Alguns princípios de orientação na seleção da configuração de armazenamento para HANA podem ser listados como:

- Decida sobre o tipo de armazenamento com base nos [tipos de armazenamento do Azure para carga de trabalho SAP](./planning-guide-storage.md) e [Selecione um tipo de disco](../../linux/disks-types.md)
- A taxa de transferência de e/s de VM geral e os limites de IOPS em mente ao dimensionar ou decidir uma VM. A taxa de transferência geral do armazenamento de VM está documentada no artigo [tamanhos de máquina virtual com otimização de memória](../../sizes-memory.md)
- Ao decidir pela configuração de armazenamento, tente ficar abaixo da taxa de transferência geral da VM com a configuração do volume **/Hana/data** . Escrevendo pontos de salvamento, SAP HANA pode ser a emissão agressiva de e/SS. É fácil enviar até limites de taxa de transferência do volume **/Hana/data** ao escrever um salvamento de pontos. Se os discos que criam o volume **/Hana/data** tiverem uma taxa de transferência maior do que a sua VM permitir, você poderá se deparar com situações em que a taxa de transferência utilizada pela gravação do ponto de salvamento está interferindo com as demandas de taxa de transferência das gravações de log de restauração. Uma situação que pode afetar a taxa de transferência do aplicativo
- Se você estiver usando o armazenamento Premium do Azure, a configuração menos dispendiosa será usar gerenciadores de volume lógico para criar conjuntos de distribuição para criar os volumes **/Hana/data** e **/Hana/log**

> [!IMPORTANT]
> As sugestões para as configurações de armazenamento são destinadas como instruções para começar. Executando a carga de trabalho e analisando os padrões de utilização de armazenamento, você pode perceber que não está utilizando toda a largura de banda de armazenamento ou IOPS fornecidos. Em seguida, você pode considerar o downsizing no armazenamento. Ou, em contrário, sua carga de trabalho pode precisar de mais taxa de transferência de armazenamento do que o sugerido com essas configurações. Como resultado, talvez seja necessário implantar mais capacidade, IOPS ou taxa de transferência. No campo de tensão entre a capacidade de armazenamento necessária, a latência de armazenamento necessária, a taxa de transferência de armazenamento e o IOPS necessários e a configuração menos dispendiosa, o Azure oferece tipos de armazenamento diferentes suficientes com diferentes recursos e pontos de preço diferentes para localizar e ajustar para o comprometimento correto para você e sua carga de trabalho do HANA.

## <a name="linux-io-scheduler-mode"></a>Modo Agendador de E/S do Linux
O Linux possui vários modos de agendamento de E/S diferentes. A recomendação comum por meio de fornecedores do Linux e do SAP é reconfigurar o modo de agendador de E/S para volumes de disco do modo **mq-deadline** ou **kyber** para o modo **noop** (não multifila) ou **none** (multifila). Detalhes são referenciados na [Nota SAP Nº 1984787](https://launchpad.support.sap.com/#/notes/1984787). 


## <a name="solutions-with-premium-storage-and-azure-write-accelerator-for-azure-m-series-virtual-machines"></a>Soluções com armazenamento Premium e Acelerador de Gravação do Azure para máquinas virtuais da série M do Azure
O Acelerador de Gravação do Azure é uma funcionalidade disponível exclusivamente para VMs da Série M do Azure. Como o nome indica, a finalidade da funcionalidade é melhorar a latência de e/s de gravações no armazenamento Premium do Azure. Para o SAP HANA, acelerador de gravação deve ser usado em relação a **hana/log** somente no volume. Portanto, **/hana/data** e **/gana/log** são volumes separados com o Acelerador de Gravação do Azure que dão suporte apenas ao volume **/hana/log**. 

> [!IMPORTANT]
> Ao usar o armazenamento Premium do Azure, o uso do Azure [acelerador de gravação](../../linux/how-to-enable-write-accelerator.md) para o volume **/Hana/log** é obrigatório. Acelerador de Gravação está disponível apenas para VMs de armazenamento Premium e série M e série Mv2. Acelerador de Gravação não está funcionando em combinação com outras famílias de VM do Azure, como Esv3 ou Edsv4.

As recomendações de cache para os discos Premium do Azure abaixo estão supondo as características de e/s para SAP HANA essa lista, como:

- Praticamente, não há nenhuma carga de trabalho de leitura nos arquivos de dados do HANA. As exceções são E / Ss de tamanho grande após o reinício da instância do HANA ou quando os dados são carregados no HANA. Outro caso de E/Ss de leitura maiores em arquivos de dados podem ser backups de banco de dados do HANA. Como resultado, em grande parte, o cache de leitura não faz sentido pois, na maioria dos casos, todos os volumes de arquivos de dados precisam ser lidos na íntegra. 
- A gravação nos arquivos de dados está ocorrendo em intermitências com base nos pontos de salvamento e na recuperação de pane do HANA. Os pontos de salvamento de gravação são assíncronos e não impedem nenhuma transação de usuário. A gravação de dados durante a recuperação de pane é crítica para o desempenho para que o sistema responda com rapidez novamente. No entanto, a recuperação de pane devem ser situações excepcionais
- Praticamente, não há nenhuma leitura dos arquivos de refazer do HANA. As exceções são E/Ss grandes ao executar backups de log de transações, recuperação de pane ou na fase de reinicialização de uma instância do HANA.  
- A carga principal no arquivo de log de refazer do SAP HANA são gravações. Dependendo da natureza da carga de trabalho, você pode ter E/Ss pequenas de 4 KB ou, em outros casos, tamanhos de E/S iguais ou superiores a 1 MB. A latência de gravação no log de refazer do SAP HANA é crítica para o desempenho.
- Todas as gravações precisam ser persistidas em disco de forma confiável

**Recomendação: como resultado desses padrões de e/s observados por SAP HANA, o cache para os diferentes volumes usando o armazenamento Premium do Azure deve ser definido como:**

- **/Hana/data** -sem cache ou cache de leitura
- **/hana/log** – sem cache – exceção para a série M e Mv2 em que Acelerador de Gravação deve ser habilitado sem cache de leitura. 
- **hana/shared** - ler o cache
- **Disco do sistema operacional** -não altere o cache padrão definido pelo Azure no momento da criação da VM


Se você estiver usando LVM ou mdadm para criar conjuntos de distribuição em vários discos Premium do Azure, será necessário definir tamanhos de distribuição. Esses tamanhos diferem entre **/Hana/data** e **/Hana/log**. **Recomendação: como tamanhos de distribuição, a recomendação é usar:**

- 256 KB para **/hana/data**
- 64 KB para **/Hana/log**

> [!NOTE]
> O tamanho de distribuição para **/Hana/data** foi alterado de recomendações anteriores chamando 64 kb ou 128 kb para 256 KB com base nas experiências do cliente com versões mais recentes do Linux. O tamanho de 256 KB fornece um desempenho ligeiramente melhor. Também alteramos a recomendação para os tamanhos de distribuição de **/Hana/log** de 32 kb a 64 KB para obter uma taxa de transferência suficiente com tamanhos maiores de e/s.

> [!NOTE]
> Você não precisa configurar nenhum nível de redundância usando volumes RAID, pois o armazenamento de blocos do Azure mantém três imagens de um VHD. O uso de um conjunto de distribuição com discos Premium do Azure é puramente para configurar volumes que fornecem IOPS e/ou taxa de transferência de e/s suficiente.

A acumulação de vários VHDs do Azure sob um conjunto de distribuição é acumulada de um lado de taxa de transferência de IOPS e armazenamento. Portanto, se você colocar um conjunto de distribuição em mais de 3 x p30 discos de armazenamento Premium do Azure, ele deverá fornecer três vezes o IOPS e três vezes a taxa de transferência de armazenamento de um único disco p30 de armazenamento Premium do Azure.

> [!IMPORTANT]
> Caso você esteja usando LVM ou mdadm como Gerenciador de volumes para criar conjuntos de distribuição em vários discos Premium do Azure, os três SAP HANA sistemas de/data,/log e/Shared não devem ser colocados em um grupo de volumes padrão ou raiz. É altamente recomendável seguir as diretrizes de fornecedores do Linux, que costumam criar grupos de volumes individuais para /data, /log e /shared.


### <a name="azure-burst-functionality-for-premium-storage"></a>Funcionalidade de intermitência do Azure para armazenamento Premium
Para discos de armazenamento Premium do Azure menores ou iguais a 512 GiB em capacidade, a funcionalidade de intermitência é oferecida. A maneira exata de como funciona a intermitência de disco é descrita no artigo [intermitência de disco](../../linux/disk-bursting.md). Ao ler o artigo, você entende o conceito de acumular IOPS e taxa de transferência nos momentos em que a carga de trabalho de e/s está abaixo do IOPS nominal e da taxa de transferência dos discos (para obter detalhes sobre a taxa de transferência nominal, consulte [preço do disco gerenciado](https://azure.microsoft.com/pricing/details/managed-disks/)). Você vai acumular o Delta de IOPS e a taxa de transferência entre o uso atual e os valores nominais do disco. As intermitências são limitadas a um máximo de 30 minutos.

Os casos ideais em que essa funcionalidade de intermitência pode ser planejada provavelmente serão os volumes ou discos que contêm arquivos de dados para o DBMS diferente. A carga de trabalho de e/s esperada nesses volumes, especialmente com sistemas de pequeno a médio porte, deve parecer com a seguinte aparência:

- Carga de trabalho de leitura baixa a moderada, pois os dados são idealmente armazenados em cache na memória, ou como no caso do HANA devem estar completamente na memória
- Picos de gravação disparadas por pontos de verificação de banco de dados ou ponto de salvamento emitidos regularmente
- Carga de trabalho de backup que lê em um fluxo contínuo em casos em que os backups não são executados por meio de instantâneos de armazenamento
- Por SAP HANA, carregar os dados na memória após uma reinicialização da instância

Especialmente em sistemas DBMS menores em que sua carga de trabalho está lidando com apenas algumas centenas de transações por segundo, uma funcionalidade de intermitência também pode fazer sentido para os discos ou volumes que armazenam a transação ou efetuar o log de refazer. A carga de trabalho esperada em um disco ou em volumes é semelhante a:

- Gravações regulares no disco que são dependentes da carga de trabalho e a natureza da carga de trabalho, pois cada confirmação emitida pelo aplicativo provavelmente disparam uma operação de e/s
- Maior carga de trabalho na taxa de transferência para casos de tarefas operacionais, como criar ou recriar índices
- Ler intermitências ao executar o log de transações ou fazer backups de log de restauração


### <a name="production-recommended-storage-solution-based-on-azure-premium-storage"></a>Solução de armazenamento recomendada para produção com base no armazenamento Premium do Azure

> [!IMPORTANT]
> Certificação do SAP HANA para máquinas virtuais da série M do Azure é exclusivamente com o Acelerador de gravação do Azure para o **hana/log** volume. Como resultado, as implantações de SAP HANA de cenário de produção em máquinas virtuais da série M do Azure devem ser configurados com o Acelerador de gravação do Azure para o **hana/log** volume.  

> [!NOTE]
> Em cenários que envolvem o armazenamento Premium do Azure, estamos implementando recursos de intermitência na configuração. Como você está usando as ferramentas de teste de armazenamento de qualquer forma ou formulário, mantenha o modo como o [Azure Premium Disk Burst](../../linux/disk-bursting.md) funciona em mente. Executando os testes de armazenamento fornecidos por meio da ferramenta SAP HWCCT ou HCMT, não estamos esperando que todos os testes passem os critérios, já que alguns dos testes excederão os créditos de intermitência que você pode acumular. Especialmente quando todos os testes são executados sequencialmente sem interrupção.


> [!NOTE]
> Para cenários de produção, verifique se um determinado tipo de VM tem suporte pelo SAP HANA pelo SAP na [documentação do SAP para IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

**Recomendação: as configurações recomendadas com o armazenamento Premium do Azure para cenários de produção são semelhantes a:**

Configuração do volume do SAP **/Hana/data** :

| SKU da VM | RAM | Máx. VM E/S<br /> Produtividade | /hana/data | Taxa de transferência máxima de intermitência | IOPS | IOPS de intermitência |
| --- | --- | --- | --- | --- | --- | --- | 
| M32ts | 192 GiB | 500 MBps | 4 x P6 | 680 MBps | 960 | 14.000 |
| M32ls | 256 GiB | 500 MBps | 4 x P6 | 680 MBps | 960 | 14.000 |
| M64ls | 512 GiB | 1\.000 MBps | 4 x P10 |  680 MBps | 2\.000 | 14.000 |
| M64s | 1\.000 GiB | 1\.000 MBps | 4 x P15 | 680 MBps | 4.400 | 14.000 |
| M64ms | 1\.750 GiB | 1\.000 MBps | 4 x P20 | 680 MBps | 9.200 | 14.000 |  
| M128s | 2\.000 GiB | 2.000 MBps | 4 x P20 | 680 MBps | 9.200| 14.000 | 
| M128ms | 3\.800 GiB | 2.000 MBps | 4 x P30 | 800 MBps (provisionado) | 20.000 | sem intermitência | 
| M208s_v2 | 2\.850 GiB | 1\.000 MBps | 4 x P30 | 800 MBps (provisionado) | 20.000| sem intermitência | 
| M208ms_v2 | 5\.700 GiB | 1\.000 MBps | 4 x P40 | 1.000 MBps (provisionado) | 25.000 | sem intermitência |
| M416s_v2 | 5\.700 GiB | 2.000 MBps | 4 x P40 | 1.000 MBps (provisionado) | 25.000 | sem intermitência |
| M416ms_v2 | 11.400 GiB | 2.000 MBps | 4 x P50 | 2.000 MBps (provisionado) | 25.000 | sem intermitência |


Para o volume **/Hana/log** . a configuração teria a seguinte aparência:

| SKU da VM | RAM | Máx. VM E/S<br /> Produtividade | volume **/Hana/log** | Taxa de transferência máxima de intermitência | IOPS | IOPS de intermitência |
| --- | --- | --- | --- | --- | --- | --- | 
| M32ts | 192 GiB | 500 MBps | 3 x P10 | 510 MBps | 1\.500 | 10.500 | 
| M32ls | 256 GiB | 500 MBps | 3 x P10 | 510 MBps | 1\.500 | 10.500 | 
| M64ls | 512 GiB | 1\.000 MBps | 3 x P10 | 510 MBps | 1\.500 | 10.500 | 
| M64s | 1\.000 GiB | 1\.000 MBps | 3 x P15 | 510 MBps | 3.300 | 10.500 | 
| M64ms | 1\.750 GiB | 1\.000 MBps | 3 x P15 | 510 MBps | 3.300 | 10.500 |  
| M128s | 2\.000 GiB | 2.000 MBps | 3 x P15 | 510 MBps | 3.300 | 10.500|  
| M128ms | 3\.800 GiB | 2.000 MBps | 3 x P15 | 510 MBps | 3.300 | 10.500 | 
| M208s_v2 | 2\.850 GiB | 1\.000 MBps | 3 x P15 | 510 MBps | 3.300 | 10.500 |  
| M208ms_v2 | 5\.700 GiB | 1\.000 MBps | 3 x P15 | 510 MBps | 3.300 | 10.500 |  
| M416s_v2 | 5\.700 GiB | 2.000 MBps | 3 x P15 | 510 MBps | 3.300 | 10.500 |  
| M416ms_v2 | 11.400 GiB | 2.000 MBps | 3 x P15 | 510 MBps | 3.300 | 10.500 | 


Para os outros volumes, a configuração teria a seguinte aparência:

| SKU da VM | RAM | Máx. VM E/S<br /> Produtividade | /hana/shared | /root volume | /usr/sap |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 GiB | 500 MBps | 1 x P20 | 1 x P6 | 1 x P6 |
| M32ls | 256 GiB | 500 MBps |  1 x P20 | 1 x P6 | 1 x P6 |
| M64ls | 512 GiB | 1000 MBps | 1 x P20 | 1 x P6 | 1 x P6 |
| M64s | 1\.000 GiB | 1\.000 MBps | 1 x P30 | 1 x P6 | 1 x P6 |
| M64ms | 1\.750 GiB | 1\.000 MBps | 1 x P30 | 1 x P6 | 1 x P6 | 
| M128s | 2\.000 GiB | 2.000 MBps | 1 x P30 | 1 x P10 | 1 x P6 | 
| M128ms | 3\.800 GiB | 2.000 MBps | 1 x P30 | 1 x P10 | 1 x P6 |
| M208s_v2 | 2\.850 GiB | 1\.000 MBps |  1 x P30 | 1 x P10 | 1 x P6 |
| M208ms_v2 | 5\.700 GiB | 1\.000 MBps | 1 x P30 | 1 x P10 | 1 x P6 | 
| M416s_v2 | 5\.700 GiB | 2.000 MBps |  1 x P30 | 1 x P10 | 1 x P6 | 
| M416ms_v2 | 11.400 GiB | 2.000 MBps | 1 x P30 | 1 x P10 | 1 x P6 | 


Verifique se a taxa de transferência de armazenamento para os diferentes volumes recomendados atende à carga de trabalho que você quer executar. Se a carga de trabalho exigir volumes maiores para **/Hana/data** e **/Hana/log**, você precisará aumentar o número de VHDs de armazenamento Premium do Azure. Dimensionar um volume com mais VHDs do que o listado aumenta a taxa de transferência de E/S e IOPS dentro dos limites do tipo de máquina virtual do Azure.

O Acelerador de Gravação do Azure funciona somente em conjunto com o [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/). Portanto, pelo menos os discos de armazenamento Premium do Azure que formam o volume **/Hana/log** precisam ser implantados como discos gerenciados. Instruções e restrições mais detalhadas do Azure Acelerador de Gravação podem ser encontradas no artigo [acelerador de gravação](../../linux/how-to-enable-write-accelerator.md).

Para as VMs certificadas do HANA da família Azure [Esv3](../../ev3-esv3-series.md?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json#esv3-series) e do [Edsv4](../../edv4-edsv4-series.md?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json#edsv4-series), você precisará seja para o volume **/Hana/data** e **/Hana/log** . Ou você precisa aproveitar o armazenamento do Azure ultra Disk em vez do armazenamento Premium do Azure somente para o volume **/Hana/log** . Como resultado, as configurações para o volume **/Hana/data** no armazenamento Premium do Azure poderiam ser semelhantes a:

| SKU da VM | RAM | Máx. VM E/S<br /> Produtividade | /hana/data | Taxa de transferência máxima de intermitência | IOPS | IOPS de intermitência |
| --- | --- | --- | --- | --- | --- | --- |
| E20ds_v4 | 160 GiB | 480 MBps | 3 x P10 | 510 MBps | 1\.500 | 10.500 |
| E32ds_v4 | 256 GiB | 768 MBps | 3 x P10 |  510 MBps | 1\.500 | 10.500|
| E48ds_v4 | 384 GiB | 1.152 MBps | 3 x P15 |  510 MBps | 3.300  | 10.500 | 
| E64ds_v4 | 504 GiB | 1\.200 MBps | 3 x P15 |  510 MBps | 3.300 | 10.500 | 
| E64s_v3 | 432 GiB | 1\.200 MB/s | 3 x P15 |  510 MBps | 3.300 | 10.500 | 

Para os outros volumes, incluindo **/Hana/log** no ultra Disk, a configuração poderia ser semelhante a:

| SKU da VM | RAM | Máx. VM E/S<br /> Produtividade | Volume /hana/log | Taxa de transferência de E/S /hana/log | IOPS /hana/log | /hana/shared | /root volume | /usr/sap |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| E20ds_v4 | 160 GiB | 480 MBps | 80 GB | 250 MBps | 1.800 | 1 x P15 | 1 x P6 | 1 x P6 |
| E32ds_v4 | 256 GiB | 768 MBps | 128 GB | 250 MBps | 1.800 | 1 x P15 | 1 x P6 | 1 x P6 |
| E48ds_v4 | 384 GiB | 1.152 MBps | 192 GB | 250 MBps | 1.800 | 1 x P20 | 1 x P6 | 1 x P6 |
| E64ds_v4 | 504 GiB | 1\.200 MBps | 256 GB | 250 MBps | 1.800 | 1 x P20 | 1 x P6 | 1 x P6 |
| E64s_v3 | 432 GiB | 1\.200 MBps | 220 GB | 250 MBps | 1.800 | 1 x P20 | 1 x P6 | 1 x P6 |


## <a name="azure-ultra-disk-storage-configuration-for-sap-hana"></a>Configuração de armazenamento de disco Ultra do Azure para SAP HANA
Outro tipo de armazenamento do Azure é chamado de [ultra Disk do Azure](../../windows/disks-types.md#ultra-disk). A diferença significativa entre o armazenamento do Azure oferecido até o momento e o disco Ultra é que as funcionalidades de disco não estão mais vinculadas ao tamanho do disco. Como um cliente, você pode definir esses recursos para o disco Ultra:

- Tamanho de um disco que varia de 4 GiB a 65.536 GiB
- O IOPS varia de 100 IOPS a 160 mil IOPS (o máximo também depende dos tipos de VM)
- Taxa de transferência de armazenamento de 300 MB/s a 2.000 MB/s

O disco Ultra oferece a possibilidade de definir um único disco que atenda a seu tamanho, ao IOPS e ao intervalo de taxa de transferência de disco. Em vez de usar gerenciadores de volume lógico como LVM ou MDADM sobre o armazenamento Premium do Azure para construir volumes que atendam aos requisitos de taxa de transferência de armazenamento e IOPS. Você pode executar uma combinação de configuração entre o ultra Disk e o armazenamento Premium. Como resultado, você pode limitar o uso de ultra Disk para os volumes críticos de desempenho **/Hana/data** e **/Hana/log** e abranger os outros volumes com o armazenamento Premium do Azure

Outras vantagens do ultra Disk podem ser a melhor latência de leitura em comparação com o armazenamento Premium. A latência de leitura mais rápida pode ter vantagens quando você deseja reduzir os tempos de inicialização do HANA e a carga subsequente dos dados na memória. As vantagens do armazenamento de disco Ultra também podem ser sentidas quando o HANA está escrevendo pontos de salvamento. 

> [!NOTE]
> O disco Ultra ainda não está presente em todas as regiões do Azure e também ainda não está dando suporte a todos os tipos de VM listados abaixo. Para obter informações detalhadas sobre onde o disco Ultra está disponível para quais famílias de VM há suporte, consulte o artigo [Quais tipos de disco estão disponíveis no Azure?](../../windows/disks-types.md#ultra-disk).

### <a name="production-recommended-storage-solution-with-pure-ultra-disk-configuration"></a>Solução de armazenamento recomendada para produção com configuração de disco Ultra pura
Nessa configuração, você mantém os volumes **/Hana/data** e **/Hana/log** separadamente. Os valores sugeridos são derivados dos KPIs que o SAP precisa certificar para tipos de VM para SAP HANA e as configurações de armazenamento, conforme recomendado no [Whitepaper de armazenamento do SAP TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html).

As recomendações geralmente estão excedendo os requisitos mínimos do SAP, conforme mencionado anteriormente neste artigo. As recomendações listadas são um comprometimento entre as recomendações de tamanho do SAP e a taxa de transferência de armazenamento máxima que os diferentes tipos de VM fornecem.

| SKU da VM | RAM | Máx. VM E/S<br /> Produtividade | Volume /hana/data | Taxa de transferência de E/S /hana/data | IOPS /hana/data | Volume /hana/log | Taxa de transferência de E/S /hana/log | IOPS /hana/log |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| E20ds_v4 | 160 GiB | 480 MB/s | 200 GB | 400 MBps | 2\.500 | 80 GB | 250 MB | 1.800 |
| E32ds_v4 | 256 GiB | 768 MB/s | 300 GB | 400 MBps | 2\.500 | 128 GB | 250 MBps | 1.800 |
| E48ds_v4 | 384 GiB | 1152 MB/s | 460 GB | 400 MBps | 3\.000 | 192 GB | 250 MBps | 1.800 |
| E64ds_v4 | 504 GiB | 1200 MB/s | 610 GB | 400 MBps | 3\.500 |  256 GB | 250 MBps | 1.800 |
| E64s_v3 | 432 GiB | 1\.200 MB/s | 610 GB | 400 MBps | 3\.500 | 220 GB | 250 MB | 1.800 |
| M32ts | 192 GiB | 500 MB/s | 250 GB | 400 MBps | 2\.500 | 96 GB | 250 MBps  | 1.800 |
| M32ls | 256 GiB | 500 MB/s | 300 GB | 400 MBps | 2\.500 | 256 GB | 250 MBps  | 1.800 |
| M64ls | 512 GiB | 1\.000 MB/s | 620 GB | 400 MBps | 3\.500 | 256 GB | 250 MBps  | 1.800 |
| M64s | 1\.000 GiB | 1\.000 MB/s |  1\.200 GB | 600 MBps | 5\.000 | 512 GB | 250 MBps  | 2\.500 |
| M64ms | 1\.750 GiB | 1\.000 MB/s | 2\.100 GB | 600 MBps | 5\.000 | 512 GB | 250 MBps  | 2\.500 |
| M128s | 2\.000 GiB | 2\.000 MB/s |2\.400 GB | 750 MBps | 7.000 | 512 GB | 250 MBps  | 2\.500 | 
| M128ms | 3\.800 GiB | 2\.000 MB/s | 4\.800 GB | 750 MBps |7.000 | 512 GB | 250 MBps  | 2\.500 | 
| M208s_v2 | 2\.850 GiB | 1\.000 MB/s | 3\.500 GB | 750 MBps | 7.000 | 512 GB | 250 MBps  | 2\.500 | 
| M208ms_v2 | 5\.700 GiB | 1\.000 MB/s | 7\.200 GB | 750 MBps | 7.000 | 512 GB | 250 MBps  | 2\.500 | 
| M416s_v2 | 5\.700 GiB | 2\.000 MB/s | 7\.200 GB | 1\.000 MBps | 9\.000 | 512 GB | 400 MBps  | 4.000 | 
| M416ms_v2 | 11.400 GiB | 2\.000 MB/s | 14.400 GB | 1\.500 MBps | 9\.000 | 512 GB | 400 MBps  | 4.000 |   

**Os valores listados se destinam a ser um ponto de partida e precisam ser avaliados em relação às demandas reais.** A vantagem com o Disco Ultra do Azure é que os valores de IOPS e taxa de transferência podem ser adaptados sem a necessidade de desligar a VM ou de interromper a carga de trabalho aplicada ao sistema.   

> [!NOTE]
> Até agora, os instantâneos de armazenamento com o armazenamento de disco Ultra não estão disponíveis. Isso bloqueia o uso de instantâneos de VM com os serviços de Backup do Azure


## <a name="nfs-v41-volumes-on-azure-netapp-files"></a>Volumes NFS v4.1 em Azure NetApp Files
O Azure NetApp Files fornece compartilhamentos NFS nativos que podem ser usados para volumes **/Hana/Shared**, **/Hana/data**e **/Hana/log** . O uso de compartilhamentos NFS baseados em seja para os volumes **/Hana/data** e **/Hana/log** requer o uso do protocolo v 4.1 NFS. Não há suporte para o protocolo de NFS v3 para o uso de volumes **/Hana/data** e **/Hana/log** ao basear os compartilhamentos em seja. 

> [!IMPORTANT]
> O protocolo NFS v3 implementado em Azure NetApp Files **não** tem suporte para ser usado para **/Hana/data** e **/Hana/log**. O uso do NFS 4,1 é obrigatório para volumes **/Hana/data** e **/Hana/log** de um ponto de vista funcional. Enquanto para o volume **/Hana/Shared** , o NFS v3 ou o protocolo NFS v 4.1 podem ser usados a partir de um ponto de vista funcional.

### <a name="important-considerations"></a>Considerações importantes
Ao considerar Azure NetApp Files para o SAP Netweaver e o SAP HANA, esteja ciente das seguintes considerações importantes:

- O pool de capacidade mínima é de 4 TiB.  
- O tamanho mínimo do volume é 100 GiB
- O Azure NetApp Files e todas as máquinas virtuais, em que os volumes do Azure NetApp Files serão montados, devem estar na mesma Rede Virtual do Azure ou em [redes virtuais emparelhadas](../../../virtual-network/virtual-network-peering-overview.md) na mesma região.  
- A rede virtual selecionada deve ter uma sub-rede, delegada ao Azure NetApp Files.
- A taxa de transferência de um volume do Azure NetApp é uma função da cota de volume e do nível de serviço, conforme documentado no [Nível de serviço para Azure NetApp Files](../../../azure-netapp-files/azure-netapp-files-service-levels.md). Ao dimensionar os volumes Azure NetApp do HANA, verifique se a taxa de transferência resultante atende aos requisitos do sistema do HANA.  
- Azure NetApp Files oferece a [política de exportação](../../../azure-netapp-files/azure-netapp-files-configure-export-policy.md): você pode controlar os clientes permitidos e o tipo de acesso (leitura e gravação, somente leitura etc.). 
- O recurso do Azure NetApp Files ainda não tem reconhecimento de zona. No momento, o recurso do Azure NetApp Files não está implantado em todas as zonas de disponibilidade em uma região do Azure. Esteja atendo às possíveis implicações de latência em algumas regiões do Azure.  
- É importante que as máquinas virtuais sejam implantadas bem próximas ao armazenamento do Azure NetApp para ter baixa latência. 
- A ID de Usuário para <b>sid</b>adm e a ID de Grupo para `sapsys` nas máquinas virtuais devem corresponder com a configuração no Azure NetApp Files. 

> [!IMPORTANT]
> Para cargas de trabalho do SAP HANA, baixa latência é fundamental. Trabalhe com seu representante da Microsoft para verificar se as máquinas virtuais e os volumes do Azure NetApp Files foram implantados bem próximos.  

> [!IMPORTANT]
> Se houver uma incompatibilidade entre a ID de usuário para <b>sid</b>adm e a ID de Grupo para `sapsys` entre a máquina virtual e a configuração do Azure NetApp, as permissões dos arquivos em volumes do Azure NetApp, montadas em máquinas virtuais, serão exibidas como `nobody`. Especifique a ID de Usuário correta para <b>sid</b>adm e a ID do Grupo para `sapsys`, ao [integrar um novo sistema](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxjSlHBUxkJBjmARn57skvdUQlJaV0ZBOE1PUkhOVk40WjZZQVJXRzI2RC4u) ao Azure NetApp Files.

### <a name="sizing-for-hana-database-on-azure-netapp-files"></a>Dimensionar o banco de dados do HANA no Azure NetApp Files

A taxa de transferência de um volume do Azure NetApp é uma função do tamanho de volume e do nível de serviço, conforme documentado no [Nível de serviço para Azure NetApp Files](../../../azure-netapp-files/azure-netapp-files-service-levels.md). 

Ao criar a infraestrutura para o SAP no Azure, você deve estar ciente de alguns requisitos de taxa de transferência mínima de armazenamento do SAP, que são convertidos em características de taxa de transferência mínima de:

- Habilitar a leitura/gravação em **/hana/log** de 250 MB/s com tamanhos de E/S de 1 MB  
- Habilite a atividade de leitura de pelo menos 400MB/seg para **/hana/data** para tamanhos de E / S de 16MB e 64MB  
- Habilite a atividade gravação de pelo menos 250 MB/seg para **/hana/data** para tamanhos de E / S de 16MB e 64MB  

Os [limites de taxa de transferência do Azure NetApp Files](../../../azure-netapp-files/azure-netapp-files-service-levels.md) por 1 TiB de cota de volume são:
- Camada de armazenamento Premium-64 MiB/s  
- Camada de Armazenamento Ultra – 128 MiB/s  

> [!IMPORTANT]
> Independentemente da capacidade que você implanta em um único volume NFS, espera-se que a taxa de transferência fique estável no intervalo de largura de banda de 1,2 a 1,4 GB/s utilizada por um consumidor em uma máquina virtual. Isso tem a ver com a arquitetura subjacente da oferta do ANF e com os limites de sessão do Linux relacionados em relação ao NFS. Os números de desempenho e taxa de transferência, conforme documentado no artigo [Resultados do teste de benchmark de desempenho para Azure NetApp Files](../../../azure-netapp-files/performance-benchmarks-linux.md) foram realizados em um volume NFS compartilhado com várias VMs de cliente e como resultado com várias sessões. Esse cenário é diferente para o cenário que medimos no SAP. Nele, medimos a taxa de transferência de uma única VM com relação a um volume NFS. hospedado no ANF.

Para atender aos requisitos de taxa de transferência mínima do SAP para dados e log e, de acordo com as diretrizes de `/hana/shared`, os tamanhos recomendados seriam semelhantes a:

| Volume | Tamanho<br /> Camada de armazenamento Premium | Tamanho<br /> Camada de armazenamento Ultra | Protocolo do NFS com suporte |
| --- | --- | --- |
| /hana/log/ | 4 TiB | 2 TiB | v4.1 |
| /hana/data | 6,3 TiB | 3,2 TiB | v4.1 |
| /hana/shared | Max(512 GB, 1xRAM) por 4 nós de trabalho | Max(512 GB, 1xRAM) por 4 nós de trabalho | v3 ou v4.1 |


> [!NOTE]
> As recomendações de dimensionamento do Azure NetApp Files declaradas aqui estão visando os requisitos mínimos que o SAP expressa para seus provedores de infraestrutura. Em implantações de clientes e cenários de carga de trabalho reais, isso pode não ser suficiente. Use essas recomendações como um ponto de partida e adapte-se, com base nos requisitos de sua carga de trabalho específica.  

Portanto, você pode considerar implantar uma taxa de transferência semelhante para os volumes do ANF conforme já listado para o armazenamento de disco Ultra. Além disso, considere os tamanhos listados para os volumes dos diferentes SKUs de VM, como já foi feito nas tabelas do disco Ultra.

> [!TIP]
> Você pode redimensionar os volumes do Azure NetApp Files dinamicamente, sem a necessidade de `unmount` os volumes, parar as máquinas virtuais ou parar o SAP HANA. Isso oferece flexibilidade para você atender às demandas de taxa de transferência tanto esperadas quanto imprevistas do seu aplicativo.

A documentação sobre como implantar uma configuração de expansão do SAP HANA com o nó em espera usando volumes NFS v4.1 hospedados no ANF foi publicada na [Expansão do SAP HANA com o nó em espera em VMs do Azure com o Azure NetApp Files no SUSE Linux Enterprise Server](./sap-hana-scale-out-standby-netapp-files-suse.md).


## <a name="next-steps"></a>Próximas etapas
Para obter mais informações, consulte:

- [Guia de alta disponibilidade do SAP HANA para máquinas virtuais do Azure](./sap-hana-availability-overview.md).
