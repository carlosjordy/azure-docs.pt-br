---
title: Usar o módulo de serviços do Azure Maps | Mapas do Microsoft Azure
description: Neste artigo, você aprenderá a utilizar o Microsoft Azure mapeia os serviços REST usando o módulo de serviços do Azure Maps.
author: rbrundritt
ms.author: richbrun
ms.date: 03/25/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.openlocfilehash: 6e9d1f35d021c3381f9c2887dfb1c150bb720871
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80804649"
---
# <a name="use-the-azure-maps-services-module"></a>Usar o módulo de serviços do Azure Maps

O SDK da Web do Azure Maps fornece um *módulo de serviços*. Esse módulo é uma biblioteca auxiliar que torna mais fácil usar os serviços REST do Azure Maps em aplicativos Web ou Node.js usando JavaScript ou TypeScript.

## <a name="use-the-services-module-in-a-webpage"></a>Usar o módulo serviços em uma página da Web

1. Criar um novo arquivo HTML.
1. Carregue o módulo serviços do Azure Maps. Você pode carregá-lo de uma das duas maneiras:
    - Use a versão de rede de distribuição de conteúdo do Azure hospedada globalmente do módulo serviços do Azure Maps. Adicione uma referência de script ao `<head>` elemento do arquivo:

        ```html
        <script src="https://atlas.microsoft.com/sdk/javascript/service/2/atlas-service.min.js"></script>
        ```

    - Como alternativa, carregue o módulo de serviços para o código-fonte do SDK Web do Azure Maps localmente usando o pacote NPM [do Azure-Maps-REST](https://www.npmjs.com/package/azure-maps-rest) e, em seguida, hospede-o com seu aplicativo. Esse pacote também inclui definições de TypeScript. Use este comando:
    
        > **npm install azure-maps-rest**
    
        Em seguida, adicione uma referência de script ao `<head>` elemento do arquivo:

         ```html
        <script src="node_modules/azure-maps-rest/dist/atlas-service.min.js"></script>
         ```

1. Crie um pipeline de autenticação. O pipeline deve ser criado antes de você poder inicializar um ponto de extremidade de cliente de URL de serviço. Use suas próprias credenciais de chave de conta do Azure Maps ou Azure Active Directory (Azure AD) para autenticar um cliente de serviço de pesquisa do Azure Maps. Neste exemplo, o cliente da URL do serviço de pesquisa será criado. 

    Se você usar uma chave de assinatura para autenticação:

    ```javascript
    // Get an Azure Maps key at https://azure.com/maps.
    var subscriptionKey = '<Your Azure Maps Key>';

    // Use SubscriptionKeyCredential with a subscription key.
    var subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(subscriptionKey);

    // Use subscriptionKeyCredential to create a pipeline.
    var pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential, {
      retryOptions: { maxTries: 4 } // Retry options
    });

    // Create an instance of the SearchURL client.
    var searchURL = new atlas.service.SearchURL(pipeline);
    ```

    Se você usar o Azure AD para autenticação:

    ```javascript
    // Enter your Azure AD client ID.
    var clientId = "<Your Azure Active Directory Client Id>";

    // Use TokenCredential with OAuth token (Azure AD or Anonymous).
    var aadToken = await getAadToken();
    var tokenCredential = new atlas.service.TokenCredential(clientId, aadToken);

    // Create a repeating time-out that will renew the Azure AD token.
    // This time-out must be cleared when the TokenCredential object is no longer needed.
    // If the time-out is not cleared, the memory used by the TokenCredential will never be reclaimed.
    var renewToken = async () => {
      try {
        console.log("Renewing token");
        var token = await getAadToken();
        tokenCredential.token = token;
        tokenRenewalTimer = setTimeout(renewToken, getExpiration(token));
      } catch (error) {
        console.log("Caught error when renewing token");
        clearTimeout(tokenRenewalTimer);
        throw error;
      }
    }
    tokenRenewalTimer = setTimeout(renewToken, getExpiration(aadToken));

    // Use tokenCredential to create a pipeline.
    var pipeline = atlas.service.MapsURL.newPipeline(tokenCredential, {
      retryOptions: { maxTries: 4 } // Retry options
    });

    // Create an instance of the SearchURL client.
    var searchURL = new atlas.service.SearchURL(pipeline);

    function getAadToken() {
      // Use the signed-in auth context to get a token.
      return new Promise((resolve, reject) => {
        // The resource should always be https://atlas.microsoft.com/.
        const resource = "https://atlas.microsoft.com/";
        authContext.acquireToken(resource, (error, token) => {
          if (error) {
            reject(error);
          } else {
            resolve(token);
          }
        });
      })
    }

    function getExpiration(jwtToken) {
      // Decode the JSON Web Token (JWT) to get the expiration time stamp.
      const json = atob(jwtToken.split(".")[1]);
      const decode = JSON.parse(json);

      // Return the milliseconds remaining until the token must be renewed.
      // Reduce the time until renewal by 5 minutes to avoid using an expired token.
      // The exp property is the time stamp of the expiration, in seconds.
      const renewSkew = 300000;
      return (1000 * decode.exp) - Date.now() - renewSkew;
    }
    ```

    Para obter mais informações, consulte [autenticação com o Azure Maps](azure-maps-authentication.md).

1. O código a seguir usa o cliente URL do serviço de pesquisa do Azure Maps recém-criado para geocodificar um endereço: "1 Microsoft Way, Redmond, WA". O código usa a `searchAddress` função e exibe os resultados como uma tabela no corpo da página.

    ```javascript
    // Search for "1 microsoft way, redmond, wa".
    searchURL.searchAddress(atlas.service.Aborter.timeout(10000), '1 microsoft way, redmond, wa')
      .then(response => {
        var html = [];

        // Display the total results.
        html.push('Total results: ', response.summary.numResults, '<br/><br/>');

        // Create a table of the results.
        html.push('<table><tr><td></td><td>Result</td><td>Latitude</td><td>Longitude</td></tr>');

        for(var i=0;i<response.results.length;i++){
          html.push('<tr><td>', (i+1), '.</td><td>', 
            response.results[i].address.freeformAddress, 
            '</td><td>', 
            response.results[i].position.lat,
            '</td><td>', 
            response.results[i].position.lon,
            '</td></tr>');
        }

        html.push('</table>');

        // Add the resulting HTML to the body of the page.
        document.body.innerHTML = html.join('');
    });
    ```

    Aqui está o exemplo de código completo em execução:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Usando o módulo de serviços" src="//codepen.io/azuremaps/embed/zbXGMR/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte a caneta <a href='https://codepen.io/azuremaps/pen/zbXGMR/'>usando o módulo serviços</a> pelo Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) em <a href='https://codepen.io'>CodePen</a>.
</iframe>

<br/>

## <a name="azure-government-cloud-support"></a>Suporte de nuvem do Azure governamental

O SDK da Web do Azure Maps dá suporte à nuvem do Azure governamental. Todas as URLs de JavaScript e CSS usadas para acessar o SDK da Web do Azure Maps permanecem as mesmas, no entanto, as tarefas a seguir precisarão ser feitas para se conectar à versão de nuvem do Azure governamental da plataforma Azure Maps.

Ao usar o controle de mapa interativo, adicione a seguinte linha de código antes de criar uma instância da `Map` classe. 

```javascript
atlas.setDomain('atlas.azure.us');
```

Certifique-se de usar os detalhes de autenticação do Azure Maps da plataforma de nuvem do Azure governamental ao autenticar o mapa e os serviços.

Ao usar o módulo de serviços, o domínio para os serviços precisa ser definido ao criar uma instância de um ponto de extremidade de URL de API. Por exemplo, o código a seguir cria uma instância da `SearchURL` classe e aponta o domínio para a nuvem do Azure governamental.

```javascript
var searchURL = new atlas.service.SearchURL(pipeline, 'atlas.azure.us');
```

Se acessar diretamente os serviços REST do Azure Maps, altere o domínio da URL para `atlas.azure.us` . Por exemplo, se estiver usando o serviço de API de pesquisa, altere o domínio de URL de `https://atlas.microsoft.com/search/` para `https://atlas.azure.us/search/` .

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre as classes e métodos usados neste artigo:

> [!div class="nextstepaction"]
> [MapsURL](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.mapsurl?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [SearchURL](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [RouteURL](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [SubscriptionKeyCredential](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.subscriptionkeycredential?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [TokenCredential](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.tokencredential?view=azure-maps-typescript-latest)

Para obter mais exemplos de código que usam o módulo serviços, consulte estes artigos:

> [!div class="nextstepaction"]
> [Mostrar resultados da pesquisa no mapa](./map-search-location.md)

> [!div class="nextstepaction"]
> [Obter informações de uma coordenada](./map-get-information-from-coordinate.md)

> [!div class="nextstepaction"]
> [Mostrar trajetos de A para B](./map-route.md)
