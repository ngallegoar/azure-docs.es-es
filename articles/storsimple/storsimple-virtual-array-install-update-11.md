---
title: Instalación de Update 1.1 en StorSimple Virtual Array | Microsoft Docs
description: Se describe cómo aplicar actualizaciones con Azure Portal y la interfaz de usuario web local para StorSimple Virtual Array.
services: storsimple
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/18/2018
ms.author: alkohli
ms.openlocfilehash: 709794dcccf9b100cbb11381ac34cfb86238b22b
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94533998"
---
# <a name="install-update-11-on-your-storsimple-virtual-array"></a>Instalación de Update 1.1 en StorSimple Virtual Array

## <a name="overview"></a>Información general

En este artículo se describen los pasos requeridos para instalar Update 1.1 en StorSimple Virtual Array a través tanto de la interfaz de usuario web local como de Azure Portal.

Se aplican las actualizaciones o revisiones de software para mantener actualizada la matriz virtual de StorSimple. Antes de aplicar una actualización, se recomienda que desconecte primero los volúmenes o recursos compartidos en el host y, luego, el dispositivo. Esto minimizará la posibilidad de daños en los datos. Cuando los volúmenes o recursos compartidos están sin conexión, también debe realizar una copia de seguridad manual del dispositivo.

> [!IMPORTANT]
> - Update 1.1 se corresponde con la versión de software **10.0.10307.0** del dispositivo. Para obtener más información sobre cuáles son las novedades de esta actualización, vaya a las [Notas de la versión de Update 1.1](storsimple-virtual-array-update-11-release-notes.md).
>
> - Tenga en cuenta que al instalar una actualización o revisión, se reiniciará el dispositivo. Dado que la matriz virtual de StorSimple es un dispositivo de nodo único, se interrumpirá cualquier operación de E/S que esté en curso y el dispositivo permanecerá un rato inactivo.
>
> - Update 1.1 solo está disponible en Azure Portal si la matriz virtual ejecuta Update 1. Si su matriz virtual ejecuta una versión Update 0.6, primero debe instalar Update 1.0 y después aplicar Update 1.1.

## <a name="use-the-azure-portal"></a>Uso de Azure Portal

Si se ejecuta Update 0.2, o cualquier versión posterior, se recomienda instalar las actualizaciones mediante Azure Portal. El procedimiento del Portal requiere que el usuario examine, descargue e instale las actualizaciones. Dependiendo de la matriz virtual que ejecute la versión del software, la aplicación de la actualización a través de Azure Portal se realiza de una forma distinta.

 - Si la matriz virtual ejecuta Update 1, Azure Portal instala directamente Update 1.1 (10.0.10307.0) en el dispositivo. Este procedimiento tarda aproximadamente 10 minutos en completarse.
 - Si la matriz virtual ejecuta Update 0.6, la actualización se realiza en dos fases. Primero, Azure Portal instala Update 1.0 (10.0.10296.0) en su dispositivo. La matriz virtual se reinicia y entonces se instala Update 1.1 (10.0.10307.0) en el dispositivo desde Azure Portal. Este procedimiento tarda aproximadamente 15 minutos en completarse.


[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-11.md)]

Una vez que la instalación esté completa, vaya a su servicio StorSimple Device Manager. Seleccione **Dispositivos** y, a continuación, seleccione y haga clic en el dispositivo que acaba de actualizar. Vaya a **Configuración > Administrar > Actualizaciones del dispositivo**. La versión de software que aparece debe ser **10.0.10307.0**.

![Versión de software después de la actualización](./media/storsimple-virtual-array-install-update-11/azupdate17m2.png)

## <a name="use-the-local-web-ui"></a>Uso de la interfaz de usuario web local

Se pueden seguir dos pasos con la interfaz de usuario web local:

* Descargar la actualización o la revisión
* Instalar la actualización o la revisión

> [!IMPORTANT] 
> **Continúe con esta actualización solo si se ejecuta Update 1 (10.0.10296.0). Si se ejecuta Update 0.6, primero [instale Update 1](storsimple-virtual-array-install-update-1.md) en el dispositivo y después instale Update 1.1.**

### <a name="download-the-update-or-the-hotfix"></a>Descargar la actualización o la revisión

Realice los pasos siguientes para descargar Update 1.1 desde el catálogo de Microsoft Update.

#### <a name="to-download-the-update-or-the-hotfix"></a>Para descargar la actualización o la revisión

