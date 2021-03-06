---
title: Problema ao adicionar um aplicativo da Galeria do Azure AD | Microsoft Docs
description: Entenda os problemas comuns que as pessoas enfrentam durante a adição de aplicativos da Galeria do Azure AD e o que você pode fazer para resolvê-los
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/11/2018
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: e4387a3ca02297311cd9ab025c315ca3324cbbbe
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84760398"
---
# <a name="problem-adding-an-azure-ad-gallery-application"></a>Problema ao adicionar um aplicativo da Galeria do Azure AD

Este artigo ajuda você a entender os problemas comuns que as pessoas enfrentam durante a adição de aplicativos da Galeria do Azure AD e o que você pode fazer para resolvê-los.

## <a name="i-clicked-the-add-button-and-my-application-took-a-long-time-to-appear"></a>Cliquei no botão "adicionar" e meu aplicativo demorou muito para aparecer

Em algumas circunstâncias, pode demorar de 1 ou 2 minutos (às vezes mais) para um aplicativo aparecer após ser adicionado ao diretório. Embora esse não seja o desempenho normal esperado, você pode ver que a adição do aplicativo está em andamento clicando no ícone **Notificações** (o sino) no canto superior direito do [portal do Azure](https://portal.azure.com/) e procurando uma notificação **Em Andamento** ou **Concluído** rotulada como **Adicionar aplicativo.**

Se seu aplicativo nunca for adicionado, ou se você encontrar um erro ao clicar no botão **Adicionar**, você verá uma **Notificação** em um estado de **Erro**. Se quiser mais detalhes sobre o erro para saber mais ou para compartilhar com um engenheiro de suporte, você poderá ver mais informações sobre o erro seguindo as etapas na seção [Como ver os detalhes de uma notificação do portal](#how-to-see-the-details-of-a-portal-notification).

## <a name="i-clicked-the-add-button-and-my-application-didnt-appear"></a>Cliquei no botão "adicionar" e meu aplicativo não apareceu

Às vezes, devido a problemas temporários, problemas de rede ou um bug, a adição de um aplicativo poderá falhar. Você sabe que isso aconteceu quando clica no ícone **Notificações** (o sino) na parte superior direita do Portal do Azure e vê um ícone vermelho (!) ao lado de sua notificação **Adicionar aplicativo**. Isso indica que ocorreu um erro ao criar o aplicativo.

Se encontrar um erro ao clicar no botão **Adicionar**, você verá uma **Notificação** em um estado **Erro**. Se quiser mais detalhes sobre o erro para saber mais ou para compartilhar com um engenheiro de suporte, você poderá ver mais informações sobre o erro seguindo as etapas na seção [Como ver os detalhes de uma notificação do portal](#how-to-see-the-details-of-a-portal-notification).

## <a name="i-dont-know-how-to-set-up-my-application-once-ive-added-it"></a>Não sei configurar meu aplicativo após tê-lo adicionado

Se você precisar de ajuda para aprender sobre aplicativos, a [lista de tutoriais sobre como integrar aplicativos SaaS com Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) artigo é um bom ponto de partida.

Além disso, a [Biblioteca de documentos de aplicativos do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) ajuda você a saber mais sobre o logon único com o Azure AD e como ele funciona.

## <a name="how-to-see-the-details-of-a-portal-notification"></a>Como ver os detalhes de uma notificação do portal

Veja os detalhes de qualquer notificação do portal executando as etapas abaixo:

1.  Selecione o ícone de **notificações** (o Bell) no canto superior direito do portal do Azure

2.  Selecione qualquer notificação com estado de **Erro** (aquelas com um (!) vermelho ao lado).

    >[!NOTE]
    >Não é possível clicar em notificações com estado de **Êxito** ou **Em Andamento**.
    >
    >

4.  Use as informações em **Detalhes da Notificação** para saber mais detalhes sobre o problema.

5.  Se ainda precisar de ajuda, você também poderá compartilhar essas informações com um engenheiro de suporte ou com o grupo de produtos para obter ajuda com o problema.

6.  Clique no **ícone de ** **cópia** à direita da caixa de texto **Copiar erro** para copiar todos os detalhes da notificação para compartilhar com um engenheiro de suporte ou de grupo de produtos.

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a>Como obter ajuda enviando detalhes da notificação a um engenheiro de suporte

É muito importante que você compartilhe **todos os detalhes abaixo** com um engenheiro de suporte caso precise de ajuda, para que ele possa ajudar rapidamente. Faça isso facilmente **tirando uma captura de tela** ou clicando no **ícone Copiar erro**, localizado à direita da caixa de texto **Copiar erro**.

## <a name="notification-details-explained"></a>Detalhes da notificação explicados

Veja as descrições a seguir para mais detalhes sobre as notificações.

### <a name="essential-notification-items"></a>Itens de notificação essenciais

- **Título** – o título descritivo da notificação

  * Exemplo – **Configurações do proxy do aplicativo**

- **Descrição** – a descrição do que ocorreu como resultado da operação

  -   Exemplo – **A URL interna inserida já está sendo usada por outro aplicativo**

- **ID da notificação** – a ID exclusiva da notificação

  -   Exemplo – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**

- **ID de solicitação do cliente** – a ID de solicitação específica feita pelo seu navegador

  -   Exemplo – **302fd775-3329-4670-a9f3-bea37004f0bc**

- **Carimbo de Data/Hora UTC** – o carimbo de data/hora durante o qual a notificação ocorreu, em UTC

  -   Exemplo – **2017-03-23T19:50:43.7583681Z**

- **ID da transação interna** – a ID interna que podemos usar para pesquisar o erro em nossos sistemas

  -   Exemplo – **71a2f329-ca29-402f-aa72-bc00a7aca603**

- **UPN** – o usuário que realizou a operação

  -   Exemplo – **tperkins \@ f128.info**

- **ID do locatário** – a ID exclusiva do locatário do qual o usuário que realizou a operação era membro

  -   Exemplo – **7918d4b5-0442-4a97-be2d-36f9f9962ece**

- **ID de objeto de usuário** – a ID exclusiva do usuário que realizou a operação

  -   Exemplo – **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>Itens de notificação detalhados

-   **Nome de Exibição** – **(pode estar vazio)** um nome de exibição mais detalhado do erro

    -   Exemplo – **Configurações do proxy do aplicativo**

-   **Status** – o status específico da notificação

    -   Exemplo – **falha**

-   **ID de objeto** – **(pode estar vazio)** a ID de objeto na qual a operação foi executada

    -   Exemplo – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **Detalhes** – a descrição detalhada do que ocorreu como resultado da operação

    -   Exemplo – a **URL interna `https://bing.com/` é inválida porque já está em uso**

-   **Copiar erro** – clique no **ícone de cópia** à direita da caixa de texto **Copiar erro** para copiar todos os detalhes de notificação para compartilhar com um suporte ou grupo de produtos 
-   engenheiro

    -   Exemplo ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'https://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'https://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

