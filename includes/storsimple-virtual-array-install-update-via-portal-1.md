---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 3129bbe171329ecf37f8712394cedf9b70188220
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2020
ms.locfileid: "96005830"
---
#### <a name="to-install-updates-via-the-azure-portal"></a>Para instalar actualizaciones mediante Azure Portal

1. Vaya a su Administrador de dispositivos de StorSimple y seleccione **Dispositivos**. En la lista de dispositivos conectados al servicio, seleccione y haga clic en el dispositivo que desea actualizar.

    ![En Recientes, están resaltados y seleccionados el administrador de dispositivos MySSDevManager, Dispositivos y el dispositivo MYSSIS1103.](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate1m.png) 

2. En la hoja **Configuración**, haga clic en **Actualizaciones del dispositivo**.

    ![Se muestra la información del dispositivo MYSSIS1103. En Configuración, está resaltada la opción las Actualizaciones del dispositivo.](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate2m.png)  

3. Verá un mensaje si hay actualizaciones de software disponibles. Para buscar actualizaciones, también puede hacer clic en **Examinar**. Tome nota de la versión de software que se ejecuta. 

    ![El panel Actualizaciones del dispositivo muestra los mensajes "New updates are available" (Hay nuevas actualizaciones) y "Last scanned / 11/01/2017 10:56 AM / Software version / 10.0.10289.0" (Ultima exploración: 11/01/2017 10:56 AM/versión del software: 10.0.10289.0). La versión aparece resaltada.](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate3m1.png)

    Se le notificará cuando se inicie el análisis y se complete correctamente.

    ![La notificación indica "Scanning updates for device MySVAFS110…. 10:57 AM/The operation is in progress." (Examinando si el dispositivo MySVAFS110 tiene actualizaciones 10:57 AM/la operación está en curso).](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate5m.png)

4. Una vez examinadas las actualizaciones, haga clic en **Descargar actualizaciones**.

    ![En el panel Actualizaciones del dispositivo aparece el mensaje "New updates are available"(Hay nuevas actualizaciones). Aparece resaltado el botón Descargar actualizaciones.](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate6m.png)

5. En la hoja **Nuevas actualizaciones**, revise las notas de la versión. Tenga en cuenta también que, una vez descargadas las actualizaciones, debe confirmar la instalación. Haga clic en **OK**.

    ![En el panel Actualizaciones nuevas se indica "After the updates are downloaded, you will need to confirm the installation" (Una vez descargadas las actualizaciones, deberá confirmar la instalación). El botón Aceptar está resaltado.](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate7m.png)

6. Se le notificará cuando se inicie la carga y se complete correctamente.

     ![La notificación indica "Downloading updates for device MySVAFS… 11:07 AM" (Descargando las actualizaciones del dispositivo MySVAFS… 11:07 AM).](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate8m.png)

5. En la hoja **Actualizaciones del dispositivo**, haga clic en **Instalar**.

     ![En Actualizaciones del dispositivo aparece el mensaje "Confirm installing updates" (Confirme la instalación de las actualizaciones). El botón Instalar aparece resaltado.](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate11m1.png)

6. En la hoja **Nuevas actualizaciones**, se le advertirá de que la actualización puede ser perjudicial. Puesto que la matriz virtual es un dispositivo de un solo nodo, el dispositivo se reiniciará una vez actualizado. Se interrumpirá cualquier E/S en curso. Haga clic en **Aceptar** para instalar las actualizaciones.

    ![El mensaje del panel Nuevas actualizaciones es "Your device will have a downtime when these updates are installed" (Durante la instalación de estas actualizaciones, el dispositivo estará sin actividad). El botón Aceptar está resaltado.](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate12m.png)

7. Se le notificará cuando se inicie el trabajo de instalación.

    ![La notificación indica "Installing updates for device MySVAFS110…. 11:09 AM" (Instalando las actualizaciones del dispositivo MySVAFS110… 11:07 AM).](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate13m.png)

8.  Cuando el trabajo de instalación finalice correctamente, haga clic en el vínculo **Ver trabajo**, en la hoja **Actualizaciones del dispositivo**, para supervisar la instalación. 

    ![En el panel Actualizaciones de dispositivos, hay un vínculo con la etiqueta "Installing updates. View Job" (Instalando actualizaciones. Ver trabajo). El botón Instalar aparece resaltado.](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate15m1.png)

    Esta acción lo remite a la hoja **Instalar actualizaciones**. Puede ver información detallada sobre el trabajo aquí.

    ![El panel Instalar actualizaciones tiene información sobre la instalación, incluidos el dispositivo, el estado, la duración, la hora de inicio y la hora de detención.](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate16m1.png)

9. Si empezó con una matriz virtual que ejecutaba la versión de software Update 0.6 (10.0.10293.0), entonces ahora ejecuta Update 1 y no debe hacer nada más. Puede omitir los demás pasos. Si empezó con una matriz virtual que ejecutaba una versión de software anterior a Update 0.6 (10.0.10293.0), entonces ahora se realiza la actualización a Update 0.6. Verá otro mensaje que indica que las actualizaciones están disponibles. Repita los pasos 4 a 8 para instalar Update 1.

    ![El panel Actualizaciones del dispositivo muestra los mensajes "New updates are available" (Hay nuevas actualizaciones) y "Last scanned / 11/1/2017 10:56 AM / Software version / 10.0.10293.0" (Ultima exploración: 11/1/2017 10:56 AM/versión del software: 10.0.10293.0).](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate17.png)

