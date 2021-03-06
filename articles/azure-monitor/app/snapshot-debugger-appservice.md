---
title: Habilitar Depurador de Instantâneos para aplicativos .NET no serviço Azure App | Microsoft Docs
description: Habilitar Depurador de Instantâneos para aplicativos .NET no serviço Azure App
ms.topic: conceptual
author: brahmnes
ms.author: bfung
ms.date: 03/26/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 6928da704236c4bb5492f99a4a5327bf297a323d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84676828"
---
# <a name="enable-snapshot-debugger-for-net-apps-in-azure-app-service"></a>Habilitar Depurador de Instantâneos para aplicativos .NET no serviço Azure App

Depurador de Instantâneos atualmente trabalha para aplicativos ASP.NET e ASP.NET Core que estão em execução no serviço Azure App nos planos de serviço do Windows.

## <a name="enable-snapshot-debugger"></a><a id="installation"></a>Habilitar Depurador de Instantâneos
Para habilitar Depurador de Instantâneos para um aplicativo, siga as instruções abaixo. Se você estiver executando um tipo diferente de serviço do Azure, aqui estão as instruções para habilitar Depurador de Instantâneos em outras plataformas com suporte:
* [Serviços de Nuvem do Azure](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Serviços de Service Fabric do Azure](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Máquinas Virtuais do Azure e Conjuntos de Dimensionamento de Máquinas Virtuais](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Máquinas virtuais ou físicas locais](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

Se você estiver usando uma versão de visualização do .NET Core, siga as instruções para [habilitar depurador de instantâneos para outros ambientes](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json) primeiro para incluir o pacote NuGet [Microsoft. ApplicationInsights. SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) com o aplicativo e, em seguida, conclua o restante das instruções abaixo. 

Application Insights Depurador de Instantâneos é pré-instalado como parte do tempo de execução dos serviços de aplicativo, mas você precisa ativá-lo para obter instantâneos para seu aplicativo do serviço de aplicativo. Depois de implantar um aplicativo, mesmo que você tenha incluído o SDK do Application Insights no código-fonte, siga as etapas abaixo para habilitar o depurador de instantâneos.

1. Navegue até o painel de controle do Azure para seu serviço de aplicativo.
2. Vá para a página **configurações > Application insights** .

   ![Habilitar Insights de Aplicativo no portal de Serviços de Aplicativos](./media/snapshot-debugger/applicationinsights-appservices.png)

3. Siga as instruções na página para criar um novo recurso ou selecione um recurso existente do App insights para monitorar seu aplicativo. Verifique também se ambas as opções de Depurador de Instantâneos estão **ativadas**.

   ![Adicione a extensão de site do App Insights][Enablement UI]

4. Depurador de Instantâneos agora está habilitado usando uma configuração de aplicativo dos serviços de aplicativo.

    ![Configuração de aplicativo para Depurador de Instantâneos][snapshot-debugger-app-setting]

## <a name="disable-snapshot-debugger"></a>Desabilitar Depurador de Instantâneos

Siga as mesmas etapas necessárias para **habilitar o depurador de instantâneos**, mas alterne ambos os comutadores para depurador de instantâneos para **desativado**.
É recomendável que você tenha Depurador de Instantâneos habilitado em todos os seus aplicativos para facilitar o diagnóstico de exceções de aplicativo.

## <a name="azure-resource-manager-template"></a>Modelo do Azure Resource Manager

Para um serviço de Azure App, você pode definir configurações de aplicativo em um modelo de Azure Resource Manager para habilitar o Depurador de Instantâneos e o profiler. Você adiciona um recurso de configuração que contém as configurações do aplicativo como um recurso filho do site:

```json
{
  "apiVersion": "2015-08-01",
  "name": "[parameters('webSiteName')]",
  "type": "Microsoft.Web/sites",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  "tags": { 
    "[concat('hidden-related:', resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName')))]": "empty",
    "displayName": "Website"
  },
  "properties": {
    "name": "[parameters('webSiteName')]",
    "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
  },
  "resources": [
    {
      "apiVersion": "2015-08-01",
      "name": "appsettings",
      "type": "config",
      "dependsOn": [
        "[parameters('webSiteName')]",
        "[concat('AppInsights', parameters('webSiteName'))]"
      ],
      "properties": {
        "APPINSIGHTS_INSTRUMENTATIONKEY": "[reference(resourceId('Microsoft.Insights/components', concat('AppInsights', parameters('webSiteName'))), '2014-04-01').InstrumentationKey]",
        "APPINSIGHTS_PROFILERFEATURE_VERSION": "1.0.0",
        "APPINSIGHTS_SNAPSHOTFEATURE_VERSION": "1.0.0",
        "DiagnosticServices_EXTENSION_VERSION": "~3",
        "ApplicationInsightsAgent_EXTENSION_VERSION": "~2"
      }
    }
  ]
},
```

## <a name="next-steps"></a>Próximas etapas

- Gere o tráfego para o aplicativo que pode disparar uma exceção. Em seguida, aguarde de 10 a 15 minutos para que os instantâneos sejam enviados para a instância de Application Insights.
- Consulte [instantâneos](snapshot-debugger.md?toc=/azure/azure-monitor/toc.json#view-snapshots-in-the-portal) na portal do Azure.
- Para obter ajuda com a solução de problemas de Depurador de Instantâneos, consulte [depurador de instantâneos solução de problemas](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json).

[Enablement UI]: ./media/snapshot-debugger/enablement-ui.png
[snapshot-debugger-app-setting]:./media/snapshot-debugger/snapshot-debugger-app-setting.png

