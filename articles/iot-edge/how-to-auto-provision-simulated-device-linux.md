---
title: 'Aprovisionamiento de dispositivos con un TPM virtual en una máquina virtual Linux: Azure IoT Edge'
description: Usar un TPM simulado en una máquina virtual con Linux para probar Device Provisioning Service en Azure para Azure IoT Edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/01/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 6bb1282212ccff45f179b8750e3ed8aec27d129e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76511066"
---
# <a name="create-and-provision-an-iot-edge-device-with-a-virtual-tpm-on-a-linux-virtual-machine"></a>Cree y aprovisione un dispositivo IoT Edge con un TPM virtual en una máquina virtual de Linux.

Los dispositivos Azure IoT Edge pueden aprovisionarse automáticamente con el [Servicio de aprovisionamiento de dispositivos](../iot-dps/index.yml). Si no está familiarizado con el proceso de aprovisionamiento automático, revise los [conceptos sobre el aprovisionamiento automático](../iot-dps/concepts-auto-provisioning.md) antes de continuar.

En este artículo se muestra cómo probar el aprovisionamiento automático en un dispositivo IoT Edge simulado con los pasos siguientes:

* Cree una máquina virtual (VM) con Linux en Hyper-V con un Módulo de plataforma segura (TPM) simulado para la seguridad de hardware.
* Cree una instancia de IoT Hub Device Provisioning Service (DPS).
* Cree una inscripción individual para el dispositivo.
* Instale el entorno de ejecución de IoT Edge y conecte el dispositivo a IoT Hub.

