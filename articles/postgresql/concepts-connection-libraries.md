---
title: 'Bibliotecas de conexiones en Azure Database for PostgreSQL: servidor único'
description: 'En este artículo se describen varias bibliotecas y controladores que pueden usarse al codificar aplicaciones para conectarse a Azure Database for PostgreSQL: servidor único y realizar consultas en este servicio.'
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 45081c6ba161686498398f2c4ccae8b4cff4c0d1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91704317"
---
# <a name="connection-libraries-for-azure-database-for-postgresql---single-server"></a>Bibliotecas de conexiones de Azure Database for PostgreSQL con un único servidor
En este artículo se enumeran las bibliotecas y los controladores que pueden usar los desarrolladores para desarrollar aplicaciones con el fin de conectarse a Azure Database for PostgreSQL y realizar ahí consultas.

## <a name="client-interfaces"></a>Interfaces de cliente
La mayoría de las bibliotecas de cliente de lenguajes para conectarse al servidor PostgreSQL son proyectos externos y se distribuyen de manera independiente. Las bibliotecas indicadas son compatibles con las plataformas Windows, Linux y Mac para la conexión con Azure Database for PostgreSQL. Se muestran varios ejemplos de inicio rápido en la sección Pasos siguientes.

| **Lenguaje** | **Interfaz de cliente** | **Información adicional** | **Descargar** |
|--------------|----------------------------------------------------------------|-------------------------------------|--------------------------------------------------------------------|
| Python | [psycopg](http://initd.org/psycopg/) | Compatible con API 2.0 de BD | [Descargar](http://initd.org/psycopg/download/) |
| PHP | [php-pgsql](https://secure.php.net/manual/en/book.pgsql.php) | Extensión de base de datos | [Instalación](https://secure.php.net/manual/en/pgsql.installation.php) |
| Node.js | [Paquete de Pg npm](https://www.npmjs.com/package/pg) | Cliente sin bloqueo de JavaScript puro | [Instalación](https://www.npmjs.com/package/pg) |
| Java | [JDBC](https://jdbc.postgresql.org/) | Controlador JDBC tipo 4 | [Descargar](https://jdbc.postgresql.org/download.html)  |
| Ruby | [Pg gem](https://deveiate.org/code/pg/) | Interfaz Ruby | [Descargar](https://rubygems.org/downloads/pg-0.20.0.gem) |
| Go | [Package pq](https://godoc.org/github.com/lib/pq) | Controlador de Postgres de Go puro | [Instalación](https://github.com/lib/pq/blob/master/README.md) |
| C\#/ .NET | [Npgsql](https://www.npgsql.org/) | Proveedor de datos de ADO.NET | [Descargar](https://www.microsoft.com/net/) |
| ODBC | [psqlODBC](https://odbc.postgresql.org/) | Controlador ODBC | [Descargar](https://www.postgresql.org/ftp/odbc/versions/) |
| C | [libpq](https://www.postgresql.org/docs/9.6/static/libpq.html) | Interfaz de idioma C principal | Se incluye |
| C++ | [libpqxx](http://pqxx.org/) | Interfaz de C++ de nuevo estilo | [Descargar](http://pqxx.org/download/software/) |

## <a name="next-steps"></a>Pasos siguientes
Lea estos tutoriales rápidos sobre cómo conectarse a Azure Database for PostgreSQL y realizar consultas ahí mediante el lenguaje de su elección:

[Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) | [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [Go](./connect-go.md)
