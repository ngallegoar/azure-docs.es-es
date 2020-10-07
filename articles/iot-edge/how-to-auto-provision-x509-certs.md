---
title: 'Aprovisionamiento automático de dispositivos con DPS mediante certificados X.509: Azure IoT Edge | Microsoft Docs'
description: Uso de certificados X.509 para probar el aprovisionamiento automático de dispositivos para Azure IoT Edge con Device Provisioning Service
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: kevindaw
ms.date: 04/09/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 13c15eeb98b13d0fe9a5b7797ec942209d403cc6
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91447755"
---
# <a name="create-and-provision-an-iot-edge-device-using-x509-certificates"></a>Creación y aprovisionamiento de un dispositivo IoT Edge mediante certificados X.509

Con [Azure IoT Hub Device Provisioning Service (DPS)](../iot-dps/index.yml), puede aprovisionar automáticamente los dispositivos IoT Edge mediante certificados X.509. Si no está familiarizado con el proceso de aprovisionamiento automático, revise la información general sobre el [aprovisionamiento](../iot-dps/about-iot-dps.md#provisioning-process) antes de continuar.

En este artículo se muestra cómo crear una inscripción de Device Provisioning Service mediante certificados X.509 en un dispositivo IoT Edge con los pasos siguientes:

* Generar certificados y claves.
* Crear una inscripción individual para un dispositivo o una inscripción de grupo para un conjunto de dispositivos.
* Instalar el entorno de ejecución de IoT Edge y registrar el dispositivo con IoT Hub.

El uso de certificados X.509 como un mecanismo de atestación es una manera excelente para escalar la producción y simplificar el aprovisionamiento de dispositivos. Normalmente, los certificados X.509 están organizados en una cadena de certificados de confianza. Comenzando por un certificado raíz de confianza o autofirmado, cada certificado de la cadena firma al certificado inmediatamente inferior. Este patrón crea una cadena delegada de confianza desde el certificado raíz a través de todos los certificados intermedios hasta el certificado "hoja" final que está instalado en un dispositivo.

## <a name="prerequisites"></a>Requisitos previos

* Una instancia de IoT Hub activa.
* Un dispositivo físico o virtual para que sea el dispositivo IoT Edge.
* Tener instalada la versión más reciente de [Git](https://git-scm.com/download/).
* Una instancia de IoT Hub Device Provisioning Service en Azure que esté vinculada a IoT Hub.
  * Si no tiene una instancia de Device Provisioning Service, siga las instrucciones de [Configuración de Azure IoT Hub DPS](../iot-dps/quick-setup-auto-provision.md).
  * Cuando Device Provisioning Service esté en ejecución, copie el valor de **Ámbito de id.** de la página de información general. Use este valor cuando configure el entorno de ejecución de IoT Edge.

## <a name="generate-device-identity-certificates"></a>Generación de los certificados de identidad del dispositivo

El certificado de identidad del dispositivo es un certificado de hoja que se conecta a través de una cadena de certificados de confianza con el certificado de la entidad de certificación X.509 (CA) superior. El nombre común del certificado de identidad del dispositivo debe estar establecido en el id. de dispositivo que quiere que el dispositivo tenga en el centro de IoT.

Los certificados de identidad del dispositivo solo se usan para aprovisionar al dispositivo IoT Edge y autenticarlo con Azure IoT Hub. No son certificados de firma, a diferencia de los certificados de CA que el dispositivo IoT Edge presenta a los módulos o los dispositivos hoja para la comprobación. Para obtener más información, consulte los [detalles de uso de los certificados de Azure IoT Edge](iot-edge-certs.md).

Después de crear el certificado de identidad del dispositivo, debe tener dos archivos: un archivo. cer o. pem que contiene la parte pública del certificado y un archivo. cer o. pem con la clave privada del certificado. Si planea usar una inscripción de grupo en DPS, también necesitará la parte pública de un certificado de CA raíz o intermedio en la misma cadena de certificados de confianza.

Necesita los siguientes archivos para configurar el aprovisionamiento automático con X.509:

* El certificado de identidad del dispositivo y su certificado de clave privada. El certificado de identidad de dispositivo se carga en DPS si crea una inscripción individual. La clave privada se pasa al entorno de ejecución de Azure IoT Edge.
* Una cadena de certificados completa, que debe contener al menos la identidad del dispositivo y los certificados intermedios. La cadena de certificados completa se pasa al entorno de ejecución de Azure IoT Edge.
* Un certificado intermedio o raíz de entidad de certificación de la cadena de certificados de confianza. Este certificado se carga en DPS si crea una inscripción de grupo.

> [!NOTE]
> Actualmente, una limitación en libiothsm impide el uso de certificados que expiran el 1 de enero de 2038 o en una fecha posterior.

### <a name="use-test-certificates"></a>Uso de certificados de pruebas

Si no tiene una entidad de certificación disponible para crear nuevos certificados de identidad y quiere probar este escenario, el repositorio de Git de Azure IoT Edge contiene scripts que puede usar para generar certificados de prueba. Estos certificados están diseñados solo para pruebas de desarrollo y no deben usarse en producción.

Para crear certificados de prueba, siga los pasos descritos en [Creación de certificados de demostración para probar las características de dispositivo IoT Edge](how-to-create-test-certificates.md). Complete las dos secciones necesarias para configurar los scripts de generación de certificados y crear un certificado de CA raíz. A continuación, siga los pasos para crear un certificado de identidad del dispositivo. Cuando haya terminado, debe tener la cadena de certificados y el par de claves siguientes:

Linux:

* `<WRKDIR>/certs/iot-edge-device-identity-<name>-full-chain.cert.pem`
* `<WRKDIR>/private/iot-edge-device-identity-<name>.key.pem`

Windows:

* `<WRKDIR>\certs\iot-edge-device-identity-<name>-full-chain.cert.pem`
* `<WRKDIR>\private\iot-edge-device-identity-<name>.key.pem`

Necesita ambos certificados en el dispositivo IoT Edge. Si va a usar la inscripción individual en DPS, deberá cargar el archivo. cert.pem. Si va a usar la inscripción de grupos en DPS, también necesitará un certificado de CA raíz o intermedio en la misma cadena de certificados de confianza que se va a cargar. Si usa certificados de demostración, use el certificado `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem` para la inscripción de grupos.

## <a name="create-a-dps-individual-enrollment"></a>Creación de una inscripción individual de DPS

Use las claves y los certificados generados para crear una inscripción individual en DPS para un único dispositivo IoT Edge. Las inscripciones individuales usan la parte pública del certificado de identidad de un dispositivo y la asocian con el certificado del dispositivo.

Si quiere aprovisionar a varios dispositivos IoT Edge, siga los pasos descritos en la siguiente sección, [Creación de una inscripción de grupo de DPS](#create-a-dps-group-enrollment).

Al crear una inscripción en DPS, tiene la oportunidad de declarar un **Estado inicial de dispositivo gemelo**. En el dispositivo gemelo, puede establecer etiquetas para agrupar dispositivos por cualquier métrica que necesite en su solución, como la región, el entorno, la ubicación o el tipo de dispositivo. Estas etiquetas se usan para crear [implementaciones automáticas](how-to-deploy-at-scale.md).

Para más información sobre las inscripciones en Device Provisioning Service, consulte [Administración de inscripciones de dispositivos](../iot-dps/how-to-manage-enrollments.md).

   > [!TIP]
   > En la CLI de Azure, puede crear una [inscripción](/cli/azure/ext/azure-iot/iot/dps/enrollment) o un [grupo de inscripción](/cli/azure/ext/azure-iot/iot/dps/enrollment-group) y usar la marca **habilitado para Edge** para especificar que un dispositivo o un grupo de dispositivos son un dispositivo IoT Edge.

1. En [Azure Portal](https://portal.azure.com), navegue hasta la instancia de IoT Hub Device Provisioning Service.

1. En **Configuración**, seleccione **Administrar inscripciones**.

1. Seleccione **Add individual enrollment** (Agregar inscripción individual) y, a continuación, complete los pasos siguientes para configurar la inscripción:  

   * **Mecanismo**: Seleccione **X.509**.

   * **Primary Certificate .pem or .cer file** (Archivo .pem o .cer del certificado principal): cargue el archivo público desde el certificado de identidad del dispositivo. Si usó los scripts para generar un certificado de prueba, elija el siguiente archivo:

      `<WRKDIR>/certs/iot-edge-device-identity-<name>.cert.pem`

   * **Id. de dispositivo IoT Hub**: Si lo desea, proporcione un identificador para el dispositivo. Puede usar identificadores de dispositivo para dirigirse a un dispositivo individual para la implementación del módulo. Si no especifica un id. de dispositivo, se usa el nombre común (CN) en el certificado X. 509.

   * **Dispositivo IoT Edge**: Seleccione **Verdadero** para declarar que la inscripción es para un dispositivo IoT Edge.

   * **Seleccione los centros de IoT a los que se puede asignar este dispositivo**: elija la instancia de IoT Hub vinculada a la que quiere conectar el dispositivo. Puede elegir varios centros y el dispositivo se asignará a uno de ellos según la directiva de asignación seleccionada.

   * **Estado inicial de dispositivo gemelo**: agregue un valor de etiqueta para que se agregue al dispositivo gemelo, si lo desea. Puede usar etiquetas para los grupos de dispositivos de destino de la implementación automática. Por ejemplo:

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

1. Seleccione **Guardar**.

Ahora que existe una inscripción para este dispositivo, el entorno de ejecución de Azure IoT Edge puede aprovisionar automáticamente el dispositivo durante la instalación. Continúe con la sección [Instalación del entorno de ejecución de Azure IoT Edge](#install-the-iot-edge-runtime) para configurar el dispositivo IoT Edge.

## <a name="create-a-dps-group-enrollment"></a>Creación de una inscripción de grupo de DPS

Use las claves y los certificados generados para crear una inscripción de grupo en DPS para varios dispositivos IoT Edge. Las inscripciones de grupo usan un certificado de CA raíz o intermedio de la cadena de certificados de confianza que se usó para generar los certificados de identidad del dispositivo individuales.

Si quiere aprovisionar un único dispositivo IoT Edge en su lugar, siga los pasos de la sección anterior, [Creación de una inscripción individual de DPS](#create-a-dps-individual-enrollment).

Al crear una inscripción en DPS, tiene la oportunidad de declarar un **Estado inicial de dispositivo gemelo**. En el dispositivo gemelo, puede establecer etiquetas para agrupar dispositivos por cualquier métrica que necesite en su solución, como la región, el entorno, la ubicación o el tipo de dispositivo. Estas etiquetas se usan para crear [implementaciones automáticas](how-to-deploy-at-scale.md).

### <a name="verify-your-root-certificate"></a>Comprobación del certificado raíz

Al crear un grupo de inscripción, tiene la opción de usar un certificado comprobado. Puede comprobar un certificado con DPS al probar que tiene la propiedad del certificado raíz. Para obtener más información, consulte [Realización de una prueba de posesión de certificados de entidad de certificación X.509](../iot-dps/how-to-verify-certificates.md).

1. En [Azure Portal](https://portal.azure.com), navegue hasta la instancia de IoT Hub Device Provisioning Service.

1. Seleccione **Certificados** en el menú de la izquierda.

1. Seleccione **Agregar** para agregar un certificado nuevo.

1. Escriba un nombre descriptivo para el certificado y después busque el archivo .cer o .pem que representa la parte pública del certificado X.509.

   Si usa los certificados de demostración, cargue el certificado `<wrkdir>/certs/azure-iot-test-only.root.ca.cert.pem`.

1. Seleccione **Guardar**.

1. Ahora, el certificado debe aparecer en la página **Certificados**. Selecciónelo para abrir los detalles del certificado.

1. Seleccione **Generar código de comprobación** y, a continuación, copie el código generado.

1. Tanto si proporcionó su propio certificado de CA como si usa los certificados de demostración, puede usar la herramienta de comprobación incluida en el repositorio de IoT Edge para realizar la prueba de posesión. La herramienta de comprobación usa el certificado de CA para firmar un nuevo certificado que tiene el código de comprobación especificado como nombre de asunto.

   * Windows:

     ```powershell
     New-CACertsVerificationCert "<verification code>"
     ```

   * Linux:

     ```bash
     ./certGen.sh create_verification_certificate <verification code>
     ```

1. En la misma página de detalles del certificado en Azure Portal, cargue el certificado de comprobación recién generado.

1. Seleccione **Comprobar**.

### <a name="create-enrollment-group"></a>Creación del grupo de inscripción

Para más información sobre las inscripciones en Device Provisioning Service, consulte [Administración de inscripciones de dispositivos](../iot-dps/how-to-manage-enrollments.md).

1. En [Azure Portal](https://portal.azure.com), navegue hasta la instancia de IoT Hub Device Provisioning Service.

1. En **Configuración**, seleccione **Administrar inscripciones**.

1. Seleccione **Add enrollment group** (Agregar grupo de inscripción) y, a continuación, complete los pasos siguientes para configurar la inscripción:

   * **Nombre de grupo**: proporcione un nombre fácil de recordar para esta inscripción de grupo.

   * **Tipo de atestación**: Seleccione **Certificado**.

   * **Dispositivo IoT Edge**: Seleccione **True**. En el caso de una inscripción de grupo, todos los dispositivos deben ser dispositivos IoT Edge, o bien ninguno puede serlo.

   * **Tipo de certificado**: seleccione **Certificado de entidad de certificación** si tiene almacenado un certificado de CA comprobado con DPS o **Intermediate Certificate** (Certificado intermedio) si quiere cargar un archivo nuevo solo para esta inscripción.

   * **Certificado principal**: si seleccionó el certificado de CA en la última sección, elija el certificado de la lista desplegable. Si seleccionó el certificado intermedio, cargue el archivo público de un certificado de CA en la cadena de certificados de confianza que se usó para generar los certificados de identidad del dispositivo.

   * **Seleccione los centros de IoT a los que se puede asignar este dispositivo**: elija la instancia de IoT Hub vinculada a la que quiere conectar el dispositivo. Puede elegir varios centros y el dispositivo se asignará a uno de ellos según la directiva de asignación seleccionada.

   * **Estado inicial de dispositivo gemelo**: agregue un valor de etiqueta para que se agregue al dispositivo gemelo, si lo desea. Puede usar etiquetas para los grupos de dispositivos de destino de la implementación automática. Por ejemplo:

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

1. Seleccione **Guardar**.

Ahora que existe una inscripción para este dispositivo, el entorno de ejecución de Azure IoT Edge puede aprovisionar automáticamente el dispositivo durante la instalación. Continúe con la sección siguiente para configurar el dispositivo IoT Edge.

## <a name="install-the-iot-edge-runtime"></a>Instalación del entorno de ejecución de IoT Edge

El runtime de IoT Edge se implementa en todos los dispositivos de IoT Edge. Sus componentes se ejecutan en contenedores y permiten implementar contenedores adicionales en el dispositivo para que pueda ejecutar código en Edge.

El aprovisionamiento de X.509 con DPS solo se admite en IoT Edge versión 1.0.9 o posterior.

Al aprovisionar el dispositivo necesitará la información siguiente:

* El valor de **Ámbito de id.** del DPS. Puede recuperar este valor de la página Información general de la instancia de DPS en Azure Portal.
* El archivo de cadena de certificados de identidad del dispositivo en el dispositivo.
* El archivo de clave de identidad del dispositivo en el dispositivo.
* Un id. de registro opcional (si no se especifica, se extrae del nombre común en el certificado de identidad del dispositivo).

### <a name="linux-device"></a>Dispositivo Linux

Use el siguiente vínculo para instalar el entorno de ejecución de Azure IoT Edge en el dispositivo con los comandos adecuados para la arquitectura del dispositivo. Cuando llegue a la sección sobre la configuración del demonio de seguridad, configure el entorno de ejecución de Azure IoT Edge para el aprovisionamiento de X.509 automático, en lugar del aprovisionamiento manual. Después de completar las secciones anteriores de este artículo, debería tener toda la información y los archivos de certificado que necesita.

[Instalación del entorno de ejecución de Azure IoT Edge en Linux](how-to-install-iot-edge-linux.md)

Al agregar el certificado X.509 y la información de clave al archivo config.yaml, las rutas de acceso se deben especificas como URI de archivo. Por ejemplo:

* `file:///<path>/identity_certificate_chain.pem`
* `file:///<path>/identity_key.pem`

La sección del archivo de configuración para el aprovisionamiento automático de X.509 tiene el siguiente aspecto:

```yaml
# DPS X.509 provisioning configuration
provisioning:
  source: "dps"
  global_endpoint: "https://global.azure-devices-provisioning.net"
  scope_id: "<SCOPE_ID>"
  attestation:
    method: "x509"
#   registration_id: "<OPTIONAL REGISTRATION ID. LEAVE COMMENTED OUT TO REGISTER WITH CN OF identity_cert>"
    identity_cert: "<REQUIRED URI TO DEVICE IDENTITY CERTIFICATE>"
    identity_pk: "<REQUIRED URI TO DEVICE IDENTITY PRIVATE KEY>"
```

Reemplace los valores de marcador de posición de `scope_id`, `identity_cert`, `identity_pk` por el identificador de ámbito de la instancia de DPS y los URI que apuntan a la cadena de certificados y las ubicaciones de los archivos de clave en el dispositivo. Si lo desea, proporcione un `registration_id` para el dispositivo o deje esta línea como comentada para registrar el dispositivo con el nombre común del certificado de identidad.

Siempre reinicie el demonio de seguridad después de actualizar el archivo config.yaml.

```bash
sudo systemctl restart iotedge
```

### <a name="windows-device"></a>Dispositivo Windows

Instale el entorno de ejecución de Azure IoT Edge en el dispositivo para el que generó la cadena de certificados de identidad y la clave de identidad. Configurará el entorno de ejecución de IoT Edge para el aprovisionamiento automático, no manual.

Para información más detallada sobre cómo instalar IoT Edge en Windows, incluidos los requisitos previos y las instrucciones para tareas como la administración de contenedores y la actualización de IoT Edge, consulte [Instalación del entorno de ejecución de Azure IoT Edge en Windows](how-to-install-iot-edge-windows.md).

1. Abra una ventana de Azure PowerShell en modo de administrador. Asegúrese de usar una sesión de AMD64 de PowerShell para instalar IoT Edge, no PowerShell (x86).

1. El comando **Deploy-IoTEdge** comprueba si la versión del equipo Windows es compatible, activa la característica de contenedores y descarga tanto el runtime de Moby como el de IoT Edge. De forma predeterminada el comando usa contenedores Windows.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

1. En este momento, los dispositivos IoT Core pueden reiniciarse automáticamente. Es posible que otros dispositivos Windows 10 o Windows Server soliciten su reinicio. En ese caso, reinícielo ahora. Una vez que el dispositivo esté listo, vuelva a ejecutar PowerShell como administrador.

1. El comando **Initialize-IoTEdge** configura el entorno de ejecución de Azure IoT Edge en el equipo. El comando tiene como valor predeterminado el aprovisionamiento manual a menos que use la marca `-Dps` para usar el aprovisionamiento automático.

   Reemplace los valores de marcador de posición de `{scope_id}`, `{identity cert chain path}` y `{identity key path}` por los valores adecuados de la instancia de DPS y las rutas de acceso de archivo en el dispositivo. Si quiere especificar el id. de registro, incluya también `-RegistrationId {registration_id}` y reemplace el marcador de posición según corresponda.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -Dps -ScopeId {scope ID} -X509IdentityCertificate {identity cert chain path} -X509IdentityPrivateKey {identity key path}
   ```

   >[!TIP]
   >El archivo config.yaml almacena la información de certificado y clave como URI de archivo. Sin embargo, el comando Initialize-IoTEdge controla este paso de formato automáticamente. Por ello, puede proporcionar la ruta de acceso absoluta a los archivos de clave y certificado del dispositivo.

## <a name="verify-successful-installation"></a>Comprobación de instalación correcta

Si el entorno de ejecución se inició correctamente, puede ir a IoT Hub y empezar a implementar módulos de IoT Edge en el dispositivo.

Puede comprobar que la inscripción individual que ha creado se ha utilizado en el servicio Device Provisioning. En Azure Portal, vaya a la instancia del servicio Device Provisioning. Abra los detalles de la inscripción para la inscripción individual que ha creado. Tenga en cuenta que el estado de la inscripción está **asignado** y se muestra el id. de dispositivo.

Use los siguientes comandos en el dispositivo para comprobar que el entorno de ejecución está instalado e iniciado correctamente.

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

## <a name="next-steps"></a>Pasos siguientes

El proceso de inscripción en Device Provisioning Service permite establecer el id. de dispositivo y las etiquetas del dispositivo gemelo al mismo tiempo que aprovisiona el nuevo dispositivo. Puede usar esos valores para dirigirse a dispositivos individuales o grupos de dispositivos con la administración automática de dispositivos. Aprenda a [implementar y supervisar los módulos de IoT Edge a escala mediante Azure Portal](how-to-deploy-at-scale.md) o la [CLI de Azure](how-to-deploy-cli-at-scale.md).
