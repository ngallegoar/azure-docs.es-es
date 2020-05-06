---
title: Aprovisionamiento de un dispositivo X.509 simulado en Azure IoT Hub mediante C
description: En esta guía de inicio rápido se utilizan inscripciones individuales. En esta guía de inicio rápido, creará y aprovisionará un dispositivo X.509 simulado mediante el SDK de dispositivos de C para Azure IoT Hub Device Provisioning Service (DPS).
author: wesmc7777
ms.author: wesmc
ms.date: 04/26/2020
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.custom: mvc
ms.openlocfilehash: 882d62c088fa51153ca496231beccd4bfd5af0cb
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82190371"
---
# <a name="quickstart-provision-an-x509-simulated-device-using-the-azure-iot-c-sdk"></a>Inicio rápido: Aprovisionamiento de un dispositivo X.509 simulado mediante el SDK para C de Azure IoT

[!INCLUDE [iot-dps-selector-quick-create-simulated-device-x509](../../includes/iot-dps-selector-quick-create-simulated-device-x509.md)]

En este inicio rápido, aprenderá a crear y ejecutar un simulador de dispositivos X.509 en un equipo de desarrollo Windows. Configurará este dispositivo simulado para asignarlo a un centro de IoT mediante una inscripción en una instancia del servicio Device Provisioning. Se usará código de ejemplo del [SDK para C de Azure IoT](https://github.com/Azure/azure-iot-sdk-c) para simular una secuencia de arranque para el dispositivo. El dispositivo se reconocerá en función de la inscripción en el servicio de aprovisionamiento y se asignará al centro de IoT.

Si no está familiarizado con el proceso de aprovisionamiento automático, revise los [Conceptos sobre aprovisionamiento automático](concepts-auto-provisioning.md). Además, asegúrese de completar los pasos descritos en [Configuración del servicio Azure IoT Hub Device Provisioning con Azure Portal](quick-setup-auto-provision.md) antes de continuar con este inicio rápido. 

Azure IoT Hub Device Provisioning Service admite dos tipos de inscripciones:

* [Grupos de inscripción](concepts-service.md#enrollment-group): usados para inscribir varios dispositivos relacionados.
* [Inscripciones individuales](concepts-service.md#individual-enrollment): usadas para inscribir un solo dispositivo.

En este artículo se mostrarán las inscripciones individuales.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Prerrequisitos

Los siguientes requisitos previos corresponden a un entorno de desarrollo de Windows. En el caso de Linux o macOS, consulte la sección correspondiente en [Preparación del entorno de desarrollo](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) en la documentación del SDK.

* [Visual Studio](https://visualstudio.microsoft.com/vs/) 2019 con la carga de trabajo ["Desarrollo para el escritorio con C++"](https://docs.microsoft.com/cpp/?view=vs-2019#pivot=workloads) habilitada. También se admiten Visual Studio 2015 y Visual Studio 2017.

* Tener instalada la versión más reciente de [Git](https://git-scm.com/download/).

## <a name="prepare-a-development-environment-for-the-azure-iot-c-sdk"></a>Preparación de un entorno de desarrollo para el SDK para C de Azure IoT

En esta sección, preparará un entorno de desarrollo para compilar el [SDK para C de Azure IoT](https://github.com/Azure/azure-iot-sdk-c), lo que incluye el código de ejemplo de la secuencia de arranque de X.509.

1. Descargue el [sistema de compilación CMake](https://cmake.org/download/).

    **Antes** de comenzar la instalación de `CMake`, es importante que los requisitos previos de Visual Studio (Visual Studio y la carga de trabajo de desarrollo de escritorio con C++) estén instalados en la máquina. Una vez que los requisitos previos están en su lugar, y se ha comprobado la descarga, instale el sistema de compilación de CMake.

2. Busque el nombre de etiqueta de la [versión más reciente](https://github.com/Azure/azure-iot-sdk-c/releases/latest) del SDK.

3. Abra un símbolo del sistema o el shell de Bash de Git. Ejecute los siguientes comandos para clonar la versión más reciente del repositorio de GitHub del [SDK de Azure IoT para C](https://github.com/Azure/azure-iot-sdk-c). Use la etiqueta que encontró en el paso anterior como valor del parámetro `-b`:

    ```cmd/sh
    git clone -b <release-tag> https://github.com/Azure/azure-iot-sdk-c.git
    cd azure-iot-sdk-c
    git submodule update --init
    ```

    Esta operación puede tardar varios minutos en completarse.

4. Cree un subdirectorio `cmake` en el directorio raíz del repositorio de Git y vaya a esa carpeta. Ejecute los siguientes comandos desde el directorio `azure-iot-sdk-c`:

    ```cmd/sh
    mkdir cmake
    cd cmake
    ```

5. El código de ejemplo usa un certificado X.509 para proporcionar atestación mediante la autenticación de X.509. Ejecute el siguiente comando para compilar una versión del SDK específica para su plataforma de desarrollo que incluya el cliente de aprovisionamiento de dispositivos. Se genera una solución de Visual Studio para el dispositivo simulado en el directorio `cmake`.

    ```cmd
    cmake -Duse_prov_client:BOOL=ON ..
    ```

    Si `cmake` no encuentra el compilador de C++, es posible que obtenga errores de compilación durante la ejecución del comando anterior. Si sucede, intente ejecutar este comando en el [símbolo del sistema de Visual Studio](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs).

    Una vez realizada la compilación, las últimas líneas de salida son similares a las siguientes:

    ```cmd/sh
    $ cmake -Duse_prov_client:BOOL=ON ..
    -- Building for: Visual Studio 16 2019
    -- The C compiler identification is MSVC 19.23.28107.0
    -- The CXX compiler identification is MSVC 19.23.28107.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: C:/code/azure-iot-sdk-c/cmake
    ```

## <a name="create-a-self-signed-x509-device-certificate"></a>Creación de un certificado de dispositivo X.509 autofirmado

En esta sección, usará un certificado X.509 autofirmado, así que es importante tener en cuenta los siguientes puntos:

* Los certificados autofirmados son solo para la realización de pruebas, no se deben usar en producción.
* La fecha de expiración predeterminada de un certificado autofirmado es de un año.

Usará código de ejemplo del SDK para C de Azure IoT para crear el certificado que se usará con la entrada de inscripción individual en el dispositivo simulado.

1. Inicie Visual Studio y abra el nuevo archivo de solución denominado `azure_iot_sdks.sln`. Este archivo de solución se encuentra en la carpeta `cmake` que creó anteriormente en la raíz del repositorio de Git azure-iot-sdk-c.

2. En el menú de Visual Studio, seleccione **Compilar** > **Compilar solución** para compilar todos los proyectos de la solución.

3. En el *Explorador de soluciones* de Visual Studio, vaya a la carpeta **Aprovisionar\_Herramientas**. Haga clic con el botón derecho en el proyecto **dice\_device\_enrollment** y seleccione **Establecer como proyecto de inicio**.

4. En el menú de Visual Studio, seleccione **Depurar** > **Iniciar sin depurar** para ejecutar la solución. En la ventana de salida, escriba **i** para la inscripción individual cuando se le solicite.

    La ventana de salida muestra un certificado X.509 autofirmado generado localmente para el dispositivo simulado. Copie la salida en el portapapeles desde **---BEGIN CERTIFICATE---** hasta **-----END CERTIFICATE-----** y asegúrese de incluir ambas líneas. Solo necesita el primer certificado de la ventana de salida.

5. Con un editor de texto, guarde el certificado en un nuevo archivo denominado **_X509testcert.pem_** .

## <a name="create-a-device-enrollment-entry-in-the-portal"></a>Creación de una entrada de inscripción de dispositivo en el portal

1. Inicie sesión en Azure Portal, seleccione el botón **Todos los recursos** en el menú de la izquierda y abra Device Provisioning Service.

2. Seleccione la pestaña **Administrar inscripciones** y, después, seleccione el botón **Agregar inscripción individual** en la parte superior.

3. En el panel **Agregar inscripción**, escriba la siguiente información y presione el botón **Guardar**.

    * **Mecanismo:** Seleccione **X.509** como *Mecanismo* de atestación de identidad.
    * **Archivo .pem o .cer del certificado principal:** Elija **Seleccionar un archivo** para seleccionar el archivo de certificado, X509testcert.pem, que creó anteriormente.
    * **Id. de dispositivo IoT Hub:** escriba **test-docs-cert-device** para dar al dispositivo un identificador.

      [![Agregar inscripción individual para la atestación X.509 en el portal](./media/quick-create-simulated-device-x509/device-enrollment.png)](./media/quick-create-simulated-device-x509/device-enrollment.png#lightbox)

      Al inscribir el dispositivo correctamente, el dispositivo X.509 aparece como **riot-device-cert** en la columna *Id. de registro* de la pestaña *Individual Enrollments* (Inscripciones individuales). 

## <a name="simulate-first-boot-sequence-for-the-device"></a>Simulación de la primera secuencia de arranque para el dispositivo

En esta sección, actualizará el código de ejemplo para enviar la secuencia de arranque del dispositivo a la instancia del servicio Device Provisioning. Esta secuencia de arranque hará que el dispositivo se reconozca y se asigne a un centro de IoT vinculado a la instancia del servicio Device Provisioning.

1. En Azure Portal, seleccione la pestaña **Información general** de Device Provisioning Service y anote el valor de **_Ámbito de id_** .

    ![Extracción de información del punto de conexión del servicio Device Provisioning desde la hoja del portal](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

2. En la ventana del *Explorador de soluciones* de Visual Studio, desplácese hasta la carpeta **Aprovisionar\_Ejemplos**. Expanda el proyecto de ejemplo denominado **prov\_dev\_client\_sample**. Expanda **Archivos de código fuente** y abra **prov\_dev\_cliente\_sample.c**.

3. Busque la constante `id_scope` y reemplace el valor por el valor de **Ámbito de id.** que copió anteriormente. 

    ```c
    static const char* id_scope = "0ne00002193";
    ```

4. Busque la definición de la función `main()` en el mismo archivo. Asegúrese de que la variable `hsm_type` está establecida en `SECURE_DEVICE_TYPE_X509` y no en `SECURE_DEVICE_TYPE_TPM`, tal como se muestra a continuación.

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    //hsm_type = SECURE_DEVICE_TYPE_TPM;
    hsm_type = SECURE_DEVICE_TYPE_X509;
    ```

5. Haga clic con el botón derecho en el proyecto **prov\_dev\_client\_sample** y seleccione **Set as Startup Project** (Establecer como proyecto de inicio).

6. En el menú de Visual Studio, seleccione **Depurar** > **Iniciar sin depurar** para ejecutar la solución. En la solicitud para volver a compilar el proyecto, haga clic en **Sí** para recompilar el proyecto antes de ejecutarlo.

    La salida siguiente es un ejemplo de arranque correcto del cliente de dispositivo de aprovisionamiento de ejemplo y su conexión a la instancia del servicio de aprovisionamiento para obtener información de IoT Hub y registrarse:

    ```cmd
    Provisioning API Version: 1.2.7

    Registering... Press enter key to interrupt.

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service:
    test-docs-hub.azure-devices.net, deviceId: test-docs-cert-device
    ```

7. En el portal, vaya hasta el centro de IoT vinculado a su servicio de aprovisionamiento y seleccione la pestaña **Dispositivos IoT**. Si el dispositivo X.509 simulado se ha aprovisionado correctamente en el centro, su identificador de dispositivo aparecerá en la hoja **Dispositivos IoT** y en *ESTADO* aparecerá el valor **Habilitado**. Es posible que tenga que pulsar el botón **Actualizar** en la parte superior. 

    ![El dispositivo se registra con el centro de IoT](./media/quick-create-simulated-device-x509/hub-registration.png) 

## <a name="clean-up-resources"></a>Limpieza de recursos

Si planea seguir trabajando con el ejemplo de cliente de dispositivo y explorándolo, no limpie los recursos que se crean en este inicio rápido. Si no va a continuar, use el siguiente comando para eliminar todos los recursos que se han creado en este inicio rápido.

1. Cierre la ventana de salida de ejemplo del cliente del dispositivo en su máquina.
1. En el menú de la izquierda de Azure Portal, seleccione **Todos los recursos** y seleccione Device Provisioning Service. Abra **Administrar inscripciones** en servicio y, después, seleccione la pestaña **Inscripciones individuales**. Active la casilla situada junto al campo *ID. DE REGISTRO* del dispositivo que ha inscrito en este inicio rápido y presione el botón **Eliminar** situado en la parte superior del panel. 
1. En el menú de la izquierda de Azure Portal, seleccione **Todos los recursos** y después su centro de IoT. Abra **Dispositivos IoT** en su centro, active la casilla que hay junto al campo *ID DE DISPOSITIVO* del dispositivo que registró en este inicio rápido y, luego, presione el botón **Eliminar** situado en la parte superior del panel.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha creado un dispositivo simulado X.509 en una máquina Windows y lo ha aprovisionado en un centro de IoT mediante IoT Hub Device Provisioning Service en el portal. Para aprender a inscribir un dispositivo X.509 mediante programación, pase al inicio rápido para la inscripción de dispositivos X.509 mediante programación. 

> [!div class="nextstepaction"]
> [Inicio rápido de Azure: Inscripción de dispositivos X.509 en Azure IoT Hub Device Provisioning Service](quick-enroll-device-x509-java.md)
