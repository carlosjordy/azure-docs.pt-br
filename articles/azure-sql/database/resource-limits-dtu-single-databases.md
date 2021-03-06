---
title: O recurso de DTU limita bancos de dados individuais
description: Esta página descreve alguns limites comuns de recursos de DTU para bancos de dados individuais no banco de dados SQL do Azure.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: seo-lt-2019 sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/20/2019
ms.openlocfilehash: 099094338deba63f678337b7ea13dd9ce9800084
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86517681"
---
# <a name="resource-limits-for-single-databases-using-the-dtu-purchasing-model---azure-sql-database"></a>Limites de recursos para bancos de dados individuais usando o modelo de compra de DTU – banco de dados SQL do Azure
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Este artigo fornece os limites de recursos detalhados para bancos de dados individuais do banco de dados SQL do Azure usando o modelo de compra DTU.

Para os limites de recursos do modelo de compra de DTU para pools elásticos, consulte [limites de recursos de DTU – pools elásticos](resource-limits-dtu-elastic-pools.md) Para limites de recursos vCore, consulte [limites de recursos VCORE-bancos de dados individuais](resource-limits-vcore-single-databases.md) e [limites de recursos VCORE-pools elásticos](resource-limits-vcore-elastic-pools.md). Para obter mais informações sobre os diferentes modelos de compras, consulte [Modelos de compras e camadas de serviço](purchasing-models.md).

## <a name="single-database-storage-sizes-and-compute-sizes"></a>Banco de dados individual: tamanhos de armazenamento e tamanhos da computação

