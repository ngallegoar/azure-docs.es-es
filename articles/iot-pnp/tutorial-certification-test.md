---
title: Certificación de un dispositivo IoT Plug and Play (versión preliminar) | Microsoft Docs
description: En este tutorial se describe la forma de agregar la información del producto al catálogo de dispositivos de Azure Certified for IoT, conectar el dispositivo al servicio de certificación de Azure IoT y luego ejecutar las pruebas de certificación de IoT Plug and Play.
manager: philmea
ms.service: iot-pnp
services: iot-pnp
ms.topic: tutorial
ms.author: koichih
author: konichi3
ms.date: 12/27/2019
ms.openlocfilehash: dc2c33659f3f3a3df0f11fcc6ab36a9fc993da43
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "75550169"
---
# <a name="tutorial-certify-your-iot-plug-and-play-preview-device"></a>Tutorial: Certificación de un dispositivo IoT Plug and Play (versión preliminar)

Para publicar un dispositivo IoT Plug and Play (versión preliminar) en el [catálogo de dispositivos Azure Certified for IoT](https://aka.ms/iotdevcat), debe pasar una serie de pruebas de certificación. Use el portal de [Azure Certified for IoT](https://aka.ms/ACFI) para enviar el dispositivo y realizar su certificación. El [servicio de certificación de Azure IoT](https://aka.ms/azure-iot-aics) ejecuta las pruebas de certificación.

En este tutorial, aprenderá:

> [!div class="checklist"]
> * Cuáles son los requisitos de certificación de IoT Plug and Play
> * Cómo agregar el nombre y la información del producto al portal
> * Cómo conectar y detectar interfaces de IoT Plug and Play
> * Cómo revisar las interfaces de IoT Plug and Play y ejecutar pruebas de certificación
> * Cómo publicar el dispositivo IoT Plug and Play certificado en el catálogo

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial, necesita:

* Una cuenta del Centro de partners de Microsoft
* El identificador de Microsoft Partner Network

Para más información, consulte [Incorporación al portal de Azure Certified for IoT](howto-onboard-portal.md).

## <a name="certification-requirements"></a>Requisitos de certificación

Para certificar el dispositivo IoT Plug and Play, debe cumplir los siguientes requisitos:

* El código del dispositivo IoT Plug and Play debe estar instalado en el dispositivo.
* El código del dispositivo IoT Plug and Play se ha creado con el SDK de Azure IoT.
* El código del dispositivo debe admitir [Azure IoT Hub Device Provisioning Service](../iot-dps/about-iot-dps.md).
* El código del dispositivo debe implementar la [interfaz de información del dispositivo](concepts-common-interfaces.md).
* El modelo de funcionalidad y el código de dispositivo funcionan con IoT Central.

Todos los dispositivos que se encuentran actualmente en el catálogo se consideran dispositivos IoT Plug and Play certificados previamente. No se garantiza la calidad ni el cumplimiento del producto final de los componentes de software de IoT Plug and Play, como el SDK y el lenguaje de definición de gemelos digitales.

Todos los dispositivos IoT Plug and Play certificados previamente deben volver a certificarse tras la disponibilidad general de IoT Plug and Play según la versión final de los requisitos de certificación y los componentes de software proporcionados por Microsoft.

## <a name="add-product-name"></a>Adición del nombre del producto

**Producto** es un nombre descriptivo de un modelo de producto que puede comprar un cliente. Para agregar un producto, siga estos pasos:

1. Inicie sesión en el [portal de Azure Certified for IoT](https://aka.ms/ACFI).
1. Vaya a la página **Products** (Productos) del portal en el menú izquierdo y seleccione **+ New** (+ Nuevo).
1. Escriba el nombre descriptivo del producto y seleccione **Create** (Crear). El nombre aquí especificado es diferente del nombre que se muestra en el catálogo de dispositivos.

## <a name="add-product-information"></a>Adición de información del producto

Después de haber creado correctamente el producto en el portal, este se muestra en la página **Products** (Productos). Para agregar la información del producto, siga estos pasos:

1. Seleccione el vínculo del producto creado que se encuentra en la página **Product** (Producto) de la columna del producto. El estado debe ser borrador.
1. Seleccione el vínculo **Edit** (Editar) junto al encabezado **Product information** (Información del producto). Escriba información sobre el producto en la página de información del producto y asegúrese de que ha completado todos los campos obligatorios.
1. Seleccione **Guardar**. El botón **Save** (Guardar) solo aparece cuando se completan todos los campos obligatorios. Si los campos están incompletos, el botón indica **Save and finish later** (Guardar y finalizar más tarde).
1. Revise el contenido que ha especificado. Complete todos los campos obligatorios para publicar el dispositivo en el catálogo de dispositivos. Siempre puede volver y realizar modificaciones en la información del producto en la página de detalles del producto; para ello, haga clic en el vínculo **Edit** (Editar) junto al encabezado **Product information** (Información del producto).

## <a name="connect-and-discover-interfaces"></a>Conexión y detección de interfaces

Para ejecutar las pruebas de certificación, conecte el dispositivo a [Azure IoT Certification Service](https://aka.ms/azure-iot-aics) (AICS) que está disponible en el portal.

Estos pasos constituyen un único paso para la ejecución de las pruebas de certificación y no es necesario cambiar el código de dispositivo del producto. Para empezar, siga estos pasos para conectarse a AICS:

1. Inicie sesión en el portal con su cuenta del Centro de partners.
1. Haga clic en **Connect + test** (Conectar y probar) para comenzar el flujo de certificación.
1. Elija el [método de autenticación](../iot-dps/concepts-security.md#attestation-mechanism) para aprovisionar el dispositivo en AICS mediante [Azure IoT Hub Device Provisioning Service](../iot-dps/about-iot-dps.md).
   * Si va a usar un [certificado X.509](../iot-hub/iot-hub-security-x509-get-started.md#prerequisites), cargue el certificado X.509 generado. Puede que quiera revisar el código de ejemplo que muestra cómo usar los certificados X.509: [C](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/samples/iothub_ll_client_x509_sample/iothub_ll_client_x509_sample.c), [C#](../iot-hub/iot-hub-security-x509-get-started.md).
   * Si va a usar una [clave simétrica](../iot-dps/concepts-symmetric-key-attestation.md), cópiela y péguela en el código del dispositivo.
   * En este momento no se admite el método de autenticación de TPM.
1. Copie y pegue los siguientes identificadores generados en el código del dispositivo.
   * [Identificador de registro](../iot-dps/use-hsm-with-sdk.md)
   * [Identificador de DPS](../iot-dps/tutorial-set-up-device.md#create-the-device-registration-software)
   * [Punto de conexión de DPS](../iot-dps/tutorial-set-up-device.md#create-the-device-registration-software)
1. Cuando el código del dispositivo se compile e implemente en el dispositivo, seleccione **Connect** (Conectar) para detectar las interfaces de IoT Plug and Play.
1. Cuando la conexión a AICS se realice correctamente, seleccione **Next** (Siguiente) para revisar las interfaces de IoT Plug and Play detectadas.

## <a name="run-tests-and-publish-the-device"></a>Ejecución de pruebas y publicación del dispositivo

En la página de revisión, puede revisar las interfaces de IoT Plug and Play detectadas. Use esta página para comprobar que las primitivas implementadas en la interfaz se muestran correctamente. También puede comprobar la ubicación de la interfaz.

1. Asegúrese de especificar entradas de carga para los campos obligatorios. Estos campos incluyen información de carga de la primitiva del comando de la interfaz especificada.
1. Cuando haya especificado toda la información necesaria, seleccione **Next** (Siguiente).
1. Para ejecutar las pruebas de las interfaces de IoT Plug and Play implementadas, seleccione **Run tests** (Ejecutar pruebas).
1. Todas las pruebas se ejecutan automáticamente. Si alguna de ellas genera un error, seleccione **View logs** (Ver registros) para ver los mensajes de error de AICS y la telemetría sin procesar enviada a Azure IoT Hub.
1. Para completar las pruebas de certificación, seleccione **Finish** (Finalizar).
1. Publique el dispositivo IoT Plug and Play certificado en el catálogo. Para agregar el dispositivo certificado al catálogo, seleccione **Add to catalog** (Agregar al catálogo) en la barra de herramientas. Si esta opción está atenuada, significa que la información del producto está incompleta o que se han producido errores en las pruebas. 
1. Seleccione el vínculo "CERTIFIED AND IN THE CATALOG" (CERTIFICADO Y EN EL CATÁLOGO) para ver el dispositivo publicado en el catálogo de dispositivos.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido sobre la certificación de dispositivos IoT Plug and Play, el siguiente paso sugerido es aprender más sobre la administración de modelos de funcionalidad:

> [!div class="nextstepaction"]
> [Administración de modelos](./howto-manage-models.md)
