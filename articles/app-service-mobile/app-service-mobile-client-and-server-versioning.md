---
title: Control de versiones del SDK de cliente y servidor
description: Lista de SDK de cliente y compatibilidad con las versiones del SDK de servidor para Mobile Services y Azure Mobile Apps.
ms.assetid: 35b19672-c9d6-49b5-b405-a6dcd1107cd5
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.openlocfilehash: a9ba442c00ec2498139ee34a1ff7497c98f17ede
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80293481"
---
# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Control de versiones de cliente y servidor en Mobile Apps y Mobile Services

La versión más reciente de Azure Mobile Services es la característica **Mobile Apps** de Azure App Service.

Los SDK de cliente y servidor de Mobile Apps originalmente se basaban en los de Mobile Services, pero *no* son compatibles entre sí.
Es decir, debe usar el SDK de cliente de *Mobile Apps* con un SDK de servidor de *Mobile Apps* y algo parecido sucede con *Mobile Services*. Este contrato se aplica a través de un valor de encabezado especial usado por los SDK de cliente y servidor, `ZUMO-API-VERSION`.

Nota: cada vez que este documento hace referencia a un back-end de *Mobile Services* , no es necesario que esté hospedado en Mobile Services. Ahora es posible migrar un servicio móvil para que se ejecute en App Service sin realizar ningún cambio en el código, pero el servicio seguirá usando versiones de SDK de *Mobile Services*.

## <a name="header-specification"></a>Especificación del encabezado
La clave `ZUMO-API-VERSION` se puede especificar en el encabezado HTTP o en la cadena de consulta. El valor es una cadena de versión con el formato **x.y.z**.

Por ejemplo:

`GET https://service.azurewebsites.net/tables/TodoItem`

HEADERS: ZUMO-API-VERSION: 2.0.0

`POST https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0`

## <a name="opting-out-of-version-checking"></a>Anulación de la comprobación de la versión
Para anular la comprobación de la versión, establezca el valor **true** en la configuración de la aplicación **MS_SkipVersionCheck**. Especifique esto en el archivo web.config o en la sección Configuración de la aplicación del Portal de Azure.

> [!NOTE]
> Hay una serie de cambios de comportamiento entre Mobile Services y Mobile Apps, especialmente en las áreas de sincronización sin conexión, autenticación y notificaciones push. Solo debe anular la comprobación de versión después de realizar pruebas exhaustivas para asegurarse de que estos cambios de comportamiento no impiden la funcionalidad de la aplicación.

## <a name="azure-mobile-apps-client-and-server"></a><a name="2.0.0"></a>Cliente y servidor de Azure Mobile Apps
### <a name="mobile-apps-client-sdks"></a><a name="MobileAppsClients"></a> SDK de cliente de *Aplicaciones* móviles
La comprobación de versión se introdujo a partir de las siguientes versiones del SDK de cliente de **Azure Mobile Apps**:

| Plataforma de cliente | Versión | Valor de encabezado de versión |
| --- | --- | --- |
| Cliente administrado (Windows, Xamarin) |[2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) |2.0.0 |
| iOS |[3.0.0](https://go.microsoft.com/fwlink/?LinkID=529823) |2.0.0 |
| Android |[3.0.0](https://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) |3.0.0 |

### <a name="mobile-apps-server-sdks"></a>SDK de servidor de *Mobile Apps*
La comprobación de versión se incluye en las siguientes versiones del SDK de servidor:

| Plataforma de servidor | SDK | Encabezado de versión aceptado |
| --- | --- | --- |
| .NET |[Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) |2.0.0 |
| Node.js |[azure-mobile-apps)](https://www.npmjs.com/package/azure-mobile-apps) |2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Comportamiento de back-ends de Mobile Apps
| ZUMO-API-VERSION | Valor de MS_SkipVersionCheck | Response |
| --- | --- | --- |
| x.y.z o Null |True |200 - CORRECTO |
| Null |False/Sin especificar |400 - Solicitud incorrecta |
| 1.x.y |False/Sin especificar |400 - Solicitud incorrecta |
| 2.0.0-2.x.y |False/Sin especificar |200 - CORRECTO |
| 3.0.0-3.x.y |False/Sin especificar |400 - Solicitud incorrecta |

[Mobile Services clients]: #MobileServicesClients
[Mobile Apps clients]: #MobileAppsClients
[Mobile App Server SDK]: https://www.nuget.org/packages/microsoft.azure.mobile.server
