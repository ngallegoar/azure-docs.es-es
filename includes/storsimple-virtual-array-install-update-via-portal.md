---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: df572b88cf8a67163b28ec87154dcea01646b9a2
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2020
ms.locfileid: "95554840"
---
#### <a name="to-install-updates-via-the-azure-portal"></a>Para instalar actualizaciones mediante Azure Portal

1. Vaya a su Administrador de dispositivos de StorSimple y seleccione **Dispositivos**. En la lista de dispositivos conectados al servicio, seleccione y haga clic en el dispositivo que desea actualizar. 

    ![En Recientes, el Administrador de dispositivos MySSDevManager está resaltado y seleccionado, Dispositivos está resaltado y seleccionado y el dispositivo MYSSIS1103 está resaltado y seleccionado.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate1m.png) 

2. En la hoja **Configuración**, haga clic en **Actualizaciones del dispositivo**. 

    ![Aparece la información del dispositivo MYSSIS1103. En Configuración, las Actualizaciones del dispositivo están resaltadas.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate2m.png)  

3. Verá un mensaje si hay actualizaciones de software disponibles. Para buscar actualizaciones, también puede hacer clic en **Examinar**.

    ![En el panel Actualizaciones del dispositivo, está resaltado el botón Examinar. Se muestra un mensaje de cuatro líneas: Último análisis / No analizado / Versión del software / 10.0.10280.0.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate3m.png)

    Se le notificará cuando se inicie el examen y se complete correctamente.

    ![La notificación se indica "Buscando actualizaciones para el dispositivo MySSIS1103.2:12 PM / La operación se completó correctamente".](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate5m.png)

4. Una vez examinadas las actualizaciones, haga clic en **Descargar actualizaciones**. 

    ![En el panel Actualizaciones del dispositivo se indica "Nuevas actualizaciones disponibles" y "Último análisis / 03/11/2016 2:12 PM/ Versión del software / 10.0.10280.0". La versión está resaltada.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate6m.png)

5. En la hoja **Nuevas actualizaciones**, revise la información y, una vez descargadas las actualizaciones, debe confirmar la instalación. Haga clic en **OK**.

    ![En el panel Actualizaciones nuevas se indica "Una vez descargadas las actualizaciones, deberá confirmar la instalación". El botón Aceptar está resaltado.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate7m.png)

6. Se le notificará cuando se inicie la carga y se complete correctamente.

     ![La notificación indica "Descargando actualizaciones para el dispositivo MySSIS... 2:13 PM".](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate8m.png)

5. En la hoja **Actualizaciones del dispositivo**, haga clic en **Instalar**.

     ![En el panel Actualizaciones del dispositivo se muestra el mensaje "Confirme la instalación de actualizaciones". El botón Instalar está resaltado.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate11m.png)   

6. En la hoja **Nuevas actualizaciones**, se le advertirá de que la actualización puede ser perjudicial. Puesto que la matriz virtual es un dispositivo de un solo nodo, el dispositivo se reiniciará una vez actualizado. Se interrumpirá cualquier E/S en curso. Haga clic en **Aceptar** para instalar las actualizaciones. 

    ![El mensaje en el panel Nuevas actualizaciones indica que "El dispositivo tendrá un tiempo de inactividad cuando se instalen estas actualizaciones". El botón Aceptar está resaltado.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate12m.png) 

7. Se le notificará cuando se inicie el trabajo de instalación. 

    ![La notificación indica "Instalando actualizaciones para el dispositivo MYSSIS1103". 2:15 PM.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate13m.png)

8.  Cuando el trabajo de instalación finalice correctamente, haga clic en el vínculo **Ver trabajo**, en la hoja **Actualizaciones del dispositivo**, para supervisar la instalación. 

    ![En el panel Actualizaciones de dispositivos, el vínculo con la etiqueta "Instalando actualizaciones. Ver trabajo" está resaltado.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate15m.png)

    Esto le lleva a la hoja **Instalar actualizaciones**. Puede ver información detallada sobre el trabajo aquí.

    ![El panel Instalar actualizaciones tiene información de instalación, incluido el dispositivo, el estado, la duración, la hora de inicio y la hora de detención.](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate16m.png)

9. Una vez instaladas correctamente las actualizaciones, verá un mensaje que lo indica en la hoja Actualizaciones del dispositivo. La versión de software también cambia a **10.0.10288.0**. 

    ![El panel Actualizaciones del dispositivo muestra el texto "El dispositivo está actualizado" y especifica la versión de software (10.0.10288.0).](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate17m.png)
