---
title: Instalación de actualizaciones en el dispositivo de la serie Azure Data Box Gateway | Microsoft Docs
description: Se describe cómo aplicar actualizaciones con Azure Portal y la interfaz de usuario web local para el dispositivo de la serie Azure Data Box Gateway.
services: databox
author: alkohli
ms.service: databox
ms.topic: article
ms.date: 06/30/2020
ms.author: alkohli
ms.openlocfilehash: 337c0c4434eb768ee45429d9b2d23536db4c3fc7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91575459"
---
# <a name="update-your-azure-data-box-gateway"></a>Actualización de Azure Data Box Gateway

En este artículo se describen los pasos necesarios para instalar actualizaciones en Azure Data Box Gateway mediante la interfaz de usuario web local y Azure Portal. Se aplican las actualizaciones o revisiones de software para mantener actualizado el dispositivo Data Box Gateway.

> [!IMPORTANT]
>
> - La actualización **1911** se corresponde con la versión de software **1.6.1049.786** del dispositivo. Para obtener más información sobre esta actualización, vaya a las [Notas de la versión](data-box-gateway-1911-release-notes.md).
>
> - Tenga en cuenta que al instalar una actualización o revisión, se reiniciará el dispositivo. Dado que Data Box Gateway es un dispositivo de un solo nodo, se interrumpe cualquier E/S en curso y el dispositivo experimenta un tiempo de inactividad de hasta 30 minutos para la actualización de software del dispositivo.

En las secciones siguientes se describen todos estos pasos.

## <a name="use-the-azure-portal"></a>Uso de Azure Portal

Se recomienda instalar las actualizaciones mediante Azure Portal. El dispositivo busca actualizaciones automáticamente una vez al día. Una vez que las actualizaciones estén disponibles, verá una notificación en el portal. Puede descargar e instalar las actualizaciones.

> [!NOTE]
> Asegúrese de que el dispositivo sea correcto y de que el estado se muestra como **En línea** antes de continuar con la instalación de las actualizaciones.

1. Cuando las actualizaciones estén disponibles para el dispositivo, verá una notificación. Seleccione la notificación o, en la barra de comandos superior, seleccione **Actualizar dispositivo**. Esto le permitirá aplicar las actualizaciones de software del dispositivo.

    ![Captura de pantalla de la página principal de Azure Data Box Gateway de Azure Stack Edge con las opciones Información general y Actualizar dispositivo seleccionadas.](./media/data-box-gateway-apply-updates/portal-apply-update-01a.png)

2. En la hoja **Actualizaciones del dispositivo**, compruebe que ha revisado los términos de licencia asociados a las nuevas características en las notas de la versión.

    Puede optar por **descargar e instalar** las actualizaciones o simplemente **descargarlas**. Después, puede optar por instalar estas actualizaciones más adelante.

    ![Captura de pantalla del cuadro de diálogo Actualizaciones del dispositivo con las opciones de aceptación y entendimiento y la opción de descarga e instalación seleccionadas.](./media/data-box-gateway-apply-updates/portal-apply-update-02.png)

    Si desea descargar e instalar las actualizaciones, active la opción que permite que las actualizaciones se instalen automáticamente una vez completada la descarga.

    ![Captura de pantalla del cuadro de diálogo Actualizaciones del dispositivo con la opción de aceptación y la opción de descarga seleccionadas.](./media/data-box-gateway-apply-updates/portal-apply-update-03.png)

3. Se inicia la descarga de actualizaciones. Verá una notificación de que la descarga está en curso.

    ![Captura de pantalla de la notificación que indica: "Descarga e instalación de actualizaciones en curso".](./media/data-box-gateway-apply-updates/portal-apply-update-05.png)

    También se muestra un banner de notificación en Azure Portal. Indica el progreso de la descarga.

    ![Captura de pantalla de la página principal de Data Box Gateway de Azure Stack Edge con la opción Información general y el banner de notificación de descarga de actualizaciones seleccionados.](./media/data-box-gateway-apply-updates/portal-apply-update-08a.png)

    Puede seleccionar esta notificación o seleccionar **Actualizar dispositivo** para ver el estado detallado de la actualización.

    ![Captura de pantalla del cuadro de diálogo Descargar e instalar actualizaciones con el mensaje de estado en curso y el mensaje de descarga de actualizaciones seleccionados.](./media/data-box-gateway-apply-updates/portal-apply-update-09.png)

