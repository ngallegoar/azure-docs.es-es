---
title: Residencia de los datos de Multi-Factor Authentication de Azure AD
description: Conozca los datos personales y de la organización que almacena Multi-Factor Authentication de Azure AD relativos a usted y sus usuarios, y los datos que permanecen en el país o región de origen.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 09/24/2020
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: inbarc
ms.collection: M365-identity-device-management
ms.openlocfilehash: 40751b7864cc62d17836ab6d57cd106f6904f64a
ms.sourcegitcommit: ad83be10e9e910fd4853965661c5edc7bb7b1f7c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2020
ms.locfileid: "96744115"
---
# <a name="data-residency-and-customer-data-for-azure-ad-multi-factor-authentication"></a>Residencia de datos y datos de cliente en Multi-Factor Authentication de Azure AD

Azure AD almacena los datos de cliente en una ubicación geográfica en función de la dirección que proporcione la organización al suscribirse a un servicio de Microsoft Online, como Microsoft 365 y Azure. Para más información sobre dónde se almacenan los datos de cliente, puede consultar la sección [Dónde se encuentran tus datos](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) de Microsoft Trust Center.

Multi-Factor Authentication de Azure AD basado en la nube y el Servidor Azure Multi-Factor Authentication procesan y almacenan algunos datos personales y de la organización. En este artículo se describe qué datos se almacenan y dónde.

El servicio Multi-Factor Authentication de Azure AD tiene centros de datos en Estados Unidos, Europa y Asia Pacífico. Las siguientes actividades se originan fuera de los centros de datos regionales, excepto donde se indica:

* La autenticación multifactor que usa llamadas telefónicas procede de centros de datos de EE. UU. y se enruta mediante proveedores internacionales.
* Las solicitudes de autenticación de usuario de uso general de otras regiones, como Europa o Australia, se procesan actualmente en función de la ubicación del usuario.
* Las notificaciones de inserción que usan la aplicación Microsoft Authenticator se procesan actualmente en los centros de datos regionales en función de la ubicación del usuario.
    * Los servicios específicos del fabricante del dispositivo, como las notificaciones de inserción de Apple, pueden estar fuera de la ubicación del usuario.

## <a name="personal-data-stored-by-azure-ad-multi-factor-authentication"></a>Datos personales almacenados por Multi-Factor Authentication de Azure AD

Los datos personales son información de nivel de usuario concerniente a una persona específica. Los siguientes almacenes de datos contienen información personal:

* Usuarios bloqueados
* Usuarios omitidos
* Solicitudes de cambio de token de dispositivo de Microsoft Authenticator
* Informes de actividad de Multi-Factor Authentication
* Activaciones de Microsoft Authenticator

Esta información se conserva durante 90 días.

Multi-Factor Authentication de Azure AD no registra datos personales, como el nombre de usuario, el número de teléfono o la dirección IP, pero existe un elemento *UserObjectId* que identifica los intentos de autenticación multifactor de los usuarios. Los datos de registro se almacenan durante 30 días.

### <a name="azure-ad-multi-factor-authentication"></a>Azure AD Multi-Factor Authentication

En el caso de las nubes públicas de Azure (salvo la autenticación de Azure B2C, la extensión NPS y el adaptador de AD FS de Windows Server 2016 o 2019), se almacenan los siguientes datos personales:

| Tipo de evento                           | Tipo de almacén de datos |
|--------------------------------------|-----------------|
| Token OATH                           | En registros de Azure Multi-Factor Authentication     |
| SMS unidireccional                          | En registros de Azure Multi-Factor Authentication     |
| Llamada de voz                           | En registros de Azure Multi-Factor Authentication<br />Almacén de datos de informes de actividad de Multi-Factor Authentication<br />Usuarios bloqueados si se notificó un fraude |
| Notificación de Microsoft Authenticator | En registros de Azure Multi-Factor Authentication<br />Almacén de datos de informes de actividad de Multi-Factor Authentication<br />Usuarios bloqueados si se notificó un fraude<br />Solicitudes de cambio cuando el token de dispositivo de Microsoft Authenticator cambia |

> [!NOTE]
> El almacén de datos de informes de actividad de Multi-Factor Authentication de todas las nubes se almacena en EE. UU., independientemente de la región que procese la solicitud de autenticación. Microsoft Azure Alemania, Microsoft Azure ofrecido por 21Vianet y Microsoft Government Cloud tienen sus propios almacenes de datos independientes de los almacenes de datos de región de nube públicos, si bien estos datos se almacenan siempre en EE. UU.

Respecto a Microsoft Azure Government, Microsoft Azure Alemania, Microsoft Azure ofrecido por 21Vianet, la autenticación de Azure B2C, la extensión NPS y el adaptador de AD FS de Windows Server 2016 o 2019, se almacenan los siguientes datos personales:

