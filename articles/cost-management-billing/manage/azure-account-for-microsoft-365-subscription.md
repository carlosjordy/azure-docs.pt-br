---
title: Inscrever-se no Microsoft 365 com a conta do Azure
description: Saiba como criar uma assinatura do Microsoft 365 usando uma conta do Azure
author: JiangChen79
ms.reviewer: adwise
tags: billing,top-support-issue
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 06/15/2020
ms.author: banders
ms.openlocfilehash: 006f8edc81ec19647ff6173a4c786c6d7e93fadc
ms.sourcegitcommit: ad66392df535c370ba22d36a71e1bbc8b0eedbe3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84810373"
---
# <a name="sign-up-for-a-microsoft--365-subscription-with-your-azure-account"></a>Inscreva-se para obter uma assinatura do Microsoft 365 com sua conta do Azure

Se for assinante do Azure, você poderá usar sua conta do Azure para se inscrever para obter uma assinatura do Microsoft 365. Se fizer parte de uma organização que tem uma assinatura do Azure, você poderá criar assinaturas do Microsoft 365 os para usuários em seu Azure AD (Azure Active Directory) existente. Inscreva-se no Microsoft 365 usando uma conta com permissões de Administrador Global ou de Administrador de Cobrança em seu locatário do Azure Active Directory. Para obter mais informações, consulte [Check my account permissions in Azure AD (Verificar minhas permissões de conta no Azure AD)](#RoleInAzureAD) e [Atribuindo funções de administrador no Azure Active Directory](../../active-directory/users-groups-roles/directory-assign-admin-roles.md).

Se já tiver uma conta do Microsoft 365 e uma assinatura do Azure, você poderá [Associar um locatário do Microsoft 365 a uma assinatura do Azure](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a name="get-a-microsoft-365-subscription-by-using-your-azure-account"></a>Obter uma assinatura do Microsoft 365 usando sua conta do Azure

1. Acesse a [página de produto do Microsoft 365](https://www.microsoft.com/microsoft-365/business/all-business) e selecione um plano.
2. Clique em **Entrar** no canto superior direito da página.

    ![captura de tela da página de avaliação do Microsoft 365](./media/azure-account-for-microsoft-365-subscription/12-office-365-trial-page.png)
3. Conecte-se com as credenciais de conta do Azure. Se você estiver criando uma assinatura para sua organização, use uma conta do Azure que seja membro da função de diretório de Administrador Global ou Administrador de Cobrança no seu locatário do Azure Active Directory.

    ![Captura de tela da entrada da Microsoft](./media/azure-account-for-microsoft-365-subscription/13-office-365-sign-in.png)
4. Clique em **Experimentar agora**.

    ![Captura de tela que confirma o pedido do Microsoft 365.](./media/azure-account-for-microsoft-365-subscription/14-office-365-confirm-your-order.png)
5. Na página de confirmação do pedido, clique em **Continuar**.

    ![Captura de tela do recibo do pedido do Microsoft 365](./media/azure-account-for-microsoft-365-subscription/15-office-365-order-receipt.png)

Está tudo pronto para você. Se você criou a assinatura do Microsoft 365 para sua organização, siga as etapas abaixo para verificar se os usuários do Azure AD agora estão no Microsoft 365.

1. Abra o Centro de administração do Microsoft 365.
2. Expanda **USUÁRIOS** e clique em **Usuários Ativos**.

    ![Captura de tela dos usuários do Centro de administração do Microsoft 365](./media/azure-account-for-microsoft-365-subscription/16-microsoft-365-admin-center-users.png)

Após você se inscrever, a assinatura do Microsoft 365 é adicionada à mesma instância do Azure Active Directory à qual sua assinatura do Azure pertence. Para obter mais informações, confira [Mais informações sobre as assinaturas do Azure e do Microsoft 365](microsoft-365-account-for-azure-subscription.md#more-about-subs) e [Como as assinaturas do Azure são associadas ao Azure Active Directory](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a name="check-my-account-permissions-in-azure-ad"></a><a id="RoleInAzureAD"></a>Verifique as permissões da minha conta no Azure AD
1. Entre no [portal do Azure](https://portal.azure.com/).
2. Clique em **Todos os serviços** e, em seguida, pesquise **Active Directory**.

    ![Captura de tela do Active Directory no portal do Azure](./media/azure-account-for-microsoft-365-subscription/billing-more-services-active-directory.png)
3. Clique em **Usuários e grupos** > **Todos os usuários**.
4. Selecione o nome de usuário.

    ![Captura de tela que mostra os usuários do Azure Active Directory](./media/azure-account-for-microsoft-365-subscription/billing-users-groups.png)

5. Clique em **Função do diretório**.

    ![Captura de tela que mostra a função do diretório do portal do Azure](./media/azure-account-for-microsoft-365-subscription/billing-user-directory-role.png)
6.  A função **Administrador Global** ou **Administrador limitado** > **Administrador de cobrança** é necessária para criar uma assinatura do Microsoft 365 para usuários no Azure Active Directory existente.

    ![Captura de tela que mostra a função do diretório Administrador de cobrança do portal do Azure](./media/azure-account-for-microsoft-365-subscription/billing-directoryrole-limited.png)

## <a name="need-help-contact-us"></a>Precisa de ajuda? Entre em contato conosco.

Caso tenha dúvidas ou precise de ajuda, [crie uma solicitação de suporte](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Próximas etapas

- [Associar um locatário do Microsoft 365 a uma assinatura do Azure](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)
