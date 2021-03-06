---
title: Transferir a propriedade de cobrança de uma assinatura do Azure
description: Descreve como transferir a propriedade de cobrança de uma assinatura do Azure para outra conta, bem como algumas perguntas frequentes sobre o processo
keywords: transferir a assinatura do Azure, assinatura de transferência do Azure, mover a assinatura do Azure para outra conta, alterar o proprietário da assinatura do Azure, transferir a assinatura do Azure para outra conta, cobrança de transferência do Azure
author: bandersmsft
ms.reviewer: amberb
tags: billing,top-support-issue
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 07/01/2020
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 722d1bca7f983c124c85e6d675f51d29c5357522
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85854949"
---
# <a name="transfer-billing-ownership-of-an-azure-subscription-to-another-account"></a>Transferir a propriedade de cobrança de uma assinatura do Azure para outra conta

Talvez você queria transferir a propriedade de cobrança da sua assinatura do Azure, caso esteja se desligando da empresa ou queira que sua assinatura seja cobrada em outra conta. A transferência da propriedade de cobrança para outra conta fornece aos administradores da noca conta a permissão para tarefas de cobrança. Eles podem alterar o método de pagamento, exibir encargos e cancelar a assinatura.

Se você quiser manter a propriedade de cobrança, mas alterar o tipo de assinatura, confira [Mudar sua assinatura do Azure para outra oferta](switch-azure-offer.md). Para controlar quem pode acessar recursos na assinatura, confira [Funções internas do Azure](../../role-based-access-control/built-in-roles.md).

