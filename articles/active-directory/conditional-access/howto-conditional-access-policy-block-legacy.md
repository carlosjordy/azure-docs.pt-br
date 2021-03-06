---
title: Acesso Condicional - Bloquear autenticação herdada - Azure Active Directory
description: Criar uma política de Acesso Condicional para bloquear protocolos de autenticação herdada
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 05/26/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2d6539a233fbb8038d82a8ea41da2c9e79745324
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83995183"
---
# <a name="conditional-access-block-legacy-authentication"></a>Acesso Condicional: Bloquear a autenticação herdada

Devido ao maior risco associado aos protocolos de autenticação herdada, a Microsoft recomenda que as organizações bloqueiem as solicitações de autenticação que usem esses protocolos e exijam autenticação moderna.

## <a name="create-a-conditional-access-policy"></a>Criar uma política de Acesso Condicional

As etapas a seguir ajudarão a criar uma política de Acesso Condicional para o bloqueio de solicitações de autenticação herdada. Inicialmente essa política é colocada em [modo somente de relatório](howto-conditional-access-report-only.md) para que os administradores possam determinar o impacto que ela terá sobre os usuários existentes. Quando os administradores tiverem certeza de que a política está sendo aplicada conforme pretendido, poderão mudar para **Ativada** ou preparar a implantação adicionando alguns grupos específicos e excluindo outros.

1. Entre no **portal do Azure** como administrador global, administrador de segurança ou administrador de Acesso Condicional.
1. Procure **Azure Active Directory** > **Segurança** > **Acesso Condicional**.
1. Selecione **Nova política**.
1. Dê um nome à sua política. Recomendamos que as organizações criem um padrão significativo para os nomes de suas políticas.
1. Em **Atribuições**, selecione **Usuários e grupos**
   1. Em **Incluir**, selecione **Todos os usuários**.
   1. Em **Excluir**, selecione **Usuários e grupos** e escolha as contas que vão continuar podendo usar a autenticação herdada. Exclua pelo menos uma conta para impedir que você fique bloqueado. Se você não excluir nenhuma conta, não poderá criar essa política.
   1. Selecione **Concluído**.
1. Em **Aplicativos ou ações de nuvem**, selecione **Todos os aplicativos de nuvem**.
   1. Selecione **Concluído**.
1. Em **Condições** > **Aplicativos clientes (versão prévia)** , defina **Configurar** com **Sim**.
   1. Marque apenas as caixas **Aplicativos móveis e clientes desktop** > **Outros clientes**.
   1. Selecione **Concluído**.
1. Em **Controles de acesso** > **Conceder**, selecione **Bloquear acesso**.
   1. Selecione **Selecionar**.
1. Confirme suas configurações e defina **Habilitar política** com **Somente relatório**.
1. Selecione **Criar** para criar e habilitar sua política.

## <a name="next-steps"></a>Próximas etapas

[Políticas comuns de Acesso Condicional ](concept-conditional-access-policy-common.md)

[Determinar o impacto usando o modo somente relatório de Acesso Condicional](howto-conditional-access-report-only.md)

[Simular comportamento de entrada usando a ferramenta What If de Acesso Condicional](troubleshoot-conditional-access-what-if.md)

[Como configurar um dispositivo ou aplicativo multifuncional para enviar e-mail usando Office 365 e Microsoft 365](/exchange/mail-flow-best-practices/how-to-set-up-a-multifunction-device-or-application-to-send-email-using-office-3)
