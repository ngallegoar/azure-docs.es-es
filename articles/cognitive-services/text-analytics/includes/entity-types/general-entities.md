---
title: Entidades con nombre generales
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 02/06/2020
ms.author: aahi
ms.openlocfilehash: ba516a548fc8385ca86526a7f2dd082b27e53208
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77211389"
---
## <a name="general-entity-types"></a>Tipos de entidades generales:

### <a name="person"></a>Person

Reconocimiento de nombres de persona en el texto.

Lenguajes:
* Versión preliminar pública: `Arabic`, `Czech`, `Chinese-Simplified`, `Danish`, `Dutch`, `English`, `Finnish`, `French`, `German`, `Hungarian`, `Italian`, `Japanese`, `Korean`, `Norwegian (Bokmål)`, `Polish`, `Portuguese (Portugal)`, `Portuguese (Brazil)`, `Russian`, `Spanish`, `Swedish` y `Turkish`

| Nombre del subtipo | Descripción                                                      | Disponible a partir de la versión del modelo. |
|--------------|------------------------------------------------------------------|---------------------------------------|
| N/D          | Nombres de persona reconocidos, como `Bill Gates` o `Marie Curie` | `2019-10-01`                          | 

### <a name="persontype"></a>PersonType
Tipo de trabajo o rol mantenido por una persona.

Lenguajes:
* Versión preliminar pública: `English`

| Nombre del subtipo | Descripción                                                                                | Disponible a partir de la versión del modelo. |
|--------------|--------------------------------------------------------------------------------------------|----------------------------------------|
| N/D          | Tipos de trabajo, por ejemplo `civil engineer`, `salesperson`, `chef`, `librarian`, `nursing aide` | `2020-02-01`                           |

### <a name="location"></a>Location

Puntos de referencia naturales y humanos, estructuras, características geográficas y entidades geopolíticas.

Lenguajes:

* Versión preliminar pública: `Arabic`, `Czech`, `Chinese-Simplified`, `Danish`, `Dutch`, `English`, `Finnish`, `French`, `German`, `Hungarian`, `Italian`, `Japanese`, `Korean`, `Norwegian (Bokmål)`, `Polish`, `Portuguese (Portugal)`, `Portuguese (Brazil)`, `Russian`, `Spanish`, `Swedish` y `Turkish`

| Nombre del subtipo              | Descripción                                                                              | Disponible a partir de la versión del modelo. |
|---------------------------|------------------------------------------------------------------------------------------|----------------------------------------|
| N/D                       | Organizaciones, como `Atlantic Ocean`, `library`, `Eiffel Tower` o `Statue of Liberty`  | `2019-10-01`                           |
| Entidad geopolítica (GPE): solo inglés| Ciudades, países, estados, por ejemplo `Seattle`, `Pennsylvania`, `South Africa`, `Tokyo` | `2020-02-01`                           |

### <a name="organization"></a>Organización  

Organizaciones, corporaciones, agencias y otros grupos de personas reconocidos. Por ejemplo: empresas, grupos políticos, bandas musicales, clubs deportivos, organismos gubernamentales y organizaciones públicas. Las nacionalidades y las religiones no se incluyen en este tipo de entidad. 

Lenguajes: 

* Versión preliminar pública: `Arabic`, `Czech`, `Chinese-Simplified`, `Danish`, `Dutch`, `English`, `Finnish`, `French`, `German`, `Hungarian`, `Italian`, `Japanese`, `Korean`, `Norwegian (Bokmål)`, `Polish`, `Portuguese (Portugal)`, `Portuguese (Brazil)`, `Russian`, `Spanish`, `Swedish` y `Turkish`

| Nombre del subtipo | Descripción                                                                                             | Disponible a partir de la versión del modelo. |
|--------------|---------------------------------------------------------------------------------------------------------|----------------------------------------|
| N/D          | Organizaciones, como `Microsoft`, `NASA`, `National Oceanic and Atmospheric Administration`,`VOA` | `2019-10-01`                           |

### <a name="event"></a>Evento  

Eventos históricos, sociales y naturales.  

Lenguajes: 

* Versión preliminar pública: `English`

| Nombre del subtipo | Descripción                                                            | Disponible a partir de la versión del modelo. |
|--------------|------------------------------------------------------------------------|----------------------------------------|
| N/D          | Eventos como `wedding`, `hurricane`, `car accident`, `solar eclipse`, `American Revolution` | `2020-02-01`                           |

