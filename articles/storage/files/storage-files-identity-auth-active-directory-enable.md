---
title: Habilitación de la autenticación de Azure Active Directory en SMB para Azure Files
description: Aprenda a habilitar la autenticación basada en la identidad en SMB para recursos compartidos de archivos de Azure mediante Active Directory. Las máquinas virtuales Windows unidas a un dominio pueden acceder entonces a los recursos compartidos de archivos de Azure con las credenciales de AD.
author: roygara
ms.service: storage
ms.subservice: files
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: rogarana
ms.openlocfilehash: febb796a47b9f5e78906d513c115b62b35c7c7d5
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83196514"
---
# <a name="enable-on-premises-active-directory-domain-services-authentication-over-smb-for-azure-file-shares"></a>Habilitación de la autenticación de Active Directory Domain Services local en SMB para recursos compartidos de archivos de Azure

[Azure Files](storage-files-introduction.md) admite la autenticación basada en la identidad en el bloque de mensajes del servidor (SMB) mediante dos tipos de servicios de dominio: Azure Active Directory Domain Services (Azure AD DS) and Active Directory Domain Services (AD DS) local (versión preliminar). Este artículo se centra en la posibilidad recién introducida (versión preliminar) de aprovechar Active Directory Domain Services para la autenticación en recursos compartidos de archivos de Azure. Si está interesado en habilitar la autenticación de Azure AD DS (GA) en recursos compartidos de archivos de Azure, consulte [nuestro artículo sobre el tema](storage-files-identity-auth-active-directory-domain-service-enable.md).

