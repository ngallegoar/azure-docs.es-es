---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/25/2019
ms.author: alkohli
ms.openlocfilehash: 1d52117440028c75b249f469f2b3576c2ab1c5c5
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "67186979"
---
El paso final consiste en preparar el dispositivo para el envío. En este paso, se desconectan todos los recursos compartidos de dispositivo. Una vez que empieza este proceso, no se puede acceder a los recursos compartidos.

> [!IMPORTANT]
> La preparación para el envío es necesario, ya que marca datos que no cumplen las convenciones de nomenclatura de Azure. Si omite este paso, podría provocar un error de carga en los datos por falta de cumplimiento.

1. Vaya a **Prepare to ship** (Preparación para el envío) y haga clic en **Start preparation** (Iniciar preparación). De forma predeterminada, las sumas de comprobación se calculan mientras se copian los datos. Preparar para enviar completa el cálculo de la suma de comprobación y crea la lista de archivos (*Archivos -BOM*). El proceso de la suma de comprobación puede tardar de horas a días, dependiendo del tamaño de los datos. 
   
    ![Preparación para el envío 1](media/data-box-prepare-to-ship/prepare-to-ship1.png)

    Si por algún motivo desea detener la preparación del dispositivo, haga clic en **Detener preparación**. Puede reanudar la preparación para el envío más adelante.
        
    ![Preparación para el envío 2](media/data-box-prepare-to-ship/prepare-to-ship2.png)
    
2. La preparación para el envío se inicia y se desconectan los recursos compartidos del dispositivo. Verá un aviso para descargar la etiqueta de envío una vez que el dispositivo esté listo.

    ![Aviso de descarga de la etiqueta de envío](media/data-box-prepare-to-ship/download-shipping-label-reminder.png)

3. Una vez completada la preparación del dispositivo, su estado se actualiza a *Ready to ship* (Listo para enviar) y el dispositivo se bloquea.
        
    ![Preparación para el envío 3](media/data-box-prepare-to-ship/prepare-to-ship3.png)

    Si desea copiar más datos en el dispositivo, puede desbloquearlo, copiar más datos y ejecutar la preparación para el envío de nuevo.

    Si hay errores en este paso, deberá descargar el registro de errores y resolverlos. Una vez hecho esto, ejecute **Preparación para el envío**.

4. Una vez completada correctamente (sin errores) la preparación para el envío, descargue la lista de archivos (también conocida como el manifiesto) que se copiaron en este proceso. Más adelante, puede usar esta lista para comprobar los archivos se cargaron en Azure. Para más información, consulte [Inspeccionar BOM durante la preparación para el envío](../articles/databox/data-box-logs.md#inspect-bom-during-prepare-to-ship).
        
    ![Preparación para el envío 1](media/data-box-prepare-to-ship/prepare-to-ship4.png)

5. Apague el dispositivo. Vaya a la página **Shut down or restart** (Apagar o reiniciar) y haga clic en **Shut down** (Apagar). Cuando se le pida confirmación, haga clic en **OK** (Aceptar) para continuar.

6. Retire los cables. El siguiente paso es enviar el dispositivo a Microsoft.
