---
title: Copia de seguridad de los recursos compartidos de archivos de Azure en Azure Portal
description: Aprenda a usar Azure Portal para realizar copias de seguridad de recursos compartidos de archivos de Azure en almacenes de Recovery Services
ms.topic: conceptual
ms.date: 01/20/2020
ms.openlocfilehash: c1dea6925bad96be178f875567077fafa4db9326
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76938188"
---
# <a name="back-up-azure-file-shares-in-a-recovery-services-vault"></a>Copia de seguridad de recursos compartidos de archivos de Azure en un almacén de Recovery Services

En este artículo se explica cómo usar Azure Portal para realizar copias de seguridad de [recursos compartidos de archivos de Azure](https://docs.microsoft.com/azure/storage/files/storage-files-introduction).

En este artículo, aprenderá a:

* Cree un almacén de Recovery Services.
* Detectar recursos compartidos de archivos y configuración de copias de seguridad.
* Ejecutar un trabajo de copia de seguridad a petición para crear un punto de restauración.

## <a name="prerequisites"></a>Prerrequisitos

* Identifique o cree un [almacén de Recovery Services](#create-a-recovery-services-vault) en la misma región que la cuenta de almacenamiento que hospeda el recurso compartido de archivos.
* Asegúrese de que el recurso compartido de archivos se encuentra en uno de los [tipos de cuenta de almacenamiento admitidos](#limitations-for-azure-file-share-backup-during-preview).

## <a name="limitations-for-azure-file-share-backup-during-preview"></a>Limitaciones de la copia de seguridad de recursos compartidos de archivos de Azure en la versión preliminar

La copia de seguridad de los recursos compartidos de archivos de Azure está en versión preliminar. Los recursos compartidos de archivos de Azure se admiten en cuentas de almacenamiento de uso general v1 y uso general v2. Estas son las limitaciones para realizar copias de seguridad de recursos compartidos de archivos de Azure:

* La compatibilidad con la copia de seguridad de recursos compartidos de archivos de Azure en cuentas de almacenamiento con replicación de [almacenamiento con redundancia de zona](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) (ZRS) está limitada actualmente a [estas regiones](https://docs.microsoft.com/azure/backup/backup-azure-files-faq#in-which-geos-can-i-back-up-azure-file-shares).
* Actualmente, Azure Backup admite la configuración de copias de seguridad diarias programadas de recursos compartidos de archivos de Azure.
* El número máximo de copias de seguridad programadas al día es una.
* El número máximo de copias de seguridad a petición al día es cuatro.
* Use los [bloqueos de recursos](https://docs.microsoft.com/cli/azure/resource/lock?view=azure-cli-latest) de la cuenta de almacenamiento para impedir la eliminación accidental de copias de seguridad del almacén de Recovery Services.
* No elimine las instantáneas que crea Azure Backup. La eliminación de instantáneas puede provocar la pérdida de puntos de recuperación o errores de restauración.
* No elimine los recursos compartidos de archivos que estén protegidos por Azure Backup. La solución actual elimina todas las instantáneas realizadas por Azure Backup una vez que se elimina el recurso compartido de archivos y, por tanto, se pierden todos los puntos de restauración.

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="modify-storage-replication"></a>Modificar la replicación de almacenamiento

De forma predeterminada, los almacenes usan el [almacenamiento con redundancia geográfica (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs).

* Si el almacén es su mecanismo principal de copia de seguridad, le recomendamos que use GRS.
* Puede usar el [almacenamiento con redundancia local (LRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) como opción de bajo costo.

Para modificar el tipo de replicación de almacenamiento:

1. En la sección **Configuración** del nuevo almacén, seleccione **Propiedades**.

1. En la hoja **Propiedades**, haga clic en **Actualizar** en la hoja **Configuración de copia de seguridad**.

1. Seleccione el tipo de replicación almacenamiento y seleccione **Guardar**.

    ![Actualización de la configuración de copia de seguridad](./media/backup-afs/backup-configuration.png)

> [!NOTE]
> No puede modificar el tipo de replicación de almacenamiento después de configurar el almacén y si este contiene elementos de copia de seguridad. Si quiere hacer esto, tiene que volver a crear el almacén.
>

## <a name="discover-file-shares-and-configure-backup"></a>Detección de recursos compartidos de archivos y configuración de copias de seguridad

1. En [Azure Portal](https://portal.azure.com/), abra el almacén de Recovery Services que quiere usar para la copia de seguridad del recurso compartido de archivos.

1. En el panel **Almacén de Recovery Services**, seleccione **+Copia de seguridad**.

   ![Almacén de Recovery Services](./media/backup-afs/recovery-services-vault.png)

    a. En **Objetivo de Backup**, establezca **¿Dónde se ejecuta su carga de trabajo?** en **Azure**.

    ![Selección de recurso compartido de archivos de Azure como objetivo de copia de seguridad](./media/backup-afs/backup-goal.png)

    b.  En **¿De qué desea hacer una copia de seguridad?** , seleccione **Recurso compartido de archivos de Azure** en la lista desplegable.

    c.  Seleccione **Copia de seguridad** para registrar la extensión del recurso compartido de archivos de Azure en el almacén.

    ![Seleccione Copia de seguridad para asociar el recurso compartido de archivos de Azure con el almacén](./media/backup-afs/register-extension.png)

1. Después de seleccionar **Copia de seguridad**, se abre el panel **Copia de seguridad** y se le pide que seleccione una cuenta de almacenamiento en una lista de cuentas de almacenamiento admitidas detectadas. Estas cuentas están asociadas a este almacén o se encuentran en la misma región que el almacén, pero aún no están asociadas a ningún almacén de Recovery Services.

   ![Selección de la cuenta de almacenamiento](./media/backup-afs/select-storage-account.png)

1. En la lista de cuentas de almacenamiento detectadas, seleccione una cuenta y seleccione **Aceptar**. Azure busca en la cuenta de almacenamiento los recursos compartidos de archivos de los que se puede realizar una copia de seguridad. Si recientemente ha agregado recursos compartidos de archivos y no los ve en la lista, espere un poco para que aparezcan.

    ![Detección de recursos compartidos de archivos](./media/backup-afs/discovering-file-shares.png)

1. En la lista **Recursos compartidos de archivos**, seleccione uno o varios recursos compartidos de archivos de los que quiera realizar una copia de seguridad. Seleccione **Aceptar**.

1. Después de seleccionar los recursos compartidos de archivos, el menú **Copia de seguridad** cambia a **Directiva de copia de seguridad**. En este menú, seleccione una directiva de copia de seguridad existente o cree una. Después, seleccione **Habilitar copia de seguridad**.

    ![Seleccionar directiva de copia de seguridad](./media/backup-afs/select-backup-policy.png)

Después de establecer una directiva de copia de seguridad, se realiza una instantánea de los recursos compartidos de archivos a la hora programada. El punto de recuperación también se conserva durante el período elegido.

## <a name="create-an-on-demand-backup"></a>Creación de una copia de seguridad a petición

En ocasiones querrá generar una instantánea de copia de seguridad o un punto de recuperación fuera de las horas programadas en la directiva de copia de seguridad. Un motivo habitual para generar una copia de seguridad a petición es justo después de haber configurado la directiva de copia de seguridad. Según la programación de la directiva de copia de seguridad, pueden transcurrir horas y días hasta que se toma una instantánea. Para proteger los datos hasta que se aplique la directiva de copia de seguridad, inicie una copia de seguridad a petición. La creación de una copia de seguridad a petición se suele exigir antes de realizar cambios planeados en los recursos compartidos de archivos.

### <a name="create-a-backup-job-on-demand"></a>Creación de un trabajo de copia de seguridad a petición

1. Abra el almacén de Recovery Services que usó para hacer la copia de seguridad de los recursos compartidos de archivos. Haga clic en **Elementos de copia de seguridad** en la sección **Elementos protegidos** de la hoja **Información general**.

   ![Selección de elementos de copia de seguridad](./media/backup-afs/backup-items.png)

1. Después de seleccionar **Elementos de copia de seguridad**, aparece un nuevo panel que muestra los posibles valores de **Tipo de administración de copia de seguridad** junto al panel **Información general**.

   ![Lista de tipos de administración de copia de seguridad](./media/backup-afs/backup-management-types.png)

1. En la lista **Tipo de administración de copia de seguridad**, seleccione **Azure Storage (Azure Files)** . Verá una lista de todos los recursos compartidos de archivos y las cuentas de almacenamiento correspondientes cuya copia de seguridad se ha realizado mediante este almacén.

   ![Elementos de copia de seguridad de Azure Storage (Azure Files)](./media/backup-afs/azure-files-backup-items.png)

1. En la lista de recursos compartidos de archivos de Azure, seleccione el que desee. Aparecen los detalles de **Elemento de copia de seguridad**. En el menú **Copia de seguridad**, seleccione **Realizar copia de seguridad ahora**. Como se trata de un trabajo de copia de seguridad a petición, no hay ninguna directiva de retención asociada con el punto de recuperación.

   ![Seleccionar Realizar copia de seguridad ahora](./media/backup-afs/backup-now.png)

1. Aparece el panel **Realizar copia de seguridad ahora**. Especifique el último día que quiere conservar el punto de recuperación. Las copias de seguridad a petición pueden tener un período de retención máximo de 10 años.

   ![Selección de la fecha de retención](./media/backup-afs/retention-date.png)

1. Seleccione **Aceptar** para confirmar la ejecución del trabajo de copia de seguridad a petición.

1. Supervise las notificaciones del portal para realizar un seguimiento de la finalización de la ejecución del trabajo de copia de seguridad. Puede supervisar el progreso del trabajo en el panel del almacén. Seleccione **Trabajos de copia de seguridad** > **En curso**.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo:
* [Restauración de recursos compartidos de archivos de Azure](restore-afs.md)
* [Administración de copias de seguridad de recursos compartidos de archivos de Azure](manage-afs-backup.md)
