---
title: 'Descripción de los modelos de objetos y del grafo de inteligencia espacial: Azure Digital Twins | Microsoft Docs'
description: Utilice Azure Digital Twins para crear modelos de relaciones entre personas, lugares y dispositivos.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/30/2019
ms.openlocfilehash: cafec321e7c40e27d8de731feda1103451271507
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79231388"
---
# <a name="understand-digital-twins-object-models-and-spatial-intelligence-graph"></a>Descripción de los modelos de objetos de Digital Twins y del grafo de inteligencia espacial

Azure Digital Twins es un servicio IoT de Azure que mejora las representaciones virtuales completas de entornos físicos y dispositivos, sensores y personas asociados. Mejora el desarrollo con la organización de los conceptos específicos de dominio en modelos útiles que se encuentran dentro de un grafo de inteligencia espacial. Estos conceptos modelan fielmente las relaciones y las interacciones entre personas, espacios y dispositivos.

Los modelos de objetos de Digital Twins describen conceptos específicos de dominio, categorías y propiedades. Los modelos los predefinen los usuarios, que quieren adaptar la solución a sus necesidades específicas. Combinados, estos modelos de objetos de Digital Twins predefinidos constituyen una _ontología_. La ontología de un edificio inteligente describe regiones, locales, plantas, oficinas, zonas, salas de reuniones y salas de pensar. Una cuadrícula de energía describe varias centrales eléctricas, subestaciones, recursos energéticos y clientes. Con los modelos de objetos de Digital Twins y las ontologías, se pueden personalizar las necesidades y diversos escenarios.

Con los modelos de objetos de Digital Twins y la ontología preparados, puede rellenar un _grafo espacial_. Los grafos espaciales son representaciones virtuales de las muchas relaciones entre espacios, dispositivos y personas importantes para una solución de IoT. En este diagrama se muestra un ejemplo de un grafo espacial que usa una ontología de edificio inteligente.

