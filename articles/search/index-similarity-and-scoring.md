---
title: Visão geral da similaridade e pontuação
titleSuffix: Azure Cognitive Search
description: Explica os conceitos de similaridade e de pontuação e o que um desenvolvedor pode fazer para personalizar o resultado da pontuação.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: 4c725fe74185088dea55b7506493fe667e71b7ae
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85806628"
---
# <a name="similarity-and-scoring-in-azure-cognitive-search"></a>Similaridade e pontuação no Azure Cognitive Search

A pontuação refere-se ao cálculo de uma pontuação de pesquisa para cada item retornado nos resultados da pesquisa para consultas de pesquisa de texto completo. A pontuação é um indicador de relevância de um item no contexto da operação de pesquisa atual. Quanto maior a pontuação, mais relevante será o item. Nos resultados da pesquisa, os itens são classificados de forma decrescente com base nas pontuações de pesquisa calculadas para cada item. 

Por padrão, os 50 primeiros são retornados na resposta, mas você pode usar o parâmetro **$top** para retornar um número menor ou maior de itens (até 1.000 em uma única resposta) e **$skip** para obter o próximo conjunto de resultados.

A pontuação de pesquisa é calculada com base nas propriedades estatísticas dos dados e da consulta. O Azure Cognitive Search localiza documentos que correspondem aos termos de pesquisa (alguns ou todos, dependendo do [searchMode](https://docs.microsoft.com/rest/api/searchservice/search-documents#searchmodeany--all-optional)), favorecendo documentos que contêm muitas instâncias do termo de pesquisa. A pontuação de pesquisa aumentará ainda mais se o termo for raro no índice de dados, mas comum no documento. A base para essa abordagem de cálculo de relevância é conhecida como *TF-IDF* ou, frequência do termo-inverso da frequência nos documentos.

Os valores de pontuação de pesquisa podem ser repetidos em todo um conjunto de resultados. Quando várias ocorrências têm a mesma pontuação de pesquisa, a ordenação dos mesmos itens pontuados não é definida e não é estável. Execute a consulta novamente e você poderá ver os itens mudarem de posição, principalmente se você estiver usando o serviço gratuito ou um serviço faturável com várias réplicas. Se houver dois itens com uma pontuação idêntica, não há garantia de qual deles aparecerá primeiro.

Se quiser quebrar o vínculo entre pontuações repetidas, você poderá adicionar uma cláusula **$orderby** para primeiro classificar por pontuação e, em seguida, por outro campo classificável (por exemplo, `$orderby=search.score() desc,Rating desc`). Para obter mais informações, consulte [$orderby](https://docs.microsoft.com/azure/search/search-query-odata-orderby).

> [!NOTE]
> Um `@search.score = 1.00` indica um conjunto de resultados sem pontuação ou sem classificação. A pontuação é uniforme entre todos os resultados. Os resultados sem pontuação ocorrem quando a forma de consulta for consulta de pesquisa difusa, curinga ou regex ou uma expressão **$filter**. 

## <a name="scoring-profiles"></a>Perfis de pontuação

Você pode personalizar a maneira como campos diferentes são classificados definindo um *perfil de pontuação* personalizado. Os perfis de pontuação oferecem maior controle sobre a classificação de itens nos resultados da pesquisa. Por exemplo, convém aumentar itens com base em seu potencial de receita, promover itens mais recentes ou talvez aumentar itens que estejam no inventário há muito tempo. 

Um perfil de pontuação faz parte da definição de índice, composta por funções, parâmetros e campos ponderados. Para obter mais informações sobre como definir um, consulte [Perfis de pontuação](index-add-scoring-profiles.md).

<a name="scoring-statistics"></a>

## <a name="scoring-statistics-and-sticky-sessions"></a>Estatísticas de Pontuação e sessões adesivas

Para escalabilidade, o Azure Cognitive Search distribui cada índice horizontalmente por meio de um processo de fragmentação, o que significa que partes de um índice são fisicamente separadas.

Por padrão, a pontuação de um documento é calculada com base nas propriedades estatísticas dos dados *dentro de um fragmento*. Essa abordagem geralmente não é um problema para uma grande corpus de dados e fornece um melhor desempenho do que ter que calcular a pontuação com base nas informações de todos os fragmentos. Dito isso, o uso dessa otimização de desempenho pode fazer com que dois documentos muito semelhantes (ou até mesmo idênticos) acabem com pontuações de relevância diferentes se eles terminarem em fragmentos diferentes.

Se preferir calcular a pontuação com base nas propriedades estatísticas de todos os fragmentos, você pode fazer isso adicionando *scoringStatistics=global* como um [parâmetro de consulta](https://docs.microsoft.com/rest/api/searchservice/search-documents) (ou adicionar *“scoringStatistics”: “global”* como um parâmetro de corpo da [solicitação de consulta](https://docs.microsoft.com/rest/api/searchservice/search-documents)).

```http
GET https://[service name].search.windows.net/indexes/[index name]/docs?scoringStatistics=global&api-version=2020-06-30&search=[search term]
  Content-Type: application/json
  api-key: [admin or query key]  
```
O uso de scoringStatistics garantirá que todos os fragmentos na mesma réplica forneçam os mesmos resultados. Dito isso, réplicas diferentes podem ser um pouco diferentes umas das outras, pois estão sempre sendo atualizadas com as alterações mais recentes no índice. Em alguns cenários, talvez você queira que os usuários obtenham resultados mais consistentes durante uma “sessão de consulta”. Nesses cenários, você pode fornecer um `sessionId` como parte de suas consultas. O `sessionId` é uma cadeia de caracteres única que você cria para se referir a uma sessão de usuário exclusiva.

```http
GET https://[service name].search.windows.net/indexes/[index name]/docs?sessionId=[string]&api-version=2020-06-30&search=[search term]
  Content-Type: application/json
  api-key: [admin or query key]  
```
Enquanto o mesmo `sessionId` for usado, será feita uma tentativa de melhor esforço para atingir a mesma réplica, aumentando a consistência dos resultados que os usuários verão. 

> [!NOTE]
> Reutilizar os mesmos valores `sessionId` repetidamente pode interferir no balanceamento de carga das solicitações entre réplicas e afetar adversamente o desempenho do serviço de pesquisa. O valor usado como sessionId não pode começar com um caractere “_”.

## <a name="similarity-ranking-algorithms"></a>Algoritmos de classificação de similaridade

O Azure Cognitive Search é compatível com dois algoritmos de classificação de similaridade diferentes: Um algoritmo de *similaridade clássica* e a implementação oficial do algoritmo *Okapi BM25* (atualmente em versão prévia). O algoritmo de similaridade clássica é o algoritmo padrão, mas a partir de 15 de julho, todos os novos serviços criados após essa data usam o novo algoritmo BM25. Ele será o único algoritmo disponível nos novos serviços.

Por enquanto, você pode especificar qual algoritmo de classificação de similaridade você gostaria de usar. Para obter mais informações, consulte [Algoritmo de classificação](index-ranking-similarity.md).

O segmento de vídeo a seguir avança rapidamente para uma explicação dos algoritmos de classificação usados no Azure Cognitive Search. Para obter mais informações, assista ao vídeo completo.

> [!VIDEO https://www.youtube.com/embed/Y_X6USgvB1g?version=3&start=322&end=643]

<a name="featuresMode-param"></a>

## <a name="featuresmode-parameter-preview"></a>parâmetro featuresmode (versão prévia)

As solicitações de [pesquisa de documentos](https://docs.microsoft.com/rest/api/searchservice/preview-api/search-documents) têm um novo parâmetro [featuresmode](https://docs.microsoft.com/rest/api/searchservice/preview-api/search-documents#featuresmode) que pode fornecer detalhes adicionais sobre a relevância no nível do campo. Enquanto o `@searchScore` é calculado para o documento tudo (o quão relevante é este documento no contexto dessa consulta), por meio de featuresmode, você pode obter informações sobre campos individuais, conforme expresso em uma `@search.features` estrutura. A estrutura contém todos os campos usados na consulta (sejam campos específicos por meio de **searchFields** em uma consulta ou todos os campos atribuídos como **pesquisáveis** em um índice). Para cada campo, você obtém os seguintes valores:

+ Número de tokens exclusivos encontrados no campo
+ Pontuação de similaridade ou uma medida de quão semelhante o conteúdo do campo é, relativo ao termo da consulta
+ Frequência de termos ou o número de vezes que o termo de consulta foi encontrado no campo

Para uma consulta que tem como alvo os campos "Descrição" e "título", uma resposta que o inclui `@search.features` pode ter esta aparência:

```json
"value": [
 {
    "@search.score": 5.1958685,
    "@search.features": {
        "description": {
            "uniqueTokenMatches": 1.0,
            "similarityScore": 0.29541412,
            "termFrequency" : 2
        },
        "title": {
            "uniqueTokenMatches": 3.0,
            "similarityScore": 1.75451557,
            "termFrequency" : 6
        }
```

Você pode consumir esses pontos de dados em [soluções de Pontuação personalizada](https://github.com/Azure-Samples/search-ranking-tutorial) ou usar as informações para depurar problemas de relevância de pesquisa.

## <a name="see-also"></a>Confira também

 [Referência da API REST](index-add-scoring-profiles.md) [dos perfis de pontuação](https://docs.microsoft.com/rest/api/searchservice/)   
 [Pesquisar API de documentos](https://docs.microsoft.com/rest/api/searchservice/search-documents)   
 [SDK do .NET do Azure Cognitive Search](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)  