| Tipo de evento                           | Tipo de almacén de datos |
|--------------------------------------|-----------------|
| Token OATH                           | En registros de Azure Multi-Factor Authentication<br />Almacén de datos de informes de actividad de Multi-Factor Authentication |
| SMS unidireccional                          | En registros de Azure Multi-Factor Authentication<br />Almacén de datos de informes de actividad de Multi-Factor Authentication |
| Llamada de voz                           | En registros de Azure Multi-Factor Authentication<br />Almacén de datos de informes de actividad de Multi-Factor Authentication<br />Usuarios bloqueados si se notificó un fraude |
| Notificación de Microsoft Authenticator | En registros de Azure Multi-Factor Authentication<br />Almacén de datos de informes de actividad de Multi-Factor Authentication<br />Usuarios bloqueados si se notificó un fraude<br />Solicitudes de cambio cuando el token de dispositivo de Microsoft Authenticator cambia |

### <a name="multi-factor-authentication-server"></a>Servidor Multi-Factor Authentication

Si implementa y ejecuta el Servidor Azure Multi-Factor Authentication, se almacenan los siguientes datos personales:

> [!IMPORTANT]
> A partir del 1 de julio de 2019, Microsoft dejará de ofrecer el Servidor Multi-Factor Authentication en las implementaciones nuevas. Los nuevos clientes que quieran exigir la autenticación multifactor a sus usuarios deberán usar Multi-Factor Authentication de Azure AD basado en la nube. Los clientes existentes que hayan activado el Servidor Multi-Factor Authentication antes del 1 de julio podrán descargar la versión más reciente y las actualizaciones futuras, así como generar credenciales de activación como de costumbre.

| Tipo de evento                           | Tipo de almacén de datos |
|--------------------------------------|-----------------|
| Token OATH                           | En registros de Azure Multi-Factor Authentication<br />Almacén de datos de informes de actividad de Multi-Factor Authentication |
| SMS unidireccional                          | En registros de Azure Multi-Factor Authentication<br />Almacén de datos de informes de actividad de Multi-Factor Authentication |
| Llamada de voz                           | En registros de Azure Multi-Factor Authentication<br />Almacén de datos de informes de actividad de Multi-Factor Authentication<br />Usuarios bloqueados si se notificó un fraude |
| Notificación de Microsoft Authenticator | En registros de Azure Multi-Factor Authentication<br />Almacén de datos de informes de actividad de Multi-Factor Authentication<br />Usuarios bloqueados si se notificó un fraude<br />Solicitudes de cambio cuando el token de dispositivo de Microsoft Authenticator cambia |

## <a name="organizational-data-stored-by-azure-ad-multi-factor-authentication"></a>Datos de la organización almacenados por Multi-Factor Authentication de Azure AD

Los datos de la organización consisten en información de inquilino que podría exponer una opción de configuración o cómo está configurado el entorno. La configuración de inquilino de las siguientes páginas de Multi-Factor Authentication de Azure Portal puede almacenar datos de la organización, como los umbrales de bloqueo o información que identifica al autor de llamada de las solicitudes de autenticación telefónica entrantes:

* Bloqueo de cuenta
* Alerta de fraude
* Notificaciones
* Configuración de la llamada telefónica

En el caso del Servidor Azure Multi-Factor Authentication, las siguientes páginas de Azure Portal pueden contener datos de la organización:

* Configuración del servidor
* Omisión por única vez
* Reglas de caché
* Estado del Servidor Multi-Factor Authentication

## <a name="log-data-location"></a>Ubicación de los datos de registro

La ubicación en la que se almacena la información de registro depende de la región en la que esta se procesa. La mayoría de las zonas geográficas tienen funcionalidades nativas de Multi-Factor Authentication de Azure AD, por lo que los datos de registro se almacenan en la misma región que procesa la solicitud de Multi-Factor Authentication. En aquellas zonas geográficas sin compatibilidad nativa con Multi-Factor Authentication de Azure AD, las zonas geográficas de Estados Unidos o Europa les proporcionan esta funcionalidad, y los datos de registro se almacenan en la misma región que procesa la solicitud de Multi-Factor Authentication.

Algunos datos de registro de autenticación esenciales solo se almacenan en EE. UU. Microsoft Azure Alemania y Microsoft Azure ofrecido por 21Vianet siempre se almacenan en sus nubes correspondientes, mientras que los datos de registro de Microsoft Government Cloud lo hacen siempre en EE. UU.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre qué datos de los usuarios recopilan Multi-Factor Authentication de Azure AD basado en la nube y el Servidor Azure Multi-Factor Authentication, consulte [Recopilación de datos de usuario de Multi-Factor Authentication de Azure AD](howto-mfa-reporting-datacollection.md).
