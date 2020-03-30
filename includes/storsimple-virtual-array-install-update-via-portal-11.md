---
title: archivo de inclusión
description: archivo de inclusión
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 07/18/2018
ms.author: alkohli
ms.custom: include file
ms.openlocfilehash: 65d5a88f7b5d059deb633f062639e455c64ef2f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "67186839"
---
#### <a name="to-install-updates-via-the-azure-portal"></a>Para instalar actualizaciones mediante Azure Portal

1. Vaya a su Administrador de dispositivos de StorSimple y seleccione **Dispositivos**. En la lista de dispositivos conectados al servicio, seleccione y haga clic en el dispositivo que desea actualizar.

2. En **Configuración**, haga clic en **Actualizaciones del dispositivo**.  

3. Verá un mensaje si hay actualizaciones de software disponibles. Para buscar actualizaciones, también puede hacer clic en **Examinar**. Tome nota de la versión de software que se ejecuta. 

    ![actualizar dispositivo](../includes/media/storsimple-virtual-array-install-update-via-portal-11/azupdate3m1.png)

    Se le notificará cuando se inicie el análisis y se complete correctamente.
 
4. Una vez examinadas las actualizaciones, haga clic en **Descargar actualizaciones**. En **Nuevas actualizaciones**, revise las notas de la versión. Tenga en cuenta también que, una vez descargadas las actualizaciones, debe confirmar la instalación. Haga clic en **OK**.

    ![actualizar dispositivo](../includes/media/storsimple-virtual-array-install-update-via-portal-11/azupdate6m.png)

    Se le notificará cuando se inicie la carga y se complete correctamente.

5. En **Actualizaciones del dispositivo**, haga clic en **Instalar**.

     ![actualizar dispositivo](../includes/media/storsimple-virtual-array-install-update-via-portal-11/azupdate11m1.png)

6. En **Nuevas actualizaciones**, se le advertirá de que la actualización puede ser perjudicial. Puesto que la matriz virtual es un dispositivo de un solo nodo, el dispositivo se reiniciará una vez actualizado. Se interrumpirá cualquier E/S en curso. Haga clic en **Aceptar** para instalar las actualizaciones.

    ![actualizar dispositivo](../includes/media/storsimple-virtual-array-install-update-via-portal-11/azupdate12m.png)

    Se le notificará cuando se inicie el trabajo de instalación.

7.  Una vez que el trabajo de instalación se haya completado correctamente, haga clic en el vínculo **Ver trabajo**. Esta acción lo remite a la hoja **Instalar actualizaciones**. Puede ver información detallada sobre el trabajo aquí. 

    ![actualizar dispositivo](../includes/media/storsimple-virtual-array-install-update-via-portal-11/azupdate16m1.png)

8. Si empezó con una matriz virtual que ejecutaba la versión de software Update 1 (10.0.10296.0), entonces ahora ejecuta Update 1.1 y no debe hacer nada más. Puede omitir los demás pasos. 

    Si empezó con una matriz virtual que ejecutaba una versión de software Update 0.6 (10.0.10293.0), entonces ahora se realiza la actualización a Update 1.0. Verá otro mensaje que indica que las actualizaciones están disponibles. Repita los pasos 4 a 7 para instalar Update 1.1.

    

