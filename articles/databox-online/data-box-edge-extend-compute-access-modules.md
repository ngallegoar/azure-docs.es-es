---
title: Administrar la red procesos en Azure Data Box Edge para obtener acceso a los módulos | Microsoft Docs
description: Aquí se describe cómo extender la red de procesos en Data Box Edge para obtener acceso a los módulos a través de una IP externa.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 05/17/2019
ms.author: alkohli
ms.openlocfilehash: 907647725dd6795b3b6482476de7442fbbf66114
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "65917241"
---
# <a name="enable-compute-network-on-your-azure-data-box-edge"></a>Habilitar la red de procesos en Azure Data Box Edge

En este artículo se describe cómo los módulos que se ejecutan en Azure Data Box Edge pueden obtener acceso a la red de procesos habilitada en el dispositivo.

Para configurar la red, deberá seguir los siguientes pasos:

- Habilite una interfaz de red en su dispositivo Data Box Edge para realizar el proceso.
- Agregue un módulo para obtener acceder a la red de procesos en Data Box Edge.
- Compruebe que el módulo pueda acceder a la interfaz de red habilitada.

En este tutorial usará un módulo de aplicaciones de servidor web para demostrar el escenario.

## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar, necesitará:

- Un dispositivo Data Box Edge con la configuración del dispositivo completada.
- Ya completó el paso **Configurar el proceso** según el [tutorial: transformar los datos con Azure Data Box Edge](data-box-edge-deploy-configure-compute-advanced.md#configure-compute) en su dispositivo. Su dispositivo debe tener un recurso de IoT Hub asociado, un dispositivo IoT y un dispositivo IoT Edge.

## <a name="enable-network-interface-for-compute"></a>Habilitar una interfaz de red para el proceso

Para obtener acceso a los módulos que se ejecutan en el dispositivo a través de una red externa, deberá asignar una dirección IP a una interfaz de red en el dispositivo. Esta configuración del proceso se puede administrar desde la interfaz de usuario web local.

Realice los pasos siguientes en la interfaz de usuario web local para configurar el proceso.

1. En la interfaz de usuario web local, vaya a **Configuración > Configuración del proceso**.  

2. **Habilite** la interfaz de red que quiera usar para conectarse al módulo de procesos que ejecutará en el dispositivo.

    - Si usa direcciones IP estáticas, escriba una dirección IP para la interfaz de red.
    - Si usa DHCP, las direcciones IP se asignan automáticamente. En este ejemplo se usa DHCP.

    ![Habilitar la configuración del proceso 1](media/data-box-edge-extend-compute-access-modules/enable-compute-setting-1.png)

3. Seleccione **Aplicar** para aplicar la configuración. Tome nota de la dirección IP asignada a la interfaz de red si usa DHCP.

    ![Habilitación de la configuración del proceso](media/data-box-edge-extend-compute-access-modules/enable-compute-setting-2.png)

## <a name="add-webserver-app-module"></a>Agregar el módulo de aplicaciones de servidor web

Siga los siguientes pasos para agregar un módulo de aplicaciones de servidor web en su dispositivo Data Box Edge.

1. Vaya al recurso de IoT Hub asociado al dispositivo Data Box Edge y seleccione el **dispositivo IoT Edge**.
2. Seleccione el dispositivo IoT Edge asociado al dispositivo Data Box Edge. En los **detalles del dispositivo**, seleccione **Establecer módulos**. En **Agregar módulos**, seleccione **+ Agregar** y **módulo de IoT Edge**.
3. En la hoja de los **módulos personalizados de IoT Edge**:

    1. Especifique un **nombre** para el módulo de aplicaciones del servidor web que quiera implementar.
    2. Proporcione un **URI de imagen** para la imagen del módulo. Se recuperará un módulo que coincide con el nombre proporcionado y las etiquetas. En ese caso, `nginx:stable` extraerá una imagen nginx estable (etiquetada como estable) del [repositorio público de Docker](https://hub.docker.com/_/nginx/).
    3. En el cuadro **Opciones de creación del contenedor**, puede pegar el ejemplo de código siguiente:  

        ```
        {
            "HostConfig": {
                "PortBindings": {
                    "80/tcp": [
                        {
                            "HostPort": "8080"
                        }
                    ]
                }
            }
        }
        ```

        Esta configuración le permite acceder al módulo usando la IP de la red de proceso sobre *http* en el puerto TCP 8080 (con el puerto del servidor web predeterminado establecido en 80).

        ![Especifique la información del puerto en la hoja de módulo personalizado de IoT Edge.](media/data-box-edge-extend-compute-access-modules/module-information.png)

    4. Seleccione **Guardar**.

## <a name="verify-module-access"></a>Comprobar el acceso del módulo

1. Compruebe que el módulo se haya implementado correctamente y se esté ejecutando. En la página de **detalles del dispositivo**, en la pestaña **Módulos**, el estado de ejecución del módulo debe ser **en ejecución**.  
2. Conéctese al módulo de aplicaciones del servidor web. Abra una ventana del navegador y escriba:

    `http://<compute-network-IP-address>:8080`

    Debería ver que la aplicación del servidor web se está ejecutando.

    ![Compruebe la conexión al módulo sobre el puerto especificado.](media/data-box-edge-extend-compute-access-modules/verify-connect-module-1.png)

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [administrar usuarios desde Azure Portal](data-box-edge-manage-users.md).
