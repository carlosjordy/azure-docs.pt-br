---
title: Perguntas frequentes sobre Azure AD Domain Services | Microsoft Docs
description: Leia e entenda algumas das perguntas frequentes sobre configuração, administração e disponibilidade para Azure Active Directory Domain Services
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 48731820-9e8c-4ec2-95e8-83dba1e58775
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 06/05/2020
ms.author: iainfou
ms.openlocfilehash: cc78df7ea904bf85f5f2561319e6fc773244e971
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87005206"
---
# <a name="frequently-asked-questions-faqs-about-azure-active-directory-ad-domain-services"></a>Perguntas frequentes (FAQs) sobre os serviços de domínio do Azure Active Directory (AD)

Esta página responde a perguntas frequentes sobre Azure Active Directory Domain Services.

## <a name="configuration"></a>Configuração

* [Posso criar vários domínios gerenciados para um único diretório do Azure AD?](#can-i-create-multiple-managed-domains-for-a-single-azure-ad-directory)
* [Posso habilitar Azure AD Domain Services em uma rede virtual clássica?](#can-i-enable-azure-ad-domain-services-in-a-classic-virtual-network)
* [Posso habilitar os Serviços de Domínio do Azure AD em uma rede virtual do Azure Resource Manager?](#can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network)
* [Posso migrar meu domínio gerenciado existente de uma rede virtual clássica para uma rede virtual do Resource Manager?](#can-i-migrate-my-existing-managed-domain-from-a-classic-virtual-network-to-a-resource-manager-virtual-network)
* [Posso habilitar o Azure AD Domain Services em uma assinatura do Azure CSP (Provedor de Soluções de Nuvem)?](#can-i-enable-azure-ad-domain-services-in-an-azure-csp-cloud-solution-provider-subscription)
* [Posso habilitar Azure AD Domain Services em um diretório federado do AD do Azure? Não sincronizar hashes de senha para o Azure AD. Posso habilitar Azure AD Domain Services para este diretório?](#can-i-enable-azure-ad-domain-services-in-a-federated-azure-ad-directory-i-do-not-synchronize-password-hashes-to-azure-ad-can-i-enable-azure-ad-domain-services-for-this-directory)
* [Posso disponibilizar os Azure AD Domain Services em várias redes virtuais na minha assinatura?](#can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription)
* [Posso habilitar os Serviços de Domínio do AD do Azure usando o PowerShell?](#can-i-enable-azure-ad-domain-services-using-powershell)
* [Posso habilitar os Azure AD Domain Services usando um modelo do Resource Manager?](#can-i-enable-azure-ad-domain-services-using-a-resource-manager-template)
* [Posso adicionar controladores de domínio a um domínio gerenciado dos Serviços de Domínio do AD do Azure?](#can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain)
* [Os usuários convidados para o meu diretório podem usar os Azure AD Domain Services?](#can-guest-users-invited-to-my-directory-use-azure-ad-domain-services)
* [Posso mover um domínio gerenciado Azure AD Domain Services existente para uma assinatura, um grupo de recursos, uma região ou uma rede virtual diferente?](#can-i-move-an-existing-azure-ad-domain-services-managed-domain-to-a-different-subscription-resource-group-region-or-virtual-network)
* [Azure AD Domain Services inclui opções de alta disponibilidade?](#does-azure-ad-domain-services-include-high-availability-options)

### <a name="can-i-create-multiple-managed-domains-for-a-single-azure-ad-directory"></a>Posso criar vários domínios gerenciados para um único diretório do Azure AD?
Não. Você pode criar apenas um único domínio atendido pelo Azure AD Domain Services para um único diretório do Azure AD.

### <a name="can-i-enable-azure-ad-domain-services-in-a-classic-virtual-network"></a>Posso habilitar Azure AD Domain Services em uma rede virtual clássica?
As redes virtuais clássicas não têm suporte para novas implantações. Os domínios gerenciados existentes implantados em redes virtuais clássicas continuam com suporte até serem desativados em 1º de março de 2023. Para implantações existentes, você pode [migrar Azure AD Domain Services do modelo de rede virtual clássica para o Gerenciador de recursos](migrate-from-classic-vnet.md).

Para obter mais informações, consulte o [aviso oficial de substituição](https://azure.microsoft.com/updates/we-are-retiring-azure-ad-domain-services-classic-vnet-support-on-march-1-2023/).

### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>Posso habilitar os Serviços de Domínio do Azure AD em uma rede virtual do Azure Resource Manager?
Sim. O Azure AD Domain Services pode ser habilitado em uma rede virtual do Azure Resource Manager. As redes virtuais clássicas do Azure não estão mais disponíveis quando você cria um domínio gerenciado.

### <a name="can-i-migrate-my-existing-managed-domain-from-a-classic-virtual-network-to-a-resource-manager-virtual-network"></a>Posso migrar meu domínio gerenciado existente de uma rede virtual clássica para uma rede virtual do Resource Manager?
Sim. Para obter mais informações, consulte [migrar Azure AD Domain Services do modelo de rede virtual clássica para o Gerenciador de recursos](migrate-from-classic-vnet.md).

### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-csp-cloud-solution-provider-subscription"></a>Posso habilitar o Azure AD Domain Services em uma assinatura do Azure CSP (Provedor de Soluções de Nuvem)?
Sim. Para obter mais informações, consulte [como habilitar Azure AD Domain Services em assinaturas do Azure CSP](csp.md).

### <a name="can-i-enable-azure-ad-domain-services-in-a-federated-azure-ad-directory-i-do-not-synchronize-password-hashes-to-azure-ad-can-i-enable-azure-ad-domain-services-for-this-directory"></a>É possível habilitar o Azure AD Domain Services em um diretório federado do Azure AD? Eu não sincronizo hashes de senha para o Azure AD. É possível habilitar o Azure AD Domain Services nesse diretório?
Não. Para autenticar usuários via NTLM ou Kerberos, Azure AD Domain Services precisa acessar os hashes de senha de contas de usuário. Em um diretório federado, os hashes de senha não são armazenados no diretório do AD do Azure. Portanto, Azure AD Domain Services não funciona com esses diretórios do Azure AD.

No entanto, se você estiver usando Azure AD Connect para sincronização de hash de senha, poderá usar Azure AD Domain Services porque os valores de hash de senha são armazenados no Azure AD.

### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>Posso disponibilizar os Azure AD Domain Services em várias redes virtuais na minha assinatura?
O serviço em si não dá suporte direto a esse cenário. Seu domínio gerenciado está disponível somente em uma rede virtual por vez. No entanto, você pode configurar a conectividade entre várias redes virtuais para expor Azure AD Domain Services a outras redes virtuais. Para obter mais informações, consulte [como conectar redes virtuais no Azure usando gateways de VPN](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) ou [emparelhamento de rede virtual](../virtual-network/virtual-network-peering-overview.md).

### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>Posso habilitar os Serviços de Domínio do AD do Azure usando o PowerShell?
Sim. Para obter mais informações, consulte [como habilitar o Azure AD Domain Services usando o PowerShell](powershell-create-instance.md).

### <a name="can-i-enable-azure-ad-domain-services-using-a-resource-manager-template"></a>Posso habilitar os Azure AD Domain Services usando um modelo do Resource Manager?
Sim, você pode criar um Azure AD Domain Services domínio gerenciado usando um modelo do Resource Manager. Uma entidade de serviço e um grupo do Azure AD para administração devem ser criados usando o portal do Azure ou Azure PowerShell antes de o modelo ser implantado. Para obter mais informações, consulte [criar um domínio gerenciado do Azure AD DS usando um modelo de Azure Resource Manager](template-create-instance.md). Quando você cria um Azure AD Domain Services domínio gerenciado no portal do Azure, há uma opção também para exportar o modelo para uso com implantações adicionais.

### <a name="can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain"></a>Posso adicionar controladores de domínio a um domínio gerenciado dos Serviços de Domínio do AD do Azure?
Não. O domínio fornecido pelo Azure AD Domain Services é um domínio gerenciado. Você não precisa provisionar, configurar ou gerenciar controladores de domínio para esse domínio. Essas atividades de gerenciamento são fornecidas como um serviço pela Microsoft. Portanto, você não pode adicionar mais controladores de domínio (leitura/gravação ou somente leitura) para o domínio gerenciado.

### <a name="can-guest-users-invited-to-my-directory-use-azure-ad-domain-services"></a>Os usuários convidados para o meu diretório podem usar os Azure AD Domain Services?
Não. Os usuários convidados para o seu diretório AD Azure usando o processo de convite [Azure AD B2B](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md) são sincronizados no seu domínio gerenciado dos Azure AD Domain Services. No entanto, as senhas para esses usuários não são armazenadas em seu diretório do Azure AD. Portanto, Azure AD Domain Services não tem como sincronizar os hashes NTLM e Kerberos para esses usuários em seu domínio gerenciado. Tais usuários não podem entrar ou ingressar computadores no domínio gerenciado.

### <a name="can-i-move-an-existing-azure-ad-domain-services-managed-domain-to-a-different-subscription-resource-group-region-or-virtual-network"></a>Posso mover um domínio gerenciado Azure AD Domain Services existente para uma assinatura, um grupo de recursos, uma região ou uma rede virtual diferente?
Não. Depois de criar um Azure AD Domain Services domínio gerenciado, você não poderá mover o domínio gerenciado para um grupo de recursos diferente, rede virtual, assinatura, etc. Tome cuidado para selecionar a assinatura, o grupo de recursos, a região e a rede virtual mais apropriados ao implantar o domínio gerenciado.

### <a name="does-azure-ad-domain-services-include-high-availability-options"></a>Azure AD Domain Services inclui opções de alta disponibilidade?

Sim. Cada domínio gerenciado Azure AD Domain Services inclui dois controladores de domínio. Você não gerencia ou se conecta a esses controladores de domínio, eles fazem parte do serviço gerenciado. Se você implantar Azure AD Domain Services em uma região que dá suporte a Zonas de Disponibilidade, os controladores de domínio serão distribuídos entre zonas. Em regiões que não dão suporte a Zonas de Disponibilidade, os controladores de domínio são distribuídos entre conjuntos de disponibilidade. Você não tem opções de configuração ou controle de gerenciamento sobre esta distribuição. Para obter mais informações, consulte [Opções de disponibilidade para máquinas virtuais no Azure](../virtual-machines/windows/availability.md).

## <a name="administration-and-operations"></a>Administração e operações

* [Posso conectar ao controlador de domínio para meu domínio gerenciado usando a Área de Trabalho Remota?](#can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop)
* [Habilitei Azure AD Domain Services. Qual conta de usuário eu uso para ingressar em computadores a este domínio?](#ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain)
* [É possível ter privilégios de administrador de domínio no domínio gerenciado fornecido pelo Azure AD Domain Services?](#do-i-have-domain-administrator-privileges-for-the-managed-domain-provided-by-azure-ad-domain-services)
* [Posso modificar associações de grupo usando o LDAP ou outras ferramentas administrativas do AD em domínios gerenciados?](#can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-managed-domains)
* [Quanto tempo demora para que as alterações que eu fiz ao meu diretório do Azure AD fiquem visíveis em meu domínio gerenciado?](#how-long-does-it-take-for-changes-i-make-to-my-azure-ad-directory-to-be-visible-in-my-managed-domain)
* [É possível estender o esquema do domínio gerenciado fornecido pelo Azure AD Domain Services?](#can-i-extend-the-schema-of-the-managed-domain-provided-by-azure-ad-domain-services)
* [Posso modificar ou adicionar registros DNS em meu domínio gerenciado?](#can-i-modify-or-add-dns-records-in-my-managed-domain)
* [Qual é a política de tempo de vida da senha em um domínio gerenciado?](#what-is-the-password-lifetime-policy-on-a-managed-domain)
* [O Azure AD Domain Services fornece proteção de bloqueio de conta para o AD?](#does-azure-ad-domain-services-provide-ad-account-lockout-protection)
* [Posso configurar o Sistema de Arquivos Distribuído (DFS) e a replicação no Azure AD Domain Services?](#can-i-configure-distributed-file-system-and-replication-within-azure-ad-domain-services)

### <a name="can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop"></a>Posso conectar ao controlador de domínio para meu domínio gerenciado usando a Área de Trabalho Remota?
Não. Você não tem permissões para se conectar a controladores de domínio para o domínio gerenciado usando o Área de Trabalho Remota. Os membros do grupo de *Administradores do AAD DC* podem administrar o domínio gerenciado usando ferramentas de administração do AD, como o ADAC (centro de administração do Active Directory) ou o AD PowerShell. Essas ferramentas são instaladas usando o recurso *ferramentas de administração de servidor remoto* em um Windows Server ingressado no domínio gerenciado. Para obter mais informações, consulte [criar uma VM de gerenciamento para configurar e administrar um Azure AD Domain Services domínio gerenciado](tutorial-create-management-vm.md).

### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain"></a>Habilitei Azure AD Domain Services. Qual conta de usuário eu posso usar para ingressar computadores no domínio para este domínio?
Qualquer conta de usuário que faça parte do domínio gerenciado pode ingressar em uma VM. Os membros do grupo de *Administradores do AAD DC* recebem acesso à área de trabalho remota para computadores que ingressaram no domínio gerenciado.

### <a name="do-i-have-domain-administrator-privileges-for-the-managed-domain-provided-by-azure-ad-domain-services"></a>É possível ter privilégios de administrador de domínio no domínio gerenciado fornecido pelo Azure AD Domain Services?
Não. Você não recebe privilégios administrativos no domínio gerenciado. Os privilégios de administrador de *domínio* e de *administrador corporativo* não estão disponíveis para uso no domínio. Os membros dos grupos de administrador de domínio ou de administrador corporativo na sua Active Directory local também não recebem privilégios de administrador de domínio/empresa no domínio gerenciado.

### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-managed-domains"></a>Posso modificar associações de grupo usando o LDAP ou outras ferramentas administrativas do AD em domínios gerenciados?
Os usuários e grupos que são sincronizados de Azure Active Directory para Azure AD Domain Services não podem ser modificados porque sua origem de origem é Azure Active Directory. Qualquer usuário ou grupo originado no domínio gerenciado pode ser modificado.

### <a name="how-long-does-it-take-for-changes-i-make-to-my-azure-ad-directory-to-be-visible-in-my-managed-domain"></a>Quanto tempo demora para que as alterações que eu fiz ao meu diretório do Azure AD fiquem visíveis em meu domínio gerenciado?
As alterações feitas no diretório do Azure AD usando a interface do usuário do Azure AD ou o PowerShell são sincronizadas automaticamente com o domínio gerenciado. Esse processo de sincronização ocorre em segundo plano. Não há nenhum período de tempo definido para que essa sincronização conclua todas as alterações de objeto.

### <a name="can-i-extend-the-schema-of-the-managed-domain-provided-by-azure-ad-domain-services"></a>É possível estender o esquema do domínio gerenciado fornecido pelo Azure AD Domain Services?
Não. O esquema é administrado pela Microsoft para o domínio gerenciado. As extensões de esquema não têm suporte pelo Azure AD Domain Services.

### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>Posso modificar ou adicionar registros DNS em meu domínio gerenciado?
Sim. Os membros do grupo de *Administradores do AAD DC* recebem privilégios de *administrador de DNS* para modificar os registros DNS no domínio gerenciado. Esses usuários podem usar o console do Gerenciador DNS em um computador que executa o Windows Server ingressado no domínio gerenciado para gerenciar o DNS. Para usar o console do Gerenciador DNS, instale as *Ferramentas do servidor DNS*, que faz parte do *ferramentas de administração de servidor remoto* recurso opcional no servidor. Para obter mais informações, consulte [administrar o DNS em um domínio gerenciado Azure AD Domain Services](manage-dns.md).

### <a name="what-is-the-password-lifetime-policy-on-a-managed-domain"></a>Qual é a política de tempo de vida da senha em um domínio gerenciado?
O tempo de vida da senha padrão em um domínio gerenciado dos Azure AD Domain Services é de 90 dias. Esse tempo de vida de senha não está sincronizado com o tempo de vida de senha configurado no Azure AD. Portanto, você pode ter uma situação em que as senhas dos usuários expiram no seu domínio gerenciado, mas ainda são válidas no Azure AD. Em tais cenários, os usuários precisam alterar sua senha no Azure AD e a nova senha será sincronizada com seu domínio gerenciado. Além disso, os atributos de *senha-não expirar* e *usuário-deve-alterar-senha-at-login* para contas de usuário não são sincronizados com o domínio gerenciado.

### <a name="does-azure-ad-domain-services-provide-ad-account-lockout-protection"></a>O Azure AD Domain Services fornece proteção de bloqueio de conta para o AD?
Sim. Cinco tentativas de senha inválida em dois minutos no domínio gerenciado fazem com que uma conta de usuário seja bloqueada por 30 minutos. Depois de 30 minutos, a conta do usuário será desbloqueada automaticamente. Tentativas de senha inválidas no domínio gerenciado não bloqueiam a conta de usuário no Azure AD. A conta de usuário é bloqueada somente no domínio gerenciado do Azure AD Domain Services. Para obter mais informações, consulte [políticas de bloqueio de senha e conta em domínios gerenciados](password-policy.md).

### <a name="can-i-configure-distributed-file-system-and-replication-within-azure-ad-domain-services"></a>Posso configurar Sistema de Arquivos Distribuído e replicação no Azure AD Domain Services?
Não. O Sistema de Arquivos Distribuído (DFS) e a replicação não estão disponíveis ao usar Azure AD Domain Services.

## <a name="billing-and-availability"></a>Disponibilidade e cobrança

* [Os Azure AD Domain Services são um serviço pago?](#is-azure-ad-domain-services-a-paid-service)
* [Há uma avaliação gratuita para o serviço?](#is-there-a-free-trial-for-the-service)
* [Posso pausar um domínio gerenciado do Azure AD Domain Services?](#can-i-pause-an-azure-ad-domain-services-managed-domain)
* [É possível fazer failover do Azure AD Domain Services para outra região para um evento de recuperação de desastre?](#can-i-pause-an-azure-ad-domain-services-managed-domain)
* [Posso obter Azure AD Domain Services como parte do EMS (Enterprise Mobility Suite)? Preciso de Azure AD Premium para usar Azure AD Domain Services?](#can-i-failover-azure-ad-domain-services-to-another-region-for-a-dr-event)
* [Em quais regiões do Azure o serviço está disponível?](#can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems-do-i-need-azure-ad-premium-to-use-azure-ad-domain-services)

### <a name="is-azure-ad-domain-services-a-paid-service"></a>Os Azure AD Domain Services são um serviço pago?
Sim. Para saber mais, confira a [página de preço](https://azure.microsoft.com/pricing/details/active-directory-ds/).

### <a name="is-there-a-free-trial-for-the-service"></a>Há uma avaliação gratuita para o serviço?
Azure AD Domain Services está incluído na avaliação gratuita do Azure. Você pode se inscrever para uma [avaliação gratuita de um mês do Azure](https://azure.microsoft.com/pricing/free-trial/).

### <a name="can-i-pause-an-azure-ad-domain-services-managed-domain"></a>Posso pausar um domínio gerenciado do Azure AD Domain Services?
Não. Depois de habilitar um Azure AD Domain Services domínio gerenciado, o serviço estará disponível em sua rede virtual selecionada até que você exclua o domínio gerenciado. Não há como pausar o serviço. A cobrança continuará por hora até que você exclua o domínio gerenciado.

### <a name="can-i-failover-azure-ad-domain-services-to-another-region-for-a-dr-event"></a>É possível fazer failover do Azure AD Domain Services para outra região para um evento de recuperação de desastre?
Não. No momento, o Azure AD Domain Services não fornece um modelo de implantação com redundância geográfica. Ele é limitado a uma única rede virtual em uma região do Azure. Se quiser utilizar várias regiões do Azure, você precisará executar seus Controladores de Domínio do Active Directory em VMs IaaS do Azure. Para obter diretrizes de arquitetura, consulte [estender seu domínio de Active Directory local para o Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/identity/adds-extend-domain).

### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems-do-i-need-azure-ad-premium-to-use-azure-ad-domain-services"></a>Posso obter os Serviços de Domínio do AD do Azure como parte do EMS (Enterprise Mobility Suite)? Preciso do Azure AD Premium para usar os Serviços de Domínio do Azure AD?
Não. Azure AD Domain Services é um serviço pago conforme o uso do Azure e não faz parte do EMS. Azure AD Domain Services pode ser usado com todas as edições do Azure AD (gratuito e Premium). Você é cobrado por hora, dependendo do uso.

### <a name="what-azure-regions-is-the-service-available-in"></a>Em quais regiões do Azure o serviço está disponível?
Consulte a página [Serviços do Azure por região](https://azure.microsoft.com/regions/#services/) para conhecer as regiões do Azure nas quais os Serviços de Domínio do Azure AD estão disponíveis.

## <a name="troubleshooting"></a>Solução de problemas

Confira o [Guia de solução de problemas](troubleshoot.md) a fim de obter soluções para os problemas comuns encontrados ao configurar ou administrar os Azure AD Domain Services.

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre Azure AD Domain Services, confira [o que é Azure Active Directory Domain Services?](overview.md).

Para começar, consulte [criar e configurar um domínio gerenciado Azure Active Directory Domain Services](tutorial-create-instance.md).
