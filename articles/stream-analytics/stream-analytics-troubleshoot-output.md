---
title: Solucionar problemas de saídas do Azure Stream Analytics
description: Este artigo descreve técnicas para solucionar problemas de conexões de entrada em trabalhos do Azure Stream Analytics.
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: troubleshooting
ms.date: 03/31/2020
ms.custom: seodec18
ms.openlocfilehash: fc35e6a723afab3f230aa91e4b6895aead35e141
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86037062"
---
# <a name="troubleshoot-azure-stream-analytics-outputs"></a>Solucionar problemas de saídas do Azure Stream Analytics

Este artigo descreve problemas comuns com conexões de saída do Azure Stream Analytics e como solucionar problemas de saída. Muitas etapas de solução de problemas exigem que os logs de recursos e outros logs de diagnóstico sejam habilitados para seu trabalho do Stream Analytics. Se você não tiver os logs de recursos habilitados, confira [Solucionar problemas do Azure Stream Analytics usando os logs de recursos](stream-analytics-job-diagnostic-logs.md).

## <a name="the-job-doesnt-produce-output"></a>O trabalho não produz a saída

1. Verifique a conectividade com as saídas usando o botão **Testar Conexão** para cada uma das saídas.
1. Dê uma olhada em [Métricas de monitoramento](stream-analytics-monitoring.md), na guia **Monitor**. Como os valores são agregados, as métricas são atrasadas em alguns minutos.

   * Se o valor de **Eventos de Entrada** é maior que zero, o trabalho pode ler os dados de entrada. Se o valor de **Eventos de Entrada** não é maior que zero, há um problema com a entrada do trabalho. Confira [Solucionar problemas de conexões de entrada](stream-analytics-troubleshoot-input.md) para obter mais informações.
   * Se o valor de **Erros de conversão de dados** for maior que zero e crescente, confira [Erros de dados do Azure Stream Analytics](data-errors.md) para obter informações detalhadas sobre erros de conversão de dados.
   * Se o valor de **Erros de tempo de execução** é maior que zero, seu trabalho recebe dados, mas gera erros durante o processamento da consulta. Para encontrar os erros, acesse os [logs de auditoria](../azure-resource-manager/management/view-activity-logs.md) e filtre pelo status **Failed** (Com falha).
   * Se o valor de **Eventos de Entrada** é maior que zero e o valor de **Eventos de Saída** é igual a zero, uma das seguintes instruções é verdadeira:
      * O processamento da consulta resultou em zero evento de saída.
      * Os eventos ou seus campos podem estar malformados, resultando em nenhuma saída após o processamento da consulta.
      * O trabalho não pôde enviar dados por push para o coletor de saída por motivos de conectividade ou autenticação.

   As mensagens do log de operações explicam os detalhes adicionais, incluindo o que está acontecendo, exceto nos casos em que a lógica da consulta filtra todos os eventos. Se o processamento de vários eventos gera erros, os erros são agregados a cada 10 minutos.

## <a name="the-first-output-is-delayed"></a>A primeira saída está atrasada

Quando um trabalho do Stream Analytics é iniciado, os eventos de entrada são lidos. Mas, pode haver um atraso na saída, em determinadas circunstâncias.

Valores de hora grande em elementos de consulta temporal podem contribuir para o atraso de saída. Para produzir a saída correta em janelas de tempo grandes, o trabalho de streaming lê dados do último tempo possível para preencher a janela de tempo. Os dados podem ter de até sete dias atrás. A saída é produzida somente depois que os eventos de entrada pendentes são lidos. Esse problema pode surgir quando o sistema atualiza os trabalhos de streaming. Quando ocorre uma atualização, o trabalho é reiniciado. Essas atualizações geralmente ocorrem uma vez a cada dois meses.

Atenção ao criar sua consulta do Stream Analytics. Se você usar uma janela de tempo grande para elementos temporais na sintaxe da consulta do trabalho, isso poderá causar um atraso na primeira saída quando o trabalho iniciar ou reiniciar. De várias horas até sete dias é considerada uma janela de tempo grande.

Uma maneira de atenuar esse tipo de atraso da primeira saída é usar técnicas de paralelização, como particionamento dos dados. Você também pode adicionar mais unidades de streaming para melhorar a taxa de transferência até que o trabalho esteja atualizado.  Para obter mais informações, confira [Considerações ao criar trabalhos do Stream Analytics](stream-analytics-concepts-checkpoint-replay.md).

Esses fatores afetam a linha do tempo da primeira saída:

* O uso de agregações em janela, como a cláusula GROUP BY de janelas em cascata, de salto e deslizantes:

  * Para agregações de janela em cascata ou de salto, os resultados são gerados no final do período de tempo da janela.
  * Para uma janela deslizante, os resultados são gerados quando um evento entra ou sai da janela deslizante.
  * Se você estiver planejando usar um tamanho de janela grande, ou seja, superior a uma hora, é melhor escolher uma janela de salto ou deslizante. Esses tipos de janela permitem ver a saída com mais frequência.

