---
title: Introdução às Filas do Azure - Armazenamento do Azure
description: Introdução às Filas do Azure
author: mhopkins-msft
ms.author: mhopkins
ms.date: 03/18/2020
ms.service: storage
ms.subservice: queues
ms.topic: overview
ms.reviewer: dineshm
ms.openlocfilehash: 678ab8a3f3dace17671c242939c20cb5499b2e39
ms.sourcegitcommit: ad66392df535c370ba22d36a71e1bbc8b0eedbe3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84805121"
---
# <a name="what-are-azure-queues"></a>O que são as Filas do Azure?

O armazenamento de Filas do Azure é um serviço usado para armazenar grandes quantidades de mensagens. Você acessa as mensagens em qualquer lugar do mundo por meio de chamadas autenticadas usando HTTP ou HTTPS. Uma mensagem da fila pode ter até 64 KB. Uma fila pode conter milhões de mensagens, até o limite da capacidade total de uma conta de armazenamento. As filas são normalmente usadas para criar uma lista de pendências de trabalho para processamento assíncrono.

## <a name="queue-service-concepts"></a>Conceitos do serviço Fila

O serviço Fila contém os seguintes componentes:

![Diagrama mostrando a relação entre uma conta de armazenamento, filas e mensagens](./media/storage-queues-introduction/queue1.png)

* **Formato de URL:** As consultas são endereçáveis usando o seguinte formato de URL:

    `https://<storage account>.queue.core.windows.net/<queue>`
  
    A URL a seguir endereça um fila no diagrama:  
  
    `https://myaccount.queue.core.windows.net/images-to-download`

* **Conta de armazenamento:** Todo o acesso ao Armazenamento do Azure ocorre por meio de uma conta de armazenamento. Para obter informações sobre a capacidade da conta de armazenamento, confira [Metas de escalabilidade e desempenho das contas de Armazenamento Standard](../common/scalability-targets-standard-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).

* **Fila:** Uma fila contém um conjunto de mensagens. O nome da fila **precisa** estar em letras minúsculas. Para saber mais sobre filas de nomenclatura, confira [Nomenclatura de filas e metadados](https://msdn.microsoft.com/library/azure/dd179349.aspx).

* **Mensagem:** Uma mensagem, em qualquer formato, de até 64 KB. Antes da versão 2017-07-29, a vida útil máxima permitida era de sete dias. Para a versão 2017-07-29 ou posterior, a vida útil máxima pode ser qualquer número positivo ou -1, indicando que a mensagem não expira. Se esse parâmetro for omitido, a vida útil padrão será de sete dias.

## <a name="next-steps"></a>Próximas etapas

* [Criar uma conta de armazenamento](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [Introdução às filas usando .NET](storage-dotnet-how-to-use-queues.md)
