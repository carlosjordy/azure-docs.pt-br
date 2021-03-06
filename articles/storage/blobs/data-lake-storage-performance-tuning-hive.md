---
title: 'Ajustar o desempenho: Hive, HDInsight & Azure Data Lake Storage Gen2 | Microsoft Docs'
description: Azure Data Lake Storage Gen2 diretrizes de ajuste de desempenho do hive.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: how-to
ms.date: 11/18/2019
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 522f9215a0b66c5e6bec5abf41e45489efec19ac
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86106304"
---
# <a name="tune-performance-hive-hdinsight--azure-data-lake-storage-gen2"></a>Ajustar o desempenho: Hive, & HDInsight Azure Data Lake Storage Gen2

As configurações padrão foram definidas para fornecer bom desempenho em muitos casos de uso diferentes.  Para consultas intensivas de E/S, o Hive pode ser ajustado para obter um melhor desempenho com o Azure Data Lake Storage Gen2.  

## <a name="prerequisites"></a>Pré-requisitos

* **Uma assinatura do Azure**. Consulte [Obter a avaliação gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Uma conta do Data Lake Storage Gen2**. Para obter instruções sobre como criar uma, consulte [início rápido: criar uma conta de armazenamento Azure data Lake Storage Gen2](data-lake-storage-quickstart-create-account.md)
* **Cluster do Azure HDInsight** com acesso a uma conta do Azure Data Lake Storage Gen2. Confira [usar o Azure data Lake Storage Gen2 com clusters do Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2)
* **Execução do Hive no HDInsight**.  Para saber mais sobre como executar trabalhos de Hive no HDInsight, confira [Usar Hive no HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-hive)
* **Diretrizes de ajuste de desempenho no Data Lake Storage Gen2**.  Para obter conceitos gerais de desempenho, consulte [diretrizes de ajuste de desempenho data Lake Storage Gen2](data-lake-storage-performance-tuning-guidance.md)

## <a name="parameters"></a>Parâmetros

Aqui estão as configurações mais importantes para melhorar o desempenho do Data Lake Storage Gen2:

* **hive.tez.container.size** – a quantidade de memória usada por cada uma das tarefas

* **tez.grouping.min-size** – tamanho mínimo de cada mapeador

* **tez.grouping.max-size** – tamanho máximo de cada mapeador

* **hive.exec.reducer.bytes.per.reducer** – tamanho de cada redutor

**hive.tez.container.size** — o tamanho do contêiner determina quanto de memória está disponível para cada tarefa.  Essa é a entrada principal para controlar a simultaneidade no Hive.  

**tez.grouping.min-size** – esse parâmetro permite definir o tamanho mínimo de cada mapeador.  Se o número de mapeadores que o Tez escolher for menor que o valor desse parâmetro, o Tez usará o valor definido aqui.

**tez.grouping.max-size** – o parâmetro permite definir o tamanho máximo de cada mapeador.  Se o número de mapeadores que o Tez escolher for maior que o valor desse parâmetro, o Tez usará o valor definido aqui.

**hive.exec.reducer.bytes.per.reducer** – esse parâmetro define o tamanho de cada redutor.  Por padrão, o tamanho de cada redutor é de 256 MB.  

## <a name="guidance"></a>Diretrizes

**Definir hive.exec.reducer.bytes.per.reducer** – o valor padrão funciona bem quando os dados são descompactados.  Para dados compactados, você deve reduzir o tamanho do redutor.  

**Definir hive.tez.container.size** – em cada nó, a memória é especificada por yarn.nodemanager.resource.memory-mb e deve ser corretamente definida no cluster HDI por padrão.  Para obter informações adicionais sobre como definir a memória adequada no YARN, confira esta [postagem](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).

As cargas de trabalho que usam muita E/S podem se beneficiar de mais paralelismo reduzindo o tamanho do contêiner do Tez. Isso proporciona ao usuário mais contêineres, o que aumenta a simultaneidade.  No entanto, algumas consultas do Hive exigem uma quantidade significativa de memória (por exemplo, MapJoin).  Se a tarefa não tiver memória suficiente, você receberá uma exceção de falta de memória durante o runtime.  Se você receber sem exceções de falta de memória, será preciso aumentá-la.   

O número simultâneo de tarefas em execução ou de paralelismo será limitado pela memória YARN total.  O número de contêineres YARN ditará quantas tarefas simultâneas podem ser executadas.  Para localizar a memória YARN por nó, você pode ir para o Ambari.  Navegue até YARN e exiba a guia Configurações.  A memória YARN é exibida nesta janela.  

- Memória total YARN = Nodes * YARN de memória por nó
- \#de contêineres YARN = tamanho total do contêiner YARN de memória/tez

A chave para melhorar o desempenho usando o Data Lake Storage Gen2 é aumentar a simultaneidade o máximo possível.  O Tez calcula automaticamente o número de tarefas que devem ser criadas, de modo que você não precisa defini-lo.   

## <a name="example-calculation"></a>Exemplo de cálculo

Digamos que você tenha um cluster D14 de 8 nós.  

- Memória total YARN = Nodes * YARN de memória por nó
- Memória total do YARN = 8 nós * 96 GB = 768GB
- \#de contêineres YARN = 768GB/3072MB = 256

## <a name="further-information-on-hive-tuning"></a>Mais informações sobre o ajuste do Hive

Veja a seguir alguns blogs que ajudarão a ajustar as consultas do Hive:
* [Otimizar consultas do Hive para Hadoop no HDInsight](https://azure.microsoft.com/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [Solução de problemas de desempenho em consultas do Hive](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)
* [Debate sobre como otimizar o Hive no HDInsight](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)
