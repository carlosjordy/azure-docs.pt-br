---
title: Criar gráficos e diagramas a partir de consultas de log do Azure Monitor | Microsoft Docs
description: Descreve várias visualizações no Azure Monitor para exibir seus dados de log de maneiras diferentes.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/16/2018
ms.openlocfilehash: 8a515f01bfa9f8ec579c51b806c997d79b629250
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77670314"
---
# <a name="creating-charts-and-diagrams-from-azure-monitor-log-queries"></a>Criar gráficos e diagramas a partir de consultas de log do Azure Monitor

> [!NOTE]
> Você deve concluir as [Agregações avançadas nas consultas de log do Azure Monitor](advanced-aggregations.md) antes de concluir esta lição.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Este artigo descreve várias visualizações no Azure Monitor para exibir seus dados de log de maneiras diferentes.

## <a name="charting-the-results"></a>Colocando os resultados em gráficos
Comece revisando quantos computadores há por sistema operacional durante a última hora:

```Kusto
Heartbeat
| where TimeGenerated > ago(1h)
| summarize count(Computer) by OSType  
```

Por padrão, os resultados são exibidos como uma tabela:

![Tabela](media/charts/table-display.png)

Para obter uma visão melhor, selecione **Chart** e escolha a opção **Pie** para visualizar os resultados:

![Gráfico de pizza](media/charts/charts-and-diagrams-pie.png)


## <a name="timecharts"></a>Timecharts
Mostre a média, o percentil 50 e 95 do tempo do processador em intervalos de 1 hora. A consulta gera várias séries e você pode selecionar quais séries serão exibidas no gráfico de tempo:

```Kusto
Perf
| where TimeGenerated > ago(1d) 
| where CounterName == "% Processor Time" 
| summarize avg(CounterValue), percentiles(CounterValue, 50, 95)  by bin(TimeGenerated, 1h)
```

Selecione a opção de exibição de gráfico de **linhas** :

![Gráfico de Linhas](media/charts/charts-and-diagrams-multiSeries.png)

### <a name="reference-line"></a>Linha de referência

Uma linha de referência pode ajudar a identificar facilmente se a métrica excedeu um limite específico. Para adicionar uma linha a um gráfico, estenda o conjunto de dados com uma coluna constante:

```Kusto
Perf
| where TimeGenerated > ago(1d) 
| where CounterName == "% Processor Time" 
| summarize avg(CounterValue), percentiles(CounterValue, 50, 95)  by bin(TimeGenerated, 1h)
| extend Threshold = 20
```

![Linha de referência](media/charts/charts-and-diagrams-multiSeriesThreshold.png)

## <a name="multiple-dimensions"></a>Várias dimensões
Várias expressões `by` na cláusula`summarize` de criam várias linhas nos resultados, uma para cada combinação de valores.

```Kusto
SecurityEvent
| where TimeGenerated > ago(1d)
| summarize count() by tostring(EventID), AccountType, bin(TimeGenerated, 1h)
```

Quando você exibe os resultados como um gráfico, ele usa a primeira coluna da cláusula `by`. O exemplo a seguir mostra um gráfico de colunas empilhadas usando o _EventID._ Dimensões devem ser do `string` de tipo, isso neste exemplo o _EventID_ está sendo convertido em cadeia de caracteres. 

![Gráfico de barras EventID](media/charts/charts-and-diagrams-multiDimension1.png)

Você pode alternar entre selecionando o menu suspenso com o nome da coluna. 

![Gráfico de barras AccountType](media/charts/charts-and-diagrams-multiDimension2.png)

## <a name="next-steps"></a>Próximas etapas
Consulte outras lições para usar a [linguagem de consulta Kusto](/azure/kusto/query/) com os dados de log do Azure Monitor:

- [Operações de cadeia de caracteres](string-operations.md)
- [Operações de data e hora](datetime-operations.md)
- [Funções de agregação](aggregations.md)
- [Agregações avançadas](advanced-aggregations.md)
- [JSON e estruturas de dados](json-data-structures.md)
- [Gravação de consulta avançada](advanced-query-writing.md)
- [Junções](joins.md)
