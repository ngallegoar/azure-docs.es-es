---
title: 'Aprovisionamiento de un dispositivo mediante la atestación de clave simétrica: Azure IoT Edge'
description: Uso de atestación de clave simétrica para probar el aprovisionamiento automático de dispositivos para Azure IoT Edge con Device Provisioning Service
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: mrohera
ms.date: 4/3/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 4c44ad91b4fb8581a67ea67e09faca4a9d96df91
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91447771"
---
# <a name="create-and-provision-an-iot-edge-device-using-symmetric-key-attestation"></a>Creación y aprovisionamiento de un dispositivo IoT Edge mediante la atestación de clave simétrica

Los dispositivos Azure IoT Edge pueden aprovisionarse automáticamente con [Device Provisioning Service](../iot-dps/index.yml), igual que los dispositivos que no están habilitados para Edge. Si no está familiarizado con el proceso de aprovisionamiento automático, revise la información general sobre el [aprovisionamiento](../iot-dps/about-iot-dps.md#provisioning-process) antes de continuar.

En este artículo se muestra cómo crear una inscripción individual de Device Provisioning Service mediante la atestación de clave simétrica en un dispositivo IoT Edge con los pasos siguientes:

* Cree una instancia de IoT Hub Device Provisioning Service (DPS).
* Cree una inscripción individual para el dispositivo.
* Instale el entorno de ejecución de IoT Edge y conéctese a IoT Hub.

La atestación de clave simétrica es un enfoque sencillo para autenticar un dispositivo con una instancia del servicio Device Provisioning. Este método de atestación representa una experiencia de "Hola mundo" para los desarrolladores que no estén familiarizados con el aprovisionamiento de dispositivos, o no tengan estrictos requisitos de seguridad. La atestación de dispositivo mediante un [TPM](../iot-dps/concepts-tpm-attestation.md) o [certificado X.509](../iot-dps/concepts-security.md#x509-certificates) es más segura y se debe usar cuando los requisitos de seguridad son más estrictos.

## <a name="prerequisites"></a>Prerrequisitos

* Una instancia de IoT Hub activa
* Un dispositivo físico o virtual

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>Configuración de IoT Hub Device Provisioning Service

Cree una nueva instancia de IoT Hub Device Provisioning Service en Azure y vincúlela a IoT Hub. Puede seguir las instrucciones de [Configuración de IoT Hub DPS](../iot-dps/quick-setup-auto-provision.md).

Cuando Device Provisioning Service esté en ejecución, copie el valor de **Ámbito de id.** de la página de información general. Use este valor cuando configure el entorno de ejecución de IoT Edge.

## <a name="choose-a-unique-registration-id-for-the-device"></a>Elección de un identificador de registro único para el dispositivo

Se debe definir un identificador de registro único para identificar cada dispositivo. Puede usar la dirección MAC, el número de serie o cualquier otra información única del dispositivo.

En este ejemplo se usa una combinación de una dirección MAC y un número de serie que forman la siguiente cadena de un identificador de registro: `sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6`.

Cree un identificador de registro único para el dispositivo. Solo se pueden usar caracteres alfanuméricos en minúsculas y guiones ("-").

## <a name="create-a-dps-enrollment"></a>Crear una inscripción de DPS

Use el identificador de registro del dispositivo para crear una inscripción individual en DPS.

Al crear una inscripción en DPS, tiene la oportunidad de declarar un **Estado inicial de dispositivo gemelo**. En el dispositivo gemelo, puede establecer etiquetas para agrupar dispositivos por cualquier métrica que necesite en su solución, como la región, el entorno, la ubicación o el tipo de dispositivo. Estas etiquetas se usan para crear [implementaciones automáticas](how-to-deploy-at-scale.md).

> [!TIP]
> Las inscripciones de grupo también son posibles cuando se usa la atestación de clave simétrica e implican las mismas decisiones que las inscripciones individuales.

1. En [Azure Portal](https://portal.azure.com), navegue hasta la instancia de IoT Hub Device Provisioning Service.

1. En **Configuración**, seleccione **Administrar inscripciones**.

1. Seleccione **Add individual enrollment** (Agregar inscripción individual) y, a continuación, complete los pasos siguientes para configurar la inscripción:  

   1. En **Mecanismo**, seleccione **Clave simétrica**.

   1. Active la casilla **Generar claves automáticamente**.

   1. Proporcione el **Id. de registro** que ha creado para el dispositivo.

   1. Si lo desea, proporcione un **Id. de dispositivo IoT Hub** para el dispositivo. Puede usar identificadores de dispositivo para dirigirse a un dispositivo individual para la implementación del módulo. Si no proporciona un id. de dispositivo, se usará el id. de registro.

   1. Seleccione **Verdadero** para declarar que la inscripción es para un dispositivo IoT Edge. En el caso de una inscripción de grupo, todos los dispositivos deben ser dispositivos IoT Edge, o bien ninguno puede serlo.

   > [!TIP]
   > En la CLI de Azure, puede crear una [inscripción](/cli/azure/ext/azure-iot/iot/dps/enrollment) o un [grupo de inscripción](/cli/azure/ext/azure-iot/iot/dps/enrollment-group) y usar la marca **habilitado para Edge** para especificar que un dispositivo o un grupo de dispositivos son un dispositivo IoT Edge.

   1. Acepte el valor predeterminado de la directiva de asignación de Device Provisioning Service para **la forma de asignar dispositivos a los centros**, o bien elija otro valor que sea específico de esta inscripción.

   1. Elija la instancia de **IoT Hub** vinculada a la que quiere conectar el dispositivo. Puede elegir varios centros y el dispositivo se asignará a uno de ellos según la directiva de asignación seleccionada.

   1. Elija **cómo quiere que se controlen los datos del dispositivo durante el reaprovisionamiento** cuando los dispositivos soliciten el aprovisionamiento después de la primera vez.

   1. Agregue un valor de etiqueta a **Estado inicial de dispositivo gemelo**, si lo desea. Puede usar etiquetas para los grupos de dispositivos de destino para la implementación del módulo. Por ejemplo:

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

   1. Asegúrese de que **Habilitar entrada** está establecido en **Habilitar**.

   1. Seleccione **Guardar**.

Ahora que existe una inscripción para este dispositivo, el entorno de ejecución de Azure IoT Edge puede aprovisionar automáticamente el dispositivo durante la instalación. Asegúrese de copiar el valor de **Clave principal** de la inscripción para usarlo al instalar el entorno de ejecución de Azure IoT Edge, o bien si va a crear claves de dispositivo para usarse con una inscripción de grupo.

## <a name="derive-a-device-key"></a>Derivación de una clave de dispositivo

> [!NOTE]
> Esta sección solo es necesaria si se usa una inscripción de grupo.

Cada dispositivo usa la clave de dispositivo derivada con el identificador de registro único para realizar la atestación de clave simétrica con la inscripción durante el aprovisionamiento. Para generar la clave del dispositivo, use la clave que ha copiado de la inscripción de DPS para calcular un valor [HMAC-SHA256](https://wikipedia.org/wiki/HMAC) del identificador de registro único del dispositivo y convierta el resultado a formato Base64.

No incluya la clave principal o secundaria de la inscripción en el código del dispositivo.

### <a name="linux-workstations"></a>Estaciones de trabajo de Linux

Si utiliza una estación de trabajo de Linux, puede usar openssl para generar la clave de dispositivo derivada tal y como se muestra en el ejemplo siguiente.

Reemplace el valor de **KEY** por el de la **clave principal** que ha apuntado anteriormente.

Reemplace el valor de **REG_ID** por el identificador de registro del dispositivo.

```bash
KEY=8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw==
REG_ID=sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6

keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
echo -n $REG_ID | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64
```

```bash
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```

### <a name="windows-based-workstations"></a>Estaciones de trabajo basadas en Windows

Si utiliza una estación de trabajo basada en Windows, puede usar PowerShell para generar las claves de dispositivo derivadas tal y como se muestra en el ejemplo siguiente.

Reemplace el valor de **KEY** por el de la **clave principal** que ha apuntado anteriormente.

Reemplace el valor de **REG_ID** por el identificador de registro del dispositivo.

```powershell
$KEY='8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw=='
$REG_ID='sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6'

$hmacsha256 = New-Object System.Security.Cryptography.HMACSHA256
$hmacsha256.key = [Convert]::FromBase64String($KEY)
$sig = $hmacsha256.ComputeHash([Text.Encoding]::ASCII.GetBytes($REG_ID))
$derivedkey = [Convert]::ToBase64String($sig)
echo "`n$derivedkey`n"
```

```powershell
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```

## <a name="install-the-iot-edge-runtime"></a>Instalación del entorno de ejecución de IoT Edge

El runtime de IoT Edge se implementa en todos los dispositivos de IoT Edge. Sus componentes se ejecutan en contenedores y permiten implementar contenedores adicionales en el dispositivo para que pueda ejecutar código en Edge.

Al aprovisionar el dispositivo necesitará la información siguiente:

* El valor **Ámbito de id.** del DPS
* El **Id. de registro** del dispositivo que ha creado
* La **clave principal** que copió de la inscripción de DPS

> [!TIP]
> Para las inscripciones de grupo, necesita la clave [derivada](#derive-a-device-key) de cada dispositivo en lugar de la clave de inscripción de DPS.

### <a name="linux-device"></a>Dispositivo Linux

Siga las instrucciones de la arquitectura del dispositivo. Asegúrese de configurar el entorno de ejecución de IoT Edge para el aprovisionamiento automático, no manual.

[Instalación del entorno de ejecución de Azure IoT Edge en Linux](how-to-install-iot-edge-linux.md)

La sección del archivo de configuración para el aprovisionamiento de clave simétrica tiene este aspecto:

```yaml
# DPS symmetric key provisioning configuration
provisioning:
   source: "dps"
   global_endpoint: "https://global.azure-devices-provisioning.net"
   scope_id: "<SCOPE_ID>"
   attestation:
      method: "symmetric_key"
      registration_id: "<REGISTRATION_ID>"
      symmetric_key: "<SYMMETRIC_KEY>"
```

Reemplace los valores de marcador de posición para `<SCOPE_ID>`, `<REGISTRATION_ID>` y `<SYMMETRIC_KEY>` con los datos que ha recopilado antes. Asegúrese de que la línea **provisioning:** no tiene ningún espacio en blanco delante y de que los elementos anidados muestran una sangría de dos espacios.

### <a name="windows-device"></a>Dispositivo Windows

Instale el entorno de ejecución de Azure IoT Edge en el dispositivo para el que ha generado una clave de dispositivo derivada. Configurará el entorno de ejecución de IoT Edge para el aprovisionamiento automático, no manual.

Para información más detallada sobre cómo instalar IoT Edge en Windows, incluidos los requisitos previos y las instrucciones para tareas como la administración de contenedores y la actualización de IoT Edge, consulte [Instalación del entorno de ejecución de Azure IoT Edge en Windows](how-to-install-iot-edge-windows.md).

1. Abra una ventana de Azure PowerShell en modo de administrador. Asegúrese de usar una sesión de AMD64 de PowerShell para instalar IoT Edge, no PowerShell (x86).

1. El comando **Deploy-IoTEdge** comprueba si la versión del equipo Windows es compatible, activa la característica de contenedores y descarga tanto el runtime de Moby como el de IoT Edge. De forma predeterminada el comando usa contenedores Windows.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

1. En este momento, los dispositivos IoT Core pueden reiniciarse automáticamente. Es posible que otros dispositivos Windows 10 o Windows Server soliciten su reinicio. En ese caso, reinícielo ahora. Una vez que el dispositivo esté listo, vuelva a ejecutar PowerShell como administrador.

1. El comando **Initialize-IoTEdge** configura el entorno de ejecución de Azure IoT Edge en el equipo. El comando tiene como valor predeterminado el aprovisionamiento manual con contenedores de Windows, a menos que use la marca `-Dps` para usar el aprovisionamiento automático.

   Reemplace los valores de marcador de posición para `{scope_id}`, `{registration_id}` y `{symmetric_key}` con los datos que ha recopilado antes.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -Dps -ScopeId {scope ID} -RegistrationId {registration ID} -SymmetricKey {symmetric key}
   ```

## <a name="verify-successful-installation"></a>Comprobación de instalación correcta

Si el entorno de ejecución se inició correctamente, puede ir a IoT Hub y empezar a implementar módulos de IoT Edge en el dispositivo. Use los siguientes comandos en el dispositivo para comprobar que el entorno de ejecución está instalado e iniciado correctamente.

### <a name="linux-device"></a>Dispositivo Linux

Compruebe el estado del servicio IoT Edge.

```cmd/sh
systemctl status iotedge
```

Examine los registros del servicio.

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

Enumere los módulos en ejecución.

```cmd/sh
iotedge list
```

### <a name="windows-device"></a>Dispositivo Windows

Compruebe el estado del servicio IoT Edge.

```powershell
Get-Service iotedge
```

Examine los registros del servicio.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Enumere los módulos en ejecución.

```powershell
iotedge list
```

Puede comprobar que la inscripción individual que ha creado se ha utilizado en el servicio Device Provisioning. En Azure Portal, vaya a la instancia del servicio Device Provisioning. Abra los detalles de la inscripción para la inscripción individual que ha creado. Tenga en cuenta que el estado de la inscripción está **asignado** y se muestra el id. de dispositivo.

## <a name="next-steps"></a>Pasos siguientes

El proceso de inscripción en Device Provisioning Service permite establecer el id. de dispositivo y las etiquetas del dispositivo gemelo al mismo tiempo que aprovisiona el nuevo dispositivo. Puede usar esos valores para dirigirse a dispositivos individuales o grupos de dispositivos con la administración automática de dispositivos. Aprenda a [implementar y supervisar los módulos de IoT Edge a escala mediante Azure Portal](how-to-deploy-at-scale.md) o la [CLI de Azure](how-to-deploy-cli-at-scale.md).
