---
title: 'Versiones admitidas en Azure Database for PostgreSQL: Servidor flexible'
description: 'Se describen las versiones principales y secundarias de PostgreSQL admitidas en Azure Database for PostgreSQL: Servidor flexible.'
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/16/2020
ms.openlocfilehash: 334eaa88ae927ae61ebb445343494cfdce570563
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2020
ms.locfileid: "92167498"
---
# <a name="supported-postgresql-major-versions-in-azure-database-for-postgresql---flexible-server"></a>Versiones principales de PostgreSQL admitidas en Azure Database for PostgreSQL: Servidor flexible

> [!IMPORTANT]
> Azure Database for PostgreSQL: Servidor flexible en versión preliminar

Azure Database for PostgreSQL: Servidor flexible actualmente admite las versiones principales siguientes:

## <a name="postgresql-version-12"></a>PostgreSQL, versión 12

La versión secundaria actual es la 12.4. Consulte la [documentación de PostgreSQL](https://www.postgresql.org/docs/12/static/release-12-4.html) para obtener más información sobre las mejoras y correcciones de esta versión secundaria.

## <a name="postgresql-version-11"></a>PostgreSQL, versión 11

La versión secundaria actual es la 11.9. Consulte la [documentación de PostgreSQL](https://www.postgresql.org/docs/11/static/release-11-9.html) para obtener más información sobre las mejoras y correcciones de esta versión secundaria.

## <a name="postgresql-version-10-and-older"></a>PostgreSQL, versión 10 y anteriores

No se admite la versión 10 de PostgreSQL ni las anteriores para Azure Database for PostgreSQL: Servidor flexible. Use la opción de implementación [Servidor único](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions) si necesita versiones anteriores.

## <a name="managing-upgrades"></a>Administración de actualizaciones

El proyecto de PostgreSQL publica periódicamente versiones secundarias para corregir errores detectados. Azure Database for PostgreSQL aplica automáticamente revisiones a los servidores con versiones secundarias durante las implementaciones mensuales del servicio.

Todavía no se admite la automatización para la actualización de la versión principal. Por ejemplo, actualmente no hay una actualización automática de PostgreSQL 11 a PostgreSQL 12.<!-- To upgrade to the next major version, create a [database dump and restore](howto-migrate-using-dump-and-restore.md) to a server that was created with the new engine version.-->

<!--
## Next steps

For information on supported PostgreSQL extensions, see [the extensions document](concepts-extensions.md).
-->
