---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: e64db93347794ccfc85252205c6d92a68e904635
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2020
ms.locfileid: "93375841"
---
#### <a name="to-install-updates-via-the-azure-portal"></a>Para instalar actualizaciones mediante Azure Portal

1. Vaya a su Administrador de dispositivos de StorSimple y seleccione **Dispositivos**. En la lista de dispositivos conectados al servicio, seleccione y haga clic en el dispositivo que desea actualizar.

    ![actualizar dispositivo](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate1m.png) 

2. En la hoja **Configuración**, haga clic en **Actualizaciones del dispositivo**.

    ![Actualización de dispositivo 2](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate2m.png)  

3. Verá un mensaje si hay actualizaciones de software disponibles. Para buscar actualizaciones, también puede hacer clic en **Examinar**. Tome nota de la versión de software que se ejecuta. 

    ![Actualización de dispositivo 3](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate3m1.png)

    Se le notificará cuando se inicie el análisis y se complete correctamente.

    ![Actualización de dispositivo 4](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate5m.png)

4. Una vez examinadas las actualizaciones, haga clic en **Descargar actualizaciones**.

    ![Actualización de dispositivo 5](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate6m.png)

5. En la hoja **Nuevas actualizaciones**, revise las notas de la versión. Tenga en cuenta también que, una vez descargadas las actualizaciones, debe confirmar la instalación. Haga clic en **OK**.

    ![Actualización de dispositivo 6](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate7m.png)

6. Se le notificará cuando se inicie la carga y se complete correctamente.

     ![Actualización de dispositivo 7](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate8m.png)

5. En la hoja **Actualizaciones del dispositivo**, haga clic en **Instalar**.

     ![Actualización de dispositivo 8](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate11m1.png)

6. En la hoja **Nuevas actualizaciones**, se le advertirá de que la actualización puede ser perjudicial. Puesto que la matriz virtual es un dispositivo de un solo nodo, el dispositivo se reiniciará una vez actualizado. Se interrumpirá cualquier E/S en curso. Haga clic en **Aceptar** para instalar las actualizaciones.

    ![Actualización de dispositivo 9](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate12m.png)

7. Se le notificará cuando se inicie el trabajo de instalación.

    ![Actualización de dispositivo 10](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate13m.png)

8.  Cuando el trabajo de instalación finalice correctamente, haga clic en el vínculo **Ver trabajo**, en la hoja **Actualizaciones del dispositivo**, para supervisar la instalación. 

    ![Actualización de dispositivo 11](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate15m1.png)

    Esta acción lo remite a la hoja **Instalar actualizaciones**. Puede ver información detallada sobre el trabajo aquí.

    ![Actualización de dispositivo 12](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate16m1.png)

9. Si empezó con una matriz virtual que ejecutaba la versión de software Update 0.6 (10.0.10293.0), entonces ahora ejecuta Update 1 y no debe hacer nada más. Puede omitir los demás pasos. Si empezó con una matriz virtual que ejecutaba una versión de software anterior a Update 0.6 (10.0.10293.0), entonces ahora se realiza la actualización a Update 0.6. Verá otro mensaje que indica que las actualizaciones están disponibles. Repita los pasos 4 a 8 para instalar Update 1.

    ![Actualización de dispositivo 13](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate17.png)

