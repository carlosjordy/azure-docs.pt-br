---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 788c8f8093cbc70857727b5c3f89e1719f3ac171
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86035586"
---
:::row:::
    :::column span="3":::
        O SDK de fala para JavaScript está disponível como um pacote NPM, consulte <a href="https://www.npmjs.com/package/microsoft-cognitiveservices-speech-sdk" target="_blank">Microsoft-cognitivaservices-Speech- <span class="docon docon-navigate-external x-hidden-focus"></span> SDK</a> e ele complementa o repositório GitHub <a href="https://github.com/Microsoft/cognitive-services-speech-sdk-js" target="_blank">cognitiva-Services-Speech-SDK- <span class="docon docon-navigate-external x-hidden-focus"></span> js </a>.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Node.js" src="https://docs.microsoft.com/media/logos/logo_nodejs.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

> [!TIP]
> Embora o SDK de fala para JavaScript esteja disponível como um pacote NPM, portanto, tanto Node.js quanto navegadores da Web cliente podem consumi-lo-considere as várias implicações arquitetônicas de cada ambiente. Por exemplo, o <a href="https://en.wikipedia.org/wiki/Document_Object_Model" target="_blank">modelo de objeto de documento ( <span class="docon docon-navigate-external x-hidden-focus"></span> dom)</a> não está disponível para aplicativos do lado do servidor, assim como o <a href="https://nodejs.org/api/fs.html" target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> sistema de arquivos</a> não está disponível para aplicativos do lado do cliente.

### <a name="nodejs-package-manager-npm"></a>Gerenciador de pacotes do Node.js (NPM)

Para instalar o SDK de fala para JavaScript, execute o seguinte `npm install` comando abaixo.

```nodejs
npm install microsoft-cognitiveservices-speech-sdk
```

Para obter mais informações, consulte o guia de <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/node" target="_blank">início <span class="docon docon-navigate-external x-hidden-focus"></span> rápido do SDK doNode.js Speech </a>.
