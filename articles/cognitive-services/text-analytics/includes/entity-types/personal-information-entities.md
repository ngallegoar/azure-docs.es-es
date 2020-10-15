---
title: Entidades con nombre de información personal
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 07/16/2020
ms.author: aahi
ms.openlocfilehash: eedfe4f24797a9b564479b2f8d3f4d04b0751272
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91779760"
---
> [!NOTE]
> Para detectar información de salud protegida (PHI), use el parámetro `domain=phi` y la versión del modelo `2020-04-01` o posterior.
>
> Por ejemplo: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.2/entities/recognition/pii?domain=phi&model-version=2020-07-01`
 
Se devuelven las siguientes categorías de entidad al enviar solicitudes al punto de conexión `/v3.1-preview.2/entities/recognition/pii`.

| Category   | Subcategoría | Descripción                          | Versión de modelo inicial | Notas |
|------------|-------------|--------------------------------------|------------------------|---|
| Person     | N/D         | Nombres de personas.  | `2019-10-01`  | También se devuelve con `domain=phi`. |
| PersonType | N/D         | Tipos de trabajo o roles mantenido por una persona. | `2020-02-01` | |
| PhoneNumber | N/D | Números de teléfono (solo números de teléfono de EE. UU y la UE). | `2019-10-01` | También se devuelve con `domain=phi`. |
|Organización  | N/D | Empresas, grupos políticos, bandas musicales, clubs deportivos, organismos gubernamentales y organizaciones públicas.  | `2019-10-01` | Las nacionalidades y las religiones no se incluyen en este tipo de entidad.  |
|Organización | Medicina | Empresas y grupos médicos. | `2020-04-01` |  |
|Organización | Bolsa de valores | Grupos de bolsa de valores. | `2020-04-01` |  |
| Organización | Deportes | Organizaciones relacionadas con los deportes. | `2020-04-01` |  |
| Dirección | N/D | Dirección de correo postal completa.  | `2020-04-01` | También se devuelve con `domain=phi`. |
| Coordenadas GPS de la UE | N/D | Coordenadas GPS de ubicaciones de la Unión Europea.  | `2019-10-01` |  |
| Email | N/D | Direcciones de correo electrónico. | `2019-10-01` | También se devuelve con `domain=phi`.   |
| URL | N/D | Direcciones URL de sitios web. | `2019-10-01` | También se devuelve con `domain=phi`. |
| IP | N/D | Direcciones IP de red. | `2019-10-01` | También se devuelve con `domain=phi`. |
| DateTime | N/D | Fechas y horas del día. | `2019-10-01` |  | 
| DateTime | Date | Fechas calendario. | `2019-10-01` | También se devuelve con `domain=phi`. |
| Cantidad | N/D | Números y cantidades numéricas. | `2019-10-01` |  |
| Cantidad | Age | Edades. | `2019-10-01` | | |

## <a name="azure-information"></a>Información de Azure

Esta categoría de entidad incluye información de Azure identificable, incluida la información de autenticación y las cadenas de conexión. Disponible a partir de la versión del modelo `2019-10-01` No se devuelve con el parámetro `domain=phi`.

| Subcategoría                           | Descripción                                                                 |
|---------------------------------------|-----------------------------------------------------------------------------|
| Clave de autorización de Azure DocumentDB             | Clave de autorización para un servidor de Azure Cosmos DB.                           |
| Cadena de conexión de base de datos de IAAS de Azure y cadena de conexión de Azure SQL | Cadena de conexión de una base de datos de infraestructura como servicio (IaaS) de Azure y una cadena de conexión SQL. |
| Cadena de conexión de Azure SQL           | Cadena de conexión de una base de datos en Azure SQL Database.                                |
| Cadena de conexión de Azure IoT           | Cadena de conexión para Azure IoT.                        |
| Contraseña de configuración de publicación de Azure        | Contraseña para la configuración de publicación de Azure.                                        |
| Cadena de conexión de Azure Redis Cache   | Cadena de conexión para una instancia de Redis Cache.                             |
| SAS de Azure                             | Cadena de conexión para software como servicio (SaaS) de Azure.                     |
| Cadena de conexión de Azure Service Bus   | Cadena de conexión de una instancia de Azure Service Bus.                                 |
| Clave de la cuenta de Azure Storage             | Clave de una cuenta de Azure Storage.                                   |
| Clave de la cuenta de Azure Storage (genérica)   | Clave de cuenta genérica para una cuenta de Azure Storage.                           |
| Cadena de conexión de SQL Server          | Cadena de conexión para un equipo donde se ejecuta SQL Server.                                         |

## <a name="identification"></a>Identificación

[!INCLUDE [supported identification entities](./identification-entities.md)]
