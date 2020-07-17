---
title: Identidades administradas en Azure SignalR Service
description: Obtenga información sobre cómo funcionan las identidades administradas en Azure SignalR Service y sobre cómo usar una identidad administrada en escenarios sin servidor.
author: chenyl
ms.service: signalr
ms.topic: article
ms.date: 06/8/2020
ms.author: chenyl
ms.openlocfilehash: abe7503e7eb73d533ae901af21de001960173fb0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85559415"
---
# <a name="managed-identities-for-azure-signalr-service"></a>Identidades administradas para Azure SignalR Service

En este artículo se explica cómo crear una identidad administrada para Azure SignalR Service y cómo usarla en escenarios sin servidor.

> [!Important] 
> Azure SignalR Service solo admite una identidad administrada. Esto significa que puede agregar una identidad asignada por el sistema o asignada por el usuario. 

## <a name="add-a-system-assigned-identity"></a>Adición de una identidad asignada por el sistema

Para configurar una identidad administrada en Azure Portal, primero tiene que crear una instancia de Azure SignalR Service y después habilitar la característica.

1. Cree una instancia de Azure SignalR Service en el portal como lo haría normalmente. Búsquela en el portal.

2. Seleccione **Identidad**.

4. En la pestaña **Asignado por el sistema**, cambie **Estado** a **Activado**. Seleccione **Guardar**.

    :::image type="content" source="media/signalr-howto-use-managed-identity/system-identity-portal.png" alt-text="Adición de una identidad asignada por el sistema en el portal":::

## <a name="add-a-user-assigned-identity"></a>Adición de una identidad asignada por el usuario

La creación de una instancia de Azure SignalR Service con una identidad asignada por el usuario requiere que se cree la identidad y luego se agregue su identificador de recurso al servicio.

1. Cree un recurso de identidad administrada asignada por el usuario según [estas instrucciones](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity).

2. Cree una instancia de Azure SignalR Service en el portal como lo haría normalmente. Búsquela en el portal.

3. Seleccione **Identidad**.

4. En la pestaña **Usuario asignado**, seleccione **Agregar**.

5. Busque la identidad que creó anteriormente y selecciónela. Seleccione **Agregar**.

    :::image type="content" source="media/signalr-howto-use-managed-identity/user-identity-portal.png" alt-text="Adición de una identidad asignada por el usuario en el portal":::

## <a name="use-a-managed-identity-in-serverless-scenarios"></a>Uso de una identidad administrada en escenarios sin servidor

Azure SignalR Service es un servicio totalmente administrado, por lo que no puede usar una identidad administrada para obtener tokens manualmente. En cambio, Azure SignalR Service usa la identidad administrada que se establece para obtener un token de acceso. Después, el servicio establece el token de acceso en un encabezado `Authorization` de una solicitud ascendente en escenarios sin servidor.

### <a name="enable-managed-identity-authentication-in-upstream-settings"></a>Habilitar la autenticación de la identidad administrada en la configuración ascendente

1. Agregue una identidad asignada por el sistema o una asignada por el usuario.

2. Ajuste la configuración ascendente y use **ManagedIdentity** como valor de **Auth**. Para obtener información sobre cómo crear una configuración ascendente con autenticación, vea [Configuración de Upstream](concept-upstream.md).

3. En la configuración de autenticación de la identidad administrada, puede especificar el recurso de destino en **Recurso**. El recurso se convertirá en una notificación `aud` en el token de acceso obtenido, que se puede usar como parte de la validación en los puntos de conexión ascendentes. El recurso puede ser uno de los siguientes:
    - Vacío
    - Identificador de la aplicación (cliente) de la entidad de servicio
    - URI del identificador de la aplicación de la entidad de servicio
    - [Identificador de recursos de un servicio de Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities#azure-services-that-support-azure-ad-authentication)

    > [!NOTE]
    > Si valida un token de acceso por sí mismo en el servicio, puede elegir cualquier formato para el recurso. Solo tiene que asegurarse de que el valor **Recurso** de la configuración de **Auth** sea coherente con la validación. Si usa el control de acceso basado en roles (RBAC) para un plano de datos, debe usar el recurso que solicite el proveedor de servicios.

### <a name="validate-access-tokens"></a>Validación de los tokens de acceso

El token del encabezado `Authorization` es un [token de acceso plataforma de identidades de Microsoft](https://docs.microsoft.com/azure/active-directory/develop/access-tokens#validating-tokens).

Para validar los tokens de acceso, la aplicación también debe validar la audiencia y los tokens de firmas. Deben validarse con los valores del documento de detección de OpenID. Por ejemplo, fijémonos en la [versión independiente del inquilino del documento](https://login.microsoftonline.com/common/.well-known/openid-configuration).

El middleware de Azure Active Directory (Azure AD) tiene capacidades integradas para validar los tokens de acceso. Puede examinar nuestros [ejemplos](https://docs.microsoft.com/azure/active-directory/develop/sample-v2-code) para encontrar uno en el idioma que elija.

Proporcionamos bibliotecas y ejemplos de código que le muestran cómo controlar la validación de tokens. También hay varias bibliotecas de asociados de código abierto disponibles para la validación de JSON Web Token (JWT). Al menos hay una opción para casi cualquier plataforma y lenguaje. Para obtener más información acerca de los ejemplos de código y las bibliotecas de autenticación de Azure AD, vea [Bibliotecas de autenticación de la plataforma de identidad de Microsoft](https://docs.microsoft.com/azure/active-directory/develop/reference-v2-libraries).

## <a name="next-steps"></a>Pasos siguientes

- [Desarrollo y configuración de Azure Functions con Azure SignalR Service](signalr-concept-serverless-development-config.md)
