---
title: incluir arquivo
description: incluir arquivo
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 06/23/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 21295089b7ed59189aeb95ba112fb2553c454f11
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85609361"
---
```azurecli
az disk show -g yourResourceGroupName -n yourDiskName --query [encryption.type] -o tsv
```