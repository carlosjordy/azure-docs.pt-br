---
title: 'Início Rápido: Enviar uma solicitação de pesquisa para a API REST usando o C# – Pesquisa de Entidade do Bing'
titleSuffix: Azure Cognitive Services
description: Use este Início Rápido para enviar solicitações para a API REST de Pesquisa de Entidade do Bing usando C# e receba uma resposta JSON.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 05/08/2020
ms.author: aahi
ms.openlocfilehash: cd89f2ae13b10c83c3fc22023fc2e3cae1770c98
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83650283"
---
# <a name="quickstart-send-a-search-request-to-the-bing-entity-search-rest-api-using-c"></a>Início Rápido: Enviar uma solicitação de pesquisa para a API REST da Pesquisa de Entidade do Bing usando o C#

Use este início rápido para fazer sua primeira chamada à API de Pesquisa de Entidade do Bing e exibir a resposta JSON. Este aplicativo C# simples envia uma consulta de pesquisa de notícias para a API e exibe a resposta. O código-fonte desse aplicativo está disponível no [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Search/BingEntitySearchv7.cs).

Embora esse aplicativo seja escrito em C#, a API é um serviço Web RESTful compatível com a maioria das linguagens de programação.


## <a name="prerequisites"></a>Pré-requisitos

- Qualquer edição do [Visual Studio 2017 ou posterior](https://www.visualstudio.com/downloads/).

- A estrutura [Json.NET](https://www.newtonsoft.com/json), disponível como um pacote do NuGet. Para instalar o pacote NuGet no Visual Studio:

   1. Clique com o botão direito do mouse no projeto no **Gerenciador de Soluções**.
   2. Selecione **Gerenciar Pacotes NuGet**.
   3. Pesquise e selecione *Newtonsoft.Json* e, em seguida, instale o pacote.

- Se você estiver usando Linux/MacOS, este aplicativo poderá ser executado usando [Mono](https://www.mono-project.com/).


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Criar e inicializar um projeto

1. Crie uma solução de console C# no Visual Studio. Então, adicione os seguintes namespaces no arquivo de código principal:
    
    ```csharp
    using Newtonsoft.Json;
    using System;
    using System.Net.Http;
    using System.Text;
    ```

2. Crie uma classe e adicione variáveis para o ponto de extremidade da API, sua chave de assinatura e a consulta que você deseja pesquisar. É possível usar o ponto de extremidade global no código a seguir ou o ponto de extremidade do [subdomínio personalizado](../../../cognitive-services/cognitive-services-custom-subdomains.md) exibido no portal do Azure para seu recurso.

    ```csharp
    namespace EntitySearchSample
    {
        class Program
        {
            static string host = "https://api.cognitive.microsoft.com";
            static string path = "/bing/v7.0/entities";
    
            static string market = "en-US";
    
            // NOTE: Replace this example key with a valid subscription key.
            static string key = "ENTER YOUR KEY HERE";
    
            static string query = "italian restaurant near me";
        //...
        }
    }
    ```

## <a name="send-a-request-and-get-the-api-response"></a>Enviar uma solicitação e obter a resposta da API

1. Na classe, crie uma função chamada `Search()`. Dentro desta função, crie um objeto `HttpClient` e adicione a chave de assinatura ao cabeçalho `Ocp-Apim-Subscription-Key`.

2. Construa o URI da sua solicitação combinando o host e o caminho. Em seguida, adicione o mercado e codifique a consulta como URL.

3. Aguarde `client.GetAsync()` para obter uma resposta HTTP e, em seguida, armazene a resposta JSON aguardando `ReadAsStringAsync()`.

4. Formate a cadeia de caracteres JSON com `JsonConvert.DeserializeObject()` e imprima-a no console.

      ```csharp
      async static void Search()
      {
       //...
       HttpClient client = new HttpClient();
       client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);

       string uri = host + path + "?mkt=" + market + "&q=" + System.Net.WebUtility.UrlEncode(query);

       HttpResponseMessage response = await client.GetAsync(uri);

       string contentString = await response.Content.ReadAsStringAsync();
       dynamic parsedJson = JsonConvert.DeserializeObject(contentString);
       Console.WriteLine(parsedJson);
      }
      ```

5. No método `Main()` do aplicativo, chame a função `Search()`.
    
    ```csharp
    static void Main(string[] args)
    {
        Search();
        Console.ReadLine();
    }
    ```


## <a name="example-json-response"></a>Resposta JSON de exemplo

Uma resposta com êxito é retornada em JSON, conforme mostrado no seguinte exemplo: 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "italian restaurant near me",
    "askUserForLocation": true
  },
  "places": {
    "value": [
      {
        "_type": "LocalBusiness",
        "webSearchUrl": "https://www.bing.com/search?q=sinful+bakery&filters=local...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "https://www.contoso.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98112",
          "addressCountry": "US",
          "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
      },

      . . .
      {
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Pickles+and+Preserves...",
        "name": "Munson's Pickles and Preserves Farm",
        "url": "https://www.princi.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness",
            "Restaurant"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98101",
          "addressCountry": "US",
          "neighborhood": "Capitol Hill"
        },
        "telephone": "(800) 555-1212"
      },
      
      . . .
    ]
  }
}
```

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Criar um aplicativo Web de página única](../tutorial-bing-entities-search-single-page-app.md)

* [O que é a API de Pesquisa de Entidade do Bing?](../overview.md )
* [Referência da API de Pesquisa de Entidade do Bing](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference).
