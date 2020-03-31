---
title: 'Administración de réplicas de lectura mediante Azure Portal en Azure Database for PostgreSQL: servidor único'
description: 'Obtenga información sobre cómo administrar réplicas de lectura mediante Azure Portal para Azure Database for PostgreSQL: servidor único.'
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/24/2020
ms.openlocfilehash: dd79618b8d9f016c92166edb9ecdb0bfb113947e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76768961"
---
# <a name="create-and-manage-read-replicas-in-azure-database-for-postgresql---single-server-from-the-azure-portal"></a>Cree y administre mediante Azure Portal réplicas de lectura en el servicio Azure Database for PostgreSQL: servidor único.

En este artículo, obtendrá información sobre cómo crear y administrar las réplicas de lectura en el servicio Azure Database for PostgreSQL mediante Azure Portal. Para más información acerca de las réplicas de lectura, consulte la [introducción](concepts-read-replicas.md).


## <a name="prerequisites"></a>Prerequisites
Un [servidor de Azure Database for PostgreSQL](quickstart-create-server-database-portal.md) que se usará como servidor maestro.

## <a name="prepare-the-master-server"></a>Preparación del servidor maestro
Estos pasos se deben utilizar para preparar un servidor maestro en los niveles de uso general u optimizado para memoria. El servidor maestro se prepara para la replicación mediante el parámetro azure.replication_support. Cuando se cambia el parámetro de replicación, es necesario reiniciar el servidor para que el cambio surta efecto. En Azure Portal, estos dos pasos están encapsulados en un solo botón, denominado **Habilitar compatibilidad con la replicación**.

1. En Azure Portal, seleccione el servidor de Azure Database for PostgreSQL existente para utilizar como servidor maestro.

2. En la barra lateral del servidor, en **CONFIGURACIÓN**, seleccione **Replicación**.

> [!NOTE] 
> Si ve que la opción **Deshabilitar compatibilidad con la replicación** está atenuada, significa que la configuración de replicación ya está establecida en el servidor de forma predeterminada. Puede omitir los pasos siguientes y pasar a crear una réplica de lectura. 

3. Seleccione **Habilitar compatibilidad con la replicación**. 

   ![Habilitar compatibilidad con la replicación](./media/howto-read-replicas-portal/enable-replication-support.png)

4. Confirme que quiere habilitar la compatibilidad con la replicación. Esta operación reiniciará el servidor maestro. 

   ![Confirmación de Habilitar compatibilidad con la replicación](./media/howto-read-replicas-portal/confirm-enable-replication.png)
   
5. Recibirá dos notificaciones de Azure Portal una vez que se haya completado la operación: una notificación sobre la actualización del parámetro de servidor y otra sobre el reinicio del servidor, que se producirá inmediatamente después.

   ![Notificaciones habilitadas sobre operaciones realizadas correctamente](./media/howto-read-replicas-portal/success-notifications-enable.png)

6. Actualice la página de Azure Portal para actualizar la barra de herramientas de replicación. Ahora puede crear réplicas de lectura para este servidor.

   ![Barra de herramientas actualizada](./media/howto-read-replicas-portal/updated-toolbar.png)
   
La habilitación de la compatibilidad con la replicación es una operación que se realiza una sola vez por cada servidor maestro. Para mayor comodidad, se proporciona el botón **Deshabilitar compatibilidad con la replicación**. No se recomienda deshabilitar la compatibilidad con la replicación, a menos que esté seguro de que nunca creará una réplica en este servidor principal. No se puede deshabilitar la compatibilidad con la replicación si el servidor maestro tiene réplicas existentes.


## <a name="create-a-read-replica"></a>Creación de una réplica de lectura
Para crear una réplica de lectura, siga estos pasos:

1. Seleccione el servidor de Azure Database for PostgreSQL existente para utilizar como servidor maestro. 

2. En la barra lateral del servidor, en **CONFIGURACIÓN**, seleccione **Replicación**.

3. Seleccione **Agregar réplica**.

   ![Adición de una réplica](./media/howto-read-replicas-portal/add-replica.png)

4. Escriba un nombre para la réplica de lectura. 

    ![Asignación de un nombre a la réplica](./media/howto-read-replicas-portal/name-replica.png)

5. Seleccione una ubicación para la réplica. La ubicación predeterminada es la misma que la del servidor maestro.

    ![Seleccionar una ubicación](./media/howto-read-replicas-portal/location-replica.png)

   > [!NOTE]
   > Para más información sobre las regiones en las que puede crear una réplica, consulte el [artículo sobre los conceptos de la réplica de lectura](concepts-read-replicas.md). 

6. Seleccione **Aceptar** para confirmar la creación de la réplica.

Una réplica se crea con la misma configuración de proceso y almacenamiento que el servidor maestro. Después de crear una réplica, se pueden cambiar varias configuraciones independientemente del servidor maestro: generación de proceso, núcleos virtuales, almacenamiento y período de retención de copia de seguridad. El plan de tarifa también se puede cambiar de forma independiente, excepto si es con origen o destino en el nivel Básico.

> [!IMPORTANT]
> Antes de actualizar la configuración de un servidor maestro a un nuevo valor, actualice la configuración de réplica a un valor igual o superior. Esta acción ayuda a que la réplica haga frente a los cambios realizados en el servidor maestro.

Después de crear la réplica de lectura, puede verla en la ventana **Replicación**:

![Visualización de la nueva réplica en la ventana Replicación](./media/howto-read-replicas-portal/list-replica.png)
 

