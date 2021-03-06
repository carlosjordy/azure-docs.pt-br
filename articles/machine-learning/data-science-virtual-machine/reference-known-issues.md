---
title: 'Referência: problemas conhecidos & solução de problemas'
titleSuffix: Azure Data Science Virtual  Machine
description: Obtenha uma lista dos problemas conhecidos, soluções alternativas e solução de problemas do Azure Máquina Virtual de Ciência de Dados
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: reference
ms.date: 10/10/2019
ms.openlocfilehash: b83effa95b17d712d4019f8ab5bf13c4f02a7d2b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78206513"
---
# <a name="known-issues-and-troubleshooting-the-azure-data-science-virtual-machine"></a>Problemas conhecidos e solução de problemas do Máquina Virtual de Ciência de Dados do Azure

Este artigo ajuda você a encontrar e corrigir erros ou falhas que podem surgir ao usar o Máquina Virtual de Ciência de Dados do Azure.

## <a name="python-package-installation-issues"></a>Problemas de instalação do pacote do Python

### <a name="installing-packages-with-pip-breaks-dependencies-on-linux"></a>Instalando pacotes com dependências de quebras de pip no Linux

Use `sudo pip install` em vez de `pip install` ao instalar pacotes.

## <a name="disk-encryption-issues"></a>Problemas de criptografia de disco

### <a name="disk-encryption-fails-on-the-ubuntu-dsvm"></a>A criptografia de disco falha no Ubuntu DSVM

Atualmente, não há suporte para Azure Disk Encryption (ADE) no DSVM do Ubuntu. Como alternativa, considere configurar a [criptografia do lado do servidor de Azure Managed disks](../../virtual-machines/windows/disk-encryption.md).

## <a name="tool-appears-disabled"></a>A ferramenta aparece desabilitada

### <a name="hyper-v-does-not-work-on-the-windows-dsvm"></a>O Hyper-V não funciona no DSVM do Windows

O Hyper-V inicialmente não funciona no Windows é o comportamento esperado. Para o desempenho de inicialização, desabilitamos alguns serviços. Para habilitar o Hyper-V:

1. Abra a barra de pesquisa no seu DSVM do Windows
1. Digite "serviços"
1. Definir todos os serviços do Hyper-V como "manual"
1. Defina "gerenciamento de máquina virtual do Hyper-V" como "automático"

Sua tela final deve ter a seguinte aparência:

   ![Habilitar Hyper-V](./media/workaround/hyperv-enable-dsvm.png)

