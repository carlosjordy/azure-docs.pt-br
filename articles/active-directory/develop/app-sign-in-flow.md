---
title: Fluxo de entrada de aplicativos com a plataforma de identidade da Microsoft | Azure
titleSuffix: Microsoft identity platform
description: Saiba mais sobre o fluxo de entrada de aplicativos Web, móveis e para desktop na plataforma de identidade da Microsoft (v2.0).
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/18/2020
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: af5b27dc85a276c731a61135ab59ab81f5aaf3c2
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83772192"
---
# <a name="app-sign-in-flow-with-microsoft-identity-platform"></a>Fluxo de entrada de aplicativos com a plataforma de identidade da Microsoft

Este tópico aborda o fluxo de entrada básico para aplicativos web, móveis e para desktop, usando a plataforma de identidade da Microsoft. Confira [Fluxos de autenticação e cenários de aplicativo](authentication-flows-app-scenarios.md) para saber mais sobre cenários de entrada compatíveis com a plataforma de identidade da Microsoft.

## <a name="web-app-sign-in-flow"></a>Fluxo de entrada de aplicativo Web

Quando um usuário acessa um aplicativo Web pelo navegador, acontece o seguinte:

* O aplicativo Web determina se o usuário é autenticado.
* Se o usuário não for autenticado, o aplicativo Web delegará ao Azure AD a entrada do usuário. Essa entrada será compatível com a política da organização, o que pode significar solicitar que o usuário insira suas credenciais, usando [autenticação multifator](../authentication/concept-mfa-howitworks.md) (às vezes chamada de autenticação de dois fatores ou 2FA) ou não usando uma senha (por exemplo, usando o Windows Hello).
* O usuário deve ter permissão para acessar o aplicativo cliente de que necessita. É por isso que os aplicativos do cliente precisam ser registrados no Azure AD, para que a plataforma de identidade da Microsoft possa fornecer tokens que representem o acesso ao qual o usuário tem permissão.

Quando a autenticação do usuário é realizada:

* A plataforma de identidade da Microsoft envia um token para o aplicativo Web.
* No jar de cookie do navegador, é salvo um cookie, associado ao domínio do Azure AD, que contém a identidade do usuário. Na próxima vez que um aplicativo usar o navegador para acessar o ponto de extremidade de autorização da plataforma de identidade da Microsoft, o navegador apresentará o cookie para que o usuário não precise entrar novamente. Essa também é a maneira como o SSO (logon único) é obtido. O cookie é produzido pelo Azure AD e só é reconhecido pelo Azure AD.
* Em seguida, o aplicativo Web valida o token. Se a validação for realizada, o aplicativo Web exibirá a página protegida e salvará um cookie de sessão no jar do cookie do navegador. Quando o usuário navega para outra página, o aplicativo Web sabe que ele já foi autenticado com base no cookie da sessão.

O diagrama de sequência a seguir resume essa interação:

![Processo de autenticação de aplicativo Web](media/authentication-scenarios/web-app-how-it-appears-to-be.png)

### <a name="how-a-web-app-determines-if-the-user-is-authenticated"></a>Como um aplicativo Web determina se o usuário é autenticado

Os desenvolvedores de aplicativos Web podem indicar se todas ou apenas determinadas páginas exigem autenticação. Por exemplo, no ASP.NET/ASP.NET Core, isso é feito adicionando o atributo `[Authorize]` às ações do controlador.

Esse atributo faz com que o ASP.NET verifique se há um cookie de sessão com a identidade do usuário. Se não houver um cookie, o ASP.NET redirecionará a autenticação para o provedor de identidade especificado. Se o provedor de identidade for o Azure AD, o aplicativo Web redirecionará a autenticação para `https://login.microsoftonline.com`, que exibirá uma caixa de diálogo de entrada.

### <a name="how-a-web-app-delegates-sign-in-to-microsoft-identity-platform-and-obtains-a-token"></a>Como um aplicativo Web delega a entrada à plataforma de identidade da Microsoft e obtém um token

A autenticação do usuário ocorre por meio do navegador. O protocolo OpenID usa mensagens do protocolo HTTP padrão.

* O aplicativo Web envia um HTTP 302 (redirecionamento) para o navegador para usar a plataforma de identidade da Microsoft.
* Quando o usuário é autenticado, a plataforma de identidade da Microsoft envia o token para o aplicativo Web, usando um redirecionamento por meio do navegador.
* O redirecionamento é fornecido pelo aplicativo Web na forma de um URI de redirecionamento. Esse URI de redirecionamento é registrado com o objeto de aplicativo do Azure AD. Pode haver vários URIs de redirecionamento, pois o aplicativo pode ser implantado em várias URLs. Portanto, o aplicativo Web também precisará especificar o URI de redirecionamento a ser usado.
* O Azure AD verifica se o URI de redirecionamento enviado pelo aplicativo Web é um dos URIs de redirecionamento registrados no aplicativo.

## <a name="desktop-and-mobile-app-sign-in-flow"></a>Fluxo de entrada de aplicativo móvel e para desktop

O fluxo descrito acima se aplica, com pequenas diferenças, a aplicativos móveis e para desktop.

Aplicativos móveis e para desktop podem usar um controle da Web incorporado ou um navegador do sistema para autenticação. O diagrama a seguir mostra como um aplicativo móvel ou para desktop usa a MSAL (biblioteca de autenticação da Microsoft) para adquirir tokens de acesso e chamar APIs da Web.

![Como é o aplicativo para desktop](media/authentication-scenarios/desktop-app-how-it-appears-to-be.png)

A MSAL usa um navegador para obter tokens. Assim como acontece com os aplicativos Web, a autenticação é delegada à plataforma de identidade da Microsoft.

Como o Azure AD salva o mesmo cookie de identidade no navegador, como ele faz para aplicativos Web, se o aplicativo nativo ou móvel usar o navegador do sistema, ele receberá imediatamente o SSO com o aplicativo Web correspondente.

Por padrão, a MSAL usa o navegador do sistema. A exceção são os aplicativos para desktop .NET Framework em que um controle incorporado é usado para fornecer ao usuário uma experiência mais integrada.

## <a name="next-steps"></a>Próximas etapas

Para outros tópicos de noções básicas sobre autenticação e autorização:

* Confira [Autenticação versus autorização](authentication-vs-authorization.md) para saber mais sobre os conceitos básicos de autenticação e autorização na plataforma de identidade da Microsoft.
* Confira [Tokens de segurança](security-tokens.md) para saber como os tokens de acesso, de atualização e de ID são usados na autorização e na autenticação.
* Confira [Modelo de aplicativo](application-model.md) para saber mais sobre o processo de registro de seu aplicativo para que ele possa se integrar à plataforma de identidade da Microsoft.

Para saber mais sobre o fluxo de entrada do aplicativo:

* Confira [Fluxos de autenticação e cenários de aplicativo](authentication-flows-app-scenarios.md) para saber mais sobre outros cenários de autenticação de usuários compatíveis com a plataforma de identidade da Microsoft.
* Confira [Bibliotecas MSAL](msal-overview.md) para saber mais sobre as bibliotecas da Microsoft que ajudam você a desenvolver aplicativos que funcionem com contas da Microsoft, contas do Azure AD e usuários do Azure AD B2C, tudo em um único modelo de programação simplificado.
