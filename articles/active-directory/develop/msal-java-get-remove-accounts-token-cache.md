---
title: Obter & remover contas do cache de token (MSAL4j) | Azure
titleSuffix: Microsoft identity platform
description: Saiba como exibir e remover contas do cache de token usando a biblioteca de autenticação da Microsoft para Java.
services: active-directory
author: sangonzal
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 11/07/2019
ms.author: sagonzal
ms.reviewer: navyasri.canumalla
ms.custom: aaddev
ms.openlocfilehash: 150708e7d54e456e1a4e35cfe381e5de5d0f1e8c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85479344"
---
# <a name="get-and-remove-accounts-from-the-token-cache-using-msal-for-java"></a>Obter e remover contas do cache de token usando MSAL para Java

O MSAL para Java fornece um cache de token na memória por padrão. O cache de token na memória dura a duração da instância do aplicativo.

## <a name="see-which-accounts-are-in-the-cache"></a>Ver quais contas estão no cache

Você pode verificar quais contas estão no cache chamando `PublicClientApplication.getAccounts()` conforme mostrado no exemplo a seguir:

```java
PublicClientApplication pca = new PublicClientApplication.Builder(
                labResponse.getAppId()).
                authority(TestConstants.ORGANIZATIONS_AUTHORITY).
                build();

Set<IAccount> accounts = pca.getAccounts().join();
```

## <a name="remove-accounts-from-the-cache"></a>Remover contas do cache

Para remover uma conta do cache, localize a conta que precisa ser removida e, em seguida, chame `PublicClientApplicatoin.removeAccount()` conforme mostrado no exemplo a seguir:

```java
Set<IAccount> accounts = pca.getAccounts().join();

IAccount accountToBeRemoved = accounts.stream().filter(
                x -> x.username().equalsIgnoreCase(
                        UPN_OF_USER_TO_BE_REMOVED)).findFirst().orElse(null);

pca.removeAccount(accountToBeRemoved).join();
```

## <a name="learn-more"></a>Saiba mais

Se você estiver usando o MSAL para Java, saiba mais sobre a [serialização de cache de token personalizada em MSAL para Java](msal-java-token-cache-serialization.md).
