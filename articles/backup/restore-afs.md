---
title: Restauración de recursos compartidos de archivos de Azure
description: Aprenda a usar Azure Portal para restaurar un recurso compartido de archivos completo o archivos específicos desde un punto de restauración creado por Azure Backup.
ms.topic: conceptual
ms.date: 01/12/2020
ms.openlocfilehash: c22078ebd89f5f6f8299e1424d4d9e21edce8b92
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77586958"
---
# <a name="restore-azure-file-shares"></a>Restauración de recursos compartidos de archivos de Azure

En este artículo se explica cómo usar Azure Portal para restaurar un recurso compartido de archivos completo o archivos específicos desde un punto de restauración creado por [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview).

En este artículo, aprenderá a:

* Restauración de un recurso compartido de archivos de Azure completo.
* Restauración de archivos o carpetas individuales.
* Seguimiento del estado de la operación de restauración.

## <a name="steps-to-perform-a-restore-operation"></a>Pasos para realizar una operación de restauración

Para realizar una operación de restauración, siga estos pasos.

### <a name="select-the-file-share-to-restore"></a>Selección del recurso compartido de archivos que se va a restaurar

1. En [Azure Portal](https://portal.azure.com/), abra el almacén de Recovery Services que usó para configurar la copia de seguridad del recurso compartido de archivos.

1. Seleccione **Elementos de copia de seguridad** en la sección **Elementos protegidos** de la hoja Información general.

    ![Selección de elementos de copia de seguridad](./media/restore-afs/backup-items.png)

1. Después de seleccionar **Elementos de copia de seguridad**, aparece un nuevo panel que muestra todos los tipos de administración de copia de seguridad junto al panel de información general.

    ![Tipos de administración de copias de seguridad](./media/restore-afs/backup-management.png)

1. En el panel **Elementos de copia de seguridad**, en **Tipo de administración de copias de seguridad**, seleccione **Azure Storage (Azure Files)** . Verá una lista de todos los recursos compartidos de archivos y las cuentas de almacenamiento correspondientes cuya copia de seguridad se ha realizado mediante este almacén.

    ![Lista de todos los recursos compartidos de archivos](./media/restore-afs/file-shares.png)

1. En la lista de recursos compartidos de archivos de Azure, seleccione el recurso compartido de archivos para el que quiere realizar la operación de restauración.

### <a name="full-share-recovery"></a>Recuperación del recurso compartido completo

Puede usar esta opción para restaurar el recurso compartido de archivos completo en la ubicación original o en otra alternativa.

1. Seleccione la opción **Restaurar recurso compartido** en el panel **Elemento de copia de seguridad** que aparece después de seleccionar el recurso compartido de archivos que quiere restaurar en el paso 5 de la sección [Selección del recurso compartido de archivos que se va a restaurar](#select-the-file-share-to-restore).

   ![Selección de Restaurar el recurso compartido](./media/restore-afs/restore-share.png)

1. Después de seleccionar **Restaurar recurso compartido**, se abre la hoja **Restaurar** con el menú **Punto de restauración**, que muestra la lista de puntos de restauración disponibles para el recurso compartido de archivos seleccionado.

1. Seleccione el punto de restauración que desee usar para la operación de restauración y seleccione **Aceptar**.

    ![Seleccionar punto de restauración](./media/restore-afs/restore-point.png)

1. Después de seleccionar **Aceptar**, el menú del panel **Restaurar** cambia a **Ubicación de restauración**. En **Ubicación de restauración**, especifique dónde o cómo quiere restaurar los datos. Seleccione una de las dos opciones siguientes:

    * **Ubicación original**: restaure el recurso compartido de archivos completo en la misma ubicación que el origen inicial.
    * **Ubicación alternativa**: restaure el recurso compartido de archivos completo en una ubicación alternativa y mantenga el recurso compartido de archivos original como está.

#### <a name="restore-to-the-original-location"></a>Restauración en la ubicación original

1. Seleccione **Ubicación original** como **Destino de recuperación** y seleccione si desea omitir o sobrescribir si hay conflictos. Después de seleccionar la opción correcta, seleccione **Aceptar**.

    ![Seleccionar la ubicación original](./media/restore-afs/original-location.png)

1. Seleccione **Restaurar** para iniciar la operación de restauración.

    ![Seleccionar Restaurar para iniciar](./media/restore-afs/click-restore.png)

#### <a name="restore-to-an-alternate-location"></a>Restauración a una ubicación alternativa

1. Seleccione **Ubicación alternativa** como **Destino de recuperación**.
1. Seleccione la cuenta de almacenamiento de destino en la que desea restaurar el contenido del que se ha realizado una copia de seguridad, en la lista desplegable **Cuenta de almacenamiento**.
1. En la lista desplegable **Seleccionar recurso compartido de archivos**, se muestran los recursos compartidos de archivos presentes en la cuenta de almacenamiento seleccionada en el paso 2. Seleccione el recurso compartido de archivos en el que desea restaurar el contenido de la copia de seguridad.
1. En el cuadro **Nombre de la carpeta**, especifique el nombre de la carpeta que desea crear en el recurso compartido de archivos de destino con el contenido restaurado.
1. Seleccione si desea omitir o sobrescribir si hay conflictos.
1. Después de escribir los valores adecuados en todos los cuadros, seleccione **Aceptar**.

    ![Selección de Ubicación alternativa](./media/restore-afs/alternate-location.png)

1. Seleccione **Restaurar** para iniciar la operación de restauración.

    ![Seleccionar Restaurar para iniciar](./media/restore-afs/click-restore.png)

### <a name="item-level-recovery"></a>Recuperación a nivel de elemento

Puede usar esta opción para restaurar archivos o carpetas en la ubicación original o en otra alternativa.

1. Seleccione la opción **Recuperación de archivos** en el panel **Elemento de copia de seguridad** que aparece después de seleccionar el recurso compartido de archivos que quiere restaurar en el paso 5 de la sección [Selección del recurso compartido de archivos que se va a restaurar](#select-the-file-share-to-restore).

    ![Selección de Recuperación de archivos](./media/restore-afs/file-recovery.png)

1. Después de seleccionar **Recuperación de archivos**, se abre la hoja **Restaurar** con el menú **Punto de restauración**, que muestra la lista de puntos de restauración disponibles para el recurso compartido de archivos seleccionado.

1. Seleccione el punto de restauración que desee usar para la operación de restauración y seleccione **Aceptar**.

    ![Seleccionar punto de restauración](./media/restore-afs/restore-point.png)

1. Después de seleccionar **Aceptar**, el menú del panel Restaurar cambia a **Ubicación de restauración**. En **Ubicación de restauración**, especifique dónde o cómo quiere restaurar los datos. Seleccione una de las dos opciones siguientes:

    * **Ubicación original**: restaure los archivos o carpetas seleccionados en el mismo recurso compartido de archivos que el origen inicial.
    * **Ubicación alternativa**: restaure los archivos o carpetas seleccionados en una ubicación alternativa y mantenga el contenido del recurso compartido de archivos original como está.

#### <a name="restore-to-the-original-location"></a>Restauración en la ubicación original

1. Seleccione **Ubicación original** como **Destino de recuperación** y seleccione si desea omitir o sobrescribir si hay conflictos.

    ![Ubicación original de la recuperación en el nivel de elementos](./media/restore-afs/original-location-item-level.png)

1. Seleccione **Seleccionar archivo** para elegir los archivos o carpetas que quiere restaurar.

    ![Elegir Seleccionar archivo](./media/restore-afs/select-file.png)

1. Después de elegir **Seleccionar archivo**, el panel de recursos compartidos de archivos muestra el contenido del punto de recuperación del recurso compartido de archivos que ha seleccionado para la restauración.

1. Active la casilla correspondiente al archivo o carpeta que quiere restaurar y elija **Seleccionar**.

    ![Selección de un archivo o una carpeta](./media/restore-afs/select-file-folder.png)

1. Repita los pasos del 2 al 4 para seleccionar varios archivos o carpetas para la restauración.
1. Después de seleccionar todos los elementos que quiere restaurar, seleccione **Aceptar**.

    ![Después de seleccionar todos los elementos que desea restaurar, seleccione Aceptar](./media/restore-afs/after-selecting-items.png)

1. Seleccione **Restaurar** para iniciar la operación de restauración.

    ![Seleccionar Restaurar para iniciar](./media/restore-afs/click-restore.png)

#### <a name="restore-to-an-alternate-location"></a>Restauración a una ubicación alternativa

1. Seleccione **Ubicación alternativa** como **Destino de recuperación**.
1. Seleccione la cuenta de almacenamiento de destino en la que desea restaurar el contenido del que se ha realizado una copia de seguridad, en la lista desplegable **Cuenta de almacenamiento**.
1. En la lista desplegable **Seleccionar recurso compartido de archivos**, se muestran los recursos compartidos de archivos presentes en la cuenta de almacenamiento seleccionada en el paso 2. Seleccione el recurso compartido de archivos en el que desea restaurar el contenido de la copia de seguridad.
1. En el cuadro **Nombre de la carpeta**, especifique el nombre de la carpeta que desea crear en el recurso compartido de archivos de destino con el contenido restaurado.
1. Seleccione si desea omitir o sobrescribir si hay conflictos.
1. Seleccione **Seleccionar archivo** para elegir los archivos o carpetas que quiere restaurar.

    ![Selección de elementos para restaurar en una ubicación alternativa](./media/restore-afs/restore-to-alternate-location.png)

1. Después de elegir **Seleccionar archivo**, el panel de recursos compartidos de archivos muestra el contenido del punto de recuperación del recurso compartido de archivos que ha seleccionado para la restauración.
1. Active la casilla correspondiente al archivo o carpeta que quiere restaurar y elija **Seleccionar**.

    ![Selección del destino de la recuperación](./media/restore-afs/recovery-destination.png)

1. Repita los pasos del 6 al 8 para seleccionar varios archivos o carpetas para la restauración.
1. Después de seleccionar todos los elementos que quiere restaurar, seleccione **Aceptar**.

    ![Seleccione Aceptar después de seleccionar todos los archivos.](./media/restore-afs/after-selecting-all-items.png)

1. Seleccione **Restaurar** para iniciar la operación de restauración.

## <a name="track-a-restore-operation"></a>Seguimiento de una operación de restauración

Una vez que se desencadene la operación de restauración, el servicio de copia de seguridad crea un trabajo para realizar su seguimiento. Azure Backup muestra las notificaciones sobre el trabajo en el portal. Para ver las operaciones del trabajo, seleccione el hipervínculo notificaciones.

![Seleccionar el hipervínculo de notificaciones](./media/restore-afs/notifications-link.png)

También puede supervisar el progreso de la restauración desde el almacén de Recovery Services:

1. Abra el almacén de Recovery Services desde donde se generó la operación de restauración.
1. Seleccione **Trabajos de copia de seguridad** en la sección **Supervisión** de la hoja Información general para ver el estado de las operaciones que se ejecutan en las diferentes cargas de trabajo.

    ![Seleccionar los trabajos de copia de seguridad](./media/restore-afs/backup-jobs.png)

1. Seleccione el nombre de la carga de trabajo correspondiente a su recurso compartido de archivos para ver más detalles sobre la restauración, como **Datos transferidos** o **Número de archivos restaurados**.

    ![Consulte los detalles restaurados](./media/restore-afs/restore-details.png)

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre cómo [administrar copias de seguridad de recursos compartidos de archivos de Azure](manage-afs-backup.md).
