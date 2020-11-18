---
title: Modelo de identidad
titleSuffix: An Azure Communication Services concept
description: Obtenga información sobre las identidades y los tokens de acceso.
author: tomaschladek
manager: nmurav
services: azure-communication-services
ms.author: tchladek
ms.date: 10/26/2020
ms.topic: conceptual
ms.service: azure-communication-services
ms.openlocfilehash: 996f138a14923319381738e7a55cd7ba4e8c4320
ms.sourcegitcommit: 5831eebdecaa68c3e006069b3a00f724bea0875a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94517774"
---
# <a name="identity-model"></a>Modelo de identidad

Azure Communication Services es un servicio independiente de la identidad. Este diseño tiene varias ventajas:
- Se reutilizan las identidades existentes de su sistema de administración de identidades
- Flexibilidad para escenarios de integración
- Sus identidades se mantienen privadas para Azure Communication Services

En lugar de duplicar la información existente en su sistema, mantendrá la relación de asignaciones específica de su caso empresarial. Por ejemplo, la asignación de identidades 1:1, 1:N, N:1 y N:M. Los identificadores externos (como números de teléfono, usuarios, dispositivos, aplicaciones, GUID) no se pueden usar como una identidad de Azure Communication Services. Los tokens de acceso generados para la identidad de Azure Communication Services se usan para acceder a los servicios originarios, como el chat o las llamadas. 

## <a name="identity"></a>Identidad

Las identidades se crean con la biblioteca de administración de Azure Communication Services. La identidad actúa como identificador en las conversaciones y se usa para la creación de tokens de acceso. La misma identidad podría participar en varias sesiones simultáneas en varios dispositivos. La identidad puede tener varios tokens de acceso activos al mismo tiempo. La eliminación de la identidad, el recurso o la suscripción provoca la invalidación de todos sus tokens de acceso y la eliminación de todos los datos almacenados para esta identidad. La identidad eliminada no puede emitir nuevos tokens de acceso ni acceder a los datos previamente almacenados (por ejemplo, mensajes de chat). 

No se le cobrará por el número de identidades que tenga, sino por el uso de los servicios originarios. El número de identidades no debe restringir cómo asignar las identidades de la aplicación a las identidades de Azure Communication Services. Gracias a la libertad de asignación, la responsabilidad recae en los términos de privacidad. Cuando el usuario de la aplicación desea que se le elimine del sistema, debe eliminar todas las identidades que se asociaron a ese usuario.

Azure Communication Services no proporciona identidades especiales para los usuarios anónimos. No mantiene la asignación entre usuarios e identidades, no podría determinar si la identidad es anónima. Puede diseñar el concepto para que se adapte a sus necesidades. Nuestra recomendación es crear una nueva identidad para cada usuario anónimo de la aplicación. Con la posesión del token de acceso válido, cualquier usuario puede acceder al contenido no eliminado de la identidad. Por ejemplo, los mensajes de chat enviados por el usuario. El acceso está restringido solo a determinados ámbitos, que forman parte del token de acceso. Encontrará más información sobre los ámbitos en la sección *Token de acceso*.

### <a name="mapping-of-identities"></a>Asignación de identidades

Azure Communication Services no replica la funcionalidad del IMS. No proporciona una manera para que los clientes usen identidades específicas del cliente. Por ejemplo, el número de teléfono o la dirección de correo electrónico. En su lugar, proporciona identificadores únicos que puede asignar a las identidades de la aplicación. Azure Communication Services no almacena ningún tipo de información que pueda revelar la identidad real de los usuarios.

En lugar de duplicar, se recomienda que diseñe cómo se asignarán los usuarios de su dominio de identidades a las identidades de Azure Communication Services. Puede seguir cualquier tipo de patrón 1:1, 1:N, N:1 o M:N. Puede decidir si un usuario único se asigna a una identidad única o a varias identidades. Cuando se crea una nueva identidad, se recomienda almacenar la asignación de esta identidad al usuario o usuarios de la aplicación. Dado que las identidades requieren los tokens de acceso para el uso de los servicios originarios, la identidad debe ser conocida por el usuario o los usuarios de la aplicación.

