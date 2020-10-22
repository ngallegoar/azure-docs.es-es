---
title: Instalación de Azure IoT Edge | Microsoft Docs
description: Instrucciones de instalación de Azure IoT Edge en dispositivos Windows o Linux
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 10/07/2020
ms.author: kgremban
ms.openlocfilehash: 3a02459f5b92aa7d708c29c737ed9428ed14215a
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92045693"
---
# <a name="install-or-uninstall-the-azure-iot-edge-runtime"></a>Instalación o desinstalación del entorno de ejecución de Azure IoT Edge

El entorno de ejecución de Azure IoT Edge es lo que convierte a un dispositivo en un dispositivo IoT Edge. El entorno de ejecución se puede implementar en dispositivos tan pequeños como un Raspberry Pi o tan grandes como un servidor industrial. Una vez que un dispositivo está configurado con el entorno de ejecución de IoT Edge, puede empezar a implementar lógica de negocios en él desde la nube. Para obtener más información, consulte [Información del entorno de ejecución de Azure IoT Edge y su arquitectura](iot-edge-runtime.md).

Hay dos pasos para configurar un dispositivo IoT Edge. El primer paso consiste en instalar el entorno de ejecución y sus dependencias, lo cual se trata en este artículo. El segundo paso es conectar el dispositivo a su identidad en la nube y configurar la autenticación con IoT Hub. Estos pasos se describen en los siguientes artículos.

En este artículo se indican los pasos para instalar el entorno de ejecución de Azure IoT Edge en dispositivos Linux o Windows. En el caso de los dispositivos Windows, tiene la opción adicional de usar contenedores de Linux o Windows. Actualmente, los contenedores de Windows se recomiendan para los escenarios de producción en Windows. Los contenedores de Linux en Windows resultan útiles en escenarios de desarrollo y pruebas, especialmente si se desarrolla en un equipo Windows para implementar en dispositivos Linux.

## <a name="prerequisites"></a>Requisitos previos

