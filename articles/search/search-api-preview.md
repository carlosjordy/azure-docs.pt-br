---
title: Lista de versão prévia do recurso
titleSuffix: Azure Cognitive Search
description: Os recursos de visualização são liberados para que os clientes possam fornecer comentários sobre o design e o utilitário. Este artigo é uma lista abrangente de todos os recursos atualmente em visualização.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 07/09/2020
ms.openlocfilehash: b952d6b6fec9a1ec0dcd8af1a9566e67d3301d77
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86496699"
---
# <a name="preview-features-in-azure-cognitive-search"></a>Recursos de visualização no Azure Pesquisa Cognitiva

Este artigo é uma lista abrangente de todos os recursos que estão em visualização pública. A funcionalidade de versão prévia é fornecida sem um Contrato de Nível de Serviço e, portanto, não é recomendada para cargas de trabalho de produção. Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Os recursos de visualização que fazem a transição para a disponibilidade geral são removidos dessa lista. Se um recurso não estiver listado abaixo, você poderá presumir que ele está disponível para o público geral. Para obter anúncios sobre a disponibilidade geral, consulte [atualizações de serviço](https://azure.microsoft.com/updates/?product=search) ou [o que há de novo](whats-new.md).

|Recurso&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Categoria | Descrição | Disponibilidade  |
|---------|------------------|-------------|---------------|
| [**Habilidade do Azure Machine Learning (AML)**](cognitive-search-aml-skill.md) | Enriquecimento de IA| Um novo tipo de habilidade para integrar um ponto de extremidade inferência de Azure Machine Learning. Veja uma introdução [neste tutorial](cognitive-search-tutorial-aml-custom-skill.md). | Use [a API REST de pesquisa 2020-06-30-Preview](https://docs.microsoft.com/rest/api/searchservice/) ou 2019-05-06-Preview. Também disponível no portal, no design de Skills, supondo que Pesquisa Cognitiva e os serviços do Azure ML sejam implantados na mesma assinatura. |
| [**parâmetro featuresmode**](https://docs.microsoft.com/rest/api/searchservice/search-documents#featuresmode) | Relevância (Pontuação) | Expansão da Pontuação de relevância para incluir detalhes: por Pontuação de similaridade de campo, por frequência de termo de campo, e por número de campo de tokens exclusivos correspondentes. Você pode consumir esses pontos de dados em [soluções de Pontuação personalizadas](https://github.com/Azure-Samples/search-ranking-tutorial). | Adicione esse parâmetro de consulta usando [Pesquisar documentos (REST)](https://docs.microsoft.com/rest/api/searchservice/search-documents) com a API-Version = 2020-06 -30-preview ou 2019-05-06-Preview. |
| [**Identidade de Serviço Gerenciada**](search-howto-managed-identities-data-sources.md) | Indexadores, segurança| Registre um serviço de pesquisa com Azure Active Directory para torná-lo um serviço confiável e, em seguida, use as permissões RBAC em fontes de dados do Azure para permitir o acesso somente leitura por um indexador. | Acesse essa funcionalidade ao usar o portal ou [criar fonte de dados (REST)](https://docs.microsoft.com/rest/api/searchservice/create-data-source) com a API-Version = 2020-06 -30-Preview ou API-Version = 2019-05-06-Preview. |
| [**Sessões de depuração**](cognitive-search-debug-session.md) | Portal, enriquecimento de ia (contextset) | Um editor de qualificações na sessão usado para investigar e resolver problemas com um qualificable. As correções aplicadas durante uma sessão de depuração podem ser salvas em um conconhecimento no serviço. | Somente no portal, usando links de página intermediária na página Visão geral para abrir uma sessão de depuração. |
| [**Exclusão reversível de blob nativo**](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection) | Indexadores, BLOBs do Azure| O indexador de armazenamento de BLOBs do Azure no Azure Pesquisa Cognitiva reconhecerá os blobs que estão em um estado de exclusão reversível e removerá o documento de pesquisa correspondente durante a indexação. | Adicione esta definição de configuração usando [criar indexador (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer) com a API-Version = 2020-06 -30-Preview ou API-Version = 2019-05-06-Preview. |
| [**Habilidades de pesquisa de entidade personalizada**](cognitive-search-skill-custom-entity-lookup.md ) | Enriquecimento de ia (contextset) | Uma habilidade cognitiva que procura texto de uma lista personalizada de palavras e frases definidas pelo usuário. Usando essa lista, ela rotula todos os documentos com entidades correspondentes. A habilidade também dá suporte a um grau de correspondência difusa que pode ser aplicado para localizar correspondências semelhantes, mas não exatas. | Faça referência a essa habilidade de visualização usando o editor de contratação no portal ou crie o con-30 [(REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset) com a API-Version = 2020-06-Preview ou API-Version = 2019-05-06-Preview. |
| [**Habilidade de detecção de PII**](cognitive-search-skill-pii-detection.md) | Enriquecimento de ia (contextset) | Uma habilidade cognitiva usada durante a indexação que extrai informações de identificação pessoal de um texto de entrada e oferece a opção de mascarar o texto de várias maneiras. | Faça referência a essa habilidade de visualização usando o editor de contratação no portal ou crie o con-30 [(REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset) com a API-Version = 2020-06-Preview ou API-Version = 2019-05-06-Preview. |
| [**Enriquecimento adicional**](cognitive-search-incremental-indexing-conceptual.md) | Configuração do indexador| Adiciona o cache a um pipeline de enriquecimento, permitindo que você reutilize a saída existente se uma modificação direcionada, como uma atualização de um conjunto de qualificações ou outro objeto, não alterar o conteúdo. O Caching aplica-se somente a documentos aprimorados produzidos por um configurador de qualificações.| Adicione esta definição de configuração usando [criar indexador (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer) com a API-Version = 2020-06 -30-Preview ou API-Version = 2019-05-06-Preview. |
| [**Cosmos DB indexador: API do MongoDB, API Gremlin, API do Cassandra**](search-howto-index-cosmosdb.md) | Fonte de dados do indexador | Por Cosmos DB, a API do SQL está geralmente disponível, mas as APIs do MongoDB, Gremlin e Cassandra estão em versão prévia. | Somente para Gremlin e Cassandra, [Inscreva-se primeiro](https://aka.ms/azure-cognitive-search/indexer-preview) para que o suporte possa ser habilitado para sua assinatura no back-end. As fontes de dados do MongoDB podem ser configuradas no Portal. Caso contrário, a configuração da fonte de dados para todas as três APIs terá suporte usando a [fonte de dados de criação (REST)](https://docs.microsoft.com/rest/api/searchservice/create-data-source) com a versão da API = 2020-06 -30-Preview ou API-Version = 2019-05-06-Preview. |
|  [**Indexador do Azure Data Lake Storage Gen2**](search-howto-index-azure-data-lake-storage.md) | Fonte de dados do indexador | Indexe o conteúdo e os metadados do Data Lake Storage Gen2.| A [inscrição](https://aka.ms/azure-cognitive-search/indexer-preview) é necessária para que o suporte possa ser habilitado para sua assinatura no back-end. Acesse essa fonte de dados usando [criar fonte de dados (REST)](https://docs.microsoft.com/rest/api/searchservice/create-data-source) com a API-Version = 2020-06 -30-Preview ou API-Version = 2019-05-06-Preview. |
| [**moreLikeThis**](search-more-like-this.md) | Consulta | Localiza documentos que são relevantes para um documento específico. Esse recurso é encontrado em versões prévias anteriores. | Adicione este parâmetro de consulta em chamadas de [documentos de pesquisa (REST)](https://docs.microsoft.com/rest/api/searchservice/search-documents) com a versão de API = 2020-06 -30-preview, 2019-05-06-preview, 2016-09-01-preview ou 2017-11-11-Preview. |

## <a name="calling-preview-rest-apis"></a>Chamando APIs REST de visualização

O Azure Pesquisa Cognitiva sempre libera recursos experimentais por meio da API REST primeiro e, em seguida, por meio de versões de pré-lançamento do SDK do .NET.

Os recursos de versão prévia estão disponíveis para teste e experimentação com a meta de coletar comentários sobre o design e a implementação de recursos. Por esse motivo, os recursos de versão prévia podem ser alterados ao longo do tempo, possivelmente de maneiras que interrompam a compatibilidade com versões anteriores. Isso é diferente de recursos em uma versão de GA, que são estáveis e têm pouca probabilidade de serem alterados, com a exceção de aprimoramentos e pequenas correções de compatibilidade com versões anteriores. Além disso, os recursos de versão prévia nem sempre são liberados para GA.

Embora alguns recursos de visualização possam estar disponíveis no portal e no SDK do .NET, a API REST sempre tem recursos de visualização.

+ Para operações de pesquisa, [**`2020-06-30-Preview`**](https://docs.microsoft.com/rest/api/searchservice/index-preview) é a versão de visualização atual.

+ Para operações de gerenciamento, [**`2019-10-01-Preview`**](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview) é a versão de visualização atual.

As versões prévias mais antigas ainda funcionam, mas se tornam obsoletas ao longo do tempo. Se seu código chamar `api-version=2019-05-06-Preview` ou `api-version=2016-09-01-Preview` ou `api-version=2017-11-11-Preview` , essas chamadas ainda serão válidas. No entanto, apenas a versão prévia mais recente é atualizada com melhorias. 

A sintaxe de exemplo a seguir ilustra uma chamada para a versão de visualização da API.

```HTTP
GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2020-06-30-Preview
```

O serviço de Pesquisa Cognitiva do Azure está disponível em várias versões. Para obter mais informações, confira [Versões de API](search-api-versions.md).

## <a name="next-steps"></a>Próximas etapas

Examine a documentação de referência da API de visualização REST de pesquisa. Se você tiver problemas, peça ajuda em [Stack Overflow](https://stackoverflow.com/) ou [entre em contato com o suporte](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Referência da API REST do serviço de pesquisa (versão prévia)](https://docs.microsoft.com/rest/api/searchservice/index-preview)