---
title: Modelos personalizados
titleSuffix: Azure Digital Twins
description: Entenda como o Azure digital gêmeos usa modelos definidos pelo usuário para descrever as entidades em seu ambiente.
author: baanders
ms.author: baanders
ms.date: 3/12/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 56ebb32e2d1c2a9bab9592da63e1ada7130bb7ff
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87131626"
---
# <a name="understand-twin-models-in-azure-digital-twins"></a>Entender os modelos de entrelaçamento no Azure digital gêmeos

Uma característica importante do Azure digital gêmeos é a capacidade de definir seu próprio vocabulário e criar seu grafo de entrelaçamento nos termos autodefinidos de sua empresa. Esse recurso é fornecido por meio de **modelos**definidos pelo usuário. Você pode considerar os modelos como os substantivos em uma descrição do seu mundo. 

Um modelo é semelhante a uma **classe** em uma linguagem de programação orientada a objeto, definindo uma forma de dados para um determinado conceito em seu ambiente de trabalho real. Os modelos têm nomes (como *sala* ou *sensor*) e contêm elementos como propriedades, telemetria/eventos e comandos que descrevem o que esse tipo de entidade em seu ambiente pode fazer. Posteriormente, você usará esses modelos para criar [**gêmeos digitais**](concepts-twins-graph.md) que representam entidades específicas que atendem a essa descrição de tipo.

Os modelos são escritos usando o **DTDL (digital Mydefinition Language)** baseado em JSON-ld.  

## <a name="digital-twin-definition-language-dtdl-for-writing-models"></a>DTDL (digital mydefinition Language) para escrever modelos

Os modelos para o gêmeos digital do Azure são definidos usando o DTDL (digital gêmeos Definition Language). O DTDL é baseado em JSON-LD e é independente da linguagem de programação. O DTDL não é exclusivo do Azure digital gêmeos, mas também é usado para representar dados de dispositivo em outros serviços de IoT, como o [IoT plug and Play](../iot-pnp/overview-iot-plug-and-play.md). 

O Azure digital gêmeos usa a *versão 2*do DTDL. Para obter mais informações sobre esta versão do DTDL, consulte sua documentação de especificações no GitHub: [*digital gêmeos Definition Language (DTDL)-versão 2*](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md).

