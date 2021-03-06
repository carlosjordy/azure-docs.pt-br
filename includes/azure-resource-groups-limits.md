---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/24/2020
ms.author: tomfitz
ms.openlocfilehash: 33a63280f6973d2c5e29db29f7a6f3fc68c57c77
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84424700"
---
| Recurso | Limite |
| --- | --- |
| Recursos por [grupo de recursos](../articles/azure-resource-manager/management/overview.md#resource-groups) | Os recursos não são limitados pelo grupo de recursos. Em vez disso, eles são limitados pelo tipo de recurso em um grupo de recursos. Consulte a próxima linha. |
| Recursos por grupo de recursos, por tipo de recurso |800-alguns tipos de recursos podem exceder o limite de 800. Consulte [recursos não limitados a 800 instâncias por grupo de recursos](../articles/azure-resource-manager/management/resources-without-resource-group-limit.md). |
| Implantações por grupo de recursos no histórico de implantações |800<sup>1</sup> |
| Recursos por implantação |800 |
| Bloqueios de gerenciamento por escopo exclusivo |20 |
| Número de marcas por recurso ou grupo de recursos |50 |
| Comprimento da chave de marca |512 |
| Comprimento do valor da marca |256 |

<sup>1</sup> A partir de junho de 2020, as implantações serão excluídas automaticamente do histórico conforme você perto do limite. A exclusão de uma entrada do histórico de implantação não afeta os recursos implantados. Para obter mais informações, consulte [exclusões automáticas do histórico de implantação](../articles/azure-resource-manager/templates/deployment-history-deletions.md).

#### <a name="template-limits"></a>Limites de modelo

| Valor | Limite |
| --- | --- |
| Parâmetros |256 |
| Variáveis |256 |
| Recursos (incluindo a contagem de cópias) |800 |
| outputs |64 |
| Expressão de modelo |24.576 caracteres |
| Recursos em modelos exportados |200 |
| Tamanho do modelo |4 MB |
| Tamanho do arquivo de parâmetro |64 KB |

Você pode exceder alguns limites de modelo usando um modelo aninhado. Para obter mais informações, consulte [usar modelos vinculados ao implantar recursos do Azure](../articles/azure-resource-manager/templates/linked-templates.md). Para reduzir o número de parâmetros, variáveis ou saídas, você pode combinar vários valores em um objeto. Para saber mais, veja [Objetos como parâmetros](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).
