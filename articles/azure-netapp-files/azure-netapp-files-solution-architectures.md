---
title: Arquiteturas de solução usando Azure NetApp Files | Microsoft Docs
description: Fornece referências a práticas recomendadas para arquiteturas de solução usando Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/06/2020
ms.author: b-juche
ms.openlocfilehash: 23ec482de740cc1ac8800a5de1c0e3be1f055df7
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86045477"
---
# <a name="solution-architectures-using-azure-netapp-files"></a>Arquiteturas da solução usando o Azure NetApp Files
Este artigo fornece referências a práticas recomendadas que podem ajudá-lo a entender as arquiteturas de solução para usar o Azure NetApp Files.  

O diagrama a seguir resume as categorias de arquiteturas de solução que Azure NetApp Files oferece:

![Categorias de arquitetura da solução](../media/azure-netapp-files/solution-architecture-categories.png)

## <a name="linux-oss-apps-and-database-solutions"></a>Aplicativos de sistemas operacionais Linux e soluções de banco de dados

Esta seção fornece referências para soluções para bancos de dados e aplicativos de OSS do Linux. 

### <a name="oracle"></a>Oracle

* [Guia de práticas recomendadas de implantação do Oracle no Azure usando o Azure NetApp Files](https://www.netapp.com/us/media/tr-4780.pdf)
* [Imagens de VM Oracle e sua implantação no Microsoft Azure: opções de configuração de armazenamento compartilhado](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-vm-solutions#shared-storage-configuration-options)
* [Benefícios do uso do Azure NetApp Files com o Oracle Database](solutions-benefits-azure-netapp-files-oracle-database.md)

## <a name="windows-apps-and-sql-server-solutions"></a>Aplicativos do Windows e soluções de SQL Server

Esta seção fornece referências para aplicativos do Windows e soluções de SQL Server.

### <a name="file-sharing-and-global-file-caching"></a>Compartilhamento de arquivos e cache de arquivos global

* [Implantação do Talon/Azure NetApp Files](https://youtu.be/91LKb1qsLIM)

### <a name="sql-server"></a>SQL Server

* [Implantar SQL Server sobre SMB com Azure NetApp Files](https://www.youtube.com/watch?v=x7udfcYbibs)
* [Implante SQL Server Cluster de failover do Always on via SMB com Azure NetApp Files](https://www.youtube.com/watch?v=zuNJ5E07e8Q)
* [Implantar grupos de disponibilidade AlwaysOn com Azure NetApp Files](https://www.youtube.com/watch?v=y3VQmzzeyvc)

## <a name="sap-on-azure-solutions"></a>Soluções SAP no Azure

Esta seção fornece referências ao SAP em soluções do Azure. 

### <a name="generic-sap-and-sap-netweaver"></a>SAP e SAP NetWeaver genéricos 

* [Aplicativos SAP em Microsoft Azure usando Azure NetApp Files](https://www.netapp.com/us/media/tr-4746.pdf)
* [Alta disponibilidade do SAP NetWeaver em VMs do Azure no SUSE Linux Enterprise Server com o Azure NetApp Files para aplicativos SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files)
* [Alta disponibilidade para SAP NetWeaver em VMs do Azure em Red Hat Enterprise Linux com Azure NetApp Files para aplicativos SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files)
* [Alta disponibilidade para SAP NetWeaver em VMs do Azure no Windows com Azure NetApp Files (SMB) para aplicativos SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-windows-netapp-files-smb)
* [Alta disponibilidade para SAP NetWeaver em VMs do Azure em Red Hat Enterprise Linux para aplicativos SAP guia de vários SIDs](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-multi-sid)

### <a name="sap-hana"></a>SAP HANA 

* [Configurações de armazenamento de máquina virtual do SAP HANA no Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage)
* [SAP HANA escalar horizontalmente com o nó em espera em VMs do Azure com Azure NetApp Files no SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse)
* [SAP HANA escalar horizontalmente com o nó em espera em VMs do Azure com Azure NetApp Files no Red Hat Enterprise Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel)

### <a name="sap-tech-community-and-blog-posts"></a>Postagens de blog e comunidade do SAP Tech 

* [Azure NetApp Files – backup de SAP HANA em segundos](https://blog.netapp.com/azure-netapp-files-sap-hana-backup-in-seconds/)
* [Azure NetApp Files – restaurar o banco de dados do HANA de um backup de instantâneo](https://blog.netapp.com/azure-netapp-files-backup-sap-hana)
* [Azure NetApp Files – SAP HANA descarregar o backup com sincronização de nuvem](https://blog.netapp.com/azure-netapp-files-sap-hana)
* [Acelere seu SAP HANA cópias do sistema usando Azure NetApp Files](https://blog.netapp.com/sap-hana-faster-using-azure-netapp-files/)
* [Os volumes de nuvem ONTAP e Azure NetApp Files: SAP HANA a migração do sistema facilitada](https://blog.netapp.com/cloud-volumes-ontap-and-azure-netapp-files-sap-hana-system-migration-made-easy/)

## <a name="virtual-desktop-infrastructure-solutions"></a>Soluções de Virtual Desktop Infrastructure

Esta seção fornece referências para soluções de infraestrutura de área de trabalho virtual.

### <a name="windows-virtual-desktop"></a>Área de Trabalho Virtual do Windows

* [Opções de armazenamento para contêineres de perfil FSLogix na área de trabalho virtual do Windows](https://docs.microsoft.com/azure/virtual-desktop/store-fslogix-profile#azure-platform-details)
* [Criar um contêiner de perfil do FSLogix para um pool de hosts usando Azure NetApp Files](https://docs.microsoft.com/azure/virtual-desktop/create-fslogix-profile-container)

## <a name="hpc-solutions"></a>Soluções de HPC

Esta seção fornece referências para soluções de HPC (computação de alto desempenho). 

### <a name="generic-hpc"></a>HPC genérico

* [Azure NetApp Files: obtendo o máximo do seu armazenamento em nuvem](https://cloud.netapp.com/hubfs/Resources/ANF%20PERFORMANCE%20TESTING%20IN%20TEMPLATE.pdf)
* [Executar cargas de trabalho MPI com o lote do Azure e Azure NetApp Files](https://azure.microsoft.com/resources/run-mpi-workloads-with-azure-batch-and-azure-netapp-files/)
* [Nuvem do Azure Cycle: ambientes CycleCloud HPC em Azure NetApp Files](https://docs.microsoft.com/azure/cyclecloud/overview)

### <a name="oil-and-gas"></a>Óleo e gás

* [HPC (computação de alto desempenho): petróleo e gás no Azure](https://techcommunity.microsoft.com/t5/azure-global/high-performance-computing-hpc-oil-and-gas-in-azure/ba-p/824926)
* [Executar o software de simulação de reservatório no Azure](https://docs.microsoft.com/azure/architecture/example-scenario/infrastructure/reservoir-simulation)

### <a name="electronic-design-automation-eda"></a>EDA (automação de design eletrônico)

* [Benefícios de usar o Azure NetApp Files para automação de design eletrônico](solutions-benefits-azure-netapp-files-electronic-design-automation.md)
* [Azure CycleCloud: laboratório de HPC EDA com Azure NetApp Files](https://github.com/Azure/cyclecloud-hands-on-labs/blob/master/EDA/README.md)

### <a name="analytics"></a>Análise

* [Azure NetApp Files: um novo sistema de arquivos compartilhado a ser usado com a grade SAS no Microsoft Azure](https://communities.sas.com/t5/Architecture/Azure-NetApp-Files-A-new-shared-file-system-to-use-with-SAS-Grid/m-p/606978)

## <a name="azure-platform-services-solutions"></a>Soluções de serviços da plataforma Azure

Esta seção fornece soluções para os serviços da plataforma Azure. 

### <a name="azure-kubernetes-services-and-kubernetes"></a>Serviços Kubernetess do Azure e kubernetes

* [Integrar Azure NetApp Files com o serviço kubernetes do Azure](https://docs.microsoft.com/azure/aks/azure-netapp-files)
* [Desempenho de kubernetes fora do mundo no Azure com Azure NetApp Files](https://cloud.netapp.com/blog/ma-anf-blg-configure-kubernetes-openshift)
* [Trident-orquestrador de armazenamento para contêineres](https://netapp-trident.readthedocs.io/en/stable-v20.04/kubernetes/operations/tasks/backends/anf.html)

### <a name="azure-batch"></a>Lote do Azure

* [Executar cargas de trabalho MPI com o lote do Azure e Azure NetApp Files](https://azure.microsoft.com/resources/run-mpi-workloads-with-azure-batch-and-azure-netapp-files/)
 