4. Una vez completada la descarga, el banner de notificación se actualiza para indicar la finalización. Si decide descargar e instalar las actualizaciones, la instalación se iniciará automáticamente.

    ![Captura de pantalla de la página principal de Data Box Gateway de Azure Stack Edge con la opción Información general y el banner de notificación de actualizaciones descargadas seleccionados.](./media/data-box-gateway-apply-updates/portal-apply-update-10a.png)

    Si decide descargar solo las actualizaciones, seleccione la notificación para abrir la hoja **Actualizaciones del dispositivo**. Seleccione **Instalar**.
  
    ![Captura de pantalla del cuadro de diálogo Actualizaciones del dispositivo con la opción Instalar seleccionada.](./media/data-box-gateway-apply-updates/portal-apply-update-11a.png)

5. Verá una notificación de que la instalación está en curso.

    ![Captura de pantalla de la página principal de Data Box Gateway de Azure Stack Edge con la opción Información general y el mensaje de progreso de la descarga e instalación seleccionados.](./media/data-box-gateway-apply-updates/portal-apply-update-12a.png)

    El portal también muestra una alerta informativa para indicar que la instalación está en curso. <!-- The device goes offline and is in maintenance mode.-->

    <!-- ![Software version after update](./media/data-box-gateway-apply-updates/update-13.png)-->

6. Dado que se trata de un dispositivo de un nodo, el dispositivo se reiniciará una vez instaladas las actualizaciones. La alerta crítica durante el reinicio indicará que el latido del dispositivo se ha perdido.

    ![Captura de pantalla de la página principal de Data Box Gateway de Azure Stack Edge con la opción Información general y el banner de notificación de alertas críticas seleccionados.](./media/data-box-gateway-apply-updates/portal-apply-update-19a.png)

    Seleccione la alerta para ver el evento de dispositivo correspondiente.

    ![Captura de pantalla de la sección Eventos de dispositivo con la opción de pérdida de latido del evento de dispositivo seleccionada.](./media/data-box-gateway-apply-updates/portal-apply-update-20a.png)

7. El estado del dispositivo se actualiza a **En línea** una vez instaladas las actualizaciones.

    ![Captura de pantalla de la página principal de Data Box Gateway de Azure Stack Edge con la opción Información general y el estado En línea seleccionados.](./media/data-box-gateway-apply-updates/portal-apply-update-23a.png)

    En la barra de comandos superior, seleccione **Actualizaciones del dispositivo**. Compruebe que la actualización se ha instalado correctamente y que la versión del software del dispositivo así lo refleja.

    ![Captura de pantalla del cuadro de diálogo Actualizaciones del dispositivo con la sección Versión del software instalada seleccionada.](./media/data-box-gateway-apply-updates/portal-apply-update-24.png)

## <a name="use-the-local-web-ui"></a>Uso de la interfaz de usuario web local

Se pueden seguir dos pasos con la interfaz de usuario web local:

- Descargar la actualización o la revisión
- Instalar la actualización o la revisión

En las siguientes secciones se describen estos pasos.

### <a name="download-the-update-or-the-hotfix"></a>Descargar la actualización o la revisión

Realice los pasos siguientes para descargar la actualización. Puede descargar la actualización desde la ubicación proporcionada por Microsoft o desde el Catálogo de Microsoft Update.

Realice los pasos siguientes para descargar la actualización desde el Catálogo de Microsoft Update.

