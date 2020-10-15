---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 06/19/2020
ms.author: alkohli
ms.openlocfilehash: 97491000c63c966c3d8159b9361fcb60e062aa49
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86208864"
---
El paso final consiste en preparar el dispositivo para el envío. En este paso, se desconectan todos los recursos compartidos de dispositivo. Una vez que empieza este proceso, no se puede acceder a los recursos compartidos. También puede optar por limpiar el dispositivo en el que los datos locales se borren de forma permanente. Una vez completado el paso, la pantalla de entrada manuscrita mostrará la etiqueta de envío de devolución.

> [!IMPORTANT]
> - Puede elegir borrar los datos del dispositivo de forma permanente en este paso y limpiar el dispositivo. Los datos de la cuenta de Azure Storage permanecerán y generarán cargos que se acumularán. Se recomienda eliminar estos datos solo después de comprobar que se ha completado su copia en un servidor de datos local.

1. Vaya a **Preparar para enviar** y seleccione **Iniciar preparación**. 
   
    ![Preparación para el envío 1](media/data-box-export-prepare-to-ship/prepare-to-ship1.png)

 
2. La preparación para el envío se inicia y se desconectan los recursos compartidos del dispositivo. De forma predeterminada, la limpieza del dispositivo se realiza y Data Box borra los datos de sus discos. 


    ![Preparación para el envío 2](media/data-box-export-prepare-to-ship/prepare-to-ship2.png)

    Para no realizar la limpieza, desactive el cuadro combinado. En este caso, los datos del dispositivo se borrarán a nivel del centro de datos más adelante.

    ![Preparación para el envío 3](media/data-box-export-prepare-to-ship/prepare-to-ship3.png)


3. Una vez iniciado **Preparar para enviar**, verá una notificación con una barra de progreso.

    ![Preparación para el envío 4](media/data-box-export-prepare-to-ship/prepare-to-ship4.png)

4. Una vez completada la preparación para el envío y la limpieza del dispositivo (si está seleccionada), la pantalla de entrada manuscrita del dispositivo también mostrará la etiqueta de envío de la devolución. 

    Además verá un número de referencia de finalización que se usa para diferentes acciones en función del país en el que se encuentre. También aparecerán las instrucciones de envío para la devolución. Las instrucciones varían según el tipo de envío elegido, administrado por Microsoft o por el usuario. 
        
    ![Preparación para el envío 5](media/data-box-export-prepare-to-ship/prepare-to-ship5.png)


5. Apague el dispositivo. Busque el botón de encendido en el panel frontal del dispositivo. Apagar el dispositivo.

    ![Botón de encendido del dispositivo Data Box](media/data-box-export-prepare-to-ship/data-box-powered-door-open.png)

6. Retire los cables. El siguiente paso es enviar el dispositivo a Microsoft.
