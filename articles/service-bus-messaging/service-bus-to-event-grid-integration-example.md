---
title: 'Tutorial: Exemplos de integração do Barramento de Serviço do Azure com a Grade de Eventos'
description: 'Tutorial: Este artigo oferece exemplos do sistema de mensagens do Barramento de Serviço e integração da Grade de Eventos.'
documentationcenter: .net
author: spelluru
ms.topic: tutorial
ms.date: 06/23/2020
ms.author: spelluru
ms.openlocfilehash: 8f947489c2298e580ae455763709df1734687130
ms.sourcegitcommit: 61d92af1d24510c0cc80afb1aebdc46180997c69
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85337055"
---
# <a name="tutorial-respond-to-azure-service-bus-events-received-via-azure-event-grid-by-using-azure-functions-and-azure-logic-apps"></a>Tutorial: Responder a eventos do Barramento de Serviço do Azure recebidos por meio da Grade de Eventos do Azure usando o Azure Functions e os Aplicativos Lógicos do Azure
Neste tutorial, você aprenderá a responder a eventos do Barramento de Serviço do Azure que são recebidos por meio da Grade de Eventos do Azure usando o Azure Functions e os Aplicativos Lógicos do Azure. 

Neste tutorial, você aprenderá como:
> [!div class="checklist"]
> * Criar um namespace do Barramento de Serviço
> * Preparar um aplicativo de exemplo para enviar as mensagens
> * Enviar mensagens para o tópico do Barramento de Serviço
> * Receber mensagens usando os Aplicativos Lógicos
> * Configurar uma função de teste no Azure
> * Conecte a função e o namespace através da Grade de Eventos
> * Receber mensagens usando o Azure Functions

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, verifique se você tem o seguinte instalado:

