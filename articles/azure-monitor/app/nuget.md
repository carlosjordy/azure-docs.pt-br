---
title: Azure Monitor Application Insights pacotes NuGet
description: Azure Monitor Application Insights listas de pacotes NuGet para ASP.NET, ASP.NET Core, Python
ms.topic: reference
ms.date: 10/16/2018
ms.openlocfilehash: 91b48aee64a6b6942719f7f4abfb444b894c728e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85807155"
---
# <a name="application-insights-nuget-packages"></a>Pacotes NuGet do Application Insights

Veja abaixo a lista atual de pacotes NuGet de versão estável para o Application Insights.

## <a name="common-packages-for-aspnet"></a>Pacotes comuns do ASP.NET

| Nome do Pacote | Versão estável | Descrição | Baixar |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights | 2.12.0 | Fornece funcionalidade básica para transmissão de todos os tipos do Application Insights Telemetry e é um pacote dependente para todos os outros pacotes do Application Insights | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) |
|Microsoft.ApplicationInsights.Agent.Intercept | 2.4.0 | Permite a interceptação de chamadas de método | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Agent.Intercept/) |
| Microsoft.ApplicationInsights.DependencyCollector | 2.12.0 | O Coletor de Dependências do Application Insights para aplicativos .NET. Esse é um pacote dependente para pacotes específicos da plataforma do Application Insights e fornece a coleta automática de telemetria de dependências. | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector/) |
| Microsoft.ApplicationInsights.PerfCounterCollector | 2.12.0 | O Coletor de Contadores de Desempenho do Application Insights permite que você envie os dados coletados pelos Contadores de desempenho para o Application Insights. | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector/) |
| Microsoft.ApplicationInsights.Web | 2.12.0 | Application Insights para aplicativos Web .NET | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) |
| Microsoft.ApplicationInsights.WindowsServer | 2.12.0 | O pacote NuGet do Windows Server do Application Insights oferece a coleta automática de telemetria de insights de aplicativos para aplicativos .NET. Esse pacote pode ser usado como um pacote dependente para pacotes específicos da plataforma do Application Insights ou como um pacote independente para aplicativos .NET que não são cobertos pelos pacotes específicos da plataforma (como para funções de trabalho do .NET). | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/)  
| Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel | 2.12.0 | Fornece um canal de telemetria para o SDK do Windows Server do Application Insights que preservará a telemetria em cenários offline. | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/) |

## <a name="common-packages-for-aspnet-core"></a>Pacotes comuns do ASP.NET Core

| Nome do Pacote | Versão estável | Descrição | Baixar |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.AspNetCore | 2.5.0 | Application Insights para aplicativos Web ASP.NET Core. | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/) |
| Microsoft.ApplicationInsights | 2.12.0 | Este pacote fornece funcionalidade básica para transmissão de todos os tipos do Application Insights Telemetry e é um pacote dependente para todos os outros pacotes do Application Insights | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) |
| Microsoft.ApplicationInsights.DependencyCollector | 2.12.0 | O Coletor de Dependências do Application Insights para aplicativos .NET. Esse é um pacote dependente para pacotes específicos da plataforma do Application Insights e fornece a coleta automática de telemetria de dependências. | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector/) |
| Microsoft.ApplicationInsights.PerfCounterCollector | 2.12.0 | O Coletor de Contadores de Desempenho do Application Insights permite que você envie os dados coletados pelos Contadores de desempenho para o Application Insights. | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector/) |
| Microsoft.ApplicationInsights.WindowsServer | 2.12.0 | O pacote NuGet do Windows Server do Application Insights oferece a coleta automática de telemetria de insights de aplicativos para aplicativos .NET. Esse pacote pode ser usado como um pacote dependente para pacotes específicos da plataforma do Application Insights ou como um pacote independente para aplicativos .NET que não são cobertos pelos pacotes específicos da plataforma (como para funções de trabalho do .NET). | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/)  |
| Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel | 2.12.0 | Fornece um canal de telemetria para o SDK do Windows Server do Application Insights que preservará a telemetria em cenários offline. | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/) |

