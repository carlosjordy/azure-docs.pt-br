---
title: Armazenamento de dados de identidade para clientes australiano e Nova Zelândia-Azure AD
description: Saiba mais sobre onde o Azure Active Directory armazena dados relacionados à identidade para seus clientes australianos.
services: active-directory
author: msaburnley
manager: daveba
ms.author: ajburnle
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 12/13/2019
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 850298719d5636e964b0c338d7a2a4cc9bb8aece
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77370300"
---
# <a name="identity-data-storage-for-australian-and-new-zealand-customers-in-azure-active-directory"></a>Armazenamento de dados de identidade para clientes australianos e da Nova Zelândia no Azure Active Directory

Os dados de identidade são armazenados pelo Azure AD em uma localização geográfica com base no endereço fornecido pela sua organização ao assinar um serviço online da Microsoft, como o Office 365 e o Azure. Para obter informações sobre onde os dados do cliente de identidade são armazenados, você pode usar a seção [onde os dados estão localizados?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) da central de confiabilidade da Microsoft.

> [!NOTE]
> Os serviços e aplicativos que se integram ao Azure AD têm acesso aos dados do cliente de identidade. Avalie cada serviço e aplicativo que você usa para determinar como os dados de identidade do cliente são processados por esse serviço e aplicativo específicos, e se eles atendem aos requisitos de armazenamento de dados da sua empresa. Para obter mais informações sobre a residência de dados de serviços da Microsoft, consulte o Onde seus dados estão localizados? seção do Microsoft Trust Center.

Para os clientes que forneceram um endereço na Austrália ou Nova Zelândia, o Azure AD mantém os dados de identidade para esses serviços em data centers australianos: 
- Gerenciamento de diretório do Azure AD 
- Autenticação

Todos os outros serviços do Azure AD armazenam dados do cliente em data centers globais. Para localizar o datacenter de um serviço, consulte [Azure Active Directory – onde os dados estão localizados?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located)

## <a name="microsoft-azure-multi-factor-authentication-mfa"></a>Microsoft Azure a autenticação multifator (MFA)

A MFA armazena dados de clientes de identidade em datacenters globais. Para saber mais sobre as informações do usuário coletadas e armazenadas pela MFA do Azure baseada em nuvem e pelo servidor do Azure MFA, confira [coleta de dados do usuário da autenticação multifator do Azure](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-data-residency).

## <a name="next-steps"></a>Próximas etapas
Para saber mais sobre qualquer um dos recursos e funcionalidades descritos acima, consulte esses artigos:
- [O que é a Autenticação Multifator?](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication)
