---
title: Programa Azure Certified Device | Microsoft Docs
description: Descripción del programa Azure Certified Device.
author: koichih
ms.author: koichih
ms.date: 09/25/2020
ms.topic: conceptual
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 1fcfc7a9e632e5db1fb809dba7a938c8641c9ddc
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92048107"
---
# <a name="what-is-the-azure-certified-device-program"></a>¿Qué es el programa Azure Certified Device?

El programa Azure Certified Device asegura que las soluciones de cliente funcionen sin problemas con los servicios de Azure. El programa usa herramientas, servicios y un marketplace para compartir los procedimientos recomendados y los conocimientos del sector con una comunidad de generadores de soluciones y dispositivos.

Este programa está diseñado para lo siguiente:

- **Brindar confianza a los clientes:** los clientes pueden comprar con confianza dispositivos certificados por Microsoft para usarlos con los servicios de Azure.

- **Ayudar a los clientes a encontrar los dispositivos adecuados para su solución:** los generadores de dispositivos pueden publicar las funcionalidades únicas de sus dispositivos como parte del proceso de certificación. Los clientes pueden encontrar fácilmente los productos que se ajustan a sus necesidades.

- **Promover dispositivos certificados:** los generadores de dispositivos obtienen mayor visibilidad, contacto con los clientes y el uso de la marca Azure Certified Device.

En este artículo se describe cómo:

- Incorporar su empresa al programa Azure Certified Device.
- Determinar qué certificación se aplica a su dispositivo.
- Buscar los requisitos del programa y otros recursos para que empiece con las pruebas.
- Usar el portal de Azure Certified Device para validar su dispositivo.

## <a name="onboarding"></a>Incorporación

