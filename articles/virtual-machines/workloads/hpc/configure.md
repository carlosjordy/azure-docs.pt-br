---
title: Computação de alto desempenho-máquinas virtuais do Azure | Microsoft Docs
description: Saiba mais sobre a computação de alto desempenho no Azure.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/07/2019
ms.author: amverma
ms.openlocfilehash: 723419b97dc024a700d860dd3fe61ff48073a587
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87019991"
---
# <a name="optimization-for-linux"></a>Otimização para Linux

Este artigo mostra algumas técnicas-chave para otimizar a imagem do sistema operacional. Saiba mais sobre como [habilitar a InfiniBand](enable-infiniband.md) e otimizar as imagens do sistema operacional.

## <a name="update-lis"></a>Atualizar LIS

Se estiver implantando usando uma imagem personalizada (por exemplo, um sistema operacional mais antigo, como CentOS/RHEL 7,4 ou 7,5), atualize LIS na VM.

```bash
wget https://aka.ms/lis
tar xzf lis
pushd LISISO
./upgrade.sh
```

## <a name="reclaim-memory"></a>Recuperar memória

Melhore a eficiência recuperando automaticamente a memória para evitar o acesso remoto à memória.

```bash
echo 1 >/proc/sys/vm/zone_reclaim_mode
```

Para fazer essa persistência após a reinicialização da VM:

```bash
echo "vm.zone_reclaim_mode = 1" >> /etc/sysctl.conf sysctl -p
```

## <a name="disable-firewall-and-selinux"></a>Desabilitar Firewall e SELinux

Desabilite o firewall e o SELinux.

```bash
systemctl stop iptables.service
systemctl disable iptables.service
systemctl mask firewalld
systemctl stop firewalld.service
systemctl disable firewalld.service
iptables -nL
sed -i -e's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
```

## <a name="disable-cpupower"></a>Desabilitar cpupower

Desabilite o cpupower.

```bash
service cpupower status
if enabled, disable it:
service cpupower stop
sudo systemctl disable cpupower
```

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre como [habilitar a InfiniBand](enable-infiniband.md) e otimizar imagens do sistema operacional.

* Saiba mais sobre o [HPC](/azure/architecture/topics/high-performance-computing/) no Azure.
