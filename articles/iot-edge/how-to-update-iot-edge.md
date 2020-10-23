---
title: 'Actualización de la versión de IoT Edge en los dispositivos: Azure IoT Edge | Microsoft Docs'
description: Cómo actualizar los dispositivos IoT Edge para ejecutar las versiones más recientes del demonio de seguridad y el entorno de ejecución de IoT Edge
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/22/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 7cde23372f6a3af0320e2d48c78a0d7fe69a2600
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92045710"
---
# <a name="update-the-iot-edge-security-daemon-and-runtime"></a>Actualice el archivo de configuración del demonio de seguridad y el entorno de ejecución de IoT Edge.

Dado que el servicio de IoT Edge lanza versiones nuevas, querrá actualizar los dispositivos IoT Edge para tener las últimas características y mejoras de seguridad. En este artículo se proporciona información sobre cómo actualizar los dispositivos IoT Edge cuando hay una versión nueva disponible.

Es necesario actualizar dos componentes de un dispositivo IoT Edge si quiere pasar a una versión más reciente. El primero es el demonio de seguridad, que se ejecuta en el dispositivo e inicia los módulos en tiempo de ejecución cuando se inicia el dispositivo. Actualmente, el demonio de seguridad solo puede actualizarse desde el propio dispositivo. El segundo componente es el entorno de ejecución, formado por los módulos Centro de IoT Edge y Agente de IoT Edge. En función del modo de estructurar la implementación, el entorno de ejecución puede actualizarse desde el dispositivo o de forma remota.

