---
title: Autorizar o acesso à Configuração de Aplicativos do Azure usando o Azure Active Directory
description: Habilitar o RBAC para autorizar o acesso à instância da Configuração de Aplicativos do Azure
author: lisaguthrie
ms.author: lcozzens
ms.date: 02/13/2020
ms.topic: conceptual
ms.service: azure-app-configuration
ms.openlocfilehash: c2812219e689cb42fd871f85300239a10ab0da0e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84116716"
---
# <a name="authorize-access-to-azure-app-configuration-using-azure-active-directory"></a>Autorizar o acesso à Configuração de Aplicativos do Azure usando o Azure Active Directory
Além de usar o HMAC (Message Authentication Code baseado em hash), a configuração do Azure App dá suporte ao uso do Azure Active Directory (AD do Azure) para autorizar solicitações às instâncias de configuração do aplicativo.  O Azure AD permite usar o RBAC (controle de acesso baseado em função) para conceder permissões a uma entidade de segurança.  Uma entidade de segurança pode ser um usuário, uma [identidade gerenciada](../active-directory/managed-identities-azure-resources/overview.md) ou uma [entidade de serviço de aplicativo](../active-directory/develop/app-objects-and-service-principals.md).  Para saber mais sobre as funções e atribuições de função, confira [Noções básicas sobre funções diferentes](../role-based-access-control/overview.md).

## <a name="overview"></a>Visão geral
As solicitações feitas por uma entidade de segurança para acessar um recurso de configuração de aplicativo devem ser autorizadas. Com o Azure AD, o acesso a um recurso é um processo de duas etapas:
1. A identidade da entidade de segurança é autenticada e um token OAuth 2.0 é retornado.  O nome do recurso para solicitar um token é `https://login.microsoftonline.com/{tenantID}`, onde `{tenantID}` corresponde à ID do locatário do Azure Active Directory ao qual a entidade de serviço pertence.
2. O token é passado como parte de uma solicitação para o serviço de Configuração de Aplicativos para autorizar o acesso ao recurso especificado.

A etapa de autenticação requer que uma solicitação do aplicativo contenha um token de acesso OAuth 2.0 no runtime.  Se um aplicativo estiver em execução em uma entidade do Azure, como um aplicativo Azure Functions, um aplicativo Web do Azure ou uma VM do Azure, ele pode usar uma identidade gerenciada para acessar os recursos.  Para saber como autenticar solicitações feitas por uma identidade gerenciada para a Configuração de Aplicativos do Azure, confira [Autenticar o acesso aos recursos da Configuração de Aplicativos do Azure com o Azure Active Directory e as identidades gerenciadas para Recursos do Azure](howto-integrate-azure-managed-service-identity.md).

A etapa de autorização requer que uma ou mais funções RBAC sejam atribuídas à entidade de segurança. A Configuração de Aplicativos do Azure fornece as funções RBAC que abrangem os conjuntos de permissões para os recursos da Configuração de Aplicativos. As funções atribuídas a uma entidade de segurança determinam as permissões fornecidas à entidade. Para obter mais informações sobre as funções RBAC, confira [Funções RBAC internas da Configuração de Aplicativos do Azure](#built-in-rbac-roles-for-azure-app-configuration). 

## <a name="assign-rbac-roles-for-access-rights"></a>Atribuir funções RBAC para direitos de acesso
Azure Active Directory (Azure AD) autoriza os direitos de acesso aos recursos protegidos por meio do [controle de acesso baseado em função (RBAC)](../role-based-access-control/overview.md).

Quando uma função RBAC é atribuída a uma entidade de segurança do Azure AD, o Azure concede acesso a esses recursos para essa entidade de segurança. O acesso é definido para o recurso de Configuração de Aplicativos. Uma entidade de segurança do Azure AD pode ser um usuário ou uma entidade de serviço de aplicativo ou uma [identidade gerenciada para recursos do Azure](../active-directory/managed-identities-azure-resources/overview.md).

## <a name="built-in-rbac-roles-for-azure-app-configuration"></a>Funções RBAC internas da Configuração de Aplicativos do Azure
O Azure fornece as seguintes funções RBAC internas para autorizar o acesso aos dados da Configuração de Aplicativos usando o Azure AD e o OAuth:

- **Proprietário de Dados da Configuração de Aplicativos**: Use essa função para conceder acesso de leitura/gravação/exclusão aos dados da Configuração de Aplicativos. Isso não concede acesso ao recurso de Configuração de Aplicativos.
- **Leitor de Dados da Configuração de Aplicativos**: Use essa função para conceder acesso de leitura aos dados da Configuração de Aplicativos. Isso não concede acesso ao recurso de Configuração de Aplicativos.
- **Colaborador**: Use essa função para gerenciar o recurso de Configuração de Aplicativos. Embora os dados de configuração do aplicativo possam ser acessados usando chaves de acesso, essa função não concede acesso direto aos dados usando o Azure AD.
- **Leitor**: Use essa função para conceder acesso de leitura ao recurso de Configuração de Aplicativos. Isso não concede acesso às chaves de acesso do recurso nem aos dados armazenados na Configuração de Aplicativos.

> [!NOTE]
> Atualmente, o portal do Azure e a CLI dão suporte apenas à autenticação HMAC para acessar os dados de configuração do aplicativo. Não há suporte para a autenticação do Azure AD. Portanto, os usuários do portal do Azure e da CLI exigem a função de *colaborador* para recuperar as chaves de acesso do recurso de configuração de aplicativo. A concessão de dados de *configuração de aplicativo* ou funções de proprietário de dados de *configuração de aplicativo* não tem impacto sobre o acesso por meio do portal e da CLI.

## <a name="next-steps"></a>Próximas etapas
Saiba mais sobre o uso de [identidades gerenciadas](howto-integrate-azure-managed-service-identity.md) para administrar o serviço de Configuração de Aplicativos.