## <a name="stop-replication"></a>Detención replicación
Puede detener la replicación entre un servidor maestro y una réplica de lectura.

> [!IMPORTANT]
> Después de detener la replicación en un servidor maestro y en una réplica de lectura, este proceso no se puede deshacer. La réplica de lectura se convierte en un servidor independiente que admite operaciones de lectura y escritura. Este servidor independiente no puede volver a convertirse en una réplica.

Para detener la replicación entre un servidor maestro y una réplica de lectura desde Azure Portal, siga estos pasos:

1. En Azure Portal, seleccione el servidor maestro de Azure Database for PostgreSQL.

2. En el menú del servidor, en **CONFIGURACIÓN**, seleccione **Replicación**.

3. Seleccione el servidor de réplica para el que desea detener la replicación.

   ![Selección de la réplica](./media/howto-read-replicas-portal/select-replica.png)
 
4. Seleccione **Detener replicación**.

   ![Selección de Detener replicación](./media/howto-read-replicas-portal/select-stop-replication.png)
 
5. Seleccione **Aceptar** para detener la replicación.

   ![Confirmación de la detención de la replicación](./media/howto-read-replicas-portal/confirm-stop-replication.png)
 

## <a name="delete-a-master-server"></a>Eliminación de un servidor maestro
Para eliminar un servidor maestro, se usan los mismos pasos que para eliminar un servidor de Azure Database for PostgreSQL independiente. 

> [!IMPORTANT]
> Cuando se elimina un servidor maestro, la replicación se detiene en todas las réplicas de lectura. Las réplicas de lectura se convierten en servidores independientes que ahora admiten tanto lectura como escritura.

Para eliminar un servidor de Azure Portal, siga estos pasos:

1. En Azure Portal, seleccione el servidor maestro de Azure Database for PostgreSQL.

2. Abra la página **Introducción** del servidor. Seleccione **Eliminar**.

   ![En la página Introducción del servidor, selección de Eliminar para eliminar el servidor maestro](./media/howto-read-replicas-portal/delete-server.png)
 
3. Escriba el nombre del servidor maestro que desea eliminar. Seleccione **Eliminar** para confirmar la eliminación del servidor maestro.

   ![Confirmación de la eliminación del servidor maestro](./media/howto-read-replicas-portal/confirm-delete.png)
 

## <a name="delete-a-replica"></a>Eliminación de una réplica
Puede eliminar una réplica de lectura similar a cómo eliminar un servidor maestro.

- En Azure Portal, abra la página **Introducción** para la réplica de lectura. Seleccione **Eliminar**.

   ![En la página Introducción de la réplica, selección de Eliminar para eliminar la réplica](./media/howto-read-replicas-portal/delete-replica.png)
 
También puede eliminar la réplica de lectura desde la ventana **Replicación** siguiendo estos pasos:

1. En Azure Portal, seleccione el servidor maestro de Azure Database for PostgreSQL.

2. En el menú del servidor, en **CONFIGURACIÓN**, seleccione **Replicación**.

3. Seleccione la réplica de lectura que desea eliminar.

   ![Selección de la réplica para eliminar](./media/howto-read-replicas-portal/select-replica.png)
 
4. Seleccione **Eliminar réplica**.

   ![Selección de Eliminar réplica](./media/howto-read-replicas-portal/select-delete-replica.png)
 
5. Escriba el nombre de la réplica que quiere eliminar. Seleccione **Eliminar** para confirmar la eliminación de la réplica.

   ![Confirmación de la eliminación de la réplica](./media/howto-read-replicas-portal/confirm-delete-replica.png)
 

## <a name="monitor-a-replica"></a>Supervisión de una réplica
Están disponibles dos métricas para supervisar las réplicas de lectura.

### <a name="max-lag-across-replicas-metric"></a>Métrica de retraso máximo entre réplicas
La métrica **Max Lag Across Replicas** (Retraso máximo entre réplicas) muestra el retardo en bytes entre la réplica con mayor retardo y el servidor maestro. 

1.  En Azure Portal, seleccione el servidor maestro de Azure Database for PostgreSQL.

2.  Seleccione **Métricas**. En la ventana de **métricas**, seleccione **Max Lag Across Replicas** (Retraso máximo entre réplicas).

    ![Supervisión del retraso máximo entre réplicas](./media/howto-read-replicas-portal/select-max-lag.png)
 
3.  En **Agregación**, seleccione **Máx**.


### <a name="replica-lag-metric"></a>Métrica de retraso de réplica
La métrica **Replica Lag** (Retraso entre réplicas) muestra el tiempo desde la última transacción reproducida en esta réplica. Si no se produce ninguna transacción en el servidor maestro, la métrica refleja este retardo de tiempo.

1. En Azure Portal, seleccione la réplica de lectura de Azure Database for PostgreSQL.

2. Seleccione **Métricas**. En la ventana de **métricas**, seleccione **Replica Lag** (Retraso entre réplicas).

   ![Supervisión del retraso de réplica](./media/howto-read-replicas-portal/select-replica-lag.png)
 
3. En **Agregación**, seleccione **Máx**. 
 
## <a name="next-steps"></a>Pasos siguientes
* Más información sobre las [réplicas de lectura en Azure Database for PostgreSQL](concepts-read-replicas.md).
* Aprenda a [crear y administrar réplicas de lectura en la CLI de Azure y con API REST](howto-read-replicas-cli.md).