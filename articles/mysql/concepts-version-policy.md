---
title: 'Directiva de versión de Azure Database for MySQL: servidor único y servidor flexible (versión preliminar)'
description: Aquí se describe la directiva relacionada con las versiones principales y secundarias de MySQL en Azure Database for MySQL
author: sr-msft
ms.author: srranga
ms.service: mysql
ms.topic: conceptual
ms.date: 11/03/2020
ms.custom: fasttrack-edit
ms.openlocfilehash: 0670107d84374589aa60cc18f184b9b3d3facce1
ms.sourcegitcommit: 46c5ffd69fa7bc71102737d1fab4338ca782b6f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "94331811"
---
# <a name="azure-database-for-mysql-versioning-policy"></a>Directiva de versión de Azure Database for MySQL

En esta página se describe la directiva de versión de Azure Database for MySQL y se puede aplicar a los modos de implementación Azure Database for MySQL: servidor único y Azure Database for MySQL: servidor flexible (versión preliminar).

## <a name="supported--mysql-versions"></a>Versiones de MySQL admitidas

Azure Database for MySQL admite las siguientes versiones de base de datos.

| Versión | Servidor único | Servidor flexible (versión preliminar) |
| ----- | :------: | :----: |
| MySQL 8 | X |  | 
| MySQL 5.7 | X | X |
| MySQL 5.6| X |  |


## <a name="major-version-support"></a>Compatibilidad con la versión principal
Cada versión principal de MySQL será compatible con Azure Database for MySQL a partir de la fecha en la que Azure comience a admitir la versión y hasta que la comunidad de MySQL retire la versión, tal como se proporciona en la [directiva de versión](https://en.wikipedia.org/wiki/mysql).

## <a name="minor-version-support"></a>Compatibilidad con la versión secundaria
Azure Database for MySQL realiza automáticamente actualizaciones de las versiones secundarias a la versión de MySQL preferida de Azure como parte del mantenimiento periódico. 

## <a name="major-version-retirement-policy"></a>Directiva de retirada de la versión principal
En la tabla siguiente se proporcionan los detalles de la retirada de las versiones principales de MySQL. Las fechas siguen la [directiva de versión de MySQL](https://www.mysql.com/support/eol-notice.html).

| Versión | Novedades | Fecha de inicio del soporte técnico de Azure | Fecha de retirada|
| ----- | ----- | ------ | ----- |
| [MySQL 5.6](https://dev.mysql.com/doc/relnotes/mysql/5.6/)| [Características](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-49.html)  | 20 de marzo de 2018 | Febrero de 2021
| [MySQL 5.7](https://dev.mysql.com/doc/relnotes/mysql/5.7/) | [Características](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-31.html) | 20 de marzo de 2018 | Octubre de 2023
| [MySQL 8](https://mysqlserverteam.com/whats-new-in-mysql-8-0-generally-available/) | [Características](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-21.html) | 11 de diciembre de 2019 | Abril de 2026


## <a name="retired-mysql-engine-versions-not-supported-in-azure-database-for-mysql"></a>Versiones del motor de MySQL retiradas que no se admiten en Azure Database for MySQL

Después de la fecha de retirada de cada versión de la base de datos de MySQL, si continúa ejecutando la versión retirada, tenga en cuenta las siguientes restricciones:
- Dado que la comunidad no va a publicar más correcciones de errores ni correcciones de seguridad, Azure Database for MySQL no revisará el motor de base de datos retirado en busca de errores o problemas de seguridad, ni tomará medidas de seguridad relacionadas con el motor de base de datos retirado. No obstante, Azure continuará realizando tareas periódicas de mantenimiento y revisión para el host, el sistema operativo, los contenedores y cualquier otro componente relacionado con el servicio.
- Si experimenta algún problema de compatibilidad con la base de datos de MySQL, quizá no le podamos proporcionar soporte técnico. En tales casos, tendrá que actualizar la base de datos para que se le proporcione soporte técnico.
- No podrá crear nuevos servidores de bases de datos para la versión retirada. Sin embargo, podrá realizar recuperaciones a un momento dado, así como crear réplicas de lectura para los servidores existentes.
- Las nuevas funcionalidades de servicio que ha desarrollado Azure Database for MySQL podrían solo estar disponibles para las versiones de servidor de bases de datos admitidas.
- Los acuerdos de nivel de servicio de tiempo de actividad solo se aplicarán a los problemas relacionados con los servicios de Azure Database for MySQL, no a los tiempos de inactividad causados por los errores relacionados con el motor.  
- En el caso de una amenaza grave para el servicio causada por la vulnerabilidad del motor de base de datos MySQL identificada en la versión de base de datos retirada, Azure puede optar por detener el nodo de proceso del servidor de bases de datos para proteger primero el servicio. Se le pedirá que actualice el servidor antes de ponerlo en línea. Durante el proceso de actualización, los datos siempre se protegerán mediante copias de seguridad automáticas realizadas en el servicio que se pueden usar para restaurar la versión anterior si se desea. 



## <a name="next-steps"></a>Pasos siguientes
- Consulte las [versiones admitidas](./concepts-supported-versions.md) en Azure Database for MySQL: servidor único.
- Consulte las [versiones admitidas](flexible-server/concepts-supported-versions.md) en Azure Database for MySQL: servidor flexible (versión preliminar).
- Consulte el artículo sobre [volcado y restauración](./concepts-migrate-dump-restore.md) de MySQL para realizar actualizaciones.
