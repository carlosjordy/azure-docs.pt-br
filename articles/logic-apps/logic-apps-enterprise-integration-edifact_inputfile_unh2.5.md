---
title: UNH 2,5 segmentos em mensagens EDIFACT
description: Resolver mensagens EDIFACT com segmentos do UNH 2.5 em aplicativos lógicos do Azure com Enterprise Integration Pack
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 04/27/2017
ms.openlocfilehash: ad50cbb423f8c60f1caad159bc1a20cf96ed98aa
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74792546"
---
# <a name="handle-edifact-documents-with-unh25-segments-in-azure-logic-apps"></a>Manipular documentos EDIFACT com segmentos UNH 2.5 nos Aplicativos Lógicos do Azure

Se existir um segmento UNH 2.5 em um documento EDIFACT, o segmento será usado para pesquisa de esquema. Por exemplo, neste exemplo de mensagem EDIFACT, o campo UNH é `EAN008` :

`UNH+SSDD1+ORDERS:D:03B:UN:EAN008`

Para lidar com essa mensagem, siga estas etapas descritas abaixo:

1. Atualize o esquema.

1. Verifique as configurações do contrato.

## <a name="update-the-schema"></a>Atualizar o esquema

Para processar a mensagem, você precisa implantar um esquema que tenha o nome do nó raiz do UNH 2.5. Por exemplo, o nome da raiz do esquema para o campo UNH de exemplo é `EFACT_D03B_ORDERS_EAN008` . Para cada `D03B_ORDERS` um com um segmento unh 2.5 diferente, você precisa implantar um esquema individual.

## <a name="add-schema-to-edifact-agreement"></a>Adicionar esquema ao contrato EDIFACT

### <a name="edifact-decode"></a>Decodificação de EDIFACT

Para decodificar a mensagem de entrada, configure o esquema nas configurações de recebimento do contrato de EDIFACT:

1. Na [portal do Azure](https://portal.azure.com), abra sua conta de integração.

1. Adicione o esquema à sua conta de integração.

1. Configure o esquema nas configurações de recebimento do contrato de EDIFACT.

1. Selecione o contrato EDIFACT e selecione **Editar como JSON**. Adicione o valor de UNH 2.5 à seção do contrato de recebimento `schemaReferences` :

   ![Adicionar o UNH 2.5 ao contrato de recebimento](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)

### <a name="edifact-encode"></a>Codificação de EDIFACT

Para codificar a mensagem de entrada, configure o esquema nas configurações de envio do contrato de EDIFACT

1. Na [portal do Azure](https://portal.azure.com), abra sua conta de integração.

1. Adicione o esquema à sua conta de integração.

1. Configure o esquema nas configurações de envio do contrato de EDIFACT.

1. Selecione o contrato de EDIFACT e clique em **Editar como JSON**.  Adicione o valor UNH2.5 no contrato de envio **schemaReferences**

1. Selecione o contrato EDIFACT e selecione **Editar como JSON**. Adicione o valor de UNH 2.5 à seção do contrato de envio `schemaReferences` :

   ![Adicionar o UNH 2.5 ao contrato de envio](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre [contratos de conta de integração](../logic-apps/logic-apps-enterprise-integration-agreements.md)