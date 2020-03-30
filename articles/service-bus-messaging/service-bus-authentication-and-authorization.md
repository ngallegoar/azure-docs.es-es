---
title: Autenticación y autorización de Azure Service Bus | Microsoft Docs
description: Autentique aplicaciones en Service Bus con la autenticación de firma de acceso compartido (SAS).
services: service-bus-messaging
documentationcenter: na
author: axisc
editor: spelluru
ms.assetid: 18bad0ed-1cee-4a5c-a377-facc4785c8c9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2019
ms.author: aschhab
ms.openlocfilehash: 7234e33c04e742c77630f8d87481c7831fb00bf2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "70013241"
---
# <a name="service-bus-authentication-and-authorization"></a>Autenticación y autorización de Service Bus

Las aplicaciones tienen acceso a los recursos de Azure Service Bus mediante la autenticación con token de firma de acceso compartido (SAS). Con SAS, las aplicaciones presentan a Service Bus un token que se ha firmado con una clave simétrica que tanto el emisor del token como Service Bus conocen ( es decir "compartida") y esa clave está directamente asociada a una regla que concede derechos de acceso específicos, como el permiso para recibir y escuchar o enviar mensajes. Las reglas de SAS se configuran en el espacio de nombres o directamente en entidades como una cola o tema, lo que permite un control de acceso específico.

Los token de SAS los puede generar un cliente de Service Bus directamente o un punto de conexión de emisión de token intermedio con el que el cliente interactúa. Por ejemplo, un sistema puede requerir que el cliente llame a un punto de conexión de servicio web protegido con autorización de Active Directory para demostrar sus derechos de acceso al sistema e identidad y el servicio web, a continuación, devuelve el token de Service Bus adecuado. Este token de SAS se puede generar fácilmente utilizando el proveedor de token de Service Bus incluido en el SDK de Azure. 

> [!IMPORTANT]
> Si utiliza Azure Active Directory Access Control (también conocido como Access Control Service o ACS) junto con Service Bus, tenga en cuenta que la compatibilidad con este método está limitada y debe migrar la aplicación para usar SAS. Para más información, vea [esta publicación del blog](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/) y este [artículo](service-bus-migrate-acs-sas.md).

## <a name="azure-active-directory"></a>Azure Active Directory
La integración de Azure Active Directory (Azure AD) para recursos de Services Bus proporciona control de acceso basado en rol (RBAC) para el control específico de acceso de los clientes a los recursos. Puede usar el control de acceso basado en rol (RBAC) para conceder permisos a una entidad de seguridad, que puede ser un usuario, un grupo o una entidad de servicio de aplicación. Azure AD autentica la entidad de seguridad para devolver un token de OAuth 2.0. El token se puede usar para autorizar una solicitud de acceso a un recurso de Service Bus (cola, tema, etc.).

Consulte los siguientes artículos para más información sobre la autenticación con Azure AD:

- [Autenticación con identidades administradas](service-bus-managed-service-identity.md)
- [Autenticación desde una aplicación](authenticate-application.md)

> [!IMPORTANT]
> La autorización de usuarios o aplicaciones mediante un token de OAuth 2.0 devuelto por Azure AD proporciona una seguridad superior y facilidad de uso sobre las firmas de acceso compartido (SAS). Con Azure AD, no hay ninguna necesidad de almacenar los tokens en su código y arriesgarse a posibles vulnerabilidades de seguridad. Se recomienda usar Azure AD con las aplicaciones de Azure Service Bus siempre que sea posible. 


## <a name="shared-access-signature"></a>Firma de acceso compartido
La [autenticación de SAS](service-bus-sas.md) le permite conceder acceso a un usuario a los recursos de Service Bus con derechos específicos. La autenticación SAS en Service Bus implica la configuración de una clave criptográfica con derechos asociados en un recurso de Service Bus. Los clientes pueden obtener acceso a ese recurso presentando un token SAS, que consta del URI del recurso al que se tiene acceso y una fecha de expiración firmada con la clave configurada.

Puede configurar claves para SAS en un espacio de nombres de Service Bus. La clave se aplica a todas las entidades de mensajes de ese espacio de nombres. También puede configurar claves en temas y colas de Service Bus. SAS también es compatible con [Azure Relay](../service-bus-relay/relay-authentication-and-authorization.md).

Para usar SAS, puede configurar un objeto [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) en un espacio de nombres, cola o tema. Esta regla está formada por los siguientes elementos:

* *KeyName*: identifica la regla.
* *PrimaryKey*: es una clave criptográfica usada para firmar o validar tokens SAS.
* *SecondaryKey*: es una clave criptográfica usada para firmar o validar tokens SAS.
* *Rights*: representa la recopilación de derechos de **escucha**, **envío** o **administración** concedidos.

Las reglas de autorización configuradas en el nivel de espacio de nombres pueden conceder acceso a todas las entidades de un espacio de nombres a los clientes con tokens firmados con la clave correspondiente. Se puede configurar un máximo de 12 reglas de autorización en un espacio de nombres, cola o tema de Service Bus. De forma predeterminada, se configura un objeto [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) con todos los derechos para cada espacio de nombres cuando se aprovisiona por primera vez.

Para obtener acceso a una entidad, el cliente requiere un token SAS generado con un objeto [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule)determinado. El token SAS se genera mediante el HMAC-SHA256 de una cadena de recurso que consta del URI del recurso al que se notifica el acceso y una fecha de expiración con una clave criptográfica asociada a la regla de autorización.

La compatibilidad de la autenticación de SAS con Service Bus se incluye en el SDK .NET de Azure 2.0 y versiones posteriores. SAS incluye compatibilidad con un objeto [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). Todas las API que aceptan una cadena de conexión como parámetro incluyen compatibilidad con cadenas de conexión SAS.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre SAS, siga consultando [Autenticación en Service Bus con Firmas de acceso compartido](service-bus-sas.md).
- Cómo [migrar desde Azure Active Directory Access Control Service a la autorización de Firma de acceso compartido](service-bus-migrate-acs-sas.md).
- [Cambios en los espacios de nombres habilitados para ACS](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/).
- Para obtener la información correspondiente a la autenticación y autorización de Azure Relay, consulte [Azure Relay authentication and authorization](../service-bus-relay/relay-authentication-and-authorization.md) (Autenticación y autorización de Azure Relay). 

