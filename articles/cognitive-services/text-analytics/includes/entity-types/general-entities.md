---
title: Entidades con nombre generales
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 05/13/2020
ms.author: aahi
ms.openlocfilehash: 630f04bf2cc9e7de6331f9d25754a20fe2327d76
ms.sourcegitcommit: ef69245ca06aa16775d4232b790b142b53a0c248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91779326"
---
Se devuelven las siguientes categorías de entidad al enviar solicitudes al punto de conexión `/entities/recognition/general`.

| Category   | Subcategoría | Descripción                          | Versión de modelo inicial                                                    | Notas |
|------------|-------------|--------------------------------------|-------------------------------------------------------------|--------------------------------------|
| Person     | N/D         | Nombres de personas.  | `2019-10-01`  | También lo devuelve NER v2.1 |
| PersonType | N/D         | Tipos de trabajo o roles mantenido por una persona. | `2020-02-01` | |
|Location    | N/D         | Puntos de referencia naturales y humanos, estructuras, características geográficas y entidades geopolíticas.     |  `2019-10-01` | También lo devuelve NER v2.1 |
|Location     | Entidad geopolítica (GPE)        | Ciudades, países o regiones, estados.      | `2020-02-01` | |
|Location     | Estructural                       | Estructuras hechas por el hombre. | `2020-04-01` | |
|Location     | Geográfica       | Características geográficas y naturales, como ríos, océanos y desiertos. |  `2020-04-01` | |
|Organización  | N/D | Empresas, grupos políticos, bandas musicales, clubs deportivos, organismos gubernamentales y organizaciones públicas.  | `2019-10-01` | Las nacionalidades y las religiones no se incluyen en este tipo de entidad. También lo devuelve NER v2.1 |
|Organización | Medicina | Empresas y grupos médicos. | `2020-04-01` |  |
|Organización | Bolsa de valores | Grupos de bolsa de valores. | `2020-04-01` | |
| Organización | Deportes | Organizaciones relacionadas con los deportes. | `2020-04-01` |  |
| Evento  | N/D | Eventos históricos, sociales y naturales. | `2020-02-01` |  |
| Evento  | Cultural | Eventos culturales y días festivos. | `2020-04-01` | |
| Evento  | Natural | Eventos de origen natural. | `2020-04-01` |  |
| Evento  | Deportes | Eventos deportivos.  | `2020-04-01` | |
| Producto | N/D | Objetos físicos de varias categorías. | `2020-02-01` | |
| Producto | Productos informáticos | Productos informáticos. |  `2020-02-01 ` | |
| Habilidad | N/D | Capacidad, aptitud o experiencia. | `2020-02-01` |  |
| Dirección | N/D | Dirección de correo postal completa.  | `2020-04-01` |  |
| PhoneNumber | N/D | Números de teléfono (solo números de teléfono de EE. UU y la UE). | `2019-10-01` | También lo devuelve NER v2.1 |
| Email | N/D | Direcciones de correo. | `2019-10-01` | También lo devuelve NER v2.1 |
| URL | N/D | Direcciones URL de sitios web. | `2019-10-01` | También lo devuelve NER v2.1  |
| IP | N/D | Direcciones IP de red. | `2019-10-01` | También lo devuelve NER v2.1 |
| DateTime | N/D | Fechas y horas del día. | `2019-10-01` | También lo devuelve NER v2.1 | 
| DateTime | Date | Fechas calendario. | `2019-10-01` | También lo devuelve NER v2.1 |
| DateTime | Time | Horas del día | `2019-10-01` | También lo devuelve NER v2.1 |
| DateTime | DateRange | Intervalos de fechas. | `2019-10-01` | También lo devuelve NER v2.1 |
| DateTime | TimeRange | Intervalos de horas. | `2019-10-01` | También lo devuelve NER v2.1 |
| DateTime | Duration | Duraciones. | `2019-10-01` | También lo devuelve NER v2.1 |
| DateTime | Set | Establecer varias veces repetidas. |  `2019-10-01` | También lo devuelve NER v2.1 |
| Cantidad | N/D | Números y cantidades numéricas. | `2019-10-01` | También lo devuelve NER v2.1  |
| Cantidad | Number | Números. | `2019-10-01` | También lo devuelve NER v2.1 |
| Cantidad | Porcentaje | Porcentajes.| `2019-10-01` | También lo devuelve NER v2.1 |
| Cantidad | Ordinal | Números ordinales. | `2019-10-01` | También lo devuelve NER v2.1 |
| Cantidad | Age | Edades. | `2019-10-01` |  También lo devuelve NER v2.1 |
| Cantidad | Moneda | Monedas. | `2019-10-01` | También lo devuelve NER v2.1 |
| Cantidad | Dimensión | Dimensiones y medidas. | `2019-10-01` | También lo devuelve NER v2.1 |
| Cantidad | Temperatura | Temperaturas. | `2019-10-01` | También lo devuelve NER v2.1 |
