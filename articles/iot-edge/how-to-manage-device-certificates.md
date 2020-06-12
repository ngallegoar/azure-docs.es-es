---
title: 'Administración de certificados de dispositivo: Azure IoT Edge | Microsoft Docs'
description: Cree certificados de prueba, instálelos y adminístrelos en un dispositivo Azure IoT Edge para preparar la implementación de producción.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/02/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: b13944e30c339357997fbc5f0919e5eb8485a0a9
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84308785"
---
# <a name="manage-certificates-on-an-iot-edge-device"></a>Administración de certificados en un dispositivo IoT Edge

Todos los dispositivos IoT Edge usan certificados para crear conexiones seguras entre el entorno de ejecución y los módulos que se ejecutan en el dispositivo. Los dispositivos IoT Edge que funcionan como puertas de enlace usan estos mismos certificados para conectarse también a sus dispositivos de nivel inferior.

## <a name="install-production-certificates"></a>Instalar certificados de producción

La primera vez que instala IoT Edge y aprovisiona el dispositivo, el dispositivo se configura con certificados temporales para que pueda probar el servicio.
Estos certificados temporales expiran en 90 días, o bien se puede reiniciar el equipo para restablecerlos.
Una vez realizada la migración a un escenario de producción, o si quiere crear un dispositivo de puerta de enlace, tendrá que proporcionar certificados propios.
En este artículo se muestran los pasos para instalar certificados en los dispositivos IoT Edge.

Para más información sobre los diferentes tipos de certificados y sus roles, vea [Información sobre los certificados de Azure IoT Edge](iot-edge-certs.md).

>[!NOTE]
>El término "entidad de certificación raíz" que se usa en este artículo hace referencia al certificado público de la entidad de nivel superior de la cadena de certificados de la solución de IoT. No es necesario usar la raíz del certificado de una entidad de certificación sindicada o la raíz de la entidad de certificación de la organización. En muchos casos, se trata realmente de un certificado público intermedio de la entidad de certificación.

### <a name="prerequisites"></a>Requisitos previos

* Un dispositivo IoT Edge que se ejecute en [Windows](how-to-install-iot-edge-windows.md) o [Linux](how-to-install-iot-edge-linux.md).
* Tener un certificado de entidad de certificación (CA) raíz, ya sea autofirmado o comprado a través de una entidad de certificación comercial de confianza como Baltimore, Verisign, DigiCert o GlobalSign.

Si todavía no tiene una entidad de certificación raíz pero quiere probar características de IoT Edge que requieren certificados de producción (por ejemplo, escenarios de puerta de enlace), puede [crear certificados de demostración para probar las características del dispositivo IoT Edge](how-to-create-test-certificates.md).

### <a name="create-production-certificates"></a>Creación de certificados de producción

Debe usar una entidad de certificación propia para crear los archivos siguientes:

* Entidad de certificación raíz
* Certificado de entidad de certificación de dispositivo
* Clave privada de entidad de certificación del dispositivo

En este artículo, lo que se denomina *entidad de certificación raíz* no es la entidad de certificación de nivel superior de una organización. Es la entidad de certificación de nivel superior para el escenario de IoT Edge, que el módulo de IoT Edge Hub, los módulos de usuario y los dispositivos de nivel inferior usan para establecer la confianza entre sí.

