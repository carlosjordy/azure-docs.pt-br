---
title: Opções de armazenamento de estado na malha de Service Fabric do Azure
description: Saiba mais sobre como armazenar o estado de forma confiável em aplicativos de Malha do Service Fabric em execução na Malha do Azure Service Fabric.
author: dkkapur
ms.author: dekapur
ms.date: 11/27/2018
ms.topic: conceptual
ms.openlocfilehash: d56ce811155f7b7f60fa870dbdae2550afab2eac
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86246819"
---
# <a name="state-management-with-service-fabric"></a>Gerenciamento de estado com o Service Fabric

O Service Fabric dá suporte a várias opções diferentes para armazenamento de estado. Para obter uma visão geral conceitual dos padrões de gerenciamento de estado e do Service Fabric, consulte [Conceitos do Service Fabric: Estado](../service-fabric/service-fabric-concepts-state.md). Os mesmos conceitos se aplicam se seus serviços forem executados dentro ou fora da Malha do Service Fabric. 

Com o serviço de Malha do Service Fabric, você pode facilmente implantar um novo aplicativo e conectá-lo em um repositório de dados existente, hospedado no Azure. Além de usar qualquer banco de dados remoto, há várias opções para armazenar dados, dependendo se o serviço deseja armazenamento local ou remoto. 

## <a name="volumes"></a>Volumes

Contêineres frequentemente usam discos temporários. Discos temporários são efêmeros, no entanto, você obtém um novo disco temporário e perde as informações quando um contêiner falha. Também é difícil de compartilhar informações sobre discos temporários com outros contêineres. Os volumes são diretórios que são montados dentro de suas instâncias de contêiner que você pode usar para manter o estado. Os volumes oferecem armazenamento de arquivos de finalidade geral e permitem a você ler/gravar arquivos usando APIs de arquivo de E/S de disco normal. O recurso de Volume descreve como montar um diretório e qual armazenamento de backup usar. Você pode escolher o armazenamento do Azure File ou disco de Volume do Service Fabric para armazenar dados.

![Volumes][image3]

### <a name="service-fabric-reliable-volume"></a>Reliable Volume do Service Fabric

O Reliable Volume do Service Fabric é um driver de volume do Docker usado para montar um volume local para um contêiner. Leituras e gravações são operações locais e rápidas. Os dados são replicados para fora dos nós secundários, tornando-os altamente disponíveis. Failover também é rápido. Quando um contêiner falha, ele fará failover para um nó que já tem uma cópia dos seus dados. Para obter um exemplo, consulte [como implantar um aplicativo com Service Fabric volume confiável](service-fabric-mesh-howto-deploy-app-sfreliable-disk-volume.md).

### <a name="azure-files-volume"></a>Volume de Armazenamento de Arquivos

Volume de Armazenamento de Arquivos é um driver de volume do Docker usado para montar Arquivos do Azure para compartilhar em um contêiner. O Armazenamento de Arquivos usa o armazenamento de rede, então, a leitura e a gravação acontecem na rede. Em comparação com o Volume Reliable do Service Fabric, o Armazenamento de Arquivos é menos importante, mas fornece um opção de dados mais baratos e completamente confiáveis. Por exemplo, consulte [Como implantar um aplicativo com o Volume de Armazenamento de Arquivos](service-fabric-mesh-howto-deploy-app-azurefiles-volume.md).

## <a name="next-steps"></a>Próximas etapas

Para obter informações sobre o modelo de aplicativo, consulte [recursos de Service Fabric](service-fabric-mesh-service-fabric-resources.md)

[image3]: ./media/service-fabric-mesh-storing-state/volumes.png
