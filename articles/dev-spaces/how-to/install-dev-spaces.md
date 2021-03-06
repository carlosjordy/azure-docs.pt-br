---
title: Habilitar o Azure Dev Spaces no AKS e instalar as ferramentas do lado do cliente
services: azure-dev-spaces
ms.date: 07/24/2019
ms.topic: conceptual
description: Saiba como habilitar o Azure Dev Spaces em um cluster AKS e instalar as ferramentas do lado do cliente.
keywords: Docker, Kubernetes, Azure, AKS, Serviço de Kubernetes do Azure, contêineres, Helm, malha de serviço, roteamento de malha de serviço, kubectl, k8s
ms.openlocfilehash: b62c4a4861529c19363f159b8cc64a32a0ba11e8
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83996254"
---
# <a name="enable-azure-dev-spaces-on-an-aks-cluster-and-install-the-client-side-tools"></a>Habilitar o Azure Dev Spaces em um cluster AKS e instalar as ferramentas do lado do cliente

Este artigo mostra várias maneiras de habilitar o Azure Dev Spaces em um cluster AKS, bem como instalar as ferramentas do lado do cliente.

## <a name="enable-or-remove-azure-dev-spaces-using-the-cli"></a>Habilitar ou remover o Azure Dev Spaces usando a CLI

Antes de poder habilitar o Dev Spaces usando a CLI, você precisa:
* Uma assinatura do Azure. Caso não tenha uma assinatura do Azure, é possível criar uma [conta gratuita][az-portal-create-account].
* [A CLI do Azure instalada][install-cli].
* [Um cluster do AKS][create-aks-cli] em uma [região com suporte][supported-regions].

Use o comando `use-dev-spaces` para habilitar o Dev Spaces no cluster do AKS e siga os prompts.

```azurecli
az aks use-dev-spaces -g myResourceGroup -n myAKSCluster
```

O comando acima habilita o Dev Spaces no cluster *myAKSCluster*, no grupo *myResourceGroup* e cria um espaço de desenvolvimento *padrão*.

```console
'An Azure Dev Spaces Controller' will be created that targets resource 'myAKSCluster' in resource group 'myResourceGroup'. Continue? (y/N): y

Creating and selecting Azure Dev Spaces Controller 'myAKSCluster' in resource group 'myResourceGroup' that targets resource 'myAKSCluster' in resource group 'myResourceGroup'...2m 24s

Select a dev space or Kubernetes namespace to use as a dev space.
 [1] default
Type a number or a new name: 1

Kubernetes namespace 'default' will be configured as a dev space. This will enable Azure Dev Spaces instrumentation for new workloads in the namespace. Continue? (Y/n): Y

Configuring and selecting dev space 'default'...3s

Managed Kubernetes cluster 'myAKSCluster' in resource group 'myResourceGroup' is ready for development in dev space 'default'. Type `azds prep` to prepare a source directory for use with Azure Dev Spaces and `azds up` to run.
```

O comando `use-dev-spaces` também instala a CLI do Azure Dev Spaces.

Para remover o Azure Dev Spaces do cluster AKS, use o comando `azds remove`. Por exemplo:

```azurecli
$ azds remove -g MyResourceGroup -n MyAKS
Azure Dev Spaces Controller 'MyAKS' in resource group 'MyResourceGroup' that targets resource 'MyAKS' in resource group 'MyResourceGroup' will be deleted. This will remove Azure Dev Spaces instrumentation from the target resource for new workloads. Continue? (y/N): y

Deleting Azure Dev Spaces Controller 'MyAKS' in resource group 'MyResourceGroup' that targets resource 'MyAks' in resource group 'MyResourceGroup' (takes a few minutes)...
```

O comando acima remove o Azure Dev Spaces do cluster *MyAKS* em *MyResourceGroup*. Todos os namespaces criados com o Azure Dev Spaces permanecerão junto com suas cargas de trabalho, mas as novas cargas de trabalho nesses namespaces não serão instrumentadas com o Azure Dev Spaces. Além disso, poderá haver falha se você reiniciar pods existentes instrumentados com o Azure Dev Spaces. Esses pods devem ser reimplantados sem as ferramentas do Azure Dev Spaces. Para remover completamente o Azure Dev Spaces do cluster, exclua todos os pods em todos os namespaces em que o Azure Dev Spaces tiver sido habilitado.

## <a name="install-the-client-side-tools"></a>Instalar as ferramentas do lado do cliente

Você pode usar as ferramentas do lado do cliente do Azure Dev Spaces para interagir com espaços de desenvolvimento em um cluster AKS do computador local. Há várias maneiras de instalar as ferramentas do lado do cliente:

* No [Visual Studio Code][vscode], instale a [extensão Azure Dev Spaces][vscode-extension].
* No [Visual Studio 2019][visual-studio], instale a carga de trabalho de Desenvolvimento do Azure.
* Baixe e instale o CLI do [Windows][cli-win], [Mac][cli-mac] ou [Linux][cli-linux].

## <a name="next-steps"></a>Próximas etapas

Saiba como o Azure Dev Spaces ajuda você a desenvolver aplicativos mais complexos em vários contêineres e como você pode simplificar o desenvolvimento colaborativo trabalhando com versões diferentes ou branches do seu código em diferentes espaços.

> [!div class="nextstepaction"]
> [Desenvolvimento em equipe com o Azure Dev Spaces][team-development-qs]

[create-aks-cli]: ../../aks/kubernetes-walkthrough.md#create-a-resource-group
[install-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[team-development-qs]: ../quickstart-team-development.md

[az-portal]: https://portal.azure.com
[az-portal-create-account]: https://azure.microsoft.com/free
[cli-linux]: https://aka.ms/get-azds-linux
[cli-mac]: https://aka.ms/get-azds-mac
[cli-win]: https://aka.ms/get-azds-windows
[visual-studio]: https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs
[visual-studio-k8s-tools]: https://aka.ms/get-vsk8stools
[vscode]: https://code.visualstudio.com/download
[vscode-extension]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