## <a name="common-packages-for-python-using-opencensus"></a>Pacotes comuns para Python usando OpenCensus
| Nome do Pacote | Versão estável | Descrição | Baixar |
|-------------------------------|-----------------------|------------|----|
| opencensus-ext-Azure | 1.0.0 | Application Insights para aplicativos Python em Azure Monitor via OpenCensus. | [Baixar Pacote](https://pypi.org/project/opencensus-ext-azure/) |
| opencensus-ext-Django | 0.7.2 | Este pacote fornece integração com a Biblioteca Python [Django](https://pypi.org/project/django/) . | [Baixar Pacote](https://pypi.org/project/opencensus-ext-django/) |
| opencensus-ext-Flask | 0.7.3 | Este pacote fornece integração com a Biblioteca Python [Flask](https://pypi.org/project/flask/) . | [Baixar Pacote](https://pypi.org/project/opencensus-ext-flask/) |
| opencensus-ext-httplib | 0.7.2 | Este pacote fornece integração com a biblioteca de [http. cliente](https://docs.python.org/3/library/http.client.html) do Python para Python3 e [httplib](https://docs.python.org/2/library/httplib.html) para Python2. | [Baixar Pacote](https://pypi.org/project/opencensus-ext-httplib/) |
| registro em log opencensus-ext | 0.1.0 | Este pacote enriquece os registros de log com os dados de rastreamento. | [Baixar Pacote](https://pypi.org/project/opencensus-ext-logging/) |
| opencensus-ext-MySQL | 0.1.2 | Este pacote fornece integração com a biblioteca do [conector MySQL](https://pypi.org/project/mysql-connector/) do Python. | [Baixar Pacote](https://pypi.org/project/opencensus-ext-mysql/) |
| opencensus-ext-PostgreSQL | 0.1.2 | Este pacote fornece integração com a Biblioteca Python [psycopg2](https://pypi.org/project/psycopg2/) . | [Baixar Pacote](https://pypi.org/project/opencensus-ext-postgresql/) |
| opencensus-ext-pymongo | 0.7.1 | Este pacote fornece integração com a Biblioteca Python [pymongo](https://pypi.org/project/pymongo/) . | [Baixar Pacote](https://pypi.org/project/opencensus-ext-pymongo/) |
| opencensus-ext-pymysql | 0.1.2 | Este pacote fornece integração com a Biblioteca Python [PyMySQL](https://pypi.org/project/PyMySQL/) . | [Baixar Pacote](https://pypi.org/project/opencensus-ext-pymysql/) |
| opencensus-ext-pirâmide | 0.7.1 | Este pacote fornece integração com a biblioteca de [pirâmides](https://pypi.org/project/pyramid/) do Python. | [Baixar Pacote](https://pypi.org/project/opencensus-ext-pyramid/) |
| opencensus-ext-solicitações | 0.7.2 | Este pacote fornece integração com a biblioteca de [solicitações](https://pypi.org/project/requests/) do Python. | [Baixar Pacote](https://pypi.org/project/opencensus-ext-requests/) |
| opencensus-ext-SQLAlchemy | 0.1.2 | Este pacote fornece integração com a Biblioteca Python [SQLAlchemy](https://pypi.org/project/SQLAlchemy/) . | [Baixar Pacote](https://pypi.org/project/opencensus-ext-sqlalchemy/) |

## <a name="listenerscollectorsappenders"></a>Ouvintes/coletores/appenders

| Nome do Pacote | Versão estável | Descrição | Baixar |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.DiagnosticSourceListener | 2.7.2 |  Permite eventos de encaminhamento do DiagnosticSource para o Application Insights. | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener/) |
| Microsoft.ApplicationInsights.EventSourceListener | 2.7.2 | O EventSourceListener do Application Insights permite enviar dados de eventos do EventSource ao Application Insights. | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener/) |
| Microsoft.ApplicationInsights.EtwCollector | 2.7.2 | O EtwCollector do Application Insights permite enviar dados do Rastreamento de Eventos para Windows (ETW) ao Application Insights. | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector/) |
| Microsoft.ApplicationInsights.TraceListener | 2.7.2 | Um TraceListener personalizado que permite enviar mensagens de log de rastreamento ao Application Insights. | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.TraceListener/) |
| Microsoft.ApplicationInsights.Log4NetAppender | 2.7.2 | Um appender personalizado que permite enviar mensagens de log do Log4Net ao Application Insights. | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Log4NetAppender/)
| Microsoft.ApplicationInsights.NLogTarget | 2.7.2 |  um destino personalizado que permite enviar mensagens de log do NLog ao Application Insights. | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.NLogTarget/)
| Microsoft.ApplicationInsights.SnapshotCollector | 1.3.1 | Monitora exceções em seu aplicativo e coleta automaticamente os instantâneos para análise offline. | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector/)

## <a name="service-fabric"></a>Service Fabric

| Nome do Pacote | Versão estável | Descrição | Baixar |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.ServiceFabric | 2.2.0 | Este pacote fornece a decoração automática de telemetria com o contexto do service fabric no qual o aplicativo está sendo executado. Não use este NuGet para aplicativos do Service Fabric nativo. | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.ServiceFabric/) |
| Microsoft.ApplicationInsights.ServiceFabric.Native | 2.2.0 | Módulo do Application Insights para aplicativos do Service Fabric. Use este NuGet apenas para aplicativos do Service Fabric nativo. Para aplicativos executados em contêineres, use o pacote Microsoft.ApplicationInsights.ServiceFabric. | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.ServiceFabric.Native/) |  

## <a name="status-monitor"></a>Status Monitor

| Nome do Pacote | Versão estável | Descrição | Baixar |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.Agent_x64 | 2.2.1 |  Habilita a coleta de dados de runtime para aplicativos x64 | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Agent_x64/) |
| Microsoft.ApplicationInsights.Agent_x86 | 2.2.1 |  Habilita a coleta de dados de runtime para aplicativos x86. | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Agent_x86/) |

Esses pacotes fazem parte da funcionalidade principal do monitoramento de runtime no [Status Monitor](../../azure-monitor/app/monitor-performance-live-website-now.md). Você não precisa baixar esses pacotes diretamente. Basta usar o instalador do Status Monitor. Se você quiser entender mais sobre como esses pacotes funcionam nos bastidores, essa [postagem no blog](https://apmtips.com/posts/2016-11-18-how-application-insights-status-monitor-not-monitors-dependencies/) de um de nossos desenvolvedores é um bom começo.

## <a name="additional-packages"></a>Pacotes adicionais

| Nome do Pacote | Versão estável | Descrição | Baixar |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.AzureWebSites | 2.6.5 | Essa extensão permite o monitoramento do Application Insights em um Serviço de Aplicativo do Azure. SDK versão 2.6.1. Instruções: adicione as configurações de aplicativo 'APPINSIGHTS_INSTRUMENTATIONKEY' com o seu ikey e reinicie o webapp para ter um efeito.| [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AzureWebSites/) |
| Microsoft.ApplicationInsights.Injector | 2.6.7 | Este pacote contém os arquivos necessários para a injeção do Application Insights sem código | [Baixar Pacote](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Injector/) |

## <a name="next-steps"></a>Próximas etapas

- Monitore o [ASP.NET Core](../../azure-monitor/app/asp-net-core.md).
- Crie o perfil de [aplicativos Web ASP.NET Core Azure Linux](../../azure-monitor/app/profiler-aspnetcore-linux.md).
- Depure [instantâneos](../../azure-monitor/app/snapshot-debugger.md) do ASP.NET.