Para obtener la versión más reciente de Azure IoT Edge, consulte [Versiones de Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

## <a name="update-the-security-daemon"></a>Actualización del demonio de seguridad

El demonio de seguridad de IoT Edge es un componente nativo que debe actualizarse mediante el administrador de paquetes en el dispositivo IoT Edge.

Compruebe la versión del demonio de seguridad que se ejecuta en el dispositivo mediante el comando `iotedge version`.

### <a name="linux-devices"></a>Dispositivos Linux

En los dispositivos Linux x64, use apt-get o el administrador de paquetes adecuado para actualizar el demonio de seguridad a la versión más reciente.

Obtenga la configuración del repositorio más reciente de Microsoft:

* **Ubuntu Server 16.04**:

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/multiarch/prod.list > ./microsoft-prod.list
   ```

* **Ubuntu Server 18.04**:

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/18.04/multiarch/prod.list > ./microsoft-prod.list
   ```

* **Raspbian Stretch**:

   ```bash
   curl https://packages.microsoft.com/config/debian/stretch/multiarch/prod.list > ./microsoft-prod.list
   ```

Copie la lista generada.

   ```bash
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

Instale de la clave pública de GPG de Microsoft.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

Actualice apt.

   ```bash
   sudo apt-get update
   ```

Compruebe qué versiones de IoT Edge están disponibles.

   ```bash
   apt list -a iotedge
   ```

Si desea actualizar a la versión más reciente del demonio de seguridad, use el siguiente comando, que también actualiza **libiothsm-std** a la versión más reciente:

   ```bash
   sudo apt-get install iotedge
   ```

Si desea actualizar el demonio de seguridad a una versión específica, especifique la versión en la salida de la lista de apt. Cada vez que **iotedge** se actualiza, se intenta actualizar automáticamente el paquete **libiothsm-std** a su versión más reciente, lo que puede provocar un conflicto de dependencia. Si no va a la versión más reciente, asegúrese de tener como destino ambos paquetes para la misma versión. Por ejemplo, el siguiente comando instala una versión específica de la versión 1.0.9:

   ```bash
   sudo apt-get install iotedge=1.0.9-1 libiothsm-std=1.0.9-1
   ```

Si la versión que desea instalar no está disponible a través de apt-get, puede usar curl para tener como destino cualquier versión del repositorio de [versiones de IoT Edge](https://github.com/Azure/azure-iotedge/releases). Para cualquier versión que desee instalar, localice los archivos **libiothsm-std** y **iotedge** adecuados para el dispositivo. En cada archivo, haga clic con el botón derecho sobre el vínculo del archivo y copie la dirección del vínculo. Use la dirección del vínculo para instalar las versiones específicas de esos componentes:

```bash
curl -L <libiothsm-std link> -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb
curl -L <iotedge link> -o iotedge.deb && sudo dpkg -i ./iotedge.deb
```

### <a name="windows-devices"></a>Dispositivos Windows

En los dispositivos Windows, use el script de PowerShell para actualizar el demonio de seguridad. El script extrae automáticamente la versión más reciente del demonio de seguridad.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Update-IoTEdge -ContainerOs <Windows or Linux>
```

Al ejecutar el comando Update-IoTEdge se quita y actualiza el demonio de seguridad del dispositivo, junto con las dos imágenes de contenedor del entorno de ejecución. El archivo config.yaml se mantiene en el dispositivo, así como los datos del motor de contenedor Moby (si usa contenedores de Windows). Si conserva la información de configuración, no tendrá que volver a proporcionar la cadena de conexión o información de Device Provisioning Service para el dispositivo durante el proceso de actualización.

Si desea actualizar el demonio de seguridad a una versión específica, búsquela entre las [versiones de IoT Edge](https://github.com/Azure/azure-iotedge/releases). En esa versión, descargue el archivo **Microsoft-Azure-IoTEdge.cab**. A continuación, use el parámetro `-OfflineInstallationPath` para que apunte a la ubicación del archivo local. Por ejemplo:

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Update-IoTEdge -ContainerOs <Windows or Linux> -OfflineInstallationPath <absolute path to directory>
```

>[!NOTE]
>El parámetro `-OfflineInstallationPath` busca un archivo llamado **Microsoft-Azure-IoTEdge.cab** en el directorio proporcionado. A partir de la versión 1.0.9-rc4 de IoT Edge, dos archivos. cab están disponibles para su uso: uno para dispositivos AMD64 y otro para dispositivos ARM32. Descargue el archivo correcto para el dispositivo y, a continuación, cambie el nombre del archivo para quitar el sufijo de la arquitectura.

Para obtener más información sobre las opciones de autenticación, use el comando `Get-Help Update-IoTEdge -full` o consulte [Script de PowerShell para IoT Edge en Windows](reference-windows-scripts.md).

## <a name="update-the-runtime-containers"></a>Actualización de los contenedores del entorno de ejecución

El modo de actualizar los contenedores del Centro de IoT Edge y del Agente de IoT Edge depende de si usa etiquetas graduales (por ejemplo, 1.0) o etiquetas específicas (por ejemplo, 1.0.7) en la implementación.

Compruebe la versión de los módulos Agente de IoT Edge y Centro de IoT Edge instalada actualmente en el dispositivo mediante los comandos `iotedge logs edgeAgent` o `iotedge logs edgeHub`.

  ![Búsqueda de la versión del contenedor en los registros](./media/how-to-update-iot-edge/container-version.png)

### <a name="understand-iot-edge-tags"></a>Información sobre las etiquetas de IoT Edge

Las imágenes del Agente de IoT Edge y del centro de IoT Edge se etiquetan con la versión de IoT Edge a la que están asociadas. Hay dos maneras diferentes de usar etiquetas con las imágenes del entorno de ejecución:

* **Etiquetas graduales**: use solo los dos primeros valores del número de versión para obtener la imagen más reciente que coincida con esos dígitos. Por ejemplo, 1.0 se actualiza cada vez que hay una nueva versión para que apunte a la versión 1.0.x más reciente. Si el entorno de ejecución del contenedor en el dispositivo IoT Edge extrae la imagen de nuevo, se actualizan los módulos del entorno de ejecución a la versión más reciente. Este enfoque se recomienda para fines de desarrollo. Las implementaciones de Azure Portal tienen como valor predeterminado las etiquetas graduales.

* **Etiquetas específicas**: use los tres valores del número de versión para establecer explícitamente la versión de la imagen. Por ejemplo, la versión 1.0.7 no cambiará después de su lanzamiento inicial. Puede declarar un nuevo número de versión del manifiesto de implementación cuando esté listo para actualizar. Este enfoque se recomienda para fines de producción.

### <a name="update-a-rolling-tag-image"></a>Actualización de una imagen de etiqueta gradual

Si usa etiquetas graduales en la implementación (por ejemplo, mcr.microsoft.com/azureiotedge-hub:**1.0**), deberá forzar el entorno de ejecución del contenedor en el dispositivo para extraer la versión más reciente de la imagen.

Elimine la versión local de la imagen del dispositivo IoT Edge. En equipos con Windows, al desinstalar el demonio de seguridad también se quitan las imágenes en tiempo de ejecución, por lo que no es necesario volver a realizar este paso.

```bash
docker rmi mcr.microsoft.com/azureiotedge-hub:1.0
docker rmi mcr.microsoft.com/azureiotedge-agent:1.0
```

Es posible que tenga que usar la marca `-f` de fuerza para eliminar las imágenes.

El servicio de IoT Edge extraerá las versiones más recientes de las imágenes del entorno de ejecución y volverá a iniciarlas automáticamente en el dispositivo.

### <a name="update-a-specific-tag-image"></a>Actualización de una etiqueta específica

Si usa etiquetas específicas en la implementación (por ejemplo, mcr.microsoft.com/azureiotedge-hub:**1.0.8**), solo tiene que actualizar la etiqueta en el manifiesto de implementación y aplicar los cambios en el dispositivo.

1. En IoT Hub en Azure Portal, seleccione el dispositivo IoT Edge y luego **Establecer módulos**.

1. En la sección **Módulos de IoT Edge**, seleccione **Configuración del entorno de ejecución**.

   ![Configuración de los parámetros de ejecución](./media/how-to-update-iot-edge/configure-runtime.png)

1. En **Configuración del entorno de ejecución**, actualice el valor **Image** del **Centro de Microsoft Edge** con la versión deseada. No seleccione **Guardar** todavía.

   ![Actualización de la versión de la imagen del Centro de Microsoft Edge](./media/how-to-update-iot-edge/runtime-settings-edgehub.png)

1. Contraiga la configuración del **Centro de Microsoft Edge**, o desplácese hacia abajo, y actualice el valor de **Image** para el **Agente de Edge** con la misma versión deseada.

   ![Actualización de la versión del agente del Centro de Microsoft Edge](./media/how-to-update-iot-edge/runtime-settings-edgeagent.png)

1. Seleccione **Guardar**.

1. Seleccione **Revisar y crear**, revise la implementación y, después, seleccione **Crear**.

## <a name="update-offline-or-to-a-specific-version"></a>Actualización sin conexión o a una versión específica

Si desea actualizar un dispositivo sin conexión o actualizar a una versión específica de IoT Edge en lugar de a la última versión, puede hacerlo con el parámetro `-OfflineInstallationPath`.

Se usan dos componentes para actualizar un dispositivo IoT Edge:

* Un script de PowerShell, que contiene las instrucciones de instalación.
* Un archivo .cab de Microsoft Azure IoT Edge, que contiene el demonio de seguridad de IoT Edge (iotedged), el motor del contenedor Moby y la CLI de Moby.

1. Para obtener los archivos de instalación de IoT Edge más recientes, junto con las versiones anteriores, vea las [versiones de Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

2. Busque la versión que desea instalar y descargue los siguientes archivos de la sección **Recursos** de las notas de la versión en el dispositivo IoT:

   * IoTEdgeSecurityDaemon.ps1
   * Microsoft-Azure-IoTEdge-amd64.cab de las versiones 1.0.9 o posteriores, o bien Microsoft-Azure-IoTEdge.cab de las versiones 1.0.8 y anteriores.

   Microsoft-Azure-IotEdge-arm32.cab también está disponible a partir de la versión 1.0.9 con fines de prueba exclusivamente. IoT Edge no es compatible actualmente con dispositivos ARM32 de Windows.

   Es importante usar el script de PowerShell de la misma versión que el archivo .cab que se usa, porque la funcionalidad cambia para admitir las características de cada versión.

3. Si el archivo .cab que descargó tiene un sufijo de arquitectura, cambie el nombre del archivo a **Microsoft-Azure-IoTEdge.cab**.

4. Para actualizar con componentes sin conexión, [use el operador punto](/powershell/module/microsoft.powershell.core/about/about_scripts?view=powershell-7#script-scope-and-dot-sourcing) en la copia local del script de PowerShell. A continuación, use el parámetro `-OfflineInstallationPath` como parte del comando `Update-IoTEdge` y proporcione la ruta de acceso absoluta al directorio de archivos. Por ejemplo,

   ```powershell
   . <path>\IoTEdgeSecurityDaemon.ps1
   Update-IoTEdge -OfflineInstallationPath <path>
   ```

## <a name="update-to-a-release-candidate-version"></a>Actualización a una versión candidata para lanzamiento

Azure IoT Edge lanza periódicamente nuevas versiones del servicio IoT Edge. Antes de cada versión estable, se publican una o varias versiones candidatas para lanzamiento (RC). Las versiones RC incluyen todas las características planeadas para la versión, pero aún se están sometiendo a pruebas y validación. Si quiere probar una nueva característica de manera anticipada, puede instalar una versión RC y dejar comentarios a través de GitHub.

Las versiones candidatas para lanzamiento siguen la misma convención de numeración que las versiones, pero tienen **-rc** más un número incremental anexados al final. Puede ver las versiones candidatas para lanzamiento en la misma lista de [versiones de Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases) que las versiones estables. Por ejemplo, busque **1.0.9-rc5** y **1.0.9-rc6**, dos de las versiones candidatas para lanzamiento anteriores a la versión **1.0.9**. También puede ver que las versiones RC se marcan con etiquetas de **versión preliminar**.

Los módulos agente y centro de IoT Edge tienen versiones RC que se etiquetan siguiendo la misma convención. Por ejemplo, **mcr.microsoft.com/azureiotedge-hub:1.0.9-rc6**.

Igual que las versiones preliminares, las versiones candidatas para lanzamiento no se incluyen como la versión más reciente que utilizan los instaladores habituales. En su lugar, debe especificar manualmente los recursos para la versión RC que quiere probar. Por lo general, la instalación o actualización de una versión RC equivale a seleccionar cualquier otra versión específica de IoT Edge.

Use las secciones de este artículo para obtener información sobre cómo actualizar un dispositivo IoT Edge a una versión específica del demonio de seguridad o de los módulos del entorno de ejecución.

Si va a instalar IoT Edge, en lugar de actualizar una instalación existente, use los pasos de la [instalación sin conexión o específica de la versión](how-to-install-iot-edge.md#offline-or-specific-version-installation).

## <a name="next-steps"></a>Pasos siguientes

Consulte las últimas [versiones de Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

Permanezca actualizado con los anuncios y las actualizaciones recientes del [blog de Internet de las cosas](https://azure.microsoft.com/blog/topics/internet-of-things/).