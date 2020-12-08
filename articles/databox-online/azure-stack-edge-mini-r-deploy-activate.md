---
title: 'Tutorial: Cifrado y activación del dispositivo Azure Stack Edge Mini R en Azure Portal | Microsoft Docs'
description: Este tutorial para implementar Azure Stack Edge Mini R explica cómo cifrar y activar el dispositivo físico.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/22/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to activate Azure Stack Edge Mini R so I can use it to transfer data to Azure.
ms.openlocfilehash: e5076888e692ad16e89f30d455e7f8c4e22a41b9
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96465103"
---
# <a name="tutorial-activate-azure-stack-edge-mini-r"></a>Tutorial: Activación de Azure Stack Edge Mini R

Este tutorial describe cómo activar el dispositivo Azure Stack Edge Mini R mediante la interfaz de usuario web local.

El proceso de activación puede tardar alrededor de 15 minutos.

En este tutorial, aprendió sobre lo siguiente:

> [!div class="checklist"]
> * Requisitos previos
> * Activación del dispositivo físico

## <a name="prerequisites"></a>Requisitos previos

Antes de configurar e instalar el dispositivo Azure Stack Edge Mini R, asegúrese de lo siguiente:

* Para el dispositivo físico: 
    
    - Ha instalado el dispositivo físico tal y como se describe en [Instalación de Azure Stack Edge Mini R](azure-stack-edge-mini-r-deploy-install.md).
    - Ha configurado la red y los valores de red de proceso tal como se detalla en [Configuración de la red, la red de proceso y el proxy web](azure-stack-edge-mini-r-deploy-configure-network-compute-web-proxy.md)
    - Ha cargado sus propios certificados en el dispositivo si el nombre del dispositivo o el dominio DNS se cambiaron a través de la página **Dispositivo**. Estos pasos se describen en [Configuración de los certificados, la VPN y el cifrado en reposo](azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption.md). Si no ha realizado este paso, se bloqueará la activación.
    - Ha configurado el cifrado en reposo para el dispositivo. Si no ha realizado este paso, verá un error durante la activación del dispositivo y se bloqueará la activación. Para más información, consulte [Configuración de los certificados, la VPN y el cifrado en reposo](azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption.md).
    
* Dispone de la clave de activación del servicio Azure Stack Edge que creó para administrar el dispositivo Azure Stack Edge Mini R. Para más información, consulte [Preparación de la implementación de Azure Stack Edge Mini R](azure-stack-edge-mini-r-deploy-prep.md).


## <a name="activate-the-device"></a>Activación del dispositivo

1. En la interfaz de usuario web local del dispositivo, vaya a la página **Introducción**.
2. En el icono **Activación**, seleccione **Activar**. 

    ![Página "Detalles de la nube" de la interfaz de usuario web local 1](./media/azure-stack-edge-mini-r-deploy-activate/activation-1.png)
    
3. En el panel de **activación**:
    1. Escriba la **clave de activación** que obtuvo en [Get the activation key for Azure Stack Edge Pro R](azure-stack-edge-pro-r-deploy-prep.md#get-the-activation-key) (Obtener la clave de activación para Azure Stack Edge Pro R).

    1. Puede habilitar la recopilación proactiva de registros para que Microsoft recopile los registros según el estado de mantenimiento del dispositivo. Los registros recopilados de este modo se cargan en una cuenta de Azure Storage.
    
    1. Seleccione **Aplicar**.

    ![Página "Detalles de la nube" de la interfaz de usuario web local 2](./media/azure-stack-edge-mini-r-deploy-activate/activation-2.png)


5. En primer lugar, se activa el dispositivo. A continuación, se le pedirá que descargue el archivo de clave.
    
    ![Página "Detalles de la nube" de la interfaz de usuario web local 3](./media/azure-stack-edge-mini-r-deploy-activate/activation-3.png)
    
    Seleccione **Download and continue** (Descargar y continuar) y guarde el archivo *device-serial-no.json* en una ubicación segura fuera del dispositivo. **Este archivo de clave contiene las claves de recuperación del disco del sistema operativo y los discos de datos del dispositivo**. Estas claves pueden ser necesarias para facilitar la recuperación del sistema en el futuro.

    Este es el contenido del archivo *json*:

        
    ```json
    {
      "Id": "<Device ID>",
      "DataVolumeBitLockerExternalKeys": {
        "hcsinternal": "<BitLocker key for data disk>",
        "hcsdata": "<BitLocker key for data disk>"
      },
      "SystemVolumeBitLockerRecoveryKey": "<BitLocker key for system volume>",
      "SEDEncryptionExternalKey": "xZM/vC7GxjqHZ3VMo7xSs/wH9rRsF/PNM+mOsZ+GaL0=",
      "ServiceEncryptionKey": "<Azure service encryption key>"
    }
    ```        
 
    En la siguiente tabla se explican las diversas claves:
    
    |Campo  |Descripción  |
    |---------|---------|
    |`Id`    | Es el id. para el dispositivo.        |
    |`DataVolumeBitLockerExternalKeys`|Estas son las claves de BitLocker para los discos de datos y se usan para recuperar los datos locales en el dispositivo.|
    |`SystemVolumeBitLockerRecoveryKey`| Esta es la clave de BitLocker para el volumen del sistema. Esta clave ayuda con la recuperación de la configuración del sistema y de los datos del sistema para el dispositivo. |
    |`SEDEncryptionExternalKey`| Esta clave proporcionada por el usuario o generada por el sistema se usa para proteger las unidades de datos de cifrado automático que tienen el cifrado integrado. |
    |`ServiceEncryptionKey`| Esta clave protege los datos que fluyen a través del servicio de Azure. Esta clave garantiza que, si la seguridad del servicio Azure se pone en peligro, la información almacenada permanecerá segura. |

6. Vaya a la página **Información general**. El estado del dispositivo debería aparecer como **Activado**.

    ![Página "Detalles de la nube" de la interfaz de usuario web local 4](./media/azure-stack-edge-mini-r-deploy-activate/activation-4.png)
 
La activación del dispositivo se ha completado. Ahora puede agregar recursos compartidos en el dispositivo.

Si encuentra algún problema durante la activación, vaya a [Solucionar problemas de activación y errores de Azure Key Vault](azure-stack-edge-gpu-troubleshoot-activation.md#activation-errors).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió sobre lo siguiente:

> [!div class="checklist"]
> * Requisitos previos
> * Activación del dispositivo físico

Para aprender a transferir datos con su dispositivo Azure Stack Edge Mini R, consulte:

> [!div class="nextstepaction"]
> [Transferencia de datos con Azure Stack Edge Mini R](./azure-stack-edge-j-series-deploy-add-shares.md)
