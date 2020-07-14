---
title: Versiones de API
titleSuffix: Azure Cognitive Search
description: Directiva de versión para las API REST de Azure Cognitive Search y la biblioteca de cliente en el SDK de .NET.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: a7179f88f507f0deedc79e7ae49988c8b5a32f86
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85830101"
---
# <a name="api-versions-in-azure-cognitive-search"></a>Versiones de API en Azure Cognitive Search

Azure Cognitive Search implementa las actualizaciones de características de forma regular. A veces, aunque no siempre, estas actualizaciones requieren una nueva versión de la API para mantener la compatibilidad con versiones anteriores. La publicación de una nueva versión le permite controlar cuándo y cómo integrar en su código las actualizaciones del servicio de búsqueda.

Por lo general, el equipo de Azure Cognitive Search publica versiones nuevas solo cuando sea necesario, ya que el proceso de actualizar el código para que use una nueva versión de API puede suponer cierto esfuerzo. Solo se necesita una versión nueva si es necesario cambiar algún aspecto de la API de tal manera que se interrumpe la compatibilidad con versiones anteriores. Estos cambios pueden deberse a correcciones en las características existentes o a características nuevas que cambian el área expuesta de la API.

La misma regla se aplica a las actualizaciones del SDK. El SDK de Azure Cognitive Search sigue las reglas de [versionamiento semántico](https://semver.org/), lo que significa que la versión tiene tres partes: principal, secundaria y número de compilación (por ejemplo, 1.1.0). Solo se lanzará una versión principal nueva del SDK en caso de que haya cambios que impidan la compatibilidad con versiones anteriores. En caso de actualizaciones de características que no producen interrupciones, incrementaremos la versión secundaria y, en caso de correcciones de errores, solo aumentaremos la versión de compilación.

> [!NOTE]
> Una instancia del servicio Search de Azure Cognitive es compatible con varias versiones de API de REST, incluida la más reciente. Puede seguir usando una versión aunque no sea la más reciente, pero es recomendable que migre el código para usar la versión más actualizada. Cuando se usa la API de REST, debe especificar la versión de API en cada solicitud realizada a través del parámetro api-version. Al usar el SDK de .NET, la versión del SDK que usa determina la versión correspondiente de la API de REST. Si usa un SDK anterior, puede seguir ejecutando ese código sin realizar ningún cambio, incluso si el servicio se ha actualizado para admitir una versión más reciente de la API.

## <a name="rest-apis"></a>API de REST

En esta tabla se proporciona el historial de versiones de las versiones actuales y publicadas anteriormente de la API de REST del servicio Search. La documentación se publica para las versiones estables actuales y en versión preliminar.

| Versión&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   | Estado | Problema de compatibilidad con versiones anteriores |
|-------------|--------|------------------------------|
| [Administración 2020-03-13](https://docs.microsoft.com/rest/api/searchmanagement/) | Disponibilidad general | Versión estable más reciente de las API de REST de administración, con avances en la protección de puntos de conexión. Agrega puntos de conexión privados, compatibilidad con vínculos privados y reglas de red para los nuevos servicios. |
| [Administración 2019-10-01-Preview](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview) | Versión preliminar  | A pesar de su número de versión, sigue siendo la versión preliminar actual de las API de REST de administración. No hay ninguna característica en versión preliminar en este momento. Todas las características en versión preliminar han pasado recientemente a disponibilidad general. |
| Administración 2015-08-19  | Stable| La primera versión disponible con carácter general de las API de REST de administración. Proporciona aprovisionamiento de servicios, escalado vertical y administración de claves de API. |
| Administración 2015-08-19-Preview | Versión preliminar| La primera versión preliminar de las API de REST de administración. |
| [Search 2020-06-30](https://docs.microsoft.com/rest/api/searchservice/index)| Stable | La versión estable más reciente de las API de REST del servicio Search, con avances en la puntuación de relevancia. |
| [Search 2020-06-30-Preview](https://docs.microsoft.com/rest/api/searchservice/index-preview)| Versión preliminar | Versión preliminar asociada a la versión estable. |
| Search 2019-05-06 | Stable | Agrega tipos complejos. |
| Search 2019-05-06-Preview | Versión preliminar | Versión preliminar asociada a la versión estable. |
| Search 2017-11-11 | Stable  | Agrega conjuntos de aptitudes y enriquecimiento con IA. |
| Search 2017-11-11-Preview | Versión preliminar | Versión preliminar asociada a la versión estable. |
| Search 2016-09-01 |Stable | Agrega indexadores.|
| Search 2016-09-01-Preview | Versión preliminar | Versión preliminar asociada a la versión estable.|
| Search 2015-02-28 | Stable  | Primera versión con disponibilidad general.  |
| Search 2015-02-28-Preview | Versión preliminar | Versión preliminar asociada a la versión estable. |
| Search 2014-10-20-Preview | Versión preliminar | Segunda versión preliminar pública. |
| Search 2014-07-31-Preview | Versión preliminar | Primera versión preliminar pública. |

## <a name="azure-sdk-for-net"></a>SDK de Azure para .NET

El historial de versiones del paquete está disponible en NuGet.org. En esta tabla se proporcionan vínculos a cada página del paquete.

| Versión del SDK | Estado | Descripción |
|-------------|--------|------------------------------|
| [**Azure.Search.Documents 1.0.0-preview.4**](https://www.nuget.org/packages/Azure.Search.Documents/1.0.0-preview.4) | Versión preliminar | Nueva biblioteca cliente del SDK de .NET de Azure, publicada en mayo de 2020. Tiene como destino la versión de la API de REST 2020-06-30.|
| [**Microsoft.Azure.Search 10.0**](https://www.nuget.org/packages/Microsoft.Azure.Search/) | Disponibilidad general, publicada en mayo de 2019. Tiene como destino la versión de la API de REST 2019-05-06.|
| [**Microsoft.Azure.Search 8.0-preview**](https://www.nuget.org/packages/Microsoft.Azure.Search/8.0.0-preview) | Versión preliminar, publicada en abril de 2019. Tiene como destino la versión de la API de REST 2019-05-06-Preview.|
| [**Microsoft.Azure.Management.Search 3.0.0**](https://docs.microsoft.com/dotnet/api/overview/azure/search/management?view=azure-dotnet) | Stable | Tiene como destino la API de REST de administración api-version=2015-08-19. |

## <a name="azure-sdk-for-java"></a>SDK de Azure para Java

| Versión del SDK | Estado | Descripción  |
|-------------|--------|------------------------------|
| [**Java SearchManagementClient 1.35.0**](https://docs.microsoft.com/java/api/overview/azure/search/management?view=azure-java-stable) | Stable | Tiene como destino la API de REST de administración api-version=2015-08-19.|

## <a name="azure-sdk-for-python"></a>SDK de Azure para Python

| Versión del SDK | Estado | Descripción  |
|-------------|--------|------------------------------|
| [**Python azure-mgmt-search 1.0**](https://docs.microsoft.com/python/api/overview/azure/search?view=azure-python) | Stable | Tiene como destino la API de REST de administración api-version=2015-08-19. |