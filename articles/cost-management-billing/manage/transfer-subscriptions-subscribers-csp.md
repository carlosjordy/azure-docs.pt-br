---
title: Transferir assinaturas do Azure entre assinantes e CSPs
description: Saiba como você pode transferir assinaturas do Azure entre assinantes e CSPs.
author: bandersmsft
ms.reviewer: dhgandhi
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 06/19/2020
ms.author: banders
ms.openlocfilehash: 925a69f1f3a408e50cb9dc1d03cb5c1cf673ef41
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86102677"
---
# <a name="transfer-azure-subscriptions-between-subscribers-and-csps"></a>Transferir assinaturas do Azure entre assinantes e CSPs

Este artigo fornece as etapas de alto nível usadas para transferir assinaturas do Azure de/para parceiros CSP (provedor de soluções de nuvem) e seus clientes.

## <a name="transfer-ea-subscriptions-to-a-csp-partner"></a>Transferir assinaturas EA para um parceiro CSP

Os parceiros CSP de cobrança direta certificados como um [Provedor de Serviços Gerenciados (MSP) Especialista em Azure](https://partner.microsoft.com/membership/azure-expert-msp) podem solicitar a transferência de assinaturas do Azure para seus clientes que têm um EA (Contrato Enterprise) direto. Transferências de assinatura são permitidas somente para clientes que aceitaram um MCA (Contrato de Cliente da Microsoft) e compraram um plano do Azure.

Quando a solicitação é aprovada, o CSP pode fornecer uma fatura combinada para seus clientes. Para saber mais sobre a transferência de assinaturas por CSPs, confira [Obter a propriedade de cobrança das assinaturas do Azure para sua conta do MPA](mpa-request-ownership.md).

## <a name="other-subscription-transfers-to-a-csp-partner"></a>Outras transferências de assinatura para um parceiro CSP

Para transferir qualquer outra assinatura do Azure para um parceiro CSP, o assinante precisa mover recursos das assinaturas de origem para as assinaturas do CSP. Use as diretrizes a seguir para mover recursos entre assinaturas.

1. Trabalhe com seu parceiro CSP para criar assinaturas do CSP do Azure de destino.
1. Verifique se as assinaturas de origem e destino do CSP estão no mesmo locatário do Azure AD (Active Directory).  
    Não é possível alterar o locatário do Azure AD para uma assinatura do CSP do Azure. Em vez disso, você precisa adicionar ou associar a assinatura de origem ao locatário do CSP do Azure AD. Para obter mais informações, confira [Associar ou adicionar uma assinatura do Azure ao seu locatário do Azure Active Directory](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).
    > [!IMPORTANT]
    > - Quando você associa uma assinatura a um diretório do Azure AD diferente, os usuários que têm funções atribuídas usando o [RBAC (controle de acesso baseado em função)](../../role-based-access-control/role-assignments-portal.md) perdem o acesso. Os administradores de assinatura clássicos, incluindo o Administrador de Serviços e os Coadministradores, também perdem o acesso.
    > - As Atribuições de Política também são removidas de uma assinatura quando ela é associada a um diretório diferente.
1. A conta de usuário que você usa para fazer a transferência precisa ter acesso de proprietário do [RBAC](add-change-subscription-administrator.md) nas duas assinaturas.
1. Antes de começar, [valide](/rest/api/resources/resources/validatemoveresources) que todos os recursos do Azure podem ser movidos da assinatura de origem para a assinatura de destino.  
    Alguns recursos do Azure não podem ser movidos entre assinaturas. Para exibir a lista completa de recursos do Azure que podem ser movidos, confira [Suporte à operação de mover para recursos](../../azure-resource-manager/management/move-support-resources.md).
    > [!IMPORTANT]
    >  - O CSP do Azure oferece suporte apenas aos recursos do Azure Resource Manager. Se algum recurso do Azure Resource Manager na assinatura de origem tiver sido criado usando o modelo de implantação clássico do Azure, você precisará migrá-los para o [Azure Resource Manager](https://docs.microsoft.com/azure/cloud-solution-provider/migration/ea-payg-to-azure-csp/ea-open-direct-asm-to-arm) antes da migração. Você precisa ser um parceiro para exibir a página da Web.

1. Verifique se todos os serviços da assinatura de origem usam o modelo do Azure Resource Manager. Em seguida, transfira recursos da assinatura de origem para a assinatura de destino usando o [Azure Resource Move](../../azure-resource-manager/management/move-resource-group-and-subscription.md).
    > [!IMPORTANT]
    >  - Mover recursos do Azure entre assinaturas pode resultar em tempo de inatividade do serviço, dependendo dos recursos presentes nas assinaturas.

## <a name="transfer-csp-subscription-to-other-offer"></a>Transferir assinatura do CSP para outra oferta

Para transferir qualquer outra assinatura de um parceiro CSP para outra oferta do Azure, o assinante precisa mover recursos entre assinaturas de CSP de origem e as assinaturas de destino.

1. Criar assinaturas do Azure de destino.
1. Verifique se as assinaturas de origem e destino estão no mesmo locatário do Azure Active Directory. Para obter mais informações sobre como alterar um locatário do Azure AD, confira [Associar ou adicionar uma assinatura do Azure ao seu locatário do Azure Active Directory](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).
    Observe que o diretório da alteração não é a assinatura do CSP. Por exemplo, você está transferindo de um CSP para uma assinatura paga conforme o uso. Você precisa alterar o diretório da assinatura paga conforme o uso de maneira a corresponder ao diretório.

    > [!IMPORTANT]
    >  - Quando você associa uma assinatura a um diretório diferente, os usuários que têm funções atribuídas usando o [RBAC](../../role-based-access-control/role-assignments-portal.md) perdem o acesso. Os administradores de assinatura clássicos, incluindo o Administrador de Serviços e os Coadministradores, também perdem o acesso.
    >  - As Atribuições de Política também são removidas de uma assinatura quando ela é associada a um diretório diferente.

1. A conta de usuário que você usa para fazer a transferência precisa ter acesso de proprietário do [RBAC](add-change-subscription-administrator.md) nas duas assinaturas.
1. Antes de começar, [valide](/rest/api/resources/resources/validatemoveresources) que todos os recursos do Azure podem ser movidos da assinatura de origem para a assinatura de destino.
    > [!IMPORTANT]
    >  - Alguns recursos do Azure não podem ser movidos entre assinaturas. Para exibir a lista completa de recursos do Azure que podem ser movidos, confira [Suporte à operação de mover para recursos](../../azure-resource-manager/management/move-support-resources.md).

1. Transfira recursos da assinatura de origem para a assinatura de destino usando o [Azure Resource Move](../../azure-resource-manager/management/move-resource-group-and-subscription.md).
    > [!IMPORTANT]
    >  - Mover recursos do Azure entre assinaturas pode resultar em tempo de inatividade do serviço, dependendo dos recursos presentes nas assinaturas.

## <a name="next-steps"></a>Próximas etapas
- [Obter a propriedade de cobrança das assinaturas do Azure para sua conta do MPA](mpa-request-ownership.md).
- Leia sobre como [Gerenciar contas e assinaturas com a Cobrança do Azure](../index.yml).
