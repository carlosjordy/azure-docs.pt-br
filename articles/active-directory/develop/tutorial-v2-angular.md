---
title: Tutorial de aplicativo de página única Angular – Azure
titleSuffix: Microsoft identity platform
description: Saiba como aplicativos SPA Angular podem chamar uma API que exige tokens de acesso do ponto de extremidade da plataforma de identidade da Microsoft.
services: active-directory
author: hamiltonha
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 03/05/2020
ms.author: hahamil
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 7cd2d5d8728e2a0539d5f106ab39c563e6e7c382
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86231685"
---
# <a name="tutorial-sign-in-users-and-call-the-microsoft-graph-api-from-an-angular-single-page-application"></a>Tutorial: Conectar usuários e chamar a API do Microsoft Graph de um aplicativo de página única Angular

Este tutorial demonstra como um aplicativo SPA (aplicativo de página única) Angular pode:
- Conectar contas pessoais, contas corporativas ou de estudante.
- Adquirir um token de acesso.
- Chamar a API do Microsoft Graph ou outras APIs que exigem tokens de acesso do *ponto de extremidade da plataforma de identidade da Microsoft*.

>[!NOTE]
>Este tutorial explica como criar um SPA Angular usando a MSAL (Biblioteca de Autenticação da Microsoft). Se desejar baixar um aplicativo de exemplo, confira o [guia de início rápido](quickstart-v2-angular.md).

## <a name="how-the-sample-app-works"></a>Como o aplicativo de exemplo funciona

![Diagrama que mostra como funciona o aplicativo de exemplo gerado neste tutorial](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.svg)

### <a name="more-information"></a>Mais informações

O aplicativo de exemplo criado neste tutorial permite que um SPA Angular consulte a API do Microsoft Graph ou uma API Web que aceita tokens do ponto de extremidade da plataforma de identidade da Microsoft. O MSAL para a biblioteca do Angular é um wrapper da biblioteca MSAL.js principal. Ele permite que aplicativos Angular (6 em diante) autentiquem usuários corporativos usando o Microsoft Azure Active Directory, usuários com conta Microsoft e usuários com identidade social (como Facebook, Google, LinkedIn). A biblioteca também permite que os aplicativos tenham acesso aos serviços em nuvem da Microsoft ou ao Microsoft Graph.

Nesse cenário, depois que um usuário se conecta, um token de acesso é adicionado às solicitações HTTP por meio do cabeçalho de autorização. A aquisição e a renovação de tokens são manipuladas pela MSAL.

### <a name="libraries"></a>Bibliotecas

Este tutorial usa a seguinte biblioteca:

|Biblioteca|Descrição|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|Biblioteca de Autenticação da Microsoft para Wrapper Angular JavaScript|