[![Creación de un grafo espacial con Digital Twins](media/concepts/digital-twins-spatial-graph-building.png)](media/concepts/digital-twins-spatial-graph-building.png#lightbox)

El grafo espacial reúne los espacios, los dispositivos, los sensores y los usuarios. Cada uno de ellos se vincula de forma que modele el mundo real. En este ejemplo, el lugar 43 tiene cuatro pisos, cada uno con diferentes áreas. Los usuarios se relacionan con las estaciones de trabajo y se les concede acceso a partes del grafo. Un administrador tiene derechos para realizar cambios en el grafo espacial, mientras que un visitante solo puede ver determinados datos del edificio.

## <a name="digital-twins-object-models"></a>Modelos de objetos de Digital Twins

Los modelos de objetos de Digital Twins admiten estas categorías principales de objetos:

- Los **espacios** son ubicaciones físicas o virtuales, como `Tenant`, `Customer`, `Region` y `Venue`.
- Los **dispositivos** son equipos físicos o virtuales, como `AwesomeCompany Device` y `Raspberry Pi 3`.
- Los **sensores** son objetos que detectan eventos, como `AwesomeCompany Temperature Sensor` y `AwesomeCompany Presence Sensor`.
- Los **usuarios** identifican a los ocupantes y sus características.

Otras categorías de objetos son:

- **Recursos**: conectados a un espacio, suelen representar recursos de Azure que van a utilizar los objetos del grafo espacial, como `IoTHub`.
- Los **blobs** están asociados a objetos (como espacios, dispositivos, sensores y usuarios). Se usan como archivos con tipo MIME y metadatos, como `maps`, `pictures` y `manuals`.
- **Tipos extendidos**: enumeraciones extensibles que aumentan las entidades con características específicas, como `SpaceType` y `SpaceSubtype`.
- **Ontologías**: representan un conjunto de tipos extendidos, como `Default`, `Building`, `BACnet` y `EnergyGrid`.
- **Claves y valores de propiedad**: características personalizadas de los usuarios, los dispositivos, los sensores y los espacios. Se pueden usar junto con las características integradas, por ejemplo, `DeltaProcessingRefreshTime` como clave y `10` como valor.
- **Roles**: conjuntos de permisos asignados a los usuarios y los dispositivos en el grafo espacial, como `Space Administrator`, `User Administrator` y `Device Administrator`.
- Las **asignaciones de roles** son la asociación entre un rol y un objeto en el gráfico espacial. Por ejemplo, un usuario o una entidad de servicio pueden tener permiso para administrar un espacio en el gráfico espacial.
- **Almacenes de claves de seguridad**: proporcionan las claves de seguridad para todos los dispositivos de la jerarquía bajo un objeto de espacio determinado con el fin de que el dispositivo se pueda comunicar con Digital Twins de manera segura.
- **Funciones definidas por el usuario** (UDF): permiten el procesamiento de los datos de telemetría del sensor personalizable dentro del grafo espacial. Por ejemplo, una UDF puede hacer lo siguiente:
  - Establecer un valor del sensor.
  - Realizar lógica personalizada en función de las lecturas de sensores y establecer la salida en un espacio.
  - Asociar metadatos a un espacio.
  - Enviar notificaciones cuando se cumplen las condiciones predefinidas. Actualmente, las UDF pueden escribirse en JavaScript.
- **Buscadores de coincidencias**: objetos que determinan qué UDF se ejecuta para un mensaje de telemetría determinado.
- **Puntos de conexión**: ubicaciones donde pueden enrutarse los mensajes de telemetría y los eventos de Digital Twins, como `Event Hub`, `Service Bus` y `Event Grid`.

## <a name="spatial-intelligence-graph"></a>Grafo de inteligencia espacial

Gráfico espacial: grafo jerárquico de los espacios, los dispositivos y las personas que se definen en el modelo de objetos de Digital Twins. El grafo espacial es compatible con la herencia, el filtrado, el recorrido, el escalado y la extensibilidad. Los usuarios pueden administrar el grafo espacial e interactuar con él con una colección de API REST.

Si implementa un servicio de Digital Twins en su suscripción, será el administrador global del nodo raíz. De este modo, se le concede automáticamente acceso completo a la estructura completa. Aprovisione los espacios en el grafo mediante Space API. Aprovisione los servicios mediante los sensores y la API de dispositivos mediante la API de sensor. También hay [herramientas de código abierto](https://github.com/Azure-Samples/digital-twins-samples-csharp) disponibles para aprovisionar el grafo en bloque.

**Herencia del grafo**. La herencia se aplica a los permisos y las propiedades que descienden de un nodo primario y a todos los nodos por debajo de este. Por ejemplo, cuando se asigna un rol a un usuario en un nodo determinado, el usuario tiene los permisos de ese rol para el nodo especificado y todos los nodos debajo de él. Todos los nodos debajo de ese nodo determinado heredarán las claves de propiedad y el tipo extendido definidos para él.

**Filtrado del grafo**. El filtrado se usa para restringir los resultados de la solicitud. Puede filtrar por id., nombre, tipos, subtipos, espacio primario y espacios asociados. También puede filtrar por tipos de datos de sensor, claves de propiedad y valores, *traverse*, *minLevel*, *maxLevel*, así como por otros parámetros de filtro de OData.

**Recorrido del grafo**. Puede recorrer el grafo espacial mediante su profundidad y amplitud. Recorra el grafo en profundidad de arriba a abajo o de abajo a arriba con los parámetros *traverse*, *minLevel* y *maxLevel*. Recorra el grafo en amplitud para obtener nodos secundarios directamente conectados a un espacio primario o a uno de los secundarios. Al consultar un objeto, puede obtener todos los objetos relacionados con dicho objeto mediante el parámetro *includes* de GET API.

**Escalabilidad del grafo**. Digital Twins garantiza la escalabilidad del grafo para poder ocuparse de las cargas de trabajo del mundo real. Digital Twins puede usarse para representar grandes carteras de bienes inmuebles, infraestructura, dispositivos, sensores, telemetría y mucho más.

**Extensibilidad del grafo**. Use la extensibilidad para personalizar los modelos de objetos de Digital Twins subyacentes con nuevos tipos y ontologías. Los datos de Digital Twins también se pueden enriquecer con propiedades y valores extensibles.

### <a name="spatial-intelligence-graph-management-apis"></a>API de administración del grafo con inteligencia espacial

Después de implementar Digital Twins desde [Azure Portal](https://portal.azure.com), se genera automáticamente la dirección URL del [Swagger](https://swagger.io/tools/swagger-ui/) de las API de Administración. Se muestra en Azure Portal en la sección **Introducción** con el formato siguiente.

```plaintext
https://YOUR_INSTANCE_NAME.YOUR_LOCATION.azuresmartspaces.net/management/swagger
```

| Nombre | Reemplazar por |
| --- | --- |
| YOUR_INSTANCE_NAME | El nombre de la instancia de Digital Twins. |
| YOUR_LOCATION | La región de servidor en la que está hospedada la instancia |

 El formato de dirección URL completo que aparece en esta imagen.

[![API de Administración del portal de Digital Twins](media/concepts/digital-twins-spatial-graph-management-api-url.png)](media/concepts/digital-twins-spatial-graph-management-api-url.png#lightbox)

Para más detalles sobre cómo usar los grafos de inteligencia espacial, consulte la versión preliminar de las API de Administración de Azure Digital Twins.

> [!TIP]
> Se proporciona un preestreno de Swagger para demostrar el conjunto de características de la API.
> Se hospeda en [docs.westcentralus.azuresmartspaces.net/management/swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger).

Más información sobre [cómo usar Swagger](how-to-use-swagger.md).

Todas las llamadas a API se deben autenticar mediante [OAuth](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code). Las API siguen las [convenciones de las directrices de API REST de Microsoft](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md). La mayoría de las API que devuelven colecciones admiten opciones de consulta del sistema de [OData](https://www.odata.org/getting-started/basic-tutorial/#queryData).

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre la conectividad de los dispositivos y cómo enviar mensajes de telemetría a Digital Twins, lea [Entrada de datos de telemetría y conectividad de dispositivo](concepts-device-ingress.md).

- Para más información sobre las limitaciones de la API de administración y las demás, lea el artículo sobre [limitaciones y administración de API de Azure Digital Twins](concepts-service-limits.md).