1. Inicie el explorador y vaya a [https://catalog.update.microsoft.com](https://catalog.update.microsoft.com).

   ![Captura de pantalla del catálogo de Microsoft Update en una ventana del explorador con Data Box Gateway escrito en el cuadro de texto de búsqueda. La barra de direcciones del explorador y el cuadro de texto de búsqueda están seleccionados.](./media/data-box-gateway-apply-updates/download-update-1.png)

2. En el cuadro de búsqueda del Catálogo de Microsoft Update, escriba el número de Knowledge Base (KB) correspondiente a la revisión o los términos de la actualización que quiera descargar. Por ejemplo, escriba **Azure Data Box Gateway** y, después, haga clic en **Buscar**.

   La lista de actualizaciones aparece como **Azure Data Box Gateway 1911**.

   ![Captura de pantalla del catálogo de Microsoft Update en una ventana del explorador con los resultados de búsqueda de Data Box Gateway mostrados y seleccionados.](./media/data-box-gateway-apply-updates/download-update-2.png)

3. Seleccione **Descargar**. Hay un único archivo para descargar llamado *SoftwareUpdatePackage.exe* que corresponde a la actualización de software del dispositivo. Descargue el archivo en una carpeta en el sistema local. También puede copiar la carpeta en un recurso compartido de red que sea accesible desde el dispositivo.

   ![Captura de pantalla del cuadro de diálogo Descargar con el vínculo al archivo .e x e del paquete de actualización de software seleccionado.](./media/data-box-gateway-apply-updates/download-update-3.png)

### <a name="install-the-update-or-the-hotfix"></a>Instalar la actualización o la revisión

Antes de instalar la actualización o revisión, compruebe que:

- Tiene la actualización o la revisión descargada localmente en el host o que puede acceder a ella a través de un recurso compartido de red.
- El estado del dispositivo es correcto, tal como se muestra en la página **información general** de la interfaz de usuario web local.

   ![Captura de pantalla de la I U web local de Data Box Gateway con la opción Panel y el mensaje de Estado del software: Correcto seleccionado.](./media/data-box-gateway-apply-updates/local-ui-update-1.png)

Este procedimiento tarda aproximadamente 20 minutos en completarse. Realice los pasos siguientes para instalar la actualización o revisión.

1. En la interfaz de usuario web local, vaya a **Mantenimiento** > **Actualización de software**. Tome nota de la versión de software que se está ejecutando.

   ![Captura de pantalla de la I U web local de Data Box Gateway con la opción de actualización de software y el mensaje de versión de software actual seleccionados.](./media/data-box-gateway-apply-updates/local-ui-update-2.png)

2. Proporcione la ruta de acceso al archivo de actualización. Asimismo, también puede acceder al archivo de instalación de la actualización si está en un recurso compartido de red. Seleccione el archivo de actualización de software con el sufijo *SoftwareUpdatePackage.exe*.

   ![Captura de pantalla del Explorador de archivos con el archivo .e.x.e del de paquete de actualización de software seleccionado.](./media/data-box-gateway-apply-updates/local-ui-update-3.png)

3. Seleccione **Aplicar**.

   ![Captura de pantalla de la I U web local de Data Box Gateway con la opción de actualización de software, el cuadro de texto de ruta de acceso al archivo de actualización y la opción Aplicar actualización seleccionados.](./media/data-box-gateway-apply-updates/local-ui-update-4.png)

4. Cuando se le pida confirmación, seleccione **Sí** para continuar. Dado que se trata de un dispositivo de un solo nodo, una vez aplicada la actualización, se reiniciará el dispositivo y habrá un tiempo de inactividad.
   ![Captura de pantalla del cuadro de diálogo de actualización de software con la opción Sí resaltada.](./media/data-box-gateway-apply-updates/local-ui-update-5.png)

5. Se inicia la actualización. Una vez que el dispositivo se actualice correctamente, este se reiniciará. La interfaz de usuario local no será accesible durante este tiempo.

6. Una vez completado el reinicio, se le llevará a la página de **inicio de sesión** . Para comprobar que el software del dispositivo se ha actualizado, en la interfaz de usuario de web local, vaya a **Mantenimiento** > **Actualización de software**. La versión de software mostrada en este ejemplo es **1.6.1049.786**.

   ![Captura de pantalla de la I U web local de Data Box Gateway con la opción de actualización de software y el mensaje de versión actual de software actualizado seleccionados.](./media/data-box-gateway-apply-updates/local-ui-update-6.png)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo [administrar Azure Data Box Gateway](data-box-gateway-manage-users.md).
