---
title: Requisitos previos del aprovisionamiento en la nube de Azure AD Connect en Azure AD
description: En este artículo se describen los requisitos previos y los requisitos de hardware que necesita para el aprovisionamiento en la nube.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 11/16/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8eb8de2424012d12f216f154eb077028a8f82d76
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96173709"
---
# <a name="prerequisites-for-azure-ad-connect-cloud-provisioning"></a>Requisitos previos del aprovisionamiento en la nube de Azure AD Connect
En este artículo se proporcionan instrucciones sobre cómo elegir y usar el aprovisionamiento en la nube de Azure Active Directory (Azure AD) Connect como solución de identidad.

## <a name="cloud-provisioning-agent-requirements"></a>Requisitos del agente de aprovisionamiento en la nube
Se necesita lo siguiente para usar el aprovisionamiento en la nube de Azure AD Connect:

- Credenciales de administrador de dominio o administrador de empresa para crear la gMSA (cuenta de servicio administrada de grupo) de sincronización en la nube de Azure AD Connect para ejecutar el servicio del agente. 
- Una cuenta de administrador de identidades híbridas para su inquilino de Azure AD que no sea un usuario invitado.
- Un servidor local para el agente de aprovisionamiento con Windows 2012 R2 o posterior.  Este servidor debe ser un servidor de nivel 0 basado en el [modelo de nivel administrativo de Active Directory](/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material).
- Configuraciones de firewall locales.

## <a name="group-managed-service-accounts"></a>Cuentas de servicio administradas de grupo
Una cuenta de servicio administradas de grupo es una cuenta de dominio administrado que proporciona administración automática de contraseñas, administración simplificada del nombre de entidad de seguridad de servicio (SPN), la posibilidad de delegar la administración a otros administradores y, además, amplía esta funcionalidad a varios servidores.  La sincronización en la nube de Azure AD Connect admite y usa una gMSA para ejecutar el agente.  Para poder crear esta cuenta, se le pedirán credenciales administrativas durante la instalación.  La cuenta aparecerá como (domain\provAgentgMSA$).  Para obtener más información sobre gMSA, consulte [Cuentas de servicio administradas de grupo](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview). 

### <a name="prerequisites-for-gmsa"></a>Requisitos previos de gMSA:
1.  El esquema de Active Directory del bosque del dominio de la gMSA se tiene que actualizar a Windows Server 2012.
2.  [Módulos de RSAT de PowerShell](/windows-server/remote/remote-server-administration-tools) en un controlador de dominio
3.  Al menos un controlador de dominio en el dominio debe ejecutar Windows Server 2012.
4.  Un servidor unido a un dominio en el que se está instalando el agente debe ser Windows Server 2012 o posterior.

Para conocer los pasos sobre cómo actualizar un agente existente para usar una cuenta gMSA, consulte [Cuentas de servicio administradas de grupo](how-to-install.md#group-managed-service-accounts).

### <a name="in-the-azure-active-directory-admin-center"></a>En el Centro de administración de Azure Active Directory

1. Cree una cuenta de administrador de identidades híbridas exclusiva para la nube en su inquilino de Azure AD. De esta manera, puede administrar la configuración del inquilino en caso de que los servicios locales fallen o no estén disponibles. Descubra cómo [agregar una cuenta de administrador de identidades híbridas exclusiva para la nube](../fundamentals/add-users-azure-active-directory.md). La finalización de este paso es esencial para garantizar que no queda bloqueado fuera de su inquilino.
1. Agregue uno o varios [nombres de dominio personalizados](../fundamentals/add-custom-domain.md) al inquilino de Azure AD. Los usuarios pueden iniciar sesión con uno de estos nombres de dominio.

### <a name="in-your-directory-in-active-directory"></a>En su directorio de Azure Active Directory

Ejecute la [herramienta IdFix](/office365/enterprise/prepare-directory-attributes-for-synch-with-idfix) para preparar los atributos del directorio para la sincronización.

### <a name="in-your-on-premises-environment"></a>En el entorno local

1. Identifique un servidor host unido a un dominio en el que se ejecuta Windows Server 2012 R2 o superior con un mínimo de 4 GB de RAM y un entorno de ejecución .NET 4.7.1 o posterior.

1. La directiva de ejecución de PowerShell en el servidor local debe establecerse en Undefined o RemoteSigned.

1. Si hay un firewall entre los servidores y Azure AD, configure los elementos siguientes:
   - Asegúrese de que los agentes pueden realizar solicitudes *de salida* a Azure AD a través de los puertos siguientes:

        | Número de puerto | Cómo se usa |
        | --- | --- |
        | **80** | Descarga las listas de revocación de certificados (CRL) al validar el certificado TLS/SSL  |
        | **443** | Controla toda la comunicación saliente con el servicio. |
        |**8082**|Obligatorio para la instalación y si quiere configurar la API de administración de HIS.  Este puerto se puede quitar una vez instalado el agente y si no está planeando usar la API.   |
        | **8080** (opcional) | Si el puerto 443 no está disponible, los agentes notifican su estado cada 10 minutos en el puerto 8080. Este estado se muestra en el portal de Azure AD. |
   
     
   - Si el firewall fuerza las reglas según los usuarios que las originan, abra estos puertos para el tráfico de servicios de Windows que se ejecutan como un servicio de red.
   - Si el firewall o proxy le permite configurar sufijos seguros, agregue conexiones a \*.msappproxy.net y \*.servicebus.windows.net. En caso contrario, permita el acceso a los [intervalos de direcciones IP del centro de datos de Azure](https://www.microsoft.com/download/details.aspx?id=41653), que se actualizan cada semana.
   - Los agentes necesitan acceder a login.windows.net y login.microsoftonline.com para el registro inicial. Abra el firewall también para esas direcciones URL.
   - Para la validación de certificados, desbloquee las siguientes direcciones URL: mscrl.microsoft.com:80, crl.microsoft.com:80, ocsp.msocsp.com:80 y www\.microsoft.com:80. Estas direcciones URL se utilizan para la validación de certificados con otros productos de Microsoft, por lo que es posible que estas direcciones URL estén bloqueadas.

>[!NOTE]
> La instalación del agente de aprovisionamiento en la nube en Windows Server Core no se admite.




### <a name="additional-requirements"></a>Requisitos adicionales
- [Microsoft .NET Framework 4.7.1](https://www.microsoft.com/download/details.aspx?id=56116) 

#### <a name="tls-requirements"></a>Requisitos de TLS

>[!NOTE]
>La seguridad de la capa de transporte (TLS) es un protocolo que proporciona comunicaciones seguras. Si se cambia la configuración de TLS, todo el bosque se verá afectado. Para más información, consulte [Actualización para habilitar TLS 1.1 y TLS 1.2 como protocolos seguros predeterminados de WinHTTP en Windows](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-default-secure-protocols-in-wi).

El servidor de Windows que hospeda el agente de aprovisionamiento en la nube de Azure AD Connect debe tener TLS 1.2 habilitado antes de instalarlo.

Para habilitar TLS 1.2, siga estos pasos.

1. Establezca las siguientes claves del Registro:
    
    ```
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001
    ```

1. Reinicie el servidor.




## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento?](what-is-provisioning.md)
- [¿Qué es el aprovisionamiento en la nube de Azure AD Connect?](what-is-cloud-provisioning.md)