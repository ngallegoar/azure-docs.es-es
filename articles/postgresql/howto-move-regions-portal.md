---
title: Traslado de un servidor único de Azure Database for PostgreSQL entre regiones de Azure mediante Azure Portal
description: Mueva un servidor de Azure Database for PostgreSQL de una región de Azure a otra mediante una réplica de lectura y Azure Portal.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 06/29/2020
ms.openlocfilehash: d237d5709f8d2bb47de3e89b0b7103b195376e11
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2020
ms.locfileid: "92489751"
---
# <a name="move-an-azure-database-for-azure-database-for-postgresql---single-server-to-another-region-by-using-the-azure-portal"></a>Traslado de una base de datos de Azure a Azure Database for PostgreSQL: servidor único a otra región mediante Azure Portal

Hay varios escenarios para mover un servidor de Azure Database for PostgreSQL existente de una región a otra. Por ejemplo, es posible que quiera mover un servidor de producción a otra región como parte de la planificación de la recuperación ante desastres.

Puede usar una [réplica de lectura entre regiones](concepts-read-replicas.md#cross-region-replication) de Azure Database for PostgreSQL para completar el traslado a otra región. Para ello, primero cree una réplica de lectura en la región de destino. A continuación, detenga la replicación en el servidor de réplica de lectura para convertirlo en un servidor independiente que acepte el tráfico tanto de lectura como de escritura. 

> [!NOTE]
> Este artículo se centra en el traslado del servidor a otra región. Si desea mover el servidor a otro grupo de recursos o suscripción, consulte el artículo de [traslado](../azure-resource-manager/management/move-resource-group-and-subscription.md). 

## <a name="prerequisites"></a>Requisitos previos

- La característica de réplica de lectura entre regiones solo está disponible para servidores únicos de Azure Database for PostgreSQL en los planes de tarifa de uso general u optimizada para memoria. Asegúrese de que el servidor de origen esté en uno de estos planes de tarifa.

- Asegúrese de que el servidor de origen de Azure Database for PostgreSQL se encuentre en la región de Azure desde la que desea moverlo.

## <a name="prepare-to-move"></a>Preparación para la migración

Para preparar el servidor de origen para la replicación mediante Azure Portal, siga estos pasos: 

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com/).
1. Seleccione el servidor de Azure Database for PostgreSQL existente que desea utilizar como servidor de origen. Esta acción abre la página **Información general** .
1. En el menú del servidor, seleccione **Replicación** . Si la compatibilidad con la replicación de Azure está establecida como mínimo en **Réplica** , puede crear réplicas de lectura. 
1. Si la compatibilidad con la replicación de Azure no está establecida como mínimo en **Réplica** , establézcala. Seleccione **Guardar** .
1. Reinicie el servidor para aplicar el cambio seleccionando **Sí** .
1. Recibirá dos notificaciones de Azure Portal una vez que se haya completado la operación: una notificación sobre la actualización del parámetro de servidor y otra sobre el reinicio del servidor, que se producirá inmediatamente después.
1. Actualice la página de Azure Portal para actualizar la barra de herramientas de replicación. Ahora puede crear réplicas de lectura para este servidor.

Para crear un servidor de réplica de lectura entre regiones en la región de destino mediante Azure Portal, siga estos pasos:

1. Seleccione el servidor de Azure Database for PostgreSQL existente que desea utilizar como servidor de origen.
1. Seleccione **Replicación** en el menú, en **CONFIGURACIÓN** .
1. Seleccione **Agregar réplica** .
1. Escriba un nombre para el servidor de réplica.
1. Seleccione la ubicación del servidor de réplica. La ubicación predeterminada es la misma que la del servidor principal. Verifique haber seleccionado la ubicación de destino en la que desea implementar la réplica.
1. Seleccione **Aceptar** para confirmar la creación de la réplica. Durante la creación de la réplica, los datos se copian del servidor de origen a la réplica. El tiempo de creación puede tardar varios minutos o más, en proporción con el tamaño del servidor de origen.

>[!NOTE]
> Al crear una réplica, no se heredan las reglas de firewall ni los puntos de conexión de servicio de red virtual del servidor principal. Estas reglas se deben configurar de forma independiente para la réplica.

## <a name="move"></a>Move

> [!IMPORTANT]
> Este servidor independiente no puede volver a convertirse en una réplica.
> Asegúrese de que la réplica tenga todos los datos que necesita antes de detener la replicación en una réplica de lectura.

Para detener la replicación en la réplica desde Azure Portal, siga estos pasos:

1. Una vez que se haya creado la réplica, busque y seleccione el servidor de origen de Azure Database for PostgreSQL. 
1. Seleccione **Replicación** en el menú, en **CONFIGURACIÓN** .
1. Seleccione el servidor de réplica.
1. Seleccione **Detener replicación** .
1. Para confirmar que desea detener la replicación, haga clic en **Aceptar** .

## <a name="clean-up-source-server"></a>Limpieza del servidor de origen

Puede que desee eliminar el servidor de Azure Database for PostgreSQL de origen. Para ello, siga estos pasos:

1. Una vez que se haya creado la réplica, busque y seleccione el servidor de origen de Azure Database for PostgreSQL.
1. En la ventana **Información general** , seleccione **Eliminar** .
1. Escriba el nombre del servidor de origen para confirmar que desea eliminarlo.
1. Seleccione **Eliminar** .

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha trasladado un servidor de Azure Database for PostgreSQL de una región a otra mediante Azure Portal y, a continuación, ha limpiado los recursos de origen innecesarios. 

- Obtenga más información sobre [las réplicas de lectura](concepts-read-replicas.md)
- Obtenga más información sobre la [administración de réplicas de lectura en Azure Portal](howto-read-replicas-portal.md)
- Más información sobre las opciones de [continuidad del negocio](concepts-business-continuity.md).