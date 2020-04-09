---
title: Administración de versiones de API para .NET y REST
titleSuffix: Azure Cognitive Search
description: Directiva de versión para las API REST de Azure Cognitive Search y la biblioteca de cliente en el SDK de .NET.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 178f56354120bf7a65c51f1c9cf54e34bd011d97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79137295"
---
# <a name="api-versions-in-azure-cognitive-search"></a>Versiones de API en Azure Cognitive Search

Azure Cognitive Search implementa las actualizaciones de características de forma regular. A veces, aunque no siempre, estas actualizaciones requieren una nueva versión de la API para mantener la compatibilidad con versiones anteriores. La publicación de una nueva versión le permite controlar cuándo y cómo integrar en su código las actualizaciones del servicio de búsqueda.

Por lo general, el equipo de Azure Cognitive Search publica versiones nuevas solo cuando sea necesario, ya que el proceso de actualizar el código para que use una nueva versión de API puede suponer cierto esfuerzo. Solo se necesita una versión nueva si es necesario cambiar algún aspecto de la API de tal manera que se interrumpe la compatibilidad con versiones anteriores. Estos cambios pueden deberse a correcciones en las características existentes o a características nuevas que cambian el área expuesta de la API.

La misma regla se aplica a las actualizaciones del SDK. El SDK de Azure Cognitive Search sigue las reglas de [versionamiento semántico](https://semver.org/), lo que significa que la versión tiene tres partes: principal, secundaria y número de compilación (por ejemplo, 1.1.0). Solo se lanzará una versión principal nueva del SDK en caso de que haya cambios que impidan la compatibilidad con versiones anteriores. En caso de actualizaciones de características que no producen interrupciones, incrementaremos la versión secundaria y, en caso de correcciones de errores, solo aumentaremos la versión de compilación.

> [!NOTE]
> La instancia del servicio Azure Cognitive Search es compatible con varias versiones de API REST, incluida la más reciente. Puede seguir usando una versión aunque no sea la más reciente, pero es recomendable que migre el código para usar la versión más actualizada. Cuando se usa la API de REST, debe especificar la versión de API en cada solicitud realizada a través del parámetro api-version. Al usar el SDK de .NET, la versión del SDK que usa determina la versión correspondiente de la API de REST. Si usa un SDK anterior, puede seguir ejecutando ese código sin realizar ningún cambio, incluso si el servicio se ha actualizado para admitir una versión más reciente de la API.

## <a name="snapshot-of-current-versions"></a>Instantánea de las versiones actuales
A continuación se incluye una instantánea de las versiones actuales de todas las interfaces de programación para Azure Cognitive Search.


| Interfaces | Versión principal más reciente | Status |
| --- | --- | --- |
| [SDK de .NET](https://aka.ms/search-sdk) |9.0 |Disponibilidad general, publicada en mayo de 2019 |
| [Versión preliminar del SDK de .NET](https://aka.ms/search-sdk-preview) |8.0-preview |Versión preliminar, publicada en abril de 2019 |
| [API de REST de servicio](https://docs.microsoft.com/rest/api/searchservice/) |06-05-2019 |Disponibilidad general |
| [API REST de servicio 06-05-2019-versión preliminar](search-api-preview.md) |06-05-2019-versión preliminar |Vista previa |
| [SDK de administración de .NET](https://aka.ms/search-mgmt-sdk) |3.0 |Disponibilidad general |
| [API de REST de administración](https://docs.microsoft.com/rest/api/searchmanagement/) |13-03-2020|Disponibilidad general |

Para las API de REST, es necesario incluir `api-version` en cada llamada. Usando `api-version`, se puede establecer fácilmente una versión específica de destino, como una versión preliminar de API. En el ejemplo siguiente se muestra cómo se especifica el parámetro `api-version` :

    GET https://my-demo-app.search.windows.net/indexes/hotels?api-version=2019-05-06

> [!NOTE]
> Aunque cada solicitud tiene una `api-version`, se recomienda usar la misma versión para todas las solicitudes de API. Esto ocurre especialmente cuando las nuevas versiones de API incorporan atributos u operaciones que las versiones anteriores no reconocen. Debe evitarse la mezcla de versiones de API, ya que puede tener consecuencias no deseadas.
>
> Las versiones de la API de REST de servicio y de la API de REST de administración son independientes entre sí. Cualquier semejanza entre los números de versión es mera coincidencia.

Las API de disponibilidad general (o GA) puede usarse en entornos de producción y están sujetas a contratos de nivel de servicio de Azure. Las versiones preliminares tienen características experimentales que no siempre se migran a una versión de disponibilidad general. **Le recomendamos encarecidamente que no use una versión preliminar de API en aplicaciones de producción.**

## <a name="update-to-the-latest-version-of-the-rest-api-by-october-15-2020"></a>Actualice la API REST a la última versión antes del 15 de octubre de 2020
Las siguientes versiones de la API REST de Azure Cognitive Search se retirarán y ya no se admitirán a partir del 15 de octubre de 2020: **2014-07-31-Preview**, **2014-10-20-Preview**, **2015-02-28-Preview** y **2015-02-28**. Además, las versiones del SDK para .NET de Azure Cognitive Search anteriores a **3.0.0-rc** también se retirarán, ya que se dirigen a una de estas versiones de la API de REST. Después de esta fecha, las aplicaciones que usen cualquiera de las versiones en desuso de la API REST o del SDK dejarán de funcionar y deben actualizarse. Con todos los cambios de este tipo, se dará una notificación de 12 meses, por lo que tiene tiempo suficiente para hacer el ajuste.  Para seguir usando Azure Cognitive Search, migre el código existente que tenga como destino la [API de REST](search-api-migration.md) a la [versión 2019-05-06 de la API de REST](https://docs.microsoft.com/rest/api/searchservice/) o posterior, o bien el SDK de .NET a la [versión 3.0](search-dotnet-sdk-migration.md) o posterior antes del 15 de octubre de 2020.  Si tiene alguna pregunta sobre la actualización a la versión más reciente, envíe un correo electrónico a azuresearch_contact@microsoft.com antes del 15 de mayo de 2020 para asegurarse de que dispone de tiempo suficiente para actualizar el código.

## <a name="about-preview-and-generally-available-versions"></a>Acerca de las versiones preliminar y de disponibilidad general
Azure Cognitive Search siempre publica previamente características experimentales primero a través de la API REST y, después, a través de las versiones preliminares del SDK de .NET.

Las características en versión preliminar están disponibles para prueba y experimentación con el fin de recabar comentarios sobre la implementación y el diseño de características. Por este motivo, las características en versión preliminar pueden cambiar con el tiempo, posiblemente de maneras que interrumpan la compatibilidad con versiones anteriores. Esto difiere de características en versión de disponibilidad general, que son estables y es poco probable que cambien, con la excepción de las mejoras y correcciones compatibles con versiones anteriores. Además, las características en versión preliminar no siempre terminando pasando a la fase de disponibilidad general.

Por estos motivos, no se recomienda escribir código de producción que dependa de versiones preliminares. Si usa una versión preliminar antigua, se recomienda que migre a la versión de disponibilidad general (GA).

Para .NET SDK: encontrará indicaciones para la migración de código en [Actualización de .NET SDK](search-dotnet-sdk-migration-version-9.md).

La disponibilidad general significa que Azure Cognitive Search está ahora sujeta al Acuerdo de Nivel de Servicio. Lo encontrará en [Acuerdo de Nivel de Servicio de Azure Cognitive Search](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
