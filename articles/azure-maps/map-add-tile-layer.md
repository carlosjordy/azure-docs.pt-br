---
title: Adicionar uma camada de bloco a um mapa | Mapas do Microsoft Azure
description: Neste artigo, você aprenderá a sobrepor uma camada de bloco em um mapa usando o SDK da Web do Microsoft Azure Maps. As camadas de bloco permitem renderizar imagens em um mapa.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 6bb3cfa7266688ac8973bd3838d0d03e9efe8d50
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86242297"
---
# <a name="add-a-tile-layer-to-a-map"></a>Adicionar uma camada de bloco a um mapa

Este artigo mostra como sobrepor uma camada de bloco no mapa. Camadas lado a lado permitem que você sobreponha imagens sobre peças de mapa base do Azure Mapas. Para obter mais informações sobre o sistema de divisão de mapas do Azure, consulte [níveis de zoom e grade de blocos](zoom-levels-and-tile-grid.md).

Uma camada de bloco é carregada em blocos de um servidor. Essas imagens podem ser renderizadas previamente ou processadas dinamicamente. As imagens previamente renderizadas são armazenadas como qualquer outra imagem em um servidor usando uma Convenção de nomenclatura que a camada de bloco compreende. As imagens renderizadas dinamicamente usam um serviço para carregar as imagens perto do tempo real. Há três convenções de nomenclatura de serviço de bloco diferentes com suporte pela classe [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest) do Azure Maps: 

* X, Y, notação de zoom-X é a coluna, Y é a posição de linha do bloco na grade de blocos e a notação de zoom um valor com base no nível de zoom.
* Notação Quadkey – combina as informações x, y e zoom em um único valor de cadeia de caracteres. Esse valor de cadeia de caracteres torna-se um identificador exclusivo para um único bloco.
* Caixa delimitadora-especifique uma imagem no formato de coordenadas da caixa delimitadora: `{west},{south},{east},{north}` . Esse formato é normalmente usado pelos [serviços de mapeamento da Web (WMS)](https://www.opengeospatial.org/standards/wms).

> [!TIP]
> Um [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest) é uma ótima maneira para visualizar grandes conjuntos de dados no mapa. Uma camada de bloco não só pode ser gerada a partir de uma imagem, os dados vetoriais também podem ser renderizados como uma camada de bloco. Ao renderizar dados de vetor como uma camada de bloco, o controle de mapeamento precisa apenas carregar os blocos que são menores no tamanho do arquivo do que os dados de vetor que eles representam. Essa técnica costuma ser usada para renderizar milhões de linhas de dados no mapa.

A URL do bloco passada para uma camada de bloco deve ser uma URL http ou HTTPS para um recurso TileJSON ou um modelo de URL de bloco que usa os seguintes parâmetros: 

* `{x}` - posição X do bloco. Também precisa `{y}` e `{z}`.
* `{y}` - posição Y do bloco. Também precisa `{x}` e `{z}`.
* `{z}` -Nível de zoom do bloco. Também precisa `{x}` e `{y}`.
* `{quadkey}` - identificador quadkey de bloco baseado a convenção de nomenclatura do sistema de blocos Bing Maps.
* `{bbox-epsg-3857}` -Uma cadeia de caracteres de caixa delimitadora com o formato `{west},{south},{east},{north}` no sistema de referência espacial do EPSG 3857.
* `{subdomain}`-Um espaço reservado para os valores de subdomínio, se especificado, `subdomain` será adicionado.
* `{azMapsDomain}`-Um espaço reservado para alinhar o domínio e a autenticação de solicitações de bloco com os mesmos valores usados pelo mapa.

## <a name="add-a-tile-layer"></a>Adicionar uma camada de bloco

 Este exemplo mostra como criar uma camada de bloco que aponta para um conjunto de blocos. Este exemplo usa o sistema de divisão x, y e zoom. A origem dessa camada lado a lado é uma sobreposição de radar clima da [Iowa Environmental Mesonet of Iowa State University](https://mesonet.agron.iastate.edu/ogc/). Ao exibir dados de radar, o ideal é que os usuários vejam claramente os rótulos das cidades à medida que navegam no mapa. Esse comportamento pode ser implementado inserindo-se a camada do bloco abaixo da `labels` camada.

```javascript
//Create a tile layer and add it to the map below the label layer.
//Weather radar tiles from Iowa Environmental Mesonet of Iowa State University.
map.layers.add(new atlas.layer.TileLayer({
    tileUrl: 'https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/{z}/{x}/{y}.png',
    opacity: 0.8,
    tileSize: 256
}), 'labels');
```

Veja abaixo o exemplo de código completo em execução da funcionalidade acima.

<br/>

<iframe height='500' scrolling='no' title='Bloco de camada usando X, Y e Z' src='//codepen.io/azuremaps/embed/BGEQjG/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a Camada <a href='https://codepen.io/azuremaps/pen/BGEQjG/'>Caneta lado a lado usando X, Y e Z</a> pelo Azure Mapas (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) em <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="customize-a-tile-layer"></a>Personalizar uma camada lado a lado

A classe da camada do bloco tem muitas opções de estilo. Aqui está uma ferramenta para experimentá-las.

<br/>

<iframe height='700' scrolling='no' title='Opções de camada de bloco' src='//codepen.io/azuremaps/embed/xQeRWX/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte as <a href='https://codepen.io/azuremaps/pen/xQeRWX/'>Opções de Camada de Bloco</a> pelo Azure Mapas (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) em <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre as classes e métodos usados neste artigo:

> [!div class="nextstepaction"]
> [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [TileLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.tilelayeroptions?view=azure-iot-typescript-latest)

Consulte os artigos a seguir para obter mais exemplos de código para adicionar aos seus mapas:

> [!div class="nextstepaction"]
> [Adicionar uma camada de imagem](./map-add-image-layer.md)
