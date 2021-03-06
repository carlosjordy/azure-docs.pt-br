---
title: Como compartilhar Azure Dev Spaces
services: azure-dev-spaces
ms.date: 05/11/2018
ms.topic: conceptual
description: Saiba como usar Azure Dev Spaces para compartilhar um espaço de desenvolvimento no serviço kubernetes do Azure com outras pessoas em sua equipe
keywords: 'Docker, Kubernetes, Azure, AKS, Serviço de Kubernetes do Azure, contêineres, Helm, malha de serviço, roteamento de malha de serviço, kubectl, k8s '
ms.openlocfilehash: 3cef7cd6a5113401cb1df3dd6c76e4dea3a7524d
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86229288"
---
# <a name="share-azure-dev-spaces"></a>Compartilhar Azure Dev Spaces

Com o Azure Dev Spaces, você pode compartilhar o espaço de desenvolvimento com outras pessoas de sua equipe. Cada desenvolvedor pode trabalhar em seu próprio espaço sem receio de interromper outros. Além disso, trabalhar em conjunto em um espaço pode permitir que você teste o código de ponta a ponta sem precisar criar simulações ou simular dependências. Consulte o guia [Saiba mais sobre o desenvolvimento em equipe](../team-development-nodejs.md).

## <a name="set-up-a-dev-space-for-multiple-developers"></a>Configurar um espaço de desenvolvimento para vários desenvolvedores

1. Crie um Dev Space no Azure. Escolha [.NET Core 3 VS Code](../get-started-netcore.md), [.NET Core e Visual Studio](../get-started-netcore-visualstudio.md) ou [Node.js e VS Code](../get-started-nodejs.md). É necessário ter acesso de Colaborador ou Proprietário para a assinatura do Azure selecionada.
1. Verifique se cada membro da equipe tem as [permissões apropriadas para acessar o controlador de Azure dev Spaces](../troubleshooting.md#incorrect-rbac-permissions-for-calling-dev-spaces-controller-and-apis). Por exemplo, você pode configurar o **grupo de recursos** do espaço de desenvolvimento do Azure para [conceder acesso de colaborador](../../role-based-access-control/role-assignments-portal.md) para cada membro da equipe. É possível verificar o grupo de recursos de um espaço de desenvolvimento executando este comando: `azds show-context`
1. Solicite aos membros da equipe para **selecionar o espaço de desenvolvimento** para desenvolver nele.
   * **Linha de comando ou VS Code**: para ver o Azure Dev Spaces existente ao qual você tem acesso: `azds space list`. Para selecionar um espaço de desenvolvimento: `azds space select`.
   * **IDE do Visual Studio**: abra um projeto no Visual Studio, selecione **Azure Dev Spaces** na menu suspenso de configurações de inicialização. Na caixa de diálogo que é aberta, selecione um cluster existente.

     ![Menu suspenso de configurações de inicialização do Visual Studio](../media/get-started-netcore-visualstudio/LaunchSettings.png)

## <a name="next-steps"></a>Próximas etapas

Consulte [Saiba mais sobre o desenvolvimento em equipe](../team-development-nodejs.md) para obter mais informações.