1. Inicie Internet Explorer y vaya a [https://catalog.update.microsoft.com](https://catalog.update.microsoft.com).

2. Si esta es la primera vez que utiliza el Catálogo de Microsoft Update en este equipo, haga clic en **Instalar** cuando se le solicite que instale el complemento del Catálogo de Microsoft Update.

3. En el cuadro de búsqueda del Catálogo de Microsoft Update, escriba el número de Knowledge Base correspondiente a la revisión que quiera descargar. Escriba **4337628** para Update 1.1 y luego haga clic en **Buscar**.
   
    Aparece la lista de revisiones, por ejemplo, **StorSimple Virtual Array Update 1.1**.
   
    ![Catálogo de búsqueda](./media/storsimple-virtual-array-install-update-11/download1.png)

4. Haga clic en **Descargar**.

5. Descargue los dos archivos en una carpeta. También puede copiar la carpeta en un recurso compartido de red que sea accesible desde el dispositivo.

6. Abra la carpeta donde se encuentran los archivos.

    ![Archivos del paquete](./media/storsimple-virtual-array-install-update-11/update01folder.png)

    Verá dos archivos:
    -  Un archivo de paquete independiente de Microsoft Update `WindowsTH-KB3011067-x64`. Este archivo se utiliza para actualizar el software del dispositivo.
    - Un archivo que contiene las actualizaciones acumulativas de junio `Windows8.1-KB4284815-x64`. Para obtener más información sobre qué se incluye en este paquete acumulativo, vaya a [paquete acumulativo de seguridad mensual de junio](https://support.microsoft.com/help/4284815/windows-81-update-kb4284815).

### <a name="install-the-update-or-the-hotfix"></a>Instalar la actualización o la revisión

Antes de instalar la actualización o revisión, compruebe que:

 - Tiene la actualización o la revisión descargada localmente en el host o que puede acceder a ella a través de un recurso compartido de red.
 - La matriz virtual está ejecutando Update 1 (10.0.10296.0). Si está ejecutando Update 0.6, primero [instale Update 1](storsimple-virtual-array-install-update-1.md) y después instale Update 1.1.

Este procedimiento tarda aproximadamente 4 minutos en completarse. Realice los pasos siguientes para instalar la actualización o revisión.

#### <a name="to-install-the-update-or-the-hotfix"></a>Instalar la actualización o la revisión

1. En la interfaz de usuario web local, vaya a **Mantenimiento** > **Actualización de software**. Tome nota de la versión de software que se está ejecutando. **Continúe con esta actualización solo si se ejecuta Update 1 (10.0.10296.0). Si se ejecuta Update 0.6, primero [instale Update 1](storsimple-virtual-array-install-update-1.md) en el dispositivo y después instale Update 1.1.**
   
    ![Captura de pantalla que muestra Actualización de software seleccionada en el menú Mantenimiento.](./media/storsimple-virtual-array-install-update-11/update1m.png)

2. En **Update file path**(Ruta de acceso del archivo de actualización), escriba el nombre del archivo de actualización o de revisión. Asimismo, también puede acceder al archivo de instalación de la actualización o de la revisión si está en un recurso compartido de red. Haga clic en **Aplicar**.
   
    ![Captura de pantalla que muestra el cuadro de texto Actualizar ruta de acceso al archivo, en la página Actualización de software.](./media/storsimple-virtual-array-install-update-11/update2m.png)

3. Se mostrará una advertencia. Dado que la matriz virtual es un dispositivo de nodo único, una vez aplicada la actualización, se reiniciará el dispositivo y habrá un tiempo de inactividad. Haga clic en el icono de marca de verificación.
   
   ![Captura de pantalla que muestra un cuadro de diálogo de advertencia de tiempo de inactividad.](./media/storsimple-virtual-array-install-update-11/update3m.png)

4. Se inicia la actualización. Una vez que el dispositivo se actualice correctamente, este se reiniciará. La interfaz de usuario local no será accesible durante este tiempo.
   
    ![Captura de pantalla que muestra un mensaje de actualización correcta.](./media/storsimple-virtual-array-install-update-11/update5m.png)

5. Una vez completado el reinicio, se le llevará a la página de **inicio de sesión** . Para comprobar que el software del dispositivo se ha actualizado, en la interfaz de usuario de web local, vaya a **Mantenimiento** > **Actualización de software**. La versión de software que aparece debe ser **10.0.0.0.0.10307** para Update 1.1.
   
   > [!NOTE]
   > Las versiones de software se muestran de forma ligeramente distinta en la interfaz de usuario web local y Azure Portal. Por ejemplo, la misma versión aparece como **10.0.0.0.0.10307** en la interfaz de usuario web local y como **10.0.10307.0** en Azure Portal.
   
    ![Captura de pantalla que muestra la página Actualización de software con la versión de software actual.](./media/storsimple-virtual-array-install-update-11/update6m.png)

6. Repita los pasos del 2 al 4 para instalar la corrección de seguridad de Windows con el archivo `Windows8.1-KB4284815-x64`. La matriz virtual se reinicia después de la instalación. Debe iniciar sesión en la interfaz de usuario web local.


## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre la [administración de la matriz virtual de StorSimple](storsimple-ova-web-ui-admin.md).
