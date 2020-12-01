---
title: 'Configuración del servidor MFA: Azure Active Directory'
description: Aprenda a configurar las opciones del servidor Azure MFA en el Azure Portal
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 06/05/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3da7c3afa6b414e3d7df3bb58766d1ab7657dbc4
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94838968"
---
# <a name="configure-mfa-server-settings"></a>Configuración del servidor MFA

Este artículo le ayuda a administrar la configuración del servidor Azure MFA en Azure Portal.

> [!IMPORTANT]
> A partir del 1 de julio de 2019, Microsoft ya no ofrecerá el Servidor MFA para nuevas implementaciones. Los nuevos clientes que quieran exigir la autenticación multifactor a sus usuarios deberán usar Azure AD Multi-Factor Authentication basado en la nube. Los clientes existentes que hayan activado el Servidor MFA antes del 1 de julio podrán descargar la versión más reciente y las actualizaciones futuras, así como generar credenciales de activación como de costumbre.

Están disponibles las siguientes opciones del servidor MFA:

| Característica | Descripción |
| ------- | ----------- |
| Configuración del servidor | Descarga el Servidor MFA y genera credenciales de activación para inicializar el entorno. |
| [Omisión por única vez](#one-time-bypass) | Permite que un usuario se autentique sin necesidad de realizar la autenticación multifactor por un tiempo limitado. |
| [Reglas de caché](#caching-rules) |  El almacenamiento en caché se utiliza principalmente cuando sistemas locales, como VPN, envían varias solicitudes de comprobación mientras la primera solicitud aún está en curso. Esta característica permite que las solicitudes posteriores se realicen correctamente de forma automática después de que el usuario lleve a cabo correctamente la primera comprobación en curso. |
| Estado del servidor | Permite ver el estado de los servidores MFA locales, lo que incluye la versión, el estado, la dirección IP y la última fecha y hora de comunicación. |

## <a name="one-time-bypass"></a>Omisión por única vez

La característica de omisión por única vez permite a un usuario autenticarse una sola vez y, por consiguiente, no realizar la autenticación multifactor. La omisión es temporal y expira una vez que ha pasado el número especificado de segundos. En las situaciones en las que la aplicación móvil o el teléfono no reciben una notificación o una llamada, puede habilitar una omisión por única vez para que el usuario pueda acceder al recurso deseado.

Para crear una omisión por única vez, complete los pasos siguientes:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador.
1. Busque y seleccione **Azure Active Directory**, a continuación, vaya a **seguridad** > **MFA** > **Omisión por única vez**.
1. Seleccione **Agregar**.
1. Si es necesario, seleccione el grupo de replicación para esta omisión.
1. Escriba el nombre de usuario de esta manera: `username\@domain.com`. Escriba el número de segundos que debe durar la omisión y el motivo de esta.
1. Seleccione **Agregar**. El límite de tiempo entra en vigor inmediatamente. El usuario tiene que iniciar sesión antes de que expire la omisión por única vez.

También puede ver el informe de omisión por única vez desde esta misma ventana.

## <a name="caching-rules"></a>Reglas de caché

Puede establecer un período de tiempo para permitir intentos de autenticación cuando se autentica un usuario mediante el uso de la característica _almacenamiento en caché_. Los intentos de autenticación posteriores para el usuario dentro del período de tiempo especificado se realizan correctamente de forma automática.

El almacenamiento en caché se utiliza principalmente cuando sistemas locales, como VPN, envían varias solicitudes de comprobación mientras la primera solicitud aún está en curso. Esta característica permite que las solicitudes posteriores se realicen correctamente de forma automática después de que el usuario lleve a cabo correctamente la primera comprobación en curso.

>[!NOTE]
> La característica de almacenamiento en caché no está destinada a utilizarse para inicios de sesión en Azure Active Directory (Azure AD).

Para establecer el almacenamiento en caché, complete los pasos siguientes:

1. Vaya a **Azure Active Directory** > **Seguridad** > **MFA** > **reglas de almacenamiento en caché**.
1. Seleccione **Agregar**.
1. Seleccione un **tipo de caché** de la lista desplegable. Escriba el número máximo de **segundos de caché**.
1. Si es necesario, seleccione un tipo de autenticación y especifique una aplicación.
1. Seleccione **Agregar**.

## <a name="next-steps"></a>Pasos siguientes

Las opciones de configuración adicionales del servidor MFA están disponibles en la consola web del propio servidor MFA. También puede [configurar el servidor Azure MFA para conseguir una mayor disponibilidad](howto-mfaserver-deploy-ha.md).