- [Atualização 3 do Visual Studio 2017 (versão 15.3, 26730.01)](https://www.visualstudio.com/vs) ou posterior.
- [NET Core SDK](https://www.microsoft.com/net/download/windows), versão 2.0 ou posterior.

## <a name="create-a-service-bus-namespace"></a>Criar um namespace do Barramento de Serviço
Siga as instruções deste tutorial: [Início Rápido: Usar o portal do Azure para criar um tópico do Barramento de Serviço e assinaturas para o tópico](service-bus-quickstart-topics-subscriptions-portal.md) para executar as seguintes tarefas:

- Criar um namespace **premium** do Barramento de Serviço. 
- Obter a cadeia de conexão. 
- Criar um tópico de Barramento de Serviço.
- Criar duas assinaturas do tópico. 

## <a name="prepare-a-sample-application-to-send-messages"></a>Preparar um aplicativo de exemplo para enviar as mensagens
Você pode usar qualquer método para enviar uma mensagem para o tópico do Barramento de Serviço. O código de exemplo no fim deste procedimento pressupõe que você está usando o Visual Studio 2017.

1. Clone [o repositório do barramento de serviço do azure GitHub](https://github.com/Azure/azure-service-bus/).
2. No Visual Studio, vá para a pasta *\samples\DotNet\Microsoft.ServiceBus.Messaging\ServiceBusEventGridIntegration* e, em seguida, abra o arquivo *SBEventGridIntegration.sln*.
3. Vá para o projeto **MessageSender** e, em seguida, selecione **Program.cs**.
4. Insira o nome do tópico do Barramento de Serviço e a cadeia de conexão que você obteve na etapa anterior:

    ```csharp
    const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
    const string TopicName = "YOUR TOPIC NAME";
    ```
5. Atualize o valor `numberOfMessages` para **5**. 
5. Compile e execute o programa para enviar mensagens de teste para o tópico do Barramento de Serviço. 

## <a name="receive-messages-by-using-logic-apps"></a>Receber mensagens usando os Aplicativos Lógicos
Conecte um aplicativo lógico com o Barramento de Serviço do Azure e a Grade de Eventos do Azure seguindo as etapas abaixo:

1. No portal do Azure, crie um aplicativo lógico.
    1. Selecione **+ Criar um recurso**, selecione **Integração** e, em seguida, selecione **Aplicativo Lógico**. 
    2. Na página **Aplicativo Lógico – Criar**, insira um **nome** para o aplicativo lógico.
    3. Selecione sua **assinatura**do Azure. 
    4. Selecione **Usar existente** para o **Grupo de recursos** e selecione o grupo de recursos que você usou para outros recursos (como a função do Azure, o namespace do Barramento de Serviço) criados anteriormente. 
    5. Selecione o **Local** para o aplicativo lógico. 
    6. Selecione **Criar** para criar o aplicativo lógico. 
2. Na página **Designer de Aplicativos Lógicos**, selecione **Aplicativo Lógico em Branco** em **Modelos**. 
3. No designer, siga as seguintes etapas:
    1. Pesquise **Grade de Eventos**. 
    2. Selecione **Quando ocorrer um evento de recurso – Grade de Eventos do Azure**. 

        ![Designer de Aplicativos Lógicos – selecione o gatilho da Grade de Eventos](./media/service-bus-to-event-grid-integration-example/logic-apps-event-grid-trigger.png)
4. Selecione **Entrar**, insira suas credenciais do Azure e selecione **Permitir o acesso**. 
5. Na página **Quando ocorrer um evento de recurso**, siga as seguintes etapas:
    1. Selecione sua assinatura do Azure. 
    2. Para **Tipo de recurso**, selecione **Microsoft.ServiceBus.Namespaces**. 
    3. Para **Nome de recurso**, selecione seu namespace do Barramento de Serviço. 
    4. Selecione **Adicionar novo parâmetro** e selecione **Filtro de sufixo**. 
    5. Para **Filtro de sufixo**, insira o nome da sua segunda assinatura do tópico do Barramento de Serviço. 
        ![Designer de Aplicativos Lógicos – configurar eventos](./media/service-bus-to-event-grid-integration-example/logic-app-configure-event.png)
6. Selecione **+ Nova Etapa** no designer e siga as etapas abaixo:
    1. Pesquise **Barramento de Serviço**.
    2. Selecione **Barramento de Serviço** na lista. 
    3. Selecione **Receber mensagens** na lista **Ações**. 
    4. Selecione **Receber mensagens de uma assinatura do tópico (bloqueio de inspeção)** . 

        ![Designer de Aplicativos Lógicos – ação de recebimento de mensagens](./media/service-bus-to-event-grid-integration-example/service-bus-get-messages-step.png)
    5. Insira um **nome para a conexão**. Por exemplo:  **Receber mensagens da assinatura do tópico** e selecione o namespace do Barramento de Serviço. 

        ![Designer de Aplicativos Lógicos – selecione o namespace do Barramento de Serviço](./media/service-bus-to-event-grid-integration-example/logic-apps-select-namespace.png) 
    6. Selecione **RootManageSharedAccessKey** e selecione **Criar**.

        ![Designer de Aplicativos Lógicos – selecione a chave de acesso compartilhada](./media/service-bus-to-event-grid-integration-example/logic-app-shared-access-key.png) 
    8. Selecione seu **tópico** e sua **assinatura**. 
    
        ![Designer de Aplicativos Lógicos – selecione a assinatura e o tópico do Barramento de Serviço](./media/service-bus-to-event-grid-integration-example/logic-app-select-topic-subscription.png)
7. Selecione **+ Nova Etapa** e siga as etapas abaixo: 
    1. Selecione **Barramento de Serviço**.
    2. Selecione **Concluir a mensagem em uma assinatura de tópico** na lista de ações. 
    3. Selecione seu **tópico** do Barramento de Serviço.
    4. Selecione a segunda **assinatura** do tópico.
    5. Para **Lock token da mensagem**, selecione **Lock token** em **Conteúdo dinâmico**. 

        ![Designer de Aplicativos Lógicos – selecione a assinatura e o tópico do Barramento de Serviço](./media/service-bus-to-event-grid-integration-example/logic-app-complete-message.png)
8. Selecione **Salvar** na barra de ferramentas do Designer de Aplicativos Lógicos para salvar o aplicativo lógico. 
9. Siga as instruções da seção [Enviar mensagens para o tópico do Barramento de Serviço](#send-messages-to-the-service-bus-topic) para enviar mensagens ao tópico. 
10. Alterne para a página **Visão geral** do seu aplicativo lógico. Você verá as execuções do aplicativo lógico no **Histórico de execuções** das mensagens enviadas.

    ![Designer de Aplicativos Lógicos – execuções de aplicativo lógico](./media/service-bus-to-event-grid-integration-example/logic-app-runs.png)

## <a name="set-up-a-test-function-on-azure"></a>Configurar uma função de teste no Azure 
Antes de trabalhar com todo o cenário, configure pelo menos uma função de teste pequena, que pode ser usada para depurar e observar quais eventos estão fluindo. Siga as instruções do artigo [Criar sua primeira função no portal do Azure](../azure-functions/functions-create-first-azure-function.md) para executar as seguintes tarefas: 

1. Criar um aplicativo de funções.
2. Criar uma função disparada por HTTP. 

Depois, execute as etapas a seguir: 


# <a name="azure-functions-v2"></a>[Azure Functions V2](#tab/v2)

1. Expanda as **Funções** no modo de exibição de árvore e selecione sua função. Substitua o código da função pelo código a seguir: 

    ```csharp
    #r "Newtonsoft.Json"
    
    using System.Net;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;
    
    public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");
        var content = req.Body;
        string jsonContent = await new StreamReader(content).ReadToEndAsync();
        log.LogInformation($"Received Event with payload: {jsonContent}");
    
        IEnumerable<string> headerValues;
        headerValues = req.Headers.GetCommaSeparatedValues("Aeg-Event-Type");
    
        if (headerValues.Count() != 0)
        {
            var validationHeaderValue = headerValues.FirstOrDefault();
            if(validationHeaderValue == "SubscriptionValidation")
            {
                log.LogInformation("Validating the subscription");            
                var events = JsonConvert.DeserializeObject<GridEvent[]>(jsonContent);
                var code = events[0].Data["validationCode"];
                log.LogInformation($"Validation code: {code}");
                return (ActionResult) new OkObjectResult(new { validationResponse = code });
            }
        }
    
        return jsonContent == null
            ? new BadRequestObjectResult("Please pass a name on the query string or in the request body")
            : (ActionResult)new OkObjectResult($"Hello, {jsonContent}");
    }
    
    public class GridEvent
    {
        public string Id { get; set; }
        public string EventType { get; set; }
        public string Subject { get; set; }
        public DateTime EventTime { get; set; }
        public Dictionary<string, string> Data { get; set; }
        public string Topic { get; set; }
    }    
    ```
2. Selecione **Salvar** na barra de ferramentas para salvar o código da função.

    ![Salvar código da função](./media/service-bus-to-event-grid-integration-example/save-function-code.png)
3. Selecione **Testar/Executar** na barra de ferramentas e execute as seguintes etapas: 
    1. Insira o seguinte JSON no **corpo**.

        ```json
        [{
          "id": "64ba80ae-9f8e-425f-8bd7-d88d2c0ba3e3",
          "topic": "/subscriptions/0000000000-0000-0000-0000-0000000000000/resourceGroups/spegridsbusrg/providers/Microsoft.ServiceBus/namespaces/spegridsbusns",
          "subject": "",
          "data": {
            "validationCode": "D7D825D4-BD04-4F73-BDE3-70666B149857",
            "validationUrl": "https://rp-eastus.eventgrid.azure.net:553/eventsubscriptions/spsbusegridsubscription/validate?id=D7D825D4-BD04-4F73-BDE3-70666B149857&t=2020-06-09T18:28:51.5724615Z&apiVersion=2020-04-01-preview&[Hidden Credential]"
          },
          "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
          "eventTime": "2020-06-09T18:28:51.5724615Z",
          "metadataVersion": "1",
          "dataVersion": "2"
        }]
        ```    
    2. Clique em **Adicionar cabeçalho** e adicione um cabeçalho com o nome `aeg-event-type` e o valor `SubscriptionValidation`. 
    3. Selecione **Executar**. 

        ![Execução de teste](./media/service-bus-to-event-grid-integration-example/test-run-function.png)
    4. Confirme se você vê o código de status de retorno de **OK** e o código de validação no corpo da resposta. Além disso, consulte as informações registradas pela função. 

        ![Execução de teste – resposta](./media/service-bus-to-event-grid-integration-example/test-function-response.png)        
3. Selecione **Obter a URL da função** e anote a URL. 

    ![Obter URL de função](./media/service-bus-to-event-grid-integration-example/get-function-url.png)
5. Selecione o botão **Copiar** ao lado do texto da URL.    
    ![Copiar URL de função](./media/service-bus-to-event-grid-integration-example/get-function-url-copy.png)

# <a name="azure-functions-v1"></a>[Azure Functions V1](#tab/v1)

1. Configurar a função para usar a versão **V1**: 
    1. Selecione seu aplicativo de funções no modo de exibição de árvore e selecione **Configurações do aplicativo de funções**. 
    2. Selecione **~1** para **Versão de runtime**. 
2. Expanda as **Funções** no modo de exibição de árvore e selecione sua função. Substitua o código da função pelo código a seguir: 

    ```csharp
    #r "Newtonsoft.Json"
    using System.Net;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        log.Info("C# HTTP trigger function processed a request.");
        // parse query parameter
        var content = req.Content;
    
        string jsonContent = await content.ReadAsStringAsync(); 
        log.Info($"Received Event with payload: {jsonContent}");
    
        IEnumerable<string> headerValues;
        if (req.Headers.TryGetValues("Aeg-Event-Type", out headerValues))
        {
            var validationHeaderValue = headerValues.FirstOrDefault();
            if(validationHeaderValue == "SubscriptionValidation")
            {
            var events = JsonConvert.DeserializeObject<GridEvent[]>(jsonContent);
                 var code = events[0].Data["validationCode"];
                 return req.CreateResponse(HttpStatusCode.OK,
                 new { validationResponse = code });
            }
        }
    
        return jsonContent == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + jsonContent);
    }
    
    public class GridEvent
    {
        public string Id { get; set; }
        public string EventType { get; set; }
        public string Subject { get; set; }
        public DateTime EventTime { get; set; }
        public Dictionary<string, string> Data { get; set; }
        public string Topic { get; set; }
    }
    ```
4. Selecione **Salvar e executar**.

    ![Saída do aplicativo de funções](./media/service-bus-to-event-grid-integration-example/function-run-output.png)
4. Selecione **Obter URL da função** na barra de ferramentas. 

    ![Obter URL de função](./media/service-bus-to-event-grid-integration-example/get-function-url.png)
5. Selecione o botão **Copiar** ao lado do texto da URL.    
    ![Copiar URL de função](./media/service-bus-to-event-grid-integration-example/get-function-url-copy.png)

---

## <a name="connect-the-function-and-namespace-via-event-grid"></a>Conecte a função e o namespace através da Grade de Eventos
Nesta seção, você une a função e o namespace do Barramento de Serviço usando o portal do Azure. 

Para criar uma assinatura da Grade de Eventos do Azure, siga as etapas a seguir:

1. No portal do Azure, vá para seu namespace e, em seguida, no painel esquerdo, selecione **Eventos**. A janela do namespace é aberta, com duas assinaturas da Grade de Eventos exibidas no painel direito. 
    
    ![Barramento de Serviço – página de eventos](./media/service-bus-to-event-grid-integration-example/service-bus-events-page.png)
2. Selecione **+ Assinatura de Evento** na barra de ferramentas. 
3. Na página **Criar Assinatura de Eventos**, faça o seguinte:
    1. Insira um **nome** para a assinatura. 
    2. Insira um **nome** para o **tópico do sistema**. Os tópicos do sistema são tópicos criados para recursos do Azure, como a conta do Armazenamento do Azure e o Barramento de Serviço do Azure. Para saber mais sobre os tópicos do sistema, confira [Visão geral dos tópicos do sistema](../event-grid/system-topics.md).
    2. Selecione **Web Hook** para o **Tipo de ponto de extremidade**. 

        ![Barramento de Serviço – Assinatura da Grade de Eventos](./media/service-bus-to-event-grid-integration-example/event-grid-subscription-page.png)
    3. Escolha **Selecionar um ponto de extremidade**, cole a URL da função e escolha **Confirmar seleção**. 

        ![Função – selecionar o ponto de extremidade](./media/service-bus-to-event-grid-integration-example/function-select-endpoint.png)
    4. Alterne para a guia **Filtros** e execute as seguintes tarefas:
        1. Selecione **Habilitar filtragem por assunto**
        2. Insira o nome da **primeira assinatura** do tópico do Barramento de Serviço criado anteriormente.
        3. Selecione o botão **Criar**. 

            ![Filtro de assinatura de eventos](./media/service-bus-to-event-grid-integration-example/event-subscription-filter.png)
4. Alterne para a guia **Assinaturas de Evento** da página **Eventos** e confirme se você vê a assinatura de evento na lista.

    ![Assinatura de eventos na lista](./media/service-bus-to-event-grid-integration-example/event-subscription-in-list.png)

## <a name="send-messages-to-the-service-bus-topic"></a>Enviar mensagens para o tópico do Barramento de Serviço
1. Execute o aplicativo C# .NET, que envia mensagens para o tópico do Barramento de Serviço. 

    ![Saída do aplicativo de console](./media/service-bus-to-event-grid-integration-example/console-app-output.png)
1. Na página do aplicativo de funções do Azure, alterne para a guia **Monitor** na guia **Código + Teste**. Você deverá ver uma entrada para cada mensagem postada no tópico do Barramento de Serviço. Se você não as vir, atualize a página depois de aguardar alguns minutos. 

    ![Função Monitor](./media/service-bus-to-event-grid-integration-example/function-monitor.png)

    Você também pode usar a guia **Logs** da página **Monitor** para ver as informações de registro em log conforme as mensagens são enviadas. Pode haver algum atraso, então aguarde alguns minutos para ver as mensagens registradas. 

## <a name="receive-messages-by-using-azure-functions"></a>Receber mensagens usando o Azure Functions
Na seção anterior, você examinou um teste simples e o cenário de depuração e verificou se os eventos estão fluindo. 

Nesta seção, você aprenderá como receber e processar mensagens depois de receber um evento.

### <a name="publish-a-function-from-visual-studio"></a>Publicar uma função do Visual Studio
1. Na mesma solução do Visual Studio (**SBEventGridIntegration**) que você abriu, selecione **ReceiveMessagesOnEvent.cs** no projeto **SBEventGridIntegration**. 
2. Insira sua cadeia de conexão do Barramento de Serviço no código a seguir:

    ```Csharp
    const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
    ```
3. Baixar o **perfil de publicação** para a função:
    1. Selecione seu aplicativo de funções. 
    2. Selecione a guia **Visão geral** se ainda não estiver selecionada. 
    3. Selecione **Obter perfil de publicação** na barra de ferramentas. 

        ![Obter o perfil de publicação para a função](./media/service-bus-to-event-grid-integration-example/function-download-publish-profile.png)
    4. Salve o arquivo na pasta do projeto. 
4. No Visual Studio, clique com o botão direito do mouse em **SBEventGridIntegration** e, em seguida, selecione **Publicar**. 
5. Em **Publicar**, execute as seguintes etapas: 
    1. Selecione **Iniciar** na página **Publicar** 
    2. Em **Destino**, selecione **Importar Perfil**. 
    3. Selecione **Avançar**. 

        ![Visual Studio – botão Importar perfil](./media/service-bus-to-event-grid-integration-example/visual-studio-import-profile-button.png)
7. Selecione o **arquivo de perfil de publicação** baixado anteriormente e selecione **Concluir**.

    ![Selecionar o perfil de publicação](./media/service-bus-to-event-grid-integration-example/select-publish-profile.png)
8. Selecione **Publicar** na página **Publicar**. 

    ![Visual Studio – Publicar](./media/service-bus-to-event-grid-integration-example/select-publish.png)
9. Confirme se está vendo a nova função do Azure **ReceiveMessagesOnEvent**. Se necessário, atualize a página. 

    ![Verifique se a nova função foi criada](./media/service-bus-to-event-grid-integration-example/function-receive-messages.png)
10. Obtenha a URL da nova função e anote. 

### <a name="event-grid-subscription"></a>Assinatura de Grade de Eventos

1. Exclua a assinatura de Grade de Eventos existente:
    1. Na página **Namespace de Barramento de Serviço**, selecione **Eventos** no menu à esquerda. 
    2. Mude para a guia **Assinaturas de Evento**. 
    2. Selecione a assinatura de eventos existente. 

        ![Selecione a assinatura do evento](./media/service-bus-to-event-grid-integration-example/select-event-subscription.png)
    3. Na página **Assinatura de Evento**, selecione **Excluir**. Clique em **Sim** para confirmar a exclusão. 
        ![Botão Excluir assinatura de evento](./media/service-bus-to-event-grid-integration-example/delete-subscription-button.png)
2. Siga as instruções da seção [Conectar a função e o namespace usando a Grade de Eventos](#connect-the-function-and-namespace-via-event-grid) para criar uma assinatura de Grade de Eventos usando a nova URL de função.
3. Siga as instruções da seção [Enviar mensagens para o tópico do Barramento de Serviço](#send-messages-to-the-service-bus-topic) para enviar mensagens ao tópico e monitorar a função. 


## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre a [Grade de Eventos do Azure](https://docs.microsoft.com/azure/event-grid/).
* Saiba mais sobre o [Azure Functions](https://docs.microsoft.com/azure/azure-functions/).
* Saiba mais sobre o [recurso de Aplicativos Lógicos do Serviço de Aplicativo do Azure](https://docs.microsoft.com/azure/logic-apps/).
* Saiba mais sobre o [Barramento de Serviço do Azure](https://docs.microsoft.com/azure/service-bus/).


[2]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid2.png
[3]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid3.png
[7]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid7.png
[8]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid8.png
[9]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid9.png
[10]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid10.png
[11]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid11.png
[12]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12.png
[12-1]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12-1.png
[12-2]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12-2.png
[13]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid13.png
[14]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid14.png
[15]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid15.png
[16]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid16.png
[17]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid17.png
[18]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid18.png
[20]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal.png
[21]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal2.png
