---
title: Considerações sobre o programa Provedor de Soluções na Nuvem
description: Para parceiros CSP, o gerenciamento de recursos delegados do Azure ajuda a melhorar a segurança e o controle oferecendo permissões granulares.
ms.date: 07/06/2020
ms.topic: conceptual
ms.openlocfilehash: 8dd40c5c4f7131554dcc3249777de9f0d92c35bb
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86131503"
---
# <a name="azure-lighthouse-and-the-cloud-solution-provider-program"></a>Azure Lighthouse e o programa Provedor de Soluções na Nuvem

Se você é [parceiro CSP (Provedor de Soluções na Nuvem)](/partner-center/csp-overview), já pode acessar as assinaturas do Azure criadas para seus clientes por meio do programa CSP usando a funcionalidade [AOBO (Administrar em nome de)](https://channel9.msdn.com/Series/cspdev/Module-11-Admin-On-Behalf-Of-AOBO). Esse acesso permite que você dê suporte, configure e gerencie diretamente as assinaturas dos seus clientes.

Com o [Azure Lighthouse](../overview.md), você pode usar o gerenciamento de recursos delegado do Azure junto com o AOBO. Isso ajuda a melhorar a segurança e reduz o acesso desnecessário habilitando permissões mais granulares para seus usuários. Isso também possibilita maior eficiência e escalabilidade, pois os usuários podem trabalhar em várias assinaturas de clientes usando um único logon em seu locatário.

> [!TIP]
> Para ajudar a proteger os recursos do cliente, lembre-se de examinar e seguir nossas [práticas de segurança recomendadas](recommended-security-practices.md), juntamente com os [requisitos de segurança do parceiro](/partner-center/partner-security-requirements).

## <a name="administer-on-behalf-of-aobo"></a>AOBO (Administrar em Nome de)

Com o AOBO, qualquer usuário com a função [Agente Administrador](/partner-center/permissions-overview#manage-commercial-transactions-in-partner-center-azure-ad-and-csp-roles) em seu locatário terá acesso AOBO às assinaturas do Azure que você criar por meio do programa CSP. Os usuários que precisarem de acesso às assinaturas de clientes deverão ser um membro deste grupo. O AOBO não permite a flexibilidade de criar grupos distintos que funcionam com clientes diferentes ou de habilitar funções diferentes para grupos ou usuários.

![Gerenciamento de locatário usando AOBO](../media/csp-1.jpg)

## <a name="azure-delegated-resource-management"></a>Gerenciamento de recursos delegados do Azure

Usando o Azure Lighthouse, você pode atribuir diferentes grupos a diferentes clientes ou funções, conforme mostrado no diagrama a seguir. Como os usuários terão o nível apropriado de acesso por meio do gerenciamento de recursos delegado do Azure, você poderá reduzir o número de usuários que têm a função de Agente Administrador (e, portanto, ter acesso total por meio do AOBO). Isso ajuda a melhorar a segurança limitando o acesso desnecessário aos recursos dos seus clientes. Isso também proporciona mais flexibilidade para gerenciar vários clientes em escala.

A integração de uma assinatura que você criou por meio do programa CSP segue as etapas descritas em integrar [uma assinatura ao Lighthouse do Azure](../how-to/onboard-customer.md). Qualquer usuário que tenha a função Agente Administrador em seu locatário poderá realizar essa integração.

![Gerenciamento de locatários usando AOBO e gerenciamento de recursos delegados do Azure](../media/csp-2.jpg)

> [!NOTE]
> A [página **meus clientes** no portal do Azure](../how-to/view-manage-customers.md) agora inclui uma seção **provedor de soluções na nuvem (versão prévia)** , que exibe informações de cobrança e recursos para clientes do CSP que [assinaram o MCA (contrato de cliente da Microsoft)](/partner-center/confirm-customer-agreement) e estão [no plano do Azure](/partner-center/azure-plan-get-started). Para saber mais, confira [Introdução à conta de cobrança do Contrato de Parceiro da Microsoft](../../cost-management-billing/understand/mpa-overview.md).
>
> Os clientes do CSP podem aparecer nesta seção se também tiverem sido integrados ao gerenciamento de recursos delegado do Azure. Se eles tiverem, eles também aparecerão na seção **clientes** , conforme descrito em [Exibir e gerenciar clientes e recursos delegados](../how-to/view-manage-customers.md). Da mesma forma, um cliente CSP não precisa aparecer na seção **provedor de soluções na nuvem (versão prévia)** de **meus clientes** para que você possa integrá-los para o gerenciamento de recursos delegado do Azure.

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre as [experiências de gerenciamento entre locatários](cross-tenant-management-experience.md).
- Saiba como [carregar uma assinatura no Azure Lighthouse](../how-to/onboard-customer.md).
- Saiba mais sobre o [programa Provedor de Soluções na Nuvem](/partner-center/csp-overview).
