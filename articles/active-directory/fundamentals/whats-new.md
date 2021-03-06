---
title: Quais são as novidades? Notas sobre a versão – Azure Active Directory | Microsoft Docs
description: Conheça as novidades do Azure Active Directory, como as últimas notas sobre a versão, problemas conhecidos, correções de bug, funcionalidades preteridas e alterações futuras.
services: active-directory
author: msaburnley
manager: daveba
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 06/30/2020
ms.author: ajburnle
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: ac87bda4251a8414ef659ad486c989c0c1cd42e1
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87034713"
---
# <a name="whats-new-in-azure-active-directory"></a>Novidades no Azure Active Directory

>Seja notificado sobre quando revisitar esta página para obter atualizações copiando e colando esta URL: `https://docs.microsoft.com/api/search/rss?search=%22Release+notes+-+Azure+Active+Directory%22&locale=en-us` em seu ![ ícone de leitor de feed RSS ](./media/whats-new/feed-icon-16x16.png) .

O Azure AD recebe melhorias de forma contínua. Para se manter atualizado com os desenvolvimentos mais recentes, este artigo fornece informações sobre:

- As versões mais recentes
- Problemas conhecidos
- Correções de bug
- Funcionalidades preteridas
- Planos de alterações

Esta página é atualizada mensalmente; portanto, visite-a regularmente. Se você estiver procurando itens que têm mais de seis meses, poderá encontrá-los nos [Arquivos de O que há de novo no Azure Active Directory](whats-new-archive.md).

---

## <a name="june-2020"></a>Junho de 2020 

### <a name="user-risk-condition-in-conditional-access-policy"></a>Condição de risco do usuário na política de acesso condicional

**Tipo:** plano de alteração  
**Categoria de serviço:** Acesso condicional  
**Funcionalidade do produto:** segurança e proteção da identidade
 

O suporte a riscos do usuário na política de acesso condicional do Azure AD permite que você crie várias políticas baseadas em risco do usuário. Diferentes níveis de risco do usuário mínimo podem ser necessários para diferentes usuários e aplicativos. Com base no risco do usuário, você pode criar políticas para bloquear o acesso, exigir autenticação multifator, alteração de senha segura ou redirecionar para Microsoft Cloud App Security para impor a política de sessão, como auditoria adicional.

