---
title: Conexión de dispositivos con certificados X.509 en una aplicación de Azure IoT Central
description: Procedimiento para conectar dispositivos con certificados X.509 mediante el SDK de dispositivo de Node.js para la aplicación de IoT Central
author: dominicbetts
ms.author: dobett
ms.date: 08/12/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.custom: device-developer
ms.openlocfilehash: 33d837f63fca2062ec930fcf0d64ee01ea822c99
ms.sourcegitcommit: 9889a3983b88222c30275fd0cfe60807976fd65b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94989537"
---
# <a name="how-to-connect-devices-with-x509-certificates-using-nodejs-device-sdk-for-iot-central-application"></a>Procedimiento para conectar dispositivos con certificados X.509 mediante el SDK de dispositivo de Node.js para la aplicación de IoT Central

IoT Central admite firmas de acceso compartido (SAS) y certificados X.509 para proteger la comunicación entre un dispositivo y la aplicación. En el tutorial [Creación y conexión de un aplicación cliente a una aplicación de Azure IoT Central](./tutorial-connect-device-nodejs.md) se usa SAS. En este artículo, aprenderá a modificar el ejemplo de código para usar X.509.  Los certificados X.509 se recomiendan en entornos de producción. Para obtener más información, consulte [Conexión a Azure IoT Central](./concepts-get-connected.md).

