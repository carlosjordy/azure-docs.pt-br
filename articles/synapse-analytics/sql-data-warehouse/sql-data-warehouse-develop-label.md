---
title: Usando rótulos para consultas de instrumento
description: Dicas para usar rótulos para instrumentar consultas no pool de SQL do Synapse para desenvolver soluções.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: d4459547300f5dfc7b7c22d1e531b928a13aa66c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85213424"
---
# <a name="using-labels-to-instrument-queries-in-synapse-sql-pool"></a>Usando rótulos para consultas de instrumento no pool de SQL do Synapse

Estão incluídas neste artigo Dicas para o desenvolvimento de soluções usando rótulos para consultas de instrumento no pool do SQL.

Dicas para usar rótulos para consultas de instrumento no SQL Data Warehouse do Azure para desenvolvimento de soluções.

## <a name="what-are-labels"></a>O que são rótulos?

O pool SQL dá suporte a um conceito chamado rótulos de consulta. Antes de entrar em qualquer profundidade, vamos examinar um exemplo:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

A última linha marca a cadeia de caracteres ‘Meu Rótulo de Consulta’ à consulta. Essa marca é útil porque o rótulo é habilitado para consulta por meio de DMVs.

Consultar rótulos fornece um mecanismo para localizar as consultas de problema e ajudar a identificar o andamento por meio de uma execução de ELT.

Uma boa convenção de nomenclatura ajuda muito. Por exemplo, iniciar o rótulo com projeto, procedimento, instrução ou comentário identifica exclusivamente a consulta entre todo o código no controle do código-fonte.

A consulta a seguir usa uma exibição de gerenciamento dinâmico para pesquisar por rótulo:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> É essencial colocar colchetes ou aspas duplas em torno da palavra do rótulo ao consultar. Rótulo é uma palavra reservada e causa um erro quando ele não é delimitado.

## <a name="next-steps"></a>Próximas etapas

Para obter mais dicas de desenvolvimento, confira [visão geral de desenvolvimento](sql-data-warehouse-overview-develop.md).
