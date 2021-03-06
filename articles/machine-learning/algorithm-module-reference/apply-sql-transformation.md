---
title: Aplicar transformação de SQL
titleSuffix: Azure Machine Learning
description: Saiba como usar o módulo aplicar transformação de SQL no Azure Machine Learning para executar uma consulta do SQLite em conjuntos de dados de entrada para transformar os dados.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 09/09/2019
ms.openlocfilehash: 2e44a4861e2522b766aab9c7151d76c471dd2d8c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76314531"
---
# <a name="apply-sql-transformation"></a>Aplicar transformação de SQL

Este artigo descreve um módulo do designer de Azure Machine Learning (versão prévia).

Usando o módulo aplicar transformação SQL, você pode:
  
-   Criar tabelas para resultados e salvar os conjuntos de dados em um banco de dados portátil.  
  
-   Realizar transformações personalizadas em tipos de dados ou criar agregações.  
  
-   Executar instruções de consulta SQL para filtrar ou alterar dados e retornar os resultados da consulta como uma tabela de dados.  

> [!IMPORTANT]
> O mecanismo SQL usado neste módulo é **SQLite**. Para obter mais informações sobre a sintaxe do SQLite, consulte [SQL como compreendido pelo SQLite](https://www.sqlite.org/index.html) para obter mais informações.  

## <a name="how-to-configure-apply-sql-transformation"></a>Como configurar aplicar transformação SQL  

O módulo pode ter até três conjuntos de dados como entradas. Ao fazer referência aos conjuntos de dados conectados a cada porta de entrada, você deve usar os nomes `t1` , `t2` e `t3` . O número da tabela indica o índice da porta de entrada.  
  
O parâmetro restante é uma consulta SQL, que usa a sintaxe do SQLite. Ao digitar várias linhas na caixa de texto **script SQL** , use um ponto-e-vírgula para encerrar cada instrução. Caso contrário, as quebras de linha serão convertidas em espaços.  

Este módulo oferece suporte a todas as instruções padrão da sintaxe do SQLite. Para obter uma lista de instruções sem suporte, consulte a seção [observações técnicas](#technical-notes) .

##  <a name="technical-notes"></a>Observações técnicas  

Esta seção contém detalhes de implementação, dicas e respostas para perguntas frequentes.

-   Uma entrada é sempre necessária na porta 1.  
  
-   Para identificadores de coluna que contêm um espaço ou outros caracteres especiais, sempre coloque o identificador de coluna entre colchetes ou aspas duplas ao se referir à coluna nas `SELECT` `WHERE` cláusulas ou.  
  
### <a name="unsupported-statements"></a>Instruções sem suporte  

Embora SQLite suporte a maior parte do padrão ANSI SQL, ele não inclui muitos recursos com suporte nos sistemas de banco de dados relacional comercial. Para obter mais informações, consulte [SQL como compreendido pelo SQLite](http://www.sqlite.org/lang.html). Além disso, esteja ciente das seguintes restrições ao criar instruções SQL:  
  
- O SQLite usa digitação dinâmica para valores, em vez de atribuir um tipo a uma coluna como na maioria dos sistemas de banco de dados relacional. Ele é digitado sem rigidez e permite a conversão implícita de tipos.  
  
- `LEFT OUTER JOIN`é implementado, mas não `RIGHT OUTER JOIN` ou `FULL OUTER JOIN` .  

- Você pode usar as instruções `RENAME TABLE` e `ADD COLUMN` com o comando `ALTER TABLE`, mas outras cláusulas não são suportadas, incluindo `DROP COLUMN`, `ALTER COLUMN` e `ADD CONSTRAINT`.  
  
- Você pode criar uma VIEW dentro do SQLite, mas depois disso as exibições serão somente leitura. Você não pode executar uma instrução `DELETE`, `INSERT` ou `UPDATE` em uma exibição. No entanto, você pode criar um disparador que é acionado em uma tentativa de `DELETE`, `INSERT` ou `UPDATE` em uma exibição e executar outras operações no corpo do gatilho.  
  

Além da lista de funções não suportadas fornecida no site do SQLite oficial, o seguinte wiki fornece uma lista de outros recursos sem suporte: [SQLite-SQL sem suporte](http://www2.sqlite.org/cvstrac/wiki?p=UnsupportedSql)  
    
## <a name="next-steps"></a>Próximas etapas

Confira o [conjunto de módulos disponíveis](module-reference.md) no Azure Machine Learning. 
