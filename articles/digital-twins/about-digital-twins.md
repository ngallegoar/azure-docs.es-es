---
title: 'Información general: Azure Digital Twins | Microsoft Docs'
description: Conozca Azure Digital Twins, una solución de Azure IoT de inteligencia espacial.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.date: 12/30/2019
ms.topic: overview
ms.service: digital-twins
services: digital-twins
ms.custom: mvc
ms.openlocfilehash: 91386eaf7753f2aefada2d1423a11b366c3ee924
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "79370906"
---
# <a name="overview-of-azure-digital-twins-preview"></a>Introducción a la versión preliminar de Azure Digital Twins

[!INCLUDE [digital-twins-preview-limit-alert](../../includes/digital-twins-preview-limit-alert.md)]

La versión preliminar de Azure Digital Twins es un servicio de Azure IoT que crea modelos completos del entorno físico. Puede crear grafos de inteligencia espacial para modelar las relaciones y las interacciones entre personas, espacios y dispositivos.

Con Azure Digital Twins puede consultar datos de un espacio físico en lugar de hacerlo de muchos sensores dispares. Este servicio le ayuda a crear experiencias reutilizables, muy escalables y conscientes del espacio que vinculan la transmisión de los datos entre el mundo físico y el digital. Las aplicaciones están mejoradas con estas características contextuales apropiadas de forma exclusiva. 

Azure Digital Twins se aplica a todo tipo de entornos, como almacenes, oficinas, colegios, hospitales y bancos. Incluso se puede utilizar para estadios, fábricas, estacionamientos, parques, cuadrículas inteligentes y ciudades. Estos son algunos escenarios donde Azure Digital Twins puede resultar útil:

- Predecir las necesidades de mantenimiento de una fábrica.
- Analizar los requisitos de energía en tiempo real para una red eléctrica.
- Optimizar el uso del espacio disponible en una oficina.
- Realizar un seguimiento de la temperatura diaria en varios Estados.
- Supervisar rutas de drones en uso.
- Identificar vehículos autónomos.
- Analizar los niveles de ocupación de un edificio.
- Encontrar la máquina registradora más ocupada del almacén.

Sea cual sea su escenario empresarial real, es probable que se pueda aprovisionar un caso digital correspondiente mediante Azure Digital Twins.

El siguiente vídeo echa un vistazo más de cerca a Azure Digital Twins.

