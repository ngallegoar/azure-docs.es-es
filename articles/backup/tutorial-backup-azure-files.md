---
title: 'Tutorial: Copia de seguridad de recursos compartidos de Azure Files'
description: En este tutorial, aprenderá a usar Azure Portal para configurar un almacén de Recovery Services y realizar copias de seguridad de recursos compartidos de archivos de Azure.
ms.date: 06/10/2019
ms.topic: tutorial
ms.openlocfilehash: ec9074a39f2ece7878c0c3ef828dc21748d0ab89
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "76293936"
---
# <a name="back-up-azure-file-shares-in-the-azure-portal"></a>Copia de seguridad de los recursos compartidos de archivos de Azure en Azure Portal

En este tutorial se explica cómo usar Azure Portal para realizar una copia de seguridad de los [recursos compartidos de archivos de Azure](../storage/files/storage-files-introduction.md).

En esta guía, aprenderá a:
> [!div class="checklist"]
>
> * Configurar un almacén de Recovery Services para realizar una copia de seguridad de archivos de Azure
> * Ejecutar un trabajo de copia de seguridad a petición para crear un punto de restauración

## <a name="prerequisites"></a>Prerequisites

Antes de realizar una copia de seguridad de un recurso compartido de archivos de Azure, asegúrese de que se encuentra en uno de los [tipos de cuenta de almacenamiento admitidos](tutorial-backup-azure-files.md#limitations-for-azure-file-share-backup-during-preview). Cuando haya comprobado esto, puede proteger los recursos compartidos de archivos.

## <a name="limitations-for-azure-file-share-backup-during-preview"></a>Limitaciones de la copia de seguridad de recursos compartidos de archivos de Azure en la versión preliminar

La copia de seguridad de los recursos compartidos de archivos de Azure está en versión preliminar. Los recursos compartidos de archivos de Azure se admiten en cuentas de almacenamiento de uso general v1 y uso general v2. No se admiten los siguientes escenarios de copia de seguridad para los recursos compartidos de archivos de Azure:

* Ni PowerShell ni la CLI están disponibles para proteger Azure Files mediante Azure Backup.
* El número máximo de copias de seguridad programadas al día es una.
* El número máximo de copias de seguridad a petición al día es cuatro.
* Use los [bloqueos de recursos](https://docs.microsoft.com/cli/azure/resource/lock?view=azure-cli-latest) de la cuenta de almacenamiento para impedir la eliminación accidental de copias de seguridad del almacén de Recovery Services.
* No elimine las instantáneas que crea Azure Backup. La eliminación de instantáneas puede provocar la pérdida de puntos de recuperación o errores de restauración.
* No elimine los recursos compartidos de archivos que estén protegidos por Azure Backup. La solución actual eliminará todas las instantáneas realizadas por Azure Backup una vez que se elimina el recurso compartido de archivos y, por consiguiente, se pierden todos los puntos de restauración

La copia de seguridad de recursos compartidos de archivos de Azure en cuentas de almacenamiento con replicación de [almacenamiento con redundancia de zona](../storage/common/storage-redundancy-zrs.md) (ZRS) solo se encuentra disponible actualmente en las regiones de Centro de EE. UU. (CUS), Este de EE. UU. (EUS), Este de EE. UU. 2 (EUS2), Norte de Europa (NE), Sudeste de Asia (SEA), Oeste de Europa (WE) y Oeste de EE. UU. 2 (WUS2).

## <a name="configuring-backup-for-an-azure-file-share"></a>Configuración de la copia de seguridad de un recurso compartido de archivos de Azure

En este tutorial se da por supuesto que ya ha establecido un recurso compartido de archivos de Azure. Para realizar una copia de seguridad del recurso compartido de archivos de Azure, siga estos pasos:

1. Cree un almacén de Recovery Services en la misma región que el recurso compartido de archivos. Si ya tiene un almacén, abra la página de información general de su almacén y haga clic en **Backup** (Copia de seguridad).

    ![Clic en Backup (Copia de seguridad) en la página de información general del almacén](./media/tutorial-backup-azure-files/overview-backup-page.png)

2. En el menú **Objetivo de Backup**, en **¿De qué quiere realizar una copia de seguridad?** , elija Recurso compartido de archivos de Azure.

    ![Elija Azure Fileshare como objetivo de copia de seguridad.](./media/tutorial-backup-azure-files/choose-azure-fileshare-from-backup-goal.png)

3. Haga clic en **Backup** (Copia de seguridad) para configurar el recurso compartido de archivos de Azure en su almacén de Recovery Services.

   ![Haga clic en Backup (Copia de seguridad) para asociar el recurso compartido de archivos de Azure con el almacén](./media/tutorial-backup-azure-files/set-backup-goal.png)

    Cuando el almacén está asociado con el recurso compartido de archivos de Azure, se abre el menú Backup (Copia de seguridad) abre y se le pide seleccionar una cuenta de almacenamiento. El menú muestra todas las cuentas de almacenamiento admitidas en la región donde existe el almacén que aún no están asociadas con un almacén de Recovery Services.

   ![Selección de la cuenta de almacenamiento](./media/tutorial-backup-azure-files/list-of-storage-accounts.png)

4. En la lista de cuentas de almacenamiento, seleccione una cuenta y haga clic en **OK** (Aceptar). Azure busca en la cuenta de almacenamiento los recursos compartidos de archivos de los que se puede realizar una copia de seguridad. Si recientemente ha agregado sus recursos compartidos de archivos y no los ve en la lista, espere un poco a que aparezcan.

   ![Se están detectando los recursos compartidos de archivos](./media/tutorial-backup-azure-files/discover-file-shares.png)

5. En la lista **File Shares** (Recursos compartidos de archivos), seleccione uno o varios recursos compartidos de archivos de los que quiera realizar una copia de seguridad y haga clic en **OK** (Aceptar).

6. Después de elegir los recursos compartidos de archivos, el menú de copia de seguridad cambia a la **directiva de copia de seguridad**. Desde este menú, seleccione una directiva de copia de seguridad existente o cree una nueva y, luego, haga clic en **Enable Backup** (Habilitar copia de seguridad).

   ![Selección o creación de una directiva de copia de seguridad](./media/tutorial-backup-azure-files/apply-backup-policy.png)

    Después de establecer una directiva de copia de seguridad, se realiza una instantánea de los recursos compartidos de archivos a la hora programada y el punto de recuperación se conserva durante el período seleccionado.

## <a name="create-an-on-demand-backup"></a>Creación de una copia de seguridad a petición

Después de configurar la directiva de copia de seguridad, querrá crear una copia de seguridad a petición para garantizar la protección de los datos hasta la próxima copia de seguridad programada.

### <a name="to-create-an-on-demand-backup"></a>Para crear una copia de seguridad a petición, siga estos pasos:

1. Abra el almacén de Recovery Services que contiene los puntos de recuperación del recurso compartido de archivos y haga clic en **Backup Items** (Elementos de copia de seguridad). Aparece la lista de tipos de elementos de copia de seguridad.

   ![Lista de elementos de copia de seguridad](./media/tutorial-backup-azure-files/list-of-backup-items.png)

2. En la lista, seleccione **Azure Storage (Azure Files)** . Aparece la lista de recursos compartidos de archivos de Azure.

   ![Lista de los recursos compartidos de archivos de Azure](./media/tutorial-backup-azure-files/list-of-azure-files-backup-items.png)

3. En la lista de recursos compartidos de archivos de Azure, seleccione el que desee. Se abre el menú de elementos de copia de seguridad correspondiente al recurso compartido de archivos seleccionado.

   ![Menú de elementos de la copia de seguridad correspondiente al recurso compartido de archivos](./media/tutorial-backup-azure-files/backup-item-menu.png)

4. En el menú de elementos de la copia de seguridad, haga clic en **Backup Now** (Crear copia de seguridad ahora). Dado que se trata de un trabajo de copia de seguridad a petición, no hay ninguna directiva de retención asociada con el punto de recuperación. Se abre el cuadro de diálogo **Backup Now** (Crear copia de seguridad ahora). Especifique el último día que quiere conservar el punto de recuperación.

   ![Elección de la fecha para la retención del punto de recuperación](./media/tutorial-backup-azure-files/backup-now-menu.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial se usa Azure Portal para las siguientes acciones:

> [!div class="checklist"]
>
> * Configurar un almacén de Recovery Services para realizar una copia de seguridad de archivos de Azure
> * Ejecutar un trabajo de copia de seguridad a petición para crear un punto de restauración

Vaya al artículo siguiente para saber cómo restaurar una copia de seguridad de un recurso compartido de archivos de Azure.

> [!div class="nextstepaction"]
> [Restauración a partir de la copia de seguridad de un recurso compartido de archivos de Azure](restore-afs.md)