Para obtener la información más reciente sobre qué sistemas operativos se admiten actualmente en los escenarios de producción, consulte [Sistemas compatibles con Azure IoT Edge](support.md#operating-systems).

# <a name="linux"></a>[Linux](#tab/linux)

Debe tener un dispositivo Linux X64, ARM32 o ARM64. Microsoft ofrece paquetes de instalación para los sistemas operativos Ubuntu Server 16.04, Ubuntu Server 18.04 y Raspbian Stretch.

>[!NOTE]
>La compatibilidad con dispositivos ARM64 está en [versión preliminar pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Prepare el dispositivo para acceder a los paquetes de instalación de Microsoft.

1. Instale la configuración del repositorio que coincida con el sistema operativo del dispositivo.

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

2. Copie la lista generada en el directorio sources.list.d.

   ```bash
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

3. Instale la clave pública de GPG de Microsoft.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

# <a name="windows"></a>[Windows](#tab/windows)

### <a name="windows-version"></a>Versión de Windows

IoT Edge para contenedores de Windows requiere la versión 1809/compilación 17762 de Windows, que es la [compilación de compatibilidad con Windows a largo plazo más reciente](/windows/release-information/). En escenarios de desarrollo y pruebas, cualquier SKU (Pro, Enterprise, Server, etc.) que admita la característica de contenedores funcionará. Sin embargo, asegúrese de revisar la lista de sistemas compatibles antes de pasar a producción.

IoT Edge con contenedores de Linux puede ejecutarse en cualquier versión de Windows que cumpla los [requisitos de Docker Desktop](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install).

### <a name="container-engine-requirements"></a>Requisitos del motor del contenedor

Azure IoT Edge se basa en un motor del contenedor [compatible con OCI](https://www.opencontainers.org/). Asegúrese de que el dispositivo admita los contenedores.

Si va a instalar IoT Edge en una máquina virtual, habilite la virtualización anidada y asigne al menos 2 GB de memoria. En el caso de Hyper-V, las máquinas virtuales de generación 2 tienen la virtualización anidada habilitada de manera predeterminada. En VMware, hay un botón de alternancia para habilitar la característica en la máquina virtual.

---

Los paquetes de software de Azure IoT Edge están sujetos a los términos de licencia que se encuentran cada paquete (`usr/share/doc/{package-name}` o el directorio `LICENSE`). Lea los términos de licencia antes de usar el paquete. La instalación y el uso de un paquete constituyen la aceptación de estos términos. Si no acepta los términos de licencia, no utilice el paquete.

## <a name="install-a-container-engine"></a>Instalación de un motor del contenedor

Azure IoT Edge utiliza un runtime de contenedor compatible con OCI. En los escenarios de producción, se recomienda utilizar el motor basado en Moby. El motor Moby es el único motor de contenedor compatible oficialmente con Azure IoT Edge. Las imágenes de contenedor de Docker CE/EE son totalmente compatibles con el entorno de ejecución de Moby.

# <a name="linux"></a>[Linux](#tab/linux)

Actualice las listas de paquetes en el dispositivo.

   ```bash
   sudo apt-get update
   ```

Instale el motor de Moby.

   ```bash
   sudo apt-get install moby-engine
   ```

Si recibe algún error al instalar el motor del contenedor Moby, compruebe la compatibilidad con Moby del kernel de Linux. Algunos fabricantes de dispositivos incrustados distribuyen imágenes de dispositivos que contienen kernels de Linux personalizados sin las características necesarias para la compatibilidad del motor del contenedor. Ejecute el siguiente comando, que usa el [script check-config](https://github.com/moby/moby/blob/master/contrib/check-config.sh) suministrado por Moby, para comprobar la configuración del kernel:

   ```bash
   curl -sSL https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh -o check-config.sh
   chmod +x check-config.sh
   ./check-config.sh
   ```

En la salida del script, compruebe que todos los elementos que figuran en `Generally Necessary` y `Network Drivers` estén habilitados. Si faltan características, puede habilitarlas volviendo a generar el kernel a partir del origen y seleccionando los módulos asociados para incluirlos en el archivo .config del kernel adecuado. Igualmente, si usa un generador de configuración de kernel como `defconfig` o `menuconfig`, busque y habilite las características correspondientes y vuelva a generar el kernel como corresponda. Una vez que haya implementado el kernel recién modificado, vuelva a ejecutar el script check-config para comprobar que se han habilitado correctamente todas las características necesarias.

# <a name="windows"></a>[Windows](#tab/windows)

En escenarios de producción, use el motor basado en Moby que se incluye en el script de instalación. No hay pasos adicionales para instalar el motor.

En el caso de IoT Edge con contenedores de Linux, debe proporcionar su propio entorno de ejecución del contenedor. Instale [Docker Desktop](https://docs.docker.com/docker-for-windows/install/) en el dispositivo y configúrelo para [usar contenedores de Linux](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers) antes de continuar.

---

## <a name="install-the-iot-edge-security-daemon"></a>Instalación del demonio de seguridad de IoT Edge

El demonio de seguridad de IoT Edge proporciona y mantiene los estándares de seguridad en el dispositivo IoT Edge. El demonio se inicia en cada arranque e inicia el resto del entorno de ejecución de IoT Edge para arrancar el dispositivo.

Los pasos de esta sección representan el proceso habitual para instalar la versión más reciente en un dispositivo que tenga conexión a Internet. Si tiene que instalar una versión específica, como una versión preliminar, o tiene que realizar la instalación mientras está sin conexión, siga los pasos de [Instalación sin conexión o de una versión específica](#offline-or-specific-version-installation), en la sección siguiente.

# <a name="linux"></a>[Linux](#tab/linux)

Actualice las listas de paquetes en el dispositivo.

   ```bash
   sudo apt-get update
   ```

Compruebe qué versiones de IoT Edge están disponibles.

   ```bash
   apt list -a iotedge
   ```

Si quiere instalar la versión más reciente del demonio de seguridad, use el siguiente comando, que también instala la versión más reciente del paquete **libiothsm-std**:

   ```bash
   sudo apt-get install iotedge
   ```

Si desea instalar una versión específica del demonio de seguridad, especifique la versión en la salida de la lista apt. Especifique también la misma versión para el paquete **libiothsm-std**, que de lo contrario instalaría su versión más reciente. Por ejemplo, el siguiente comando instala la versión más reciente de la versión 1.0.8:

   ```bash
   sudo apt-get install iotedge=1.0.8* libiothsm-std=1.0.8*
   ```

Si la versión que quiere instalar no aparece en la lista, siga los pasos de [Instalación sin conexión o de una versión específica](#offline-or-specific-version-installation), en la sección siguiente. En esta sección se muestra cómo seleccionar cualquier versión anterior del demonio de seguridad de IoT Edge o versiones candidatas para lanzamiento.

# <a name="windows"></a>[Windows](#tab/windows)

1. Ejecute PowerShell como administrador.

   Use una sesión de AMD64 de PowerShell, no PowerShell (x86). Si no está seguro del tipo de sesión que usa, ejecute el comando siguiente:

   ```powershell
   (Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   ```

2. Ejecute el comando [Deploy-IoTEdge](reference-windows-scripts.md#deploy-iotedge), que realiza las tareas siguientes:

   * Comprueba que la máquina con Windows se encuentra en una versión compatible.
   * Activa la característica de los contenedores.
   * Descarga el motor de Moby y el entorno de ejecución de Azure IoT Edge.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

   El comando `Deploy-IoTEdge` usa contenedores de Windows de forma predeterminada. Si quiere usar contenedores de Linux, agregue el parámetro `ContainerOs`:

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge -ContainerOs Linux
   ```

3. En este momento, puede que la salida le pida que reinicie. En ese caso, reinícielo ahora.

Al instalar IoT Edge en un dispositivo, puede usar parámetros adicionales para modificar el proceso, entre los que se incluyen los siguientes:

* Dirigir el tráfico para que pase por un servidor proxy
* Haga que el instalador apunte a un directorio local, en el caso de una instalación sin conexión.

Para obtener más información sobre estos parámetros adicionales, vea [Scripts de PowerShell para IoT Edge en Windows](reference-windows-scripts.md).

---

Ahora que el motor del contenedor y el entorno de ejecución de Azure IoT Edge están instalados en el dispositivo, está a punto para el paso siguiente, que consiste en registrar el dispositivo en IoT Hub y configurarlo con su identidad de nube y la información de autenticación.

Elija el siguiente artículo en función del tipo de autenticación que quiera usar:

* La [autenticación de clave simétrica](how-to-manual-provision-symmetric-key.md) es más rápida para comenzar.
* La [autenticación de certificados X.509](how-to-manual-provision-x509.md) es más segura para escenarios de producción.

## <a name="offline-or-specific-version-installation"></a>Instalación sin conexión o de una versión específica

Los pasos de esta sección corresponden a escenarios que no se tratan en los pasos de instalación estándar. Estos pueden incluir lo siguiente:

* Instalación de IoT Edge sin conexión
* Instalación de una versión candidata para lanzamiento
* Instalación de una versión que no sea la más reciente (en Windows)

# <a name="linux"></a>[Linux](#tab/linux)

Siga los pasos de esta sección si quiere instalar una versión específica del entorno de ejecución de Azure IoT Edge que no esté disponible mediante `apt-get install`. La lista de paquetes de Microsoft solo contiene un conjunto limitado de versiones recientes y sus subversiones, por lo que estos pasos son para cualquiera que quiera instalar una versión anterior o una versión candidata para lanzamiento.

Con los comandos curl, puede dirigirse a los archivos de componentes directamente desde el repositorio de GitHub de IoT Edge.

<!-- TODO: Offline installation? -->

1. Vaya a [Versiones de Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases) y busque la versión de lanzamiento que desea tener como destino.

2. Expanda la sección **Recursos** para esa versión.

3. Cada versión debe tener archivos nuevos para el demonio de seguridad de IoT Edge y hsmlib. Use los comandos siguientes para actualizar esos componentes.

   1. Busque el archivo **libiothsm-std** que coincida con la arquitectura del dispositivo IoT Edge. Haga clic con el botón derecho en el vínculo del archivo y copie la dirección del vínculo.

   2. Use el vínculo copiado en el comando siguiente para instalar esa versión de hsmlib:

      ```bash
      curl -L <libiothsm-std link> -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb
      ```

   3. Busque el archivo **iotedge** que coincida con la arquitectura del dispositivo IoT Edge. Haga clic con el botón derecho en el vínculo del archivo y copie la dirección del vínculo.

   4. Use el vínculo copiado en el comando siguiente para instalar esa versión del demonio de seguridad de IoT Edge.

      ```bash
      curl -L <iotedge link> -o iotedge.deb && sudo dpkg -i ./iotedge.deb
      ```

# <a name="windows"></a>[Windows](#tab/windows)

Durante la instalación, se descargan tres archivos:

* Un script de PowerShell, que contiene las instrucciones de instalación.
* Un archivo .cab de Microsoft Azure IoT Edge, que contiene el demonio de seguridad de IoT Edge (iotedged), el motor del contenedor Moby y la CLI de Moby.
* El instalador del paquete redistribuible de Visual C++ (VC Runtime).

Si el dispositivo va a estar sin conexión durante la instalación o si desea instalar una versión específica de IoT Edge, puede descargar anticipadamente estos archivos en el dispositivo. En el momento de la instalación, haga que el script de instalación apunte hacia el directorio que contiene los archivos descargados. El instalador comprueba en primer lugar ese directorio y luego descarga solo los componentes que no se encuentren en él. Si los archivos están disponibles sin conexión, la instalación se puede realizar sin conexión a Internet.

1. Para obtener los archivos de instalación de IoT Edge más recientes, junto con las versiones anteriores, vea las [versiones de Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

2. Busque la versión que desea instalar y descargue los siguientes archivos de la sección **Recursos** de las notas de la versión en el dispositivo IoT:

   * IoTEdgeSecurityDaemon.ps1
   * Microsoft-Azure-IoTEdge-amd64.cab de las versiones 1.0.9 o posteriores, o bien Microsoft-Azure-IoTEdge.cab de las versiones 1.0.8 y anteriores.

   Microsoft-Azure-IotEdge-arm32.cab también está disponible a partir de la versión 1.0.9 con fines de prueba exclusivamente. IoT Edge no es compatible actualmente con dispositivos ARM32 de Windows.

   Es importante usar el script de PowerShell de la misma versión que el archivo .cab que se usa, porque la funcionalidad cambia para admitir las características de cada versión.

3. Si el archivo .cab que descargó tiene un sufijo de arquitectura, cambie el nombre del archivo a **Microsoft-Azure-IoTEdge.cab**.

4. Opcionalmente, descargue un instalador para Visual C++ Redistributable. Por ejemplo, el script de PowerShell usa esta versión: [vc_redist.x64.exe](https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe). Guarde el instalador en la misma carpeta del dispositivo IoT que los archivos IoT Edge.

5. Para instalar con componentes sin conexión, [use el operador punto](/powershell/module/microsoft.powershell.core/about/about_scripts#script-scope-and-dot-sourcing) en la copia local del script de PowerShell. 

6. Ejecute el comando [Deploy-IoTEdge](reference-windows-scripts.md#deploy-iotedge) con el parámetro `-OfflineInstallationPath`. Especifique la ruta de acceso absoluta al directorio de archivos. Por ejemplo,

   ```powershell
   . <path>\IoTEdgeSecurityDaemon.ps1
   Deploy-IoTEdge -OfflineInstallationPath <path>
   ```

   El comando de implementación usará cualquier componente que se encuentre en el directorio de archivos local proporcionado. Si falta el archivo .cab o el instalador de Visual C++, intentará descargarlos.

---

Ahora que el motor del contenedor y el entorno de ejecución de Azure IoT Edge están instalados en el dispositivo, está a punto para el paso siguiente, que consiste en [autenticar un dispositivo IoT Edge en IoT Hub](how-to-manual-provision-symmetric-key.md).

## <a name="uninstall-iot-edge"></a>Desinstalación de IoT Edge

Si quiere quitar la instalación de IoT Edge de su dispositivo, use los siguientes comandos.

# <a name="linux"></a>[Linux](#tab/linux)

Quite el entorno de ejecución de Azure IoT Edge.

```bash
sudo apt-get remove --purge iotedge
```

Al quitar el entorno de ejecución de IoT Edge, los contenedores que se hayan creado en él se detendrán, pero seguirán existiendo en el dispositivo. Observe todos los contenedores para ver cuáles permanecen.

```bash
sudo docker ps -a
```

Elimine los contenedores del dispositivo, incluidos los dos contenedores del entorno en tiempo de ejecución.

```bash
sudo docker rm -f <container name>
```

Por último, quite el entorno en tiempo de ejecución del contenedor del dispositivo.

```bash
sudo apt-get remove --purge moby-cli
sudo apt-get remove --purge moby-engine
```

# <a name="windows"></a>[Windows](#tab/windows)

Si quiere quitar la instalación de IoT Edge del dispositivo Windows, use el comando [Uninstall-IoTEdge](reference-windows-scripts.md#uninstall-iotedge) en una ventana administrativa de PowerShell. Este comando quita el entorno de ejecución de IoT Edge, junto con la configuración existente y los datos de motor de Moby.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Uninstall-IoTEdge
```

Para más información acerca de las opciones de desinstalación, use el comando `Get-Help Uninstall-IoTEdge -full`.

---

## <a name="next-steps"></a>Pasos siguientes

Después de instalar el entorno de ejecución de IoT Edge, configure el dispositivo para que se conecte con IoT Hub. En los siguientes artículos se describe cómo registrar un nuevo dispositivo en la nube y, luego, aprovisionarlo con su identidad y la información de autenticación.

Elija el siguiente artículo en función del tipo de autenticación que quiera usar:

* **Clave simétrica**: tanto IoT Hub como el dispositivo IoT Edge tienen una copia de una clave segura. Cuando el dispositivo se conecta a IoT Hub, comprueba que las claves coincidan. Este método de autenticación es más rápido para comenzar, pero no es tan seguro.

  [Configuración de un dispositivo Azure IoT Edge con autenticación de claves simétricas](how-to-manual-provision-symmetric-key.md)

* **X.509 autofirmado**: el dispositivo IoT Edge tiene certificados de identidad X.509 y se proporciona la huella digital de los certificados a IoT Hub. Cuando el dispositivo se conecta a IoT Hub, estos comparan el certificado con su huella digital. Este método de autenticación es más seguro y se recomienda para los escenarios de producción.

  [Configuración de un dispositivo Azure IoT Edge con autenticación de certificados X.509](how-to-manual-provision-x509.md)