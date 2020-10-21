---
title: 'Administración de réplicas de lectura mediante Azure Portal en Azure Database for PostgreSQL: servidor único'
description: 'Obtenga información sobre cómo administrar réplicas de lectura mediante Azure Portal para Azure Database for PostgreSQL: servidor único.'
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: how-to
ms.date: 07/10/2020
ms.openlocfilehash: 08d1d393b4ba52e6feeb36c0538f2664e1407d38
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91708295"
---
# <a name="create-and-manage-read-replicas-in-azure-database-for-postgresql---single-server-from-the-azure-portal"></a>Cree y administre mediante Azure Portal réplicas de lectura en el servicio Azure Database for PostgreSQL: servidor único.

En este artículo, obtendrá información sobre cómo crear y administrar las réplicas de lectura en el servicio Azure Database for PostgreSQL mediante Azure Portal. Para más información acerca de las réplicas de lectura, consulte la [introducción](concepts-read-replicas.md).


## <a name="prerequisites"></a>Requisitos previos
Un [servidor de Azure Database for PostgreSQL](quickstart-create-server-database-portal.md) que se usará como servidor principal.

## <a name="azure-replication-support"></a>Soporte de replicación de Azure

Tanto[las réplicas de lectura](concepts-read-replicas.md) como [la descodificación lógica](concepts-logical.md) dependen del registro de escritura previa (WAL) Postgres para obtener información. Estas dos características necesitan diferentes niveles de registro de Postgres. La descodificación lógica requiere un mayor nivel de registro que las réplicas de lectura.

Para configurar el nivel de registro adecuado, use el parámetro de soporte de replicación de Azure. El soporte de la replicación de Azure tiene tres opciones de valor:

* **Desactivado**: coloca la más mínima información en el WAL. Este valor no está disponible en la mayoría de los servidores Azure Database for PostgreSQL.  
* **Réplica**: más detallado que **Off**. Este es el nivel mínimo de registro necesario para que [las réplicas de lectura](concepts-read-replicas.md) funcionen. Esta es la configuración predeterminada en la mayoría de los servidores.
* **Lógico**: más detallado que **Réplica**. Este es el nivel mínimo de registro para que funcione la descodificación lógica. Las réplicas de lectura también funcionan con este valor.

El servidor debe reiniciarse después de un cambio de este parámetro. Internamente, este parámetro establece los parámetros Postgres `wal_level`, `max_replication_slots` y `max_wal_senders`.

## <a name="prepare-the-primary-server"></a>Preparación del servidor principal

1. En Azure Portal, seleccione un servidor de Azure Database for PostgreSQL existente para utilizar como servidor maestro.

2. En el menú del servidor, seleccione **Replicación**. Si la compatibilidad con la replicación de Azure está establecida como mínimo en **Réplica**, puede crear réplicas de lectura. 

3. Si la compatibilidad con la replicación de Azure no está establecida como mínimo en **Réplica**, establézcala. Seleccione **Guardar**.

   :::image type="content" source="./media/howto-read-replicas-portal/set-replica-save.png" alt-text="Réplica de Azure Database for PostgreSQL: establecer réplica y guardarla":::

4. Reinicie el servidor para aplicar el cambio seleccionando **Sí**.

   :::image type="content" source="./media/howto-read-replicas-portal/confirm-restart.png" alt-text="Réplica de Azure Database for PostgreSQL: establecer réplica y guardarla":::

5. Recibirá dos notificaciones de Azure Portal una vez que se haya completado la operación: una notificación sobre la actualización del parámetro de servidor y otra sobre el reinicio del servidor, que se producirá inmediatamente después.

   :::image type="content" source="./media/howto-read-replicas-portal/success-notifications.png" alt-text="Réplica de Azure Database for PostgreSQL: establecer réplica y guardarla":::

6. Actualice la página de Azure Portal para actualizar la barra de herramientas de replicación. Ahora puede crear réplicas de lectura para este servidor.
   

## <a name="create-a-read-replica"></a>Creación de una réplica de lectura
Para crear una réplica de lectura, siga estos pasos:

