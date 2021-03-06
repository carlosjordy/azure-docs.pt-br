---
title: Azure Functions de monitoramento com logs de Azure Monitor
description: Saiba como usar os logs de Azure Monitor com Azure Functions para monitorar as execuções de função.
author: craigshoemaker
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: cshoe
ms.custom: tracking-python
ms.openlocfilehash: a84dda648ccc1dc73956aeb5d8364c4c3f31c9f6
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87055282"
---
# <a name="monitoring-azure-functions-with-azure-monitor-logs"></a>Azure Functions de monitoramento com logs de Azure Monitor

O Azure Functions oferece uma integração com [Azure monitor logs](../azure-monitor/platform/data-platform-logs.md) para monitorar funções. Este artigo mostra como configurar Azure Functions para enviar logs gerados pelo sistema e pelo usuário a logs do Azure Monitor.

Os logs de Azure Monitor fornecem a capacidade de consolidar logs de diferentes recursos no mesmo espaço de trabalho, onde podem ser analisados com [consultas](../azure-monitor/log-query/log-query-overview.md) para recuperar, consolidar e analisar rapidamente os dados coletados.  É possível criar e testar consultas usando o [Log Analytics](../azure-monitor/log-query/log-query-overview.md) no portal do Azure e, em seguida, analisar diretamente os dados usando essas ferramentas ou salvar consultas para uso com [visualizações](../azure-monitor/visualizations.md) ou [regras de alerta](../azure-monitor/platform/alerts-overview.md).

O Azure Monitor usa uma versão da [linguagem de consulta Kusto](/azure/kusto/query/) usada pelo Azure Data Explorer que é adequada para consultas de log simples, mas também inclui funcionalidades avançadas como agregações, junções e análises inteligentes. É possível aprender a linguagem de consulta rapidamente por meio de [várias lições](../azure-monitor/log-query/get-started-queries.md).

> [!NOTE]
> A integração com os logs de Azure Monitor está atualmente em visualização pública para aplicativos de funções em execução no consumo do Windows, Premium e planos de hospedagem dedicados.

## <a name="setting-up"></a>Configurando

1. Na seção **monitoramento** do seu aplicativo de funções no [portal do Azure](https://portal.azure.com), selecione **configurações de diagnóstico**e, em seguida, selecione **Adicionar configuração de diagnóstico**.

   :::image type="content" source="media/functions-monitor-log-analytics/diagnostic-settings-add.png" alt-text="Selecionar configurações de diagnóstico":::

1. Na página **configurações de diagnóstico** , em **detalhes da categoria** e **log**, escolha **FunctionAppLogs**.

   A tabela **FunctionAppLogs** contém os logs desejados.

1. Em **detalhes de destino**, escolha **Enviar para log Analytics**. em seguida, selecione o **espaço de trabalho log Analytics**. 

1. Insira um **nome de configurações de diagnóstico**e, em seguida, selecione **salvar**.

   :::image type="content" source="media/functions-monitor-log-analytics/choose-table.png" alt-text="Adicionar uma configuração de diagnóstico":::

## <a name="user-generated-logs"></a>Logs gerados pelo usuário

Para gerar logs personalizados, use a instrução de registro em log específica para seu idioma. Aqui estão os trechos de código de exemplo:


# <a name="c"></a>[C#](#tab/csharp)

```csharp
log.LogInformation("My app logs here.");
```

# <a name="java"></a>[Java](#tab/java)

```java
context.getLogger().info("My app logs here.");
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
context.log('My app logs here.');
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
Write-Host "My app logs here."
```

# <a name="python"></a>[Python](#tab/python)

```python
logging.info('My app logs here.')
```

---

## <a name="querying-the-logs"></a>Consultando os logs

Para consultar os logs gerados:
 
1. Em seu aplicativo de funções, selecione **configurações de diagnóstico**. 

1. Na lista **configurações de diagnóstico** , selecione o espaço de trabalho log Analytics que você configurou para enviar os logs de função. 

1. Na página **log Analytics espaço de trabalho** , selecione **logs**.

   Azure Functions grava todos os logs na tabela **FunctionAppLogs** em **LogManagement**. 

   :::image type="content" source="media/functions-monitor-log-analytics/querying.png" alt-text="Janela de consulta no espaço de trabalho Log Analytics":::

Aqui estão algumas consultas de exemplo:

### <a name="all-logs"></a>Todos os logs

```

FunctionAppLogs
| order by TimeGenerated desc

```

### <a name="specific-function-logs"></a>Logs de função específicos

```

FunctionAppLogs
| where FunctionName == "<Function name>" 

```

### <a name="exceptions"></a>Exceções

```

FunctionAppLogs
| where ExceptionDetails != ""  
| order by TimeGenerated asc

```

## <a name="next-steps"></a>Próximas etapas

- Examine a [Azure Functions visão geral](functions-overview.md).
- Saiba mais sobre [os logs de Azure monitor](../azure-monitor/platform/data-platform-logs.md).
- Saiba mais sobre a [linguagem de consulta](../azure-monitor/log-query/get-started-queries.md).
