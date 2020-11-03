---
title: Acerca de los conectores de API en Azure AD B2C
description: Use los conectores de API de Azure Active Directory (Azure AD) para personalizar y ampliar los flujos de usuario de registro con las API web.
services: active-directory-b2c
ms.service: active-directory
ms.subservice: B2C
ms.topic: how-to
ms.date: 10/15/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.openlocfilehash: 94d6b0192b014396f8751e58f5620aec5c132203
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92503885"
---
# <a name="use-api-connectors-to-customize-and-extend-sign-up-user-flows"></a>Uso de conectores de API para personalizar y extender los flujos de usuario de registro

> [!IMPORTANT]
> Los conectores de API para el registro son una característica en versión preliminar pública de Azure AD B2C. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="overview"></a>Información general 
Como desarrollador o administrador de TI, puede usar conectores de API para integrar los flujos de usuario de registro en API web para personalizar la experiencia de registro. Por ejemplo, con los conectores de API, puede:

- **Validar los datos de entrada del usuario**. Haga una validación de los datos de usuario mal formados o no válidos. Por ejemplo, puede validar los datos proporcionados por el usuario frente a los datos existentes en un almacén de datos externo o una lista de valores permitidos. Si los datos no son válidos, puede pedirle al usuario que proporcione datos válidos o impedir que el usuario continúe con el flujo de registro.
- **Integrarse con un flujo de trabajo de aprobación personalizado**. Conéctese a un sistema de aprobaciones personalizado para administrar y limitar la creación de cuentas.
- **Sobrescribir atributos de usuario**. Asigne un valor a un atributo recopilado del usuario o cámbiele el formato. Por ejemplo, si un usuario escribe el nombre con mayúsculas o minúsculas, se le puede dar un formato en el que solo la primera letra esté en mayúscula. 
- **Realizar la verificación de identidades**. Use un servicio de verificación de identidad para agregar un nivel de seguridad adicional a las decisiones de creación de cuentas.
- **Ejecutar una lógica de negocios personalizada**. Puede desencadenar eventos descendentes en los sistemas de la nube para enviar notificaciones push, actualizar las bases de datos corporativas, administrar permisos, auditar bases de datos y realizar otras acciones personalizadas.

Un conector de API proporciona a Azure Active Directory la información necesaria para llamar a una API, incluida una dirección URL del punto de conexión HTTP y la autenticación. Una vez que configure un conector de API, puede habilitarlo para un paso específico de un flujo de usuario. Cuando un usuario llega a ese paso del flujo de registro, se invoca el conector de API y se materializa como una solicitud HTTP POST a la API, enviando la información de usuario ("notificaciones") como pares de clave y valor en un cuerpo JSON. La respuesta de la API puede afectar a la ejecución del flujo de usuario. Por ejemplo, la respuesta de la API puede impedir que un usuario se registre, pedir al usuario que vuelva a especificar la información, o sobrescribir y anexar atributos de usuario.

## <a name="where-you-can-enable-an-api-connector-in-a-user-flow"></a>Dónde se puede habilitar un conector de API en un flujo de usuario

El conector de API se puede habilitar en dos lugares del flujo de usuario:

- Después de iniciar sesión con un proveedor de identidades
- Antes de crear el usuario

> [!IMPORTANT]
> En ambos casos, los conectores de la API se invocan durante el **registro** del usuario y no en el inicio de sesión.

### <a name="after-signing-in-with-an-identity-provider"></a>Después de iniciar sesión con un proveedor de identidades

Inmediatamente después de que el usuario se autentique con un proveedor de identidades (como Google, Facebook o Azure AD), se invoca un conector de API en este paso del proceso de registro. Este paso precede a la **_página de recopilación de atributos_** , que es el formulario que se muestra al usuario para recopilar los atributos de usuario. Este paso no se invoca si un usuario se registra con una cuenta local. A continuación, se muestran ejemplos de escenarios del conector de API que se pueden habilitar en este paso:

- Use el correo electrónico o la identidad federada que el usuario proporcionó para buscar notificaciones en un sistema existente. Devuelva estas notificaciones del sistema existente, rellene previamente la página de recopilación de atributos y haga que estén disponibles para que se devuelvan en el token.
- Implemente una lista de permitidos o bloqueados según la identidad social.

### <a name="before-creating-the-user"></a>Antes de crear el usuario

Después de la página de recopilación de atributos, se invoca un conector de API en este paso del proceso de registro, si se incluye uno. Este paso siempre se invoca antes de crear una cuenta de usuario. A continuación, se muestran ejemplos de escenarios que se pueden habilitar en este momento del registro:

- Validar datos introducidos por el usuario y pedirle que vuelva a enviarlos.
- Bloquear un registro de usuario en función de los datos especificados por el usuario.
- Realizar la verificación de identidades.
- Consultar en sistemas externos los datos existentes sobre el usuario para devolverlos en el token de aplicación o almacenarlos en Azure AD.


## <a name="next-steps"></a>Pasos siguientes
- Obtener información sobre cómo [agregar un conector de API a un flujo de usuario](add-api-connector.md)
- Comience a trabajar con nuestros [ejemplos](code-samples.md#api-connectors).
<!-- - Learn how to [add a custom approval system to self-service sign-up](add-approvals.md) -->