1. Seleccione el servidor de Azure Database for PostgreSQL existente para utilizar como servidor principal. 

2. En la barra lateral del servidor, en **CONFIGURACIÓN**, seleccione **Replicación**.

3. Seleccione **Agregar réplica**.

   :::image type="content" source="./media/howto-read-replicas-portal/add-replica.png" alt-text="Réplica de Azure Database for PostgreSQL: establecer réplica y guardarla":::

4. Escriba un nombre para la réplica de lectura. 

    :::image type="content" source="./media/howto-read-replicas-portal/name-replica.png" alt-text="Réplica de Azure Database for PostgreSQL: establecer réplica y guardarla":::

5. Seleccione una ubicación para la réplica. La ubicación predeterminada es la misma que la del servidor principal.

    :::image type="content" source="./media/howto-read-replicas-portal/location-replica.png" alt-text="Réplica de Azure Database for PostgreSQL: establecer réplica y guardarla":::

   > [!NOTE]
   > Para más información sobre las regiones en las que puede crear una réplica, consulte el [artículo sobre los conceptos de la réplica de lectura](concepts-read-replicas.md). 

6. Seleccione **Aceptar** para confirmar la creación de la réplica.

Después de crear la réplica de lectura, puede verla en la ventana **Replicación**:

:::image type="content" source="./media/howto-read-replicas-portal/list-replica.png" alt-text="Réplica de Azure Database for PostgreSQL: establecer réplica y guardarla":::
 

