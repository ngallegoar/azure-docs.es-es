---
title: 'Directiva de versión de Azure Database for PostgreSQL: servidor único y servidor flexible (versión preliminar)'
description: 'Aquí se describe la directiva relacionada con las versiones principales y secundarias de Postgres en Azure Database for PostgreSQL: servidor único.'
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/03/2020
ms.custom: fasttrack-edit
ms.openlocfilehash: b88d42071beba0ddd5a5627cefbe50229b4d27eb
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93294220"
---
# <a name="azure-database-for-postgresql-versioning-policy"></a>Directiva de versión de Azure Database for PostgreSQL

En esta página se describe la directiva de versión de Azure Database for PostgreSQL y se puede aplicar a los modos de implementación Azure Database for PostgreSQL: servidor único y Azure Database for PostgreSQL: servidor flexible (versión preliminar).

## <a name="supported--postgresql-versions"></a>Versiones de PostgreSQL admitidas

Azure Database for PostgreSQL admite las siguientes versiones de base de datos.

| Versión | Servidor único | Servidor flexible (versión preliminar) |
| ----- | :------: | :----: |
| PostgreSQL 12 |  | X  | 
| PostgreSQL 11 | X | X |
| PostgreSQL 10 | X |  |
| PostgreSQL 9.6 | X |  |
| PostgreSQL 9.5 | X |  |

## <a name="major-version-support"></a>Compatibilidad con la versión principal
Cada versión principal de PostgreSQL será compatible con Azure Database for PostgreSQL a partir de la fecha en la que Azure comience a admitir la versión y hasta que la comunidad de PostgreSQL retire la versión, tal como se proporciona en la [directiva de control de versiones de la comunidad de PostgreSQL](https://www.postgresql.org/support/versioning/).

## <a name="minor-version-support"></a>Compatibilidad con la versión secundaria
Azure Database for PostgreSQL realiza automáticamente actualizaciones de las versiones secundarias a la versión de PostgreSQL preferida de Azure como parte del mantenimiento periódico. 

## <a name="major-version-retirement-policy"></a>Directiva de retirada de la versión principal
En la tabla siguiente se proporcionan los detalles de la retirada de las versiones principales de PostgreSQL. Las fechas siguen la [directiva de versión de la comunidad de PostgreSQL](https://www.postgresql.org/support/versioning/).

| Versión | Novedades | Fecha de inicio del soporte técnico de Azure | Fecha de retirada|
| ----- | ----- | ------ | ----- |
| PostgreSQL 9.5| [Características](https://www.postgresql.org/docs/9.5/release-9-5.html)  | 18 de abril de 2018    | 11 de febrero de 2021
| [PostgreSQL 9.6](https://www.postgresql.org/about/news/postgresql-96-released-1703/) | [Características](https://wiki.postgresql.org/wiki/NewIn96) | 18 de abril de 2018  | 11 de noviembre de 2021
| [PostgreSQL 10](https://www.postgresql.org/about/news/postgresql-10-released-1786/) | [Características](https://wiki.postgresql.org/wiki/New_in_postgres_10) | 4 de junio de 2018  | 10 de noviembre de 2022
| [PostgreSQL 11](https://www.postgresql.org/about/news/postgresql-11-released-1894/) | [Características](https://www.postgresql.org/docs/11/release-11.html) | 24 de julio de 2019  | 9 de noviembre de 2023
| [PostgreSQL 12](https://www.postgresql.org/about/news/postgresql-12-released-1976/) | [Características](https://www.postgresql.org/docs/12/release-12.html) | 22 de septiembre de 2020  | 14 de noviembre de 2024

## <a name="retired-postgresql-engine-versions-not-supported-in-azure-postgresql"></a>Versiones del motor de PostgreSQL retiradas que no son compatibles con Azure PostgreSQL

Después de la fecha de retirada de cada versión de la base de datos de PostgreSQL, si continúa ejecutando la versión retirada, tenga en cuenta las siguientes restricciones:
- Dado que la comunidad no va a publicar correcciones de errores ni correcciones de seguridad adicionales, Azure for PostgreSQL no revisará el motor de base de datos retirado en busca de errores o problemas de seguridad, ni tomará medidas de seguridad relacionadas con el motor de base de datos retirado. Como resultado, puede encontrarse con vulnerabilidades de seguridad u otros problemas. No obstante, Azure continuará realizando tareas periódicas de mantenimiento y revisión para el host, el sistema operativo, los contenedores y cualquier otro componente relacionado con el servicio.
- Si experimenta algún problema de compatibilidad con la base de datos de PostgreSQL, no se le ofrecerá soporte técnico. En tales casos, tendrá que actualizar la base de datos para que se le proporcione soporte técnico.
- No podrá crear nuevos servidores de bases de datos para la versión retirada. Sin embargo, podrá realizar recuperaciones a un momento dado, así como crear réplicas de lectura para los servidores existentes.
- Las nuevas capacidades de servicio que ha desarrollado Azure Database for PostgreSQL podrían solo estar disponibles para las versiones de servidor de bases de datos compatibles.
- Los acuerdos de nivel de servicio de tiempo de actividad solo se aplicarán a los problemas relacionados con los servicios de Azure Database for PostgreSQL, no a los tiempos de inactividad causados por los errores relacionados con el motor.  
- En el caso de una vulnerabilidad de seguridad de base de datos importante que se haya identificado en la versión de base de datos retirada, Azure podrá optar por actualizar automáticamente la base de datos a una versión posterior.

## <a name="postgresql-version-syntax"></a>Sintaxis de la versión de PostgreSQL
Antes de la versión 10 de PostgreSQL, la [directiva de versiones de PostgreSQL](https://www.postgresql.org/support/versioning/) consideraba que una actualización de la _versión principal_ suponía un aumento en el primer _o_ segundo número. Por ejemplo, de 9.5 a 9.6 se consideraba una actualización de la versión _principal_. A partir de la versión 10, solo se considera una actualización de la versión principal un cambio en el primer número. Por ejemplo, de 10.0 a 10.1 es una actualización de versión _secundaria_. De la versión 10 a la 11 se consideraría una actualización de versión _principal_.

## <a name="next-steps"></a>Pasos siguientes
- Consulte las [versiones compatibles](./concepts-supported-versions.md) con Azure Database for PostgreSQL: servidor único.
- Consulte las [versiones compatibles](flexible-server/concepts-supported-versions.md) con Azure Database for PostgreSQL: servidor flexible (versión preliminar).
- Para obtener información sobre cómo realizar actualizaciones de versión principales, consulte la documentación sobre [actualizaciones de versiones principales](how-to-upgrade-using-dump-and-restore.md).
- Para más información sobre las extensiones de PostgreSQL admitidas,consulte el [documento de extensiones](concepts-extensions.md).