> [!TIP] 
> Nem todos os serviços que usam DTDL implementam exatamente os mesmos recursos de DTDL. Por exemplo, a Plug and Play de IoT não usa os recursos de DTDL que são para grafos, enquanto o Azure digital gêmeos atualmente não implementa comandos DTDL. Para obter mais informações sobre os recursos do DTDL que são específicos do Azure digital gêmeos, consulte a seção mais adiante neste artigo sobre [especificações de implementação do Azure digital gêmeos DTDL](#azure-digital-twins-dtdl-implementation-specifics).

## <a name="elements-of-a-model"></a>Elementos de um modelo

Dentro de uma definição de modelo, o item de código de nível superior é uma **interface**. Isso encapsula todo o modelo e o restante do modelo é definido dentro da interface. 

Uma interface de modelo DTDL pode conter zero, um ou muitos dos seguintes campos:
* Propriedades de **Propriedade** são campos de dados que representam o estado de uma entidade (como as propriedades em muitas linguagens de programação orientada a objeto). Ao contrário da telemetria, que é um evento de dados com limite de tempo, as propriedades têm armazenamento de backup e podem ser lidas a qualquer momento.
* **Telemetria** -os campos de telemetria representam medições ou eventos e geralmente são usados para descrever as leituras do sensor de dispositivo. A telemetria não é armazenada em um teledigital; é mais parecido com um fluxo de eventos de dados prontos para ser enviado em algum lugar. 
* **Componente** -componentes permitem que você crie sua interface de modelo como um assembly de outras interfaces, se desejar. Um exemplo de componente é uma interface *frontCamera* (e outra *backcamera*da interface de componente) que são usados na definição de um modelo para um *telefone*. Primeiro, você deve definir uma interface para *frontCamera* como se fosse seu próprio modelo e, em seguida, pode fazer referência a ela ao definir o *telefone*.

    Use um componente para descrever algo que é parte integrante da sua solução, mas que não precisa de uma identidade separada, e não precisa ser criado, excluído ou reorganizado no grafo de entrelaçamento de forma independente. Se você quiser que as entidades tenham existência independentes no grafo de entrelaçamento, represente-as como gêmeos digitais separadas de modelos diferentes, conectadas por *relações* (consulte o próximo marcador).
    
    >[!TIP] 
    >Os componentes também podem ser usados para a organização, para agrupar conjuntos de propriedades relacionadas em uma interface de modelo. Nessa situação, você pode considerar cada componente como um namespace ou "pasta" dentro da interface.
* As relações de **relacionamento** permitem que você represente como uma teledigital pode ser envolvida com outros gêmeos digitais. As relações podem representar significados de semântica diferentes, como *Contains* ("piso contém sala"), *frios* ("sala de frios do HVAC"), *isBilledTo* ("o compresso é cobrado pelo usuário") etc. As relações permitem que a solução forneça um grafo de entidades inter-relacionadas.

> [!NOTE]
> A especificação para DTDL também define **comandos**, que são métodos que podem ser executados em uma troca digital (como um comando de redefinição ou um comando para ativar ou desativar um ventilador). No entanto, os *comandos não têm suporte no momento no Azure digital gêmeos.*

### <a name="azure-digital-twins-dtdl-implementation-specifics"></a>Especificações de implementação do Azure digital gêmeos DTDL

Para que um modelo DTDL seja compatível com o gêmeos digital do Azure, ele deve atender a esses requisitos.

* Todos os elementos DTDL de nível superior em um modelo devem ser do tipo *interface*. Isso ocorre porque as APIs do modelo do gêmeos digital do Azure podem receber objetos JSON que representam uma interface ou uma matriz de interfaces. Como resultado, nenhum outro tipo de elemento DTDL é permitido no nível superior.
* O DTDL para o gêmeos digital do Azure não deve definir nenhum *comando*.
* O Azure digital gêmeos permite apenas um único nível de aninhamento de componentes. Isso significa que uma interface que está sendo usada como um componente não pode ter nenhum componente. 
* Interfaces não podem ser definidas embutidas em outras interfaces DTDL; Eles devem ser definidos como entidades de nível superior separadas com suas próprias IDs. Em seguida, quando outra interface deseja incluir essa interface como um componente ou por herança, ela pode referenciar sua ID.

## <a name="example-model-code"></a>Exemplo de código de modelo

Os modelos de tipo de entrelaçamento podem ser escritos em qualquer editor de texto. A linguagem DTDL segue a sintaxe JSON, portanto, você deve armazenar modelos com a extensão *. JSON*. Usar a extensão JSON permitirá que muitos editores de texto de programação forneçam verificação e realce de sintaxe básica para seus documentos do DTDL. Também há uma [extensão DTDL](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-dtdl) disponível para [Visual Studio Code](https://code.visualstudio.com/).

Esta seção contém um exemplo de um modelo típico, escrito como uma interface DTDL. O modelo descreve os **planetas**, cada um com um nome, uma massa e uma temperatura.
 
Considere que os planetas também podem interagir com **luas** que são seus satélites e **podem conter**enjuntores. No exemplo a seguir, o `Planet` modelo expressa conexões com essas outras entidades fazendo referência a dois modelos externos — `Moon` e `Crater` . Esses modelos também são definidos no código de exemplo abaixo, mas são mantidos muito simples para não detrair o `Planet` exemplo primário.

```json
[
  {
    "@id": "dtmi:com:contoso:Planet;1",
    "@type": "Interface",
    "@context": "dtmi:dtdl:context;2",
    "displayName": "Planet",
    "contents": [
      {
        "@type": "Property",
        "name": "name",
        "schema": "string"
      },
      {
        "@type": "Property",
        "name": "mass",
        "schema": "double"
      },
      {
        "@type": "Telemetry",
        "name": "Temperature",
        "schema": "double"
      },
      {
        "@type": "Relationship",
        "name": "satellites",
        "target": "dtmi:com:contoso:Moon;1"
      },
      {
        "@type": "Component",
        "name": "deepestCrater",
        "schema": "dtmi:com:contoso:Crater;1"
      }
    ]
  },
  {
    "@id": "dtmi:com:contoso:Crater;1",
    "@type": "Interface",
    "@context": "dtmi:dtdl:context;2"
  },
  {
    "@id": "dtmi:com:contoso:Moon;1",
    "@type": "Interface",
    "@context": "dtmi:dtdl:context;2"
  }
]
```

Os campos do modelo são:

| Campo | Descrição |
| --- | --- |
| `@id` | Um identificador para o modelo. Deve estar no formato `dtmi:<domain>:<unique model identifier>;<model version number>` . |
| `@type` | Identifica o tipo de informações que estão sendo descritas. Para uma interface, o tipo é *interface*. |
| `@context` | Define o [contexto](https://niem.github.io/json/reference/json-ld/context/) para o documento JSON. Os modelos devem usar o `dtmi:dtdl:context;2` . |
| `displayName` | adicional Permite que você dê um nome amigável ao modelo, se desejado. |
| `contents` | Todos os dados de interface restantes são colocados aqui, como uma matriz de definições de atributo. Cada atributo deve fornecer um `@type` (*Propriedade*, *telemetria*, *comando*, *relação*ou *componente*) para identificar o tipo de informações de interface que ele descreve e, em seguida, um conjunto de propriedades que definem o atributo real (por exemplo, `name` e `schema` para definir uma *Propriedade*). |

> [!NOTE]
> Observe que a interface de componente (*Crater* neste exemplo) é definida na mesma matriz que a interface que a usa (*planeta*). Os componentes devem ser definidos dessa forma em chamadas à API para que a interface seja encontrada.

### <a name="possible-schemas"></a>Esquemas possíveis

De acordo com o DTDL, o esquema para atributos de *Propriedade* e *telemetria* pode ser de tipos primitivos padrão — `integer` ,, `double` `string` e `Boolean` — e outros tipos, como `DateTime` e `Duration` . 

Além dos tipos primitivos, os campos de *Propriedade* e *telemetria* podem ter esses tipos complexos:
* `Object`
* `Map`
* `Enum`

Os campos de *telemetria* também dão suporte a `Array` .

### <a name="model-inheritance"></a>Herança de modelo

Às vezes, talvez você queira especializar ainda mais um modelo. Por exemplo, pode ser útil ter uma *sala*de modelo genérico e variantes especializadas *conferenceroom* e *Gym*. Para expressar especialização, o DTDL dá suporte à herança: as interfaces podem herdar de uma ou mais interfaces. 

O exemplo a seguir reimagina o modelo *planeta* do exemplo de DTDL anterior como um subtipo de um modelo de *CelestialBody* maior. O modelo "pai" é definido primeiro e, em seguida, o modelo "filho" é criado com base nele usando o campo `extends` .

```json
[
  {
    "@id": "dtmi:com:contoso:CelestialBody;1",
    "@type": "Interface",
    "@context": "dtmi:dtdl:context;2",
    "displayName": "Celestial body",
    "contents": [
      {
        "@type": "Property",
        "name": "name",
        "schema": "string"
      },
      {
        "@type": "Property",
        "name": "mass",
        "schema": "double"
      },
      {
        "@type": "Telemetry",
        "name": "temperature",
        "schema": "double"
      }
    ]
  },
  {
    "@id": "dtmi:com:contoso:Planet;1",
    "@type": "Interface",
    "@context": "dtmi:dtdl:context;2",
    "displayName": "Planet",
    "extends": "dtmi:com:contoso:CelestialBody;1",
    "contents": [
      {
        "@type": "Relationship",
        "name": "satellites",
        "target": "dtmi:com:contoso:Moon;1"
      },
      {
        "@type": "Component",
        "name": "deepestCrater",
        "schema": "dtmi:com:contoso:Crater;1"
      }
    ]
  },
  {
    "@id": "dtmi:com:contoso:Crater;1",
    "@type": "Interface",
    "@context": "dtmi:dtdl:context;2"
  }
]
```

Neste exemplo, *CelestialBody* contribui com um nome, uma massa e uma temperatura para o *planeta*. A `extends` seção é um nome de interface ou uma matriz de nomes de interface (permitindo que a interface de extensão herde de vários modelos pai, se desejado).

Depois que a herança é aplicada, a interface de extensão expõe todas as propriedades de toda a cadeia de herança.

A interface de extensão não pode alterar nenhuma das definições das interfaces pai; Ele só pode adicionar a eles. Ele também não pode redefinir um recurso já definido em qualquer uma de suas interfaces pai (mesmo que os recursos estejam definidos para serem os mesmos). Por exemplo, se uma interface pai define uma `double` propriedade de *massa*, a interface de extensão não pode conter uma declaração de *massa*, mesmo que também seja um `double` .

## <a name="validating-models"></a>Validando modelos

> [!TIP]
> É recomendável validar seus modelos offline antes de carregá-los na instância do gêmeos digital do Azure.

Há um exemplo independente de linguagem disponível para validar documentos de modelo para verificar se o DTDL está correto. Ele está localizado aqui: [**exemplo de validador de DTDL**](https://docs.microsoft.com/samples/azure-samples/dtdl-validator/dtdl-validator).

O exemplo do validador DTDL é criado em uma biblioteca do analisador do .NET DTDL, que está disponível no NuGet como uma biblioteca do lado do cliente: [**Microsoft. Azure. DigitalTwins. Parser**](https://nuget.org/packages/Microsoft.Azure.DigitalTwins.Parser/). Você também pode usar a biblioteca diretamente para criar sua própria solução de validação. Ao usar a biblioteca do analisador, certifique-se de usar uma versão compatível com a versão que o Azure digital gêmeos está executando. Durante a visualização, esta é a versão *3.7.0*.

Você pode aprender mais sobre a biblioteca do analisador, incluindo exemplos de uso, em [*instruções: analisar e validar modelos*](how-to-use-parser.md).

## <a name="next-steps"></a>Próximas etapas

Consulte como gerenciar modelos com as APIs do DigitalTwinsModels:
* [*Instruções: Gerenciar modelos personalizados*](how-to-manage-model.md)

Ou então, saiba como os gêmeos digitais são criados com base em modelos:
* [*Conceitos: digital gêmeos e o gráfico de entrelaçamento*](concepts-twins-graph.md)