Para certificar dispositivos en el [portal de Azure Certified Device](https://aka.ms/acdp), debe completar estos pasos:

1. Asegúrese de que su empresa tiene una cuenta de Azure Active Directory con un inquilino profesional o educativo.
2. Con su cuenta, únase a [Microsoft Partner Network (MPN)](https://partner.microsoft.com/).
3. Inicie sesión en el [portal de Azure Certified Device](https://aka.ms/acdp) después de unirse a MPN.
4. Revise y firme el [contrato del programa](https://aka.ms/acdagreement) en la página de perfil de la empresa.

### <a name="company-profile"></a>Perfil de empresa

Para administrar el perfil de su empresa en el portal de Azure Certified Device, seleccione **Perfil de empresa**. El perfil de empresa incluye la dirección URL de la empresa, la dirección de correo electrónico y el logotipo. Acepte el contrato del programa en esta página antes de continuar con cualquier operación de certificación.

La página de descripción del dispositivo en el catálogo de Azure Certified Device usa la información del perfil de la empresa.

## <a name="choose-the-certification"></a>Elección de la certificación

Hay tres certificaciones distintas, cada una de las cuales centrada en proporcionar un valor de cliente diferente. En función del tipo de dispositivo que esté generando y del público de destino, puede elegir la certificación o certificaciones que sean más aplicables:

### <a name="azure-certified-device"></a>Azure Certified Device

La _certificación Azure Certified Device_ valida que un dispositivo se puede conectar con Azure IoT Hub y aprovisionar de manera segura a través de Device Provisioning Service (DPS). Esta certificación refleja la funcionalidad y la interoperabilidad de un dispositivo, que es una base de referencia necesaria para certificaciones más avanzadas.

- Para más información, consulte los [requisitos de certificación](https://aka.ms/acdrequirements).
- Para más información sobre cómo usar DPS para conectar un dispositivo a Azure IoT Hub, consulte la [información general sobre el aprovisionamiento de dispositivos](../iot-dps/about-iot-dps.md).

### <a name="iot-plug-and-play"></a>IoT Plug and Play

La _certificación IoT Plug and Play_, una certificación incremental para la certificación Azure Certified Device, simplifica el proceso de generar dispositivos sin código de dispositivo personalizado. IoT Plug and Play permite que los asociados de hardware generen dispositivos que se integren fácilmente con soluciones en la nube basadas en Azure IoT Central y soluciones de terceros.

- Para más información, consulte los [requisitos de certificación](https://aka.ms/acdiotpnprequirements).
- Para más información sobre cómo preparar un dispositivo para las pruebas de IoT Plug and Play, consulte [Certificación de dispositivos IoT Plug and Play](howto-certify-device.md).

### <a name="edge-managed"></a>Edge Managed

La _certificación Edge Managed_, una certificación incremental para la certificación Azure Certified Device, se centra en los estándares de administración de dispositivos para dispositivos Azure IoT que ejecutan Windows, Linux o RTOS. Actualmente, esta certificación de programa se centra en la compatibilidad del entorno de ejecución de Edge para la implementación y administración de módulos.

> [!TIP]
> Anteriormente, este programa se conocía como el _programa de certificación IoT Edge_.

- Para más información, consulte los [requisitos de certificación](https://aka.ms/acdedgemanagedrequirements).
- Para más información sobre IoT Edge, consulte la [introducción a IoT Edge](../iot-edge/about-iot-edge.md).
- Para más información sobre los sistemas operativos y los contenedores compatibles, consulte [Sistemas compatibles con Azure IoT Edge](../iot-edge/support.md).

## <a name="use-the-azure-certified-device-portal"></a>Uso del portal de Azure Certified Device

En esta sección se resume cómo usar el [portal de Azure Certified Device](https://certify.azure.com). Para más información, consulte la [guía de introducción al portal](https://aka.ms/acdhelp).

Para certificar un dispositivo en el programa Azure Certified Device, complete los cuatro pasos siguientes:

1. Proporcionar los detalles del sistema operativo
2. Probar el dispositivo
3. Enviar y completar la revisión
4. Publicar en el catálogo de Azure Certified Device (opcional)

### <a name="provide-device-details"></a>Proporcionar los detalles del servicio

Para cada dispositivo que quiera certificar, use los formularios del portal de certificación para registrar detalles sobre el hardware del dispositivo, las instrucciones de configuración y el material de marketing:

- **Información del dispositivo:** recopila información sobre el dispositivo, como su nombre, descripción, detalles de hardware y sistema operativo.
- **Guía de introducción**: documento PDF que un cliente puede usar para utilizar rápidamente el producto. Hay disponibles [plantillas de ejemplo](https://aka.ms/GSTemplate).
- **Detalles de marketing:** proporcione información de marketing lista para el cliente sobre el dispositivo, como una imagen o información del distribuidor.
- **Certificaciones adicionales del sector:** sección opcional que le permite proporcionar información sobre cualquier otra certificación que tenga el dispositivo.

### <a name="test-the-device"></a>Probar el dispositivo

Esta fase interactúa con el dispositivo y ejecuta una serie de pruebas después de que el dispositivo usa DPS para conectarse a IoT Hub. Al finalizar, puede ver un conjunto de archivos de registro con los resultados de las pruebas del dispositivo.

En el portal de certificación encontrará instrucciones sobre cómo conectarse a la instancia de IoT Hub que se usa para las pruebas. Puede establecer la conexión de DPS a través de cualquiera de los [métodos de atestación compatibles](../iot-dps/concepts-service.md#attestation-mechanism).

El equipo de Azure Certified Device puede ponerse en contacto con el generador del dispositivo para realizar una validación manual adicional del dispositivo.

### <a name="submit-and-publish"></a>Envío y publicación

La fase final requerida es enviar el proyecto para su revisión. Este paso notifica a un miembro del equipo Azure Certified Device que revise el proyecto para comprobar su integridad, incluidos los detalles del dispositivo y de marketing, y la guía de introducción. Es posible que un miembro del equipo se ponga en contacto con usted en la dirección de correo electrónico de la empresa que se proporcionó anteriormente con preguntas o solicitudes de edición antes de la aprobación.

Si el dispositivo requiere una validación manual adicional como parte de la certificación, recibirá un aviso en este momento.

Cuando un dispositivo está certificado, puede elegir publicar los detalles del producto en el catálogo de Azure Certified Device mediante la característica **Publicar en el catálogo** de la página Resumen del producto.

## <a name="if-you-have-questions"></a>En caso de preguntas

Póngase en contacto con el equipo en [iotcert@microsoft.com](mailto:iotcert@microsoft.com?subject=Azure%20Certified%20Device%20question) si tiene preguntas sobre los programas de certificación, el portal de certificación o el catálogo de Azure Certified Device.

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene una visión general del programa Azure Certified Device, el paso siguiente que se sugiere es aprender a [certificar dispositivos IoT Plug and Play](howto-certify-device.md).