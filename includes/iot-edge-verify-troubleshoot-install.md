---
title: Comprobación de que la operación se ha realizado correctamente y solución de problemas después de la instalación y el aprovisionamiento
description: archivo de inclusión
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 10/06/2020
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 295769e5d4924e529b296dbb0b9d405ee197c1db
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91979326"
---
## <a name="verify-successful-setup"></a>Comprobación de la configuración correcta de la operación

Compruebe el estado del servicio IoT Edge. Debe aparecer como en ejecución.  

# <a name="linux"></a>[Linux](#tab/linux)

```bash
systemctl status iotedge
```

# <a name="windows"></a>[Windows](#tab/windows)

```powershell
Get-Service iotedge
```

---

Examine los registros del servicio.

# <a name="linux"></a>[Linux](#tab/linux)

```bash
journalctl -u iotedge --no-pager --no-full
```

# <a name="windows"></a>[Windows](#tab/windows)

Si acaba de instalar el runtime de IoT Edge, puede ver una lista de errores desde el tiempo transcurrido entre la ejecución de **Deploy-IoTEdge** e **Initialize IoTEdge**. Estos errores son esperables, ya que el servicio está intentando iniciar antes de que se haya configurado.

```powershell
. {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

---

Ejecute la [herramienta para la solución de problemas](../articles/iot-edge/troubleshoot.md#run-the-check-command) para consultar los errores de configuración y redes más comunes.

```powershell
iotedge check
```

Hasta que implemente el primer módulo en IoT Edge en el dispositivo, el módulo del sistema **$edgeHub** no se implementará en el dispositivo. Como resultado, la comprobación automatizada devolverá un error para la comprobación de conectividad de `Edge Hub can bind to ports on host`. Este error se puede omitir a menos que se produzca después de implementar un módulo en el dispositivo.

Por último, enumere los módulos en ejecución:

```powershell
iotedge list
```

Después de una instalación nueva, el único módulo que debería en ejecución es **edgeAgent**.

## <a name="tips-and-troubleshooting"></a>Sugerencias y solución de problemas

En dispositivos con recursos limitados, se recomienda encarecidamente establecer la variable de entorno *OptimizeForPerformance* en *false*, tal como se indica en las instrucciones de la [guía para la solución de problemas](../articles/iot-edge/troubleshoot.md).

Si el dispositivo no se puede conectar a IoT Hub y la red tiene un servidor proxy, siga los pasos descritos en [Configuración de un dispositivo de IoT Edge para que se comunique a través de un servidor proxy](../articles/iot-edge/how-to-configure-proxy-support.md) para instalar e iniciar el entorno de ejecución de Azure IoT Edge.

# <a name="linux"></a>[Linux](#tab/linux)

En dispositivos Linux, necesita privilegios elevados para ejecutar comandos `iotedge`. Después de instalar el entorno de ejecución, cierre la sesión en la máquina e iníciela de nuevo para actualizar sus permisos automáticamente. Hasta entonces, use `sudo` para ejecutar comandos con privilegios elevados.

# <a name="windows"></a>[Windows](#tab/windows)

En los dispositivos Windows que ejecutan contenedores de Windows, el motor de contenedor de Moby se ha instalado como parte de IoT Edge. El motor de Moby se ha diseñado para ejecutarse en paralelo con Docker Desktop. Puede usar comandos `docker` si quiere interactuar directamente con los contenedores del dispositivo. Sin embargo, debe establecer como destino el motor de Moby de forma específica en caso de que Docker Desktop también esté instalado en el dispositivo.

Por ejemplo, para ver una lista de todas las imágenes de Docker, use el comando siguiente:

```powershell
docker images
```

Para ver una lista de todas las imágenes de Moby, modifique el mismo comando con un puntero al motor de Moby:

```powershell
docker -H npipe:////./pipe/iotedge_moby_engine images
```

El identificador URI del motor se muestra en la salida del script de instalación, pero también puede encontrarlo en la sección de configuración del runtime del contenedor para el archivo config.yaml.

![uri de moby_runtime en config.yaml](./media/iot-edge-verify-troubleshoot-install/moby-runtime-uri.png)

---
