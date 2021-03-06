---
title: Adicionar um aplicativo inexistente na galeria – plataforma de identidade da Microsoft | Microsoft Docs
description: Adicione um aplicativo inexistente na galeria ao seu locatário do Azure AD.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: tutorial
ms.workload: identity
ms.date: 10/24/2019
ms.author: kenwith
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5db8aed0a47e7d8d928ef3287010d60efbc5e5da
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86200444"
---
# <a name="add-an-unlisted-non-gallery-application-to-your-azure-ad-organization"></a>Adicionar um aplicativo não listado (inexistente na galeria) à sua organização do Azure AD

Além das opções da [Galeria de aplicativos do Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/), você pode adicionar um **aplicativo inexistente na galeria**. É possível adicionar qualquer aplicativo que já exista em sua organização ou qualquer aplicativo de terceiros, de um fornecedor que ainda não faça parte da galeria do Azure AD. Dependendo do seu [contrato de licença](https://azure.microsoft.com/pricing/details/active-directory/), os seguintes recursos estão disponíveis:

- Integração de autoatendimento de qualquer aplicativo compatível com provedores de identidade [SAML (Security Assertion Markup Language) 2.0](https://wikipedia.org/wiki/SAML_2.0) (iniciado por SP ou IdP)
- Integração de autoatendimento de qualquer aplicativo Web que tenha uma página de entrada baseada em HTML usando o [SSO baseado em senha](what-is-single-sign-on.md#password-based-sso)
- Conexão de autoatendimento para aplicativos que usam o [protocolo SCIM (Sistema de Gerenciamento de Usuários entre Domínios) para provisionamento de usuário](../app-provisioning/use-scim-to-provision-users-and-groups.md)
- Capacidade de adicionar links aos aplicativos no [inicializador de aplicativos do Office 365](https://www.microsoft.com/microsoft-365/blog/2014/10/16/organize-office-365-new-app-launcher-2/) ou no [Painel de acesso do Azure AD](what-is-single-sign-on.md#linked-sign-on)

Este artigo descreve como adicionar um aplicativo inexistente na galeria aos **Aplicativos Empresariais** no portal do Azure sem escrever código. Se você estiver procurando diretrizes para desenvolvedores sobre como integrar aplicativos personalizados com o Azure AD, veja [Cenários de autenticação do Azure AD](../develop/authentication-scenarios.md). Ao desenvolver um aplicativo que usa um protocolo moderno como [OpenId Connect/OAuth](../develop/active-directory-v2-protocols.md) para autenticar usuários, você pode registrá-lo na plataforma de identidade da Microsoft usando a experiência de [Registros de aplicativo](../develop/quickstart-register-app.md) no portal do Azure.

## <a name="add-a-non-gallery-application"></a>Adicionar um aplicativo inexistente na galeria

1. Entre no [portal do Azure Active Directory](https://aad.portal.azure.com/) usando sua conta Administrador da plataforma de identidade da Microsoft.

2. Selecione **Aplicativos Empresariais** > **Novo aplicativo**.

3. (Opcional, mas recomendado) Na caixa de pesquisa **Procurar na Galeria do Azure AD**, insira o nome de exibição do aplicativo. 

4. Selecione **Criar seu próprio aplicativo**. A página **Criar seu próprio aplicativo** é exibida.

   ![Adicionar aplicativo](media/add-non-gallery-app/create-your-own-application.png)

5. Comece a digitar o nome de exibição do seu novo aplicativo. Se houver aplicativos na galeria com nomes semelhantes, eles aparecerão em uma lista de resultados da pesquisa.

   > [!NOTE]
   > É recomendável usar a versão da galeria do seu aplicativo sempre que possível. Se o aplicativo que você deseja adicionar aparecer nos resultados da pesquisa, selecione o aplicativo e ignore o restante deste procedimento.

6. Em **O que você deseja fazer com o seu aplicativo?** escolha **Integrar qualquer outro aplicativo que você não encontrar na galeria**. Essa opção é normalmente usada para aplicativos SAML e WS-Fed.

   > [!NOTE]
   > As outras duas opções são usadas nos seguintes cenários:
   >* **Configurar o Proxy de Aplicativo para acesso remoto seguro a um aplicativo local** abre a página de configuração para os conectores e o Proxy de Aplicativo do Azure AD.
   >* **Registrar um aplicativo no qual você está trabalhando para se integrar ao Azure AD** abre a página **Registros de aplicativo**. Essa opção é normalmente usada para aplicativos OpenID Connect.

7. Selecione **Criar**. A página **Visão Geral** do aplicativo será aberta.

## <a name="configure-user-sign-in-properties"></a>Configurar propriedades de logon do usuário

1. Selecione **Propriedades** para abrir o painel de propriedades para edição.

    ![Editar painel de propriedades](media/add-non-gallery-app/edit-properties.png)

2. Defina as opções a seguir para determinar como os usuários atribuídos ou não atribuídos ao aplicativo podem entrar no aplicativo e se um usuário pode ver o aplicativo no Painel de Acesso.

    - **Habilitado para que os usuários entrem** determina se os usuários atribuídos ao aplicativo podem entrar nele.
    - A **atribuição de usuário obrigatória** determina se os usuários não atribuídos ao aplicativo podem entrar nele.
    - **Visível para usuário** determina se os usuários atribuídos a um aplicativo podem vê-lo no painel de acesso e no inicializador do O365.

      Comportamento de usuários **atribuídos**:

       | Propriedade do aplicativo | Propriedade do aplicativo | Propriedade do aplicativo | Experiência do usuário atribuído | Experiência do usuário atribuído |
       |---|---|---|---|---|
       | Habilitado para os usuários entrarem? | Atribuição de usuário obrigatória? | Visível para os usuários? | Os usuários atribuídos podem entrar? | Os usuários atribuídos podem ver o aplicativo?* |
       | sim | sim | sim | sim | sim  |
       | sim | sim | não  | sim | não   |
       | sim | não  | sim | sim | sim  |
       | sim | não  | não  | sim | não   |
       | não  | sim | sim | não  | não   |
       | não  | sim | não  | não  | não   |
       | não  | não  | sim | não  | não   |
       | não  | não  | não  | não  | não   |

      Comportamento de usuários **não atribuídos**:

       | Propriedade do aplicativo | Propriedade do aplicativo | Propriedade do aplicativo | Experiência de usuário não atribuído | Experiência de usuário não atribuído |
       |---|---|---|---|---|
       | Habilitado para os usuários entrarem? | Atribuição de usuário obrigatória? | Visível para os usuários? | Os usuários não atribuídos podem entrar? | Os usuários não atribuídos podem ver o aplicativo?* |
       | sim | sim | sim | não  | não   |
       | sim | sim | não  | não  | não   |
       | sim | não  | sim | sim | não   |
       | sim | não  | não  | sim | não   |
       | não  | sim | sim | não  | não   |
       | não  | sim | não  | não  | não   |
       | não  | não  | sim | não  | não   |
       | não  | não  | não  | não  | não   |

     *O usuário pode ver o aplicativo no painel de acesso e no iniciador de aplicativos do Office 365?

3. Para usar um logotipo personalizado, crie um logotipo de 215 x 215 pixels e salve-o no formato PNG. Em seguida, procure o logotipo e carregue-o.

    ![Alterar o logotipo](media/add-non-gallery-app/change-logo.png)

4. Quando terminar, selecione **Salvar**.

## <a name="next-steps"></a>Próximas etapas

Agora que você adicionou o aplicativo à sua organização do Azure AD, [escolha um método de logon único](what-is-single-sign-on.md#choosing-a-single-sign-on-method) que deseja usar e veja o artigo apropriado abaixo:

- [Configurar o logon único baseado em SAML](configure-single-sign-on-non-gallery-applications.md)
- [Configurar o logon único com senha](configure-password-single-sign-on-non-gallery-applications.md)
- [Configurar o logon vinculado](configure-linked-sign-on.md)
