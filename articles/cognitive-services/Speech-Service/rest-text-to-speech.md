---
title: REST (referência de API de conversão de texto em fala) – serviço de fala
titleSuffix: Azure Cognitive Services
description: Saiba como usar a API REST de conversão de texto em fala. Neste artigo, você aprenderá sobre opções de autorização, opções de consulta, como estruturar uma solicitação e receber uma resposta.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/23/2020
ms.author: trbye
ms.openlocfilehash: 0f43d1f780f838fdc49eb055536204026edcc729
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87079232"
---
# <a name="text-to-speech-rest-api"></a>API REST conversão de texto em fala

O serviço de fala permite que você [converta o texto em fala sintetizada](#convert-text-to-speech) e [obtenha uma lista de vozes com suporte](#get-a-list-of-voices) para uma região usando um conjunto de APIs REST. Cada ponto de extremidade disponível é associado a uma região. É necessária uma chave de assinatura para o ponto de extremidade/região que você planeja usar.

A API REST de conversão de texto em fala é compatível com vozes neurais e padrão de conversão de texto em fala, cada uma delas compatível com um idioma e dialeto específicos, identificados pela localidade.

* Para obter uma lista completa de vozes, consulte [suporte para idioma](language-support.md#text-to-speech).
* Para obter informações sobre a disponibilidade regional, consulte [regiões](regions.md#text-to-speech).

> [!IMPORTANT]
> Os custos variam para vozes padrão, personalizadas e neurais. Para saber mais, consulte [Preços](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

Antes de usar essa API, entenda:

* A API REST de conversão de texto em voz requer um cabeçalho de autorização. Isso significa que você precisa concluir uma troca de tokens para acessar o serviço. Para obter mais informações, consulte [Autenticação](#authentication).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-rest-auth.md)]

## <a name="get-a-list-of-voices"></a>Obter uma lista de vozes

O `voices/list` ponto de extremidade permite obter uma lista completa de vozes para uma região/um ponto de extremidade específico.

### <a name="regions-and-endpoints"></a>Regiões e endpoints

| Região | Ponto de extremidade |
|--------|----------|
| Leste da Austrália | `https://australiaeast.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Sul do Brasil | `https://brazilsouth.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Canadá Central | `https://canadacentral.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Centro dos EUA | `https://centralus.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Leste da Ásia | `https://eastasia.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Leste dos EUA | `https://eastus.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Leste dos EUA 2 | `https://eastus2.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| França Central | `https://francecentral.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Centro da Índia | `https://centralindia.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Japan East | `https://japaneast.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Coreia Central | `https://koreacentral.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Centro-Norte dos EUA | `https://northcentralus.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Norte da Europa | `https://northeurope.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| South Central US | `https://southcentralus.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Sudeste Asiático | `https://southeastasia.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Sul do Reino Unido | `https://uksouth.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Europa Ocidental | `https://westeurope.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Oeste dos EUA | `https://westus.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Oeste dos EUA 2 | `https://westus2.tts.speech.microsoft.com/cognitiveservices/voices/list` |

### <a name="request-headers"></a>Cabeçalhos da solicitação

Esta tabela lista os cabeçalhos obrigatórios e opcionais para solicitações de conversão de texto em fala.

| Cabeçalho | Descrição | Obrigatório/Opcional |
|--------|-------------|---------------------|
| `Authorization` | Um token de autorização precedido pela palavra `Bearer`. Para obter mais informações, consulte [Autenticação](#authentication). | Obrigatório |

### <a name="request-body"></a>Corpo da solicitação

Um corpo não é necessário para `GET` solicitações a esse ponto de extremidade.

### <a name="sample-request"></a>Solicitação de exemplo

Essa solicitação requer apenas um cabeçalho de autorização.

```http
GET /cognitiveservices/voices/list HTTP/1.1

Host: westus.tts.speech.microsoft.com
Authorization: Bearer [Base64 access_token]
```

### <a name="sample-response"></a>Resposta de exemplo

Essa resposta foi truncada para ilustrar a estrutura de uma resposta.

> [!NOTE]
> A disponibilidade de voz varia por região/ponto de extremidade.

```json
[
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (ar-EG, Hoda)",
        "ShortName": "ar-EG-Hoda",
        "Gender": "Female",
        "Locale": "ar-EG",
        "SampleRateHertz": "16000",
        "VoiceType": "Standard"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (ar-SA, Naayf)",
        "ShortName": "ar-SA-Naayf",
        "Gender": "Male",
        "Locale": "ar-SA",
        "SampleRateHertz": "16000",
        "VoiceType": "Standard"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (bg-BG, Ivan)",
        "ShortName": "bg-BG-Ivan",
        "Gender": "Male",
        "Locale": "bg-BG",
        "SampleRateHertz": "16000",
        "VoiceType": "Standard"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (ca-ES, HerenaRUS)",
        "ShortName": "ca-ES-HerenaRUS",
        "Gender": "Female",
        "Locale": "ca-ES",
        "SampleRateHertz": "16000",
        "VoiceType": "Standard"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (zh-CN, XiaoxiaoNeural)",
        "ShortName": "zh-CN-XiaoxiaoNeural",
        "Gender": "Female",
        "Locale": "zh-CN",
        "SampleRateHertz": "24000",
        "VoiceType": "Neural"
    },

    ...
]
```

### <a name="http-status-codes"></a>Códigos de status HTTP

O código de status HTTP para cada resposta indica sucesso ou erros comuns.

| Código de status HTTP | Descrição | Possível motivo |
|------------------|-------------|-----------------|
| 200 | OK | A solicitação foi bem-sucedida. |
| 400 | Solicitação incorreta | Um parâmetro obrigatório está ausente, vazio ou nulo. Ou então, o valor passado como um parâmetro obrigatório ou opcional é inválido. Um problema comum é um cabeçalho que é muito longo. |
| 401 | Não Autorizado | A solicitação não foi autorizada. Verifique se a chave de assinatura ou o token são válidos e se estão na região correta. |
| 429 | Número Excessivo de Solicitações | Você excedeu a cota ou a taxa de solicitações permitidas para a sua assinatura. |
| 502 | Gateway incorreto    | Problema de rede ou do servidor. Também pode indicar cabeçalhos inválidos. |


## <a name="convert-text-to-speech"></a>Converter texto em fala

O `v1` ponto de extremidade permite converter conversão de texto em fala usando a [linguagem de marcação de síntese de fala (SSML)](speech-synthesis-markup.md).

### <a name="regions-and-endpoints"></a>Regiões e endpoints

Essas regiões são suportadas para text-to-speech usando a API REST. Certifique-se de selecionar o terminal que corresponde à sua região de assinatura.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

### <a name="request-headers"></a>Cabeçalhos da solicitação

Esta tabela lista os cabeçalhos obrigatórios e opcionais para solicitações de conversão de texto em fala.

| Cabeçalho | Descrição | Obrigatório/Opcional |
|--------|-------------|---------------------|
| `Authorization` | Um token de autorização precedido pela palavra `Bearer`. Para obter mais informações, consulte [Autenticação](#authentication). | Obrigatório |
| `Content-Type` | Especifica o tipo de conteúdo para o texto fornecido. Aceita o valor: `application/ssml+xml`. | Obrigatório |
| `X-Microsoft-OutputFormat` | Especifica o formato de saída de áudio. Para obter uma lista completa dos valores aceitos, consulte [saídas de áudio](#audio-outputs). | Obrigatório |
| `User-Agent` | O nome do aplicativo. O valor fornecido deve ter menos de 255 caracteres. | Obrigatório |

### <a name="audio-outputs"></a>Saídas de áudio

Esta é uma lista de formatos de áudio suportados que são enviados em cada solicitação como o cabeçalho `X-Microsoft-OutputFormat`. Cada um incorpora um tipo de taxa de bits e codificação. O serviço de fala dá suporte a saídas de áudio de 24 kHz, 16 kHz e 8 kHz.

```output
raw-16khz-16bit-mono-pcm            raw-8khz-8bit-mono-mulaw
riff-8khz-8bit-mono-alaw            riff-8khz-8bit-mono-mulaw
riff-16khz-16bit-mono-pcm           audio-16khz-128kbitrate-mono-mp3
audio-16khz-64kbitrate-mono-mp3     audio-16khz-32kbitrate-mono-mp3
raw-24khz-16bit-mono-pcm            riff-24khz-16bit-mono-pcm
audio-24khz-160kbitrate-mono-mp3    audio-24khz-96kbitrate-mono-mp3
audio-24khz-48kbitrate-mono-mp3     ogg-24khz-16bit-mono-opus
```

> [!NOTE]
> Se sua voz selecionada e o formato de saída tiverem diferentes taxas de bits, o áudio é aumentado conforme necessário. Ogg-24kHz-16 bits-mono-Opus pode ser decodificado com o [codec Opus](https://opus-codec.org/downloads/)

### <a name="request-body"></a>Corpo da solicitação

O corpo de cada solicitação `POST` é enviada como [SSML (Speech Synthesis Markup Language)](speech-synthesis-markup.md). A SSML permite que você escolha o idioma e a voz da fala sintetizada retornada pelo serviço de conversão de texto em fala. Para obter uma lista completa de vozes compatíveis, confira [suporte para idioma](language-support.md#text-to-speech).

> [!NOTE]
> Se usar uma voz personalizada, o corpo de uma solicitação poderá ser enviado como texto sem formatação (ASCII ou UTF-8).

### <a name="sample-request"></a>Solicitação de exemplo

Esta solicitação HTTP usa SSML para especificar a voz e o idioma. Se o comprimento do corpo for longo e o áudio resultante exceder 10 minutos, ele será truncado para 10 minutos. Em outras palavras, o tamanho do áudio não pode exceder 10 minutos.

```http
POST /cognitiveservices/v1 HTTP/1.1

X-Microsoft-OutputFormat: raw-16khz-16bit-mono-pcm
Content-Type: application/ssml+xml
Host: westus.tts.speech.microsoft.com
Content-Length: 225
Authorization: Bearer [Base64 access_token]

<speak version='1.0' xml:lang='en-US'><voice xml:lang='en-US' xml:gender='Female'
    name='en-US-AriaRUS'>
        Microsoft Speech Service Text-to-Speech API
</voice></speak>
```

Consulte nossos guias de início rápido para obter exemplos específicos de idioma:

* [.NET Core, C #](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-csharp&tabs=dotnetcore)
* [Python](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-python)
* [Node.js](quickstart-nodejs-text-to-speech.md)

### <a name="http-status-codes"></a>Códigos de status HTTP

O código de status HTTP para cada resposta indica sucesso ou erros comuns.

| Código de status HTTP | Descrição | Possível motivo |
|------------------|-------------|-----------------|
| 200 | OK | A solicitação foi bem-sucedida. O corpo da resposta é um arquivo de áudio. |
| 400 | Solicitação incorreta | Um parâmetro obrigatório está ausente, vazio ou nulo. Ou então, o valor passado como um parâmetro obrigatório ou opcional é inválido. Um problema comum é um cabeçalho que é muito longo. |
| 401 | Não Autorizado | A solicitação não foi autorizada. Verifique se a chave de assinatura ou o token são válidos e se estão na região correta. |
| 413 | Entidade de solicitação muito grande | A entrada de SSML tem mais de 1024 caracteres. |
| 415 | Tipo de Mídia Sem Suporte | É possível que o incorreto `Content-Type` tenha sido fornecido. `Content-Type`deve ser definido como `application/ssml+xml` . |
| 429 | Número Excessivo de Solicitações | Você excedeu a cota ou a taxa de solicitações permitidas para a sua assinatura. |
| 502 | Gateway incorreto    | Problema de rede ou do servidor. Também pode indicar cabeçalhos inválidos. |

Se o status HTTP for `200 OK`, o corpo da resposta conterá um arquivo de áudio no formato solicitado. Este arquivo pode ser reproduzido enquanto é transferido, salvo em um buffer ou salvo em um arquivo.

## <a name="next-steps"></a>Próximas etapas

- [Obtenha sua assinatura de avaliação de Fala](https://azure.microsoft.com/try/cognitive-services)
- [Síntese assíncrona para áudio de forma longa](quickstarts/text-to-speech/async-synthesis-long-form-audio.md)
- [Introdução à Voz Personalizada](how-to-custom-voice.md)
