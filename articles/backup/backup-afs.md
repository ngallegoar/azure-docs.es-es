---
title: Copia de seguridad de los recursos compartidos de archivos de Azure en Azure Portal
description: Aprenda a usar Azure Portal para realizar copias de seguridad de recursos compartidos de archivos de Azure en almacenes de Recovery Services
ms.topic: conceptual
ms.date: 01/20/2020
ms.openlocfilehash: 76bf8e00dede5f227cb862f9c9474844e349e298
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85391167"
---
# <a name="back-up-azure-file-shares"></a>Copia de seguridad de recursos compartidos de archivos de Azure

En este artículo se explica cómo usar Azure Portal para realizar copias de seguridad de [recursos compartidos de archivos de Azure](https://docs.microsoft.com/azure/storage/files/storage-files-introduction).

En este artículo, aprenderá a:

* Cree un almacén de Recovery Services.
* Detectar recursos compartidos de archivos y configuración de copias de seguridad.
* Ejecutar un trabajo de copia de seguridad a petición para crear un punto de restauración.

## <a name="prerequisites"></a>Requisitos previos

* Identifique o cree un [almacén de Recovery Services](#create-a-recovery-services-vault) en la misma región que la cuenta de almacenamiento que hospeda el recurso compartido de archivos.
* Asegúrese de que el recurso compartido de archivos se encuentra en uno de los [tipos de cuenta de almacenamiento admitidos](azure-file-share-support-matrix.md).

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-file-shares-and-configure-backup"></a>Detección de recursos compartidos de archivos y configuración de copias de seguridad

1. En [Azure Portal](https://portal.azure.com/), abra el almacén de Recovery Services que quiere usar para configurar la copia de seguridad del recurso compartido de archivos.

1. En el panel **Almacén de Recovery Services**, seleccione **+Copia de seguridad**en el menú de la parte superior.

   ![Almacén de Recovery Services](./media/backup-afs/recovery-services-vault.png)

    1. En el panel **Objetivo de Backup**, establezca **¿Dónde se ejecuta su carga de trabajo?** en **Azure**; para ello, seleccione la opción **Azure** en la lista desplegable.

          ![Selección de Azure como carga de trabajo](./media/backup-afs/backup-goal.png)

    2. En **¿De qué desea hacer una copia de seguridad?** , seleccione **Recurso compartido de archivos de Azure** en la lista desplegable.

          ![Selección del recurso compartido de archivos de Azure](./media/backup-afs/select-azure-file-share.png)

    3. Seleccione **Copia de seguridad** para registrar la extensión del recurso compartido de archivos de Azure en el almacén.

          ![Seleccione Copia de seguridad para asociar el recurso compartido de archivos de Azure con el almacén](./media/backup-afs/register-extension.png)

1. Después de seleccionar **Copia de seguridad**, se abre el panel **Copia de seguridad**. Para seleccionar la cuenta de almacenamiento que hospeda el recurso compartido de archivos que quiere proteger, haga clic en el texto del vínculo **Seleccionar** debajo del cuadro de texto **Cuenta de almacenamiento**.

   ![Selección del vínculo "Seleccionar"](./media/backup-afs/choose-select-link.png)

1. El **panel Seleccionar cuenta de almacenamiento** se abre a la derecha y enumera un conjunto de cuentas de almacenamiento admitidas que se han detectado. Estas cuentas están asociadas a este almacén o se encuentran en la misma región que el almacén, pero aún no están asociadas a ningún almacén de Recovery Services.

1. En la lista de cuentas de almacenamiento detectadas, seleccione una cuenta y seleccione **Aceptar**.

   ![Selección de una de las cuentas de almacenamiento detectadas](./media/backup-afs/select-discovered-storage-account.png)

1. El siguiente paso consiste en seleccionar los recursos compartidos de archivos de los que quiere realizar copias de seguridad. Haga clic en el botón **Agregar** de la sección **Recursos compartidos para realizar la copia de seguridad**.

   ![Selección de los recursos compartidos de archivos de los que quiere hacer una copia de seguridad](./media/backup-afs/select-file-shares-to-back-up.png)

1. Se abre a la derecha el panel de contexto **Seleccionar los recursos compartidos de archivos**. Azure busca en la cuenta de almacenamiento los recursos compartidos de archivos de los que se puede realizar una copia de seguridad. Si recientemente ha agregado recursos compartidos de archivos y no los ve en la lista, espere un poco para que aparezcan.

1. En la lista **Seleccionar los recursos compartidos de archivos**, seleccione uno o varios recursos compartidos de archivos de los que quiera realizar una copia de seguridad. Seleccione **Aceptar**.

   ![Selección de los recursos compartidos de archivos](./media/backup-afs/select-file-shares.png)

1. Para elegir una directiva de copia de seguridad para el recurso compartido de archivos, tiene tres opciones:

   * Elija la directiva predeterminada.<br>
   Esta opción permite habilitar la copia de seguridad diaria que se conservará durante 30 días. Si no tiene una directiva de copia de seguridad existente en el almacén, se abre el panel de copia de seguridad con la configuración de directivas predeterminada. Si quiere elegir la configuración predeterminada, puede hacer clic directamente en **Habilitar copia de seguridad**.

   * Creación de una nueva directiva <br>

      1. Para crear una nueva directiva de copia de seguridad para el recurso compartido de archivos, haga clic en el texto del vínculo situado debajo de la lista desplegable de la sección **Directiva de copia de seguridad**.<br>

         ![Cree una nueva directiva.](./media/backup-afs/create-new-policy.png)

      1. Se abre el panel de contexto **Directiva de copia de seguridad** a la derecha. Especifique un nombre de directiva en el cuadro de texto y elija el período de retención según sus requisitos. De forma predeterminada, solo está habilitada la opción de retención diaria. Si quiere elegir una retención semanal, mensual o anual, seleccione la casilla correspondiente y especifique el valor de retención deseado.

      1. Después de especificar los valores de retención y un nombre de directiva válido, haga clic en Aceptar.<br>

         ![Proporcionar el nombre de la directiva y los valores de retención](./media/backup-afs/policy-name.png)

   * Elija una de las directivas de copia de seguridad existentes <br>

   Para elegir una de las directivas de copia de seguridad existentes para configurar la protección, seleccione la directiva que desee en la lista desplegable **Directiva de copia de seguridad**.<br>

   ![Elección de directiva existente](./media/backup-afs/choose-existing-policy.png)

1. Haga clic en **Habilitar copia de seguridad** para empezar a proteger el recurso compartido de archivos.

   ![Elección de la opción Habilitar copia de seguridad](./media/backup-afs/enable-backup.png)

Después de establecer una directiva de copia de seguridad, se realiza una instantánea de los recursos compartidos de archivos a la hora programada. El punto de recuperación también se conserva durante el período elegido.

>[!NOTE]
>Azure Backup admite ahora directivas con retención diaria, semanal, mensual o anual para la copia de seguridad de recursos compartidos de archivos de Azure.

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

>[!NOTE]
>Azure Backup bloquea la cuenta de almacenamiento cuando se configura la protección para cualquier recurso compartido de archivos en la cuenta correspondiente. Este proceso ofrece protección contra la eliminación accidental de una cuenta de almacenamiento con recursos compartidos de archivos de copia de seguridad.

## <a name="best-practices"></a>Procedimientos recomendados

* No elimine las instantáneas que crea Azure Backup. La eliminación de instantáneas puede provocar la pérdida de puntos de recuperación o errores de restauración.

* No quite el bloqueo realizado en la cuenta de almacenamiento mediante Azure Backup. Si elimina el bloqueo, la cuenta de almacenamiento será propensa a la eliminación accidental y, si ocurre, perderá las instantáneas o copias de seguridad.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo:

* [Restauración de recursos compartidos de archivos de Azure](restore-afs.md)
* [Administración de copias de seguridad de recursos compartidos de archivos de Azure](manage-afs-backup.md)
