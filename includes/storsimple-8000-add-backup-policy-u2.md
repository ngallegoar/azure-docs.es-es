---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 02274bacb66a33ef54e07bc8113d7db46d4d5296
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "67186505"
---
#### <a name="to-add-a-storsimple-backup-policy"></a>Para agregar una directiva de copia de seguridad de StorSimple

1. Vaya al dispositivo StorSimple y haga clic en **Directiva de copia de seguridad**.

2. En la hoja **Directiva de copia de seguridad**, haga clic en **+ Agregar directiva** en la barra de comandos.
   
    ![Agregar una directiva de copia de seguridad](./media/storsimple-8000-add-backup-policy-u2/addbupol1.png)

3. En la hoja **Crear directiva de copia de seguridad**, realice los siguientes pasos:
   
   1. El campo **Seleccionar dispositivo** se rellena automáticamente según el dispositivo seleccionado.
   
   2. Especifique un **nombre** que tenga entre 3 y 150 caracteres para la directiva de copia de seguridad. Una vez creada la directiva, no se puede cambiar el nombre de la misma.
       
   3. Para asignar volúmenes a esta directiva de copia de seguridad, seleccione **Agregar volúmenes** y, en la lista tabular de volúmenes, haga clic en las casillas para asignar uno o más volúmenes.

       ![Agregar una directiva de copia de seguridad](./media/storsimple-8000-add-backup-policy-u2/addbupol2.png)

   4. Para definir una programación para esta directiva de copia de seguridad, haga clic en la **primera programación** y luego modifique los parámetros siguientes:

       ![Agregar una directiva de copia de seguridad](./media/storsimple-8000-add-backup-policy-u2/addbupol3.png)

       1. En **Tipo de instantánea**, seleccione **Nube** o **Local**.

       2. Indique la frecuencia de las copias de seguridad (especifique un número y elija **Días** o **Semanas** en la lista desplegable).

       3. Especifique una programación de retención.

       4. Escriba una fecha y hora para que comience la directiva de copia de seguridad.

       5. Haga clic en **Aceptar** para definir la programación.

   5. Haga clic en **Crear** para crear una directiva de copia de seguridad.

       ![Agregar una directiva de copia de seguridad](./media/storsimple-8000-add-backup-policy-u2/addbupol4.png)
   
   6. Cuando la directiva de copia de seguridad se haya creado, recibirá una notificación. La directiva recién agregada se muestra en la vista tabular en la hoja **Directiva de copia de seguridad**.

       ![Agregar una directiva de copia de seguridad](./media/storsimple-8000-add-backup-policy-u2/addbupol7.png)