Você pode localizar o código-fonte da biblioteca MSAL.js no repositório [AzureAD/microsoft-authentication-library-for-js](https://github.com/AzureAD/microsoft-authentication-library-for-js) no GitHub.

## <a name="prerequisites"></a>Pré-requisitos

Para este tutorial, você precisa do:

* Um servidor Web local, como [Node.js](https://nodejs.org/en/download/). As instruções neste tutorial se baseiam no Node.js.
* Um IDE (ambiente de desenvolvimento integrado), como o [Visual Studio Code](https://code.visualstudio.com/download), para editar os arquivos de projeto.

## <a name="create-your-project"></a>Criar seu projeto

Gere um aplicativo Angular usando os seguintes comandos npm:

```Bash
npm install -g @angular/cli@8                    # Install the Angular CLI
npm install @angular/material@8 @angular/cdk@8   # Install the Angular Material component library (optional, for UI)
ng new my-application --routing=true --style=css # Generate a new Angular app
npm install msal @azure/msal-angular             # Install MSAL and MSAL Angular in your application
ng generate component page-name                  # To add a new page (such as a home or profile page)
```

## <a name="register-your-application"></a>Registre seu aplicativo

Siga as [instruções para registrar um aplicativo de página única](https://docs.microsoft.com/azure/active-directory/develop/scenario-spa-app-registration) no portal do Azure.

Na página **Visão geral** do aplicativo do seu registro, anote o valor da **ID do aplicativo (cliente)** para uso posterior.

Registre o valor **URI de Redirecionamento** como **http://localhost:4200/** e habilite as configurações de concessão implícita.

## <a name="configure-the-application"></a>Configurar o aplicativo

1. Na pasta *src/app*, edite o *app.module.ts* e adicione o `MSALModule` ao `imports`, bem como a constante `isIE`:

    ```javascript
    const isIE = window.navigator.userAgent.indexOf('MSIE ') > -1 || window.navigator.userAgent.indexOf('Trident/') > -1;
    @NgModule({
      declarations: [
        AppComponent
      ],
      imports: [
        BrowserModule,
        AppRoutingModule,
        MsalModule.forRoot({
          auth: {
            clientId: 'Enter_the_Application_Id_here', // This is your client ID
            authority: 'Enter_the_Cloud_Instance_Id_Here'/'Enter_the_Tenant_Info_Here', // This is your tenant ID
            redirectUri: 'Enter_the_Redirect_Uri_Here'// This is your redirect URI
          },
          cache: {
            cacheLocation: 'localStorage',
            storeAuthStateInCookie: isIE, // Set to true for Internet Explorer 11
          },
        }, {
          popUp: !isIE,
          consentScopes: [
            'user.read',
            'openid',
            'profile',
          ],
          unprotectedResources: [],
          protectedResourceMap: [
            ['https://graph.microsoft.com/v1.0/me', ['user.read']]
          ],
          extraQueryParameters: {}
        })
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    ```

    Substitua esses valores:

    |Nome do valor|Sobre o|
    |---------|---------|
    |Enter_the_Application_Id_Here|Na página **Visão Geral** do seu registro de aplicativo, esta é seu valor **ID do Aplicativo (cliente)** . |
    |Enter_the_Cloud_Instance_Id_Here|Essa é a instância da nuvem do Azure. Para a nuvem principal ou global do Azure, insira **https://login.microsoftonline.com** . Para nuvens nacionais (por exemplo, China), confira [Nuvens nacionais](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud).|
    |Enter_the_Tenant_Info_Here| Defina como uma das seguintes opções: Se o aplicativo der suporte a *contas neste diretório organizacional*, substitua esse valor pela ID do diretório (locatário) ou pelo nome do locatário (por exemplo, **contoso.microsoft.com**). Se o aplicativo for compatível com as *contas em qualquer diretório organizacional*, substitua esse valor por **organizações**. Se o seu aplicativo for compatível com as *contas em qualquer diretório organizacional e contas pessoais da Microsoft*, substitua esse valor por **comum**. Para restringir o suporte a *contas pessoais da Microsoft*, substitua esse valor por **consumidores**. |
    |Enter_the_Redirect_Uri_Here|Substitua por **http://localhost:4200** .|

    Para saber mais sobre opções configuráveis disponíveis, confira [Inicializar aplicativos cliente](msal-js-initializing-client-applications.md).

2. Na parte superior do mesmo arquivo, adicione a seguinte instrução de importação:

    ```javascript
    import { MsalModule, MsalInterceptor } from '@azure/msal-angular';
    ```

3. Adicione as seguintes instruções de importação à parte superior de `src/app/app.component.ts`:

    ```javascript
    import { MsalService, BroadcastService } from '@azure/msal-angular';
    import { Component, OnInit } from '@angular/core';
    ```
## <a name="sign-in-a-user"></a>Conectar um usuário

Adicione o seguinte código a `AppComponent` para conectar um usuário:

```javascript
export class AppComponent implements OnInit {
    constructor(private broadcastService: BroadcastService, private authService: MsalService) { }
    
    ngOnInit() { }

    login() {
        const isIE = window.navigator.userAgent.indexOf('MSIE ') > -1 || window.navigator.userAgent.indexOf('Trident/') > -1;

        if (isIE) {
          this.authService.loginRedirect({
            extraScopesToConsent: ["user.read", "openid", "profile"]
          });
        } else {
          this.authService.loginPopup({
            extraScopesToConsent: ["user.read", "openid", "profile"]
          });
        }
    }
}
```

> [!TIP]
> É recomendável usar `loginRedirect` para usuários do Internet Explorer.

## <a name="acquire-a-token"></a>Adquirir um token

### <a name="angular-interceptor"></a>Interceptor Angular

A MSAL Angular fornece uma classe `Interceptor` que adquire automaticamente tokens para solicitações de saída que usam o cliente `http` do Angular para recursos protegidos conhecidos.

Primeiro, inclua a classe `Interceptor` como um provedor para seu aplicativo:

```javascript
import { MsalInterceptor, MsalModule } from "@azure/msal-angular";
import { HTTP_INTERCEPTORS, HttpClientModule } from "@angular/common/http";

@NgModule({
    // ...
    providers: [
        {
            provide: HTTP_INTERCEPTORS,
            useClass: MsalInterceptor,
            multi: true
        }
    ]
}
```

Em seguida, forneça um mapa dos recursos protegidos para `MsalModule.forRoot()` como `protectedResourceMap` e inclua esses escopos no `consentScopes`:

```javascript
@NgModule({
  // ...
  imports: [
    // ...
    MsalModule.forRoot({
      auth: {
        clientId: 'Enter_the_Application_Id_here', // This is your client ID
        authority: 'https://login.microsoftonline.com/Enter_the_Tenant_Info_Here', // This is your tenant info
        redirectUri: 'Enter_the_Redirect_Uri_Here' // This is your redirect URI
      },
      cache: {
        cacheLocation: 'localStorage',
        storeAuthStateInCookie: isIE, // Set to true for Internet Explorer 11
      },
    },
    {
      popUp: !isIE,
      consentScopes: [
        'user.read',
        'openid',
        'profile',
      ],
      unprotectedResources: [],
      protectedResourceMap: [
        ['https://graph.microsoft.com/v1.0/me', ['user.read']]
      ],
      extraQueryParameters: {}
    })
  ],
});
```

Por fim, recupere o perfil de um usuário com uma solicitação HTTP:

```JavaScript
const graphMeEndpoint = "https://graph.microsoft.com/v1.0/me";

getProfile() {
  this.http.get(graphMeEndpoint).toPromise()
    .then(profile => {
      this.profile = profile;
    });
}
```

### <a name="acquiretokensilent-acquiretokenpopup-acquiretokenredirect"></a>acquireTokenSilent, acquireTokenPopup, acquireTokenRedirect
A MSAL usa três métodos para adquirir tokens: `acquireTokenRedirect`, `acquireTokenPopup` e `acquireTokenSilent`. No entanto, é recomendável usar a classe `MsalInterceptor` para aplicativos Angular, conforme mostrado na seção anterior.

#### <a name="get-a-user-token-silently"></a>Obter um token de usuário no modo silencioso

O método `acquireTokenSilent` manipula as aquisições e a renovação de tokens sem interação do usuário. Após o método `loginRedirect` ou `loginPopup` ser executado pela primeira vez, o `acquireTokenSilent` é normalmente usado para obter tokens usados para acessar recursos protegidos nas chamadas posteriores. As chamadas para solicitar ou renovar tokens são feitas no modo silencioso.

```javascript
const requestObj = {
    scopes: ["user.read"]
};

this.authService.acquireTokenSilent(requestObj).then(function (tokenResponse) {
    // Callback code here
    console.log(tokenResponse.accessToken);
}).catch(function (error) {
    console.log(error);
});
```

Nesse código, `scopes` contém os escopos solicitados para retorno no token de acesso para a API.

Por exemplo:

* `["user.read"]` para Microsoft Graph
* `["<Application ID URL>/scope"]` para APIs Web personalizadas (ou seja, `api://<Application ID>/access_as_user`)

#### <a name="get-a-user-token-interactively"></a>Obter um token de usuário interativamente

Às vezes, você precisa que o usuário interaja com o ponto de extremidade da plataforma de identidade da Microsoft. Por exemplo:

* Os usuários podem precisar reinserir as credenciais porque a senha expirou.
* Seu aplicativo está solicitando acesso a escopos de recursos adicionais com os quais o usuário precisa concordar.
* A autenticação de dois fatores é necessária.

O padrão recomendado para a maioria dos aplicativos é chamar `acquireTokenSilent` primeiro, depois capturar a exceção e, em seguida, chamar `acquireTokenPopup` (ou `acquireTokenRedirect`) para iniciar uma solicitação interativa.

Chamar `acquireTokenPopup` resulta em uma janela pop-up de credenciais. Como alternativa, `acquireTokenRedirect` redireciona os usuários para o ponto de extremidade da plataforma de identidade da Microsoft. Nessa janela, os usuários precisam confirmar suas credenciais, dar consentimento ao recurso necessário ou realizar a autenticação de dois fatores.

```javascript
  const requestObj = {
      scopes: ["user.read"]
  };

  this.authService.acquireTokenPopup(requestObj).then(function (tokenResponse) {
      // Callback code here
      console.log(tokenResponse.accessToken);
  }).catch(function (error) {
      console.log(error);
  });
```

> [!NOTE]
> Este guia de início rápido usa os métodos `loginRedirect` e `acquireTokenRedirect` com o Microsoft Internet Explorer devido a um [problema conhecido](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) relacionado ao tratamento de janelas pop-up pelo Internet Explorer.

## <a name="log-out"></a>Faça logoff

Adicione o seguinte código para fazer logoff de um usuário:

```javascript
logout() {
  this.authService.logout();
}
```

## <a name="add-ui"></a>Adicionar a interface do usuário
Para obter um exemplo de como adicionar a interface do usuário usando a biblioteca de componentes Angular Material, confira o [aplicativo de exemplo](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular).

## <a name="test-your-code"></a>Testar seu código

1.  Inicie o servidor Web para escutar a porta executando os seguintes comandos em um prompt de linha de comando na pasta do aplicativo:

    ```bash
    npm install
    npm start
    ```
1. No navegador, digite **http://localhost:4200** ou **http://localhost:{port}** , em que *port* é a porta na qual o servidor Web está escutando.


### <a name="provide-consent-for-application-access"></a>Fornecer autorização para acesso de aplicativo

Na primeira vez que começar a entrar no aplicativo, você deverá permitir acesso ao seu perfil e permitir que ele conecte você:

![A janela de "Permissões solicitadas"](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

## <a name="add-scopes-and-delegated-permissions"></a>Adicionar escopos e permissões delegadas

A API do Microsoft Graph requer o escopo *user.read* para ler o perfil do usuário. Por padrão, este escopo é adicionado automaticamente a cada aplicativo registrado no portal de registro. Outras APIs do Microsoft Graph, bem como APIs personalizadas do servidor de back-end, podem exigir escopos adicionais. Por exemplo, a API do Microsoft Graph requer o escopo *Calendars.Read* para listar os calendários do usuário.

Para acessar os calendários do usuário no contexto de um aplicativo, adicione a permissão delegada *Calendars.Read* às informações de registro do aplicativo. Em seguida, adicione o escopo *Calendars.Read* à chamada `acquireTokenSilent`.

>[!NOTE]
>Talvez o usuário precise fornecer autorizações adicionais à medida que o número de escopos aumentar.

Se uma API de back-end não exigir um escopo (não recomendado), você poderá usar a *clientId* como o escopo nas chamadas para obter tokens.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Próximas etapas

Se você é novo no gerenciamento de identidade e acesso, temos vários artigos para ajudar você a aprender conceitos de autenticação modernos, começando com [autenticação vs. autorização](authentication-vs-authorization.md).

Se você quiser aprofundar-se no desenvolvimento de aplicativos de página única na plataforma de identidade da Microsoft, a série de artigos [Cenário: aplicativo de página única](scenario-spa-overview.md) em várias partes pode ajudar você a começar.
