---
title: Bibliotecas de conexões-banco de dados do Azure para MySQL
description: Este artigo lista cada biblioteca ou driver que os programas de cliente podem usar ao se conectarem ao Banco de Dados do Azure para MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 5520072ba16b117c33c2ad172030807e51c7cf7a
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86203896"
---
# <a name="connection-libraries-for-azure-database-for-mysql"></a>Bibliotecas de conexão para o Banco de Dados do Azure para MySQL
Este artigo lista cada biblioteca ou driver que os programas de cliente podem usar ao se conectarem ao Banco de Dados do Azure para MySQL.

## <a name="client-interfaces"></a>Interfaces de cliente
O MySQL oferece conectividade de driver de banco de dados padrão para usar o MySQL com aplicativos e ferramentas compatíveis com ODBC e JDBC padrões do setor. Qualquer sistema que funciona com ODBC ou JDBC pode usar o MySQL.

| **Idioma** | **Plataforma** | **Recursos adicionais** | **Download** |
| :----------- | :------------| :-----------------------| :------------|
| PHP | Windows, Linux | [Driver nativo do MySQL para PHP - mysqlnd](https://dev.mysql.com/downloads/connector/php-mysqlnd/) | [Download](https://secure.php.net/downloads.php) |
| ODBC | Plataformas Windows, Linux, Mac OS X e Unix | [Guia do desenvolvedor do MySQL Connector/ODBC](https://dev.mysql.com/doc/connector-odbc/en/) | [Download](https://dev.mysql.com/downloads/connector/odbc/) |
| ADO.NET | Windows | [Guia do desenvolvedor do MySQL Connector/Net](https://dev.mysql.com/doc/connector-net/en/) | [Download](https://dev.mysql.com/downloads/connector/net/) |
| JDBC | Independente de plataforma | [Guia do desenvolvedor do MySQL Connector/J 5.1](https://dev.mysql.com/doc/connector-j/5.1/en/) | [Download](https://dev.mysql.com/downloads/connector/j/) |
| Node.js | Windows, Linux, Mac OS X | [sidorares/node-mysql2](https://github.com/sidorares/node-mysql2/tree/master/documentation) | [Download](https://github.com/sidorares/node-mysql2) |
| Python | Windows, Linux, Mac OS X | [Guia do desenvolvedor do MySQL Connector/Python](https://dev.mysql.com/doc/connector-python/en/) | [Download](https://dev.mysql.com/downloads/connector/python/) |
| C++ | Windows, Linux, Mac OS X | [Guia do desenvolvedor do MySQL Connector/C++](https://dev.mysql.com/doc/connector-cpp/en/) | [Download](https://dev.mysql.com/downloads/connector/python/) |
| C | Windows, Linux, Mac OS X | [Guia do desenvolvedor do MySQL Connector/C](https://dev.mysql.com/doc/refman/8.0/en/c-api.html) | [Download](https://dev.mysql.com/downloads/connector/c/)
| Perl | Plataformas Windows, Linux, Mac OS X e Unix | [DBD::MySQL](https://metacpan.org/pod/DBD::mysql) | [Download](https://metacpan.org/pod/DBD::mysql) |


## <a name="next-steps"></a>Próximas etapas
Leia estes guias de início rápido para saber como se conectar e consultar o Banco de Dados do Azure para MySQL usando a linguagem de sua escolha:

- [PHP](./connect-php.md)
- [Java](./connect-java.md)
- [.NET (C#)](./connect-csharp.md)
- [Python](./connect-python.md)
- [Node.JS](./connect-nodejs.md)
- [Ruby](./connect-ruby.md)
- [C++](connect-cpp.md)
- [Go](./connect-go.md)
