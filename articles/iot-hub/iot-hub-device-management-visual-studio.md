---
title: Administración de dispositivos IoT de Azure con Visual Studio Cloud Explorer
description: Use la herramienta Cloud Explorer para Visual Studio para la administración de dispositivos de Azure IoT Hub, que incluye métodos directos y opciones de administración de las propiedades deseadas de los dispositivos gemelos.
author: shizn
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: xshi
ms.openlocfilehash: 6fe5a45dda6632c56b3c6714827950e25e7d26af
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "73953179"
---
# <a name="use-cloud-explorer-for-visual-studio-for-azure-iot-hub-device-management"></a>Uso de Cloud Explorer para Visual Studio para la administración de dispositivos de Azure IoT Hub

![Diagrama integral](media/iot-hub-device-management-visual-studio/iot-e2e-simple.png)

[Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS) es una extensión útil de Visual Studio que le permite ver los recursos de Azure, inspeccionar sus propiedades y realizar acciones de desarrollador clave desde dentro de Visual Studio. Incluye opciones de administración que puede usar para realizar varias tareas.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| Opción de administración          | Tarea                    |
|----------------------------|--------------------------------|
| Métodos directos             | Hacer que un dispositivo actúe, por ejemplo, para iniciar o detener el envío de mensajes o reiniciar el dispositivo.                                        |
| Leer dispositivo gemelo           | Obtener el estado notificado de un dispositivo. Por ejemplo, el dispositivo informa de que el LED está parpadeando.                                    |
| Actualizar dispositivo gemelo         | Poner un dispositivo en determinados estados, como establecer un indicador LED en verde o establecer el intervalo de envío de telemetría en 30 minutos.         |
| Mensajes de nube a dispositivo   | Enviar notificaciones a un dispositivo. Por ejemplo, "Es muy probable que llueva hoy. No olvide traerse un paraguas".              |

Para obtener una explicación más detallada acerca de las diferencias y orientación sobre el uso de estas opciones, consulte la [Guía de comunicación de dispositivo a nube](iot-hub-devguide-d2c-guidance.md) y la [Guía de comunicación de nube a dispositivo](iot-hub-devguide-c2d-guidance.md).

Los dispositivos gemelos son documentos JSON que almacenan información acerca del estado del dispositivo, incluidos metadatos, configuraciones y condiciones. IoT Hub conserva un dispositivo gemelo por cada dispositivo que se conecta a él. Para más información acerca de los dispositivos gemelos, consulte [Introducción a los dispositivos gemelos](iot-hub-node-node-twin-getstarted.md).

## <a name="what-you-learn"></a>Conocimientos que adquirirá

En este artículo, aprenderá a usar Cloud Explorer para Visual Studio con distintas opciones de administración en su equipo de desarrollo.

## <a name="what-you-do"></a>Qué debe hacer

En este artículo, ejecutará Cloud Explorer para Visual Studio con distintas opciones de administración.

## <a name="what-you-need"></a>Lo que necesita

Los siguientes requisitos previos son necesarios:

- Una suscripción de Azure activa.

- Una instancia de Azure IoT Hub en su suscripción.

- Microsoft Visual Studio 2017 Update 9 o posterior. En este artículo se utiliza [Visual Studio 2017 o Visual Studio 2019](https://www.visualstudio.com/vs/).

- El componente Cloud Explorer desde el Instalador de Visual Studio (se selecciona de forma predeterminada con la carga de trabajo de Azure).

## <a name="update-cloud-explorer-to-latest-version"></a>Actualización de Cloud Explorer a la versión más reciente

El componente Cloud Explorer desde el Instalador de Visual Studio 2017 solo admite la supervisión de mensajes del dispositivo a la nube y de la nube al dispositivo. Para usar Visual Studio 2017, descargue e instale la versión más reciente de [Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS).

## <a name="sign-in-to-access-your-hub"></a>Inicio de sesión para acceder al centro

1. En Visual Studio, seleccione **Ver** > **Cloud Explorer** para abrir Cloud Explorer.

1. Seleccione el icono de administración de cuentas para mostrar las suscripciones.

    ![Icono de administración de cuentas](media/iot-hub-visual-studio-cloud-device-messaging/account-management-icon.png)

1. Si ha iniciado sesión en Azure, aparecerán las cuentas. Para iniciar sesión en Azure por primera vez, elija **Agregar una cuenta**.

1. Seleccione las suscripciones de Azure que quiere usar y seleccione **Aplicar**.

1. Expanda la suscripción y luego **IoT Hubs**.  En cada centro, puede ver sus dispositivos. Haga clic en un dispositivo para tener acceso a las opciones de administración.

    ![Opciones de administración](media/iot-hub-device-management-visual-studio/management-options-vs2019.png)

## <a name="direct-methods"></a>Métodos directos

Para usar métodos directos, realice los pasos siguientes:

1. Haga clic con el botón derecho en el dispositivo y seleccione **Invoke Device Direct Method** (Invocar método directo de dispositivo).

1. Escriba el nombre del método y la carga en**Invocar un método directo** y, a continuación, seleccione **Aceptar**.

    Los resultados aparecerán en **Resultado**.

## <a name="update-device-twin"></a>Actualizar dispositivo gemelo

Para editar un dispositivo gemelo, realice los pasos siguientes:

1. Haga clic con el botón derecho en el dispositivo y seleccione **Editar dispositivo gemelo**.

   Se abrirá un archivo **azure-iot-device-twin.json** con el contenido del dispositivo gemelo.

1. Realice algunas modificaciones en los campos **tags** o **properties.desired** del archivo **azure-iot-dispositivo-twin.json**.

1. Presione **CTRL+S** para actualizar el dispositivo gemelo.

   Los resultados aparecerán en **Resultado**.

## <a name="send-cloud-to-device-messages"></a>Envío de mensajes de nube a dispositivo

Para enviar un mensaje desde IoT Hub al dispositivo, siga estos pasos:

1. Haga clic con el botón derecho en el dispositivo y seleccione **Send C2D Message** (Enviar mensaje de C2D).

1. Escriba el mensaje en **Enviar mensaje C2D** y seleccione **Aceptar**.

   Los resultados aparecerán en **Resultado**.

## <a name="next-steps"></a>Pasos siguientes

Ha aprendido a utilizar Cloud Explorer para Visual Studio con distintas opciones de administración.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
