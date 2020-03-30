---
title: 'Procedimientos recomendados de seguridad: Azure Digital Twins | Microsoft Docs'
description: Aprenda los procedimientos recomendados de seguridad para Azure Digital Twins y el Internet de las cosas.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/15/2020
ms.openlocfilehash: 5fc5ba447557aa89e8f0870c576d6d4c439f3353
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76122566"
---
# <a name="azure-digital-twins-security-best-practices"></a>Procedimientos recomendados de seguridad de Azure Digital Twins

La seguridad de Azure Digital Twins permite el acceso preciso a recursos y acciones específicos del grafo de IoT. Para ello, se usa la administración pormenorizada de roles y permisos que se conoce como [control de acceso basado en rol](./security-role-based-access-control.md).

Azure Digital Twins usa también otras características de seguridad que están presentes en Azure IoT, incluido Azure Active Directory (Azure AD). Por ese motivo, la configuración y protección de aplicaciones compiladas en Azure Digital Twins implica el uso de muchos de los mismos [procedimientos de seguridad de Azure IoT](../iot-fundamentals/iot-security-best-practices.md) actualmente recomendados.

En este artículo se resumen los principales procedimientos recomendados que deben seguirse.

> [!IMPORTANT]
> Para garantizar la máxima seguridad para el espacio de IoT, revise los recursos de seguridad adicionales. Asegúrese de incluir los proveedores de dispositivos.

> [!TIP]
> Use [Azure Security Center para IoT](https://docs.microsoft.com/azure/asc-for-iot/) para ayudar a detectar vulnerabilidades y amenazas de seguridad de IoT.

## <a name="iot-security-best-practices"></a>Procedimientos recomendados para la seguridad de IoT

Algunos de los procedimientos principales para proteger los dispositivos IoT de forma segura son:

> [!div class="checklist"]
> * Proteja cada dispositivo que esté conectado al espacio de IoT con medios a prueba de manipulaciones.
> * Limite el rol de cada dispositivo, sensor y persona dentro del espacio de IoT. Si se pone en riesgo, el efecto será menor.
> * Considere la posibilidad de usar el filtrado de direcciones IP de dispositivo y la restricción de puertos.
> * Limite el ancho de banda de E/S y dispositivo para mejorar el rendimiento. La limitación de velocidad puede mejorar la seguridad al impedir ataques por denegación de servicio.
> * Mantenga actualizado el firmware del dispositivo, el sistema operativo y el software.
> * Audite y revise periódicamente los procedimientos recomendados de seguridad de dispositivos, software, redes y puertas de enlace, ya que siguen mejorando y evolucionando.
> * Use sistemas, software y dispositivos de seguridad de confianza, certificados y compatibles. Por ejemplo, revise [las ofertas de cumplimiento](https://azure.microsoft.com/overview/trusted-cloud/compliance/) para la nube de Azure.

Algunos de los procedimientos más importantes para proteger de forma segura un espacio de IoT son:

> [!div class="checklist"]
> * Cifre los datos persistentes, guardados o almacenados.
> * Exija el cambio o la actualización de claves y contraseñas de forma periódica.
> * Restrinja cuidadosamente el acceso y los permisos por rol. Lea la sección [Procedimientos recomendados del control de acceso basado en rol](#role-based-access-control-best-practices) más abajo.
> * Considere una topología de red dividida para que los dispositivos de cada red estén aislados de los demás.
> * Use cifrado seguro. Exija contraseñas largas, use protocolos seguros y la [autenticación multifactor](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks).

[Supervise](./how-to-configure-monitoring.md) los recursos de IoT para inspeccionar los valores atípicos, las amenazas o los parámetros de recursos que se encuentran fuera del intervalo de funcionamiento habitual. Use Azure Analytics para administrar la supervisión.

> [!IMPORTANT]
> Lea el artículo [Procedimientos recomendados para la seguridad de IoT](../iot-fundamentals/iot-security-best-practices.md) de Azure para comenzar una estrategia completa de seguridad de IoT.

> [!NOTE]
> Para más información sobre el procesamiento y la supervisión de eventos, lea [Enrutamiento de eventos y mensajes con Azure Digital Twins](./concepts-events-routing.md).

## <a name="azure-active-directory-best-practices"></a>Procedimientos recomendados de Azure Active Directory

Azure Digital Twins usa [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/authentication/) para autenticar a los usuarios y proteger las aplicaciones. Azure Active Directory admite la autenticación de diversas arquitecturas modernas. Todas se basan en protocolos estándar del sector como OAuth 2.0 u OpenID Connect. Algunos de los principales procedimientos recomendados para proteger su espacio de IoT en Azure Active Directory son:

> [!div class="checklist"]
> * Almacene secretos y claves de aplicación de Azure Active Directory en una ubicación segura, como [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).
> * Para autenticarse, use un certificado emitido por una [entidad emisora de certificados](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md) de confianza en lugar de secretos de aplicación.
> * Limite el ámbito de acceso de OAuth 2.0 para un token.
> * Compruebe la longitud de tiempo que es válido un token y si un token sigue siendo válido.
> * Establezca periodos de tiempo adecuados durante los cuales los tokens son válidos. Actualice los tokens expirados.
> * Quite los **URI de redirección** y permisos que no se usan, siguiendo los [Procedimientos recomendados del control de acceso basado en roles](#role-based-access-control-best-practices).

## <a name="role-based-access-control-best-practices"></a>Procedimientos recomendados del control de acceso basado en roles

[!INCLUDE [digital-twins-rbac-best-practices](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre los procedimientos recomendados de Azure IoT, lea [Procedimientos recomendados de seguridad de Internet de las cosas](../iot-fundamentals/iot-security-best-practices.md).

* Para información sobre el control de acceso basado en rol, lea [Control de acceso basado en rol](./security-role-based-access-control.md).

* Para información sobre la autenticación, lea [Autenticación con las API](./security-authenticating-apis.md).