> [!TIP]
> En este artículo se describe cómo probar el aprovisionamiento de DPS con un simulador de TPM, pero en gran medida se aplica tanto al hardware físico de TPM como al [TPM Infineon OPTIGA&trade;](https://catalog.azureiotsolutions.com/details?title=OPTIGA-TPM-SLB-9670-Iridium-Board), un dispositivo Azure Certified for IoT.
>
> Si usa un dispositivo físico, puede ir directamente a la sección [Recuperación de la información de aprovisionamiento de un dispositivo físico](#retrieve-provisioning-information-from-a-physical-device) de este artículo.

## <a name="prerequisites"></a>Prerrequisitos

* Un equipo de desarrollo de Windows [habilitado para Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v). En este artículo se usa Windows 10 que ejecuta una máquina virtual de servidor de Ubuntu.
* Una instancia de IoT Hub activa.
* Si usa un TPM simulado, [Visual Studio](https://visualstudio.microsoft.com/vs/) 2015 o una versión posterior con la carga de trabajo ["Desarrollo para el escritorio con C++"](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) habilitada.

> [!NOTE]
> Se necesita la versión TPM 2.0 al usar la atestación de TPM con DPS y solo se puede usar para crear inscripciones individuales, no de grupo.

## <a name="create-a-linux-virtual-machine-with-a-virtual-tpm"></a>Crear una máquina virtual con Linux con TPM virtual

En esta sección, se va a crear una nueva máquina virtual con Linux en Hyper-V. Se ha configurado esta máquina virtual con un TPM simulado con el fin de que se pueda usar para probar cómo funciona el aprovisionamiento automático con IoT Edge.

### <a name="create-a-virtual-switch"></a>Crear un conmutador virtual

Un conmutador virtual permite que la máquina virtual se conecte a una red física.

1. Abra el administrador de Hyper-V en la máquina Windows.

2. En el menú **Acciones**, seleccione **Administrador de conmutadores virtuales**.

3. Elija un conmutador virtual **Externo** y, después, seleccione **Crear conmutador virtual**.

4. Asigne un nombre al nuevo conmutador virtual, por ejemplo **EdgeSwitch**. Asegúrese de que el tipo de conexión esté establecido en **Red externa** y, a continuación, seleccione **Aceptar**.

5. Un mensaje emergente advierte de que es posible que se interrumpa la conectividad de red. Seleccione **Yes** (Sí) para continuar.

Si ve errores al crear el nuevo conmutador virtual, asegúrese de que ningún otro conmutador utilice el adaptador Ethernet y que ningún otro conmutador use el mismo nombre.

### <a name="create-virtual-machine"></a>Crear máquina virtual

1. Descargue un archivo de imagen de disco para usarlo para la máquina virtual y guárdelo localmente. Por ejemplo, [Servidor de Ubuntu](https://www.ubuntu.com/download/server).

2. De nuevo en el administrador de Hyper-V, en el menú **Acciones**, seleccione **Nuevo** > **Máquina virtual**.

3. Complete el **Asistente para crear nueva máquina virtual** con las siguientes configuraciones específicas:

   1. **Especificación de la generación**: seleccione **Generación 2**. Las máquinas virtuales de generación 2 tienen la virtualización anidada habilitada, lo cual es necesario para ejecutar IoT Edge en una máquina virtual.
   2. **Configurar funciones de red**: establezca el valor de **Conexión** en el conmutador virtual que creó en la sección anterior.
   3. **Opciones de instalación**: seleccione **Instalar un sistema operativo desde un archivo de imagen de arranque** y busque el archivo de imagen de disco que ha guardado localmente.

4. Seleccione **Finalizar** en el asistente para crear la máquina virtual.

La creación de la máquina virtual puede tardar unos minutos.

### <a name="enable-virtual-tpm"></a>Habilitar TPM virtual

Una vez creada la máquina virtual, abra la configuración para habilitar el módulo de plataforma segura (TPM) virtual, que permite aprovisionar de forma automática el dispositivo.

1. Seleccione la máquina virtual y luego abra la **Configuración**.

2. Navegue hasta **Seguridad**.

3. Anule la selección de la opción **Habilitar arranque seguro**.

4. Seleccione **Habilitar módulo de plataforma segura**.

5. Haga clic en **OK**.  

### <a name="start-the-virtual-machine-and-collect-tpm-data"></a>Iniciar la máquina virtual y recopilar datos de TPM

En la máquina virtual, compile una herramienta que pueda utilizar para recuperar el **Id. de registro** y la **clave de aprobación** del dispositivo.

1. Inicie la máquina virtual y conéctese.

1. Siga las indicaciones en la máquina virtual para finalizar el proceso de instalación y reinicie la máquina.

1. Inicie sesión en la máquina virtual y luego siga los pasos de [Configurar un entorno de desarrollo con Linux](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux) para instalar y compilar el SDK de dispositivo IoT de Azure para C.

   >[!TIP]
   >Durante este artículo, copiará en la máquina virtual y pegará desde esta, cosa que no es sencilla, a través de la aplicación de conexión de administrador de Hyper-V. Es posible que quiera conectarse a la máquina virtual mediante el administrador de Hyper-V una vez para recuperar su dirección IP: `ifconfig`. Después, puede usar la dirección IP para conectarse a través de SSH: `ssh <username>@<ipaddress>`.

1. Ejecute los comandos siguientes para compilar una herramienta de SDK que recupere la información de aprovisionamiento del simulador de TPM.

   ```bash
   cd azure-iot-sdk-c/cmake
   cmake -Duse_prov_client:BOOL=ON -Duse_tpm_simulator:BOOL=ON ..
   cd provisioning_client/tools/tpm_device_provision
   make
   sudo ./tpm_device_provision
   ```

1. Desde una ventana de comandos, vaya al directorio `azure-iot-sdk-c` y ejecute el simulador de TPM. Atiende en un socket en los puertos 2321 y 2322. No cierre esta ventana de comandos; debe mantener este simulador en ejecución.

   En el directorio `azure-iot-sdk-c`, ejecute el siguiente comando para iniciar el simulador:

   ```bash
   ./provisioning_client/deps/utpm/tools/tpm_simulator/Simulator.exe
   ```

1. Con Visual Studio, abra la solución generada en el directorio `cmake` llamada `azure_iot_sdks.sln` y compílela mediante el comando **Compilar solución** del menú **Compilar**.

1. En el panel **Explorador de soluciones** de Visual Studio, vaya a la carpeta **Provision\_Tools**. Haga clic con el botón derecho en el proyecto **tpm_device_provision** y seleccione **Establecer como proyecto de inicio**.

1. Ejecute la solución con cualquiera de los comandos **Inicio** del menú **Depurar**. La ventana de salida muestra el **Id. de registro** del simulador de TPM y la **Clave de aprobación**, que debe copiar para usarlos más adelante al crear una inscripción individual para el dispositivo. Puede cerrar esta ventana (con el identificador de registro y la clave de aprobación), pero deje la ventana del simulador de TPM en ejecución.

## <a name="retrieve-provisioning-information-from-a-physical-device"></a>Recuperación de la información de aprovisionamiento de un dispositivo físico

En el dispositivo, compile una herramienta que pueda usar para recuperar la información de aprovisionamiento del dispositivo.

1. Siga los pasos de [Configuración de un entorno de desarrollo en Linux](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux) para instalar y compilar el SDK de dispositivo IoT de Azure para C.

1. Ejecute los comandos siguientes para compilar una herramienta de SDK que recupere la información de aprovisionamiento del dispositivo TPM.

   ```bash
   cd azure-iot-sdk-c/cmake
   cmake -Duse_prov_client:BOOL=ON ..
   cd provisioning_client/tools/tpm_device_provision
   make
   sudo ./tpm_device_provision
   ```

1. Copie los valores de **Id. de registro** y **Clave de aprobación**. Use estos valores para crear una inscripción individual para el dispositivo en DPS.

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>Configuración de IoT Hub Device Provisioning Service

Cree una nueva instancia de IoT Hub Device Provisioning Service en Azure y vincúlela a IoT Hub. Puede seguir las instrucciones de [Configuración de IoT Hub DPS](../iot-dps/quick-setup-auto-provision.md).

Cuando Device Provisioning Service esté en ejecución, copie el valor de **Ámbito de id.** de la página de información general. Use este valor cuando configure el entorno de ejecución de IoT Edge.

## <a name="create-a-dps-enrollment"></a>Crear una inscripción de DPS

Recupere la información de aprovisionamiento de la máquina virtual y úsela para crear una inscripción individual en Device Provisioning Service.

Al crear una inscripción en DPS, tiene la oportunidad de declarar un **Estado inicial de dispositivo gemelo**. En el dispositivo gemelo, puede establecer etiquetas para agrupar dispositivos por cualquier métrica que necesite en su solución, como la región, el entorno, la ubicación o el tipo de dispositivo. Estas etiquetas se usan para crear [implementaciones automáticas](how-to-deploy-monitor.md).

1. En [Azure Portal](https://portal.azure.com), navegue hasta la instancia de IoT Hub Device Provisioning Service.

2. En **Configuración**, seleccione **Administrar inscripciones**.

3. Seleccione **Add individual enrollment** (Agregar inscripción individual) y, a continuación, complete los pasos siguientes para configurar la inscripción:  

   1. En **Mecanismo**, seleccione **TPM**.

   2. Proporcione la **Clave de aprobación** y la **Id. de registro** que se ha copiado de la máquina virtual.

      > [!TIP]
      > Si usa un dispositivo de TPM físico, debe determinar la **Clave de aprobación**, que es única para cada chip de TPM y se obtiene del fabricante del chip de TPM asociado. Puede derivar un único **identificador de registro** para el dispositivo TPM, por ejemplo, al aplicar un algoritmo hash SHA-256 a la clave de aprobación.

   3. Seleccione **Verdadero** para declarar que esta máquina virtual es un dispositivo IoT Edge.

   4. Elija la instancia de **IoT Hub** vinculada a la que quiere conectar el dispositivo. Puede elegir varios centros y el dispositivo se asignará a uno de ellos según la directiva de asignación seleccionada.

   5. Si lo desea, proporcione un identificador para el dispositivo. Puede usar identificadores de dispositivo para dirigirse a un dispositivo individual para la implementación del módulo. Si no proporciona un id. de dispositivo, se usará el id. de registro.

   6. Agregue un valor de etiqueta a **Estado inicial de dispositivo gemelo**, si lo desea. Puede usar etiquetas para los grupos de dispositivos de destino para la implementación del módulo. Por ejemplo:

      ```json
      {
         "tags": {
            "environment": "test"
         },
         "properties": {
            "desired": {}
         }
      }
      ```

   7. Seleccione **Guardar**.

Ahora que existe una inscripción para este dispositivo, el entorno de ejecución de Azure IoT Edge puede aprovisionar automáticamente el dispositivo durante la instalación.

## <a name="install-the-iot-edge-runtime"></a>Instalación del entorno de ejecución de IoT Edge

El runtime de IoT Edge se implementa en todos los dispositivos de IoT Edge. Sus componentes se ejecutan en contenedores y permiten implementar contenedores adicionales en el dispositivo para que pueda ejecutar código en Edge. Instale el entorno de ejecución de IoT Edge en la máquina virtual.

Averigüe el **Ámbito de id.** de DPS y la **Id. de registro** del dispositivo antes de comenzar el artículo que coincide con el tipo de su dispositivo. Si instaló el servidor de Ubuntu de ejemplo, use las instrucciones de **x64**. Asegúrese de configurar el entorno de ejecución de IoT Edge para el aprovisionamiento automático, no manual.

[Instalación del entorno de ejecución de Azure IoT Edge en Linux](how-to-install-iot-edge-linux.md)

## <a name="give-iot-edge-access-to-the-tpm"></a>Conceder acceso a IoT Edge en el TPM

Para que el entorno de ejecución de IoT Edge aprovisione automáticamente el dispositivo, necesita acceso al TPM.

Puede conceder acceso TPM al entorno de ejecución de Azure IoT Edge mediante la invalidación de la configuración de systemd para que el servicio **iotedge** tenga privilegios raíz. Si no desea elevar los privilegios de servicio, también puede usar los pasos siguientes para proporcionar manualmente el acceso TPM.

1. Busque la ruta de acceso para el módulo de hardware TPM en el dispositivo y guárdela como variable local.

   ```bash
   tpm=$(sudo find /sys -name dev -print | fgrep tpm | sed 's/.\{4\}$//')
   ```

2. Cree una nueva regla que conceda al entorno de ejecución de IoT Edge acceso a tpm0.

   ```bash
   sudo touch /etc/udev/rules.d/tpmaccess.rules
   ```

3. Abra el archivo de reglas.

   ```bash
   sudo nano /etc/udev/rules.d/tpmaccess.rules
   ```

4. Copie la siguiente información de acceso en el archivo de reglas.

   ```input
   # allow iotedge access to tpm0
   KERNEL=="tpm0", SUBSYSTEM=="tpm", GROUP="iotedge", MODE="0660"
   ```

5. Guarde y cierre el archivo.

6. Desencadene el sistema udev para evaluar la nueva regla.

   ```bash
   /bin/udevadm trigger $tpm
   ```

7. Compruebe que la regla se haya aplicado correctamente.

   ```bash
   ls -l /dev/tpm0
   ```

   La salida correcta tendrá un aspecto similar al siguiente:

   ```output
   crw-rw---- 1 root iotedge 10, 224 Jul 20 16:27 /dev/tpm0
   ```

   Si no ve que se hayan aplicado los permisos correctos, intente reiniciar la máquina para actualizar udev.

## <a name="restart-the-iot-edge-runtime"></a>Reinicio del entorno de ejecución de IoT Edge

Reinicie el entorno de ejecución de IoT Edge para que aplique todos los cambios de configuración realizados en el dispositivo.

   ```bash
   sudo systemctl restart iotedge
   ```

Compruebe que el entorno de ejecución de IoT Edge esté en ejecución.

   ```bash
   sudo systemctl status iotedge
   ```

Si ve errores de aprovisionamiento, es posible que los cambios de configuración no hayan surtido efecto todavía. Pruebe a reiniciar de nuevo el demonio de IoT Edge.

   ```bash
   sudo systemctl daemon-reload
   ```

O bien, pruebe a reiniciar la máquina virtual para ver si los cambios surten efecto con un inicio nuevo.

## <a name="verify-successful-installation"></a>Comprobación de instalación correcta

Si el entorno de ejecución se inició correctamente, puede entrar en su instancia de IoT Hub y ver que el nuevo dispositivo se aprovisionó automáticamente. Ahora el dispositivo está listo para ejecutar módulos de IoT Edge.

Compruebe el estado de IoT Edge Daemon.

```cmd/sh
systemctl status iotedge
```

Examine los registros del daemon.

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

Enumere los módulos en ejecución.

```cmd/sh
iotedge list
```

Puede comprobar que la inscripción individual que ha creado se ha utilizado en el servicio Device Provisioning. En Azure Portal, vaya a la instancia del servicio Device Provisioning. Abra los detalles de la inscripción para la inscripción individual que ha creado. Tenga en cuenta que el estado de la inscripción está **asignado** y se muestra el id. de dispositivo.

## <a name="next-steps"></a>Pasos siguientes

El proceso de inscripción en Device Provisioning Service permite establecer el id. de dispositivo y las etiquetas del dispositivo gemelo al mismo tiempo que aprovisiona el nuevo dispositivo. Puede usar esos valores para dirigirse a dispositivos individuales o grupos de dispositivos con la administración automática de dispositivos. Aprenda a [implementar y supervisar los módulos de IoT Edge a escala mediante Azure Portal](how-to-deploy-monitor.md) o la [CLI de Azure](how-to-deploy-monitor-cli.md).
