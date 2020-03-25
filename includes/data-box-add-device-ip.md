---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/07/2018
ms.author: alkohli
ms.openlocfilehash: e4b366075cb16f62a0e16b5b06da6fb19ffefdb9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "67186980"
---
1. Inicio de sesión en el dispositivo Data Box. Asegúrese de que está desbloqueado.

    ![Panel de Data Box](media/data-box-add-device-ip/data-box-connect-via-rest-1.png)

2. Vaya a **Establecer interfaces de red**. Anote la dirección IP del dispositivo de la interfaz de red utilizada para conectar con el cliente.

    ![Panel de Data Box](media/data-box-add-device-ip/data-box-connect-via-rest-2.png)

3. Vaya a **Conectar y copiar** y haga clic en **REST**.

    ![Panel de Data Box](media/data-box-add-device-ip/data-box-connect-via-rest-3.png)

4. En el cuadro de diálogo **Acceder a la cuenta de almacenamiento y cargar datos**, copie el valor de **Punto de conexión de Blob service**.

    ![Panel de Data Box](media/data-box-add-device-ip/data-box-connect-via-rest-4.png)

5. Inicie el **Bloc de notas** como administrador y abra el archivo **hosts** que se encuentra en `C:\Windows\System32\Drivers\etc`.
6. Agregue la entrada siguiente a su archivo **hosts**: `<device IP address> <Blob service endpoint>`
7. Como referencia, utilice la siguiente imagen. Guarde el archivo **hosts**.

    ![Panel de Data Box](media/data-box-add-device-ip/data-box-connect-via-rest-5.png)
