---
title: Introdução ao suporte interno do Jupyter Notebooks no Azure Cosmos DB (versão prévia)
description: Saiba como usar o suporte interno para Jupyter Notebooks no Azure Cosmos DB para executar consultas interativamente.
ms.service: cosmos-db
ms.topic: overview
ms.date: 05/19/2020
author: deborahc
ms.author: dech
ms.openlocfilehash: 53725e7d4c39405e7ba47f8915e7444ce6a2167a
ms.sourcegitcommit: 23604d54077318f34062099ed1128d447989eea8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2020
ms.locfileid: "85118442"
---
# <a name="built-in-jupyter-notebooks-support-in-azure-cosmos-db-preview"></a>Suporte interno para o Jupyter Notebooks no Azure Cosmos DB (versão prévia)

O Jupyter Notebook é um aplicativo Web de software livre que permite que criar e compartilhar documentos que contêm código ao vivo, equações, visualizações e texto narrativo. 

Os notebooks Jupyter internos do Azure Cosmos DB são integrados diretamente ao portal do Azure e às suas contas do Azure Cosmos DB, tornando-os convenientes e fáceis de usar. Desenvolvedores, cientistas de dados, engenheiros e analistas podem usar a experiência familiar dos Jupyter notebooks para realizar exploração de dados, limpeza de dados, transformações de dados, simulações numéricas, modelagem estatística, visualização de dados e aprendizado de máquina.

:::image type="content" source="./media/cosmosdb-jupyter-notebooks/cosmos-notebooks-overview.png" alt-text="Visualizações de Jupyter Notebooks no Azure Cosmos DB":::

O Azure Cosmos DB dá suporte notebooks de C# e Python para todas as APIs, incluindo Core (SQL), Cassandra, Gremlin, Table e a API do MongoDB. Dentro do notebook, você pode aproveitar os comandos e recursos internos que facilitam a criação de recursos do Azure Cosmos DB, o carregamento de dados e a consulta e visualização dos dados no Azure Cosmos DB. 

:::image type="content" source="./media/cosmosdb-jupyter-notebooks/jupyter-notebooks-portal.png" alt-text="Suporte aos notebooks Jupyter no Azure Cosmos DB":::

## <a name="benefits-of-jupyter-notebooks"></a>Benefício do Jupyter Notebooks

Os notebooks Jupyter foram desenvolvido originalmente para aplicativos de ciência de dados escritos em Python, R. No entanto, eles podem ser usados de várias maneiras para diferentes tipos de projetos, incluindo:

**Visualização de dados:** os notebooks Jupyter permitem visualizar dados na forma de um notebook compartilhado que renderiza um conjunto de dados como um gráfico. Você pode criar visualizações, fazer alterações interativas no conjunto de dados e no código compartilhado e compartilhar os resultados.

**Compartilhamento de código:** serviços como o GitHub fornecem maneiras de compartilhar código, mas são, na maior parte, não interativos. Com um Jupyter Notebook, você pode exibir o código, executá-lo e exibir os resultados diretamente no portal do Azure.

**Interações dinâmicas com o código:** o código em um notebook Jupyter é dinâmico; você pode editá-lo e executar as atualizações incrementalmente em tempo real. Você também pode inserir controles de usuário (por exemplo, controles deslizantes ou campos de entrada de texto) que são usados como fontes de entrada para código, demonstrações ou POCs (provas de conceito).

**Documentação de exemplos de código e resultados da exploração de dados:** Se tiver um trecho de código e desejar explicar como ele funciona, você poderá incorporá-lo em uma Jupyter Notebook. Você pode adicionar interatividade com a documentação ao mesmo tempo.

**Comandos internos para o Azure Cosmos DB:** os comandos magic internos do Azure Cosmos DB facilitam a interação com sua conta. Você pode usar comandos como %%upload e %%sql para carregar dados em um contêiner e consultá-los usando a [sintaxe da API do SQL](sql-query-getting-started.md). Você não precisa escrever código personalizado adicional.

**Ambiente completo em um local:** o Jupyter Notebooks combina código, rich text, imagens, vídeos, animações, equações matemáticas, plotagens, mapas, figuras interativas, widgets e interfaces gráficas do usuário em um único documento.

## <a name="components-of-a-jupyter-notebook"></a>Componentes de um Jupyter Notebook

Os Jupyter Notebooks podem incluir vários tipos de componentes, cada um organizado em blocos ou células discretas:

**Texto e HTML:** texto sem formatação ou texto anotado na sintaxe de Markdown para gerar HTML, pode ser inserido no documento a qualquer momento. O estilo de CSS também pode ser incluído embutido ou adicionado ao modelo usado para gerar o notebook.

**Código e saída:** Jupyter Notebooks têm suporte para código Python e C#. Os resultados do código executado aparecem imediatamente após os blocos de código. Os blocos de código podem ser executados várias vezes em qualquer ordem desejada.

**Visualizações:** você pode gerar grafos e gráficos do código usando módulos como Matplotlib, Plotly, Bokeh e outros. De forma semelhante à saída, essas visualizações aparecem embutidas ao lado do código que as gera. De forma semelhante à saída, essas visualizações aparecem embutidas ao lado do código que as gera.

**Multimídia:** como os notebooks Jupyter são criados com base na tecnologia da Web, eles podem exibir todos os tipos de multimídia com suporte de uma página da Web. Você pode incluí-los em um notebook como elementos HTML ou pode gerá-los de forma programática usando o módulo `IPython.display`.

**Dados:** você pode importar dos dados dos contêineres do Azure Cosmos ou os resultados das consultas em um notebook Jupyter de maneira programática. Use comandos magic internos para carregar ou consultar dados no Azure Cosmos DB. 

## <a name="next-steps"></a>Próximas etapas

Para começar a usar notebooks Jupyter internos no Azure Cosmos DB, confira os seguintes artigos:

* [Habilitar notebooks em uma conta do Azure Cosmos](enable-notebooks.md)
* [Usar recursos e comandos de notebook Python](use-python-notebook-features-and-commands.md)
* [Usar recursos e comandos de notebook C#](use-csharp-notebook-features-and-commands.md)