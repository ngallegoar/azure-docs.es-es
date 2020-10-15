---
title: Versiones de API
titleSuffix: Azure Cognitive Search
description: Directiva de versión para las API REST de Azure Cognitive Search y la biblioteca de cliente en el SDK de .NET.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: fab49c4242afc2e4f2d0595e2aa95f19f3d976f8
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2020
ms.locfileid: "91951586"
---
# <a name="api-versions-in-azure-cognitive-search"></a>Versiones de API en Azure Cognitive Search

Azure Cognitive Search implementa las actualizaciones de características de forma regular. A veces, aunque no siempre, estas actualizaciones requieren una nueva versión de la API para mantener la compatibilidad con versiones anteriores. La publicación de una nueva versión le permite controlar cuándo y cómo integrar en su código las actualizaciones del servicio de búsqueda.

Por lo general, el equipo de Azure Cognitive Search publica versiones nuevas solo cuando sea necesario, ya que el proceso de actualizar el código para que use una nueva versión de API puede suponer cierto esfuerzo. Solo se necesita una versión nueva si es necesario cambiar algún aspecto de la API de tal manera que se interrumpe la compatibilidad con versiones anteriores. Estos cambios pueden deberse a correcciones en las características existentes o a características nuevas que cambian el área expuesta de la API.

