---
title: Chamar uma ASP.NET Web API protegida pela plataforma de identidade da Microsoft
description: Neste início rápido, saiba como chamar uma ASP.NET Web API protegida pela plataforma de identidade da Microsoft de um aplicativo WPF (Área de Trabalho do Windows). O cliente do WPF autentica um usuário, solicita um token de acesso e chama a API Web.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 12/12/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:ASP.NET
ms.openlocfilehash: 22bf7e85a48e0d138bfdbca82cf032287d982899
ms.sourcegitcommit: 61d92af1d24510c0cc80afb1aebdc46180997c69
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85339582"
---
# <a name="quickstart-call-an-aspnet-web-api-protected-by-microsoft-identity-platform"></a>Início Rápido: Chamar uma ASP.NET Web API protegida pela plataforma de identidade da Microsoft

Neste guia de início rápido, você expõe uma API Web e a protege para que somente o usuário autenticado possa acessá-la. Este exemplo mostra como expor uma ASP.NET Web API para que ela possa aceitar tokens emitidos por contas pessoais (incluindo outlook.com, live.com e outras), bem como contas corporativas e de estudante de qualquer empresa ou organização integrada à plataforma de identidade da Microsoft.

O exemplo também inclui um cliente de aplicativo de Área de Trabalho do Windows (WPF) que demonstra como você pode solicitar um token de acesso para acessar uma API Web.

## <a name="prerequisites"></a>Pré-requisitos

Para executar este exemplo, você precisará do seguinte:

* Visual Studio 2017 ou 2019.  Baixe o [Visual Studio gratuitamente](https://www.visualstudio.com/downloads/).

* Uma [conta Microsoft](https://www.outlook.com) ou um [Programa para Desenvolvedores do Microsoft 365](/office/developer-program/office-365-developer-program)

## <a name="download-or-clone-this-sample"></a>Baixar ou clonar este exemplo

Você pode clonar este exemplo do shell ou da linha de comando:

  ```console
  git clone https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git
  ```

Você pode [baixar o exemplo como um arquivo .zip](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip).

## <a name="register-your-web-api-in-the-application-registration-portal"></a>Registrar sua API Web no portal de registro de aplicativo

### <a name="choose-the-azure-ad-tenant-where-you-want-to-create-your-applications"></a>Escolha o locatário do Azure AD no qual você deseja criar seus aplicativos

Se você quiser registrar seus aplicativos manualmente, como uma primeira etapa, você precisará:

1. Entre no [portal do Azure](https://portal.azure.com) usando uma conta corporativa ou de estudante ou uma conta pessoal da Microsoft.
1. Se sua conta estiver presente em mais de um locatário do Azure AD, selecione seu perfil no canto superior direito no menu na parte superior da página e, em seguida, **alternar diretório**.
   Altere a sessão do portal para o locatário do Azure AD desejado.

### <a name="register-the-service-app-todolistservice"></a>Registrar o aplicativo de serviço (TodoListService)

1. Navegue até a página [Registros de aplicativo](https://go.microsoft.com/fwlink/?linkid=2083908) da plataforma de identidade da Microsoft para desenvolvedores.
1. Selecione **Novo registro**.
1. Quando a página **Registrar um aplicativo** for exibida, insira as informações de registro do aplicativo:
   - Na seção **Nome**, insira um nome de aplicativo relevante que será exibido aos usuários do aplicativo, por exemplo, `AppModelv2-NativeClient-DotNet-TodoListService`.
   - Altere os **Tipos de conta com suporte** para **Contas em qualquer diretório organizacional**.
   - Selecione **Registrar** para criar o aplicativo.

1. Na página **Visão geral** do aplicativo, localize o valor de **ID do aplicativo (cliente)** e registre-o para uso posterior. Você precisará dele para definir o arquivo de configuração do Visual Studio para este projeto (`ClientId` em `TodoListService\Web.config`).
1. Selecione a seção **Expor uma API** e:
   - Selecione **Adicionar um escopo**
   - aceite o URI da ID do aplicativo proposto (api://{clientId}) selecionando **Salvar e Continuar**
   - Insira os parâmetros s seguir:
     - para **Nome de escopo**, use `access_as_user`
     - Verifique se a opção **Administradores e usuários** está selecionada para **Quem pode consentir**
     - em **Nome de exibição de consentimento do administrador**, digite `Access TodoListService as a user`
     - em **Descrição do consentimento do administrador**, digite `Accesses the TodoListService web API as a user`
     - em **Nome de exibição de consentimento do usuário**, digite `Access TodoListService as a user`
     - em **Descrição do consentimento do usuário**, digite `Accesses the TodoListService web API as a user`
     - Manter **Estado** como **Habilitado**
     - Selecione **Adicionar escopo**

### <a name="configure-the-service-project-to-match-the-registered-web-api"></a>Configurar o projeto de serviço para que corresponda à API Web registrada

1. Abra a solução no Visual Studio e, em seguida, abra o arquivo **Web.config** na raiz do projeto **TodoListService**.
1. Substitua o valor do parâmetro `ida:ClientId` pela **ID do Cliente (ID do Aplicativo)** do aplicativo que você acabou de registrar no Portal de Registro de Aplicativo.

### <a name="add-the-new-scope-to-the-todolistclients-appconfig"></a>Adicione o novo escopo ao arquivo app.config do *TodoListClient*

1. Abra o arquivo **app.config** localizado na pasta raiz do projeto do **TodoListClient** e, em seguida, cole a **ID do Aplicativo** do aplicativo que você acabou de registrar em seu *TodoListService* no parâmetro `TodoListServiceScope`, substituindo a cadeia de caracteres `{Enter the Application ID of your TodoListService from the app registration portal}`.

   > Observação: Verifique se ele usa o seguinte formato:
   >
   > `api://{TodoListService-Application-ID}/access_as_user`
   >
   >(em que {TodoListService-Application-ID} é o GUID que representa a ID do aplicativo para seu TodoListService).

## <a name="register-the-client-app-todolistclient"></a>Registrar o aplicativo cliente (TodoListClient)

Nesta etapa, você configura seu projeto *TodoListClient* registrando um novo aplicativo no Portal de registro de aplicativo. Nos casos em que o cliente e o servidor são considerados *o mesmo aplicativo*, você também pode apenas reutilizar o mesmo aplicativo registrado na "Etapa 2". Usar o mesmo aplicativo será necessário se você quiser que os usuários entrem com contas pessoais da Microsoft

### <a name="register-the-todolistclient-application-in-the-application-registration-portal"></a>Registrar o aplicativo *TodoListClient* no *Portal de registro de aplicativo*

1. Navegue até a página [Registros de aplicativo](https://go.microsoft.com/fwlink/?linkid=2083908) da plataforma de identidade da Microsoft para desenvolvedores.
1. Selecione **Novo registro**.
1. Quando a página **Registrar um aplicativo** for exibida, insira as informações de registro do aplicativo:
   - Na seção **Nome**, insira um nome de aplicativo relevante que será exibido aos usuários do aplicativo, por exemplo, `NativeClient-DotNet-TodoListClient`.
   - Altere os **Tipos de conta com suporte** para **Contas em qualquer diretório organizacional**.
   - Selecione **Registrar** para criar o aplicativo.
1. Na página Visão Geral do aplicativo, selecione a seção **Autenticação**.
   - Na seção **URIs de Redirecionamento** | **URIs de redirecionamento sugeridos para clientes públicos (móvel, área de trabalho)** , marque **https://login.microsoftonline.com/common/oauth2/nativeclient**
   - Clique em **Salvar**.
1. Selecione a seção **Permissões da API**
   - Clique no botão **Adicionar uma permissão** e, em seguida,
   - Selecione a guia **Minhas APIs**.
   - Na lista de APIs, selecione o `AppModelv2-NativeClient-DotNet-TodoListService API` ou o nome que você inseriu para a API Web.
   - Marque a permissão **access_as_user** se ela ainda não estiver marcada. Use a caixa de pesquisa, se necessário.
   - Selecione o botão **Adicionar permissões**

### <a name="configure-your-todolistclient-project"></a>Configurar o projeto do *TodoListClient*

1. No *Portal de registro de aplicativo*, na página **Visão geral**, copie o valor da **ID do aplicativo (cliente)**
1. Abra o arquivo **app.config** localizado na pasta raiz do projeto do **TodoListClient** e cole o valor no valor do parâmetro `ida:ClientId`

## <a name="run-your-project"></a>Execute seu projeto

1. Pressione `<F5>` para executar o projeto. Seu *TodoListClient* deve ser aberto.
1. Selecione **Entrar** no canto superior direito e entre com o mesmo usuário que você usou para registrar seu aplicativo ou um usuário no mesmo diretório.
1. Neste ponto, se você estiver entrando pela primeira vez, talvez seja solicitado a consentir com a API Web do *TodoListService*.
1. A entrada também solicita o token de acesso para o escopo do *access_as_user* acessar a API WEB do *TodoListService* e manipular a lista *To-Do*.

## <a name="pre-authorize-your-client-application"></a>Pré-autorizar seu aplicativo cliente

Uma das maneiras de permitir que os usuários de outros diretórios acessem a API Web é *pré-autorizar* os aplicativos cliente para acessá-la. Isso é feito adicionando as IDs de aplicativo dos aplicativos cliente à lista de aplicativos *pré-autorizados* para a API Web. Ao adicionar um cliente previamente autorizado, você não precisará que o usuário consinta com o uso da API Web. Siga as etapas abaixo para autorizar previamente seu Aplicativo Web:

1. Volte para o *Portal de registro de aplicativo* e abra as propriedades de seu **TodoListService**.
1. Na seção **Expor uma API**, clique em **Adicionar um aplicativo cliente** na seção *Aplicativos cliente autorizados*.
1. No campo *ID do cliente*, Cole a ID do aplicativo `TodoListClient`.
1. Na seção *Escopos autorizados*, selecione o escopo desta API Web `api://<Application ID>/access_as_user`.
1. Pressione o botão **Adicionar aplicativo** na parte inferior da página.

## <a name="run-your-project"></a>Execute seu projeto

1. Pressione `<F5>` para executar o projeto. Seu *TodoListClient* deve ser aberto.
1. Selecione **Entrar** no canto superior direito (ou Limpa Cache/Entrar) e, em seguida, entre usando uma conta Microsoft pessoal (live.com ou hotmail.com) ou uma conta corporativa ou de estudante.

## <a name="optional-restrict-sign-in-access-to-your-application"></a>Opcional: Limite o acesso de entrada para seu aplicativo

Por padrão, quando você baixa este exemplo de código e configura o aplicativo para usar o ponto de extremidade do Azure Active Directory v2 seguindo as etapas anteriores, as contas pessoais, como outlook.com, live.com e outras, bem como contas corporativas ou de estudante de organizações integradas ao Azure AD, podem solicitar tokens e acessar a sua API Web.

Para restringir quem pode entrar em seu aplicativo, use uma destas opções:

### <a name="option-1-restrict-access-to-a-single-organization-single-tenant"></a>Opção 1: Restringir o acesso a uma única organização (locatário único)

Você pode restringir o acesso de entrada a seu aplicativo somente a contas de usuário que estejam em um único locatário do Azure AD, incluindo *contas de convidado* daquele locatário. Esse cenário é comum para *aplicativos de linha de negócios*:

1. Abra o arquivo **App_Start\Startup.Auth** e altere o valor do ponto de extremidade de metadados que é passado para o `OpenIdConnectSecurityTokenProvider` para `"https://login.microsoftonline.com/{Tenant ID}/v2.0/.well-known/openid-configuration"` (você também pode usar o nome do locatário, como `contoso.onmicrosoft.com`).
2. No mesmo arquivo, defina a propriedade `ValidIssuer` no `TokenValidationParameters` como `"https://sts.windows.net/{Tenant ID}/"` e o argumento `ValidateIssuer` como `true`.

### <a name="option-2-use-a-custom-method-to-validate-issuers"></a>Opção 2: Usar um método personalizado para validar emissores

É possível implementar um método personalizado para validar emissores usando o parâmetro **IssuerValidator**. Para obter mais informações sobre como usar esse parâmetro, leia sobre a [classe TokenValidationParameters](/dotnet/api/microsoft.identitymodel.tokens.tokenvalidationparameters?view=azure-dotnet).

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Próximas etapas
Saiba mais sobre o cenário de API Web compatível com a plataforma de identidade da Microsoft:
> [!div class="nextstepaction"]
> [Cenário da API Web protegida](scenario-protected-web-api-overview.md)
