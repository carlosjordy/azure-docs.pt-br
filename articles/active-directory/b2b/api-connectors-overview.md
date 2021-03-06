---
title: Sobre conectores de API em fluxos de inscrição de autoatendimento-Azure AD
description: Use os conectores de API do Azure Active Directory (AD do Azure) para personalizar e estender seus fluxos de usuário de inscrição de autoatendimento usando APIs da Web.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 06/16/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1c5e546c6eac77c4952a0d32d360f49d4251d49d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84905170"
---
# <a name="use-api-connectors-to-customize-and-extend-self-service-sign-up"></a>Usar conectores de API para personalizar e estender a inscrição de autoatendimento 

## <a name="overview"></a>Visão geral 
Como desenvolvedor ou administrador de ti, você pode usar conectores de API para integrar seus [fluxos de usuário de inscrição de autoatendimento](self-service-sign-up-overview.md) com sistemas externos, aproveitando as APIs da Web. Por exemplo, você pode usar conectores de API para:

- [**Integre com um fluxo de trabalho de aprovação personalizado**](self-service-sign-up-add-approvals.md). Conecte-se a um sistema de aprovação personalizado para gerenciar a criação da conta.
- [**Execute a verificação de identidade**](code-samples-self-service-sign-up.md#identity-verification). Use um serviço de verificação de identidade para adicionar um nível extra de segurança às decisões de criação de conta.
- **Validar os dados de entrada do usuário**. Validar contra dados de usuário malformados ou inválidos. Por exemplo, você pode validar dados fornecidos pelo usuário em relação a dados existentes em um repositório de dados externo ou lista de valores permitidos. Se for inválido, você poderá solicitar que um usuário forneça dados válidos ou impedir que o usuário continue o fluxo de inscrição.
- **Substituir atributos de usuário**. Reformate ou atribua um valor a um atributo coletado do usuário. Por exemplo, se um usuário inserir o nome com todas as letras maiúsculas ou minúsculas, você poderá formatar o nome usando somente a primeira letra maiúscula. 
<!-- - **Enrich user data**. Integrate with your external cloud systems that store user information to integrate them with the sign-up flow. For example, your API can receive the user's email address, query a CRM system, and return the user's loyalty number. Returned claims can be used to pre-fill form fields or return additional data in the application token.  -->
- **Executar a lógica de negócios personalizada**. Você pode disparar eventos downstream em seus sistemas de nuvem para enviar notificações por push, atualizar bancos de dados corporativos, gerenciar permissões, auditar bancos de dados e executar outras ações personalizadas.

Um conector de API representa um contrato entre o Azure AD e um ponto de extremidade de API definindo o ponto de extremidade HTTP, a autenticação, a solicitação e a resposta esperada. Depois de configurar um conector de API, você pode habilitá-lo para uma etapa específica em um fluxo de usuário. Quando um usuário atinge essa etapa no fluxo de inscrição, o conector de API é invocado e materializa como uma solicitação HTTP POST, enviando declarações selecionadas como pares de chave-valor em um corpo JSON. A resposta da API pode afetar a execução do fluxo do usuário. Por exemplo, a resposta da API pode impedir que um usuário se inscreva, pedir que o usuário insira novamente as informações ou substitua e acrescente atributos de usuário.

## <a name="where-you-can-enable-an-api-connector-in-a-user-flow"></a>Onde você pode habilitar um conector de API em um fluxo de usuário

Há dois locais em um fluxo de usuário onde você pode habilitar um conector de API:

- Depois de entrar com um provedor de identidade
- Antes de criar o usuário

Em ambos os casos, os conectores de API são invocados durante a inscrição, não entrem.

### <a name="after-signing-in-with-an-identity-provider"></a>Depois de entrar com um provedor de identidade

Um conector de API nesta etapa no processo de inscrição é invocado imediatamente depois que o usuário é autenticado com um provedor de identidade (Google, Facebook, Azure AD). Esta etapa precede a ***página coleção de atributos***, que é o formulário apresentado ao usuário para coletar atributos de usuário. Veja a seguir exemplos de cenários de conector de API que você pode habilitar nesta etapa:

- Use o email ou a identidade federada que o usuário forneceu para pesquisar declarações em um sistema existente. Retorne essas declarações do sistema existente, preencha previamente a página de coleção de atributos e torne-as disponíveis para retornar no token.
- Validar se o usuário está incluído em uma lista de permissão ou negação e controlar se eles podem continuar com o fluxo de inscrição.

### <a name="before-creating-the-user"></a>Antes de criar o usuário

Um conector de API nesta etapa no processo de inscrição é invocado após a página de coleção de atributos, se um for incluído. Essa etapa é sempre invocada antes de uma conta de usuário ser criada no Azure AD. Veja a seguir exemplos de cenários que você pode habilitar neste ponto durante a inscrição:

- Valide os dados de entrada do usuário e peça a um usuário para reenviar os dados.
- Bloquear uma inscrição de usuário com base nos dados inseridos pelo usuário.
- Execute a verificação de identidade.
- Consultar sistemas externos para obter dados existentes sobre o usuário para retorná-los no token do aplicativo ou armazená-los no Azure AD.

<!-- > [!IMPORTANT]
> If an invalid response is returned or another error occurs (for example, a network error), the user will be redirected to the app with the error re -->

## <a name="next-steps"></a>Próximas etapas
- Saiba como [Adicionar um conector de API a um fluxo de usuário](self-service-sign-up-add-api-connector.md)
- Saiba como [Adicionar um sistema de aprovação personalizado à inscrição de autoatendimento](self-service-sign-up-add-approvals.md)