> [!IMPORTANT]
> Revise la [sección sobre las consideraciones de la información general de Réplicas de lectura](concepts-read-replicas.md#considerations).
>
> Antes de actualizar la configuración de un servidor principal a un nuevo valor, actualice la configuración de réplica a un valor igual o superior. Esta acción ayuda a que la réplica haga frente a los cambios realizados en el servidor maestro.

## <a name="stop-replication"></a>Detención replicación
Puede detener la replicación entre un servidor principal y una réplica de lectura.

> [!IMPORTANT]
> Después de detener la replicación en un servidor principal y en una réplica de lectura, este proceso no se puede deshacer. La réplica de lectura se convierte en un servidor independiente que admite operaciones de lectura y escritura. Este servidor independiente no puede volver a convertirse en una réplica.

Para detener la replicación entre un servidor principal y una réplica de lectura desde Azure Portal, siga estos pasos:

1. En Azure Portal, seleccione el servidor principal de Azure Database for PostgreSQL.

2. En el menú del servidor, en **CONFIGURACIÓN**, seleccione **Replicación**.

3. Seleccione el servidor de réplica para el que desea detener la replicación.

   :::image type="content" source="./media/howto-read-replicas-portal/select-replica.png" alt-text="Réplica de Azure Database for PostgreSQL: establecer réplica y guardarla":::
 
4. Seleccione **Detener replicación**.

   :::image type="content" source="./media/howto-read-replicas-portal/select-stop-replication.png" alt-text="Réplica de Azure Database for PostgreSQL: establecer réplica y guardarla":::
 
5. Seleccione **Aceptar** para detener la replicación.

   :::image type="content" source="./media/howto-read-replicas-portal/confirm-stop-replication.png" alt-text="Réplica de Azure Database for PostgreSQL: establecer réplica y guardarla":::
 

## <a name="delete-a-primary-server"></a>Eliminación de un servidor principal
Para eliminar un servidor principal, se usan los mismos pasos que para eliminar un servidor de Azure Database for PostgreSQL independiente. 

> [!IMPORTANT]
> Cuando se elimina un servidor principal, la replicación se detiene en todas las réplicas de lectura. Las réplicas de lectura se convierten en servidores independientes que ahora admiten tanto lectura como escritura.

Para eliminar un servidor de Azure Portal, siga estos pasos:

1. En Azure Portal, seleccione el servidor principal de Azure Database for PostgreSQL.

2. Abra la página **Introducción** del servidor. Seleccione **Eliminar**.

   :::image type="content" source="./media/howto-read-replicas-portal/delete-server.png" alt-text="Réplica de Azure Database for PostgreSQL: establecer réplica y guardarla":::
 
3. Escriba el nombre del servidor principal que desea eliminar. Seleccione **Eliminar** para confirmar la eliminación del servidor principal.

   :::image type="content" source="./media/howto-read-replicas-portal/confirm-delete.png" alt-text="Réplica de Azure Database for PostgreSQL: establecer réplica y guardarla":::
 

## <a name="delete-a-replica"></a>Eliminación de una réplica
Puede eliminar una réplica de lectura similar a cómo eliminar un servidor principal.

- En Azure Portal, abra la página **Introducción** para la réplica de lectura. Seleccione **Eliminar**.

   :::image type="content" source="./media/howto-read-replicas-portal/delete-replica.png" alt-text="Réplica de Azure Database for PostgreSQL: establecer réplica y guardarla":::
 
También puede eliminar la réplica de lectura desde la ventana **Replicación** siguiendo estos pasos:

1. En Azure Portal, seleccione el servidor principal de Azure Database for PostgreSQL.

2. En el menú del servidor, en **CONFIGURACIÓN**, seleccione **Replicación**.

3. Seleccione la réplica de lectura que desea eliminar.

   :::image type="content" source="./media/howto-read-replicas-portal/select-replica.png" alt-text="Réplica de Azure Database for PostgreSQL: establecer réplica y guardarla":::
 
4. Seleccione **Eliminar réplica**.

   :::image type="content" source="./media/howto-read-replicas-portal/select-delete-replica.png" alt-text="Réplica de Azure Database for PostgreSQL: establecer réplica y guardarla":::
 
5. Escriba el nombre de la réplica que quiere eliminar. Seleccione **Eliminar** para confirmar la eliminación de la réplica.

   :::image type="content" source="./media/howto-read-replicas-portal/confirm-delete-replica.png" alt-text="Réplica de Azure Database for PostgreSQL: establecer réplica y guardarla":::
 

## <a name="monitor-a-replica"></a>Supervisión de una réplica
Están disponibles dos métricas para supervisar las réplicas de lectura.

### <a name="max-lag-across-replicas-metric"></a>Métrica de retraso máximo entre réplicas
La métrica **Max Lag Across Replicas** (Retraso máximo entre réplicas) muestra el retardo en bytes entre la réplica con mayor retardo y el servidor principal. 

1.  En Azure Portal, seleccione el servidor principal de Azure Database for PostgreSQL.

2.  Seleccione **Métricas**. En la ventana de **métricas**, seleccione **Max Lag Across Replicas** (Retraso máximo entre réplicas).

    :::image type="content" source="./media/howto-read-replicas-portal/select-max-lag.png" alt-text="Réplica de Azure Database for PostgreSQL: establecer réplica y guardarla":::
 
3.  En **Agregación**, seleccione **Máx**.


### <a name="replica-lag-metric"></a>Métrica de retraso de réplica
La métrica **Replica Lag** (Retraso entre réplicas) muestra el tiempo desde la última transacción reproducida en esta réplica. Si no se produce ninguna transacción en el servidor maestro, la métrica refleja este retardo de tiempo.

1. En Azure Portal, seleccione la réplica de lectura de Azure Database for PostgreSQL.

2. Seleccione **Métricas**. En la ventana de **métricas**, seleccione **Replica Lag** (Retraso entre réplicas).

   :::image type="content" source="./media/howto-read-replicas-portal/select-replica-lag.png" alt-text="Réplica de Azure Database for PostgreSQL: establecer réplica y guardarla":::
 
3. En **Agregación**, seleccione **Máx**. 
 
## <a name="next-steps"></a>Pasos siguientes
* Más información sobre las [réplicas de lectura en Azure Database for PostgreSQL](concepts-read-replicas.md).
* Aprenda a [crear y administrar réplicas de lectura en la CLI de Azure y con API REST](howto-read-replicas-cli.md).