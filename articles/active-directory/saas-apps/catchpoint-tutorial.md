---
title: 'Tutorial: Integração do SSO (logon único) do Azure Active Directory ao Catchpoint'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Catchpoint.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ab3eead7-8eb2-4c12-bb3a-0e46ec899d37
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/27/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7b19e286d299811a950df05f93d221bd710676ea
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80743501"
---
# <a name="tutorial-azure-active-directory-single-sign-on-integration-with-catchpoint"></a>Tutorial: Integração de logon único do Microsoft Azure Active Directory ao Catchpoint

Neste tutorial, você aprenderá a integrar o Catchpoint ao Microsoft Azure AD (Microsoft Azure Active Directory). Ao integrar o Catchpoint ao Azure AD, você pode:

* Controlar o acesso do usuário ao Catchpoint do Azure AD.
* Habilitar as credenciais automáticas do Catchpoint para usuários com contas do Microsoft Azure AD.
* Gerenciar suas contas em um local central: o portal do Azure.

Para saber mais sobre a integração de aplicativos SaaS ao Azure AD, consulte [O que é o acesso de aplicativos e o logon único com o Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Pré-requisitos

Para começar, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Caso você não tenha uma assinatura, obtenha uma [conta gratuita](https://azure.microsoft.com/free/).
* Uma assinatura do Catchpoint com o SSO (logon único) habilitado.

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o SSO do Azure AD em um ambiente de teste.

* O Catchpoint dá suporte ao SSO iniciado por SP e iniciado por IDP.
* O Catchpoint dá suporte ao provisionamento de usuário JIT (Just-In-Time).
* Depois de configurar o Catchpoint, você pode impor o controle de sessão. Essa precaução protege contra exfiltração e infiltração dos dados confidenciais de sua organização em tempo real. O controle de sessão é uma extensão do acesso condicional. [Saiba como impor o controle de sessão com o Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="add-catchpoint-from-the-gallery"></a>Adicionar o Catchpoint por meio da galeria

Para configurar a integração do Catchpoint ao Microsoft Azure AD, adicione o Catchpoint à lista de aplicativos SaaS gerenciados.

1. Entre no [portal do Azure](https://portal.azure.com) com uma conta Microsoft corporativa, de estudante ou pessoal.
1. No painel esquerdo, selecione o serviço **Microsoft Azure Active Directory**.
1. Acesse **Aplicativos Empresariais** e, em seguida, selecione **Todos os Aplicativos**.
1. Para adicionar um novo aplicativo, selecione **Novo aplicativo**.
1. Na seção **Adicionar por meio da galeria**, digite **Catchpoint** na caixa de pesquisa.
1. Selecione **Catchpoint** no painel de resultados e, em seguida, adicione o aplicativo. Aguarde alguns segundos enquanto o aplicativo é adicionado ao seu locatário.

## <a name="configure-and-test-azure-ad-single-sign-on-for-catchpoint"></a>Configurar e testar o logon único do Azure AD para o Catchpoint

Para que o SSO funcione, você precisa vincular um usuário do Microsoft Azure AD a um usuário no Catchpoint. Para este tutorial, configuraremos um usuário de teste chamado **B.Fernandes**. 

Conclua as seguintes seções:

1. [Configurar o SSO do Microsoft Azure AD](#configure-azure-ad-sso) para habilitar esse recurso para seus usuários.
    * [Criar um usuário de teste do Microsoft Azure AD](#create-an-azure-ad-test-user) para testar o logon único do Microsoft Azure AD com B.Fernandes.
    * [Atribuir o usuário de teste do Microsoft Azure AD](#assign-the-azure-ad-test-user) para permitir que B.Fernandes use o logon único do Microsoft Azure AD.
1. [Configurar o SSO do Catchpoint](#configure-catchpoint-sso) para definir as configurações de logon único no lado do aplicativo.
    * [Criar usuário de teste do Catchpoint](#create-a-catchpoint-test-user) para permitir a vinculação da conta de teste do Microsoft Azure AD de B.Fernandes com uma conta de usuário semelhante no Catchpoint.
1. [Testar o SSO](#test-sso) para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estas etapas no portal do Azure para habilitar o SSO do Microsoft Azure AD:

1. Entre no [portal do Azure](https://portal.azure.com/).
1. Na página de integração de aplicativos do **Catchpoint**, localize a seção **Gerenciar** e selecione **Logon único**.
1. Na página **Selecionar um método de logon único**, escolha **SAML**.
1. Na página **Configurar o logon único com o SAML**, selecione o ícone de caneta para editar as configurações da **Configuração básica de SAML**.

   ![Editar a Configuração Básica de SAML](common/edit-urls.png)

1. Configurar o modo iniciado para o Catchpoint:
   - Para o modo iniciado por **IDP**, insira os valores dos seguintes campos:
     - Para **Identificador**: `https://portal.catchpoint.com/SAML2`
     - Para **URL de resposta**: `https://portal.catchpoint.com/ui/Entry/SingleSignOn.aspx`
   - Para o modo iniciado por **SP**, selecione **Definir URLs adicionais** e insira o seguinte valor:
     - Para **URL de logon único**: `https://portal.catchpoint.com/ui/Entry/SingleSignOn.aspx`

1. O aplicativo Catchpoint espera que as declarações SAML estejam em um formato específico. Adicione mapeamentos de atributos personalizados à configuração de atributos de token SAML. A seguinte tabela contém a lista dos atributos padrão:

    | Nome | Atributo de origem|
    | ------------ | --------- |
    | Givenname | user.givenneame |
    | Sobrenome | user.surname |
    | Emailaddress | user.mail |
    | Nome | user.userprincipalname |
    | Identificador Exclusivo do Usuário | user.userprincipalname |

    ![Captura de tela da lista de Atributos do usuário e Declarações](common/default-attributes.png)

1. Além disso, o aplicativo Catchpoint espera que outro atributo seja passado em uma resposta SAML. Confira a tabela a seguir. Esse atributo também é preenchido previamente, mas você pode examiná-lo e atualizá-lo de acordo com suas necessidades.

    | Nome | Atributo de origem|
    | ------------ | --------- |
    | namespace | user.assignedrole |

    > [!NOTE]
    > A declaração `namespace` precisa ser mapeada com o nome da conta. Esse nome da conta deve ser configurado com uma função no Microsoft Azure AD para transmitir novamente na resposta SAML. Para saber mais sobre as funções no Microsoft Azure AD, confira [Configurar a declaração de função emitida no token SAML para aplicativos empresariais](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management).

1. Acesse a página **Configurar o logon único com o SAML**. Na seção **Certificado de autenticação SAML**, localize **Certificados (Base64)** . Selecione **Baixar** para salvar o certificado em seu computador.

    ![O link de download do certificado](common/certificatebase64.png)

1. Na seção **Configurar o Catchpoint**, copie as URLs necessárias em uma etapa posterior.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

Nesta seção, você usará o portal do Azure para criar um usuário de teste do Microsoft Azure AD chamado B.Fernandes.

1. No painel esquerdo do portal do Azure, selecione **Azure Active Directory** > **Usuários** > **Todos os usuários**.
1. Selecione **Novo usuário** na parte superior da tela.
1. Nas propriedades do **Usuário**, siga estas etapas:
   1. No campo **Nome**, insira `B.Simon`.  
   1. No campo **Nome de usuário**, insira username@companydomain.extension. Por exemplo, insira `B.Simon@contoso.com`.
   1. Selecione a caixa de seleção **Mostrar Senha**. Observe o valor da senha exibido.
   1. Selecione **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que B.Fernandes use o logon único do Azure permitindo acesso ao Catchpoint.

1. No portal do Azure, selecione **Aplicativos Empresariais** > **Todos os aplicativos**.
1. Na lista de aplicativos, selecione **Catchpoint**.
1. Na página de visão geral do aplicativo, localize a seção **Gerenciar** e escolha **Usuários e grupos**.

   ![O link “Usuários e grupos”](common/users-groups-blade.png)

1. Selecione **Adicionar usuário** e, em seguida, selecione **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O link “Adicionar usuário”](common/add-assign-user.png)

1. Na caixa de diálogo **Usuários e grupos**, selecione **B.Fernandes** na lista de usuários. Clique em **Selecionar** na parte inferior da tela.
1. Se você esperar um valor de função na instrução de declaração SAML, veja a caixa de diálogo **Selecionar função** e escolha a função de usuário na lista. Clique no botão **Selecionar** na parte inferior da tela.
1. Na caixa de diálogo **Adicionar Atribuição**, selecione **Atribuir**.

## <a name="configure-catchpoint-sso"></a>Configurar o SSO do Catchpoint

1. Em outra janela do navegador da Web, entre no aplicativo Catchpoint como administrador.

1. Selecione o ícone de **Configurações** e, em seguida, **Provedor de identidade SSO**.

    ![Captura de tela das configurações do Catchpoint com provedor de identidade SSO selecionado](./media/catchpoint-tutorial/configuration1.png)

1. Na página **Logon único**, insira os seguintes campos:

   ![Captura de tela da página de logon único do Catchpoint](./media/catchpoint-tutorial/configuration2.png)

   Campo | Valor
   ----- | ----- 
   **Namespace** | Um valor de namespace válido.
   **Emissor do provedor de identidade** | O valor `Azure AD Identifier` do portal do Azure.
   **URL de logon único** | O valor `Login URL` do portal do Azure.
   **Certificado** | O conteúdo do arquivo `Certificate (Base64)` baixado do portal do Azure. Use o bloco de notas para ver e copiar.

   Você também pode carregar o **XML de metadados de federação** selecionando a opção **Carregar metadados**.

1. Clique em **Salvar**.

### <a name="create-a-catchpoint-test-user"></a>Criar um usuário de teste do Catchpoint

O Catchpoint dá suporte ao provisionamento de usuário just-in-time, que está habilitado por padrão. Você não tem itens de ação nesta seção. Se B.Fernandes ainda não existir como usuário no Catchpoint, será criado após a autenticação.

## <a name="test-sso"></a>Testar o SSO

Nesta seção, você testará a configuração de logon único do Azure AD usando o portal Meus Aplicativos.

Ao selecionar a peça do Catchpoint no portal Meus Aplicativos, você deverá ser conectado automaticamente ao aplicativo Catchpoint com o SSO configurado. Para obter mais informações sobre o portal Meus Aplicativos, confira [Entrar e iniciar aplicativos no portal Meus Aplicativos](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access).

> [!NOTE]
> Quando você entrar no aplicativo Catchpoint por meio da página de logon, depois de fornecer as **Credenciais do Catchpoint**, insira o valor válido do **Namespace** no campo **Credenciais da empresa (SSO)** e selecione **Logon**.
> 
> ![Configuração do Catchpoint](./media/catchpoint-tutorial/loginimage.png)

## <a name="additional-resources"></a>Recursos adicionais

- [Lista de tutoriais sobre como integrar aplicativos SaaS ao Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Experimentar o Catchpoint com o Azure AD](https://aad.portal.azure.com/)

- [O que é controle de sessão no Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