La misma regla se aplica a las actualizaciones del SDK. El SDK de Azure Cognitive Search sigue las reglas de [versionamiento semántico](https://semver.org/), lo que significa que la versión tiene tres partes: principal, secundaria y número de compilación (por ejemplo, 1.1.0). Solo se lanzará una versión principal nueva del SDK en caso de que haya cambios que impidan la compatibilidad con versiones anteriores. En caso de actualizaciones de características que no producen interrupciones, incrementaremos la versión secundaria y, en caso de correcciones de errores, solo aumentaremos la versión de compilación.

> [!Important]
> Los SDK de Azure para .NET, Java, Python y JavaScript están implementando nuevas bibliotecas de cliente para Azure Cognitive Search. Actualmente, ninguna de las bibliotecas de SDK de Azure admite totalmente las API REST de búsqueda más recientes (2020-06-30) o las API REST de administración (2020-03-13), pero esto cambiará en el futuro. Puede consultar periódicamente esta página o las [Novedades](whats-new.md) para estar al tanto de los anuncios sobre mejoras funcionales.

<a name="unsupported-versions"></a>

## <a name="unsupported-versions"></a>Versiones no admitidas

Actualice las soluciones de búsqueda existentes a la versión más reciente de la API de REST antes del 15 de octubre de 2020. Las siguientes versiones de la API de REST de Azure Cognitive Search se retirarán y ya no se admitirán a partir de esa fecha:

+ **2015-02-28**
+ **2015-02-28-Preview**
+ **2014-07-31-Preview**
+ **2014-10-20-Preview**

Además, las versiones del SDK de .NET de Azure Cognitive Search anteriores a [**3.0.0-rc**](https://www.nuget.org/packages/Microsoft.Azure.Search/3.0.0-rc) también se retirarán, ya que su destino es una de estas versiones de la API de REST. 

Después de esta fecha, las aplicaciones que usen cualquiera de las versiones en desuso de la API REST o del SDK dejarán de funcionar y deben actualizarse. Con todos los cambios de este tipo, se dará una notificación de 12 meses, por lo que tiene tiempo suficiente para hacer el ajuste.

Para seguir usando Azure Cognitive Search, migre el código existente cuyo destino sea la [API de REST](search-api-migration.md) a la [versión 2020-06-30 de la API de REST](/rest/api/searchservice/) o a un SDK más reciente antes del 15 de octubre de 2020.  Si tiene alguna pregunta sobre la actualización a la versión más reciente, envíe un correo electrónico a azuresearch_contact@microsoft.com antes del 15 de mayo de 2020 para asegurarse de que dispone de tiempo suficiente para actualizar el código.

## <a name="rest-apis"></a>API de REST

Una instancia del servicio Search de Azure Cognitive es compatible con varias versiones de API de REST, incluida la más reciente. Puede seguir usando una versión aunque no sea la más reciente, pero es recomendable que [migre el código](search-api-migration.md) para usar la versión más actualizada. Cuando se usa la API de REST, debe especificar la versión de API en cada solicitud realizada a través del parámetro api-version. Al usar el SDK de .NET, la versión del SDK que usa determina la versión correspondiente de la API de REST. Si usa un SDK anterior, puede seguir ejecutando ese código sin realizar ningún cambio, incluso si el servicio se ha actualizado para admitir una versión más reciente de la API.

En la siguiente tabla se proporciona el historial de versiones de las versiones actuales y publicadas anteriormente de la API REST del servicio Search. La documentación se publica solo para las versiones estables más recientes y en versión preliminar.

### <a name="search-service-apis"></a>API del servicio Search

Cree y administre contenido en un servicio de búsqueda.

| Versión&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   | Estado | Descripción |
|-------------------------|--------|------------------------------|
| [Search 2020-06-30](/rest/api/searchservice/index)| Stable | La versión estable más reciente de las API REST del servicio Search, con avances en la puntuación de relevancia y disponibilidad general para el almacén de conocimiento.|
| [Search 2020-06-30-Preview](/rest/api/searchservice/index-preview)| Versión preliminar | Versión preliminar asociada a la versión estable. Incluye varias [características en vista previa](search-api-preview.md). |
| Search 2019-05-06 | Stable  | Agrega [tipos complejos](search-howto-complex-data-types.md). |
| Search 2019-05-06-Preview | Versión preliminar | Versión preliminar asociada a la versión estable. |
| Search 2017-11-11 | Stable | Agrega conjuntos de aptitudes y [enriquecimiento con IA](cognitive-search-concept-intro.md). |
| Search 2017-11-11-Preview | Versión preliminar | Versión preliminar asociada a la versión estable. |
| Search 2016-09-01 |Stable | Agrega [indexadores](search-indexer-overview.md). |
| Search 2016-09-01-Preview | Versión preliminar | Versión preliminar asociada a la versión estable.|
| Search 2015-02-28 | No se admite a partir del 10-10-2020   | Primera versión con disponibilidad general.  |
| Search 2015-02-28-Preview | No se admite a partir del 10-10-2020  | Versión preliminar asociada a la versión estable. |
| Search 2014-10-20-Preview | No se admite a partir del 10-10-2020 | Segunda versión preliminar pública. |
| Search 2014-07-31-Preview | No se admite a partir del 10-10-2020  | Primera versión preliminar pública. |

### <a name="management-rest-apis"></a>API de REST de administración

Crea y configura un servicio de búsqueda y administra claves de API.

| Versión&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   | Estado | Descripción |
|-------------------------|--------|------------------------------|
| [Administración 2020-08-01](/rest/api/searchmanagement/) | Stable | Versión estable más reciente de las API REST de administración. Agrega compatibilidad con recursos de vínculo privado compartido y disponible con carácter general a todos los recursos a los que se tiene acceso de salida, excepto los indicados en la versión preliminar. |
| [Administración 2020-08-01-Preview](/rest/api/searchmanagement/index-preview) | Versión preliminar  | Actualmente en versión preliminar: compatibilidad con recursos de vínculo privado compartidos para Azure Functions y Azure Database for MySQL. |
| Management 2020-03-13  | Stable | Agrega un [punto de conexión privado](service-create-private-endpoint.md) mediante un vínculo privado y [reglas de IP de red](service-configure-firewall.md) para nuevos servicios. Para más información, consulte esta [especificación de Swagger](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/search/resource-manager/Microsoft.Search/stable/2020-08-01). |
| Management 2019-10-01-Preview | Vista previa  | No se presentaron características en versión preliminar en esta lista. Esta versión preliminar es funcionalmente equivalente a la versión 2020-03-13. Para más información, consulte esta [especificación de Swagger](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/search/resource-manager/Microsoft.Search/preview/2019-10-01-preview). |
| Administración 2015-08-19  | Stable | La primera versión disponible con carácter general de las API de REST de administración. Proporciona aprovisionamiento de servicios, escalado vertical y administración de claves de API. Para más información, consulte esta [especificación de Swagger](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/search/resource-manager/Microsoft.Search/stable/2015-08-19). |
| Administración 2015-08-19-Preview  | Versión preliminar | La primera versión preliminar de las API de REST de administración. Para más información, consulte esta [especificación de Swagger](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/search/resource-manager/Microsoft.Search/stable/2015-08-19). |

## <a name="azure-sdk-for-net"></a>SDK de Azure para .NET

En la tabla siguiente se proporcionan vínculos a versiones más recientes del SDK. 

| Versión del SDK | Estado | Descripción |
|-------------|--------|------------------------------|
| [Azure.Search.Documents 11.0](/dotnet/api/overview/azure/search.documents-readme) | Stable | Nueva biblioteca cliente del SDK de .NET de Azure, publicada en julio de 2020. Tiene como destino la API REST de búsqueda API-version = 2020-06-30, pero aún no admite, filtros geográficos ni [FieldBuilder](/dotnet/api/microsoft.azure.search.fieldbuilder). |
| [Microsoft.Azure.Search 10.0](https://www.nuget.org/packages/Microsoft.Azure.Search/) | Stable | Publicada en mayo de 2019. Tiene como destino la API REST del servicio Search api-version=2019-05-06.|
| [Microsoft.Azure.Management.Search 4.0.0](/dotnet/api/overview/azure/search/management) | Stable | Tiene como destino la API REST de administración api-version=2020-08-01.  |
| Microsoft.Azure.Management.Search 3.0.0 | Stable | Tiene como destino la API de REST de administración api-version=2015-08-19.  |

## <a name="azure-sdk-for-java"></a>SDK de Azure para Java

| Versión del SDK | Estado | Descripción  |
|-------------|--------|------------------------------|
| [Java azure-search-documents 11](https://newreleases.io/project/github/Azure/azure-sdk-for-java/release/azure-search-documents_11.1.0) | Stable | Nueva biblioteca cliente del SDK de .NET de Azure, publicada en julio de 2020. Tiene como destino la API REST del servicio Search api-version=2019-05-06. |
| [Java Management Client 1.35.0](/java/api/overview/azure/search/management) | Stable | Tiene como destino la API de REST de administración api-version=2015-08-19. |

## <a name="azure-sdk-for-javascript"></a>SDK de Azure para JavaScript

| Versión del SDK | Estado | Descripción  |
|-------------|--------|------------------------------|
| [JavaScript azure-search 11.0](https://azure.github.io/azure-sdk-for-node/azure-search/latest/) | Stable | Nueva biblioteca cliente del SDK de .NET de Azure, publicada en julio de 2020. Tiene como destino la API REST del servicio Search api-version=2016-09-01. |
| [JavaScript azure-arm-search](https://azure.github.io/azure-sdk-for-node/azure-arm-search/latest/) | Stable | Tiene como destino la API de REST de administración api-version=2015-08-19. |

## <a name="azure-sdk-for-python"></a>SDK de Azure para Python

| Versión del SDK | Estado | Descripción  |
|-------------|--------|------------------------------|
| [Python azure-search-documents 11.0](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-search-documents/11.0.0/index.html) | Stable | Nueva biblioteca cliente del SDK de .NET de Azure, publicada en julio de 2020. Tiene como destino la API REST del servicio Search api-version=2019-05-06. |
| [Python azure-mgmt-search 1.0](/python/api/overview/azure/search) | Stable | Tiene como destino la API de REST de administración api-version=2015-08-19. |