> [!NOTE]
> Los recursos compartidos de archivos de Azure solo admiten la autenticación en un servicio de dominio: Azure Active Directory Domain Services (Azure AD DS) o Active Directory Domain Services (AD DS) local. 
>
> Las identidades de AD DS que se usen para la autenticación de recursos compartidos de archivos de Azure se deben sincronizar con Azure AD. La sincronización de hash de contraseña es opcional. 
> 
> La autenticación de AD DS local no admite la autenticación en cuentas de equipo creadas en AD DS. 
> 
> La autenticación de AD DS local solo se puede admitir en un bosque de AD en el que se haya registrado la cuenta de almacenamiento. De forma predeterminada, solo se puede acceder a los recursos compartidos de archivos de Azure con las credenciales de AD DS desde un solo bosque. Si necesita acceso al recurso compartido de archivos de Azure desde otro bosque, asegúrese de tener configurada la confianza de bosque adecuada; para más información, consulte las [preguntas más frecuentes](https://docs.microsoft.com/azure/storage/files/storage-files-faq#security-authentication-and-access-control).  
> 
> Se admite la autenticación de AD DS para el acceso de SMB y la persistencia de ACL en los recursos compartidos de archivos de Azure administrados por Azure File Sync.
>
> Azure Files admite la autenticación Kerberos con AD con cifrado RC4-HMAC. Todavía no se admite el cifrado de Kerberos con AES.

Al habilitar AD DS para recursos compartidos de archivos de Azure en SMB, las máquinas unidas a AD DS pueden montar recursos compartidos de archivos de Azure con sus credenciales de AD existentes. Esta funcionalidad se puede habilitar con un entorno de AD DS hospedado en máquinas locales o en Azure.

Las identidades que se usan para acceder a los recursos compartidos de archivos de Azure se deben sincronizar con Azure AD para aplicar los permisos de archivo de nivel de recurso compartido mediante el modelo de [control de acceso basado en rol (RBAC)](../../role-based-access-control/overview.md). Se conservarán y aplicarán las [DACL tipo Windows](https://docs.microsoft.com/previous-versions/technet-magazine/cc161041(v=msdn.10)?redirectedfrom=MSDN) en archivos o directorios transferidos desde servidores de archivos existentes. Esta característica ofrece una perfecta integración con el entorno de AD DS de la empresa. A medida que reemplaza los servidores de archivos locales por recursos compartidos de archivos de Azure, los usuarios existentes pueden acceder a estos desde sus clientes actuales con una experiencia de inicio de sesión único, sin ningún cambio en las credenciales en uso.  

> [!NOTE]
> Para ayudarle a configurar la autenticación con AD de Azure Files para algunos casos de uso comunes, hemos publicado [dos vídeos](https://docs.microsoft.com/azure/storage/files/storage-files-introduction#videos) con una guía paso a paso:
> - Reemplazo de servidores de archivos locales por Azure Files (incluida la configuración en un vínculo privado para la autenticación de archivos y AD)
> - Uso de Azure Files como contenedor de perfiles para Windows Virtual Desktop (incluida la configuración de la autenticación de AD y la configuración de FsLogix)

## <a name="prerequisites"></a>Prerrequisitos 

Antes de habilitar la autenticación de AD DS para los recursos compartidos de archivos de Azure, asegúrese de que cumple los siguientes requisitos previos: 

- Seleccione o cree un entorno de AD DS y [sincronícelo con Azure AD](../../active-directory/hybrid/how-to-connect-install-roadmap.md). 

    La característica se puede habilitar en un entorno de AD DS nuevo o existente. Las identidades usadas para el acceso deben estar sincronizadas con Azure AD. El inquilino de Azure AD y el recurso compartido de archivos al que accede debe estar asociado con la misma suscripción. 

    Para configurar un entorno de dominio de AD, consulte [Introducción a Active Directory Domain Services](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview). Si no ha sincronizado AD con Azure AD, siga las instrucciones que se proporcionan en [Guía de instalación de Azure AD Connect y Azure AD Connect Health](../../active-directory/hybrid/how-to-connect-install-roadmap.md) para instalar y configurar Azure AD Connect. 

- Unir una máquina local o una máquina virtual de Azure por dominio a un AD DS local. 

    Para acceder a un recurso compartido de archivos mediante las credenciales de AD desde una máquina local o virtual, el dispositivo debe estar unido a un dominio de AD DS. Para información acerca de cómo unirse a un dominio, consulte [Unión de un equipo a un dominio](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/join-a-computer-to-a-domain). 

- Seleccione o cree una cuenta de almacenamiento de Azure.  Para conseguir un rendimiento óptimo, se recomienda implementar la cuenta de almacenamiento en la misma región que la máquina virtual desde la que vaya a acceder al recurso compartido.

    Asegúrese de que la cuenta de almacenamiento que contiene los recursos compartidos de archivos todavía no está configurada para la autenticación de Azure AD DS. Si la autenticación de Azure AD DS de Azure Files está habilitada en la cuenta de almacenamiento, se debe deshabilitar antes de cambiar al uso de una versión local de AD DS. Esto implica que las ACL existentes configuradas en el entorno de Azure AD DS se deberán volver a configurar para que se apliquen los permisos adecuados.
    
    Para información sobre cómo crear un nuevo recurso compartido de archivos, consulte  [Creación de un recurso compartido de archivos en Azure Files](storage-how-to-create-file-share.md).

- Para comprobar la conectividad, monte los recursos compartidos de archivos de Azure con la clave de su cuenta de almacenamiento. 

    Para comprobar que el dispositivo y el recurso compartido de archivos están configurados correctamente, intente [montar el recurso compartido de archivos](storage-how-to-use-files-windows.md) con la clave de la cuenta de almacenamiento. Si tiene problemas para conectarse a Azure Files, consulte [la herramienta de solución de problemas publicada para solucionar los errores de montaje de Azure Files en Windows](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5). También proporcionamos una [guía](https://docs.microsoft.com/azure/storage/files/storage-files-faq#on-premises-access) para solucionar aquellos escenarios en los que el puerto 445 está bloqueado. 

## <a name="regional-availability"></a>Disponibilidad regional

La autenticación de Azure Files con AD DS (versión preliminar) está disponible en [todas las regiones públicas y las regiones de Azure Government](https://azure.microsoft.com/global-infrastructure/locations/).

## <a name="overview"></a>Información general

Si planea habilitar configuraciones de red en el recurso compartido de archivos, se recomienda que evalúe la [consideración de redes](https://docs.microsoft.com/azure/storage/files/storage-files-networking-overview) y que complete la configuración relacionada antes de habilitar la autenticación de AD DS.

Habilitar la autenticación de AD DS para los recursos compartidos de archivos de Azure le permite autenticarse en los recursos compartidos de archivos de Azure con las credenciales de AD DS local. Además, le permite administrar mejor los permisos para permitir el control de acceso granular. Para ello, se requiere la sincronización de identidades desde AD DS local a Azure AD con AD Connect. El usuario controla el acceso de nivel de recursos compartidos con identidades sincronizadas con Azure AD al administrar el acceso de nivel de archivos o recursos compartidos con credenciales de AD DS local.

A continuación, siga los pasos que se indican a continuación para configurar Azure Files para la autenticación de AD DS: 

1. [Habilite la autenticación con AD DS de Azure Files en la cuenta de almacenamiento.](#1-enable-ad-ds-authentication-for-your-account)

1. [Asigne permisos de acceso para un recurso compartido a la identidad de Azure AD (un usuario, grupo o entidad de servicio) que esté sincronizada con la identidad de AD de destino.](#2-assign-access-permissions-to-an-identity)

1. [Configure las ACL en SMB para directorios y archivos.](#3-configure-ntfs-permissions-over-smb)
 
1. [Monte un recurso compartido de archivos de Azure en una VM unida a su AD DS.](#4-mount-a-file-share-from-a-domain-joined-vm)

1. [Actualice la contraseña de la identidad de la cuenta de almacenamiento en AD DS.](#5-update-the-password-of-your-storage-account-identity-in-ad-ds)

En el diagrama siguiente se ilustra el flujo de trabajo de un extremo a otro para habilitar la autenticación de Azure AD en SMB en los recursos compartidos de archivos de Azure. 

![Diagrama de flujo de trabajo de AD de Files](media/storage-files-active-directory-domain-services-enable/diagram-files-ad.png)

> [!NOTE]
> La autenticación de AD DS en SMB para los recursos compartidos de archivos de Azure solo se admite en máquinas locales o virtuales que se ejecutan en versiones de sistema operativo posteriores a Windows 7 o Windows Server 2008 R2. 

## <a name="1-enable-ad-ds-authentication-for-your-account"></a>1 Habilitación de la autenticación de AD DS para la cuenta 

Para habilitar la autenticación de AD DS en SMB para los recursos compartidos de archivos de Azure, primero debe registrar la cuenta de almacenamiento en AD DS y, luego, establecer las propiedades de dominio necesarias en la cuenta de almacenamiento. Cuando la característica está habilitada en la cuenta de almacenamiento, se aplica a todos los recursos compartidos de archivos nuevos y existentes de la cuenta. Descargue el módulo AzFilesHybrid de Powershell y use `join-AzStorageAccountForAuth` para habilitar la característica. Puede encontrar la descripción detallada del flujo de trabajo de un extremo a otro en el script de esta sección. 

> [!IMPORTANT]
> El cmdlet `Join-AzStorageAccountForAuth` realizará modificaciones en el entorno de AD. Lea la explicación siguiente para comprender mejor lo que está haciendo para asegurarse de que tiene los permisos adecuados para ejecutar el comando y que los cambios aplicados se alinean con las directivas de cumplimiento y seguridad. 

El cmdlet `Join-AzStorageAccountForAuth` realizará la acción equivalente a la unión a un dominio sin conexión en nombre de la cuenta de almacenamiento indicada. El script usa el cmdlet para crear una cuenta en el dominio de AD, bien una [cuenta de equipo](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) (predeterminada) o una [cuenta de inicio de sesión de servicio](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts). Si decide hacerlo manualmente, debe seleccionar la cuenta que mejor se adapte a su entorno.

La cuenta de AD DS que crea el cmdlet representa la cuenta de almacenamiento en el dominio de AD. Si la cuenta de AD DS se crea en una unidad organizativa (OU) que aplica la expiración de contraseñas, debe actualizar la contraseña antes de su vigencia máxima. Si no actualiza la contraseña de la cuenta, se producirán errores de autenticación al acceder a recursos compartidos de archivos de Azure. Para saber cómo actualizar la contraseña, consulte [Actualización de la contraseña de la cuenta de AD DS](#5-update-the-password-of-your-storage-account-identity-in-ad-ds).

Puede usar el script siguiente para realizar el registro y habilitar la característica o, como alternativa, puede realizar manualmente las operaciones que el script haría. Dichas operaciones se describen en la sección que sigue al script. No es necesario hacer ambas cosas.

### <a name="11-script-prerequisites"></a>1.1 Requisitos previos del script
- [Descarga y descompresión del módulo AzFilesHybrid](https://github.com/Azure-Samples/azure-files-samples/releases)
- Instale y ejecute el módulo en un dispositivo que se haya unido a un dominio a AD DS local con credenciales de AD DS que tengan permisos para crear una cuenta de inicio de sesión de servicio o una cuenta de equipo en la instancia de destino.
-  Ejecute el script con una credencial de AD DS local que esté sincronizada con su instancia de Azure AD. La credencial de AD DS local debe tener los permisos de la cuenta de propietario o del rol de RBAC de colaborador.
- Asegúrese de que la cuenta de almacenamiento se encuentra en una [región admitida](#regional-availability).

### <a name="12-domain-join-your-storage-account"></a>1.2 Unión a un dominio de la cuenta de almacenamiento
No olvide reemplazar los valores de marcador de posición por los suyos propios en los parámetros siguientes antes de ejecutar el script en PowerShell.
> [!IMPORTANT]
> El cmdlet de unión a un dominio creará una cuenta de AD para representar la cuenta de almacenamiento (recurso compartido de archivos) en AD. Puede optar por registrarla como una cuenta de equipo o una cuenta de inicio de sesión de servicio; consulte [Preguntas más frecuentes](https://docs.microsoft.com/azure/storage/files/storage-files-faq#security-authentication-and-access-control) para obtener más información. En el caso de las cuentas de equipo, se establece un tiempo de expiración de contraseña predeterminado en AD de 30 días. Del mismo modo, la cuenta de inicio de sesión de servicio puede tener establecido un tiempo de expiración de contraseña predeterminado en el dominio o la unidad organizativa (OU) de AD.
> Para ambos tipos de cuenta, se recomienda encarecidamente comprobar cuál es el tiempo de expiración de la contraseña configurado en su entorno de AD y que planee la [Actualización de la contraseña de la identidad de la cuenta de almacenamiento en AD](#5-update-the-password-of-your-storage-account-identity-in-ad-ds) de la cuenta de AD siguiente antes el tiempo máximo de la contraseña. Puede considerar [crear una nueva unidad organizativa (OU) de AD en AD](https://docs.microsoft.com/powershell/module/addsadministration/new-adorganizationalunit?view=win10-ps) y deshabilitar la directiva de expiración de contraseñas en las [cuentas de equipo](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj852252(v=ws.11)?redirectedfrom=MSDN) o las cuentas de inicio de sesión de servicio, según corresponda. 

```PowerShell
#Change the execution policy to unblock importing AzFilesHybrid.psm1 module
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser

# Navigate to where AzFilesHybrid is unzipped and stored and run to copy the files into your path
.\CopyToPSPath.ps1 

#Import AzFilesHybrid module
Import-Module -Name AzFilesHybrid

#Login with an Azure AD credential that has either storage account owner or contributer RBAC assignment
Connect-AzAccount

#Define parameters
$SubscriptionId = "<your-subscription-id-here>"
$ResourceGroupName = "<resource-group-name-here>"
$StorageAccountName = "<storage-account-name-here>"

#Select the target subscription for the current session
Select-AzSubscription -SubscriptionId $SubscriptionId 

# Register the target storage account with your active directory environment under the target OU (for example: specify the OU with Name as "UserAccounts" or DistinguishedName as "OU=UserAccounts,DC=CONTOSO,DC=COM"). 
# You can use to this PowerShell cmdlet: Get-ADOrganizationalUnit to find the Name and DistinguishedName of your target OU. If you are using the OU Name, specify it with -OrganizationalUnitName as shown below. If you are using the OU DistinguishedName, you can set it with -OrganizationalUnitDistinguishedName. You can choose to provide one of the two names to specify the target OU.
# You can choose to create the identity that represents the storage account as either a Service Logon Account or Computer Account (default parameter value), depends on the AD permission you have and preference. 
# You can run Get-Help Join-AzStorageAccountForAuth to find more details on this cmdlet.

Join-AzStorageAccountForAuth `
        -ResourceGroupName $ResourceGroupName `
        -Name $StorageAccountName `
        -DomainAccountType "<ComputerAccount|ServiceLogonAccount>" `
        -OrganizationalUnitName "<ou-name-here>" #You can also use -OrganizationalUnitDistinguishedName "<ou-distinguishedname-here>" instead. If you don't provide the OU name as an input parameter, the AD identity that represents the storage account will be created under the root directory.

#You can run the Debug-AzStorageAccountAuth cmdlet to conduct a set of basic checks on your AD configuration with the logged on AD user. This cmdlet is supported on AzFilesHybrid v0.1.2+ version. For more details on the checks performed in this cmdlet, go to Azure Files FAQ.
Debug-AzStorageAccountAuth -StorageAccountName $StorageAccountName -ResourceGroupName $ResourceGroupName -Verbose

```

La descripción siguiente resume todas las acciones que se realizan cuando se ejecuta el cmdlet `Join-AzStorageAccountForAuth`. Si prefiere no usar el comando, puede realizar estos pasos manualmente:

> [!NOTE]
> Si ya ha ejecutado correctamente el script `Join-AzStorageAccountForAuth` anterior, vaya a la sección siguiente "1.3 Confirmación de que la característica está habilitada". No es necesario volver a realizar las operaciones siguientes.

#### <a name="a-checking-environment"></a>a. Comprobación del entorno

En primer lugar, el script comprueba el entorno. En concreto, comprueba si [Active Directory PowerShell](https://docs.microsoft.com/powershell/module/addsadministration/?view=win10-ps) está instalado y si el shell se ejecuta con privilegios de administrador. Luego, se comprueba si está instalado el [módulo Az.Storage 1.11.1-preview](https://www.powershellgallery.com/packages/Az.Storage/1.11.1-preview) y, en caso negativo, se instala. Si los resultados de esas comprobaciones son correctos, se comprobará la instancia de AD DS para ver si hay una [cuenta de equipo](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) (valor predeterminado) o una [cuenta de inicio de sesión de servicio](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts) que ya se haya creado con SPN/UPN como "cifs/your-storage-account-name-here.file.core.windows.net". Si la cuenta no existe, se creará una como se describe en la sección b que aparece a continuación.

#### <a name="b-creating-an-identity-representing-the-storage-account-in-your-ad-manually"></a>b. Creación de una identidad que representa la cuenta de almacenamiento en AD manualmente

Para crear esta cuenta manualmente, cree una clave Kerberos para la cuenta de almacenamiento mediante `New-AzStorageAccountKey -KeyName kerb1`. Luego, use esa clave de Kerberos como contraseña para la cuenta. Esta clave solo se usa durante la configuración y no se puede usar para las operaciones de control o de plano de datos en la cuenta de almacenamiento.

Cuando tenga esa clave, cree una cuenta de servicio o de equipo en la unidad organizativa. Use la siguiente especificación: SPN: "cifs/your-storage-account-name-here.file.core.windows.net" Contraseña: clave Kerberos de la cuenta de almacenamiento.

Si la unidad organizativa impone la expiración de la contraseña, debe actualizar la contraseña antes de su vigencia máxima para evitar errores de autenticación al acceder a recursos compartidos de archivos de Azure. Para más información, consulte [Actualización de la contraseña de la identidad de la cuenta de almacenamiento en AD DS](#5-update-the-password-of-your-storage-account-identity-in-ad-ds).

Mantenga el SID de la cuenta recién creada; lo necesitará en el siguiente paso. No es preciso sincronizar con Azure AD la identidad que ha creado que representa la cuenta de almacenamiento.

##### <a name="c-enable-the-feature-on-your-storage-account"></a>c. Habilitación de la característica en la cuenta de almacenamiento

Luego, el script habilitaría la característica en la cuenta de almacenamiento. Para realizar esta configuración manualmente, proporcione algunos detalles de configuración para las propiedades de dominio en el siguiente comando y, luego, ejecútelo. El SID de la cuenta de almacenamiento necesario en el siguiente comando es el de la identidad que creó en su AD DS en [la sección anterior](#b-creating-an-identity-representing-the-storage-account-in-your-ad-manually).

```PowerShell
# Set the feature flag on the target storage account and provide the required AD domain information
Set-AzStorageAccount `
        -ResourceGroupName "<your-resource-group-name-here>" `
        -Name "<your-storage-account-name-here>" `
        -EnableActiveDirectoryDomainServicesForFile $true `
        -ActiveDirectoryDomainName "<your-domain-name-here>" `
        -ActiveDirectoryNetBiosDomainName "<your-netbios-domain-name-here>" `
        -ActiveDirectoryForestName "<your-forest-name-here>" `
        -ActiveDirectoryDomainGuid "<your-guid-here>" `
        -ActiveDirectoryDomainsid "<your-domain-sid-here>" `
        -ActiveDirectoryAzureStorageSid "<your-storage-account-sid>"
```


### <a name="13-confirm-that-the-feature-is-enabled"></a>1.3 Confirmación de que la característica está habilitada

Para confirmar que la característica está habilitada en la cuenta de almacenamiento, puede usar el siguiente script:

```PowerShell
# Get the target storage account
$storageaccount = Get-AzStorageAccount `
        -ResourceGroupName "<your-resource-group-name-here>" `
        -Name "<your-storage-account-name-here>"

# List the directory service of the selected service account
$storageAccount.AzureFilesIdentityBasedAuth.DirectoryServiceOptions

# List the directory domain information if the storage account has enabled AD DS authentication for file shares
$storageAccount.AzureFilesIdentityBasedAuth.ActiveDirectoryProperties
```

Ahora ha habilitado correctamente la característica en la cuenta de almacenamiento. Ahora que la característica está habilitada, hay que realizar más pasos para usar la característica.

[!INCLUDE [storage-files-aad-permissions-and-mounting](../../../includes/storage-files-aad-permissions-and-mounting.md)]

Ahora ha habilitado correctamente la autenticación de AD DS en SMB y ha asignado un rol personalizado que proporciona acceso a un recurso compartido de archivos de Azure con una identidad de AD DS. Para conceder a otros usuarios acceso al recurso compartido de archivos, siga las instrucciones de las secciones [Asignación de permisos de acceso](#2-assign-access-permissions-to-an-identity) y [Configuración de los permisos NTFS en SMB](#3-configure-ntfs-permissions-over-smb).

## <a name="5-update-the-password-of-your-storage-account-identity-in-ad-ds"></a>5 Actualización de la contraseña de la identidad de la cuenta de almacenamiento en AD DS

Si registró la cuenta o identidad de AD DS que representa la cuenta de almacenamiento bajo una unidad organizativa que aplica el tiempo de expiración de la contraseña, debe rotar la contraseña antes de su vigencia máxima. Si no se actualiza la contraseña de la cuenta de AD DS, se producirán errores de autenticación en el acceso a recursos compartidos de archivos de Azure.  

Para desencadenar la rotación de contraseñas, puede ejecutar el comando `Update-AzStorageAccountADObjectPassword` desde el módulo AzFilesHybrid. El cmdlet realiza acciones similares a la rotación de claves de la cuenta de almacenamiento. Obtiene la segunda clave Kerberos de la cuenta de almacenamiento y la usa para actualizar la contraseña de la cuenta registrada en AD DS. Luego, vuelve a generar la clave Kerberos de destino de la cuenta de almacenamiento y actualiza la contraseña de la cuenta registrada en AD DS. Este cmdlet se debe ejecutar en un entorno unido a un dominio de AD DS local.

```PowerShell
# Update the password of the AD DS account registered for the storage account
Update-AzStorageAccountADObjectPassword `
        -RotateToKerbKey kerb2 `
        -ResourceGroupName "<your-resource-group-name-here>" `
        -StorageAccountName "<your-storage-account-name-here>"
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure Files y el uso de AD en SMB, consulte estos recursos:

- [Introducción a la compatibilidad de la autenticación basada en la identidad de Azure Files con el acceso SMB](storage-files-active-directory-overview.md)
- [P+F](storage-files-faq.md)
