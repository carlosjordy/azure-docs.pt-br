---
title: Gerenciar regras de firewall – Hiperescala (Citus) – Banco de Dados do Azure para PostgreSQL
description: Criar e gerenciar regras de firewall para o Banco de Dados do Azure para PostgreSQL – Hiperescala (Citus) usando o portal do Azure
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 9/12/2019
ms.openlocfilehash: c84616e8a9b9ff9722f5a104175c80c37dbcbcc3
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86116906"
---
# <a name="manage-firewall-rules-for-azure-database-for-postgresql---hyperscale-citus"></a>Gerenciar regras de firewall para o Banco de Dados do Azure para PostgreSQL – Hiperescala (Citus)
As regras de firewall no nível do servidor podem ser usadas para gerenciar o acesso a um nó coordenador de Hiperescala (Citus) de um endereço IP especificado ou de um intervalo de endereços IP.

## <a name="prerequisites"></a>Pré-requisitos
Para seguir este guia de instruções, você precisa:
- Um grupo de servidores [Criar um grupo de servidores do Banco de Dados do Azure para PostgreSQL – Hiperescala (Citus)](quickstart-create-hyperscale-portal.md).

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Criar uma regra de firewall de nível de servidor no portal do Azure

> [!NOTE]
> Essas configurações também são acessíveis durante a criação de um grupo de servidores do Banco de Dados do Azure para PostgreSQL – Hiperescala (Citus). Na guia **Rede**, clique em **Ponto de extremidade público**.
> ![Portal do Azure – guia Rede](./media/howto-hyperscale-manage-firewall-using-portal/0-create-public-access.png)

1. Na página do grupo de servidores PostgreSQL, no título Segurança, clique em **Rede** para abrir as Regras de firewall.

   ![Portal do Azure – clique em Rede](./media/howto-hyperscale-manage-firewall-using-portal/1-connection-security.png)

2. Clique em **Adicionar IP do cliente** na barra de ferramentas (opção A abaixo) ou no link (opção B). As duas opções criam automaticamente uma regra de firewall com o endereço IP público do seu computador, como visto pelo sistema do Azure.

   ![Portal do Azure – clique em Adicionar IP do cliente](./media/howto-hyperscale-manage-firewall-using-portal/2-add-my-ip.png)

Como alternativa, clicar em **+ Adicionar 0.0.0.0 - 255.255.255.255** (à direita da opção B) permite que não apenas o IP, mas a Internet inteira acesse a porta 5432 do nó coordenador. Nessa situação, os clientes ainda precisam fazer logon com o nome de usuário e a senha corretos para usar o cluster. No entanto, recomendamos permitir acesso mundial apenas por períodos curtos e somente para bancos de dados que não são de produção.

3. Verifique seu endereço IP antes de salvar a configuração. Em algumas situações, o endereço IP observado pelo Portal do Azure é diferente do endereço IP usado ao acessar a Internet e os servidores do Azure. Portanto, talvez seja necessário alterar o IP inicial e o IP final para fazer a regra funcionar conforme o esperado.
   Use um mecanismo de pesquisa ou outra ferramenta online para verificar seu próprio endereço IP. Por exemplo, pesquise "qual é meu IP".

   ![Pesquisa do Bing para Qual é meu IP](./media/howto-hyperscale-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Adicionar outros intervalos de endereço. Nas regras de firewall, você pode especificar apenas um endereço IP ou um intervalo de endereços. Se você desejar limitar a regra a um único endereço IP, digite o mesmo endereço no campo IP inicial e IP final. Abrir o firewall permite que administradores, usuários e aplicativos acessem o nó coordenador na porta 5432.

5. Clique em **Salvar** na barra de ferramentas para salvar essa regra de firewall no nível de servidor. Aguarde a confirmação de que a atualização das regras de firewall foi bem-sucedida.

## <a name="connecting-from-azure"></a>Conexão pelo Azure

Há uma forma fácil de conceder ao Banco de dados de hiperescala acesso aos aplicativos hospedados no Azure (como um aplicativo dos Aplicativos Web do Azure ou aqueles em execução em uma VM do Azure). Basta definir a opção **Permitir que serviços e recursos do Azure acessem este grupo de servidores** como **Sim** no portal, no painel **Rede**, e clicar em **Salvar**.

> [!IMPORTANT]
> Esta opção configura o firewall para permitir todas as conexões do Azure, incluindo as conexões das assinaturas de outros clientes. Ao selecionar essa opção, verifique se as permissões de logon e de usuário limitam o acesso somente a usuários autorizados.

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Gerenciar regras de firewall existentes no nível de servidor pelo Portal do Azure
Repita as etapas para gerenciar as regras de firewall.
* Para adicionar o computador atual, clique no botão + **Adicionar IP do cliente**. Clique em **Salvar** para salvar as alterações.
* Para adicionar mais endereços IP, digite o Nome da Regra, o Endereço IP Inicial e o Endereço IP Final. Clique em **Salvar** para salvar as alterações.
* Para modificar uma regra existente, clique em qualquer um dos campos na regra e modifique. Clique em **Salvar** para salvar as alterações.
* Para excluir uma regra existente, clique nas reticências [...] e clique em **Excluir** para remover a regra. Clique em **Salvar** para salvar as alterações.

## <a name="next-steps"></a>Próximas etapas
- Saiba mais sobre o [Conceito de regras de firewall](concepts-hyperscale-firewall-rules.md), incluindo como solucionar problemas de conexão.
