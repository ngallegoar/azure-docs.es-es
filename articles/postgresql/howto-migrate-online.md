---
title: Migración con tiempo de inactividad mínimo a Azure Database for PostgreSQL con un único servidor
description: En este artículo se describe cómo realizar una migración con tiempo de inactividad mínimo de una base de datos de PostgreSQL a Azure Database for PostgreSQL con un único servidor mediante Azure Database Migration Service.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: how-to
ms.date: 5/6/2019
ms.openlocfilehash: 27da5f1b731b2cdb0604f91f7f9e78b19ee2908b
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2020
ms.locfileid: "92489802"
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql---single-server"></a>Migración con tiempo de inactividad mínimo a Azure Database for PostgreSQL con un único servidor
[!INCLUDE[applies-to-postgres-single-flexible-server](includes/applies-to-postgres-single-flexible-server-hyperscale.md)]

Puede realizar migraciones de PostgreSQL a Azure Database for PostgreSQL con un tiempo de inactividad mínimo mediante la **funcionalidad de sincronización continua** recién introducida para el [servicio Azure Database Migration Service](https://aka.ms/get-dms) (DMS). Esta funcionalidad limita la cantidad de tiempo de inactividad en el que incurre la aplicación.

## <a name="overview"></a>Información general
Azure DMS realiza una carga inicial de la base de datos local en Azure Database for PostgreSQL y, a continuación, realiza la sincronización continua de todas las transacciones nuevas de Azure mientras la aplicación se siga ejecutando. Una vez que los datos se ponen al día en el destino de Azure, puede detener la aplicación por un breve período de tiempo (tiempo de inactividad mínimo), esperar al último lote de datos (desde el momento en que detiene la aplicación hasta el momento en que la aplicación deja de estar disponible efectivamente para aceptar cualquier tráfico nuevo) para ponerse al día en el destino y, a continuación, actualizar la cadena de conexión para que apunte a Azure. Cuando haya terminado, la aplicación estará funcionando en Azure.

:::image type="content" source="./media/howto-migrate-online/ContinuousSync.png" alt-text="Sincronización continua con Azure Database Migration Service":::

## <a name="next-steps"></a>Pasos siguientes
- Vea el vídeo sobre [modernización de aplicaciones con Microsoft Azure](https://medius.studios.ms/Embed/Video/BRK2102?sid=BRK2102), que contiene una demostración que muestra cómo migrar aplicaciones de PostgreSQL a Azure Database for PostgreSQL.
- Vea el tutorial [Migración de PostgreSQL a Azure Database for PostgreSQL en línea mediante DMS](../dms/tutorial-postgresql-azure-postgresql-online.md).