Se você for um cliente do EA (Contrato Enterprise), seus administradores corporativos poderão transferir a propriedade de cobrança de suas assinaturas entre contas. Para obter mais informações, confira [Transferir a propriedade de cobrança de assinaturas do EA (Contrato Enterprise)](#EA).

## <a name="transfer-billing-ownership-of-an-azure-subscription"></a>Transferir a propriedade de cobrança de uma assinatura do Azure

1. Entre no [portal do Azure](https://portal.azure.com) como um administrador da conta de cobrança que tem a assinatura que você deseja transferir. Para descobrir se você é um administrador, confira as [Perguntas frequentes](#faq).

1. Pesquise **Gerenciamento de Custos do Azure + Cobrança**.

   ![Captura de tela que mostra a pesquisa do portal do Azure](./media/billing-subscription-transfer/billing-search-cost-management-billing.png)

1. Selecione **Assinaturas** no painel à esquerda. Dependendo do seu nível de acesso, você pode precisar selecionar um escopo do orçamento e, em seguida, **Assinaturas** ou **assinaturas do Azure**.

1. Selecione **Transferir a propriedade de cobrança** para a assinatura que deseja transferir.

   ![Selecione a assinatura a transferir](./media/billing-subscription-transfer/billing-select-subscription-to-transfer.png)

1. Insira o endereço de email do usuário administrador de cobrança da conta que será o novo proprietário da assinatura.

1. Se você estiver transferindo sua assinatura para uma conta em outro locatário do Azure AD, selecione se deseja mover a assinatura para o locatário da nova conta. Para obter mais informações, confira [Como transferir uma assinatura para uma conta em outro locatário do Azure AD](#transfer-a-subscription-to-another-azure-ad-tenant-account)

    > [!IMPORTANT]
    >
    > Se você optar por migrar a assinatura para o locatário do Azure AD da nova conta, todas as [atribuições de função do Azure](../../role-based-access-control/role-assignments-portal.md) para acessar recursos na assinatura serão permanentemente removidas. Somente o usuário da nova conta que aceitar sua solicitação de transferência terá acesso para gerenciar os recursos da assinatura. Para obter mais informações, confira a próxima seção [Transferir uma assinatura para outra conta de locatário do Azure AD](#transfer-a-subscription-to-another-azure-ad-tenant-account)). Como alternativa, você pode desmarcar a caixa para o **Locatário do Azure AD da assinatura** para transferir a propriedade da cobrança sem mover a assinatura para o locatário da nova conta. Se você fizer isso, as atribuições de função do Azure atuais para acessar os recursos do Azure serão mantidas.

    ![Enviar página de transferência](./media/billing-subscription-transfer/billing-send-transfer-request.PNG)

1. Selecione **Enviar solicitação de transferência**.

1. O usuário recebe um email com instruções para revisar sua solicitação de transferência.

   ![Email de transferência de assinatura enviado para o destinatário](./media/billing-subscription-transfer/billing-receiver-email.png)

1. Para aprovar a solicitação de transferência, o usuário seleciona o link no email e segue as instruções. Depois, o usuário seleciona uma forma de pagamento que será usada para pagar a assinatura. Se o usuário não tiver uma conta do Azure, precisará se inscrever em uma nova conta.

   ![Primeira página da Web de transferência de assinatura](./media/billing-subscription-transfer/billing-accept-ownership-step1.png)

   ![Segunda página da Web de transferência de assinatura](./media/billing-subscription-transfer/billing-accept-ownership-step2.png)

   ![Segunda página da Web de transferência de assinatura](./media/billing-subscription-transfer/billing-accept-ownership-step3.png)

1. Êxito! Agora a assinatura será transferida.

## <a name="transfer-a-subscription-to-another-azure-ad-tenant-account"></a>Transferir uma assinatura para outra conta de locatário do Azure AD

Um locatário do Azure AD (Active Directory) é criado quando você se inscreve no Azure. O locatário representa sua conta. Você usa o locatário para gerenciar o acesso às assinaturas e aos recursos.

Quando você cria uma nova assinatura, ela é hospedada no locatário do Azure AD da sua conta. Se você quiser fornecer acesso à assinatura ou aos recursos dela a outras pessoas, será necessário convidá-las para ingressar em seu locatário. Isso ajuda a controlar o acesso às assinaturas e aos recursos.

Quando você transfere a propriedade de cobrança da sua assinatura para uma conta em outro locatário do Azure AD, você pode mover a assinatura para o locatário da nova conta. Se fizer isso, todos os usuários, os grupos ou as entidades de serviço que tinham [atribuições de função do Azure](../../role-based-access-control/role-assignments-portal.md) para gerenciar assinaturas e recursos perderão o acesso. Somente o usuário da nova conta que aceitar sua solicitação de transferência terá acesso para gerenciar os recursos. O novo proprietário deve adicionar manualmente esses usuários à assinatura para fornecer acesso ao usuário que a perdeu. Para obter mais informações, confira [Transferir uma assinatura do Azure para um diretório diferente do Azure AD (versão prévia)](../../role-based-access-control/transfer-subscription.md).


## <a name="transfer-visual-studio-and-partner-network-subscriptions"></a>Transferir assinaturas do Visual Studio e do Partner Network

As assinaturas do Visual Studio e do Microsoft Partner Network têm crédito Azure mensal recorrente associado a elas. Quando você transfere essas assinaturas, o crédito não fica disponível na conta de cobrança de destino. A assinatura usará o crédito existente na conta de cobrança de destino. Por exemplo, suponha que Bob tenha transferido uma assinatura do Visual Studio Enterprise para a conta de Jane no dia nove de setembro e ela tenha aceitado a transferência. Após a conclusão da transferência, a assinatura passa a usar o crédito da conta de Jane. O crédito será redefinido todo nono dia do mês.


<a id="EA"></a>

## <a name="transfer-ea-subscription-billing-ownership"></a>Transferir a propriedade de cobrança da assinatura do EA

O administrador corporativo pode transferir a propriedade das assinaturas entre contas dentro de um registro. Para obter mais informações, confira [Alterar proprietário da conta](https://docs.microsoft.com/azure/cost-management-billing/manage/ea-portal-get-started#change-account-owner) no Portal do EA.

## <a name="next-steps-after-accepting-billing-ownership"></a>Próximas etapas após aceitar a propriedade de cobrança

Se você aceitou a propriedade de cobrança de uma assinatura do Azure, recomendamos que você examine as próximas etapas:

1. Examine e atualize o administrador do serviço, os coadministradores e as atribuições de função do Azure. Para saber mais, confira [Adicionar ou alterar os administradores de assinatura do Azure](add-change-subscription-administrator.md) e [Adicionar ou remover atribuições de função do Azure usando o portal do Azure](../../role-based-access-control/role-assignments-portal.md).
1. Atualize as credenciais associadas aos serviços dessa assinatura, incluindo:
   1. Certificados de gerenciamento que concedem ao usuário direitos de administrador aos recursos de assinatura. Para saber mais, confira [Criar e carregar um certificado de gerenciamento do Azure](../../cloud-services/cloud-services-certs-create.md)
   1. Chaves de acesso para serviços como Armazenamento. Para saber mais, consulte [Sobre as contas de Armazenamento do Azure](../../storage/common/storage-create-storage-account.md)
   1. Credenciais de Acesso Remoto para serviços como Máquinas Virtuais do Azure.
1. Se estiver trabalhando com um parceiro, considere a atualização da ID do parceiro nessa assinatura. Você pode atualizar a ID do parceiro no [Portal do Azure](https://portal.azure.com). Para obter mais informações, confira [Vincular uma ID de parceiro a suas contas do Azure](link-partner-id.md)

<a id="supported"></a>

## <a name="supported-subscription-types"></a>Tipos de assinatura com suporte

A transferência de assinatura no portal do Azure está disponível para os tipos de assinatura listados abaixo. Atualmente, não é possível transferir assinaturas de [Avaliação Gratuita](https://azure.microsoft.com/offers/ms-azr-0044p/) ou [AIO (Azure via Open)](https://azure.microsoft.com/offers/ms-azr-0111p/). Para uma solução alternativa, consulte [Mover recursos para um novo grupo de recursos ou uma nova assinatura](../../azure-resource-manager/management/move-resource-group-and-subscription.md). Para transferir outras assinaturas, como planos de suporte, [entre em contato com o Suporte do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

- [EA (Contrato Enterprise)](https://azure.microsoft.com/pricing/enterprise-agreement/)\*
- [Microsoft Partner Network](https://azure.microsoft.com/offers/ms-azr-0025p/)  
- [Assinantes do Visual Studio Enterprise (MPN)](https://azure.microsoft.com/offers/ms-azr-0029p/)
- [Plataformas MSDN](https://azure.microsoft.com/offers/ms-azr-0062p/)  
- [Pré-paga](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Desenvolvimento/Teste pré-pago](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/)
- [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p/)
- [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)
- [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)
- [Plano do Microsoft Azure](https://azure.microsoft.com/offers/ms-azr-0017g/)\*\*

\* [Por meio do Portal do EA](#EA).

\*\* Só há suporte para contas criadas durante a inscrição no site do Azure.

<a id="faq"></a>

## <a name="frequently-asked-questions-faq-for-senders"></a>Perguntas frequentes para remetentes

Essas perguntas frequentes se aplicam aos usuários que estão transferindo a propriedade de cobrança de uma assinatura do Azure para outra conta.

### <a name="who-is-a-billing-administrator-of-an-account"></a><a name="whoisaa"></a> Quem é o administrador de cobrança de uma conta?

O administrador de cobrança é uma pessoa que tem permissão para gerenciar a cobrança de uma conta. Ele tem autoridade para acessar a cobrança no [portal do Azure](https://portal.azure.com) e executar várias tarefas de cobrança, como criar assinaturas, exibir e pagar faturas ou atualizar formas de pagamento.

Para identificar em quais contas você é um administrador de cobrança, use as seguintes etapas:

1. Acesse a [página de Gerenciamento de custos + Cobrança no portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade/Overview).
1. Selecione **Todos os escopos de cobrança** no painel esquerdo.
1. A página de assinaturas lista todas as assinaturas das quais você é administrador de cobrança.

Se você não tiver certeza de quem é o administrador da conta de uma assinatura, use as etapas a seguir para descobrir.

1. Acesse a [página Assinaturas no Portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Selecione a assinatura que você deseja verificar e olhe as **Configurações**.
1. Selecione **Propriedades**. O administrador da conta da assinatura será exibido na caixa **Administrador da Conta** .

### <a name="does-everything-transfer-including-resource-groups-vms-disks-and-other-running-services"></a>Tudo é transferido? Incluindo grupos de recursos, VMs, discos e outros serviços em execução?

Todos os recursos – como VMs, discos e sites – são transferidos para a nova conta. No entanto, se você transferir uma assinatura para uma conta em outro locatário do Azure AD, as [funções de administrador](add-change-subscription-administrator.md) e as [atribuições de função do Azure](../../role-based-access-control/role-assignments-portal.md) na assinatura [não serão transferidas](#transfer-a-subscription-to-another-azure-ad-tenant-account). Além disso, os [Registros de aplicativo](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md) e outros serviços específicos do locatário também não serão transferidos juntamente com a assinatura.

### <a name="can-i-transfer-ownership-to-an-account-in-another-countryregion"></a>Posso transferir a propriedade para uma conta em outro país/região?
Transferências entre países/regiões não podem ser executadas no portal do Azure. Para transferir a assinatura para outro país/região, [entre em contato com o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="i-am-an-administrator-on-two-accounts-can-i-transfer-a-subscription-from-one-of-my-accounts-to-another"></a>Sou administrador em duas contas. Posso transferir uma assinatura de uma das minhas contas para a outra?
Sim, você pode transferir assinaturas entre suas contas. Conceitualmente, suas contas são consideradas como pertencentes a dois usuários distintos, logo, você pode usar as etapas acima para transferir assinaturas entre suas contas.

### <a name="does-a-subscription-transfer-result-in-any-service-downtime"></a>Uma transferência de assinatura resulta em qualquer tempo de inatividade do serviço?

Se você transferir uma assinatura para uma conta no mesmo locatário do Azure AD, não haverá nenhum impacto sobre os recursos em execução na assinatura. No entanto, as informações de contexto salvas no PowerShell não são atualizadas, portanto, talvez seja necessário limpá-las ou alterar as configurações. Se você transferir a assinatura para uma conta em outro locatário e decidir mover a assinatura para esse locatário, todos os usuários, grupos e entidades de serviço que tinham [atribuições de função do Azure](../../role-based-access-control/role-assignments-portal.md) para acessar recursos na assinatura perderão o acesso. Como resultado, pode ocorrer o tempo de inatividade do serviço.

### <a name="can-users-in-new-account-access-usage-and-billing-history"></a>Os usuários da nova conta podem acessar o histórico de cobrança e de uso?

As únicas informações disponíveis para os usuários em uma nova conta são os custos do mês passado para sua assinatura. O restante do histórico de cobrança e de uso não será transferido com a assinatura

### <a name="how-do-i-migrate-data-and-services-for-my-azure-subscription-to-new-subscription"></a>Como faço para migrar dados e serviços da minha assinatura do Azure para uma nova assinatura?

Se não for possível transferir a propriedade da assinatura, migre os recursos manualmente. Consulte [Mover recursos para um novo grupo de recursos ou uma nova assinatura](../../azure-resource-manager/management/move-resource-group-and-subscription.md).

### <a name="if-i-transfer-a-visual-studio-or-microsoft-partner-network-subscription-does-my-credit-carry-forward-with-the-subscription-in-the-new-account"></a>Se eu transferir uma assinatura do Visual Studio ou do Microsoft Partner Network, meu crédito será transferido para a nova conta juntamente com a assinatura?

Não, seu crédito não estará disponível na nova conta. O usuário de destino precisa ter uma licença do Visual Studio para aceitar a solicitação de transferência. A assinatura usa o crédito do Visual Studio que está disponível na conta do usuário. Para saber mais, confira [Como transferir assinaturas do Visual Studio e do Partner Network](#transfer-visual-studio-and-partner-network-subscriptions).


## <a name="frequently-asked-questions-faq-for-recipients"></a>Perguntas frequentes para destinatários

Essas perguntas frequentes se aplicam aos usuários que estão aceitando a propriedade de cobrança de uma assinatura do Azure proveniente de outra conta.

### <a name="if-i-take-over-billing-ownership-of-a-subscription-from-another-account-do-users-in-that-account-continue-to-have-access-to-my-resources"></a>Se eu assumir a propriedade de cobrança de uma assinatura de outra conta, os usuários dessa conta continuarão a ter acesso aos meus recursos?

Sim. No entanto, [funções de administrador](add-change-subscription-administrator.md) e [atribuições de função do Azure](../../role-based-access-control/role-assignments-portal.md) podem ser removidas. A perda de acesso ocorre quando sua conta está em um locatário do Azure AD diferente do locatário da assinatura, e o usuário que enviou a solicitação de transferência move a assinatura para o locatário da sua conta. Para ver os usuários que têm atribuições de função do Azure para acessar recursos na assinatura, siga estas etapas:

1. Acesse a [página de Assinaturas no portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Selecione a assinatura que deseja verificar e, em seguida, selecione **Controle de Acesso (IAM)** no painel à esquerda.
1. Na parte superior da página, selecione **Atribuições de função**. A página de atribuições de função lista todos os usuários que têm acesso à assinatura.

Mesmo que as [atribuições de função do Azure](../../role-based-access-control/role-assignments-portal.md) sejam removidas durante a transferência, os usuários da conta do proprietário original continuarão tendo acesso à assinatura por meio de outros mecanismos de segurança, incluindo:

* Certificados de gerenciamento que concedem ao usuário direitos de administrador aos recursos de assinatura. Para saber mais, confira [Criar e carregar um certificado de gerenciamento do Azure](../../cloud-services/cloud-services-certs-create.md).
* Chaves de acesso para serviços como Armazenamento. Para saber mais, confira [Sobre as contas de armazenamento do Azure](../../storage/common/storage-create-storage-account.md).
* Credenciais de Acesso Remoto para serviços como Máquinas Virtuais do Azure.

Se o destinatário precisar restringir o acesso a seus recursos, ele deverá considerar a atualização dos segredos associados ao serviço. A maioria dos recursos pode ser atualizada usando as seguintes etapas:

  1. Entre no [portal do Azure](https://portal.azure.com).
  2. No menu Hub, selecione **Todos os recursos**.
  3. Selecione o recurso.
  4. Na página de recursos, selecione **Configurações**. Aqui você pode exibir e atualizar os segredos existentes.

### <a name="if-i-take-over-the-billing-ownership-of-a-subscription-in-the-middle-of-the-billing-cycle-do-i-have-to-pay-for-the-entire-billing-cycle"></a>Se eu assumir o controle da propriedade de cobrança de uma assinatura no meio do período de cobrança, terei de pagar pelo período de cobrança inteiro?

Sua conta será responsável pelo pagamento de todo uso registrado a partir do momento da transferência. Pode haver algum uso que ocorreu antes da transferência, mas foi relatado posteriormente. O uso está incluído na fatura da sua conta.

### <a name="can-i-use-a-different-payment-method"></a>Eu posso usar uma forma de pagamento diferente?

Sim. Ao aceitar a solicitação de transferência, você pode selecionar uma forma de pagamento existente que esteja vinculada à sua conta ou adicionar uma nova forma de pagamento.

### <a name="how-can-i-transfer-ownership-of-my-enterprise-agreement-ea-subscription-account-ownership-if-the-original-account-owner-is-no-longer-with-the-organization"></a>Como posso transferir a propriedade da minha conta de assinatura do Enterprise Agreement (EA) quando o proprietário original da conta não está mais na organização?

O administrador corporativo pode atualizar a propriedade da conta de qualquer conta, mesmo se o proprietário original da conta não faz mais parte da organização. Isso é possível seguindo as instruções para [Transferir a propriedade da conta para todas as assinaturas](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription) no portal do EA.

## <a name="troubleshooting"></a>Solução de problemas

### <a name="why-dont-i-see-the-transfer-subscription-button"></a><a id="no-button"></a> Por que não vejo o botão “Transferir Assinatura”?

A transferência de assinatura via autoatendimento não está disponível para sua conta de cobrança. Atualmente, não damos suporte para a transferência da propriedade de cobrança de assinaturas em contas do EA (Contrato Enterprise) no portal do Azure. Além disso, as contas de Contrato de Cliente da Microsoft criadas enquanto trabalham com um representante da Microsoft não dão suporte à transferência de propriedade de cobrança.

### <a name="why-doesnt-my-subscription-type-support-transfer"></a><a id="no-button"></a> Por que o meu tipo de assinatura não é compatível com a transferência?

Nem todos os tipos de assinatura são compatíveis com a transferência de propriedade de cobrança. Para exibir uma lista de tipos de assinatura que são compatíveis com transferências, confira [Tipos de assinatura compatíveis](#supported-subscription-types)

### <a name="why-am-i-receiving-an-access-denied-error-when-i-try-to-transfer-billing-ownership-of-a-subscription"></a><a id="no-button"></a> Por que estou recebendo um erro de acesso negado ao tentar transferir a propriedade de cobrança de uma assinatura?

Você verá esse erro se estiver tentando transferir uma assinatura do Plano do Microsoft Azure e não tiver as permissões necessárias. Para transferir uma assinatura do plano do Microsoft Azure, você precisará ser um proprietário ou um colaborador na seção da fatura em que a assinatura é cobrada. Para obter mais informações, confira [Gerenciar assinaturas para a seção de fatura](understand-mca-roles.md#manage-subscriptions-for-invoice-section).


## <a name="need-help-contact-us"></a>Precisa de ajuda? Entre em contato conosco.

Caso tenha dúvidas ou precise de ajuda, [crie uma solicitação de suporte](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Próximas etapas

- Examine e atualize o administrador do serviço, os coadministradores e as atribuições de função do Azure. Para saber mais, confira [Adicionar ou alterar os administradores de assinatura do Azure](add-change-subscription-administrator.md) e [Adicionar ou remover atribuições de função do Azure usando o portal do Azure](../../role-based-access-control/role-assignments-portal.md).