Para ver un ejemplo de estos certificados, revise los scripts que crean los certificados de demostración en [Administración de certificados de entidad de certificación de prueba para ejemplos y tutoriales](https://github.com/Azure/iotedge/tree/master/tools/CACertificates).

### <a name="install-certificates-on-the-device"></a>Instalación de certificados en el dispositivo

Instale la cadena de certificados en el dispositivo IoT Edge y configure el entorno de ejecución de IoT Edge para que haga referencia a los nuevos certificados.

Por ejemplo, si ha usado los scripts de ejemplo para [Crear certificados de demostración](how-to-create-test-certificates.md), copie los siguientes archivos en el dispositivo IoT Edge:

* Certificado de entidad de certificación del dispositivo: `<WRKDIR>\certs\iot-edge-device-MyEdgeDeviceCA-full-chain.cert.pem`
* Clave privada de entidad de certificación del dispositivo: `<WRKDIR>\private\iot-edge-device-MyEdgeDeviceCA.key.pem`
* Entidad de certificación raíz: `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

1. Copie los tres archivos de certificado y clave en el dispositivo IoT Edge.

   Puede usar un servicio como [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) o una función como [Protocolo de copia segura](https://www.ssh.com/ssh/scp/) para mover los archivos de certificado.  Si ha generado los certificados en el propio dispositivo IoT Edge, puede omitir este paso y usar la ruta de acceso al directorio de trabajo.

1. Abra el archivo de configuración del demonio de seguridad de IoT Edge.

   * Windows: `C:\ProgramData\iotedge\config.yaml`
   * Linux: `/etc/iotedge/config.yaml`

1. Establezca las propiedades de **certificado** de config.yaml en la ruta de acceso del URI de archivo de los archivos de certificado y de clave del dispositivo IoT Edge. Elimine el carácter `#` antes de las propiedades de certificado para quitar la marca de comentario de las cuatro líneas. Asegúrese de que la línea **certificates:** no tiene ningún espacio en blanco delante y de que los elementos anidados muestran una sangría de dos espacios. Por ejemplo:

   * Windows:

      ```yaml
      certificates:
        device_ca_cert: "file:///C:/<path>/<device CA cert>"
        device_ca_pk: "file:///C:/<path>/<device CA key>"
        trusted_ca_certs: "file:///C:/<path>/<root CA cert>"
      ```

   * Linux:

      ```yaml
      certificates:
        device_ca_cert: "file:///<path>/<device CA cert>"
        device_ca_pk: "file:///<path>/<device CA key>"
        trusted_ca_certs: "file:///<path>/<root CA cert>"
      ```

1. En los dispositivos Linux, asegúrese de que el usuario **iotedge** tiene permisos de lectura para el directorio que contiene los certificados.

1. Si anteriormente ha usado cualquier otro certificado para IoT Edge en el dispositivo, elimine los archivos de los dos directorios siguientes antes de iniciar o reiniciar IoT Edge:

   * Windows: `C:\ProgramData\iotedge\hsm\certs` y `C:\ProgramData\iotedge\hsm\cert_keys`

   * Linux: `/var/lib/iotedge/hsm/certs` y `/var/lib/iotedge/hsm/cert_keys`

## <a name="customize-certificate-lifetime"></a>Personalizar vigencia del certificado

IoT Edge genera automáticamente certificados en el dispositivo en varios casos, entre los que se incluyen:

* Si no proporciona sus propios certificados de producción al instalar y aprovisionar IoT Edge, el administrador de seguridad de IoT Edge genera automáticamente un **certificado de CA del dispositivo**. Este certificado autofirmado está concebido únicamente para escenarios de desarrollo y pruebas, no para producción. Este certificado expira después de 90 días.
* El administrador de seguridad de IoT Edge también genera un **certificado de CA de carga de trabajo** firmado por el certificado de CA del dispositivo

Para obtener más información sobre la función de los distintos certificados en un dispositivo IoT Edge, consulte [Información sobre los certificados de Azure IoT Edge](iot-edge-certs.md).

En el caso de estos dos certificados generados automáticamente, tiene la opción de establecer la marca **auto_generated_ca_lifetime_days** en config.yaml para configurar el número de días para la vigencia de los certificados.

>[!NOTE]
>Hay un tercer certificado generado automáticamente que crea el administrador de seguridad de IoT Edge, el **certificado de servidor del centro de IoT Edge**. Este certificado siempre tiene una vigencia de 90 días, pero siempre se renueva automáticamente antes de expirar. El valor **auto_generated_ca_lifetime_days** no afecta a este certificado.

Para configurar la expiración de certificados en un valor distinto a los 90 días predeterminados, agregue el valor en días a la sección **Certificados** del archivo config.yaml.

```yaml
certificates:
  device_ca_cert: "<ADD URI TO DEVICE CA CERTIFICATE HERE>"
  device_ca_pk: "<ADD URI TO DEVICE CA PRIVATE KEY HERE>"
  trusted_ca_certs: "<ADD URI TO TRUSTED CA CERTIFICATES HERE>"
  auto_generated_ca_lifetime_days: <value>
```

Si ha proporcionado sus propios certificados de CA del dispositivo, este valor seguirá aplicándose al certificado de CA de carga de trabajo, siempre que el valor de vigencia establecido sea menor que la vigencia del certificado de CA del dispositivo.

Una vez que especifique la marca en el archivo config.yaml, siga los pasos siguientes:

1. Elimine el contenido de la carpeta `hsm`.

   Windows: `C:\ProgramData\iotedge\hsm\certs and C:\ProgramData\iotedge\hsm\cert_keys`Linux: `/var/lib/iotedge/hsm/certs and /var/lib/iotedge/hsm/cert_keys`

1. Reinicie el servicio IoT Edge.

   Windows:

   ```powershell
   Restart-Service iotedge
   ```

   Linux:

   ```bash
   sudo systemctl restart iotedge
   ```

1. Confirme el valor de vigencia.

   Windows:

   ```powershell
   iotedge check --verbose
   ```

   Linux:

   ```bash
   sudo iotedge check --verbose
   ```

   Compruebe la salida de la comprobación **preparación de producción: certificados**, que muestra el número de días hasta la expiración de los certificados de CA del dispositivo generados automáticamente.

## <a name="next-steps"></a>Pasos siguientes

La instalación de certificados en un dispositivo IoT Edge es un paso necesario antes de implementar la solución en producción. Obtenga más información sobre cómo [preparar la implementación de la solución de IoT Edge en producción](production-checklist.md).
