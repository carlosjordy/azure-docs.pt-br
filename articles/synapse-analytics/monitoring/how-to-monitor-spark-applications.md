---
title: Como monitorar Apache Spark aplicativos no Synapse Studio
description: Use o Synapse Studio para monitorar seus aplicativos Apache Spark.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: monitoring
ms.date: 04/15/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: a4dc2604dbd62da1baa4278ff3463f41337886bf
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87052501"
---
# <a name="use-synapse-studio-preview-to-monitor-your-apache-spark-applications"></a>Use o Synapse Studio (versão prévia) para monitorar seus aplicativos Apache Spark

Com o Azure Synapse Analytics, você pode usar o Spark para executar blocos de anotações, trabalhos e outros tipos de aplicativos em seus pools do Spark em seu espaço de trabalho.

Este artigo explica como monitorar seus aplicativos Apache Spark, permitindo que você fique atento ao status, aos problemas e ao progresso mais recentes.

## <a name="accessing-the-list-of-apache-spark-applications"></a>Acessando a lista de aplicativos Apache Spark

Para ver a lista de aplicativos Apache Spark em seu espaço de trabalho, primeiro [abra o Synapse Studio](https://web.azuresynapse.net/) e selecione seu espaço de trabalho.

![Fazer logon no espaço de trabalho](./media/common/login-workspace.png)

Depois de abrir seu espaço de trabalho, selecione a seção **Monitor** à esquerda.

![Selecionar Hub de monitor](./media/common/left-nav.png)

Selecione **Apache Spark aplicativos** para exibir a lista de aplicativos Apache Spark.

 ![Selecionar aplicativos Spark](./media/how-to-monitor-spark-applications/monitor-hub-nav-sparkapplications.png)

## <a name="filtering-your-apache-spark-applications"></a>Filtrando seus aplicativos Apache Spark

Você pode filtrar a lista de aplicativos Apache Spark para aqueles nos quais está interessado. Os filtros na parte superior da tela permitem especificar um campo no qual você deseja filtrar.

Por exemplo, você pode filtrar a exibição para ver apenas os Apache Spark aplicativos que contêm o nome "vendas":

![Botão Filtrar](./media/common/filter-button.png)

![Filtro de exemplo](./media/how-to-monitor-spark-applications/filter-example.png)

## <a name="viewing-details-about-a-specific-apache-spark-application"></a>Exibindo detalhes sobre um aplicativo Apache Spark específico

Para exibir os detalhes sobre um de seus aplicativos Apache Spark, selecione o aplicativo Apache Spark e exiba os detalhes. Se o aplicativo Apache Spark ainda estiver em execução, você poderá monitorar o progresso. [Leia mais](apache-spark-applications.md).

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre o monitoramento de execuções de pipeline, consulte o artigo [Monitor pipeline executa o Synapse Studio](how-to-monitor-pipeline-runs.md) . 

Para obter mais informações sobre como depurar Apache Spark aplicativo, consulte o artigo [monitorar Apache Spark aplicativos no Synapse Studio](apache-spark-applications.md) .