Si utiliza una base de datos relacional para el almacenamiento de usuarios, la implementación puede variar en función del escenario de asignación. En escenarios con una asignación 1:1 o N:1, puede agregar la columna *CommunicationServicesId* a la tabla para almacenar la identidad de Azure Communication Services. En escenarios con una relación 1:N o N:M, considere la posibilidad de crear una tabla independiente en la base de datos relacional.

## <a name="access-token"></a>Access token

El token de acceso es un token JWT que se puede usar para acceder a los servicios originarios de Azure Communication Services. El token de acceso emitido tiene protección de integridad y sus notificaciones no se pueden cambiar después de su emisión. Es decir, el cambio manual de propiedades como la identidad, la expiración o los ámbitos hará que el token de acceso no sea válido. El uso de los servicios originarios con tokens invalidados dará lugar a la denegación del acceso al servicio originario. 

Las propiedades del token de acceso son: *identidad, expiración* y *ámbitos*. El token de acceso siempre es válido durante 24 horas. Después de este tiempo, el token de acceso se invalida y no se puede usar para acceder a ningún servicio originario. La identidad debe tener una manera de solicitar un token de acceso nuevo desde el servicio en el servidor. El parámetro *scope* (ámbito) define un conjunto no vacío de servicios originarios que se pueden usar. Azure Communication Services admite los siguientes ámbitos para los tokens de acceso:

|Nombre|Descripción|
|---|---|
|Chat|  Ofrece la posibilidad de participar en un chat|
|VoIP|  Concede la capacidad de llamar a identidades y números de teléfono.|


Si desea revocar el token de acceso antes de su expiración, puede usar la biblioteca de administración de Azure Communication Services para hacerlo. La revocación del token no es inmediata y tarda hasta 15 minutos en propagarse. La eliminación de la identidad, el recurso o la suscripción producirá la revocación de todos los tokens de acceso. Si desea eliminar la capacidad de un usuario para acceder a una funcionalidad específica, debe revocar todos los tokens de acceso. Después, emita un nuevo token de acceso con un conjunto de ámbitos más limitado.
La rotación de las claves de acceso de Azure Communication Services producirá la revocación de todos los tokens de acceso activos creados con la clave de acceso anterior. Todas las identidades perderán el acceso a Azure Communication Services y se les pedirá que emitan nuevos tokens de acceso. 

Se recomienda emitir los tokens de acceso en el servicio del servidor y no en la aplicación del cliente. La razón es que la emisión requiere una clave de acceso o ser una identidad administrada. No se recomienda por razones de seguridad compartir las claves de acceso con la aplicación del cliente. La aplicación cliente debe usar un punto de conexión de servicio de confianza que pueda autenticar a los clientes y emitir el token de acceso en su nombre. [Aquí](./client-and-server-architecture.md) puede encontrar más detalles sobre la arquitectura.

Si almacena en caché los tokens de acceso en una memoria auxiliar, se recomienda usar cifrado. El token de acceso es información confidencial y se puede usar para actividades malintencionadas si no está protegido. Con la posesión del token de acceso, puede inicializar el SDK y acceder a la API. La API accesible solo está restringida en función de los ámbitos que tiene el token de acceso. Se recomienda emitir los tokens de acceso solo con los ámbitos que son necesarios.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener una introducción a la administración de tokens de acceso, consulte [Creación y administración de tokens de acceso](https://docs.microsoft.com/azure/communication-services/quickstarts/access-tokens).
* Para una introducción a la autenticación, consulte [Autenticación en Azure Communication Services](https://docs.microsoft.com/azure/communication-services/concepts/authentication).
* Para una introducción a la residencia y la privacidad de los datos, consulte [Disponibilidad de regiones y residencia de datos](https://docs.microsoft.com/azure/communication-services/concepts/privacy).