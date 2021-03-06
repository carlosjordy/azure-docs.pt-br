---
title: Inspecionar a qualidade de dados para o serviço de Fala Personalizada-fala
titleSuffix: Azure Cognitive Services
description: Fala Personalizada fornece ferramentas que permitem inspecionar visualmente a qualidade de reconhecimento de um modelo, comparando dados de áudio com o resultado de reconhecimento correspondente. Você pode reproduzir áudio carregado e determinar se o resultado de reconhecimento fornecido está correto.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: erhopf
ms.openlocfilehash: e871d2c8e0fe00fa7db3144a787447163c82e62d
ms.sourcegitcommit: d7fba095266e2fb5ad8776bffe97921a57832e23
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84629031"
---
# <a name="inspect-custom-speech-data"></a>Inspecionar dados da Fala Personalizada

> [!NOTE]
> Esta página pressupõe que você tenha lido [preparar dados de teste para fala personalizada](how-to-custom-speech-test-data.md) e ter carregado um DataSet para inspeção.

Fala Personalizada fornece ferramentas que permitem inspecionar visualmente a qualidade de reconhecimento de um modelo, comparando dados de áudio com o resultado de reconhecimento correspondente. No [portal de fala personalizada](https://speech.microsoft.com/customspeech), você pode reproduzir áudio carregado e determinar se o resultado de reconhecimento fornecido está correto. Essa ferramenta ajuda a inspecionar a qualidade do modelo de fala-para-texto de linha de base da Microsoft, inspecionar um modelo personalizado treinado ou comparar a transcrição por dois modelos.

Neste documento, você aprenderá a inspecionar visualmente a qualidade de um modelo usando os dados de treinamento carregados anteriormente.

Nesta página, você aprenderá a inspecionar visualmente a qualidade do modelo de fala-a-texto de linha de base da Microsoft e/ou um modelo personalizado que você tenha treinado. Você usará os dados carregados na guia **dados** para teste.

## <a name="create-a-test"></a>Criar um teste

Siga estas instruções para criar um teste:

1. Entre no portal de [fala personalizada](https://speech.microsoft.com/customspeech).
2. Navegue até **conversão de fala em texto > Fala Personalizada > [nome do projeto] > teste**.
3. Clique em **Adicionar teste**.
4. Selecione **inspecionar qualidade (dados somente de áudio)**. Dê ao teste um nome, uma descrição e selecione seu conjunto de dado de áudio.
5. Selecione até dois modelos que você gostaria de testar.
6. Clique em **Criar**.

Depois que um teste tiver sido criado com êxito, você poderá ver como um modelo transcreve o conjunto de testes de áudio especificado ou comparar os resultados de dois modelos lado a lado.

[!INCLUDE [service-pricing-advisory](includes/service-pricing-advisory.md)]

## <a name="side-by-side-model-comparisons"></a>Comparações de modelo lado a lado

Quando o status do teste for _bem-sucedido_, clique no nome do item de teste para ver os detalhes do teste. Essa página de detalhes lista todos os declarações em seu conjunto de seus conjuntos de anotações e mostra os resultados de reconhecimento dos dois modelos que você está comparando.

Para ajudar a inspecionar a comparação lado a lado, você pode alternar vários tipos de erro, incluindo inserção, exclusão e substituição. Ao ouvir o áudio e comparar os resultados de reconhecimento em cada coluna (mostrando a transcrição com rótulo humano e os resultados de dois modelos de fala em texto), você pode decidir qual modelo atende às suas necessidades e onde são necessárias melhorias.

O teste de modelo lado a lado é útil para validar qual modelo de reconhecimento de fala é melhor para um aplicativo. Para uma medida objetiva de precisão, exigindo áudio transcrita, siga as instruções encontradas em [avaliar exatidão](how-to-custom-speech-evaluate-data.md).

## <a name="next-steps"></a>Próximas etapas

- [Avalie seus dados](how-to-custom-speech-evaluate-data.md)
- [Treinar seu modelo](how-to-custom-speech-train-model.md)
- [Melhorar o modelo](how-to-custom-speech-improve-accuracy.md)
- [Implantar o seu modelo](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Recursos adicionais

- [Preparar dados de teste para Fala Personalizada](how-to-custom-speech-test-data.md)
