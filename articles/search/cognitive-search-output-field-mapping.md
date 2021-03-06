---
title: Mapear entrada para campos de saída
titleSuffix: Azure Cognitive Search
description: Extrair e enriquecer campos de dados de origem e mapear para campos de saída em um índice de Pesquisa Cognitiva do Azure.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: c9b0b34202f35babcaa3dce37331d31edf641254
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85557266"
---
# <a name="how-to-map-ai-enriched-fields-to-a-searchable-index"></a>Como mapear campos de ia-ricos para um índice pesquisável

Neste artigo, você aprenderá como mapear campos de entrada enriquecidos para campos de saída em um índice pesquisável. Uma vez que você [definiu um conjunto de qualificações](cognitive-search-defining-skillset.md), mapeie os campos de saída de qualquer habilidade diretamente a contribuição de valores para um determinado campo no índice de pesquisa. 

Os mapeamentos de campo de saída são necessários para mover o conteúdo de documentos aprimorados para o índice.  O documento aprimorado é realmente uma árvore de informações e, embora haja suporte para tipos complexos no índice, às vezes você pode querer transformar as informações da árvore aprimorada em um tipo mais simples (por exemplo, uma matriz de cadeias de caracteres). Os mapeamentos de campo de saída permitem que você execute transformações de forma de dados por meio do nivelamento de informações.

> [!NOTE]
> Habilitamos recentemente a funcionalidade das funções de mapeamento em mapeamentos de campo de saída. Para obter mais detalhes sobre as funções de mapeamento, consulte [funções de mapeamento de campos](https://docs.microsoft.com/azure/search/search-indexer-field-mappings#field-mapping-functions)

## <a name="use-outputfieldmappings"></a>Use outputFieldMappings
Para mapear campos, adicione `outputFieldMappings` à sua definição do indexador, conforme mostrado abaixo:

```http
PUT https://[servicename].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
api-key: [admin key]
Content-Type: application/json
```

O corpo da solicitação é estruturado da seguinte maneira:

```json
{
    "name": "myIndexer",
    "dataSourceName": "myDataSource",
    "targetIndexName": "myIndex",
    "skillsetName": "myFirstSkillSet",
    "fieldMappings": [
        {
            "sourceFieldName": "metadata_storage_path",
            "targetFieldName": "id",
            "mappingFunction": {
                "name": "base64Encode"
            }
        }
    ],
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/content/organizations/*/description",
            "targetFieldName": "descriptions",
            "mappingFunction": {
                "name": "base64Decode"
            }
        },
        {
            "sourceFieldName": "/document/content/organizations",
            "targetFieldName": "orgNames"
        },
        {
            "sourceFieldName": "/document/content/sentiment",
            "targetFieldName": "sentiment"
        }
    ]
}
```

Para cada mapeamento de campo de saída, defina o local dos dados na árvore de documentos enriquecedo (sourceFieldName) e o nome do campo como referenciado no índice (targetFieldName).

## <a name="flattening-information-from-complex-types"></a>Nivelamento de informações de tipos complexos 

O caminho em um sourceFieldName pode representar um elemento ou vários elementos. No exemplo acima, ```/document/content/sentiment``` representa um único valor numérico, enquanto ```/document/content/organizations/*/description``` representa várias descrições de organização. 

Em casos onde há vários elementos, são "simples" em uma matriz que contém cada um dos elementos. 

Mais concretamente, para o ```/document/content/organizations/*/description``` exemplo, os dados no campo *descrições* seriam uma matriz simples de descrições antes que ele obtém indexado:

```
 ["Microsoft is a company in Seattle","LinkedIn's office is in San Francisco"]
```

Esse é um princípio importante, então forneceremos outro exemplo. Imagine que você tenha uma matriz de tipos complexos como parte da árvore de enriquecimento. Digamos que haja um membro chamado customEntities que tenha uma matriz de tipos complexos, como o descrito abaixo.

```json
"document/customEntities": 
[
    {
        "name": "heart failure",
        "matches": [
            {
                "text": "heart failure",
                "offset": 10,
                "length": 12,
                "matchDistance": 0.0
            }
        ]
    },
    {
        "name": "morquio",
        "matches": [
            {
                "text": "morquio",
                "offset": 25,
                "length": 7,
                "matchDistance": 0.0
            }
        ]
    }
    //...
]
```

Vamos supor que o índice tenha um campo chamado ' doenças ' do tipo Collection (EDM. String), em que você gostaria de armazenar cada um dos nomes das entidades. 

Isso pode ser feito facilmente usando o símbolo " \* ", da seguinte maneira:

```json
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/customEntities/*/name",
            "targetFieldName": "diseases"
        }
    ]
```

Essa operação simplesmente "achata" cada um dos nomes dos elementos customEntities em uma única matriz de cadeias de caracteres como esta:

```json
  "diseases" : ["heart failure","morquio"]
```

## <a name="next-steps"></a>Próximas etapas
Após mapear os campos enriquecidos para campos de pesquisa, defina os atributos de campo para cada um dos campos pesquisáveis [como parte da definição do índice](search-what-is-an-index.md).

Para obter mais informações sobre mapeamento de campo, consulte [mapeamentos de campo no Azure pesquisa cognitiva indexadores](search-indexer-field-mappings.md).
