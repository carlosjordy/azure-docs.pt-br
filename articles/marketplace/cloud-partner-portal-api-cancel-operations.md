---
title: Cancelar API de operação-Microsoft Commercial Marketplace
description: A API para cancelar uma operação atualmente em andamento na oferta
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: emuench
ms.author: mingshen
ms.date: 07/14/2020
ms.openlocfilehash: 462ca525be9cf46c87acdf4025223a98afaf8e3b
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86520367"
---
# <a name="cancel-operation"></a>Cancelar operação

> [!NOTE]
> As APIs de Portal do Cloud Partner são integradas ao e continuarão funcionando no Partner Center. A transição apresenta pequenas alterações. Examine as alterações listadas em [portal do Cloud Partner referência de API](./cloud-partner-portal-api-overview.md) para garantir que seu código continue funcionando após a transição para o Partner Center. As APIs de CPP só devem ser usadas para produtos existentes que já foram integrados antes da transição para o Partner Center; os novos produtos devem usar as APIs de envio do Partner Center.

Essa API cancela uma operação atualmente em andamento na oferta. Use [Recuperar API de operações](./cloud-partner-portal-api-retrieve-operations.md) para obter um `operationId` para passar a essa API. Geralmente, o cancelamento é uma operação síncrona, no entanto, em alguns cenários complexos uma nova operação pode ser necessária para cancelar uma operação existente. Nesse caso, o corpo da resposta HTTP contém o local da operação que deve ser usado para consultar o status.

  `POST https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/cancel?api-version=2017-10-31`

## <a name="uri-parameters"></a>Parâmetros do URI

--------------

|  **Nome**    |      **Descrição**                                  |    **Data type**  |
| ------------ |     ----------------                                  |     -----------   |
| publisherId  |  Identificador do publicar, por exemplo, `contoso`         |   Cadeia de caracteres          |
| offerId      |  Identificador da oferta                                     |   Cadeia de caracteres          |
| api-version  |  Versão atual da API                               |    Data           |
|  |  |  |

## <a name="header"></a>Cabeçalho
------

|  **Nome**              |  **Valor**         |
|  ---------             |  ----------        |
|  Tipo de conteúdo          |  aplicativo/json  |
|  Autorização         |  TOKEN de seu portador |
|  |  |

## <a name="body-example"></a>Exemplo de corpo
------------

### <a name="request"></a>Solicitação

``` json
{
   "metadata": {
     "notification-emails": "jondoe@contoso.com"
    }
}     
```

### <a name="request-body-properties"></a>Solicitar propriedades do corpo

|  **Nome**                |  **Descrição**                                               |
|  --------                |  ---------------                                               |
|  emails de notificação     | Lista separada por vírgula de IDs de email a serem notificados sobre o andamento da operação de publicação. |
|  |  |

### <a name="response"></a>Resposta

#### <a name="migrated-offers"></a>Ofertas migradas

`Location: /api/publishers/contoso/offers/contoso-offer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8?api-version=2017-10-31`

#### <a name="non-migrated-offers"></a>Ofertas não migradas

`Location: /api/operations/contoso$contoso-offer$2$preview?api-version=2017-10-31`

### <a name="response-header"></a>Cabeçalho de Resposta

|  **Nome**             |    **Valor**                       |
|  ---------            |    ----------                      |
| Localização    | O caminho relativo para recuperar o status da operação. |
|  |  |

### <a name="response-status-codes"></a>Códigos de status de resposta

| **Código**  |  **Descrição**                                                                       |
|  ------   |  ------------------------------------------------------------------------               |
|  200      | OK. A solicitação foi processada com êxito e a operação é cancelada de forma síncrona. |
|  202      | Aceita. A solicitação foi processada com êxito e a operação está em processo de cancelamento. A localização da operação de cancelamento é retornada no cabeçalho de resposta. |
|  400      | Solicitação incorreta/malformada. O corpo da resposta ao erro pode fornecer mais informações.  |
|  403      | Acesso proibido. O cliente não tem acesso ao namespace especificado na solicitação. |
|  404      | Não encontrado. A entidade especificada não existe. |
|  |  |
