---
title: 'Azure AD Connect: Requisitos previos y hardware | Microsoft Docs'
description: En este tema se describen los requisitos previos y los requisitos de hardware de Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 91b88fda-bca6-49a8-898f-8d906a661f07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 02/27/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2bcf7b5b8791b813a28133d8a662d1736aacf35a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85358725"
---
# <a name="prerequisites-for-azure-ad-connect"></a>Requisitos previos de Azure AD Connect
En este tema se describen los requisitos previos y los de hardware de Azure AD Connect.

## <a name="before-you-install-azure-ad-connect"></a>Antes de instalar Azure AD Connect
Antes de instalar Azure AD Connect, hay algunas cosas que necesita.

### <a name="azure-ad"></a>Azure AD
* Un inquilino de Azure AD. Obtiene uno con [una prueba gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/). Puede usar uno de los siguientes portales para administrar Azure AD Connect:
  * [Azure Portal](https://portal.azure.com)
  * [Portal de Office](https://portal.office.com)  
* [Agregue y compruebe el dominio](../active-directory-domains-add-azure-portal.md) que pretende usar en Azure AD. Por ejemplo, si tiene previsto usar contoso.com para los usuarios, asegúrese de que este dominio se ha comprobado y no usa solamente el dominio predeterminado contoso.onmicrosoft.com.
* Un inquilino de Azure AD permite de forma predeterminada objetos de 50 k. Al comprobar el dominio, el límite se incrementa a 300 000 objetos. Si todavía necesita más objetos en Azure AD, tiene que abrir una incidencia para aumentar el límite aún más. Si necesita más de 500 000 objetos, necesitará una licencia como Office 365, Azure AD Basic, Azure AD Premium o Enterprise Mobility and Security.

### <a name="prepare-your-on-premises-data"></a>Preparación de los datos locales
* Use [IdFix](https://support.office.com/article/Install-and-run-the-Office-365-IdFix-tool-f4bd2439-3e41-4169-99f6-3fabdfa326ac) para identificar errores tales como duplicados y problemas de formato en el directorio antes de sincronizar con Azure AD y Office 365.
* Revise [las características de sincronización opcionales que se pueden habilitar en Azure AD](how-to-connect-syncservice-features.md) y valore cuáles son las que debe habilitar.

### <a name="on-premises-active-directory"></a>Active Directory local
* La versión del esquema de AD y el nivel funcional del bosque deben ser Windows Server 2003 o una versión posterior. Los controladores de dominio pueden ejecutar cualquier versión siempre que se cumplan los requisitos de nivel de bosque y esquema.
* Si pretende usar la característica de **escritura diferida de contraseñas**, los controladores de dominio deben estar en Windows Server 2008 R2 o versiones posteriores.
* El controlador de dominio usado por Azure AD debe ser grabable. **No se admite** el uso de un RODC (controlador de dominio de solo lectura) y Azure AD Connect no sigue las redirecciones de escritura.
* **No se admite** el uso de bosques o dominios con locales con nombres de NetBios con puntos (el nombre contiene un punto ".").
* Se recomienda [habilitar la papelera de reciclaje de Active Directory](how-to-connect-sync-recycle-bin.md).

### <a name="azure-ad-connect-server"></a>Servidor de Azure AD Connect
>[!IMPORTANT]
>El servidor de Azure AD Connect contiene datos de identidad críticos y debería tratarse como un componente de nivel 0, tal como se documenta en el [modelo de nivel administrativo de Active Directory](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material).

* Azure AD Connect no puede instalarse en Small Business Server o Windows Server Essentials antes de 2019 (se admite Windows Server Essentials 2019). El servidor debe usar Windows Server estándar o una versión superior.
* No se recomienda instalar Azure AD Connect en un controlador de dominio debido a las prácticas de seguridad y una configuración más restrictiva que puede impedir que se instale correctamente Azure AD Connect.
* El servidor de Azure AD Connect debe tener una GUI completa instalada. **No se admite** la instalación en Server Core.
>[!IMPORTANT]
>No se admite la instalación de Azure AD Connect en Small Business Server, Server Essentials o Core Server.

* Azure AD Connect debe estar instalado en Windows Server 2012 o en una versión superior. Este servidor debe estar unido a un dominio y puede ser un controlador de dominio o un servidor miembro.
* El servidor de Azure AD Connect no debe tener la directiva de grupo de transcripción de PowerShell habilitada si usa el Asistente de Azure AD Connect para administrar la configuración de ADFS. Puede habilitar la transcripción de PowerShell si usa el asistente de Azure AD Connect para administrar la configuración de sincronización.
* Si se implementa Servicios de federación de Active Directory, los servidores en los que se instale AD FS o el Proxy de aplicación web deben ser Windows Server 2012 R2 o versiones posteriores. [Administración remota de Windows](#windows-remote-management) debe estar habilitada en estos servidores para la instalación remota.
* Si se implementa Servicios de federación de Active Directory, necesita [Certificados TLS/SSL](#tlsssl-certificate-requirements).
* Si se implementan los Servicios de federación de Active Directory, necesitará configurar la [resolución de nombres](#name-resolution-for-federation-servers).
* Si los administradores globales tienen MFA habilitado, la dirección URL **https://secure.aadcdn.microsoftonline-p.com** debe estar en la lista de sitios de confianza. Se le solicita que agregue este sitio a la lista de sitios de confianza cuando se le pide un desafío MFA y no se ha agregado antes. Puede utilizar Internet Explorer para agregarla a los sitios de confianza.
* Microsoft recomienda reforzar su servidor Azure AD Connect para reducir la superficie de ataque de seguridad para este componente esencial de su entorno de TI.  Siga las recomendaciones que tiene a continuación y podrá reducir los riesgos de seguridad de su organización.

* Implemente Azure AD Connect en un servidor unido a un dominio y restrinja el acceso administrativo a los administradores del dominio o a otros grupos de seguridad rigurosamente controlados.

Para obtener más información, consulte: 

* [Protección de los grupos de administradores](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/appendix-g--securing-administrators-groups-in-active-directory)

* [Protección de las cuentas predefinidas de administrador](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/appendix-d--securing-built-in-administrator-accounts-in-active-directory)

* [Mejora de la seguridad y el sostenimiento al reducir las superficies de ataque](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access#2-reduce-attack-surfaces )

* [Reducción de la superficie de ataque de Active Directory](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface)

### <a name="sql-server-used-by-azure-ad-connect"></a>SQL Server usado por Azure AD Connect
* Azure AD Connect requiere una base de datos de SQL Server para almacenar datos de identidad. De forma predeterminada, se instala SQL Server 2012 Express LocalDB (versión ligera de SQL Server Express). SQL Server Express tiene un límite de tamaño de 10 GB que le permite administrar aproximadamente 100 000 objetos. Si tiene que administrar un volumen superior de objetos de directorio, es necesario que el asistente para la instalación apunte a otra instalación de SQL Server. El tipo de instalación de SQL Server puede afectar al [rendimiento de Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-performance-factors#sql-database-factors).
* Si usa una instalación diferente de SQL Server, se aplican estos requisitos:
  * Azure AD Connect admite todas las versiones de Microsoft SQL Server, desde 2012 (con el Service Pack más reciente) hasta SQL Server 2019. **No se admite** Microsoft Azure SQL Database como base de datos.
  * Debe usar una intercalación de SQL sin distinción de mayúsculas y minúsculas. Estas intercalaciones se identifican porque el nombre incluye \_CI_. **No se puede utilizar** para emplear una intercalación sin distinción de mayúsculas y minúsculas, y se identifica porque el nombre incluye \_CS_.
  * Solo se puede tener un motor de sincronización por cada instancia de SQL. **No se puede** compartir una instancia de SQL con FIM/MIM Sync, DirSync o la Sincronización de Azure AD.

### <a name="accounts"></a>Cuentas
* Una cuenta de administrador global de Azure AD para el inquilino de Azure AD con el que desea realizar la integración. Debe tratarse de una **cuenta profesional o educativa** y no puede ser una **cuenta Microsoft**.
* Si usa la [configuración rápida](reference-connect-accounts-permissions.md#express-settings-installation) o actualiza desde DirSync, debe tener una cuenta de administrador de empresa para la instancia de Active Directory local.
* Si usa la ruta de acceso de instalación de configuración personalizada, dispone de más opciones. Para más información, consulte [Configuración de la instalación personalizada](reference-connect-accounts-permissions.md#custom-installation-settings).

### <a name="connectivity"></a>Conectividad
* El servidor de Azure AD Connect necesita resolución DNS para intranet e Internet. El servidor DNS debe ser capaz de resolver nombres en su Active Directory local y en los puntos de conexión de Azure AD.
* Si tiene firewalls en la Intranet y necesita abrir puertos entre los servidores de Azure AD Connect y los controladores de dominio, consulte [La identidad híbrida requería puertos y protocolos](reference-connect-ports.md) para más información.
* Si el proxy o firewall limita a qué direcciones URL se puede acceder, deben abrirse las direcciones URL que se documentan en [URL de Office 365 e intervalos de direcciones IP](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).
  * Si está usando Microsoft Cloud en Alemania o Microsoft Azure Government Cloud, consulte [Azure AD Connect: consideraciones especiales para instancias](reference-connect-instances.md) para ver las direcciones URL.
* Azure AD Connect (versión 1.1.614.0 y posteriores) usa de forma predeterminada TLS 1.2 para cifrar la comunicación entre el motor de sincronización y Azure AD. Si TLS 1.2 no está disponible en el sistema operativo subyacente, Azure AD Connect recurre de mayor a menor a los protocolos anteriores (TLS 1.1 y TLS 1.0).
* Antes de la versión 1.1.614.0, Azure AD Connect usa de forma predeterminada TLS 1.0 para cifrar la comunicación entre el motor de sincronización y Azure AD. Para cambiar este protocolo a TLS 1.2, siga los pasos que se describen en [Habilitación de TLS 1.2 en Azure AD Connect](#enable-tls-12-for-azure-ad-connect).
* Si usa un proxy de salida para realizar la conexión a Internet, se tiene que agregar la siguiente configuración del archivo **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** con el fin de que el Asistente para instalación y la sincronización de Azure AD Connect se puedan conectar a Internet y Azure AD. Este texto debe escribirse en la parte inferior del archivo. En este código, &lt;PROXYADRESS&gt; representa el nombre de host o la dirección IP de proxy real.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

* Si el servidor proxy requiere autenticación, la [cuenta de servicio](reference-connect-accounts-permissions.md#adsync-service-account) tiene que estar ubicada en el dominio y tendrá que usar la ruta de instalación de la configuración personalizada para especificar una [cuenta de servicio personalizada](how-to-connect-install-custom.md#install-required-components). También necesita otro cambio en el archivo machine.config. Con este cambio en el archivo machine.config, el asistente para instalación y el motor de sincronización responden a las solicitudes de autenticación del servidor proxy. En todas las páginas del asistente para instalación, excepto la página **Configurar** , se usan las credenciales del usuario que ha iniciado sesión. En la página **Configurar** al final del Asistente para instalación, el contexto se cambia a la [cuenta de servicio](reference-connect-accounts-permissions.md#adsync-service-account) que creó. La sección del archivo machine.config debería tener este aspecto.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

* Cuando Azure AD Connect envía una solicitud web a Azure AD como parte de la sincronización de directorios, Azure AD puede tardar hasta 5 minutos en responder. Es habitual que los servidores proxy tengan la configuración de tiempo de espera de inactividad de conexión. Asegúrese de que la configuración esté establecida en 6 minutos o más.

Para obtener más información, consulte el artículo de MSDN sobre el [elemento de proxy predeterminado](https://msdn.microsoft.com/library/kd3cf2ex.aspx).  
Para obtener más información si tiene problemas de conectividad, consulte [Solución de problemas de conectividad](tshoot-connect-connectivity.md).

### <a name="other"></a>Otros
* Opcional: una cuenta de usuario de prueba para comprobar la sincronización.

## <a name="component-prerequisites"></a>Requisitos previos de los componentes
### <a name="powershell-and-net-framework"></a>PowerShell y .NET Framework
Azure AD Connect depende de Microsoft PowerShell y .NET Framework 4.5.1. Necesita que esta versión o una posterior esté instalada en el servidor. Dependiendo de la versión de Windows Server, haga lo siguiente:

* Windows Server 2012 R2
  * Microsoft PowerShell se instala de forma predeterminada, No se requiere ninguna acción.
  * .NET Framework 4.5.1 y versiones posteriores se ofrecen mediante Windows Update. Asegúrese de que ha instalado las actualizaciones más recientes de Windows Server en el Panel de Control.
* Windows Server 2012
  * La versión más reciente de Microsoft PowerShell está disponible en **Windows Management Framework 4.0**, que encontrará en el [Centro de descarga de Microsoft](https://www.microsoft.com/downloads).
  * .NET Framework 4.5.1 y versiones posteriores están disponibles en el [Centro de descarga de Microsoft](https://www.microsoft.com/downloads).


### <a name="enable-tls-12-for-azure-ad-connect"></a>Habilitación de TLS 1.2 en Azure AD Connect
Antes de la versión 1.1.614.0, Azure AD Connect usa de forma predeterminada TLS 1.0 para cifrar la comunicación entre el servidor de sincronización y Azure AD. Puede cambiar este protocolo al configurar las aplicaciones .NET para que empleen TLS 1.2 de forma predeterminada en el servidor. Puede encontrar más información sobre TLS 1.2 en [Documento informativo sobre seguridad de Microsoft 2960358 ](https://technet.microsoft.com/security/advisory/2960358).

1.  Asegúrese de que la revisión de .NET 4.5.1 esté instalada en el sistema operativo. Consulte [Documento informativo sobre seguridad de Microsoft 2960358](https://technet.microsoft.com/security/advisory/2960358). Puede que ya tenga esta revisión o una posterior instalada en el servidor.
    ```
2. For all operating systems, set this registry key and restart the server.
    ```
    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319 "SchUseStrongCrypto"=dword:00000001
    ```
4. If you also want to enable TLS 1.2 between the sync engine server and a remote SQL Server, then make sure you have the required versions installed for [TLS 1.2 support for Microsoft SQL Server](https://support.microsoft.com/kb/3135244).

## Prerequisites for federation installation and configuration
### Windows Remote Management
When using Azure AD Connect to deploy Active Directory Federation Services or the Web Application Proxy, check these requirements:

* If the target server is domain joined, then ensure that Windows Remote Managed is enabled
  * In an elevated PowerShell command window, use command `Enable-PSRemoting –force`
* If the target server is a non-domain joined WAP machine, then there are a couple of additional requirements
  * On the target machine (WAP machine):
    * Ensure the winrm (Windows Remote Management / WS-Management) service is running via the Services snap-in
    * In an elevated PowerShell command window, use command `Enable-PSRemoting –force`
  * On the machine on which the wizard is running (if the target machine is non-domain joined or untrusted domain):
    * In an elevated PowerShell command window, use the command `Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
    * In Server Manager:
      * add DMZ WAP host to machine pool (server manager -> Manage -> Add Servers...use DNS tab)
      * Server Manager All Servers tab: right click WAP server and choose Manage As..., enter local (not domain) creds for the WAP machine
      * To validate remote PowerShell connectivity, in the Server Manager All Servers tab: right click WAP server and choose Windows PowerShell. A remote PowerShell session should open to ensure remote PowerShell sessions can be established.

### TLS/SSL Certificate Requirements
* It's strongly recommended to use the same TLS/SSL certificate across all nodes of your AD FS farm and all Web Application proxy servers.
* The certificate must be an X509 certificate.
* You can use a self-signed certificate on federation servers in a test lab environment. However, for a production environment, we recommend that you obtain the certificate from a public CA.
  * If using a certificate that is not publicly trusted, ensure that the certificate installed on each Web Application Proxy server is trusted on both the local server and on all federation servers
* The identity of the certificate must match the federation service name (for example, sts.contoso.com).
  * The identity is either a subject alternative name (SAN) extension of type dNSName or, if there are no SAN entries, the subject name specified as a common name.  
  * Multiple SAN entries can be present in the certificate, provided one of them matches the federation service name.
  * If you are planning to use Workplace Join, an additional SAN is required with the value **enterpriseregistration.** followed by the User Principal Name (UPN) suffix of your organization, for example, **enterpriseregistration.contoso.com**.
* Certificates based on CryptoAPI next generation (CNG) keys and key storage providers are not supported. This means you must use a certificate based on a CSP (cryptographic service provider) and not a KSP (key storage provider).
* Wild-card certificates are supported.

### Name resolution for federation servers
* Set up DNS records for the AD FS federation service name (for example sts.contoso.com) for both the intranet (your internal DNS server) and the extranet (public DNS through your domain registrar). For the intranet DNS record, ensure that you use A records and not CNAME records. This is required for windows authentication to work correctly from your domain joined machine.
* If you are deploying more than one AD FS server or Web Application Proxy server, then ensure that you have configured your load balancer and that the DNS records for the AD FS federation service name (for example sts.contoso.com) point to the load balancer.
* For windows integrated authentication to work for browser applications using Internet Explorer in your intranet, ensure that the AD FS federation service name (for example sts.contoso.com) is added to the intranet zone in IE. This can be controlled via group policy and deployed to all your domain joined computers.

## Azure AD Connect supporting components
The following is a list of components that Azure AD Connect installs on the server where Azure AD Connect is installed. This list is for a basic Express installation. If you choose to use a different SQL Server on the Install synchronization services page, then SQL Express LocalDB is not installed locally.

* Azure AD Connect Health
* Microsoft SQL Server 2012 Command Line Utilities
* Microsoft SQL Server 2012 Express LocalDB
* Microsoft SQL Server 2012 Native Client
* Microsoft Visual C++ 2013 Redistribution Package

## Hardware requirements for Azure AD Connect
The table below shows the minimum requirements for the Azure AD Connect sync computer.

| Number of objects in Active Directory | CPU | Memory | Hard drive size |
| --- | --- | --- | --- |
| Fewer than 10,000 |1.6 GHz |4 GB |70 GB |
| 10,000–50,000 |1.6 GHz |4 GB |70 GB |
| 50,000–100,000 |1.6 GHz |16 GB |100 GB |
| For 100,000 or more objects the full version of SQL Server is required | | | |
| 100,000–300,000 |1.6 GHz |32 GB |300 GB |
| 300,000–600,000 |1.6 GHz |32 GB |450 GB |
| More than 600,000 |1.6 GHz |32 GB |500 GB |

The minimum requirements for computers running AD FS or Web Application Proxy Servers is the following:

* CPU: Dual core 1.6 GHz or higher
* MEMORY: 2 GB or higher
* Azure VM: A2 configuration or higher

## Next steps
Learn more about [Integrating your on-premises identities with Azure Active Directory](whatis-hybrid-identity.md).