> [!VIDEO https://www.youtube.com/embed/TvN_NxpgyzQ]

## <a name="key-capabilities"></a>Principales capacidades

Azure Digital Twins tiene las siguientes funcionalidades principales.

### <a name="spatial-intelligence-graph"></a>Grafo de inteligencia espacial

El [*grafo de inteligencia espacial*](./concepts-objectmodel-spatialgraph.md#spatial-intelligence-graph) o *grafo espacial* es una representación virtual del entorno físico. Lo puede utilizar para modelar las relaciones entre personas, lugares y dispositivos.

Piense en una aplicación de servicios públicos inteligentes que utiliza varios medidores del uso de la electricidad en un barrio. La empresa de servicios públicos inteligentes debe supervisar y predecir con precisión el uso y la facturación de la electricidad. Cada dispositivo y cada sensor se deben modelar con contexto sobre la ubicación y el cliente al que se va a facturar. Puede usar el grafo de inteligencia espacial para modelar estos tipos de relaciones complejas.

### <a name="digital-twin-object-models"></a>Modelos de objetos gemelos digitales

Los [modelos de objetos de Digital Twins](./concepts-objectmodel-spatialgraph.md#digital-twins-object-models) son protocolos de dispositivo y esquemas de datos predefinidos. Alinean las necesidades específicas del dominio de la solución para acelerar y simplificar el desarrollo.

Un ejemplo sería una aplicación de ocupación de habitaciones que podría usar tipos de espacios predefinidos, como campus, edificio, piso y habitación.

### <a name="multiple-and-nested-tenants"></a>Inquilinos múltiples y anidados

Puede crear soluciones que se escalan de forma segura y que se pueden reutilizar para varios inquilinos. También puede crear varios subinquilinos cuyo acceso y uso se puede realizar de forma aislada y segura.

Un ejemplo es una aplicación de uso de espacio que está configurada para aislar los datos de un inquilino de los datos de otros inquilinos dentro de un solo edificio. O bien, la aplicación se usa para combinar datos de un único inquilino con varios edificios.

### <a name="advanced-compute-capabilities"></a>Funcionalidades de proceso avanzadas

Con las [funciones definidas por el usuario](./concepts-user-defined-functions.md), puede definir y ejecutar funciones personalizadas con los [datos de dispositivo](./concepts-device-ingress.md) de entrada para enviar señales a puntos de conexión predefinidos. Esta funcionalidad avanzada mejora la personalización y la automatización de tareas de dispositivos.

Un ejemplo sería una aplicación de agricultura inteligente que incluye una función definida por el usuario para evaluar las lecturas del sensor de humedad del suelo y la previsión meteorológica. La aplicación, a continuación, envía señales acerca de las necesidades de riego.

### <a name="built-in-access-control"></a>Control de acceso integrado

Mediante el uso de las características de administración de acceso e identidad, como el [control de acceso basado en rol](./security-role-based-access-control.md) y [Azure Active Directory](./security-authenticating-apis.md), puede controlar de forma segura el acceso de los individuos y los dispositivos.

Un ejemplo es una aplicación de administración de instalaciones que está configurada para permitir que los ocupantes de una habitación puedan fijar la temperatura dentro de un intervalo especificado. Los administradores de las instalaciones pueden fijar la temperatura de cualquier habitación en cualquier valor.

### <a name="ecosystem"></a>Ecosistema

Puede conectar una instancia de Azure Digital Twins a muchos de los eficaces servicios de Azure. Estos servicios incluyen Azure Stream Analytics, Azure AI y Azure Storage. También incluyen Azure Maps, Microsoft Mixed Reality, Office 365 o Dynamics 365.

Un ejemplo sería una aplicación de edificios de oficinas inteligentes que usa Azure Digital Twins para representar los equipos y dispositivos que se encuentran en varias plantas. A medida que los dispositivos transmiten los datos en vivo a la instancia de Digital Twin aprovisionada, Stream Analytics procesa esos datos para proporcionar detalles clave a partir de los cuales poder actuar. Los datos se almacenan en Azure Storage y se convierten a un formato de archivo que se puede compartir. El archivo se distribuye en toda la organización mediante el uso de Office 365.

## <a name="solutions-that-benefit-from-azure-digital-twins"></a>Soluciones que se benefician de Azure Digital Twins

Azure Digital Twins es útil para representar el mundo físico y sus muchas relaciones. Simplifica el modelado, el procesamiento de datos, el control de eventos y el seguimiento de dispositivos IoT. Piense solo en algunos de los siguientes escenarios de varios sectores industriales. Se benefician de su uso para:

* Mostrar a una empresa de administración de propiedades los niveles de ocupación de un espacio a lo largo del tiempo para obtener conclusiones sobre las mejores maneras de configurar su edificio de oficinas.
* Desencadenar vales de órdenes de trabajo para una aplicación móvil. Puede usarlo para enviar guardas de seguridad y programar servicios de conserjería y otros servicios en un espacio de comercio minorista o una instalación deportiva.
* Mostrar al ocupante de un edificio qué habitaciones están ocupadas en un edificio en tiempo real. A continuación, ayudar al ocupante a reservar espacios de trabajo que se ajusten a sus necesidades.
* Realizar un seguimiento del lugar donde se encuentran los recursos dentro de un espacio.
* Optimizar la carga de vehículos eléctricos mediante el modelado de las preferencias del usuario y las restricciones de la red eléctrica.

## <a name="azure-digital-twins-in-the-context-of-other-iot-services"></a>Azure Digital Twins en el contexto de otros servicios de IoT

Azure Digital Twins usa Azure IoT Hub para conectar los dispositivos y sensores IoT que mantienen todo actualizado con el mundo físico. En el diagrama siguiente se muestra cómo se relaciona Azure Digital Twins con otros servicios de IoT de Azure.

[![Azure Digital Twins es un servicio basado en Azure IoT Hub](media/overview/azure-digital-twins-in-iot-ecosystem.png)](media/overview/azure-digital-twins-in-iot-ecosystem.png#lightbox).

Para más información acerca de IoT, consulte [Tecnologías y soluciones IoT de Azure](../iot-fundamentals/iot-services-and-technologies.md).

## <a name="next-steps"></a>Pasos siguientes

Vaya a una breve demostración sobre Azure Digital Twins:

>[!div class="nextstepaction"]
>[Inicio rápido: Búsqueda de salas disponibles mediante Azure Digital Twins](./quickstart-view-occupancy-dotnet.md)

Profundice en una aplicación de administración de instalaciones mediante Azure Digital Twins:

>[!div class="nextstepaction"]
>[Tutorial: Implementación de Azure Digital Twins y configuración de un grafo espacial](./tutorial-facilities-setup.md)

Conozca los conceptos básicos de Azure Digital Twins:

>[!div class="nextstepaction"]
>[Modelo de objetos de Digital Twins y grafo de inteligencia espacial](./concepts-objectmodel-spatialgraph.md)