En este artículo se muestran dos formas de usar X.509: en [inscripciones de grupo](how-to-connect-devices-x509.md#use-a-group-enrollment), que se usan normalmente en un entorno de producción, y en [inscripciones individuales](how-to-connect-devices-x509.md#use-an-individual-enrollment), que resultan útiles para las pruebas.

## <a name="prerequisites"></a>Requisitos previos

- Haber completado el tutorial [Creación y conexión de un aplicación cliente a una aplicación de Azure IoT Central (Node.js)](./tutorial-connect-device-nodejs.md).
- [Git](https://git-scm.com/download/).
- Descargar e instalar [OpenSSL](https://www.openssl.org/). Si usa Windows, puede usar los archivos binarios de la [página de OpenSSL en SourceForge](https://sourceforge.net/projects/openssl/).

## <a name="use-a-group-enrollment"></a>Uso de una inscripción de grupo

Use certificados X.509 con una inscripción de grupo en un entorno de producción. En una inscripción de grupo, agregue un certificado X.509 raíz o intermedio a la aplicación de IoT Central. Los dispositivos con certificados hoja derivados del certificado raíz o intermedio pueden conectarse a la aplicación.

## <a name="generate-root-and-device-cert"></a>Generación de certificados raíz y de dispositivo

En esta sección, se usa un certificado X.509 para conectar un dispositivo con un certificado derivado del certificado del grupo de inscripciones, que puede conectarse a la aplicación de IoT Central.

> [!WARNING]
> Esta forma de generar certificados X.509 es solo para pruebas. En un entorno de producción, debe usar su mecanismo oficial y seguro para la generación de certificados.

1. Abra un símbolo del sistema. Clone el repositorio de GitHub para los scripts de generación de certificados:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-node.git
    ```

1. Navegue hasta el script del generador de certificados e instale los paquetes necesarios:

    ```cmd/sh
    cd azure-iot-sdk-node/provisioning/tools
    npm install
    ```

1. Cree un certificado raíz y luego derive un certificado de dispositivo mediante la ejecución del script:

    ```cmd/sh
    node create_test_cert.js root mytestrootcert
    node create_test_cert.js device sample-device-01 mytestrootcert
    ```

    > [!TIP]
    > Un identificador de dispositivo puede contener letras, números y el carácter `-`.

Estos comandos generan tres archivos cada uno para el certificado raíz y el certificado de dispositivo.

filename | contenido
-------- | --------
\<name\>_cert.pem | Parte pública del certificado X.509
\<name\>_key.pem | Clave privada del certificado X.509
\<name\>_fullchain.pem | Cadena de claves completa del certificado X.509

## <a name="create-a-group-enrollment"></a>Creación de una inscripción de grupo

1. Abra la aplicación de IoT Central, vaya a **Administración**, en el panel izquierdo, y seleccione **Conexión del dispositivo**.

1. Seleccione **+ Agregar grupo de inscripciones** y cree uno nuevo con el nombre _MyX509Group_ y el tipo de atestación **Certificados (X.509)** .

1. Abra el grupo de inscripciones que ha creado y seleccione **Administrar principal**.

1. Seleccione la opción Archivo y cargue el archivo de certificado raíz llamado _mytestrootcert_cert.pem_ generado anteriormente:

    ![Carga de certificados](./media/how-to-connect-devices-x509/certificate-upload.png)

1. Para completar la comprobación, genere el código de verificación, cópielo y úselo para crear un certificado de verificación X.509 en el símbolo del sistema:

    ```cmd/sh
    node create_test_cert.js verification --ca mytestrootcert_cert.pem --key mytestrootcert_key.pem --nonce  {verification-code}
    ```

1. Cargue el certificado de verificación firmado _verification_cert.pem_ para completar la comprobación:

    ![Certificado comprobado](./media/how-to-connect-devices-x509/verified.png)

Ahora podrá conectar los dispositivos que tengan un certificado X.509 derivado de este certificado raíz principal.

Después de guardar el grupo de inscripciones, tome nota del ámbito de identificador.

## <a name="run-sample-device-code"></a>Ejecución del código del dispositivo de ejemplo

1. Copie los archivos **sampleDevice01_key.pem** y **sampleDevice01_cert.pem** en la carpeta _azure-iot-sdk-node/device/samples/pnp_ que contiene la aplicación **simple_thermostat.js**. Utilizó esta aplicación cuando completó el tutorial [Conexión de un dispositivo (Node.js)](./tutorial-connect-device-nodejs.md).

1. Vaya a la carpeta _azure-iot-sdk-node/device/samples/pnp_ que contiene la aplicación **simple_thermostat.js** y ejecute el siguiente comando para instalar el paquete X.509:

    ```cmd/sh
    npm install azure-iot-security-x509 --save
    ```

1. Abra el archivo **simple_thermostat.js** en un editor de texto.

1. Edite las instrucciones `require` para incluir lo siguiente:

    ```javascript
    const fs = require('fs');
    const X509Security = require('azure-iot-security-x509').X509Security;
    ```

1. Agregue las cuatro líneas siguientes a la sección "DPS connection information" (Información de conexión de DPS) para inicializar la variable `deviceCert`:

    ```javascript
    const deviceCert = {
      cert: fs.readFileSync(process.env.IOTHUB_DEVICE_X509_CERT).toString(),
      key: fs.readFileSync(process.env.IOTHUB_DEVICE_X509_KEY).toString()
    };
    ```

1. Edite la función `provisionDevice` que crea el cliente; para ello, reemplace la primera línea por lo siguiente:

    ```javascript
    var provSecurityClient = new X509Security(registrationId, deviceCert);
    ```

1. En la misma función, modifique la línea que establece la variable `deviceConnectionString` como se indica a continuación:

    ```javascript
    deviceConnectionString = 'HostName=' + result.assignedHub + ';DeviceId=' + result.deviceId + ';x509=true';
    ```

1. En la función `main`, agregue la siguiente línea después de la línea que llama a `Client.fromConnectionString`:

    ```javascript
    client.setOptions(deviceCert);
    ```

1. En el entorno de shell, establezca las dos variables de entorno siguientes:

    ```cmd/sh
    set IOTHUB_DEVICE_X509_CERT=sampleDevice01_cert.pem
    set IOTHUB_DEVICE_X509_KEY=sampleDevice01_key.pem
    ```

    > [!TIP]
    > Establezca las otras dos variables de entorno necesarias cuando complete el tutorial [Creación y conexión de un aplicación cliente a una aplicación de Azure IoT Central](./tutorial-connect-device-nodejs.md).

1. Ejecute el script y compruebe que el dispositivo se haya aprovisionado correctamente:

    ```cmd/sh
    node simple_thermostat.js
    ```

    También puede comprobar que la telemetría aparezca en el panel.

    ![Comprobación de la telemetría del dispositivo](./media/how-to-connect-devices-x509/telemetry.png)

## <a name="use-an-individual-enrollment"></a>Uso de una inscripción individual

Use certificados X.509 con una inscripción individual para probar el dispositivo y la solución. En una inscripción individual, no hay ningún certificado X.509 intermedio o raíz en la aplicación de IoT Central. Los dispositivos usan un certificado X.509 autofirmado para conectarse a la aplicación.

## <a name="generate-self-signed-device-cert"></a>Generación de certificados autofirmados de los dispositivos

En esta sección, se usa un certificado X.509 autofirmado para conectar dispositivos para la inscripción individual; su uso permite la inscripción de un único dispositivo. Los certificados autofirmados son solo para pruebas.

Cree un certificado X.509 autofirmado del dispositivo mediante la ejecución del script. Asegúrese de usar solo caracteres alfanuméricos en minúsculas y guiones para el nombre del certificado:

  ```cmd/sh
    cd azure-iot-sdk-node/provisioning/tools
    node create_test_cert.js device mytestselfcertprimary
    node create_test_cert.js device mytestselfcertsecondary 
  ```

## <a name="create-individual-enrollment"></a>Creación de una inscripción individual

1. En la aplicación de Azure IoT Central, seleccione **Dispositivos** y cree otro dispositivo con _mytestselfcertprimary_ como **Id. de dispositivo** en la plantilla del dispositivo termostato. Tome nota del valor **Ámbito de id.** , ya que lo usará más adelante.

1. Abra el dispositivo que acaba de crear y seleccione **Conectar**.

1. Seleccione **Inscripciones individuales** como **Método de conexión** y **Certificados (X.509)** como mecanismo:

    ![Inscripción individual](./media/how-to-connect-devices-x509/individual-device-connect.png)

1. Seleccione la opción Archivo en principal y cargue el archivo del certificado llamado _mytestselfcertprimary_cert.pem_ generado anteriormente.

1. Seleccione la opción Archivo del certificado secundario y cargue el archivo del certificado llamado _mytestselfcertsecondary_cert.pem_. Después, seleccione **Save** (Guardar):

    ![Carga de certificados de inscripción individual](./media/how-to-connect-devices-x509/individual-enrollment.png)

El dispositivo ya está aprovisionado con el certificado X.509.

## <a name="run-a-sample-individual-enrollment-device"></a>Ejecución de un dispositivo de inscripción individual de ejemplo

1. Copie los archivos _mytestselfcertprimary_key.pem_ y _mytestselfcertprimary_cert.pem_ en la carpeta _azure-iot-sdk-node/device/samples/pnp_ que contiene la aplicación **simple_thermostat.js**. Utilizó esta aplicación cuando completó el tutorial [Conexión de un dispositivo (Node.js)](./tutorial-connect-device-nodejs.md).

1. Modifique las variables de entorno que usó de la siguiente manera:

    ```cmd/sh
    set IOTHUB_DEVICE_DPS_DEVICE_ID=mytestselfcertprimary
    set IOTHUB_DEVICE_X509_CERT=mytestselfcertprimary_cert.pem
    set IOTHUB_DEVICE_X509_KEY=mytestselfcertprimary_key.pem
    ```

1. Ejecute el script y compruebe que el dispositivo se haya aprovisionado correctamente:

    ```cmd/sh
    node environmentalSensor.js
    ```

    También puede comprobar que la telemetría aparezca en el panel.

    ![Inscripción individual de la telemetría](./media/how-to-connect-devices-x509/telemetry-primary.png)

También puede repetir los pasos anteriores para el certificado _mytestselfcertsecondary_.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a conectar dispositivos mediante certificados X.509, el paso siguiente que se sugiere es consultar el artículo [Supervisión de la conectividad de dispositivos mediante la CLI de Azure](howto-monitor-devices-azure-cli.md).
