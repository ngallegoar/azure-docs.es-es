---
title: 'Optimización de inserciones masivas en Azure Database for PostgreSQL: servidor único'
description: En este artículo se describe cómo puede optimizar las operaciones de inserción masiva en Azure Database for PostgreSQL con un único servidor.
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 4c4bac16917be0064ebb111328753d378d462a2a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74770142"
---
# <a name="optimize-bulk-inserts-and-use-transient-data-on-an-azure-database-for-postgresql---single-server"></a>Optimización de las inserciones masivas y uso de datos transitorios en Azure Database for PostgreSQL con un único servidor 
En este artículo se describe cómo puede optimizar las operaciones de inserción masiva y usar datos transitorios en un servidor de Azure Database for PostgreSQL.

## <a name="use-unlogged-tables"></a>Uso de tablas sin registrar
Si tiene operaciones de carga de trabajo que impliquen datos transitorios o que inserten grandes conjuntos de datos de forma masiva, debe considerar la posibilidad de usar tablas sin registrar.

Las tablas sin registrar son una característica de PostgreSQL que puede usarse de forma eficaz para optimizar las inserciones masivas. PostgreSQL usa el registro de escritura previa (WAL), que proporciona atomicidad y durabilidad de forma predeterminada. Las propiedades ACID incluyen la Atomicidad, Coherencia, aIslamiento y Durabilidad. 

Realizar inserciones en tablas sin registrar significa que PostgreSQL las realiza sin escribir en el registro de transacciones, que a su vez es una operación de E/S. Como resultado, estas tablas son notablemente más rápidas que las tablas normales.

Para crear una tabla sin registrar, utilice las siguientes opciones:
- Cree una tabla sin registrar con la sintaxis `CREATE UNLOGGED TABLE <tableName>`.
- Convierta una tabla registrada existente en una tabla sin registrar con la sintaxis `ALTER TABLE <tableName> SET UNLOGGED`.  

Para invertir el proceso, use la sintaxis `ALTER TABLE <tableName> SET LOGGED`.

## <a name="unlogged-table-tradeoff"></a>Concesiones de las tablas sin registrar
Las tablas sin registrar no están protegidas contra bloqueos. Una tabla sin registrar se trunca automáticamente después de un bloqueo o está sujeta a un apagado no limpio. Además, el contenido de una tabla sin registrar no se replica en los servidores en espera. Los índices creados en una tabla sin registrar también se quitan del registro automáticamente. Una vez que se complete la operación de inserción, convierta la tabla en registrada para que la inserción sea duradera.

Algunas cargas de trabajo de clientes han experimentado una mejora de rendimiento aproximada del 15 al 20 por ciento al usar tablas sin registrar.

## <a name="next-steps"></a>Pasos siguientes
Revise la carga de trabajo en busca de usos de datos transitorios e inserciones masivas de gran tamaño. Consulte la siguiente documentación de PostgreSQL:
 
- [Comandos de SQL "Create table"](https://www.postgresql.org/docs/current/static/sql-createtable.html)
