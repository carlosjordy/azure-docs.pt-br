---
title: Provisionar e gerenciar um ambiente de Gen 2 – série temporal do Azure | Microsoft Docs
description: Saiba como provisionar e gerenciar um ambiente Azure Time Series Insights Gen 2.
author: shipra1mishra
ms.author: shmishr
manager: diviso
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 07/08/2020
ms.custom: seodec18
ms.openlocfilehash: d067d4a7fff385deea946ffa5475e1eb83548a50
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87095825"
---
# <a name="provision-and-manage-azure-time-series-insights-gen2"></a>Provisionar e gerenciar Azure Time Series Insights Gen2

Este artigo descreve como criar e gerenciar um ambiente de Azure Time Series Insights Gen2 usando o [portal do Azure](https://portal.azure.com/).

## <a name="overview"></a>Visão geral

Ao provisionar um ambiente de Azure Time Series Insights Gen2, você cria esses recursos do Azure:

* Um Azure Time Series Insights ambiente Gen2 que segue o modelo de preços pago conforme o uso
* Uma conta de armazenamento do Azure
* Um armazenamento inesperado opcional para consultas mais rápidas e ilimitadas

> [!TIP]
> * Saiba [como planejar seu ambiente](./time-series-insights-update-plan.md).
> * Leia sobre como [Adicionar uma origem do hub de eventos](./time-series-insights-how-to-add-an-event-source-eventhub.md) ou como [Adicionar uma origem do Hub IOT](./time-series-insights-how-to-add-an-event-source-iothub.md).

Você saberá como:

1.  Associe cada ambiente Azure Time Series Insights Gen 2 a uma origem de evento. Você também fornecerá uma propriedade de ID de carimbo de data/hora e um grupo de consumidores exclusivo para garantir que o ambiente tenha acesso aos eventos apropriados.

1. Após a conclusão do provisionamento, você pode modificar suas políticas de acesso e outros atributos de ambiente para atender às suas necessidades de negócios.

   > [!NOTE]
   > A primeira etapa é opcional ao provisionar um ambiente. Se você ignorar essa etapa, deverá anexar uma origem do evento ao ambiente posteriormente para que os dados possam começar a fluir para o seu ambiente e possam ser acessados por meio da consulta.

## <a name="create-the-environment"></a>Criar o ambiente

Para criar um ambiente Azure Time Series Insights Gen 2:
1. Crie um recurso de Azure Time Series Insights em *Internet das coisas* no [portal do Azure](https://portal.azure.com/).

1. Selecione **Gen2 (L1)** como a **camada**. Forneça um nome de ambiente e escolha o grupo de assinaturas e o grupo de recursos a serem usados. Em seguida, selecione um local com suporte para hospedar o ambiente.

   [![Criar uma instância do Azure Time Series Insights.](media/v2-update-manage/create-and-manage-configuration.png)](media/v2-update-manage/create-and-manage-configuration.png#lightbox)

1. Insira uma ID do Time Series.

    > [!NOTE]
    > * A ID da série temporal diferencia *maiúsculas de minúsculas* e *imutável*. (Ela não poderá ser alterada após definida.)
    > * As IDs de série temporal podem ter até *três* chaves. Imagine-o como uma chave primária em um banco de dados, que representa exclusivamente cada sensor de dispositivo que enviaria dados para o seu ambiente. Pode ser uma propriedade ou uma combinação de até três propriedades.
    > * Leia mais sobre [como escolher uma ID de série temporal](time-series-insights-update-how-to-id.md)

1. Crie uma conta de armazenamento do Azure selecionando um nome de conta de armazenamento, tipo de conta e designando uma opção de [replicação](https://docs.microsoft.com/azure/storage/common/redundancy-migration?tabs=portal) . Fazer isso cria automaticamente uma conta de armazenamento do Azure. Por padrão, a conta de [uso geral v2](https://docs.microsoft.com/azure/storage/common/storage-account-overview) será criada. A conta é criada na mesma região que o ambiente de Azure Time Series Insights Gen2 que você selecionou anteriormente. Como alternativa, você também pode trazer seu próprio armazenamento (BYOS) por meio do [modelo ARM](./time-series-insights-manage-resources-using-azure-resource-manager-template.md) ao criar um novo ambiente do Azure Time Series Gen2. 

1. **(Opcional)** Habilite a loja a quente para seu ambiente se você quiser consultas mais rápidas e ilimitadas sobre os dados mais recentes em seu ambiente. Você também pode criar ou excluir uma loja quente por meio da opção de **configuração de armazenamento** no painel de navegação esquerdo, depois de criar um ambiente de Azure Time Series insights Gen2.

1. **(Opcional)** Você pode adicionar uma origem do evento agora. Você também pode aguardar até que a instância tenha sido provisionada.

   * O Azure Time Series Insights dá suporte ao [Hub IOT do Azure](./time-series-insights-how-to-add-an-event-source-iothub.md) e aos [hubs de eventos do Azure](./time-series-insights-how-to-add-an-event-source-eventhub.md) como opções de origem do evento. Embora seja possível adicionar apenas uma única origem do evento ao criar o ambiente, você pode adicionar outra origem do evento posteriormente. 
   
     Você pode selecionar um grupo de consumidores existente ou criar um novo grupo de consumidores ao adicionar a origem do evento. Observe que a origem do evento requer um grupo de consumidores exclusivo para que o seu ambiente leia dados nele.

   * Escolha a propriedade de carimbo de data/hora apropriada. Por padrão, Azure Time Series Insights usa o tempo de enfileiramento de mensagens para cada origem de evento.

     > [!TIP]
     > O tempo de enfileiramento de mensagens pode não ser a configuração mais adequada para uso em cenários de eventos de lote ou em cenários de carregamento de dados históricos. Nesses casos, certifique-se de verificar sua decisão de usar ou não usar uma propriedade Timestamp.

     [![Guia Configuração de origem do evento](media/v2-update-manage/create-and-manage-event-source.png)](media/v2-update-manage/create-and-manage-event-source.png#lightbox)

1. Confirme se o seu ambiente foi provisionado e configurado da maneira desejada.

    [![Revisar + criar guia](media/v2-update-manage/create-and-manage-review-and-confirm.png)](media/v2-update-manage/create-and-manage-review-and-confirm.png#lightbox)

## <a name="manage-the-environment"></a>Gerenciar o ambiente

Você pode gerenciar seu ambiente de Azure Time Series Insights Gen2 usando o portal do Azure. Há algumas diferenças importantes entre um ambiente Gen2 e os ambientes Gen1 S1 ou Gen1 S2 para se lembrar ao gerenciar seu ambiente por meio do portal do Azure:

* A folha de **visão geral** do portal do Azure Gen2 tem as seguintes alterações:

  * A capacidade é removida porque não se aplica a ambientes Gen2.
  * A propriedade **ID da série temporal** é adicionada. Ela determina como os dados são particionados.
  * Conjuntos de dados de referência são removidos.
  * A URL exibida direciona você para o [Gerenciador de Azure Time Series insights](./time-series-insights-update-explorer.md).
  * O nome da sua conta de Armazenamento do Azure é listado.

* A folha **Configurar** do portal do Azure é removida porque as unidades de escala não se aplicam a ambientes Azure Time Series insights Gen2. No entanto, você pode usar a **configuração de armazenamento** para configurar o armazenamento quente introduzido recentemente.

* A folha de **dados de referência** do portal do Azure é removida em Azure Time Series insights Gen2 porque o conceito de dados de referência foi substituído pelo [TSM (modelo de série temporal)](./time-series-insights-update-how-to-tsm.md).

[![Azure Time Series Insights ambiente Gen2 no portal do Azure](media/v2-update-manage/create-and-manage-overview-confirm.png)](media/v2-update-manage/create-and-manage-overview-confirm.png#lightbox)

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre Azure Time Series Insights ambientes geralmente disponíveis e ambientes Gen2 lendo [planejar seu ambiente](./time-series-insights-update-plan.md).

- Saiba como [Adicionar uma origem do hub de eventos](./time-series-insights-how-to-add-an-event-source-eventhub.md).

- Configurar uma [origem do Hub IOT](./time-series-insights-how-to-add-an-event-source-iothub.md).
