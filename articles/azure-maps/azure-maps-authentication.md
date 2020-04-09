---
title: Métodos de autenticación | Microsoft Azure Maps
description: En este artículo, obtendrá información sobre la autenticación de clave compartida y de Azure Active Directory (Azure AD). Ambos métodos se usan para los servicios de Microsoft Azure Maps. Obtenga información sobre cómo obtener una clave de suscripción de Azure Maps.
author: philmea
ms.author: philmea
ms.date: 01/28/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 21d29cba85adfc147ec9deb6ab362a5da943bf10
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335705"
---
# <a name="authentication-with-azure-maps"></a>Autenticación con Azure Maps

Azure Maps admite dos formas de autenticar las solicitudes: Autenticación de clave compartida y autenticación de Azure Active Directory. En este artículo se explican estos métodos de autenticación para guiar su implementación de los servicios de Azure Maps.

> [!NOTE]
> Para mejorar la comunicación segura con Azure Maps, ahora admitimos Seguridad de la capa de transporte (TLS) 1.2 y retiramos la compatibilidad con TLS 1.0 y 1.1. Para evitar cualquier interrupción del servicio, **actualice sus servidores y aplicaciones para usar TLS 1.2 antes del 2 de abril de 2020**.  Si actualmente usa TLS 1.x, evalúe su preparación de TLS 1.2 y desarrolle un plan de migración con las pruebas descritas en [Solución del problema de TLS 1.0](https://docs.microsoft.com/security/solving-tls1-problem).

## <a name="shared-key-authentication"></a>Autenticación de clave compartida

 Las claves principal y secundaria se generan después de crearse la cuenta de Azure Maps. Se recomienda usar la clave principal como clave de suscripción al llamar a Azure Maps mediante la autenticación de clave compartida. La autenticación de clave compartida pasa las claves generadas por una cuenta de Azure Maps a un servicio de Azure Maps. En solicitud a los servicios de Azure Maps requiere, agregue la *clave de suscripción* como parámetro a la dirección URL. La clave secundaria se puede usar, por ejemplo, para cambios de clave graduales.  

Para obtener información sobre cómo ver sus claves en Azure Portal, consulte [Administración de la autenticación](https://aka.ms/amauthdetails).

> [!Tip]
> Se recomienda regenerar las claves periódicamente. Dado que se le proporcionan dos claves, puede mantener las conexiones con una mientras se regenera la otra. Cuando regenere las claves, debe actualizar con las nuevas claves todas las aplicaciones que acceden a la cuenta.

## <a name="authentication-with-azure-active-directory-preview"></a>Autenticación con Azure Active Directory (versión preliminar)

Azure Maps ahora ofrece la autenticación de solicitudes de servicios de Azure Maps mediante [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis). Azure AD proporciona autenticación basada en identidades, que incluye el [control de acceso basado en rol (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview). RBAC se usa para conceder acceso de nivel de usuario, grupo o aplicación a recursos de Azure Maps. En las secciones siguientes se describen los conceptos y componentes de la integración de Azure Maps con Azure AD.

## <a name="authentication-with-oauth-access-tokens"></a>Autenticación con tokens de acceso de OAuth

Azure Maps acepta tokens de acceso de **OAuth 2.0** para inquilinos de Azure AD asociados a suscripciones de Azure que contienen una cuenta de Azure Maps. Azure Maps también acepta tokens de:

* Usuarios de Azure AD
* Aplicaciones de asociados que usan permisos delegados por los usuarios
* Identidades administradas de recursos de Azure

Azure Maps genera un *identificador único (Id. de cliente)* para cada cuenta de Azure Maps. Puede solicitar tokens de Azure AD cuando combine este identificador de cliente con otros parámetros. Para solicitar un token, especifique los valores de la tabla siguiente en función de su entorno de Azure.

| Entorno de Azure   | Punto de conexión del token de Azure AD |
| --------------------|-------------------------|
| Azure Public        | https://login.microsoftonline.com |
| Azure Government    | https://login.microsoftonline.us |


Para más información sobre cómo configurar Azure AD y solicitar tokens para Azure Maps, consulte [Administración de la autenticación en Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

Para información general sobre la solicitud de tokens de Azure AD, consulte [¿Qué es la autenticación?](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).

## <a name="request-azure-map-resources-with-oauth-tokens"></a>Solicitud de recursos de Azure Maps con tokens de OAuth

Después de recibir un token de Azure AD, Azure Maps envía una solicitud con el siguiente conjunto de encabezados de solicitud requeridos:

| Encabezado de solicitud    |    Value    |
|:------------------|:------------|
| x-ms-client-id    | 30d7cc….9f55|
| Authorization     | Bearer eyJ0e….HNIVN |

> [!Note]
> `x-ms-client-id` es el GUID basado en la cuenta de Azure Maps que aparece en la página de autenticación de Azure Maps.

A continuación se muestra un ejemplo de una solicitud de ruta de Azure Maps que usa un token de OAuth:

```
GET /route/directions/json?api-version=1.0&query=52.50931,13.42936:52.50274,13.43872 
Host: atlas.microsoft.com 
x-ms-client-id: 30d7cc….9f55 
Authorization: Bearer eyJ0e….HNIVN 
```

Para información sobre cómo ver el identificador de cliente, consulte [Visualización de los detalles de la autenticación](https://aka.ms/amauthdetails).

## <a name="control-access-with-rbac"></a>Control del acceso con RBAC

En Azure AD, use RBAC para controlar el acceso a los recursos protegidos. Configure su cuenta de Azure Maps y registre su inquilino de Azure AD para Azure Maps. Azure Maps admite el control de acceso de lectura para usuarios, grupos y aplicaciones de Azure AD, recursos de Azure y servicios de Azure individuales mediante identidades administradas para recursos de Azure. En la página del portal de Azure Maps, puede configurar RBAC para los roles elegidos.

![Azure Maps Data Reader (versión preliminar)](./media/azure-maps-authentication/concept.png)

Para información sobre cómo ver la configuración de RBAC, consulte [Configuración de Azure Maps](https://aka.ms/amrbac).

## <a name="managed-identities-for-azure-resources-and-azure-maps"></a>Identidades administradas para recursos de Azure y Azure Maps

Las [identidades administradas para los recursos de Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) proporcionan a los servicios de Azure una identidad administrada automáticamente, que se puede autorizar para acceder a los servicios de Azure Maps. Algunos ejemplos de identidades administradas son: Azure App Service, Azure Functions y Azure Virtual Machines.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre la autenticación de una aplicación con Azure AD y Azure Maps, consulte [Administración de la autenticación en Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

* Para más información sobre la autenticación del Control mapa de Azure Maps y Azure AD, consulte [Uso del Control de mapa de Azure Maps](https://aka.ms/amaadmc).
