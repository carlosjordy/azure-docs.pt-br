---
title: API do Processador do feed de alterações do .NET do Azure Cosmos DB, notas de versão do SDK
description: Saiba tudo sobre o SDK e a API do processador de feed de alterações, incluindo as datas de lançamento, as datas de desativação e as alterações feitas entre cada versão do SDK do processador de feed de alterações do .NET.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/11/2020
ms.author: anfeldma
ms.openlocfilehash: 679f3113cddbfe13370483f2678154f4dd1f8ab2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85392056"
---
# <a name="net-change-feed-processor-sdk-download-and-release-notes"></a>SDK do Processador do Feed de Alterações do .NET: Downloads e notas sobre a versão

> [!div class="op_single_selector"]
>
> * [SDK v3 do .NET](sql-api-sdk-dotnet-standard.md)
> * [SDK do .NET v2](sql-api-sdk-dotnet.md)
> * [SDK v2 do .NET Core](sql-api-sdk-dotnet-core.md)
> * [SDK v2 do feed de alterações do .NET](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [SDK do Java v4](sql-api-sdk-java-v4.md)
> * [SDK do Java Assíncrono v2](sql-api-sdk-async-java.md)
> * [SDK do Java Síncrono v2](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * Deixa (/rest/api
> * [Provedor de recursos REST] (/rest/api
> * [SQL](sql-api-query-reference.md)
> * [Executor em massa-.NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [Executor em massa – Java](sql-api-sdk-bulk-executor-java.md)

|   |   |
|---|---|
|**Baixe o SDK**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)|
|**Documentação da API**|[Alterar a documentação de referência de API da biblioteca do Processador de Feed](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)|
|**Introdução**|[Introdução ao SDK .NET do processador do feed de alterações](change-feed.md)|
|**Framework atualmente com suporte**| [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</br> [Microsoft .NET Core](https://www.microsoft.com/net/download/core) |

> [!NOTE]
> Se você estiver usando o processador do feed de alterações, consulte a versão mais recente 3.x do [SDK .NET](change-feed-processor.md), que tem o feed de alterações interno do SDK. 

## <a name="release-notes"></a>Notas de versão

### <a name="v2-builds"></a>v2 builds

### <a name="230"></a><a name="2.3.0"></a>2.3.0
* Adição de um novo método `ChangeFeedProcessorBuilder.WithCheckpointPartitionProcessorFactory` e da interface pública `ICheckpointPartitionProcessorFactory` correspondente. Isso permite que uma implementação da interface de `IPartitionProcessor` use o mecanismo de ponto de verificação interno. O novo alocador é semelhante ao atual `IPartitionProcessorFactory`, exceto que seu método `Create` também usa o parâmetro `ILeaseCheckpointer`.
* Somente um dos dois métodos, `ChangeFeedProcessorBuilder.WithPartitionProcessorFactory` ou `ChangeFeedProcessorBuilder.WithCheckpointPartitionProcessorFactory`, pode ser usado para a mesma instância de `ChangeFeedProcessorBuilder`.

### <a name="228"></a><a name="2.2.8"></a>2.2.8
* Aprimoramentos de estabilidade e diagnóstico:
  * Adição de suporte para detectar a leitura do feed de alterações que demora muito. Quando demora mais do que o valor especificado pela propriedade `ChangeFeedProcessorOptions.ChangeFeedTimeout`, as etapas a seguir são executadas:
    * A operação para ler o feed de alterações na partição problemática foi anulada.
    * A instância do processador do feed de alterações elimina a propriedade da concessão problemática. A concessão descartada será coletada durante a próxima etapa de aquisição de concessão, que será feita pela mesma instância do processador do feed de alterações ou por uma diferente. Dessa forma, a leitura do feed de alterações será reiniciada.
    * Um problema é relatado no monitor de integridade. O monitor de integridade padrão envia todos os problemas relatados para o log de rastreamento.
  * Adição de uma nova propriedade pública: `ChangeFeedProcessorOptions.ChangeFeedTimeout`. O valor padrão dessa propriedade é de 10 minutos.
  * Adição de um novo valor enum: `Monitoring.MonitoredOperation.ReadChangeFeed`. Quando o valor de `HealthMonitoringRecord.Operation` é definido como `Monitoring.MonitoredOperation.ReadChangeFeed`, isso indica que o problema de integridade está relacionado à leitura do feed de alterações.

### <a name="227"></a><a name="2.2.7"></a>2.2.7
* Melhoria na estratégia de balanceamento de carga para o cenário quando obter todas as concessões leva mais tempo do que o intervalo de expiração de concessão, por exemplo, devido a problemas de rede:
  * Nesse cenário, o algoritmo de balanceamento de carga usado para considerar de forma falsa as concessões expiradas, causando o roubo de concessões de proprietários ativos. Isso poderia acionar um novo balanceamento desnecessário de muitas concessões.
  * Esse problema foi corrigido nesta versão, evitando a repetição em conflito adquirindo a concessão expirada cujo proprietário não foi alterado e adiando a aquisição da concessão expirada para a próxima iteração de balanceamento de carga.

### <a name="226"></a><a name="2.2.6"></a>2.2.6
* Melhoria no tratamento de exceções de Observador.
* Informações mais detalhadas sobre erros do Observador:
  * Quando um Observador for fechado devido a uma exceção gerada por ProcessChangesAsync do Observador, o CloseAsync receberá o parâmetro de motivo definido como ChangeFeedObserverCloseReason.ObserverError.
  * Rastreamentos adicionados para identificar erros no código do usuário em um Observador.

### <a name="225"></a><a name="2.2.5"></a>2.2.5
* Suporte adicionado para manipular a divisão em coleções que usam taxa de transferência de banco de dados compartilhado.
  * Esta versão corrige um problema que pode ocorrer durante a divisão em coleções que usam a taxa de transferência de banco de dados compartilhado, quando a divisão resulta no rebalanceamento da partição com somente um intervalo de chave de partição filho em vez de duas. Quando isso acontece, o Processador do Feed de Alterações poderá ficar preso ao excluir a concessão para o intervalo de chave de partição antigo e não criar novas concessões. Esse problema foi corrigido nesta versão.

### <a name="224"></a><a name="2.2.4"></a>2.2.4
* Adicionada nova propriedade ChangeFeedProcessorOptions.StartContinuation para dar suporte ao feed de alterações do token de continuação da solicitação. Isso é usado apenas quando a coleção de concessão estiver vazia ou uma concessão não tiver ContinuationToken definido. Para concessões na coleção de concessão que têm o ContinuationToken definido, o ContinuationToken é usado e ChangeFeedProcessorOptions.StartContinuation será ignorado.

### <a name="223"></a><a name="2.2.3"></a>2.2.3
* Adicionado suporte para o uso de armazenamento personalizado para manter os tokens de continuação por partição.
  * Por exemplo, um armazenamento personalizado de concessão pode ser a coleção de concessão do Azure Cosmos DB particionada de qualquer forma personalizada.
  * Os armazenamentos de concessão personalizados podem usar o novo ChangeFeedProcessorBuilder.WithLeaseStoreManager(ILeaseStoreManager) de ponto de extensibilidade e interface pública ILeaseStoreManager.
  * Refatorada a interface ILeaseManager em várias interfaces de função.
* Pequena alteração significativa: ponto de extensibilidade removido ChangeFeedProcessorBuilder.WithLeaseManager(ILeaseManager), use ChangeFeedProcessorBuilder.WithLeaseStoreManager(ILeaseStoreManager)no lugar.

### <a name="222"></a><a name="2.2.2"></a>2.2.2
* Esta versão corrige um problema que ocorre durante o processamento de uma divisão na coleção monitorada e usa uma coleção de concessão particionada. Ao processar uma concessão de partição de divisão, a concessão correspondente a essa partição não pode ser excluída. Esse problema foi corrigido nesta versão.

### <a name="221"></a><a name="2.2.1"></a>2.2.1
* Correção de cálculo do Estimador para contas Multimestre e novo formato do Token de Sessão.

### <a name="220"></a><a name="2.2.0"></a>2.2.0
* Adicionado suporte a coleções particionadas de concessão. A chave de partição deve ser definida como /id.
* Pequena alteração interruptiva: os métodos da interface IChangeFeedDocumentClient e a classe ChangeFeedDocumentClient foram alterados para incluir parâmetros de RequestOptions e CancellationToken. IChangeFeedDocumentClient é um ponto de extensibilidade avançada que permite que você forneça uma implementação personalizada do Cliente de Documento a ser usado com o Processador do Feed de Alteração, por exemplo, decorar DocumentClient e interceptar todas as chamadas para ele para fazer rastreamento extra, tratamento de erros etc. Com essa atualização, o código que implementa IChangeFeedDocumentClient precisará ser alterado para incluir novos parâmetros na implementação.
* Pequenos aprimoramentos de diagnóstico.

### <a name="210"></a><a name="2.1.0"></a>2.1.0
* Adicionada nova API, Task &lt;IReadOnlyList&lt; RemainingPartitionWork&gt;&gt; IRemainingWorkEstimator.GetEstimatedRemainingWorkPerPartitionAsync (). Isso pode ser usado para obter trabalho estimado para cada partição.
* Dá suporte a Microsoft.Azure.DocumentDB SDK 2.0. Requer Microsoft.Azure.DocumentDB 2.0 ou posterior.

### <a name="206"></a><a name="2.0.6"></a>2.0.6
* Adicionada a propriedade pública ChangeFeedEventHost.HostName para compatibilidade com v1.

### <a name="205"></a><a name="2.0.5"></a>2.0.5
* Correção de uma condição de corrida que ocorre durante a divisão de partição. A condição de corrida pode levar a aquisição de concessão e imediatamente perdê-la durante a divisão de partição e causando a contenção. O problema de condição de corrida é corrigido com esta versão.

### <a name="204"></a><a name="2.0.4"></a>2.0.4
* SDK DO GA

### <a name="203-prerelease"></a><a name="2.0.3-prerelease"></a>2.0.3-prerelease
* Os seguintes problemas foram corrigidos:
  * Quando ocorre a divisão de partição, pode ser processamento duplicado de documentos modificados antes da divisão.
  * A API GetEstimatedRemainingWork retornou 0 quando nenhuma concessão estava presente na coleção de concessão.

* As seguintes exceções são tornadas públicas. Extensões que implementam IPartitionProcessor podem gerar essas exceções.
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.LeaseLostException. 
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionException. 
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionNotFoundException.
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionSplitException. 

### <a name="202-prerelease"></a><a name="2.0.2-prerelease"></a>2.0.2 - pré-lançamento
* Pequenas alterações na API:
  * Removido ChangeFeedProcessorOptions.IsAutoCheckpointEnabled que foi marcado como obsoleto.

### <a name="201-prerelease"></a><a name="2.0.1-prerelease"></a>2.0.1-pré-lançamento
* Aprimoramentos de estabilidade:
  * Melhor tratamento da inicialização do armazenamento de concessão. Quando o armazenamento de concessão estiver vazio, apenas uma instância do processador poderá inicializá-lo, os outros aguardarão.
  * Lançamento/renovação da concessão mais estável/eficiente. Renovar e liberar uma concessão de uma partição independe de outras renovações. Na v1 isso foi feito sequencialmente para todas as partições.
* Nova API v2:
  * Padrão de construtor para construção flexível do processador: a classe ChangeFeedProcessorBuilder.
    * Pode ser qualquer combinação de parâmetros.
    * Pode usar a instância de DocumentClient para monitoramento e/ou coleta de concessão (não disponível em v1).
  * IChangeFeedObserver.ProcessChangesAsync agora utiliza CancellationToken.
  * IRemainingWorkEstimator - o avaliador de trabalho restante pode ser usado separadamente do processador.
  * Novos pontos de extensibilidade:
    * IPartitionLoadBalancingStrategy - para balanceamento de carga personalizado de partições entre instâncias do processador.
    * ILease, ILeaseManager - para gerenciamento de concessão personalizado.
    * IPartitionProcessor - para processamento personalizado de alterações em uma partição.
* Registro em log - usa a biblioteca [LibLog](https://github.com/damianh/LibLog).
* 100% compatível com versões anteriores com API v1.
* Nova base de código.
* Compatível com o [SQL .NET SDK](sql-api-sdk-dotnet.md) versões 1.21.1 e superiores.

### <a name="v1-builds"></a>v1 builds

### <a name="133"></a><a name="1.3.3"></a>1.3.3
* Adição de mais registros em log.
* Correção de um vazamento de DocumentClient ao chamar a estimativa de trabalho pendente várias vezes.

### <a name="132"></a><a name="1.3.2"></a>1.3.2
* Correções na estimativa de trabalho pendente.

### <a name="131"></a><a name="1.3.1"></a>1.3.1
* Aprimoramentos de estabilidade.
  * Correção na manipulação de problemas de tarefas canceladas que podem levar a observadores parados em algumas partições.
* Suporte para o ponto de verificação manual.
* Compatível com o [SDK do .NET para SQL](sql-api-sdk-dotnet.md) versões 1.21 e superiores.

### <a name="120"></a><a name="1.2.0"></a>1.2.0
* Adiciona suporte para o .NET Standard 2.0. O pacote agora dá suporte a monikers de estrutura `netstandard2.0` e `net451`.
* Compatível com o [SDK do .NET para SQL](sql-api-sdk-dotnet.md) versões 1.17.0 e superiores.
* Compatível com o [SDK do .NET Core para SQL](sql-api-sdk-dotnet-core.md) versões 1.5.1 e superiores.

### <a name="111"></a><a name="1.1.1"></a>1.1.1
* Corrige um problema com o cálculo da estimativa de trabalho restante quando o feed de alterações estava vazio ou nenhum trabalho estava pendente.
* Compatível com o [SDK do .NET para SQL](sql-api-sdk-dotnet.md) versões 1.13.2 e superiores.

### <a name="110"></a><a name="1.1.0"></a>1.1.0
* Adição de um método para obter uma estimativa do trabalho restante a ser processado no Feed de Alterações.
* Compatível com o [SDK do .NET para SQL](sql-api-sdk-dotnet.md) versões 1.13.2 e superiores.

### <a name="100"></a><a name="1.0.0"></a>1.0.0
* SDK DO GA
* Compatível com o [SDK do .NET para SQL](sql-api-sdk-dotnet.md) versões 1.14.1 e inferiores.

## <a name="release--retirement-dates"></a>Datas de lançamento e desativação

A Microsoft fornecerá uma notificação pelo menos **12 meses** antes de desativar um SDK, a fim de realizar uma transição tranquila para uma versão mais recente/com suporte.

Os novos recursos, funcionalidades e otimizações são adicionados apenas ao SDK atual. Portanto, recomendamos que você atualize sempre que possível para a versão do SDK mais recente. 

Qualquer solicitação feita ao Cosmos DB com o uso de um SDK desativado será rejeitada pelo serviço.

<br/>

| Versão | Data de lançamento | Data de desativação |
| --- | --- | --- |
| [2.3.0](#2.3.0) |2 de abril de 2020 |--- |
| [2.2.8](#2.2.8) |28 de outubro de 2019 |--- |
| [2.2.7](#2.2.7) |14 de maio de 2019 |--- |
| [2.2.6](#2.2.6) |29 janeiro de 2019 |--- |
| [2.2.5](#2.2.5) |13 de dezembro de 2018 |--- |
| [2.2.4](#2.2.4) |29 de novembro de 2018 |--- |
| [2.2.3](#2.2.3) |19 de novembro de 2018 |--- |
| [2.2.2](#2.2.2) |31 de outubro de 2018 |--- |
| [2.2.1](#2.2.1) |24 de outubro de 2018 |--- |
| [1.3.3](#1.3.3) |08 de maio de 2018 |--- |
| [1.3.2](#1.3.2) |18 de abril de 2018 |--- |
| [1.3.1](#1.3.1) |13 de março de 2018 |--- |
| [1.2.0](#1.2.0) |31 de outubro de 2017 |--- |
| [1.1.1](#1.1.1) |29 de agosto de 2017 |--- |
| [1.1.0](#1.1.0) |13 de agosto de 2017 |--- |
| [1.0.0](#1.0.0) |07 de julho de 2017 |--- |

## <a name="faq"></a>Perguntas frequentes

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Confira também

Para saber mais sobre o Cosmos DB, consulte a página de serviço do [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).