A condição de risco do usuário requer o Azure AD Premium P2 porque ele usa a proteção de identidade do Azure, que é uma oferta P2. para obter mais informações sobre o acesso condicional, consulte a [documentação de acesso condicional do Azure ad](https://docs.microsoft.com/azure/active-directory/conditional-access/).

---

### <a name="saml-sso-now-supports-apps-that-require-spnamequalifier-to-be-set-when-requested"></a>O SSO do SAML agora dá suporte a aplicativos que exigem que o SPNameQualifier seja definido quando solicitado

**Tipo:** corrigido  
**Categoria de serviço:** Aplicativos empresariais  
**Funcionalidade do produto:** SSO
 
Alguns aplicativos SAML exigem que SPNameQualifier sejam retornados no assunto da asserção quando solicitado. Agora, o Azure AD responde corretamente quando um SPNameQualifier é solicitado na política NameID da solicitação. Isso também funciona para entrada iniciada pelo SP e a entrada iniciada pelo IdP será a seguinte.  Para saber mais sobre o protocolo SAML no Azure Active Directory, consulte [protocolo SAML de logon único](https://docs.microsoft.com/azure/active-directory/develop/single-sign-on-saml-protocol).

---

### <a name="azure-ad-b2b-collaboration-supports-inviting-msa-and-google-users-in-azure-government-tenants"></a>A colaboração B2B do Azure AD dá suporte a convidar usuários do MSA e do Google nos locatários do Azure governamental

**Tipo:** novo recurso  
**Categoria de serviço:** B2B  
**Funcionalidade do produto:** B2B/B2C
 

Os locatários do Azure governamental usando os recursos de colaboração B2B agora podem convidar usuários que têm uma conta da Microsoft ou do Google. Para descobrir se o seu locatário pode usar esses recursos, siga as instruções em [como posso saber se a colaboração B2B está disponível no meu locatário do governo dos EUA do Azure?](https://docs.microsoft.com/azure/active-directory/b2b/current-limitations#how-can-i-tell-if-b2b-collaboration-is-available-in-my-azure-us-government-tenant)

 
---
 
### <a name="user-object-in-ms-graph-v1-now-includes-externaluserstate-and-externaluserstatechangeddatetime-properties"></a>O objeto de usuário no MS Graph v1 agora inclui as propriedades externalUserState e externalUserStateChangedDateTime

**Tipo:** novo recurso  
**Categoria de serviço:** B2B  
**Funcionalidade do produto:** B2B/B2C
 

As propriedades externalUserState e externalUserStateChangedDateTime podem ser usadas para encontrar convidados B2B convidados que ainda não aceitaram seus convites, bem como a automação da compilação, como a exclusão de usuários que não aceitaram seus convites após um número de dias. Essas propriedades agora estão disponíveis no MS Graph v1. Para obter orientação sobre como usar essas propriedades, consulte [tipo de recurso de usuário](https://docs.microsoft.com/graph/api/resources/user?view=graph-rest-1.0).
 
---

### <a name="manage-authentication-sessions-in-azure-ad-conditional-access-is-now-generally-available"></a>Gerenciar sessões de autenticação no acesso condicional do Azure AD agora está disponível para o público geral

**Tipo:** novo recurso  
**Categoria de serviço:** Acesso condicional  
**Funcionalidade do produto:** segurança e proteção da identidade
 
Os recursos de gerenciamento de sessão de autenticação permitem que você configure com que frequência os usuários precisam fornecer credenciais de entrada e se precisam fornecer credenciais após fechar e reabrir os navegadores para oferecer mais segurança e flexibilidade em seu ambiente.
 
Além disso, o gerenciamento de sessão de autenticação usado para se aplicar somente à autenticação de primeiro fator no Azure AD ingressado, ingressado no Azure AD híbrido e dispositivos registrados no Azure AD. Agora, o gerenciamento de sessão de autenticação também será aplicado ao MFA. Para obter mais informações, consulte [Configurar o gerenciamento de sessão de autenticação com acesso condicional](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-session-lifetime).

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---june-2020"></a>Novos aplicativos federados disponíveis na Galeria de aplicativos do Azure AD – junho de 2020

**Tipo:** novo recurso  
**Categoria de serviço:** Aplicativos empresariais  
**Funcionalidade do produto:** integração de terceiros
 
Em junho de 2020, adicionamos os 29 novos aplicativos a seguir em nossa galeria de aplicativos com suporte à Federação:

[Shopify Plus](https://docs.microsoft.com/azure/active-directory/saas-apps/shopify-plus-tutorial), [Ekarda](https://docs.microsoft.com/azure/active-directory/saas-apps/ekarda-tutorial), [MailGates](https://docs.microsoft.com/azure/active-directory/saas-apps/mailgates-tutorial), [BullseyeTDP](https://docs.microsoft.com/azure/active-directory/saas-apps/bullseyetdp-tutorial), [Raketa](https://docs.microsoft.com/azure/active-directory/saas-apps/raketa-tutorial), [Segment](https://docs.microsoft.com/azure/active-directory/saas-apps/segment-tutorial), [ai auditor](https://www.mindbridge.ai/products/ai-auditor/), [Pobuca Connect](https://app.pobu.ca/), [proto.Io](https://docs.microsoft.com/azure/active-directory/saas-apps/proto.io-tutorial), [gatekeeper](https://www.gatekeeperhq.com/), [planejador de Hub](https://docs.microsoft.com/azure/active-directory/saas-apps/hub-planner-tutorial), [caixa de ferramentas de Ansira para o mercado](https://ansira.com/technology/channel-engagement), [IBM Digital Business Automation na nuvem](https://docs.microsoft.com/azure/active-directory/saas-apps/ibm-digital-business-automation-on-cloud-tutorial), [segurança física Kisi](https://docs.microsoft.com/azure/active-directory/saas-apps/kisi-physical-security-tutorial), [ViewpointOne](https://team.viewpoint.com/), [IntelligenceBank](https://docs.microsoft.com/azure/active-directory/saas-apps/intelligencebank-tutorial), [pymetrics](https://docs.microsoft.com/azure/active-directory/saas-apps/pymetrics-tutorial), [zero](https://www.teamzero.com/), [instation](https://instation.invillia.com/), [edX para negócios SAML 2,0 integração](https://docs.microsoft.com/azure/active-directory/saas-apps/edx-for-business-saml-integration-tutorial), [MOOC Office 365](https://mooc.office365-training.com/en/), [SmartKargo](https://docs.microsoft.com/azure/active-directory/saas-apps/smartkargo-tutorial), [PKIsigning plataforma](https://platform.pkisigning.nl/), [SiteIntel](https://docs.microsoft.com/azure/active-directory/saas-apps/siteintel-tutorial), [ID de campo](https://docs.microsoft.com/azure/active-directory/saas-apps/field-id-tutorial), [currículos SAML](https://docs.microsoft.com/azure/active-directory/saas-apps/curricula-saml-tutorial), [Perforce Helix Core-Helix serviço de autenticação](https://docs.microsoft.com/azure/active-directory/saas-apps/perforce-helix-core-tutorial), minha [conformidade Cloud](https://cloud.metacompliance.com/), [Smallstep SSH](https://smallstep.com/sso-ssh/)  

Você também pode encontrar a documentação de todos os aplicativos aqui https://aka.ms/AppsTutorial . Para listar seu aplicativo na Galeria de aplicativos do Azure AD, leia os detalhes aqui: https://aka.ms/AzureADAppRequest .

---

### <a name="api-connectors-for-external-identities-self-service-sign-up-are-now-in-public-preview"></a>Conectores de API para identidade externa a inscrição de autoatendimento agora estão em visualização pública

**Tipo:** novo recurso  
**Categoria de serviço:** B2B  
**Funcionalidade do produto:** B2B/B2C
 
Conectores de API de identidades externas permitem que você aproveite as APIs da Web para integrar a inscrição de autoatendimento com sistemas de nuvem externos. Isso significa que agora você pode invocar APIs Web como etapas específicas em um fluxo de inscrição para disparar fluxos de trabalho personalizados baseados em nuvem. Por exemplo, você pode usar conectores de API para:

- Integre-se a fluxos de trabalho de aprovação personalizados.
- Executar a verificação de identidade
- Validar dados de entrada do usuário
- Substituir atributos de usuário
- Executar lógica de negócios personalizada

Para obter mais informações sobre todas as experiências possíveis com conectores de API, consulte [usar conectores de API para personalizar e estender a inscrição de autoatendimento](https://docs.microsoft.com/azure/active-directory/b2b/api-connectors-overview)ou [Personalizar identidades externas inscrição de autoatendimento com integrações de API Web](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/customize-external-identities-self-service-sign-up-with-web-api/ba-p/1257364#.XvNz2fImuQg.linkedin).
 
---

### <a name="provision-on-demand-and-get-users-into-your-apps-in-seconds"></a>Provisione sob demanda e obtenha usuários em seus aplicativos em segundos

**Tipo:** novo recurso  
**Categoria de serviço:** provisionamento de aplicativos  
**Funcionalidade do produto:** Gerenciamento do ciclo de vida de identidade
 
O serviço de provisionamento do Azure AD opera em um momento cíclico. O serviço é executado a cada 40 minutos. O [recurso de provisionamento sob demanda](https://aka.ms/provisionondemand) permite que você escolha um usuário e provisione-os em segundos. Essa funcionalidade permite que você solucione problemas de provisionamento rapidamente, sem precisar fazer uma reinicialização para forçar o ciclo de provisionamento a iniciar novamente. 
 
---

### <a name="new-permission-for-using-azure-ad-entitlement-management-in-graph"></a>Nova permissão para usar o gerenciamento de direitos do Azure AD no grafo

**Tipo:** novo recurso  
**Categoria de serviço:** outro  
**Funcionalidade do produto:** Gerenciamento de direitos
 
Uma nova permissão delegada EntitlementManagement. Read. All agora está disponível para uso com a API de gerenciamento de direitos no Microsoft Graph beta. Para saber mais sobre as APIs disponíveis, consulte [trabalhando com a API de gerenciamento de direitos do Azure ad](https://docs.microsoft.com/graph/api/resources/entitlementmanagement-root?view=graph-rest-beta).

---

### <a name="identity-protection-apis-available-in-v10"></a>APIs de proteção de identidade disponíveis em v 1.0

**Tipo:** novo recurso  
**Categoria de serviço:** Proteção de identidade  
**Funcionalidade do produto:** segurança e proteção da identidade
 
As APIs de Microsoft Graph riskyUsers e riskDetections agora estão disponíveis para o público geral. Agora que estão disponíveis no ponto de extremidade v 1.0, convidamos você a usá-los na produção. Para obter mais informações, consulte o [Microsoft Graph docs](https://docs.microsoft.com/graph/api/resources/identityprotectionroot?view=graph-rest-1.0).
 
---

### <a name="sensitivity-labels-to-apply-policies-to-microsoft-365-groups-is-now-generally-available"></a>Os rótulos de sensibilidade para aplicar políticas a grupos de Microsoft 365 agora estão disponíveis para o público geral

**Tipo:** novo recurso  
**Categoria de serviço:** gerenciamento de grupo  
**Recurso de produto:** colaboração
 

Agora você pode criar rótulos de sensibilidade e usar as configurações de rótulo para aplicar políticas a grupos de Microsoft 365, incluindo privacidade (pública ou privada) e política de acesso de usuário externo. Você pode criar um rótulo com a política de privacidade como particular e a política de acesso de usuário externo para não permitir que o adicione usuários convidados. Quando um usuário aplica esse rótulo a um grupo, o grupo será particular e nenhum usuário convidado poderá ser adicionado ao grupo. 

Rótulos de sensibilidade são importantes para proteger seus dados críticos para os negócios e permitem gerenciar grupos em escala, de maneira segura e em conformidade. Para obter orientação sobre como usar rótulos de sensibilidade, consulte [atribuir rótulos de sensibilidade a grupos do Office 365 em Azure Active Directory (versão prévia)](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-assign-sensitivity-labels).
 
---

### <a name="updates-to-support-for-microsoft-identity-manager-for-azure-ad-premium-customers"></a>Atualizações para suporte para Microsoft Identity Manager para clientes do Azure AD Premium

**Tipo:** recurso alterado  
**Categoria de serviço:** Microsoft Identity Manager  
**Funcionalidade do produto:** Gerenciamento do ciclo de vida de identidade
 
O suporte do Azure agora está disponível para os componentes de integração do Azure AD do Microsoft Identity Manager 2016, por meio do fim do suporte estendido para Microsoft Identity Manager 2016. Leia mais em [atualização de suporte para clientes Azure ad Premium usando Microsoft Identity Manager](https://docs.microsoft.com/microsoft-identity-manager/support-update-for-azure-active-directory-premium-customers).

---

### <a name="the-use-of-group-membership-conditions-in-sso-claims-configuration-is-increased"></a>O uso de condições de associação de grupo na configuração de declarações de SSO é aumentado

**Tipo:** recurso alterado  
**Categoria de serviço:** Aplicativos empresariais  
**Funcionalidade do produto:** SSO
 
Anteriormente, o número de grupos que você poderia usar ao alterar condicionalmente as declarações com base na associação de grupo em qualquer configuração de aplicativo individual era limitada a 10. O uso de condições de associação de grupo na configuração de declarações de SSO agora aumentou para um máximo de 50 grupos. Para obter mais informações sobre como configurar declarações, consulte [configuração de declarações de SSO de aplicativos empresariais](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization#emitting-claims-based-on-conditions). 

---

### <a name="enabling-basic-formatting-on-the-sign-in-page-text-component-in-company-branding"></a>Habilitando a formatação básica no componente texto da página de entrada na identidade visual da empresa.

**Tipo:** recurso alterado  
**Categoria de serviço:** autenticações (logons)  
**Funcionalidade do produto:** Autenticação do usuário
 
A funcionalidade de identidade visual da empresa na experiência de logon do Azure AD/Microsoft 365 foi atualizada para permitir que o cliente adicione hiperlinks e formatação simples, incluindo negrito, fonte, sublinhado e itálico. Para obter orientação sobre como usar essa funcionalidade, consulte [Adicionar identidade visual à página de entrada Azure Active Directory de sua organização](https://docs.microsoft.com/azure/active-directory/fundamentals/customize-branding).

---

### <a name="provisioning-performance-improvements"></a>Aprimoramentos de desempenho de provisionamento

**Tipo:** recurso alterado  
**Categoria de serviço:** provisionamento de aplicativos  
**Funcionalidade do produto:** Gerenciamento do ciclo de vida de identidade
 
O serviço de provisionamento foi atualizado para reduzir o tempo de conclusão de um [ciclo incremental](https://docs.microsoft.com/azure/active-directory/app-provisioning/how-provisioning-works#incremental-cycles) . Isso significa que os usuários e grupos serão provisionados em seus aplicativos mais rapidamente do que antes. Todos os novos trabalhos de provisionamento criados após 6/10/2020 irão se beneficiar automaticamente das melhorias de desempenho. Todos os aplicativos configurados para provisionamento antes de 6/10/2020 precisarão ser reiniciados uma vez após 6/10/2020 para aproveitar as melhorias de desempenho. 

---

### <a name="announcing-the-deprecation-of-adal-and-ms-graph-parity"></a>Anunciando a reprovação da ADAL e da paridade do MS Graph

**Tipo:** preterido  
**Categoria de serviço:** N/A  
**Funcionalidade do produto:** Gerenciamento do ciclo de vida do dispositivo

Agora que as MSAL (bibliotecas de autenticação da Microsoft) estão disponíveis, não adicionaremos mais novos recursos às bibliotecas de autenticação de Azure Active Directory (ADAL) e encerraremos os patches de segurança em 30 de junho de 2022. Para obter mais informações sobre como migrar para o MSAL, consulte [migrar aplicativos para a biblioteca de autenticação da Microsoft (MSAL)](https://docs.microsoft.com/azure/active-directory/develop/msal-migration).

Além disso, concluimos o trabalho para disponibilizar todas as funcionalidades do Azure AD Graph por meio do MS Graph. Assim, as APIs do Azure AD Graph receberão apenas Bugfix e correções de segurança até 30 de junho de 2022. Para obter mais informações, consulte [atualizar seus aplicativos para usar a biblioteca de autenticação da Microsoft e a API de Microsoft Graph](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/update-your-applications-to-use-microsoft-authentication-library/ba-p/1257363)
 
---
 
## <a name="may-2020"></a>Maio de 2020

### <a name="retirement-of-properties-in-signins-riskyusers-and-riskdetections-apis"></a>Desativação de propriedades em APIs entrada, riskyUsers e riskDetections

**Tipo:** plano de alteração  
**Categoria de serviço:** Proteção de identidade  
**Funcionalidade do produto:** segurança e proteção da identidade

Atualmente, os tipos enumerados são usados para representar a propriedade riscotype na API riskDetections e riskyUserHistoryItem (em versão prévia). Os tipos enumerados também são usados para a propriedade riskEventTypes na API entrada. No futuro, representaremos essas propriedades como cadeias de caracteres. 

Os clientes devem fazer a transição para a propriedade riskEventType na API beta riskDetections e riskyUserHistoryItem e para riskEventTypes_v2 Propriedade na API beta entrada por 9 de setembro de 2020. Nessa data, iremos desativar as propriedades CurrentType e riskEventTypes atuais. Para obter mais informações, consulte [alterações nas propriedades do evento de risco e APIs de proteção de identidade no Microsoft Graph](https://developer.microsoft.com/graph/blogs/changes-to-risk-event-properties-and-identity-protection-apis-on-microsoft-graph/).

--- 

### <a name="deprecation-of-riskeventtypes-property-in-signins-v10-api-on-microsoft-graph"></a>Substituição da propriedade riskEventTypes na API entrada v 1.0 no Microsoft Graph

**Tipo:** plano de alteração  
**Categoria de serviço:** relatórios  
**Funcionalidade do produto:** segurança e proteção da identidade

Os tipos enumerados mudarão para os tipos de cadeia de caracteres ao representar as propriedades do evento de risco no Microsoft Graph de setembro de 2020. Além de impactar as APIs de visualização, essa alteração também afetará a API entrada em produção.

Introduzimos uma nova propriedade riskEventsTypes_v2 (String) para a API entrada v 1.0. Desativaremos a propriedade riskEventTypes (enum) atual em 11 de junho de 2022 de acordo com nossa política de substituição de Microsoft Graph. Os clientes devem fazer a transição para a propriedade riskEventTypes_v2 na API entrada v 1.0 até 11 de junho de 2022. Para obter mais informações, consulte [reprovação da propriedade riskEventTypes na API entrada v 1.0 no Microsoft Graph](https://developer.microsoft.com/graph/blogs/deprecation-of-riskeventtypes-property-in-signins-v1-0-api-on-microsoft-graph//).

--- 

### <a name="upcoming-changes-to-mfa-email-notifications"></a>Alterações futuras nas notificações de email do MFA

**Tipo:** plano de alteração  
**Categoria de serviço:** FATO  
**Funcionalidade do produto:** segurança e proteção da identidade
 

Estamos fazendo as seguintes alterações nas notificações de email para a MFA de nuvem:

As notificações de email serão enviadas do seguinte endereço: azure-noreply@microsoft.com e msonlineservicesteam@microsoftonline.com . Estamos atualizando o conteúdo dos emails de alerta de fraude para indicar melhor as etapas necessárias para desbloquear os usos.

---

### <a name="new-self-service-sign-up-for-users-in-federated-domains-who-cant-access-microsoft-teams-because-they-arent-synced-to-azure-active-directory"></a>Nova inscrição de autoatendimento para usuários em domínios federados que não podem acessar o Microsoft Teams porque eles não são sincronizados com o Azure Active Directory.

**Tipo:** plano de alteração  
**Categoria de serviço:** autenticações (logons)  
**Funcionalidade do produto:** Autenticação do usuário
 

Atualmente, os usuários que estão em domínios federados no Azure AD, mas que não estão sincronizados com o locatário, não podem acessar as equipes. A partir do final de junho, essa nova funcionalidade permitirá que ele faça isso estendendo o recurso de inscrição verificada por email existente. Isso permitirá que os usuários que podem entrar em um IdP federado, mas que ainda não tenham um objeto de usuário na ID do Azure, tenham um objeto de usuário criado automaticamente e sejam autenticados para as equipes. Seu objeto de usuário será marcado como "inscrição de autoatendimento". Essa é uma extensão da capacidade existente de enviar por email a autoinscrição verificada que os usuários em domínios gerenciados podem fazer e podem ser controlados usando o mesmo sinalizador. Essa alteração concluirá a distribuição durante os dois meses a seguir. Veja as atualizações de documentação [aqui](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-self-service-signup).
 
---

### <a name="upcoming-fix-the-oidc-discovery-document-for-the-azure-government-cloud-is-being-updated-to-reference-the-correct-graph-endpoints"></a>Próxima correção: o documento de descoberta do OIDC para a nuvem do Azure governamental está sendo atualizado para fazer referência aos pontos de extremidade corretos do grafo.

**Tipo:** plano de alteração  
**Categoria de serviço:** nuvens soberanas  
**Funcionalidade do produto:** Autenticação do usuário
 
A partir de junho, o documento de descoberta do OIDC da [plataforma Microsoft Identity e o protocolo OpenID Connect](https://docs.microsoft.com/azure/active-directory/develop/v2-protocols-oidc) no ponto de extremidade de [nuvem do Azure governamental](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud) (login.microsoftonline.us) começarão a retornar o ponto de extremidade correto do [grafo de nuvem nacional](https://docs.microsoft.com/graph/deployments) ( https://graph.microsoft.us ou https://dod-graph.microsoft.us) , com base no locatário fornecido.  Atualmente, ele fornece o campo de ponto de extremidade de grafo incorreto (graph.microsoft.com) "msgraph_host".  

Essa correção de bug será distribuída gradualmente por aproximadamente 2 meses.  

---

### <a name="azure-government-users-will-no-longer-be-able-to-sign-in-on-loginmicrosoftonlinecom"></a>Os usuários do Azure governamental não poderão mais entrar no login.microsoftonline.com

**Tipo:** Planejar alteração  
**Categoria de serviço:** nuvens soberanas  
**Funcionalidade do produto:** Autenticação do usuário
 
Em 1 de junho de 2018, a autoridade de Azure Active Directory oficial (AAD) para o Azure governamental mudou de https://login-us.microsoftonline.com para https://login.microsoftonline.us . Se você possui um aplicativo em um locatário do Azure governamental, você deve atualizar seu aplicativo para conectar os usuários no ponto de extremidade. EUA.

A partir de 1º de maio, o Azure AD começará a impor a alteração do ponto de extremidade, bloqueando os usuários do Azure governamental de entrar em aplicativos hospedados nos locatários do Azure governamental usando o ponto de extremidade público (microsoftonline.com). Os aplicativos impactados começarão a ver um erro AADSTS900439-USGClientNotSupportedOnPublicEndpoint. 

Haverá uma distribuição gradual dessa alteração com a imposição prevista para ser concluída em todos os aplicativos de junho de 2020. Para obter mais detalhes, consulte a [postagem no blog do Azure governamental](https://devblogs.microsoft.com/azuregov/azure-government-aad-authority-endpoint-update/).

---

### <a name="saml-single-logout-request-now-sends-nameid-in-the-correct-format"></a>A solicitação de logoff único do SAML agora envia NameID no formato correto

**Tipo:** corrigido  
**Categoria de serviço:** autenticações (logons)  
**Funcionalidade do produto:** Autenticação do usuário
 
Quando um usuário clica em sair (por exemplo, no portal myapps), o Azure AD envia uma mensagem de logoff único do SAML para cada aplicativo que está ativo na sessão do usuário e tem uma URL de logout configurada. Essas mensagens contêm uma NameID em um formato persistente.

Se o token de entrada SAML original usou um formato diferente para NameID (por exemplo, email/UPN), o aplicativo SAML não poderá correlacionar a NameID na mensagem de logout a uma sessão existente (já que o NameIDs usado em ambas as mensagens é diferente), o que fez com que a mensagem de logout seja descartada pelo aplicativo SAML e o usuário permaneça conectado. Essa correção torna a mensagem de saída consistente com a NameID configurada para o aplicativo.

---

### <a name="hybrid-identity-administrator-role-is-now-available-with-cloud-provisioning"></a>A função de administrador de identidade híbrida agora está disponível com provisionamento de nuvem

**Tipo:** novo recurso  
**Categoria de serviço:** Provisionamento de nuvem do Azure AD  
**Funcionalidade do produto:** Gerenciamento do ciclo de vida de identidade
 
Os administradores de ti podem começar a usar a nova função "administrador híbrido" como a função menos privilegiada para configurar o provisionamento de nuvem do Azure ADConnect. Com essa nova função, você não precisa mais usar a função de administrador global para configurar e configurar o provisionamento de nuvem. [Saiba mais](https://docs.microsoft.com/azure/active-directory/users-groups-roles/roles-delegate-by-task#connect).
 
---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---may-2020"></a>Novos aplicativos federados disponíveis na Galeria de aplicativos do Azure AD – maio de 2020

**Tipo:** novo recurso  
**Categoria de serviço:** Aplicativos empresariais  
**Funcionalidade do produto:** integração de terceiros
 
Em maio de 2020, adicionamos os seguintes 36 novos aplicativos em nossa galeria de aplicativos com suporte à Federação:

[Moula](https://moula.com.au/pay/merchants), [Surveypal](https://www.surveypal.com/app), [Kbot365](https://www.konverso.ai/virtual-assistant-digital-workplace/), [TackleBox](http://www.tacklebox.app/), [Powell Teams](https://powell-software.com/en/powell-teams-en/), [Talentsoft Assistant](https://msteams.talent-soft.com/), [ASC Recording insights](https://teams.asc-recording.app/product), [GO1](https://www.go1.com/), [B-engrenado](https://b-engaged.se/), [Competella Contact Center Workgroup](http://www.competella.com/), [Asite](http://www.asite.com/), [ImageSoft Identity](https://identity.imagesoftinc.com/), [meu IBISWorld](https://identity.imagesoftinc.com/), [adequação](https://docs.microsoft.com/azure/active-directory/saas-apps/insuite-tutorial), [Gerenciamento de processo de alteração](https://docs.microsoft.com/azure/active-directory/saas-apps/change-process-management-tutorial), plataforma de garantia do [Cyara CX](https://docs.microsoft.com/azure/active-directory/saas-apps/cyara-cx-assurance-platform-tutorial), [governança inteligente](https://docs.microsoft.com/azure/active-directory/saas-apps/smart-global-governance-tutorial) [,,](https://docs.microsoft.com/azure/active-directory/saas-apps/prezi-tutorial) [Mapbox](https://docs.microsoft.com/azure/active-directory/saas-apps/mapbox-tutorial), [datava Enterprise Service Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/datava-enterprise-service-platform-tutorial), [estranho](https://docs.microsoft.com/azure/active-directory/saas-apps/whimsical-tutorial), [Trelica](https://docs.microsoft.com/azure/active-directory/saas-apps/trelica-tutorial), [EasySSO para Confluence](https://docs.microsoft.com/azure/active-directory/saas-apps/easysso-for-confluence-tutorial), [EasySSO para BitBucket](https://docs.microsoft.com/azure/active-directory/saas-apps/easysso-for-bitbucket-tutorial), [EasySSO para bambu](https://docs.microsoft.com/azure/active-directory/saas-apps/easysso-for-bamboo-tutorial), [torii](https://docs.microsoft.com/azure/active-directory/saas-apps/torii-tutorial), [Axiad Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/axiad-cloud-tutorial), [humanment](https://docs.microsoft.com/azure/active-directory/saas-apps/humanage-tutorial), ColorTokens [ZTNA](https://docs.microsoft.com/azure/active-directory/saas-apps/colortokens-ztna-tutorial), [cch Tagetik](https://docs.microsoft.com/azure/active-directory/saas-apps/cch-tagetik-tutorial), [ShareVault](https://docs.microsoft.com/azure/active-directory/saas-apps/sharevault-tutorial), [Vyond](https://docs.microsoft.com/azure/active-directory/saas-apps/vyond-tutorial), [superexpander](https://docs.microsoft.com/azure/active-directory/saas-apps/textexpander-tutorial), [todos os CRM](https://docs.microsoft.com/azure/active-directory/saas-apps/anyone-home-crm-tutorial), [askSpoke](https://docs.microsoft.com/azure/active-directory/saas-apps/askspoke-tutorial), [centro de contato Ice](https://docs.microsoft.com/azure/active-directory/saas-apps/ice-contact-center-tutorial)

Você também pode encontrar a documentação de todos os aplicativos aqui https://aka.ms/AppsTutorial .

Para listar seu aplicativo na Galeria de aplicativos do Azure AD, leia os detalhes aqui https://aka.ms/AzureADAppRequest .

---

### <a name="report-only-mode-for-conditional-access-is-now-generally-available"></a>O modo somente de relatório para acesso condicional agora está disponível para o público geral

**Tipo:** novo recurso  
**Categoria de serviço:** Acesso condicional  
**Funcionalidade do produto:** segurança e proteção da identidade

O [modo somente de relatório para acesso condicional do Azure ad](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-report-only) permite avaliar o resultado de uma política sem impor controles de acesso. Você pode testar políticas somente de relatório em sua organização e entender seu impacto antes de habilitá-las, tornando a implantação mais segura e fácil. Nos últimos meses, vimos uma adoção forte do modo somente de relatório – em 26M, os usuários já estão no escopo de uma política somente de relatório. Com o comunicado hoje, as novas políticas de acesso condicional do Azure AD serão criadas no modo somente de relatório por padrão. Isso significa que você pode monitorar o impacto de suas políticas desde o momento em que elas são criadas. E para aqueles que usam as APIs do MS Graph, você também pode [gerenciar políticas somente de relatório de forma programática](https://docs.microsoft.com/graph/api/resources/conditionalaccesspolicy?view=graph-rest-beta) . 

---

### <a name="self-service-sign-up-for-guest-users"></a>Inscrição de autoatendimento para usuários convidados

**Tipo:** novo recurso  
**Categoria de serviço:** B2B  
**Funcionalidade do produto:** B2B/B2C
 
Com identidades externas no Azure AD, você pode permitir que as pessoas fora da sua organização acessem seus aplicativos e recursos enquanto permitem que eles entrem usando qualquer identidade que preferir. Quando compartilhar um aplicativo com usuários externos, é possível que você não saiba com antecedência quem precisará de acesso a um aplicativo. Com a [inscrição de autoatendimento](https://docs.microsoft.com/azure/active-directory/b2b/self-service-sign-up-overview), você pode habilitar usuários convidados para inscrever-se e obter uma conta de convidado para seus aplicativos de linha de negócios (LOB). O fluxo de inscrição pode ser criado e personalizado para dar suporte a identidades sociais e do Azure AD. Você também pode coletar informações adicionais sobre o usuário durante a inscrição.

---

 ### <a name="conditional-access-insights-and-reporting-workbook-is-generally-available"></a>As informações de acesso condicional e a pasta de trabalho de relatórios estão geralmente disponíveis

**Tipo:** novo recurso  
**Categoria de serviço:** Acesso condicional  
**Funcionalidade do produto:** segurança e proteção da identidade

A [pasta de trabalho insights e Reporting](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-insights-reporting) fornece aos administradores uma exibição resumida do acesso condicional do Azure AD em seu locatário. Com a capacidade de selecionar uma política individual, os administradores podem entender melhor o que cada política faz e monitorar as alterações em tempo real. A pasta de trabalho transmite os dados armazenados no Azure Monitor, que você pode configurar em alguns minutos [seguindo estas instruções](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics). Para tornar o painel mais detectável, nós o movemos para a guia novas informações e relatórios no menu de acesso condicional do Azure AD.

---

### <a name="policy-details-blade-for-conditional-access-is-in-public-preview"></a>A folha detalhes da política para acesso condicional está em visualização pública

**Tipo:** novo recurso  
**Categoria de serviço:** Acesso condicional  
**Funcionalidade do produto:** segurança e proteção da identidade

A [folha detalhes](https://docs.microsoft.com/azure/active-directory/conditional-access/troubleshoot-conditional-access) da nova política exibe as atribuições, as condições e os controles atendidos durante a avaliação da política de acesso condicional. Você pode acessar a folha selecionando uma linha nas guias acesso condicional ou somente relatório dos detalhes de entrada.

---

### <a name="new-query-capabilities-for-directory-objects-in-microsoft-graph-are-in-public-preview"></a>Novos recursos de consulta para objetos de diretório no Microsoft Graph estão em visualização pública

**Tipo:** novo recurso  
**Categoria de serviço:** Funcionalidade do **produto** MS Graph: experiência do desenvolvedor

Novos recursos estão sendo introduzidos para APIs de objetos do Microsoft Graph Directory, habilitando as operações de contagem, pesquisa, filtro e classificação. Isso dará aos desenvolvedores a capacidade de consultar rapidamente nossos objetos de diretório sem soluções alternativas como filtragem e classificação na memória. Saiba mais nesta [postagem no blog](https://aka.ms/CountFilterMSGraphAAD).

Estamos atualmente em visualização pública, procurando comentários. Envie seus comentários com esta [breve pesquisa](https://aka.ms/MsGraphAADSurveyDocs).

---

### <a name="configure-saml-based-single-sign-on-using-microsoft-graph-api-beta"></a>Configurar o logon único baseado em SAML usando a API do Microsoft Graph (beta)

**Tipo:** novo recurso  
**Categoria de serviço:** Aplicativos empresariais  
**Funcionalidade do produto:** SSO
 
O suporte para criar e configurar um aplicativo da galeria do Azure AD usando APIs do MS Graph no beta agora está disponível. Se você precisar configurar o logon único baseado em SAML para várias instâncias de um aplicativo, Economize tempo usando as APIs de Microsoft Graph para [automatizar a configuração do logon único baseado em SAML](https://docs.microsoft.com/azure/active-directory/manage-apps/application-saml-sso-configure-api).
 
---

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---may-2020"></a>Novos conectores de provisionamento na Galeria de aplicativos do Azure AD – maio de 2020

**Tipo:** novo recurso  
**Categoria de serviço:** provisionamento de aplicativos  
**Funcionalidade do produto:** integração de terceiros
 
Agora você pode automatizar a criação, a atualização e a exclusão de contas de usuário para esses aplicativos integrados recentemente:

* [8x8](https://docs.microsoft.com/azure/active-directory/saas-apps/8x8-provisioning-tutorial)
* [Juno Journey](https://docs.microsoft.com/azure/active-directory/saas-apps/juno-journey-provisioning-tutorial)
* [MediusFlow](https://docs.microsoft.com/azure/active-directory/saas-apps/mediusflow-provisioning-tutorial)
* [New Relic por Organização](https://docs.microsoft.com/azure/active-directory/saas-apps/new-relic-by-organization-provisioning-tutorial)
* [Console da Infraestrutura de Nuvem da Oracle](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-cloud-infratstructure-console-provisioning-tutorial)

Para obter mais informações para proteger melhor sua organização com o provisionamento automatizado de contas de usuário, consulte [Automatizar o provisionamento de usuário para aplicativos SaaS com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="saml-token-encryption-is-generally-available"></a>A criptografia de token SAML está geralmente disponível

**Tipo:** novo recurso  
**Categoria de serviço:** Aplicativos empresariais  
**Funcionalidade do produto:** SSO
 
A [criptografia de token SAML](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption) permite que os aplicativos sejam configurados para receber asserções SAML criptografadas. Agora, o recurso está disponível em todas as nuvens.
 
---

### <a name="group-name-claims-in-application-tokens-is-generally-available"></a>As declarações de nome de grupo em tokens de aplicativo estão geralmente disponíveis

**Tipo:** novo recurso  
**Categoria de serviço:** Aplicativos empresariais  
**Funcionalidade do produto:** SSO
 
As declarações de grupo emitidas em um token agora podem ser limitadas apenas aos grupos atribuídos ao aplicativo.  Isso é especialmente importante quando os usuários são membros de um grande número de grupos e houve um risco de exceder os limites de tamanho de token. Com esse novo recurso em vigor, a capacidade de [Adicionar nomes de grupo a tokens](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-fed-group-claims) está disponível para o público geral.
 
---

### <a name="workday-writeback-now-supports-setting-work-phone-number-attributes"></a>O Write-back do workday agora dá suporte à configuração de atributos de número de

**Tipo:** novo recurso  
**Categoria de serviço:** provisionamento de aplicativos  
**Funcionalidade do produto:** Gerenciamento do ciclo de vida de identidade
 
Aprimoramos o aplicativo de provisionamento do workday write-back para agora dar suporte ao Write-back dos atributos número de telefone comercial e número móvel. Além de email e nome de usuário, agora você pode configurar o aplicativo de provisionamento de write-back do WORKDAY para transmitir valores de número de telefone do Azure AD para o workday. Para obter mais detalhes sobre como configurar o Write-back de número de telefone, consulte o tutorial do aplicativo de [write-back do workday](https://aka.ms/WorkdayWriteback) . 

---

### <a name="publisher-verification-preview"></a>Verificação do Publicador (versão prévia)

**Tipo:** novo recurso  
**Categoria de serviço:** outro  
**Funcionalidade do produto:** experiência de desenvolvedor
 
A verificação do editor (versão prévia) ajuda administradores e usuários finais a entender a autenticidade dos desenvolvedores de aplicativos que se integram com a plataforma de identidade da Microsoft. Para obter detalhes, consulte [verificação do Publicador (versão prévia)](https://docs.microsoft.com/azure/active-directory/develop/publisher-verification-overview).
 
---

### <a name="authorization-code-flow-for-single-page-apps"></a>Fluxo de código de autorização para aplicativos de página única

**Tipo:** Categoria do **serviço** de recurso alterado: **funcionalidade do produto** de autenticação: experiência do desenvolvedor

Devido às restrições de cookie de terceiros do navegador moderno, [como o Safari ITP](https://docs.microsoft.com/azure/active-directory/develop/reference-third-party-cookies-spas), o spas terá que usar o fluxo do código de autorização em vez do fluxo implícito para manter o SSO; MSAL.js v 2. x agora dará suporte ao fluxo do código de autorização. Como as atualizações correspondentes para o portal do Azure, você pode atualizar seu SPA para que ele seja do tipo "Spa" e usar o fluxo de código de autenticação. Para obter diretrizes, consulte [início rápido: conectar usuários e obter um token de acesso em um JavaScript Spa usando o fluxo de código de autenticação](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v2-javascript-auth-code).

---

### <a name="improved-filtering-for-devices-is-in-public-preview"></a>A filtragem aprimorada para dispositivos está em visualização pública

**Tipo:** Recurso alterado   
**Categoria de serviço:** Funcionalidade do **produto** de gerenciamento de dispositivos: gerenciamento de ciclo de vida do dispositivo
 
Anteriormente, os únicos filtros que você poderia usar eram "Enabled" e "data da atividade". Agora, você pode [filtrar a lista de dispositivos em mais propriedades](https://docs.microsoft.com/azure/active-directory/devices/device-management-azure-portal#device-list-filtering-preview), incluindo tipo de sistema operacional, tipo de junção, conformidade e muito mais. Essas adições devem simplificar a localização de um dispositivo específico.

---

### <a name="the-new-app-registrations-experience-for-azure-ad-b2c-is-now-generally-available"></a>A nova experiência de Registros de aplicativo para Azure AD B2C agora está disponível para o público geral

**Tipo:** Recurso alterado   
**Categoria de serviço:** B2C - gerenciamento de identidades de consumidor  
**Funcionalidade do produto:** Gerenciamento do ciclo de vida de identidade
 
A nova experiência de Registros de aplicativo para Azure AD B2C agora está disponível para o público em geral. 

Anteriormente, era necessário gerenciar seus aplicativos voltados ao consumidor B2C separadamente do restante dos seus aplicativos usando a experiência de "aplicativos" herdada. Isso significava experiências de criação de aplicativo diferentes em locais diferentes no Azure.

A nova experiência mostra todos os registros de aplicativo do B2C e registros de aplicativo do Azure AD em um único local e fornece uma maneira consistente de gerenciá-los. Independentemente de você precisar gerenciar um aplicativo voltado para o cliente ou um aplicativo que tenha acesso ao Microsoft Graph para gerenciar programaticamente Azure AD B2C recursos, você só precisa aprender uma maneira de fazer coisas.

Você pode acessar a nova experiência navegando pelo serviço Azure AD B2C e selecionando a folha Registros de aplicativo. A experiência também pode ser acessada pelo serviço de Azure Active Directory.

A experiência de Registros de aplicativo Azure AD B2C é baseada na experiência geral de [registro de aplicativo](https://developer.microsoft.com/identity/blogs/new-app-registrations-experience-is-now-generally-available/) para locatários do Azure AD, mas é adaptada para Azure ad B2C. A experiência de "aplicativos" herdada será preterida no futuro.

Para obter mais informações, visite [a nova experiência de registro de aplicativo para Azure ad B2C](https://aka.ms/b2cappregtraining).

---

## <a name="april-2020"></a>Abril de 2020

### <a name="combined-security-info-registration-experience-is-now-generally-available"></a>A experiência de registro de informações de segurança combinada já está disponível para o público geral

**Tipo:** novo recurso

**Categoria de serviço:** autenticações (logons)

**Funcionalidade do produto:** segurança e proteção da identidade

A experiência de registro combinada para MFA (autenticação multifator) e redefinição de senha de autoatendimento (SSPR) agora está disponível para o público em geral. Essa nova experiência de registro permite que os usuários se registrem para MFA e SSPR em um único processo passo a passo. Quando você implanta a nova experiência para sua organização, os usuários podem se registrar em menos tempo e com menos complicações. Confira a postagem do blog [aqui](https://bit.ly/3etiRyQ).

---

### <a name="continuous-access-evaluation"></a>Avaliação de acesso contínuo

**Tipo:** novo recurso

**Categoria de serviço:** autenticações (logons)

**Funcionalidade do produto:** segurança e proteção da identidade

A avaliação de acesso contínuo é um novo recurso de segurança que permite a imposição quase em tempo real de políticas em partes confiáveis que consomem tokens de acesso do Azure AD quando ocorrem eventos no Azure AD (como a exclusão da conta de usuário). Estamos lançando esse recurso primeiro para equipes e clientes do Outlook. Para obter mais detalhes, leia nosso [blog](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/moving-towards-real-time-policy-and-security-enforcement/ba-p/1276933) e [documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-continuous-access-evaluation).

---

### <a name="sms-sign-in-firstline-workers-can-sign-in-to-azure-ad-backed-applications-with-their-phone-number-and-no-password"></a>Entrada do SMS: os trabalhos de início podem entrar em aplicativos com suporte do Azure AD com seu número de telefone e nenhuma senha

**Tipo:** novo recurso

**Categoria de serviço:** autenticações (logons)

**Funcionalidade do produto:** Autenticação do usuário

O Office está lançando uma série de aplicativos de negócios para dispositivos móveis que atendem a organizações não tradicionais e para funcionários em grandes organizações que não usam email como seu método de comunicação principal. Esses aplicativos visam funcionários frentes, trabalhadores sem mesa, agentes de campo ou funcionários de varejo que podem não receber um endereço de email de seu empregador, ter acesso a um computador ou a ele. Esse projeto permitirá que esses funcionários entrem em aplicativos de negócios inserindo um número de telefone e roundtripping um código. Para obter mais detalhes, consulte nossa [documentação de administração](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-sms-signin) e [documentação do usuário final](https://docs.microsoft.com/azure/active-directory/user-help/sms-sign-in-explainer).

---

### <a name="invite-internal-users-to-use-b2b-collaboration"></a>Convidar usuários internos para usar a colaboração B2B

**Tipo:** novo recurso

**Categoria de serviço:** B2B

**Funcionalidade do produto:**

Estamos expandindo o recurso de convite B2B para permitir que contas internas existentes sejam convidadas para usar as credenciais de colaboração B2B no futuro. Isso é feito passando o objeto de usuário para a API de convite, além de parâmetros típicos, como o endereço de email convidado. A ID de objeto do usuário, o UPN, a associação de grupo, a atribuição de aplicativo etc. permanecem intactos, mas continuará a usar o B2B para autenticar com suas credenciais de locatário inicial, em vez das credenciais internas usadas antes do convite. Para obter detalhes, consulte a [documentação](https://docs.microsoft.com/azure/active-directory/b2b/invite-internal-users).

---

### <a name="report-only-mode-for-conditional-access-is-now-generally-available"></a>O modo somente de relatório para acesso condicional agora está disponível para o público geral

**Tipo:** novo recurso

**Categoria de serviço:** Acesso condicional

**Funcionalidade do produto:** segurança e proteção da identidade

O [modo somente de relatório para acesso condicional do Azure ad](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-report-only) permite avaliar o resultado de uma política sem impor controles de acesso. Você pode testar políticas somente de relatório em sua organização e entender seu impacto antes de habilitá-las, tornando a implantação mais segura e fácil. Nos últimos meses, vimos uma adoção forte do modo somente de relatório, com 26M usuários já no escopo de uma política somente de relatório. Com este anúncio, as novas políticas de acesso condicional do Azure AD serão criadas no modo somente de relatório por padrão. Isso significa que você pode monitorar o impacto de suas políticas desde o momento em que elas são criadas. E para aqueles que usam as APIs do MS Graph, você também pode [gerenciar políticas somente de relatório programaticamente](https://docs.microsoft.com/graph/api/resources/conditionalaccesspolicy?view=graph-rest-beta). 

---

### <a name="conditional-access-insights-and-reporting-workbook-is-generally-available"></a>As informações de acesso condicional e a pasta de trabalho de relatórios estão geralmente disponíveis

**Tipo:** novo recurso

**Categoria de serviço:** Acesso condicional

**Funcionalidade do produto:** segurança e proteção da identidade

A pasta de [trabalho informações](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-insights-reporting) de acesso condicional e relatórios fornece aos administradores uma exibição resumida do acesso condicional do Azure AD em seu locatário. Com a capacidade de selecionar uma política individual, os administradores podem entender melhor o que cada política faz e monitorar as alterações em tempo real. A pasta de trabalho transmite os dados armazenados no Azure Monitor, que você pode configurar em alguns minutos [seguindo estas instruções](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics). Para tornar o painel mais detectável, nós o movemos para a guia novas informações e relatórios no menu de acesso condicional do Azure AD.

---

### <a name="policy-details-blade-for-conditional-access-is-in-public-preview"></a>A folha detalhes da política para acesso condicional está em visualização pública

**Tipo:** novo recurso

**Categoria de serviço:** Acesso condicional

**Funcionalidade do produto:** segurança e proteção da identidade

A [folha detalhes](https://docs.microsoft.com/azure/active-directory/conditional-access/troubleshoot-conditional-access) da nova política exibe quais atribuições, condições e controles foram atendidos durante a avaliação da política de acesso condicional. Você pode acessar a folha selecionando uma linha nas guias **acesso condicional** ou **somente relatório** dos detalhes de entrada.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---april-2020"></a>Novos aplicativos federados disponíveis na Galeria de Aplicativo Azure AD – abril de 2020

**Tipo:** novo recurso

**Categoria de serviço:** Aplicativos empresariais

**Funcionalidade do produto:** integração de terceiros

Em abril de 2020, adicionamos esses 31 novos aplicativos com suporte à Federação para a Galeria de aplicativos: 

[Aplicativos SincroPool](https://www.sincropool.com/), [SmartDB](https://hibiki.dreamarts.co.jp/smartdb/trial/), [float](https://docs.microsoft.com/azure/active-directory/saas-apps/float-tutorial), [LMS365](https://lms.365.systems/), [IWT de compras](https://docs.microsoft.com/azure/active-directory/saas-apps/iwt-procurement-suite-tutorial), [Lunni](https://lunni.fi/), [EasySSO para Jira](https://docs.microsoft.com/azure/active-directory/saas-apps/easysso-for-jira-tutorial), [virtual Training Academy](https://vta.c3p.ca/app/en/openid?authenticate_with=microsoft), [Meraki Dashboard](https://docs.microsoft.com/azure/active-directory/saas-apps/meraki-dashboard-tutorial), [Office 365 mover](https://app.mover.io/login), [palestrante](https://speakerengage.com/login.php), [honestamente](https://docs.microsoft.com/azure/active-directory/saas-apps/honestly-tutorial), [Ally](https://docs.microsoft.com/azure/active-directory/saas-apps/ally-tutorial), [DutyFlow](https://app.dutyflow.nl/), [AlertMedia](https://docs.microsoft.com/azure/active-directory/saas-apps/alertmedia-tutorial), [gr8 People](https://docs.microsoft.com/azure/active-directory/saas-apps/gr8-people-tutorial), [pendo](https://docs.microsoft.com/azure/active-directory/saas-apps/pendo-tutorial), [HighGround](https://docs.microsoft.com/azure/active-directory/saas-apps/highground-tutorial), [harmonia](https://docs.microsoft.com/azure/active-directory/saas-apps/harmony-tutorial), [timetabling Solutions](https://docs.microsoft.com/azure/active-directory/saas-apps/timetabling-solutions-tutorial), [SynchroNet Click](https://docs.microsoft.com/azure/active-directory/saas-apps/synchronet-click-tutorial), [Capacite](https://www.made-in-office.com/en/), s, decisivo [,](https://docs.microsoft.com/azure/active-directory/saas-apps/litmus-tutorial)GroupTalk [,](https://recorder.grouptalk.com/) [Frontify](https://docs.microsoft.com/azure/active-directory/saas-apps/frontify-tutorial), [nuvem MongoDB](https://docs.microsoft.com/azure/active-directory/saas-apps/mongodb-cloud-tutorial), [TickitLMS saiba](https://docs.microsoft.com/azure/active-directory/saas-apps/tickitlms-learn-tutorial), coco [,](https://hexaware.com/partnerships-and-alliances/digital-transformation-using-microsoft-azure/) [Nitro Productivity Suite](https://docs.microsoft.com/azure/active-directory/saas-apps/nitro-productivity-suite-tutorial) , TMWS [(Trend Micro Web Security)](https://review.docs.microsoft.com/azure/active-directory/saas-apps/trend-micro-tutorial) [Fortes Change Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/fortes-change-cloud-tutorial)

Para obter mais informações sobre os aplicativos, consulte [integração de aplicativos SaaS com o Active Directory do Azure](https://aka.ms/appstutorial). Para obter mais informações sobre como listar seu aplicativo na galeria de aplicativos do Azure AD, consulte [Listar seu aplicativo na galeria de aplicativos do Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="microsoft-graph-delta-query-support-for-oauth2permissiongrant-available-for-public-preview"></a>Suporte de consulta Delta Microsoft Graph para oAuth2PermissionGrant disponível para visualização pública

**Tipo:** novo recurso

**Categoria de serviço:** MS Graph

**Funcionalidade do produto:** experiência de desenvolvedor

A consulta Delta para oAuth2PermissionGrant está disponível para visualização pública! Agora você pode controlar as alterações sem precisar sondar continuamente Microsoft Graph. [Saiba mais.](https://docs.microsoft.com/graph/api/oAuth2PermissionGrant-delta?view=graph-rest-beta&tabs=http)

---

### <a name="microsoft-graph-delta-query-support-for-organizational-contact-generally-available"></a>Suporte de consulta Delta Microsoft Graph para contato organizacional disponível para o público geral

**Tipo:** novo recurso

**Categoria de serviço:** MS Graph

**Funcionalidade do produto:** experiência de desenvolvedor

A consulta Delta para contatos organizacionais está geralmente disponível! Agora você pode controlar as alterações nos aplicativos de produção sem precisar sondar continuamente Microsoft Graph. Substitua qualquer código existente que sonda continuamente dados de orgContact por consulta Delta para melhorar significativamente o desempenho. [Saiba mais.](https://docs.microsoft.com/graph/api/orgcontact-delta?view=graph-rest-1.0&tabs=http)

---

### <a name="microsoft-graph-delta-query-support-for-application-generally-available"></a>Suporte de consulta Delta Microsoft Graph para o aplicativo disponível para o público geral

**Tipo:** novo recurso

**Categoria de serviço:** MS Graph

**Funcionalidade do produto:** experiência de desenvolvedor

A consulta Delta para aplicativos está geralmente disponível! Agora você pode controlar as alterações nos aplicativos de produção sem precisar sondar continuamente Microsoft Graph. Substitua qualquer código existente que sonda continuamente os dados do aplicativo por consulta Delta para melhorar significativamente o desempenho. [Saiba mais.](https://docs.microsoft.com/graph/api/application-delta?view=graph-rest-1.0)

---

### <a name="microsoft-graph-delta-query-support-for-administrative-units-available-for-public-preview"></a>Suporte de consulta Delta Microsoft Graph para unidades administrativas disponíveis para visualização pública

**Tipo:** novo recurso

**Categoria de serviço:** MS Graph

**Funcionalidade do produto:** A consulta Delta de experiência do desenvolvedor para unidades administrativas está disponível para visualização pública! Agora você pode controlar as alterações sem precisar sondar continuamente Microsoft Graph. [Saiba mais.](https://docs.microsoft.com/graph/api/administrativeunit-delta?view=graph-rest-beta&tabs=http)

---

### <a name="manage-authentication-phone-numbers-and-more-in-new-microsoft-graph-beta-apis"></a>Gerenciar números de telefone de autenticação e mais em novas APIs do Microsoft Graph beta

**Tipo:** novo recurso

**Categoria de serviço:** MS Graph

**Funcionalidade do produto:** experiência de desenvolvedor

Essas APIs são uma ferramenta importante para gerenciar os métodos de autenticação dos seus usuários. Agora, você pode pré-configurar e gerenciar programaticamente os autenticadores usados para MFA e redefinição de senha de autoatendimento (SSPR). Esse é um dos recursos mais solicitados nos espaços do Azure MFA, SSPR e Microsoft Graph. As novas APIs que lançamos nesta onda oferecem a você a capacidade de:

- Ler, adicionar, atualizar e remover telefones de autenticação de um usuário
- Redefinir a senha de um usuário
- Ligar e desligar o SMS-Sign-in

Para obter mais informações, consulte [visão geral da API dos métodos de autenticação do Azure ad](https://docs.microsoft.com/graph/api/resources/authenticationmethods-overview?view=graph-rest-beta).

---

### <a name="administrative-units-public-preview"></a>Visualização pública de unidades administrativas

**Tipo:** novo recurso

**Categoria de serviço:** RBAC

**Funcionalidade do produto:** Controle de Acesso

Unidades administrativas permitem que você conceda permissões de administrador restritas a um departamento, uma região ou outro segmento de sua organização que você define. Você pode usar unidades administrativas para delegar permissões a administradores regionais ou definir uma política em um nível granular. Por exemplo, um administrador de conta de usuário poderia atualizar as informações do perfil, redefinir senhas e atribuir licenças para usuários apenas na unidade administrativa dele.

Usando unidades administrativas, um administrador central poderia:

- Criar uma unidade administrativa para o gerenciamento descentralizado de recursos
- Atribuir uma função com permissões administrativas somente para usuários do Azure AD em uma unidade administrativa
- Popular as unidades administrativas com usuários e grupos conforme necessário

Para obter mais informações, consulte [Gerenciamento de unidades administrativas em Azure Active Directory (versão prévia)](https://aka.ms/AdminUnitsDocs).

---

### <a name="printer-administrator-and-printer-technician-built-in-roles"></a>Funções internas de administrador de impressora e técnico de impressora

**Tipo:** novo recurso

**Categoria de serviço:** RBAC

**Funcionalidade do produto:** Controle de Acesso

**Administrador de impressora**: os usuários com essa função podem registrar impressoras e gerenciar todos os aspectos de todas as configurações de impressora na solução de impressão universal da Microsoft, incluindo as configurações do conector de impressão universal. Eles podem consentir com todas as solicitações de permissão de impressão delegadas. Os Administradores de impressora também têm acesso aos relatórios de impressão. 

**Técnico de impressora**: os usuários com essa função podem registrar impressoras e gerenciar o status da impressora na solução de impressão universal da Microsoft. Eles também podem ler todas as informações do conector. As principais tarefas que um técnico de impressora não pode fazer são definir permissões de usuário em impressoras e compartilhar impressoras. [Saiba mais.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#printer-administrator)

---

### <a name="hybrid-identity-admin-built-in-role"></a>Função interna do administrador de identidade híbrida

**Tipo:** novo recurso

**Categoria de serviço:** RBAC

**Funcionalidade do produto:** Controle de Acesso

Os usuários com essa função podem habilitar, definir e gerenciar serviços e configurações relacionados à habilitação da identidade híbrida no Azure AD. Essa função concede a capacidade de configurar o Azure AD para um dos três métodos de autenticação com suporte&#8212;sincronização de hash de senha (PHS), autenticação de passagem (PTA) ou Federação (AD FS ou provedor de Federação de terceiros) &#8212;e implantar a infraestrutura local relacionada para habilitá-los. A infraestrutura local inclui agentes de provisionamento e PTA. Essa função concede a capacidade de habilitar o S-SSO (Logon único contínuo) para habilitar a autenticação direta em dispositivos que não usem o Windows 10 ou em computadores que não usem o Windows Server 2016. Além disso, essa função concede a capacidade de ver os logs de entrada e acessar a integridade e a análise para fins de monitoramento e solução de problemas. [Saiba mais.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#hybrid-identity-administrator)

---

### <a name="network-administrator-built-in-role"></a>Função interna de administrador de rede

**Tipo:** novo recurso

**Categoria de serviço:** RBAC

**Funcionalidade do produto:** Controle de Acesso

Os usuários com essa função podem revisar as recomendações de arquitetura de perímetro de rede da Microsoft que se baseiam na telemetria de rede de seus locais de usuário. O desempenho de rede para o Office 365 depende da arquitetura de perímetro de rede do cliente de uma empresa cuidadosa, que geralmente é específica ao local do usuário. Essa função permite a edição de locais de usuários descobertos e a configuração de parâmetros de rede desses locais para facilitar medidas de telemetria e recomendações de design melhores. [Saiba mais.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#network-administrator)

---

### <a name="bulk-activity-and-downloads-in-the-azure-ad-admin-portal-experience"></a>Atividade em massa e downloads na experiência do portal de administração do Azure AD

**Tipo:** novo recurso

**Categoria de serviço:** Gerenciamento de usuários

**Funcionalidade do produto:** diretório

Agora você pode executar atividades em massa em usuários e grupos no Azure AD carregando um arquivo CSV na experiência do portal de administração do Azure AD. Você pode criar usuários, excluir usuários e convidar usuários convidados. E você pode adicionar e remover membros de um grupo.

Você também pode baixar listas de recursos do Azure AD da experiência do portal de administração do Azure AD. Você pode baixar a lista de usuários no diretório, a lista de grupos no diretório e os membros de um grupo específico.

Para obter mais informações, confira o seguinte:

- [Criar usuários](https://docs.microsoft.com/azure/active-directory/users-groups-roles/users-bulk-add) ou [convidar usuários convidados](https://docs.microsoft.com/azure/active-directory/b2b/tutorial-bulk-invite)
- [Excluir usuários](https://docs.microsoft.com/azure/active-directory/users-groups-roles/users-bulk-delete) ou [restaurar usuários excluídos](https://docs.microsoft.com/azure/active-directory/users-groups-roles/users-bulk-restore)
- [Baixar lista de usuários](https://docs.microsoft.com/azure/active-directory/users-groups-roles/users-bulk-download) ou [baixar lista de grupos](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download)
- [Adicionar (importar) Membros](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-import-members) ou [remover membros](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-remove-members) ou [baixar a lista de membros](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download-members) de um grupo

---

### <a name="my-staff-delegated-user-management"></a>Minha equipe delegou o gerenciamento de usuários

**Tipo:** novo recurso

**Categoria de serviço:** Gerenciamento de usuários

**Funcionalidade do produto:**

Minha equipe permite que os gerentes de primeira a, como um gerente de loja, verifiquem se os membros da equipe podem acessar suas contas do Azure AD. Em vez de depender de um helpdesk central, as organizações podem delegar tarefas comuns, como redefinição de senhas ou alteração de números de telefone, para um gerente de primeira linha. Com a minha equipe, um usuário que não consegue acessar sua conta pode obter acesso novamente em apenas alguns cliques, sem necessidade de assistência técnica ou equipe de ti. Para obter mais informações, consulte [gerenciar seus usuários com minha equipe (versão prévia)](https://aka.ms/MyStaffAdminDocs) e [delegar o gerenciamento de usuários com minha equipe (versão prévia)](https://aka.ms/MyStaffUserDocs).

---

### <a name="an-upgraded-end-user-experience-in-access-reviews"></a>Uma experiência do usuário final atualizada nas revisões de acesso

**Tipo:** recurso alterado

**Categoria de serviço:** Revisões de acesso

**Funcionalidade do produto:** Governança de identidade

Atualizamos a experiência do revisor para as revisões de acesso do Azure AD no portal meus aplicativos. No final de abril, os revisores que estiverem conectados à experiência de revisor de revisões de acesso do Azure AD verão uma faixa que permitirá que eles experimentem a experiência atualizada no meu acesso. Observe que a experiência de revisões de acesso atualizada oferece a mesma funcionalidade que a experiência atual, mas com uma interface de usuário aprimorada sobre novos recursos para permitir que os usuários sejam produtivos. [Você pode aprender mais sobre a experiência atualizada aqui](https://docs.microsoft.com/azure/active-directory/governance/perform-access-review). Essa visualização pública durará até o final de julho de 2020. No final de julho, os revisores que não tiverem optado pela experiência de visualização serão automaticamente direcionados para o meu acesso para realizar revisões de acesso. Se você quiser que os revisores alternem permanentemente para a experiência de visualização em meu acesso agora, [faça uma solicitação aqui](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR5dv-S62099HtxdeKIcgO-NUOFJaRDFDWUpHRk8zQ1BWVU1MMTcyQ1FFUi4u).

---

### <a name="workday-inbound-user-provisioning-and-writeback-apps-now-support-the-latest-versions-of-workday-web-services-api"></a>O provisionamento e os aplicativos de write-back do usuário de entrada do workday agora dão suporte às versões mais recentes da API de serviços Web do workday

**Tipo:** recurso alterado

**Categoria de serviço:** provisionamento de aplicativos

**Funcionalidade do produto:** 

Com base nos comentários dos clientes, atualizamos agora os aplicativos de write-back e de provisionamento do usuário de entrada do workday na Galeria de aplicativos empresariais para dar suporte às versões mais recentes da API do WWS (workday Web Services). Com essa alteração, os clientes podem especificar a versão da API do WWS que gostaria de usar na cadeia de conexão. Isso oferece aos clientes a capacidade de recuperar mais atributos de RH disponíveis nas versões do workday. O aplicativo de write-back do workday agora usa o serviço Web Change_Work_Contact_Info workday recomendado para superar as limitações do Maintain_Contact_Info.

Se nenhuma versão for especificada na cadeia de conexão, por padrão, os aplicativos de provisionamento de entrada do workday continuarão a usar o WWS v 21.1 para alternar para as APIs mais recentes do WORKDAY para o provisionamento de usuário de entrada, os clientes precisarão atualizar a cadeia de conexão conforme documentado [no tutorial](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#which-workday-apis-does-the-solution-use-to-query-and-update-workday-worker-profiles) e também atualizar os XPaths usados para os atributos workday, conforme documentado no [Guia de referência](https://docs.microsoft.com/azure/active-directory/app-provisioning/workday-attribute-reference#xpath-values-for-workday-web-services-wws-api-v30)de 

Para usar a nova API para Write-back, não há nenhuma alteração necessária no aplicativo de provisionamento de write-back do workday. No lado do workday, verifique se a conta de usuário do sistema de integração do workday (ISU) tem permissões para invocar o processo comercial Change_Work_Contact conforme documentado na seção tutorial, [configurar permissões de política de segurança de processo de negócios](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#configuring-business-process-security-policy-permissions). 

Atualizamos nosso [Guia de tutorial](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial) para refletir o novo suporte à versão de API.

---

### <a name="users-with-default-access-role-are-now-in-scope-for-provisioning"></a>Os usuários com função de acesso padrão agora estão no escopo para provisionamento

**Tipo:** recurso alterado

**Categoria de serviço:** provisionamento de aplicativos

**Funcionalidade do produto:** Gerenciamento do ciclo de vida de identidade

Historicamente, os usuários com a função de acesso padrão estão fora do escopo para provisionamento. Ouvimos comentários de que os clientes querem que os usuários com essa função estejam no escopo para provisionamento. A partir de 16 de abril de 2020, todas as novas configurações de provisionamento permitem que os usuários com a função de acesso padrão sejam provisionados. Gradativamente, alteraremos o comportamento das configurações de provisionamento existentes para dar suporte ao provisionamento de usuários com essa função. [Saiba mais.](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-config-problem-no-users-provisioned)

---

### <a name="updated-provisioning-ui"></a>Interface do usuário de provisionamento atualizada

**Tipo:** recurso alterado

**Categoria de serviço:** provisionamento de aplicativos

**Funcionalidade do produto:** Gerenciamento do ciclo de vida de identidade

Atualizamos nossa experiência de provisionamento para criar uma exibição de gerenciamento mais focalizada. Ao navegar até a folha de provisionamento de um aplicativo empresarial que já foi configurado, você poderá monitorar facilmente o progresso do provisionamento e do gerenciamento de ações, como iniciar, parar e reiniciar o provisionamento.... [Saiba mais.](https://docs.microsoft.com/azure/active-directory/app-provisioning/configure-automatic-user-provisioning-portal)

---

### <a name="dynamic-group-rule-validation-is-now-available-for-public-preview"></a>A validação de regra de grupo dinâmico agora está disponível para visualização pública

**Tipo:** recurso alterado

**Categoria de serviço:** gerenciamento de grupo

**Recurso de produto:** colaboração

O Azure Active Directory (Azure AD) agora fornece os meios para validar regras de grupo dinâmicos. Na guia **validar regras** , você pode validar sua regra dinâmica em relação aos membros do grupo de exemplo para confirmar se a regra está funcionando conforme o esperado. Ao criar ou atualizar regras de grupo dinâmicos, os administradores desejam saber se um usuário ou um dispositivo será membro do grupo. Isso ajuda a avaliar se um usuário ou dispositivo atende aos critérios de regra e auxilia na solução de problemas quando a associação não é esperada.

Para obter mais informações, consulte [validar uma regra de associação de grupo dinâmico (versão prévia)](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-rule-validation).

---

### <a name="identity-secure-score---security-defaults-and-mfa-improvement-action-updates"></a>Pontuação segura de identidade-padrões de segurança e atualizações de ação de melhoria de MFA

**Tipo:** recurso alterado

**Categoria de serviço:** N/A

**Funcionalidade do produto:** segurança e proteção da identidade

**Suporte a padrões de segurança para ações de aperfeiçoamento do Azure AD:** A pontuação segura da Microsoft atualizará as ações de aperfeiçoamento para dar suporte aos [padrões de segurança no Azure ad](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults), o que facilita ajudar a proteger sua organização com configurações de segurança pré-configuradas para ataques comuns. Isso afetará as seguintes ações de aprimoramento:

- Verifique se todos os usuários podem concluir a autenticação multifator para acesso seguro
- Exigir MFA para funções administrativas
- Habilitar política para bloquear autenticação herdada
 
**Atualizações de ação de melhoria de MFA:** Para refletir a necessidade de que as empresas assegurem a maior parte da segurança ao aplicar políticas que funcionam com seus negócios, a pontuação segura da Microsoft removeu três ações de melhoria centralizadas em relação à autenticação multifator e adicionadas duas.

Ações de aperfeiçoamento removidas:

- Registrar todos os usuários para autenticação multifator
- Exigir MFA para todos os usuários
- Exigir MFA para funções privilegiadas do Azure AD

Ações de aprimoramento adicionadas:

- Verifique se todos os usuários podem concluir a autenticação multifator para acesso seguro
- Exigir MFA para funções administrativas

Essas novas ações de aprimoramento exigem o registro de seus usuários ou administradores para a MFA (autenticação multifator) em seu diretório e o estabelecimento do conjunto certo de políticas que atenda às suas necessidades organizacionais. O principal objetivo é ter flexibilidade ao garantir que todos os usuários e administradores possam autenticar com vários fatores ou prompts de verificação de identidade com base em risco. Isso pode assumir a forma de ter várias políticas que aplicam decisões com escopo definido ou definir padrões de segurança (a partir de 16 de março) que permitem que a Microsoft decida quando desafiar os usuários para MFA. [Leia mais sobre o que há de novo na pontuação segura da Microsoft](https://docs.microsoft.com/microsoft-365/security/mtp/microsoft-secure-score?view=o365-worldwide#whats-new).

---

## <a name="march-2020"></a>Março de 2020

### <a name="unmanaged-azure-active-directory-accounts-in-b2b-update-for-march-2021"></a>Contas de Azure Active Directory não gerenciadas na atualização B2B para março de 2021

**Tipo:** plano de alteração  
**Categoria de serviço:** B2B  
**Funcionalidade do produto:** B2B/B2C
 
A **partir de 31 de março de 2021**, a Microsoft não dará mais suporte ao resgate de convites criando contas e locatários não gerenciados do Azure Active Directory (AD do Azure) para cenários de colaboração B2B. Na preparação para isso, incentivamos você a aceitar o [email de autenticação de senha de uso único](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode).

---

### <a name="users-with-the-default-access-role-will-be-in-scope-for-provisioning"></a>Os usuários com a função de acesso padrão estarão no escopo para provisionamento

**Tipo:** plano de alteração  
**Categoria de serviço:** provisionamento de aplicativos  
**Funcionalidade do produto:** Gerenciamento do ciclo de vida de identidade
 
Historicamente, os usuários com a função de acesso padrão estão fora do escopo para provisionamento. Ouvimos comentários de que os clientes querem que os usuários com essa função estejam no escopo para provisionamento. Estamos trabalhando na implantação de uma alteração para que todas as novas configurações de provisionamento permitam que os usuários com a função de acesso padrão sejam provisionados. Gradualmente, alteraremos o comportamento das configurações de provisionamento existentes para dar suporte ao provisionamento de usuários com essa função. Nenhuma ação do cliente é necessária. Publicaremos uma atualização em nossa [documentação](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-config-problem-no-users-provisioned) quando essa alteração estiver em vigor.

---

### <a name="azure-ad-b2b-collaboration-will-be-available-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet-tenants"></a>A colaboração B2B do Azure AD estará disponível no Microsoft Azure operado por locatários da 21Vianet (Azure China 21Vianet)

**Tipo:** plano de alteração  
**Categoria de serviço:** B2B  
**Funcionalidade do produto:** B2B/B2C
 
Os recursos de colaboração B2B do Azure AD serão disponibilizados no Microsoft Azure operado por locatários da 21Vianet (Azure China 21Vianet), permitindo que os usuários em um locatário do Azure China 21Vianet colaborem de forma integrada com os usuários em outros locatários da 21Vianet da China do Azure. [Saiba mais sobre a colaboração B2B do Azure ad](https://docs.microsoft.com/azure/active-directory/b2b/).

---
 
### <a name="azure-ad-b2b-collaboration-invitation-email-redesign"></a>Redesignação de email de convite de colaboração B2B do Azure AD

**Tipo:** plano de alteração  
**Categoria de serviço:** B2B  
**Funcionalidade do produto:** B2B/B2C
 
Os [emails](https://docs.microsoft.com/azure/active-directory/b2b/invitation-email-elements) enviados pelo serviço de convite de colaboração B2B do Azure ad para convidar usuários para o diretório serão reprojetados para tornar mais claras as informações de convite e as próximas etapas do usuário.

---

### <a name="homerealmdiscovery-policy-changes-will-appear-in-the-audit-logs"></a>As alterações de política de HomeRealmDiscovery aparecerão nos logs de auditoria

**Tipo:** corrigido  
**Categoria de serviço:** Submeti  
**Funcionalidade do produto:** monitoramento e relatórios
 
Corrigimos um bug em que as alterações na [política HomeRealmDiscovery](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-authentication-for-federated-users-portal) não foram incluídas nos logs de auditoria. Agora você poderá ver quando e como a política foi alterada e por quem. 

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---march-2020"></a>Novos aplicativos federados disponíveis na Galeria de Aplicativo Azure AD – março de 2020

**Tipo:** novo recurso  
**Categoria de serviço:** Aplicativos empresariais  
**Funcionalidade do produto:** integração de terceiros
 
Em março de 2020, adicionamos esses 51 novos aplicativos com suporte de Federação à galeria de aplicativos: 

[Cisco AnyConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-anyconnect), [Zoho One China](https://docs.microsoft.com/azure/active-directory/saas-apps/zoho-one-china-tutorial), [PlusPlus](https://test.plusplus.app/auth/login/azuread-outlook/), [profit.co aplicativo SAML](https://docs.microsoft.com/azure/active-directory/saas-apps/profitco-saml-app-tutorial), [provedor de serviços iPoint](https://docs.microsoft.com/azure/active-directory/saas-apps/ipoint-service-provider-tutorial), [esfera Contexxt.ai](https://contexxt-sphere.com/login), [sabedoria por Invictus](https://docs.microsoft.com/azure/active-directory/saas-apps/wisdom-by-invictus-tutorial), inscrições digitais do [FLARE](https://spark-dev.pixelnebula.com/login), [LOGZ.Io-observação de capacidade de nuvem para engenheiros](https://docs.microsoft.com/azure/active-directory/saas-apps/logzio-cloud-observability-for-engineers-tutorial), [ESPECTROu](https://docs.microsoft.com/azure/active-directory/saas-apps/spectrumu-tutorial), [BizzContact](https://bizzcontact.app/), [Elqano SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/elqano-sso-tutorial), [MarketSignShare](http://www.signshare.com/), [CrossKnowledge Learning Suite](https://docs.microsoft.com/azure/active-directory/saas-apps/crossknowledge-learning-suite-tutorial), [Netvision compr](https://docs.microsoft.com/azure/active-directory/saas-apps/netvision-compas-tutorial), Hub de [FCM](https://docs.microsoft.com/azure/active-directory/saas-apps/fcm-hub-tutorial) [, RIB](https://www.devfinition.com/) [A/S Byggeweb Mobile,](https://apps.apple.com/us/app/docia/id529058757) [GoLinks](https://demo.asterapp.io/login) [,](https://docs.microsoft.com/azure/active-directory/saas-apps/golinks-tutorial)Datadog [,](https://docs.microsoft.com/azure/active-directory/saas-apps/datadog-tutorial)portal de usuário [B2B,](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-b2b-user-portal-tutorial)Planview Enterprise One [Skills Workflow](https://docs.microsoft.com/azure/active-directory/saas-apps/skills-workflow-tutorial) [plataforma IP](https://docs.microsoft.com/azure/active-directory/saas-apps/ip-platform-tutorial), [Node Insight](https://admin.nodeinsight.com/AADLogin.aspx) [Invision](https://docs.microsoft.com/azure/active-directory/saas-apps/invision-tutorial), [Pipedrive](https://docs.microsoft.com/azure/active-directory/saas-apps/pipedrive-tutorial), [Workshop de apresentação](https://app.showcaseworkshop.com/), [plataforma de integração Greenlight](https://docs.microsoft.com/azure/active-directory/saas-apps/greenlight-integration-platform-tutorial), [Gerenciamento de acesso compatível com Greenlight](https://docs.microsoft.com/azure/active-directory/saas-apps/greenlight-compliant-access-management-tutorial), [compreendo Learning](https://docs.microsoft.com/azure/active-directory/saas-apps/grok-learning-tutorial), [Miradore online](https://login.online.miradore.com/), Khoros [Care](https://docs.microsoft.com/azure/active-directory/saas-apps/khoros-care-tutorial), [AskYourTeam](https://docs.microsoft.com/azure/active-directory/saas-apps/askyourteam-tutorial), [TruNarrative](https://docs.microsoft.com/azure/active-directory/saas-apps/trunarrative-tutorial), [Smartwaiver](https://www.smartwaiver.com/m/user/sw_login.php?wms_login), [BizAgi Studio para automação de processo digital](https://docs.microsoft.com/azure/active-directory/saas-apps/bizagi-studio-for-digital-process-automation-tutorial), [insuiteX](https://www.insuite.jp/), [sybo](https://www.systexsoftware.com.tw/), [Britive](https://docs.microsoft.com/azure/active-directory/saas-apps/britive-tutorial), [WhosOffice](https://docs.microsoft.com/azure/active-directory/saas-apps/whosoffice-tutorial), [E-Days](https://docs.microsoft.com/azure/active-directory/saas-apps/e-days-tutorial), [Kollective Sdn](https://portal.kollective.app/login), [Witivio](https://app.witivio.com/), [Playvox](https://my.playvox.com/login), [Korn Ferry 360](https://docs.microsoft.com/azure/active-directory/saas-apps/korn-ferry-360-tutorial), [campus café](https://docs.microsoft.com/azure/active-directory/saas-apps/campus-cafe-tutorial), [captura](https://docs.microsoft.com/azure/active-directory/saas-apps/catchpoint-tutorial), [Code42](https://docs.microsoft.com/azure/active-directory/saas-apps/code42-tutorial) [LIFT](https://docs.microsoft.com/azure/active-directory/saas-apps/lift-tutorial) [Planview Enterprise One](https://docs.microsoft.com/azure/active-directory/saas-apps/planview-enterprise-one-tutorial)

Para obter mais informações sobre os aplicativos, consulte [integração de aplicativos SaaS com o Active Directory do Azure](https://aka.ms/appstutorial). Para obter mais informações sobre como listar seu aplicativo na galeria de aplicativos do Azure AD, consulte [Listar seu aplicativo na galeria de aplicativos do Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="azure-ad-b2b-collaboration-available-in-azure-government-tenants"></a>Colaboração B2B do Azure AD disponível nos locatários do Azure governamental

**Tipo:** novo recurso  
**Categoria de serviço:** B2B  
**Funcionalidade do produto:** B2B/B2C
 
Os recursos de colaboração B2B do Azure AD agora estão disponíveis entre alguns locatários do Azure governamental.  Para descobrir se o seu locatário é capaz de usar esses recursos, siga as instruções em [como posso saber se a colaboração B2B está disponível no meu locatário do Azure no governo dos EUA?](https://docs.microsoft.com/azure/active-directory/b2b/current-limitations#how-can-i-tell-if-b2b-collaboration-is-available-in-my-azure-us-government-tenant).

---

### <a name="azure-monitor-integration-for-azure-logs-is-now-available-in-azure-government"></a>A integração do Azure Monitor para logs do Azure agora está disponível no Azure governamental

**Tipo:** novo recurso  
**Categoria de serviço:** relatórios  
**Funcionalidade do produto:** monitoramento e relatórios
 
A integração do Azure Monitor com os logs do Azure AD agora está disponível no Azure governamental. Você pode rotear logs do Azure AD (logs de auditoria e de logon) para uma conta de armazenamento, Hub de eventos e Log Analytics. Confira a [documentação detalhada](https://aka.ms/aadlogsinamd) , bem como os [planos de implantação para relatórios e monitoramento](https://docs.microsoft.com/azure/active-directory/reports-monitoring/plan-monitoring-and-reporting) para cenários do Azure AD.

---

### <a name="identity-protection-refresh-in-azure-government"></a>Atualização da proteção de identidade no Azure governamental

**Tipo:** novo recurso  
**Categoria de serviço:** Proteção de identidade  
**Funcionalidade do produto:** segurança e proteção da identidade

Estamos empolgados em compartilhar que agora distribuímos a experiência de [Azure ad Identity Protection](https://aka.ms/IdentityProtectionDocs)atualizada   no [portal de Microsoft Azure governamental](https://portal.azure.us/). Para obter mais informações, consulte nossa [postagem no blog de anúncios](https://techcommunity.microsoft.com/t5/public-sector-blog/identity-protection-refresh-in-microsoft-azure-government/ba-p/1223667).

---

### <a name="disaster-recovery-download-and-store-your-provisioning-configuration"></a>Recuperação de desastre: Baixe e armazene sua configuração de provisionamento

**Tipo:** novo recurso  
**Categoria de serviço:** provisionamento de aplicativos  
**Funcionalidade do produto:** Gerenciamento do ciclo de vida de identidade
 
O serviço de provisionamento do Azure AD fornece um rico conjunto de recursos de configuração. Os clientes precisam ser capazes de salvar suas configurações para que possam consultá-las mais tarde ou reverta para uma versão válida conhecida. Adicionamos a capacidade de baixar sua configuração de provisionamento como um arquivo JSON e carregá-la quando necessário. [Saiba mais](https://docs.microsoft.com/azure/active-directory/app-provisioning/export-import-provisioning-configuration).

---
 
### <a name="sspr-self-service-password-reset-now-requires-two-gates-for-admins-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>SSPR (autoatendimento de redefinição de senha) agora requer duas Gates para administradores no Microsoft Azure operado pela 21Vianet (Azure China 21Vianet) 

**Tipo:** recurso alterado  
**Categoria de serviço:** Redefinição de senha de autoatendimento  
**Funcionalidade do produto:** segurança e proteção da identidade
 
Anteriormente no Microsoft Azure operado pela 21Vianet (Azure China 21Vianet), os administradores usando a redefinição de senha de autoatendimento (SSPR) para redefinir suas próprias senhas precisavam apenas de um "portão" (desafio) para provar sua identidade. No público e em outras nuvens nacionais, os administradores geralmente devem usar duas Gates para provar sua identidade ao usar o SSPR. Mas como não damos suporte a chamadas SMS ou telefônicas na 21Vianet da China do Azure, permitimos a redefinição de senha de uma porta por administradores.

Estamos criando a paridade do recurso SSPR entre o Azure China 21Vianet e a nuvem pública. No futuro, os administradores devem usar dois Gates ao usar o SSPR. O SMS, chamadas telefônicas e códigos e notificações do aplicativo autenticador serão suportados. [Saiba mais](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-policy#administrator-reset-policy-differences).

---

### <a name="password-length-is-limited-to-256-characters"></a>O comprimento da senha é limitado a 256 caracteres

**Tipo:** recurso alterado  
**Categoria de serviço:** autenticações (logons)  
**Funcionalidade do produto:** Autenticação do usuário
 
Para garantir a confiabilidade do serviço do Azure AD, as senhas de usuário agora são limitadas de comprimento a 256 caracteres. Os usuários com senhas maiores que isso serão solicitados a alterar sua senha no logon subsequente, seja entrando em contato com o administrador ou usando o recurso de redefinição de senha de autoatendimento.

Essa alteração foi habilitada em 13 de março de 2020, em 10h PST (18:00 UTC) e o erro é AADSTS 50052, InvalidPasswordExceedsMaxLength. Consulte o [aviso de alteração significativa](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#user-passwords-will-be-restricted-to-256-characters) para obter mais detalhes.

---

### <a name="azure-ad-sign-in-logs-are-now-available-for-all-free-tenants-through-the-azure-portal"></a>Os logs de entrada do Azure AD agora estão disponíveis para todos os locatários gratuitos por meio do portal do Azure

**Tipo:** recurso alterado  
**Categoria de serviço:** relatórios  
**Funcionalidade do produto:** monitoramento e relatórios
 
A partir de agora, os clientes que têm locatários gratuitos podem acessar os [logs de entrada do Azure ad da portal do Azure](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins) por até 7 dias. Anteriormente, os logs de entrada estavam disponíveis apenas para clientes com licenças Azure Active Directory Premium. Com essa alteração, todos os locatários podem acessar esses logs por meio do Portal.

> [!NOTE]
> Os clientes ainda precisam de uma licença Premium (Azure Active Directory Premium P1 ou P2) para acessar os logs de entrada por meio da API Microsoft Graph e Azure Monitor.

---

### <a name="deprecation-of-directory-wide-groups-option-from-groups-general-settings-on-azure-portal"></a>Substituição da opção de grupos em todo o diretório por meio de grupos configurações gerais no portal do Azure

**Tipo:** preterido  
**Categoria de serviço:** gerenciamento de grupo  
**Recurso de produto:** colaboração

Para fornecer uma maneira mais flexível para os clientes criarem grupos de todo o diretório que melhor atendam às suas necessidades, substituímos a opção **grupos de todo o diretório** das configurações gerais dos **grupos**  >  **General** na portal do Azure com um link para a [documentação do grupo dinâmico](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership). Aperfeiçoamos nossa documentação para incluir mais instruções para que os administradores possam criar grupos de todos os usuários que incluem ou excluem usuários convidados.

---

## <a name="february-2020"></a>Fevereiro de 2020

### <a name="upcoming-changes-to-custom-controls"></a>Alterações futuras em controles personalizados

**Tipo:** plano de alteração  
**Categoria de serviço:** FATO  
**Funcionalidade do produto:** segurança e proteção da identidade
 
Estamos planejando substituir a visualização dos controles personalizados atuais por uma abordagem que permite que os recursos de autenticação fornecidos pelo parceiro funcionem diretamente com as experiências de administrador Azure Active Directory e usuário final. Hoje, as soluções de MFA do parceiro enfrentam as seguintes limitações: elas só funcionam depois que uma senha é inserida; Eles não servem como MFA para autenticação de Step-up em outros cenários principais; e eles não se integram com funções de usuário final ou de gerenciamento de credenciais administrativas. A nova implementação permitirá que fatores de autenticação fornecidos por parceiros funcionem junto com fatores internos para cenários-chave, incluindo registro, uso, declarações de MFA, autenticação de depuração, relatórios e registro em log. 

Os controles personalizados continuarão a ter suporte na visualização junto com o novo design até atingirem a disponibilidade geral. Nesse ponto, daremos tempo para que os clientes migrem para o novo design. Devido às limitações da abordagem atual, não integraremos novos provedores até que o novo design esteja disponível. Estamos trabalhando junto com clientes e provedores e comunicaremos a linha do tempo à medida que ficarmos mais próximos. [Saiba mais](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/upcoming-changes-to-custom-controls/ba-p/1144696#).

---

### <a name="identity-secure-score---mfa-improvement-action-updates"></a>Pontuação segura de identidade-atualizações de ação de melhoria de MFA

**Tipo:** plano de alteração  
**Categoria de serviço:** FATO  
**Funcionalidade do produto:** segurança e proteção da identidade
 
Para refletir a necessidade de que as empresas assegurem a maior parte da segurança ao aplicar políticas que funcionam com seus negócios, a pontuação segura da Microsoft está removendo três ações de aprimoramento centralizadas em relação à MFA (autenticação multifator) e à adição de duas.

As seguintes ações de aprimoramento serão removidas:

- Registrar todos os usuários para MFA
- Exigir MFA para todos os usuários
- Exigir MFA para funções privilegiadas do Azure AD

As seguintes ações de aprimoramento serão adicionadas:

- Garantir que todos os usuários possam concluir a MFA para acesso seguro
- Exigir MFA para funções administrativas

Essas novas ações de aprimoramento exigirão o registro de seus usuários ou administradores para MFA em seu diretório e o estabelecimento do conjunto certo de políticas que atendam às suas necessidades organizacionais. O principal objetivo é ter flexibilidade ao garantir que todos os usuários e administradores possam autenticar com vários fatores ou prompts de verificação de identidade com base em risco. Isso pode assumir a forma de definir os padrões de segurança que permitem que a Microsoft decida quando desafiar os usuários para MFA ou ter várias políticas que apliquem decisões com escopo. Como parte dessas atualizações de ação de aperfeiçoamento, as políticas de proteção de linha de base não serão mais incluídas nos cálculos de pontuação. [Leia mais sobre o que está chegando na pontuação segura da Microsoft](https://docs.microsoft.com/microsoft-365/security/mtp/microsoft-secure-score-whats-coming?view=o365-worldwide).

---

### <a name="azure-ad-domain-services-sku-selection"></a>Seleção Azure AD Domain Services SKU

**Tipo:** novo recurso  
**Categoria de serviço:** Serviços de Domínio do Azure AD  
** Capacidade do produto: ** Serviços de Domínio do Azure AD
 
Ouvimos comentários que Azure AD Domain Services os clientes desejam mais flexibilidade na seleção de níveis de desempenho para suas instâncias. A partir de 1º de fevereiro de 2020, mudamos de um modelo dinâmico (no qual o Azure AD determina o desempenho e o tipo de preço com base na contagem de objetos) para um modelo de seleção automática. Agora, os clientes podem escolher um nível de desempenho que corresponda a seu ambiente. Essa alteração também nos permite habilitar novos cenários como florestas de recursos e recursos premium como backups diários. A contagem de objetos agora é ilimitada para todas as SKUs, mas continuaremos a oferecer sugestões de contagem de objetos para cada camada.

**Nenhuma ação imediata do cliente é necessária.** Para clientes existentes, a camada dinâmica que estava em uso em 1º de fevereiro de 2020 determina a nova camada padrão. Não há nenhum impacto de preço ou desempenho como resultado dessa alteração. No futuro, os clientes do Azure AD DS precisarão avaliar os requisitos de desempenho à medida que o tamanho do diretório e as características da carga de trabalho forem alterados. A alternância entre as camadas de serviço continuará a ser uma operação sem tempo de inatividade, e não mais moveremos os clientes automaticamente para novas camadas com base no crescimento de seu diretório. Além disso, não haverá aumento de preço e novos preços serão alinhados com nosso modelo de cobrança atual. Para obter mais informações, consulte a [documentação dos SKUs do Azure AD DS](https://docs.microsoft.com/azure/active-directory-domain-services/administration-concepts#azure-ad-ds-skus) e a [página de preços do Azure AD Domain Services](https://azure.microsoft.com/pricing/details/active-directory-ds/).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery---february-2020"></a>Novos aplicativos federados disponíveis na Galeria de Aplicativo Azure AD – fevereiro de 2020

**Tipo:** novo recurso  
**Categoria de serviço:** Aplicativos empresariais  
**Funcionalidade do produto:** integração de terceiros
 
Em fevereiro de 2020, adicionamos esses 31 novos aplicativos com suporte à Federação para a Galeria de aplicativos: 

[Plataforma de patente IamIP](https://docs.microsoft.com/azure/active-directory/saas-apps/iamip-patent-platform-tutorial), [experiência em nuvem](https://docs.microsoft.com/azure/active-directory/saas-apps/experience-cloud-tutorial), [SSO ns1 para Azure](https://docs.microsoft.com/azure/active-directory/saas-apps/ns1-sso-azure-tutorial), [serviço de segurança de email Barracuda](https://ess.barracudanetworks.com/sso/azure), [relatório aba](https://myaba.co.uk/client-access/signin/auth/msad), [no caso de crise – portal online](https://docs.microsoft.com/azure/active-directory/saas-apps/in-case-of-crisis-online-portal-tutorial), [design de nuvem de BIC](https://docs.microsoft.com/azure/active-directory/saas-apps/bic-cloud-design-tutorial), conector de [dados do Azure ad](https://docs.microsoft.com/azure/active-directory/saas-apps/beekeeper-azure-ad-data-connector-tutorial), [avaliações de ferry Korn](https://www.kornferry.com/solutions/kf-digital/kf-assess), [comando Verkada](https://docs.microsoft.com/azure/active-directory/saas-apps/verkada-command-tutorial), [Splashtop](https://docs.microsoft.com/azure/active-directory/saas-apps/splashtop-tutorial), [Syxsense](https://docs.microsoft.com/azure/active-directory/saas-apps/syxsense-tutorial), [EAB navegar](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-tutorial), [novo Relic (versão limitada)](https://docs.microsoft.com/azure/active-directory/saas-apps/new-relic-limited-release-tutorial), [Thulium](https://admin.thulium.com/login/instance), [Gerenciador de tíquetes](https://docs.microsoft.com/azure/active-directory/saas-apps/ticketmanager-tutorial), [seletor de modelo para equipes](https://links.officeatwork.com/templatechooser-download-teams), [abelhas](https://www.beesy.me/index.php/site/login), sistema de [suporte de integridade](https://docs.microsoft.com/azure/active-directory/saas-apps/health-support-system-tutorial), [mural](https://app.mural.co/signup), [Hive](https://docs.microsoft.com/azure/active-directory/saas-apps/hive-tutorial), [LavaDo](https://appsource.microsoft.com/product/web-apps/lavaloon.lavado_standard?tab=Overview), [Wakelet](https://wakelet.com/login), [Firmex VDR](https://docs.microsoft.com/azure/active-directory/saas-apps/firmex-vdr-tutorial), [ThingLink para professores e escolas](https://www.thinglink.com/), [Coda](https://docs.microsoft.com/azure/active-directory/saas-apps/coda-tutorial), [NearpodApp](https://nearpod.com/signup/?oc=Microsoft&utm_campaign=Microsoft&utm_medium=site&utm_source=product), [WEDO](https://docs.microsoft.com/azure/active-directory/saas-apps/wedo-tutorial), [InvitePeople](https://invitepeople.com/login), [reimpressões de mesa-artigo Galaxy](https://docs.microsoft.com/azure/active-directory/saas-apps/reprints-desk-article-galaxy-tutorial), [TeamViewer](https://docs.microsoft.com/azure/active-directory/saas-apps/teamviewer-tutorial)

 
Para obter mais informações sobre os aplicativos, consulte [integração de aplicativos SaaS com o Active Directory do Azure](https://aka.ms/appstutorial). Para obter mais informações sobre como listar seu aplicativo na galeria de aplicativos do Azure AD, consulte [Listar seu aplicativo na galeria de aplicativos do Azure Active Directory](https://aka.ms/azureadapprequest).

---
 
### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---february-2020"></a>Novos conectores de provisionamento na Galeria de aplicativos do Azure AD – fevereiro de 2020

**Tipo:** novo recurso  
**Categoria de serviço:** Aplicativos empresariais  
**Funcionalidade do produto:** integração de terceiros
 
Agora você pode automatizar a criação, a atualização e a exclusão de contas de usuário para esses aplicativos integrados recentemente:

- [Mixpanel](https://docs.microsoft.com/azure/active-directory/saas-apps/mixpanel-provisioning-tutorial)
- [TeamViewer](https://docs.microsoft.com/azure/active-directory/saas-apps/teamviewer-provisioning-tutorial)
- [Azure Databricks](https://docs.microsoft.com/azure/active-directory/saas-apps/azure-databricks-scim-connector-provisioning-tutorial)
- [PureCloud by Genesys](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-provisioning-tutorial)
- [Zapier](https://docs.microsoft.com/azure/active-directory/saas-apps/zapier-provisioning-tutorial)

Para obter mais informações para proteger melhor sua organização com o provisionamento automatizado de contas de usuário, consulte [Automatizar o provisionamento de usuário para aplicativos SaaS com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---
 
### <a name="azure-ad-support-for-fido2-security-keys-in-hybrid-environments"></a>Suporte do Azure AD para chaves de segurança FIDO2 em ambientes híbridos

**Tipo:** novo recurso  
**Categoria de serviço:** autenticações (logons)  
**Funcionalidade do produto:** Autenticação do usuário
 
Estamos anunciando a visualização pública do suporte do Azure AD para chaves de segurança FIDO2 em ambientes híbridos. Agora, os usuários podem usar as chaves de segurança FIDO2 para entrar em seus dispositivos Windows 10 híbridos ingressados no Azure AD e obter logon contínuo em seus recursos locais e na nuvem. O suporte para ambientes híbridos tem sido o principal recurso mais solicitado de nossos clientes sem senha, pois inicialmente lançamos a visualização pública para suporte FIDO2 em dispositivos ingressados no Azure AD. A autenticação sem senha usando tecnologias avançadas, como a biometria e a criptografia de chave pública/privada, proporcionam conveniência e facilidade de uso enquanto são seguras. Com essa visualização pública, agora você pode usar autenticação moderna, como chaves de segurança FIDO2, para acessar recursos tradicionais de Active Directory. Para obter mais informações, acesse [SSO para recursos locais](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-security-key-on-premises). 

Para começar, visite [habilitar chaves de segurança FIDO2 para seu locatário](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-security-key) para obter instruções passo a passo. 

---
 
### <a name="the-new-my-account-experience-is-now-generally-available"></a>A nova experiência de minha conta agora está disponível para o público geral

**Tipo:** recurso alterado  
**Categoria de serviço:** Meu perfil/conta  
**Funcionalidade do produto:** Experiências do usuário final
 
Minha conta, a única loja para todas as necessidades de gerenciamento de conta de usuário final, já está disponível para o público geral! Os usuários finais podem acessar esse novo site via URL ou no cabeçalho da nova experiência meus aplicativos. Saiba mais sobre todos os recursos de autoatendimento que a nova experiência oferece em [minha visão geral do portal de contas](https://docs.microsoft.com/azure/active-directory/user-help/my-account-portal-overview).

---
 
### <a name="my-account-site-url-updating-to-myaccountmicrosoftcom"></a>URL do site da minha conta Atualizando para o myaccount.microsoft.com

**Tipo:** recurso alterado  
**Categoria de serviço:** Meu perfil/conta  
**Funcionalidade do produto:** Experiências do usuário final
 
A nova experiência do usuário final da minha conta atualizará sua URL para `https://myaccount.microsoft.com` no próximo mês. Encontre mais informações sobre a experiência e todos os recursos de autoatendimento da conta que ele oferece aos usuários finais na [ajuda do portal da minha conta](https://docs.microsoft.com/azure/active-directory/user-help/my-account-portal-overview).

---
 
## <a name="january-2020"></a>Janeiro de 2020
 
### <a name="the-new-my-apps-portal-is-now-generally-available"></a>O novo portal meus aplicativos agora está disponível para o público geral

**Tipo:** plano de alteração  
**Categoria de serviço:** Meus Aplicativos  
**Funcionalidade do produto:** Experiências do usuário final
 
Atualize sua organização para o novo portal meus aplicativos que agora está disponível para o público geral! Encontre mais informações sobre o novo portal e coleções em [criar coleções no portal meus aplicativos](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-collections).

---
 
### <a name="workspaces-in-azure-ad-have-been-renamed-to-collections"></a>Os espaços de trabalho no Azure AD foram renomeados para coleções

**Tipo:** recurso alterado  
**Categoria de serviço:** Meus Aplicativos   
**Funcionalidade do produto:** Experiências do usuário final
 
Nos espaços de trabalho, os administradores de filtros podem configurar o para organizar os aplicativos dos usuários, agora serão chamados de coleções. Encontre mais informações sobre como configurá-los em [criar coleções no portal meus aplicativos](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-collections).

---
 
### <a name="azure-ad-b2c-phone-sign-up-and-sign-in-using-custom-policy-public-preview"></a>Azure AD B2C inscrição e entrada no telefone usando a política personalizada (visualização pública)

**Tipo:** novo recurso  
**Categoria de serviço:** B2C - gerenciamento de identidades de consumidor  
**Funcionalidade do produto:** B2B/B2C
 
Com a inscrição e a entrada do número de telefone, os desenvolvedores e as empresas podem permitir que seus clientes se inscrevam e entrem usando uma senha de uso único enviada ao número de telefone do usuário via SMS. Esse recurso também permite que o cliente altere seu número de telefone se eles perderem o acesso ao seu telefone. Com o poder das políticas personalizadas, a inscrição e a entrada do telefone permitem que os desenvolvedores e as empresas comuniquem sua marca por meio da personalização da página. Descubra como configurar a [inscrição e a entrada do telefone com políticas personalizadas no Azure ad B2C](https://docs.microsoft.com/azure/active-directory-b2c/phone-authentication).
 
---
 
### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---january-2020"></a>Novos conectores de provisionamento na Galeria de aplicativos do Azure AD – janeiro de 2020

**Tipo:** novo recurso  
**Categoria de serviço:** Aplicativos empresariais  
**Funcionalidade do produto:** integração de terceiros
 
Agora você pode automatizar a criação, a atualização e a exclusão de contas de usuário para esses aplicativos integrados recentemente:

- [Promapp]( https://docs.microsoft.com/azure/active-directory/saas-apps/promapp-provisioning-tutorial)
- [Zscaler Private Access](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-private-access-provisioning-tutorial)

Para obter mais informações para proteger melhor sua organização com o provisionamento automatizado de contas de usuário, consulte [Automatizar o provisionamento de usuário para aplicativos SaaS com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2020"></a>Novos aplicativos federados disponíveis na Galeria de Aplicativo Azure AD – janeiro de 2020

**Tipo:** novo recurso  
**Categoria de serviço:** Aplicativos empresariais  
**Funcionalidade do produto:** integração de terceiros
 
Em janeiro de 2020, adicionamos esses 33 novos aplicativos com suporte de Federação à galeria de aplicativos: 

[Josa](https://docs.microsoft.com/azure/active-directory/saas-apps/josa-tutorial), [nuvem de ponta rápida](https://docs.microsoft.com/azure/active-directory/saas-apps/fastly-edge-cloud-tutorial), [Terraform Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/terraform-enterprise-tutorial), [Spintr SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/spintr-sso-tutorial), [Abibot Netlogistik](https://azuremarketplace.microsoft.com/marketplace/apps/aad.abibotnetlogistik), [SkyKick](https://login.skykick.com/login?state=g6Fo2SBTd3M5Q0xBT0JMd3luS2JUTGlYN3pYTE1remJQZnR1c6N0aWTZIDhCSkwzYVQxX2ZMZjNUaWxNUHhCSXg2OHJzbllTcmYto2NpZNkgM0h6czk3ZlF6aFNJV1VNVWQzMmpHeFFDbDRIMkx5VEc&client=3Hzs97fQzhSIWUMUd32jGxQCl4H2LyTG&protocol=oauth2&audience=https://papi.skykick.com&response_type=code&redirect_uri=https://portal.skykick.com/callback&scope=openid%20profile%20offline_access), [upshoty](https://docs.microsoft.com/azure/active-directory/saas-apps/upshotly-tutorial), [LeaveBot](https://leavebot.io/#home), [Datacamp](https://docs.microsoft.com/azure/active-directory/saas-apps/datacamp-tutorial), [TripActions](https://docs.microsoft.com/azure/active-directory/saas-apps/tripactions-tutorial), [inteligente](https://www.intumit.com/english/SmartWork.html), [Dotcom-monitor](https://docs.microsoft.com/azure/active-directory/saas-apps/dotcom-monitor-tutorial), [SSOGEN-gateway de SSO do Azure ad para Oracle E-Business Suite-EBS, PeopleSoft e JDE](https://docs.microsoft.com/azure/active-directory/saas-apps/ssogen-tutorial), [hospedagem MYCIRQA SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/hosted-mycirqa-sso-tutorial), [Yuhu plataforma de gerenciamento de propriedade](https://docs.microsoft.com/azure/active-directory/saas-apps/yuhu-property-management-platform-tutorial), [LumApps](https://sites.lumapps.com/login), [trabalho corporativo](https://docs.microsoft.com/azure/active-directory/saas-apps/upwork-enterprise-tutorial), [Talentsoft](https://docs.microsoft.com/azure/active-directory/saas-apps/talentsoft-tutorial), [SmartDB para Microsoft Teams](http://teams.smartdb.jp/login/), [PressPage](https://docs.microsoft.com/azure/active-directory/saas-apps/presspage-tutorial), [ContractSafe Saml2 SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/contractsafe-saml2-sso-tutorial), [Maxient conduzindo o software Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/maxient-conduct-manager-software-tutorial), [Helpshift](https://docs.microsoft.com/azure/active-directory/saas-apps/helpshift-tutorial), PortalTalk [365](https://www.portaltalk.com/), [coreview](https://portal.coreview.com/), silenciar [Cloud Office365 Connector](https://laxmi.squelch.io/login), [autenticação PingFlow](https://app-staging.pingview.io/), [PrinterLogic SaaS](https://docs.microsoft.com/azure/active-directory/saas-apps/printerlogic-saas-tutorial), [tarefa Connect](https://docs.microsoft.com/azure/active-directory/saas-apps/taskize-connect-tutorial), [Sandwai](https://app.sandwai.com/) [, EZRentOut](https://docs.microsoft.com/azure/active-directory/saas-apps/ezrentout-tutorial), [AssetSonar](https://docs.microsoft.com/azure/active-directory/saas-apps/assetsonar-tutorial), [Akari assistente virtual](https://akari.io/akari-virtual-assistant/)

Para obter mais informações sobre os aplicativos, consulte [integração de aplicativos SaaS com o Active Directory do Azure](https://aka.ms/appstutorial). Para obter mais informações sobre como listar seu aplicativo na galeria de aplicativos do Azure AD, consulte [Listar seu aplicativo na galeria de aplicativos do Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="two-new-identity-protection-detections"></a>Duas novas detecções de proteção de identidade

**Tipo:** novo recurso  
**Categoria de serviço:** Proteção de identidade  
**Funcionalidade do produto:** segurança e proteção da identidade
 
Adicionamos dois novos tipos de detecção vinculados de entrada à proteção de identidade: regras de manipulação de caixa de entrada suspeita e viagem impossível. Essas detecções offline são descobertas por Microsoft Cloud App Security (MCAS) e influenciam o usuário e o risco de entrada no Identity Protection. Para obter mais informações sobre essas detecções, consulte nossos [tipos de risco de entrada](https://docs.microsoft.com/azure/active-directory/identity-protection/concept-identity-protection-risks#sign-in-risk).
 
---
 
### <a name="breaking-change-uri-fragments-will-not-be-carried-through-the-login-redirect"></a>Alteração significativa: fragmentos de URI não serão transferidos por meio do redirecionamento de logon

**Tipo:** recurso alterado  
**Categoria de serviço:** autenticações (logons)  
**Funcionalidade do produto:** Autenticação do usuário
 
A partir de 8 de fevereiro de 2020, quando uma solicitação for enviada ao login.microsoftonline.com para entrar em um usuário, o serviço acrescentará um fragmento vazio à solicitação.  Isso impede uma classe de ataques de redirecionamento, garantindo que o navegador apague qualquer fragmento existente na solicitação. Nenhum aplicativo deve ter uma dependência desse comportamento. Para obter mais informações, consulte [alterações recentes](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#february-2020) na documentação da plataforma de identidade da Microsoft.

