---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/07/2018
ms.author: alkohli
ms.openlocfilehash: 10feccb53b28181d260e7738ab99bdc2e3c9340c
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94553121"
---
Siga estos pasos para conectarse a la cuenta de almacenamiento y comprobar la conexión.

1. En el Explorador de Storage, seleccione **Connect to Azure Storage** (Conectar con Azure Storage). En el cuadro de diálogo **Connect to Azure Storage** (Conectar con Azure Storage), seleccione **Use a storage account name and key** (Usar un nombre y una clave de cuenta de almacenamiento).

    ![Captura de pantalla que muestra el cuadro de diálogo Conectar a Azure Storage, con la opción Usar una clave y un nombre de cuenta de almacenamiento seleccionada.](media/data-box-verify-connection/data-box-connect-via-rest-9.png)

2. Pegue los valores de **Account name** (Nombre de cuenta) y **Account key** (Clave de cuenta) (valor de la clave 1 en la página **Connect and copy** en la interfaz de usuario web local). Seleccione el dominio de puntos de conexión de Storage como **Other (enter below)** [Otros (indicar a continuación)] y, después, proporcione el punto de conexión de Blob service como se muestra a continuación. Active la opción **Use HTTP** (Usar HTTP) solo si la transferencia se va a hacer mediante *http*. Si usa *https*, deje la opción desactivada. Seleccione **Next** (Siguiente).

    ![Captura de pantalla que muestra el cuadro de diálogo Conectarse con el nombre y la clave con los valores especificados.](media/data-box-verify-connection/data-box-connect-via-rest-11.png)    

3. En el cuadro de diálogo **Connection Summary** (Resumen de conexiones), compruebe la información proporcionada. Seleccione **Conectar**.

    ![Captura de pantalla que muestra el cuadro de diálogo Resumen de conexiones con la opción Conectar seleccionada.](media/data-box-verify-connection/data-box-connect-via-rest-12.png)

4. La cuenta que ha agregado correctamente se muestra en el panel izquierdo del Explorador de Storage, con (External, Other) anexado al nombre. Haga clic en **Contenedores de blobs** para ver el contenedor.

    ![Captura de pantalla que muestra el menú Explorador con Contenedores de blobs seleccionados.](media/data-box-verify-connection/data-box-connect-via-rest-17.png)
