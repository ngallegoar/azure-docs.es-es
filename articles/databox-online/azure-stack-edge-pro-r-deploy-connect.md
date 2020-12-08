---
title: 'Tutorial: Conexión, configuración y activación de un dispositivo Azure Stack Edge Pro R en Azure Portal | Microsoft Docs'
description: Aprenda cómo conectarse al dispositivo Azure Stack Edge Pro R mediante la interfaz de usuario web local.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/15/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to connect and activate Azure Stack Edge Pro R so I can use it to transfer data to Azure.
ms.openlocfilehash: ce97c22cf4bfbe5cca01183574597706a8c239e7
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96464612"
---
# <a name="tutorial-connect-to-azure-stack-edge-pro-r"></a>Tutorial: Conexión a Azure Stack Edge Pro R

Este tutorial describe cómo conectarse al dispositivo Azure Stack Edge Pro R mediante la interfaz de usuario web local.

El proceso de conexión puede tardar unos 5 minutos en completarse.

En este tutorial, obtendrá información sobre lo siguiente:

> [!div class="checklist"]
>
> * Requisitos previos
> * A conectarse a un dispositivo físico


## <a name="prerequisites"></a>Requisitos previos

Antes de configurar e instalar el dispositivo Azure Stack Edge Pro R, asegúrese de lo siguiente:

* Ha instalado el dispositivo físico tal y como se describe en [Instalación de Azure Stack Edge Pro R](azure-stack-edge-pro-r-deploy-install.md).


## <a name="connect-to-the-local-web-ui-setup"></a>Conexión a la configuración de la interfaz de usuario web local

1. Configure el adaptador Ethernet en el equipo para conectarse al dispositivo Azure Stack Edge Pro R, con la dirección IP estática 192.168.100.5 y la subred 255.255.255.0.

2. Conecte el equipo a PUERTO 1 en el dispositivo. Si conecta el equipo directamente al dispositivo (sin un conmutador), use un cable cruzado o un adaptador Ethernet USB. Use la siguiente ilustración para identificar el PUERTO 1 en el dispositivo.

    ![Placa posterior de un dispositivo cableado](./media/azure-stack-edge-pro-r-deploy-install/backplane-cabled.png)


3. Abra una ventana del explorador y acceda a la interfaz de usuario web local del dispositivo en `https://192.168.100.10`.  
    Esta acción puede tardar unos minutos en completarse después de que haya activado el dispositivo.

    Ve un error o advertencia que indica que hay un problema con el certificado de seguridad del sitio web. 
   
    ![Mensaje de error del certificado de seguridad del sitio web](media/azure-stack-edge-pro-r-deploy-connect/connect-web-ui-1.png)


4. Seleccione **Continue to this webpage**(Continuar a esta página web).  
    Estos pasos pueden variar en función del explorador que se use.

5. Inicie sesión en la interfaz de usuario web del dispositivo. La contraseña predeterminada es *Password1*. 
   
    ![Página de inicio de sesión del dispositivo de Azure Stack Edge](media/azure-stack-edge-pro-r-deploy-connect/connect-web-ui-3.png)

6. En el aviso, cambie la contraseña del administrador de dispositivos.  
    Recuerde que la nueva contraseña debe tener entre 8 y 16 caracteres. Esta debe contener tres de los siguientes caracteres: mayúsculas, minúsculas, números y caracteres especiales.

Ahora está en la página **Información general** del dispositivo. El siguiente paso es configurar los valores de red del dispositivo.


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió sobre lo siguiente:

> [!div class="checklist"]
> * Requisitos previos
> * A conectarse a un dispositivo físico


Para aprender a configurar los valores de red del dispositivo Azure Stack Edge Pro R, consulte:

> [!div class="nextstepaction"]
> [Configuración de la red](./azure-stack-edge-pro-r-deploy-configure-network-compute-web-proxy.md)