* O uso das junções temporais, como JOIN com DATEDIFF:
  * As correspondências serão geradas na chegada de ambos os lados dos eventos correspondentes.
  * Dados que não têm uma correspondência, como LEFT OUTER JOIN (junção externa esquerda), são gerados no final da janela DATEDIFF, para cada evento no lado esquerdo.

* O uso de funções analíticas temporais, como ISFIRST, LAST e LAG com LIMIT DURATION:
  * Para funções analíticas, a saída é gerada por evento. Não há atraso.

## <a name="the-output-falls-behind"></a>A saída fica atrás

Durante a operação normal de um trabalho, a saída pode ter períodos de latência mais longos. Se a saída ficar por trás dessa forma, você poderá identificar as causas raiz examinando os seguintes fatores:

* Se o coletor de downstream está limitado
* Se o coletor de upstream está limitado
* Se a lógica de processamento da consulta está com uso intensivo de computação

Para ver os detalhes da saída, selecione o trabalho de streaming no Portal do Azure e escolha **Diagrama de trabalho**. Para cada entrada, há uma métrica de evento de lista de pendências por partição. Se a métrica continuar aumentando, é um indicador de que os recursos do sistema estão restritos. O aumento possivelmente é causado pela limitação do coletor de saída ou pelo uso elevado da CPU. Para obter mais informações, confira [Depuração orientada a dados usando o diagrama de trabalho](stream-analytics-job-diagram-with-metrics.md).

## <a name="key-violation-warning-with-azure-sql-database-output"></a>Aviso de violação de chave com a saída do Banco de Dados SQL do Azure

Quando você configura um banco de dados SQL do Azure como saída para um trabalho do Stream Analytics, ele faz a inserção em massa de registros na tabela de destino. Em geral, o Azure Stream Analytics garante [entrega pelo menos uma vez ](https://docs.microsoft.com/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics) ao coletor de saída. Você ainda poderá [obter entrega exatamente uma vez]( https://blogs.msdn.microsoft.com/streamanalytics/2017/01/13/how-to-achieve-exactly-once-delivery-for-sql-output/) para uma saída de SQL quando uma tabela SQL tiver uma restrição exclusiva definida.

Quando você configura restrições de chave exclusivas na tabela SQL, o Azure Stream Analytics remove registros duplicados. Ele divide os dados em lotes e insere recursivamente os lotes até que um único registro duplicado seja encontrado. O processo de divisão e inserção ignora os registros duplicados, um de cada vez. Para um trabalho de streaming com muitas linhas duplicadas, o processo é ineficiente e demorado. Se visualizar várias mensagens de aviso de violação da chave no log de atividades da hora anterior, é provável que a saída do SQL esteja retardando o trabalho inteiro.

Para resolver esse problema, [configure o índice]( https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql) que está causando a violação da chave, habilitando a opção IGNORE_DUP_KEY. Essa opção permite que o SQL ignore valores duplicados durante inserções em massa. O Banco de Dados SQL do Azure simplesmente produz uma mensagem de aviso, em vez de um erro. Como resultado, o Azure Stream Analytics não produz mais erros de violação de chave primária.

Observe as observações a seguir ao configurar IGNORE_DUP_KEY para vários tipos de índices:

* Não é possível definir IGNORE_DUP_KEY em uma chave primária ou uma restrição exclusiva que utilize ALTER INDEX. Você precisa descartar o índice e recriá-lo.  
* Você pode definir IGNORE_DUP_KEY usando ALTER INDEX para um índice exclusivo. Essa instância é diferente de uma restrição PRIMARY KEY/UNIQUE e é criada usando uma definição CREATE INDEX ou INDEX.  
* A opção IGNORE_DUP_KEY não se aplica a índices de repositório de coluna porque não é possível impor a exclusividade a eles.  

## <a name="column-names-are-lowercase-in-azure-stream-analytics-10"></a>Os nomes de coluna estão em letras minúsculas no Azure Stream Analytics (1.0)

Ao usar o nível de compatibilidade original (1.0), o Azure Stream Analytics altera os nomes de coluna para letras minúsculas. Esse comportamento foi corrigido nos níveis de compatibilidade posteriores. Para preservar o tipo de letra, acesse o nível de compatibilidade 1.1 ou posterior. Para obter mais informações, confira [Nível de compatibilidade para trabalhos do Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-compatibility-level).

## <a name="get-help"></a>Obter ajuda

Para obter mais assistência, confira nossa [página de Perguntas e respostas do Microsoft do Azure Stream Analytics](https://docs.microsoft.com/answers/topics/azure-stream-analytics.html).

## <a name="next-steps"></a>Próximas etapas

* [Introdução ao Stream Analytics do Azure](stream-analytics-introduction.md)
* [Introdução ao uso do Stream Analytics do Azure](stream-analytics-real-time-fraud-detection.md)
* [Dimensionar trabalhos do Stream Analytics do Azure](stream-analytics-scale-jobs.md)
* [Referência de linguagem de consulta do Azure Stream Analytics](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Referência da API REST do gerenciamento do Stream Analytics do Azure](https://msdn.microsoft.com/library/azure/dn835031.aspx)
