---
title: Glosario de términos - Versión preliminar de IoT Plug and Play | Microsoft Docs
description: 'Conceptos: glosario de términos comunes relacionados con la versión preliminar de IoT Plug and Play.'
author: Philmea
ms.author: philmea
ms.date: 12/23/2019
ms.topic: conceptual
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: f0c21626c664f2d72b534ebae7f0a257620be07d
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2020
ms.locfileid: "81767068"
---
# <a name="glossary-of-terms-for-iot-plug-and-play-preview"></a>Glosario de términos de la versión preliminar de IoT Plug and Play

Definiciones de los términos comunes que se usan en los artículos de IoT Plug and Play.

## <a name="azure-certified-for-iot-portal"></a>Portal de Azure Certified for IoT

Puede usar el sitio web del [portal de Azure Certified for IoT](https://aka.ms/ACFI) para:

- Completar el [proceso de certificación](#device-certification) del [dispositivo IoT Plug and Play](#iot-plug-and-play-device).
- Buscar [modelos de funcionalidad del dispositivo](#device-capability-model).
- Publicar un modelo de funcionalidad del dispositivo en el [repositorio de modelos público](#public-model-repository).

## <a name="azure-cli"></a>Azure CLI

La CLI de Azure es una herramienta de línea de comandos multiplataforma para administrar recursos de Azure. La extensión de Azure IOT para la CLI de Azure es una herramienta de línea de comandos para interactuar con los [dispositivos IoT Plug and Play](#iot-plug-and-play-device) y probarlos. Puede usar la extensión para lo siguiente:

- Conectar con un dispositivo IoT Plug and Play.
- Ver la [telemetría](#telemetry) que envía el dispositivo.
- Trabajar con las [propiedades](#properties) del dispositivo.
- Llamar a [comandos](#commands) del dispositivo.
- Administrar [repositorios de modelos ](#model-repository), [interfaces](#interface) y [modelos de funcionalidad del dispositivo](#device-capability-model).

## <a name="azure-iot-central"></a>Azure IoT Central

Azure IoT Central es una solución de software como servicio totalmente administrada que facilita la conexión, la supervisión y la administración de sus [dispositivos IoT Plug and Play](#iot-plug-and-play-device). Puede usar los [modelos de funcionalidad del dispositivo](#device-capability-model) para configurar automáticamente una aplicación IoT Central a fin de supervisar y administrar los dispositivos.

## <a name="azure-iot-certification-service"></a>Servicio de certificación de Azure IoT

El servicio de certificación de Azure IoT ejecuta un conjunto de pruebas de certificación cuando se envía un [dispositivo IoT Plug and Play](#iot-plug-and-play-device) para su certificación a través del [portal de Azure Certified for IoT](#azure-certified-for-iot-portal). Para agregar un dispositivo al [catálogo de dispositivos Certified for IoT](#certified-for-iot-device-catalog), el dispositivo debe estar certificado.

## <a name="azure-iot-tools-extension"></a>Extensión de Azure IoT Tools

Azure IoT Tools es una colección de extensiones en [Visual Studio Code](#visual-studio-code) que le ayuda a interactuar con IOT Hub y a desarrollar dispositivos IoT. Para el desarrollo de dispositivos IoT Plug and Play, le ayuda a:

- Crear [modelos de funcionalidad del dispositivo](#device-capability-model) e [interfaces](#interface).
- Publicar en [repositorios de modelos](#model-repository).
- Generar código de esqueleto para implementar la aplicación de dispositivo.

## <a name="azure-iot-explorer-tool"></a>Herramienta Azure IoT Explorer

Azure IoT Explorer es una herramienta gráfica que puede usar para interactuar con [dispositivos IoT Plug and Play](#iot-plug-and-play-device) y probarlos. Después de instalar la herramienta en la máquina local, puede usarla para:

- Ver los dispositivos conectados a su [centro de IoT](#azure-iot-hub).
- Conectar con un dispositivo IoT Plug and Play.
- Ver la [telemetría](#telemetry) que envía el dispositivo.
- Trabajar con las [propiedades](#properties) del dispositivo.
- Llamar a [comandos](#commands) del dispositivo.

## <a name="azure-iot-hub"></a>Azure IoT Hub

IoT Hub es un servicio administrado, hospedado en la nube, que actúa como centro de mensajes para comunicaciones bidireccionales entre la aplicación de IoT y los dispositivos que administra. Los [dispositivos IoT Plug and Play](#iot-plug-and-play-device) pueden conectarse a un centro de IoT. Una solución de IoT usa un centro de IoT para habilitar:

- Dispositivos que envían telemetría a una solución basada en la nube.
- Una solución basada en la nube para administrar los dispositivos conectados.

## <a name="azure-iot-device-sdk"></a>SDK de dispositivos IoT de Azure

Hay SDK de dispositivos para varios lenguajes que puede usar para crear aplicaciones cliente de dispositivos IoT Plug and Play. Uno de los requisitos para la [certificación de dispositivos](#device-certification) es que el código de cliente del dispositivo use uno de los SDK de dispositivo IoT de Azure.

## <a name="certified-for-iot-device-catalog"></a>Catálogo de dispositivos Certified for IoT

El [catálogo de dispositivos Certified for IoT](https://catalog.azureiotsolutions.com/) enumera los [dispositivos IoT Plug and Play](#iot-plug-and-play-device) que han superado las pruebas de [certificación de dispositivos](#device-certification). Los [modelos de funcionalidad del dispositivo](#device-capability-model) para los dispositivos IoT Plug and Play del catálogo se publican en el repositorio de modelos público.

## <a name="commands"></a>Comandos:

Los comandos definidos en una [interfaz](#interface) representan los métodos que se pueden ejecutar en el [gemelo digital](#digital-twin). Por ejemplo, un comando para restablecer un dispositivo.

## <a name="common-interface"></a>Interfaz común

Se espera que todos los [dispositivos IoT Plug and Play](#iot-plug-and-play-device) implementen algunas [interfaces](#interface) comunes. Por ejemplo, la interfaz de información del dispositivo define la información del hardware y del sistema operativo del dispositivo. La [certificación del dispositivo](#device-certification) requiere que el dispositivo implemente varias interfaces comunes. Puede recuperar las definiciones de las interfaces comunes del repositorio de modelos público.

## <a name="company-model-repository"></a>Repositorio de modelos de empresa

Una organización puede usar un [repositorio de modelos](#model-repository) de empresa como almacén privado para [modelos de funcionalidad del dispositivo](#device-capability-model) e [interfaces](#interface).

## <a name="connection-string"></a>Cadena de conexión

Una cadena de conexión encapsula la información necesaria para conectarse a un punto de conexión. Normalmente, una cadena de conexión incluye la dirección de la información del punto de conexión y de seguridad, pero los formatos de la cadena de conexión varían en función de los servicios. Hay dos tipos de cadena de conexión asociadas con el servicio de IoT Hub:

- Las cadenas de conexión de dispositivo permiten que los [dispositivos IoT Plug and Play](#iot-plug-and-play-device) se conecten a puntos de conexión orientados a dispositivos de un centro de IoT. El código de cliente en un dispositivo usa la cadena de conexión para establecer una conexión segura con un centro de IoT.
- Las cadenas de conexión de IoT Hub permiten que las aplicaciones y herramientas de back-end se conecten de forma segura a puntos de conexión orientados a servicios de un centro de IoT. Estas soluciones y herramientas administran el centro de IoT y los dispositivos conectados a él.
- Las cadenas de conexión del repositorio de modelos de la empresa permiten la conexión segura de soluciones y herramientas de back-end a un [repositorio de modelos de la empresa](#company-model-repository). Estas soluciones y herramientas consumen o administran los [modelos de funcionalidad del dispositivo](#device-capability-model) y las [interfaces](#interface) en el repositorio.

## <a name="device-capability-model"></a>Modelo de funcionalidad del dispositivo

Un modelo de funcionalidad del dispositivo describe un [dispositivo IoT Plug and Play](#iot-plug-and-play-device) y define el conjunto de [interfaces](#interface) implementadas por este. Un modelo de funcionalidad del dispositivo se corresponde normalmente con un dispositivo físico, un producto o una SKU. Para definir un modelo de funcionalidad del dispositivo, se usa el [lenguaje de definición de gemelos digitales](#digital-twin-definition-language).

## <a name="device-certification"></a>Certificación de dispositivos

La certificación de dispositivos es el proceso de certificar un [dispositivo IoT Plug and Play](#iot-plug-and-play-device) para que se pueda agregar al [catálogo de dispositivos Certified for IoT](#certified-for-iot-device-catalog) y que su [modelo de funcionalidad del dispositivo](#device-capability-model) y sus [interfaces](#interface) se agreguen al [repositorio de modelos públicos](#public-model-repository).

## <a name="device-developer"></a>Desarrollador de dispositivo

Un desarrollador de dispositivos usa un [modelo de funcionalidad del dispositivo](#device-capability-model), [interfaces](#interface) y un [SDK de dispositivo IOT de Azure](#azure-iot-device-sdk) para implementar el código que se ejecutará en un [dispositivo IoT Plug and Play](#iot-plug-and-play-device).

## <a name="device-modeling"></a>Modelado de dispositivos

Un [desarrollador de dispositivos](#device-developer) usa el [lenguaje de definición de gemelos digitales](#digital-twin-definition-language) para modelar las funcionalidades de un [dispositivo IoT Plug and Play](#iot-plug-and-play-device). El modelo se puede compartir mediante un repositorio de modelos. Un desarrollador de dispositivos puede generar el código básico del dispositivo a partir del modelo. Un [desarrollador de soluciones](#solution-developer) puede configurar una solución de IOT a partir del modelo.

## <a name="device-provisioning-service"></a>Servicio Device Provisioning

[Azure IoT Central](#azure-iot-central) usa el servicio Device Provisioning para administrar el registro y la conexión de todos los dispositivos. Para obtener más información, consulte [Conectividad de dispositivos en Azure IoT Central](../iot-central/core/concepts-get-connected.md). También puede usar el servicio Device Provisioning para administrar el registro y la conexión de dispositivos a la solución de IoT basada en IoT Hub. Para obtener más información, consulte [Aprovisionamiento de dispositivos con el servicio Azure IoT Hub Device Provisioning](../iot-dps/about-iot-dps.md).

## <a name="device-registration"></a>Registro de dispositivos

Para que un [dispositivo IoT Plug and Play](#iot-plug-and-play-device) pueda conectarse a una solución de IoT, debe registrarse con la solución. [Azure IoT Central](#azure-iot-central) usa el [servicio Device Provisioning](#device-provisioning-service) para administrar el registro de los dispositivos. En una solución de IoT personalizada, puede registrar dispositivos con su centro de IoT en Azure Portal o mediante programación.

## <a name="device-first"></a>Dispositivo inicial

[Azure IoT Central](#azure-iot-central) admite un escenario de conexión y registro del dispositivo inicial. En este escenario, un [dispositivo IoT Plug and Play](#iot-plug-and-play-device) puede conectarse a una aplicación IoT Central sin registrarse con anterioridad. El registro se produce automáticamente cuando un dispositivo se conecta por primera vez a la aplicación.

## <a name="digital-twin"></a>Gemelo digital

Un gemelo digital es un modelo de [dispositivo IoT Plug and Play](#iot-plug-and-play-device). Un gemelo digital se modela mediante el [lenguaje de definición de gemelos digitales (DTDL)](#digital-twin-definition-language). Puede usar los [SDK de dispositivo IoT de Azure](#azure-iot-device-sdk) para interactuar con los gemelos digitales en tiempo de ejecución. Por ejemplo, puede establecer un valor de propiedad en un gemelo digital en un dispositivo y el SDK comunica este cambio a su solución de IoT en la nube.

## <a name="digital-twin-change-events"></a>Eventos de cambio de gemelo digital

Cuando un [dispositivo IoT Plug and Play](#iot-plug-and-play-device) se conecta a un [centro de IoT](#azure-iot-hub), el centro puede usar su capacidad de enrutamiento para enviar notificaciones de cambios de gemelos digitales. Por ejemplo, cada vez que un valor de [propiedad](#properties) cambia en un dispositivo, IoT Hub puede enviar una notificación a un punto de conexión, como una cola de Service Bus.

## <a name="digital-twin-definition-language"></a>Lenguaje de definición de gemelos digitales

Lenguaje que sirve para describir los modelos y las interfaces de los [dispositivos IoT Plug and Play](#iot-plug-and-play-device). Use el [lenguaje de definición de gemelos digitales](https://aka.ms/DTDL) para describir las funcionalidades de un [gemelo digital](#digital-twin) y para habilitar la plataforma IoT y las soluciones IoT con el fin de aprovechar la semántica de la entidad.

## <a name="digital-twin-route"></a>Enrutamiento de gemelo digital

Una ruta configurada en un [centro de IoT](#azure-iot-hub) para ofrecer [eventos de cambio de gemelo digital](#digital-twin-change-events) a un punto de conexión, como una cola de Service Bus.

## <a name="interface"></a>Interfaz

Una interfaz describe las funcionalidades relacionadas que un [dispositivo IoT Plug and Play](#iot-plug-and-play-device) o un [gemelo digital](#digital-twin) implementa. Puede reutilizar las interfaces en distintos [modelos de funcionalidad del dispositivo](#device-capability-model).

## <a name="iot-hub-query-language"></a>Lenguaje de consulta de IoT Hub

El lenguaje de consulta de IoT Hub se usa para varios propósitos. Por ejemplo, puede usar el lenguaje para buscar [dispositivos registrados](#device-registration) en su centro de IoT o para refinar el comportamiento de [enrutamiento del gemelo digital](#digital-twin-route).

## <a name="iot-plug-and-play-device"></a>Dispositivo IoT Plug and Play

Un dispositivo IoT Plug and Play suele ser un dispositivo informático independiente y a pequeña escala que recopila datos o controla otros dispositivos, y que ejecuta software o firmware que implementa un [modelo de funcionalidad del dispositivo](#device-capability-model).  Por ejemplo, un dispositivo IoT Plug and Play podría ser un dispositivo de supervisión ambiental o un controlador de un sistema de riego de agricultura inteligente. Puede escribir una solución de IoT hospedada en la nube para imponer, controlar y recibir datos de dispositivos IoT Plug and Play. El [catálogo de dispositivos Azure Certified for IoT](#certified-for-iot-device-catalog) enumera dispositivos IoT Plug and Play disponibles. Todos los dispositivos IoT Plug and Play del catálogo están validados y tienen un [modelo de funcionalidad del dispositivo](#device-capability-model).

## <a name="microsoft-partner-center"></a>Centro de partners de Microsoft

El [centro de partners de Microsoft](https://docs.microsoft.com/partner-center/) es donde su organización administra su relación de un extremo a otro con Microsoft. Necesitará una cuenta del centro de partners de Microsoft para poder certificar el [dispositivo IoT Plug and Play](#iot-plug-and-play-device) en el [portal de Azure Certified for IoT](#azure-certified-for-iot-portal).

## <a name="model-discovery"></a>Detección de modelo

Cuando un [dispositivo IoT Plug and Play](#iot-plug-and-play-device) se conecta a una solución de IoT, la solución puede detectar las funcionalidades del dispositivo mediante la búsqueda del [modelo de funcionalidad del dispositivo](#device-capability-model). Un dispositivo puede enviar su modelo de funcionalidad a la solución, o la solución puede buscar un modelo de funcionalidad del dispositivo en un [repositorio de modelos](#model-repository).

## <a name="model-repository"></a>Repositorio de modelos

En el repositorio de modelos se almacenan las [interfaces](#device-capability-model) y los [modelos de funcionalidad del dispositivo](#interface). Hay un solo [repositorio de modelos públicos](#public-model-repository). Las organizaciones pueden crear sus propios repositorios de modelos organizativos.

## <a name="model-repository-rest-api"></a>API REST del repositorio de modelos

Una API para administrar e interactuar con los repositorios de modelos. Por ejemplo, puede usar la API para agregar [modelos de funcionalidad del dispositivo](#device-capability-model) y buscar modelos de funcionalidad.

## <a name="properties"></a>Propiedades

Las propiedades son campos de datos definidos en una [interfaz](#interface) que representan un estado de un gemelo digital. Las propiedades se pueden declarar como de solo lectura o de escritura. Las propiedades de solo lectura, como el número de serie, se establecen mediante código que se ejecuta en el propio [dispositivo IoT Plug and Play](#iot-plug-and-play-device).  Las propiedades que se pueden escribir, como un umbral de alarma, se establecen normalmente a partir de la solución de IoT basada en la nube.

## <a name="public-model-repository"></a>Repositorio de modelos público

Hay un único repositorio de modelos público donde se almacenan las [interfaces](#device-capability-model) y los [modelos de funcionalidad del dispositivo](#interface) de los [dispositivos certificados](#device-certification). El repositorio de modelos público almacena también las definiciones de las [interfaces comunes](#common-interface).

## <a name="registration-id"></a>Identificador de registro

Un identificador de registro identifica de forma única un dispositivo en el [servicio Device Provisioning](#device-provisioning-service). Este identificador no es el mismo que el identificador de dispositivo que es un identificador único para un dispositivo en un [centro de IoT](#azure-iot-hub).

## <a name="scope-id"></a>Identificador de ámbito

El identificador. de ámbito identifica de forma única una instancia del [servicio Device Provisioning](#device-provisioning-service).

## <a name="shared-access-signature"></a>Firma de acceso compartido

Las firmas de acceso compartido son un mecanismo de autenticación basado en hash seguros SHA-256 o URI. La autenticación de firma de acceso compartido tiene dos componentes: una directiva de acceso compartido y una firma de acceso compartido (a menudo denominada token). Un [dispositivo IoT Plug and Play](#iot-plug-and-play-device) usa una firma de acceso compartido para autenticarse con un [centro de IoT](#azure-iot-hub).

## <a name="solution-developer"></a>Desarrollador de soluciones

Un desarrollador de soluciones crea el back-end de la solución. Normalmente, un desarrollador de soluciones trabaja con recursos de Azure, como [IoT Hub](#azure-iot-hub) y [repositorios de modelos](#model-repository), o funciona con [IoT Central](#azure-iot-central).

## <a name="telemetry"></a>Telemetría

Los campos de telemetría definidos en una [interfaz](#interface) representan medidas. Estas medidas suelen ser valores como lecturas de sensores que envía el [dispositivo IoT Plug and Play](#iot-plug-and-play-device) como un flujo de datos.

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code es un moderno editor de código disponible para varias plataformas. Las extensiones, como las del paquete [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools), le permiten personalizar el editor para admitir una amplia gama de escenarios de desarrollo.
