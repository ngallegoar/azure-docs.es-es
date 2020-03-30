---
title: Visualización del contenido de los archivos con Azure Automation
description: Use la característica de cambio en el contenido de los archivos de Change Tracking para ver el contenido de un archivo que ha cambiado.
services: automation
ms.subservice: change-inventory-management
ms.date: 07/03/2018
ms.topic: conceptual
ms.openlocfilehash: 23c4f24e430d58895eb551c3e2cb62b5f0003ac0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75418821"
---
# <a name="view-contents-of-a-file-that-is-being-tracked-with-change-tracking"></a>Visualización del contenido de un archivo del que se está realizando un seguimiento con Change Tracking

El seguimiento del contenido de archivos le permite ver el contenido de un archivo antes y después de que se produzca un cambio que se está siguiendo con Change Tracking. Para ello, guarda el contenido del archivo en una cuenta de almacenamiento después de que se produzca cada cambio.

## <a name="requirements"></a>Requisitos

* Para almacenar el contenido del archivo se requiere una cuenta de almacenamiento estándar que use el modelo de implementación de Resource Manager. Las cuentas de almacenamiento de los modelos de implementación Premium y clásica no deben usarse. Para más información acerca de las cuentas de almacenamiento, consulte [Acerca de las cuentas de almacenamiento de Azure](../storage/common/storage-create-storage-account.md).

* La cuenta de almacenamiento usada solo puede tener una cuenta de Automation conectada.

* [Change Tracking](automation-change-tracking.md) se habilita en su cuenta de Automation.

## <a name="enable-file-content-tracking"></a>Habilitación del seguimiento del contenido de los archivos

1. En Azure Portal, abra su cuenta de Automation y seleccione **Change Tracking**.
2. En el menú superior, seleccione **Editar configuración**.
3. Seleccione **Contenido del archivo** y haga clic en **Vínculo**. De esta forma se abre el panel **Agregar ubicación de contenido para Change Tracking**.

   ![enable](./media/change-tracking-file-contents/enable.png)

4. Seleccione la suscripción y la cuenta de almacenamiento que se va a usar para almacenar el contenido del archivo. Si desea habilitar el seguimiento del contenido de los archivos para todos los archivos que se sigan, seleccione **Activar** para **cargar el contenido del archivo para todas las configuraciones**. Puede cambiar esto para la ruta de acceso de cada archivo más adelante.

   ![establecer cuenta de almacenamiento](./media/change-tracking-file-contents/storage-account.png)

5. Una vez habilitada, se muestran los URI de SAS y de la cuenta de almacenamiento. Los URI de SAS expiran después de 365 días y se pueden volver a crear haciendo clic en el botón **Regenerar**.

   ![enumeración de claves de cuenta](./media/change-tracking-file-contents/account-keys.png)

## <a name="add-a-file"></a>Agregar un archivo

Los siguientes pasos le guiarán por la activación del seguimiento de cambios para un archivo:

1. En la página **Editar configuración** de **Change Tracking**, seleccione la pestaña **Archivos de Windows** o **Archivos de Linux**, y haga clic en  **Agregar**.

1. Rellene la información de la ruta de acceso de archivo y seleccione **True** en **Cargar contenido de archivo para todas las configuraciones**. Esta opción habilita el seguimiento del contenido del archivo solo para esa ruta de acceso de archivo.

   ![agregar un archivo de Linux](./media/change-tracking-file-contents/add-linux-file.png)

## <a name="viewing-the-contents-of-a-tracked-file"></a>Visualizar el contenido de un archivo que se sigue

1. Una vez que detectado un cambio en el archivo o un archivo en la ruta de acceso, se muestra en el portal. Seleccione el cambio de archivo en la lista de cambios. Se muestra el panel **Detalles del cambio**.

   ![enumerar cambios](./media/change-tracking-file-contents/change-list.png)

1. En la página **Detalles del cambio**, consulte el estándar antes y después de la información de archivo, en la esquina superior izquierda, haga clic en **Ver cambios de contenido del archivo** para ver el contenido del archivo.

   ![detalles del cambio](./media/change-tracking-file-contents/change-details.png)

1. La nueva página muestra el contenido del archivo en una vista en paralelo. También puede seleccionar **Alineado** para ver una vista alineada de los cambios.

   ![ver los cambios de archivos](./media/change-tracking-file-contents/view-file-changes.png)

## <a name="next-steps"></a>Pasos siguientes

Consulte el tutorial de Change Tracking para más información acerca del uso de la solución:

> [!div class="nextstepaction"]
> [Solución de problemas de cambios en el entorno](automation-tutorial-troubleshoot-changes.md)

* Consulte [Búsquedas de registros en los registros de Azure Monitor](../log-analytics/log-analytics-log-searches.md) para ver datos detallados sobre el seguimiento de cambios.

