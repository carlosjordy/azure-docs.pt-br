---
title: Problemas de diagnóstico da Área de Trabalho Virtual do Windows - Azure
description: Como usar o recurso de diagnóstico da Área de Trabalho Virtual do Windows para diagnosticar problemas.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 4b9e63ca3c6822f4fd9ff215ccddaaf82ff55369
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87075665"
---
# <a name="identify-and-diagnose-windows-virtual-desktop-issues"></a>Identificar e diagnosticar problemas de área de trabalho virtual do Windows

>[!IMPORTANT]
>Este conteúdo se aplica à atualização da Spring 2020 com objetos da Área de Trabalho Virtual do Windows do Azure Resource Manager. Se você estiver usando a Área de Trabalho Virtual do Windows na versão 2019, sem objetos do Azure Resource Manager, confira [este artigo](./virtual-desktop-fall-2019/diagnostics-role-service-2019.md).
>
> A atualização 2020 da Área de Trabalho Virtual do Windows está em versão prévia pública no momento. Essa versão prévia é fornecida sem um contrato de nível de serviço e não é recomendamos usá-la para cargas de trabalho de produção. Alguns recursos podem não ter suporte ou podem ter restrição de recursos.
> Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

A Área de Trabalho Virtual do Windows oferece um recurso de diagnóstico que permite ao administrador identificar problemas por meio de uma única interface. Para saber mais sobre os recursos de diagnóstico da área de trabalho virtual do Windows, confira [usar o log Analytics para o recurso de diagnóstico](diagnostics-log-analytics.md).

As conexões que não acessam a Área de Trabalho Virtual do Windows não são exibidas nos resultados do diagnóstico porque o próprio serviço da função de diagnóstico faz parte da Área de Trabalho Virtual do Windows. Problemas de conexão da Área de Trabalho Virtual do Windows podem ocorrer quando o usuário final enfrenta problemas de conectividade de rede.

## <a name="common-error-scenarios"></a>Cenários de erro comuns

Os cenários de erro são categorizados como internos ao serviço e externos à Área de Trabalho Virtual do Windows.

* Problema interno: especifica cenários que não podem ser mitigados pelo cliente e precisam ser resolvidos como um problema de suporte. Ao fornecer comentários por meio da [comunidade de tecnologia de área de trabalho virtual do Windows](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop), inclua a ID de correlação e o período de tempo aproximado de quando o problema ocorreu.
* Problema externo: relaciona-se a cenários que podem ser atenuados pelo cliente. São externos à Área de Trabalho Virtual do Windows.

A tabela a seguir lista os erros comuns que os administradores podem encontrar.

>[!NOTE]
>Essa lista inclui os erros mais comuns e é atualizada regularmente. Para garantir que você tenha as informações mais atualizadas, verifique este artigo pelo menos uma vez por mês.

## <a name="management-errors"></a>Erros de gerenciamento

|Mensagem de erro|Solução sugerida|
|---|---|
|Falha ao criar a chave de registro |Não foi possível criar o token de registro. Tente criá-lo novamente com um tempo de expiração mais curto (entre 1 hora e 1 mês). |
|Falha ao excluir a chave de registro|Não foi possível excluir o token de registro. Tente excluí-lo novamente. Se ainda não funcionar, use o PowerShell para verificar se o token ainda está lá. Se ele estiver lá, exclua-o com o PowerShell.|
|Falha ao alterar o modo de descarga do host da sessão |Não foi possível alterar o modo de descarga na VM. Verifique o status da VM. Se a VM não estiver disponível, o modo de descarga não poderá ser alterado.|
|Falha ao desconectar sessões de usuário |Não foi possível desconectar o usuário da VM. Verifique o status da VM. Se a VM não estiver disponível, a sessão de usuário não poderá ser desconectada. Se a VM estiver disponível, verifique o status da sessão do usuário para ver se ela está desconectada. |
|Falha ao fazer logoff de todos os usuários no host da sessão |Não foi possível desconectar os usuários da VM. Verifique o status da VM. Se não estiver disponível, os usuários não poderão ser desconectados. Verifique o status da sessão de usuário para ver se eles já estão desconectados. Você pode forçar a desconexão com o PowerShell. |
|Falha ao cancelar a atribuição do usuário do grupo de aplicativos|Não foi possível cancelar a publicação de um grupo de aplicativos para um usuário. Verifique se o usuário está disponível no Azure AD. Verifique se o usuário faz parte de um grupo de usuários no qual o grupo de aplicativos está publicado. |
|Erro ao recuperar os locais disponíveis |Verifique o local da VM usado no Assistente para criar pool de hosts. Se a imagem não estiver disponível nesse local, adicione a imagem nesse local ou escolha um local de VM diferente. |

### <a name="external-connection-error-codes"></a>Códigos de erro de conexão externa

|Código numérico|Código do erro|Solução sugerida|
|---|---|---|
|-2147467259|ConnectionFailedAdTrustedRelationshipFailure|O host da sessão não entrou corretamente no Active Directory.|
|-2146233088|ConnectionFailedUserHasValidSessionButRdshIsUnhealthy|As conexões falharam porque o host da sessão não está disponível. Verifique a integridade do host da sessão.|
|-2146233088|ConnectionFailedClientDisconnect|Se você observar esse erro com frequência, verifique se o computador do usuário está conectado à rede.|
|-2146233088|ConnectionFailedNoHealthyRdshAvailable|A sessão à qual o usuário do host tentou se conectar não está íntegra. Depurar a máquina virtual.|
|-2146233088|ConnectionFailedUserNotAuthorized|O usuário não tem permissão para acessar a área de trabalho ou o aplicativo publicado. O erro pode aparecer depois que o administrador removeu os recursos publicados. Peça ao usuário para atualizar o feed no aplicativo da Área de Trabalho Remota.|
|2|FileNotFound|O aplicativo que o usuário tentou acessar foi instalado incorretamente ou definido como um caminho incorreto.<br><br>Ao publicar novos aplicativos enquanto o usuário tem uma sessão ativa, o usuário não poderá acessar esse aplicativo. A sessão deve ser desligada e reiniciada para que o usuário possa acessar o aplicativo. |
|3|InvalidCredentials|O nome de usuário ou a senha digitada pelo usuário não corresponde aos nomes de usuário ou senhas existentes. Examine as credenciais quanto a erros de digitação e tente novamente.|
|8|ConnectionBroken|A conexão entre o cliente e o gateway ou o servidor caiu. Nenhuma ação necessária, a menos que isso ocorra inesperadamente.|
|14|UnexpectedNetworkDisconnect|A conexão com a rede caiu. Peça ao usuário para se conectar novamente.|
|24|ReverseConnectFailed|A máquina virtual do host não tem uma linha de visão direta para o gateway de área de trabalho remota. Verifique se o endereço IP do gateway pode ser resolvido.|

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre as funções na Área de Trabalho Virtual do Windows, confira [ambiente da Área de Trabalho Virtual do Windows](environment-setup.md).

Para ver uma lista dos cmdlets do PowerShell disponíveis para a Área de Trabalho Virtual do Windows, confira a [referência do PowerShell](/powershell/windows-virtual-desktop/overview).
