---
title: 'Azure AD Connect: Cuenta de servicio de ADSync | Microsoft Docs'
description: En este tema se describe la cuenta de servicio de ADSync y se proporcionan procedimientos recomendados relacionados con la cuenta.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 875c503a9959565d76d46902b5ecb386995ef1e5
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86144717"
---
# <a name="adsync-service-account"></a>Cuenta del servicio ADSync
Azure AD Connect instala un servicio local que coordina la sincronización entre Active Directory y Azure Active Directory.  El servicio de sincronización de Microsoft Azure AD Sync (ADSync) se ejecuta en un servidor en su entorno local.  Las credenciales del servicio se establecen de forma predeterminada en las instalaciones rápidas, pero se pueden personalizar para satisfacer los requisitos de seguridad de su organización.  Estas credenciales no se usan para conectarse a los bosques locales ni a Azure Active Directory.

Elegir la cuenta de servicio de ADSync es una decisión importante de planificación que debe tomarse antes de instalar Azure AD Connect.  Cualquier intento de cambiar las credenciales tras la instalación conllevará que el servicio no se pueda iniciar, que se pierda el acceso a la base de datos de sincronización y que no se pueda autenticar con sus directorios conectados (Azure y AD DS).  No se producirá ninguna sincronización hasta que se restauren las credenciales originales.

## <a name="the-default-adsync-service-account"></a>La cuenta de servicio de ADSync predeterminada

Cuando se ejecuta en un servidor miembro, el servicio AdSync se ejecuta en el contexto de una cuenta de servicio virtual (VSA).  Debido a una limitación del producto, se crea una cuenta de servicio personalizada cuando se instala en un controlador de dominio.  Si la cuenta de servicio de configuración rápida no cumple los requisitos de seguridad de la organización, implemente Azure AD Connect eligiendo la opción de personalización.  A continuación, elija la opción de cuenta de servicio que cumpla los requisitos de su organización.

>[!NOTE]
>La cuenta de servicio predeterminada cuando se instala en un controlador de dominio tiene el formato Domain\AAD_InstallationIdentifier.  La contraseña para esta cuenta se genera aleatoriamente y presenta importantes desafíos para la rotación y la recuperación de contraseñas.  Microsoft recomienda la personalización de la cuenta de servicio durante la instalación inicial en un controlador de dominio para usar una cuenta de servicio administrada independiente o de grupo (sMSA / gMSA)

|Ubicación de Azure AD Connect|Cuenta de servicio creada|
|-----|-----|
|Servidor miembro|NT SERVICE\ADSync|
|Controlador de dominio|Domain\AAD_74dc30c01e80 (vea la nota)|

## <a name="custom-adsync-service-accounts"></a>Cuentas de servicio de ADSync personalizadas
Microsoft recomienda ejecutar el servicio ADSync en el contexto de una cuenta de servicio virtual o una cuenta de servicio administrada independiente o de grupo.  El administrador del dominio también puede crear una cuenta de servicio aprovisionada para satisfacer los requisitos de seguridad concretos de la organización.   Para personalizar la cuenta de servicio usada durante la instalación, elija la opción Personalizar en la página Configuración rápida a continuación.   Están disponibles las siguientes opciones:

- cuenta predeterminada: Azure AD Connect aprovisionará la cuenta de servicio como se describió anteriormente
- cuenta de servicio administrada: use una MSA independiente o de grupo aprovisionada por el administrador
- cuenta de dominio: use una cuenta de servicio del dominio aprovisionada por el administrador

![Captura de pantalla de la página Configuración de Azure AD Connect Express con los botones de opción "Personalizar" o "Usar la configuración rápida".](media/concept-adsync-service-account/adsync1.png)

![Captura de pantalla de la página "Instalar componentes necesarios" de Azure AD Connect con la opción para usar una cuenta de servicio administrada existente seleccionada.](media/concept-adsync-service-account/adsync2.png)

## <a name="diagnosing-adsync-service-account-changes"></a>Diagnóstico de los cambios en la cuenta de servicio de ADSync
Cambiar las credenciales del servicio AdSync tras la instalación conllevará que el servicio no se pueda iniciar, que se pierda el acceso a la base de datos de sincronización y que no se pueda autenticar con sus directorios conectados (Azure y AD DS).  Conceder acceso a la base de datos a la nueva cuenta de servicio de ADSync no es suficiente para recuperarse de este problema. No se producirá ninguna sincronización hasta que se restauren las credenciales originales.

El servicio ADSync emitirá un error al nivel de mensaje en el registro de eventos cuando no se pueda iniciar.  El contenido del mensaje variará dependiendo de si se está usando la base de datos integrada (localdb) o SQL completa.  A continuación se muestran ejemplos de las entradas del registro de eventos que pueden estar presentes.

### <a name="example-1"></a>Ejemplo 1

Las claves de cifrado del servicio AdSync no se han encontrado y se han vuelto a crear.  No se producirá ninguna sincronización hasta que se corrija este problema.

Solución del problema Las claves de cifrado de Microsoft Azure AD Sync dejarán de estar accesibles si se cambian las credenciales de inicio de sesión del servicio AdSync.  Si se han cambiado las credenciales, utilice la aplicación Servicios para volver a cambiar la cuenta de inicio de sesión al valor configurado en un principio (p. ej. NT SERVICE\AdSync) y reinicie el servicio.  De esta forma, se restaurará de inmediato el funcionamiento correcto del servicio AdSync.

Consulte el siguiente [artículo](https://go.microsoft.com/fwlink/?linkid=2086764) para obtener más información.

### <a name="example-2"></a>Ejemplo 2

El servicio no pudo iniciarse porque no se pudo establecer una conexión a la base de datos local (localdb).

Solución de este problema El servicio Microsoft Azure AD Sync perderá el permiso para acceder al proveedor de bases de datos local si cambian las credenciales de inicio de sesión del servicio AdSync.  Si se han cambiado las credenciales, utilice la aplicación Servicios para volver a cambiar la cuenta de inicio de sesión al valor configurado en un principio (p. ej. NT SERVICE\AdSync) y reinicie el servicio.  De esta forma, se restaurará de inmediato el funcionamiento correcto del servicio AdSync.

Consulte el siguiente [artículo](https://go.microsoft.com/fwlink/?linkid=2086764) para obtener más información.

Detalles adicionales El proveedor ha devuelto la información de error siguiente:
 

``` 
OriginalError=0x80004005 OLEDB Provider error(s): 
Description  = 'Login timeout expired'
Failure Code = 0x80004005
Minor Number = 0 
Description  = 'A network-related or instance-specific error has occurred while establishing a connection to SQL Server. Server is not found or not accessible. Check if instance name is correct and if SQL Server is configured to allow remote connections. For more information see SQL Server Books Online.'
```
## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre la [Integración de las identidades locales con Azure Active Directory](whatis-hybrid-identity.md).
