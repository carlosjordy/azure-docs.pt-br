---
title: Diagnosticar erros com o serviço conectado do Azure AD (Visual Studio)
description: O serviço conectado do Active Directory detectou um tipo de autenticação incompatível
author: ghogen
manager: jillfra
ms.prod: visual-studio-windows
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: how-to
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.openlocfilehash: 5cefc59a6072a945be493487c09b1cc7f9827475
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85830563"
---
# <a name="diagnosing-errors-with-the-azure-active-directory-connected-service"></a>Diagnosticando erros com o Serviço Conectado do Azure Active Directory

Ao detectar o código de autenticação anterior, o servidor conectado do Azure Active Directory detectou um tipo de autenticação incompatível.

Para detectar corretamente o código de autenticação anterior em um projeto, o projeto precisa ser recompilado. Se este erro for exibido e você não tiver um código de autenticação anterior no projeto, recompile-o e repita a operação.

## <a name="project-types"></a>Tipos de projeto

O serviço conectado verifica o tipo de projeto que está sendo desenvolvido para que possa injetar a lógica de autenticação adequada ao projeto. Se houver um controlador derivado de `ApiController` no projeto, o projeto será considerado um projeto de API Web. Se houver apenas controladores que derivam de `MVC.Controller` no projeto, o projeto será considerado um projeto MVC. O serviço conectado não oferece suporte a qualquer outro tipo de projeto.

## <a name="compatible-authentication-code"></a>Código de autenticação compatível

O serviço conectado também verifica configurações de autenticação configuradas anteriormente ou que são compatíveis com o serviço. Se todas as configurações estiverem presentes, ele será considerado um caso reentrante, e o serviço conectado será aberto e exibirá as configurações.  Se apenas algumas das configurações estiverem presentes, ele será considerado um caso de erro.

Em um projeto do MVC, o serviço conectado verifica qualquer uma das configurações a seguir, que são resultado do uso anterior do serviço:

```xml
<add key="ida:ClientId" value="" />
<add key="ida:Tenant" value="" />
<add key="ida:AADInstance" value="" />
<add key="ida:PostLogoutRedirectUri" value="" />
```

Além disso, o serviço conectado verifica uma das seguintes configurações em um projeto de API Web, que são o resultado do uso anterior do serviço:

```xml
<add key="ida:ClientId" value="" />
<add key="ida:Tenant" value="" />
<add key="ida:Audience" value="" />
```

## <a name="incompatible-authentication-code"></a>Código de autenticação incompatível

Por fim, o serviço conectado tenta detectar versões do código de autenticação que foram configuradas com versões anteriores do Visual Studio. Se você recebeu esse erro, significa que seu projeto contém um tipo de autenticação incompatível. O serviço conectado detecta os seguintes tipos de autenticação de versões anteriores do Visual Studio:

* Autenticação do Windows
* Contas Individuais de Usuário
* Contas organizacionais

Para detectar a Autenticação do Windows em um projeto do MVC, o serviço conectado procura pelo `authentication` elemento em seu `web.config` arquivo.

```xml
<configuration>
    <system.web>
        <authentication mode="Windows" />
    </system.web>
</configuration>
```

Para detectar a Autenticação do Windows em um projeto de API da Web, o serviço conectado procura pelo `IISExpressWindowsAuthentication` elemento em seu `.csproj` arquivo do projeto:

```xml
<Project>
    <PropertyGroup>
        <IISExpressWindowsAuthentication>enabled</IISExpressWindowsAuthentication>
    </PropertyGroup>
</Project>
```

Para detectar a autenticação de Contas de Usuário Individuais, o serviço conectado procura o elemento de pacote no seu `packages.config` arquivo.

```xml
<packages>
    <package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" />
</packages>
```

Para detectar uma forma anterior de autenticação de Conta Organizacional, o serviço conectado procura o seguinte elemento em`web.config`:

```xml
<configuration>
    <appSettings>
        <add key="ida:Realm" value="***" />
    </appSettings>
</configuration>
```

Para alterar o tipo de autenticação, remova o tipo de autenticação incompatível e tente adicionar o serviço conectado novamente.

Para obter mais informações, consulte [Cenários de autenticação para o Azure AD](authentication-scenarios.md).