### <a name="product"></a>Producto  

Objetos físicos de varias categorías.  

Lenguajes: 

* Versión preliminar pública: `English`

| Nombre del subtipo | Descripción                                                                        | Disponible a partir de la versión del modelo. |
|--------------|------------------------------------------------------------------------------------|----------------------------------------|
| N/D          | Por ejemplo, `Microsoft Surface laptop`, `sunglasses`, `motorcycle`, `bag`, `Xbox` | `2020-02-01`                           |
| Procesamiento    | `Azure Cosmos DB`, `Azure Kubernetes Service`                                     | `2020-02-01`                           |

### <a name="skill"></a>Habilidad  

Una entidad que describe una funcionalidad o experiencia.  

Lenguajes: 

* Versión preliminar pública: `English`

| Nombre del subtipo | Descripción                                                                 | Disponible a partir de la versión del modelo. |
|--------------|-----------------------------------------------------------------------------|----------------------------------------|
| N/D          | `nursing`, `data mining`, `linguistics`, `critical thinking`, `photography` | `2020-02-01`                           |

### <a name="phone-number"></a>Número de teléfono

Números de teléfono (solo números de teléfono de EE. UU.). 

Lenguajes:

* Versión preliminar pública: `English`

| Nombre del subtipo | Descripción                                    | Disponible a partir de la versión del modelo. |
|--------------|------------------------------------------------|----------------------------------------|
| N/D          | Números de teléfono de EE. UU., como `(312) 555-0176`. | `2019-10-01`                           |

### <a name="email"></a>Email

Dirección de correo electrónico. 

Lenguajes:

* Versión preliminar pública: `English`

| Nombre del subtipo | Descripción                                      | Disponible a partir de la versión del modelo. |
|--------------|--------------------------------------------------|----------------------------------------|
| N/D          | Dirección de correo electrónico, por ejemplo `support@contoso.com` | `2019-10-01`                           |

### <a name="url"></a>URL

Direcciones URL de Internet.

Lenguajes:

* Versión preliminar pública: `English`

| Nombre del subtipo | Descripción                                          | Disponible a partir de la versión del modelo. |
|--------------|------------------------------------------------------|----------------------------------------|
| N/D          | Direcciones URL a sitios web, como `https://www.bing.com`. | `2019-10-01`                           |

### <a name="ip-address"></a>Dirección IP

Dirección de protocolo de Internet

Lenguajes:

* Versión preliminar pública: `English`

| Nombre del subtipo | Descripción                              | Disponible a partir de la versión del modelo. |
|--------------|------------------------------------------|----------------------------------------|
| N/D          | Dirección de red, como `10.0.0.101` | `2019-10-01`                           |

###  <a name="datetime"></a>DateTime

Entidades de fecha y hora. 

* Disponible a partir de la versión del modelo `2019-10-01`

Lenguajes:

* Versión preliminar pública: `Chinese-Simplified`, `English`, `French`, `German` y `Spanish`

| Nombre del subtipo    | Ejemplos                     |
|-------------|------------------------------|
| N/D         | `6:30PM February 4, 2012`, `4/1/2011 2:45`                   |
| Date  | `May 2nd, 2017`, `05/02/2017`       |
| Time     | `8:15`, `6AM`              |
| DateRange    | `August 2nd to August 5th`         |
| TimeRange   | `4-6PM`, `10:00AM to Noon`          |
| Duration | `2.5 minutes`, `one and a half hours`         |
| Set | `every Saturday`         |

###  <a name="quantity"></a>Cantidad

Números y cantidades numéricas. 

* Disponible a partir de la versión del modelo `2019-10-01`

Lenguajes:

* Versión preliminar pública: `Chinese-Simplified`, `English`, `French`, `German` y `Spanish`

| Nombre del subtipo    | Ejemplos                     |
|-------------|------------------------------|
| Number         | `6`, `six`                   |
| Porcentaje  | `50%`, `fifty percent`       |
| Ordinal     | `2nd`, `second`              |
| Age         | `90 day old`, `30 years old` |
| Moneda    | `$10.99`, `€30.00`           |
| Dimensión   | `10 miles`, `40 cm`          |
| Temperatura | `32 degrees`, `10°C`         |
