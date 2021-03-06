---
title: 'Início Rápido: executar uma pesquisa com Python – API de Pesquisa na Web do Bing'
titleSuffix: Azure Cognitive Services
description: Use este Início Rápido para enviar solicitações para a API REST de Pesquisa na Web do Bing usando o Python e receba uma resposta JSON
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: quickstart
ms.date: 05/22/2020
ms.author: aahi
ms.custom: seodec2018, tracking-python
ms.openlocfilehash: c4dd2de53f0222b687a05690727f0aa9e25c7d53
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84608665"
---
# <a name="quickstart-use-python-to-call-the-bing-web-search-api"></a>Início Rápido: usar Python para chamar a API de Pesquisa na Web do Bing  

Use este início rápido para fazer sua primeira chamada à API da Pesquisa na Web do Bing. Este aplicativo Python envia uma solicitação de pesquisa à API e exibe a resposta JSON. Embora esse aplicativo seja escrito em Python, a API é um serviço Web RESTful compatível com a maioria das linguagens de programação.

Este exemplo é executado como um Jupyter Notebook em [MyBinder](https://mybinder.org). Para executá-lo, selecione a notificação iniciar associador:

[![Associador](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingWebSearchAPI.ipynb)

## <a name="prerequisites"></a>Pré-requisitos

* [Python 2.x ou 3.x](https://www.python.org/)

[!INCLUDE [bing-web-search-quickstart-signup](../../../../includes/bing-web-search-quickstart-signup.md)]

## <a name="define-variables"></a>Definir variáveis

1. Substitua o valor `subscription_key` por uma chave de assinatura válida da sua conta do Azure.

   ```python
   subscription_key = "YOUR_ACCESS_KEY"
   assert subscription_key
   ```

2. Declare o ponto de extremidade da API de Pesquisa na Web do Bing. É possível usar o ponto de extremidade global no código a seguir ou o ponto de extremidade do [subdomínio personalizado](../../../cognitive-services/cognitive-services-custom-subdomains.md) exibido no portal do Azure para seu recurso.

   ```python
   search_url = "https://api.cognitive.microsoft.com/bing/v7.0/search"
   ```

3. Opcionalmente, personalize a consulta de pesquisa substituindo o valor para `search_term`.

   ```python
   search_term = "Azure Cognitive Services"
   ```

## <a name="make-a-request"></a>Fazer uma solicitação

Este código usa a biblioteca `requests` para chamar a API de Pesquisa na Web do Bing e retornar os resultados como um objeto JSON. A chave de API é passada no dicionário `headers` e os parâmetros de consulta e termo de pesquisa são passados no dicionário `params`. 

Para obter uma lista completa de opções e parâmetros, confira [API de Pesquisa na Web do Bing v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference).

```python
import requests

headers = {"Ocp-Apim-Subscription-Key": subscription_key}
params = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

## <a name="format-and-display-the-response"></a>Formatar e exibir a resposta

O objeto `search_results` inclui os resultados da pesquisa e esses metadados como consultas e as páginas relacionadas. Este código usa a biblioteca `IPython.display` para formatar e exibir a resposta no seu navegador.

```python
from IPython.display import HTML

rows = "\n".join(["""<tr>
                       <td><a href=\"{0}\">{1}</a></td>
                       <td>{2}</td>
                     </tr>""".format(v["url"], v["name"], v["snippet"])
                  for v in search_results["webPages"]["value"]])
HTML("<table>{0}</table>".format(rows))
```

## <a name="sample-code-on-github"></a>Exemplo de código no GitHub

Para executar esse código localmente, confira o [exemplo completo disponível no GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingWebSearchv7.py).

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Tutorial de aplicativo de página única da API de Pesquisa na Web do Bing](../tutorial-bing-web-search-single-page-app.md)

[!INCLUDE [bing-web-search-quickstart-see-also](../../../../includes/bing-web-search-quickstart-see-also.md)]