As tabelas a seguir mostram os recursos disponíveis para um único banco de dados individual em cada camada de serviço e tamanho da computação. É possível definir a camada de serviço, o tamanho da computação e a quantidade de armazenamento para um banco de dados individual usando o [portal do Azure](single-database-manage.md#the-azure-portal), o [Transact-SQL](single-database-manage.md#transact-sql-t-sql), o [PowerShell](single-database-manage.md#powershell), a [CLI do Azure](single-database-manage.md#the-azure-cli) ou a [API REST](single-database-manage.md#rest-api).

> [!IMPORTANT]
> Para obter diretrizes e considerações sobre o dimensionamento, consulte [dimensionar um banco de dados individual](single-database-scale.md)

### <a name="basic-service-tier"></a>Camada de serviço Básica

| **Tamanho da computação** | **Basic** |
| :--- | --: |
| Número máximo de DTUs | 5 |
| Armazenamento incluído (GB) | 2 |
| Opções de espaço de armazenamento máximo (GB) | 2 |
| Armazenamento máximo OLTP na memória (GB) |N/D |
| Máximo de trabalhos simultâneos (solicitações) | 30 |
| Máximo de sessões simultâneas | 300 |
|||

> [!IMPORTANT]
> A camada de serviço básica fornece menos de um vCore (CPU).  Para cargas de trabalho com uso intensivo de CPU, é recomendável uma camada de serviço S3 ou superior. 
>
>Em relação ao armazenamento de dados, a camada de serviço básica é colocada em blobs de páginas padrão. Os blobs de páginas padrão usam mídia de armazenamento baseada em HDD (unidade de disco rígido) e são mais adequados para desenvolvimento, teste e outras cargas de trabalho acessadas com pouca frequência que são menos sensíveis à variabilidade de desempenho.
>

### <a name="standard-service-tier"></a>Camada de serviço Standard

| **Tamanho da computação** | **S0** | **S1** | **S2** | **S3** |
| :--- |---:| ---:|---:|---:|
| Número máximo de DTUs | 10 | 20 | 50 | 100 |
| Armazenamento incluído (GB) | 250 | 250 | 250 | 250 |
| Opções de espaço de armazenamento máximo (GB) | 250 | 250 | 250 | 250, 500, 750, 1024 |
| Armazenamento máximo OLTP na memória (GB) | N/D | N/D | N/D | N/D |
| Máximo de trabalhos simultâneos (solicitações)| 60 | 90 | 120 | 200 |
| Máximo de sessões simultâneas |600 | 900 | 1200 | 2400 |
||||||

> [!IMPORTANT]
> As camadas padrão S0, S1 e S2 fornecem menos de um vCore (CPU).  Para cargas de trabalho com uso intensivo de CPU, é recomendável uma camada de serviço S3 ou superior. 
>
>Em relação ao armazenamento de dados, as camadas de serviço S0 e S1 padrão são colocadas em blobs de páginas padrão. Os blobs de páginas padrão usam mídia de armazenamento baseada em HDD (unidade de disco rígido) e são mais adequados para desenvolvimento, teste e outras cargas de trabalho acessadas com pouca frequência que são menos sensíveis à variabilidade de desempenho.
>

### <a name="standard-service-tier-continued"></a>Camada de serviço Standard (continuação)

| **Tamanho da computação** | **S4** | **S6** | **S7** | **S9** | **S12** |
| :--- |---:| ---:|---:|---:|---:|
| Número máximo de DTUs | 200 | 400 | 800 | 1600 | 3000 |
| Armazenamento incluído (GB) | 250 | 250 | 250 | 250 | 250 |
| Opções de espaço de armazenamento máximo (GB) | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 |
| Armazenamento máximo OLTP na memória (GB) | N/D | N/D | N/D | N/D |N/D |
| Máximo de trabalhos simultâneos (solicitações)| 400 | 800 | 1600 | 3200 |6000 |
| Máximo de sessões simultâneas |4800 | 9600 | 19200 | 30000 |30000 |
|||||||

### <a name="premium-service-tier"></a>Camada de serviço Premium

| **Tamanho da computação** | **P1** | **P2** | **P4** | **P6** | **P11** | **P15** |
| :--- |---:|---:|---:|---:|---:|---:|
| Número máximo de DTUs | 125 | 250 | 500 | 1000 | 1750 | 4000 |
| Armazenamento incluído (GB) | 500 | 500 | 500 | 500 | 4096 * | 4096 * |
| Opções de espaço de armazenamento máximo (GB) | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 4096 * | 4096 * |
| Armazenamento máximo OLTP na memória (GB) | 1 | 2 | 4 | 8 | 14 | 32 |
| Máximo de trabalhos simultâneos (solicitações)| 200 | 400 | 800 | 1600 | 2800 | 6400 |
| Máximo de sessões simultâneas | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
|||||||

\*De 1024 GB até 4096 GB em incrementos de 256 GB

> [!IMPORTANT]
> Mais de 1 TB de armazenamento na camada Premium está disponível atualmente em todas as regiões, exceto: Leste da China, Norte da China, Alemanha central, Alemanha nordeste, Oeste EUA Central, regiões de US DoD e central do governo dos EUA. Nessas regiões, o armazenamento máximo na camada Premium é limitado a 1 TB.  Para obter mais informações, confira [Limitações atuais de P11-P15](single-database-scale.md#p11-and-p15-constraints-when-max-size-greater-than-1-tb).  
> [!NOTE]
> Para `tempdb` limites, consulte [limites de tempdb](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database?view=sql-server-2017#tempdb-database-in-sql-database).

## <a name="next-steps"></a>Próximas etapas

- Para limites de recursos vCore para um único banco de dados, confira [limites de recursos para bancos de dados individuais usando o modelo de compra vCore](resource-limits-vcore-single-databases.md)
- Para limites de recursos vCore para pools elásticos, consulte [limites de recursos para pools elásticos usando o modelo de compra vCore](resource-limits-vcore-elastic-pools.md)
- Para os limites de recursos de DTU para pools elásticos, consulte [limites de recursos para pools elásticos usando o modelo de compra de DTU](resource-limits-dtu-elastic-pools.md)
- Para os limites de recursos para instâncias gerenciadas no Azure SQL Instância Gerenciada, consulte [limites de recursos do sql instância gerenciada](../managed-instance/resource-limits.md).
- Para saber mais sobre limites gerais do Azure, confira [Assinatura do Azure e limites de serviço, cotas e restrições](../../azure-resource-manager/management/azure-subscription-service-limits.md).
- Para obter informações sobre os limites de recursos em um SQL Server lógico, consulte [visão geral dos limites de recursos em um servidor SQL lógico](resource-limits-logical-server.md) para obter informações sobre os limites nos níveis de servidor e assinatura.
