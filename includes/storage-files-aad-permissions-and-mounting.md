---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 04/11/2019
ms.author: rogara
ms.custom: include file
ms.openlocfilehash: 5fc106bfd97e8decd47ac7d43383907dcbbbda9c
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/05/2020
ms.locfileid: "82792993"
---
## <a name="2-assign-access-permissions-to-an-identity"></a>2 Asignar permisos de acceso a una identidad

Para acceder a los recursos de Azure Files con la autenticación basada en identidades, una identidad (usuario, grupo o entidad de servicio) debe tener los permisos necesarios en el nivel de recurso compartido. Este proceso es similar a especificar los permisos de recurso compartido de Windows, donde se especifica el tipo de acceso que tiene un usuario determinado a un recurso compartido de archivos. Las instrucciones de esta sección muestran cómo asignar permisos de lectura, escritura o eliminación para un recurso compartido de archivos a una identidad. 

Se han incorporado tres roles integrados de Azure para conceder permisos de nivel de recurso compartido a los usuarios:

- El **lector de recursos compartidos de SMB para los datos del archivo de almacenamiento** permite el acceso de lectura en los recursos compartidos de archivos de Azure Storage a través de SMB.
- El **colaborador de recursos compartidos de SMB para los datos del archivo de almacenamiento** permite el acceso de lectura, escritura y eliminación en los recursos compartidos de archivos de Azure Storage a través de SMB.
- El **colaborador con privilegios elevados de recursos compartidos de SMB para los datos del archivo de almacenamiento** permite el acceso de lectura, escritura y eliminación en los recursos compartidos de archivos de Azure Storage a través de SMB.

> [!IMPORTANT]
> El control administrativo total de un recurso compartido de archivos, incluida la capacidad de tomar posición de un archivo, requiere usar la clave de la cuenta de almacenamiento. El control administrativo no puede realizarse con credenciales de Azure AD.

Puede usar Azure Portal, PowerShell o la CLI de Azure para asignar los roles integrados a la identidad de Azure AD de un usuario a fin de conceder permisos de nivel de recurso compartido. Tenga en cuenta que la asignación de roles RBAC de nivel de recurso compartido puede tardar un tiempo en estar en vigor. 

> [!NOTE]
> Recuerde [sincronizar sus credenciales de AD DS con Azure AD](../articles/active-directory/hybrid/how-to-connect-install-roadmap.md) si planea usar AD DS local para la autenticación. La sincronización de hash de contraseña de AD DS con Azure AD es opcional. El permiso de nivel de recurso compartido se concederá a la identidad de Azure AD sincronizada desde AD DS local.

La recomendación general es usar el permiso de nivel de recurso compartido para la administración del acceso de alto nivel para un grupo de AD que represente un grupo de usuarios e identidades y, después, aprovechar los permisos NTFS para el control de acceso pormenorizado en el nivel de directorio o archivo. 

#### <a name="azure-portal"></a>Azure Portal
Para asignar un rol de RBAC a una identidad de Azure AD mediante [Azure Portal](https://portal.azure.com), siga estos pasos:

1. En Azure Portal, vaya al recurso compartido de archivos o [cree un recurso compartido de archivos](../articles/storage/files/storage-how-to-create-file-share.md).
2. Seleccione **Access Control (IAM)** .
3. Seleccione **Agregar una asignación de roles**.
4. En la hoja **Agregar asignación de roles**, seleccione el rol integrado adecuado (lector o colaborador de recursos compartidos de SMB para datos del archivo de almacenamiento) en la lista desplegable **Rol**. Mantenga la opción **Asignar acceso a** en la configuración predeterminada: **Usuario, grupo o entidad de servicio de Azure AD**. Seleccione la identidad de Azure AD de destino por nombre o dirección de correo electrónico.
5. Seleccione **Guardar** para completar la operación de asignación de roles.

#### <a name="powershell"></a>PowerShell

El siguiente ejemplo de PowerShell muestra cómo asignar un rol de RBAC a una identidad de Azure AD, según el nombre de inicio de sesión. Para más información sobre la asignación de roles RBAC con PowerShell, consulte [Administración del acceso mediante RBAC y Azure PowerShell](../articles/role-based-access-control/role-assignments-powershell.md).

Antes de ejecutar el siguiente script de ejemplo, no olvide reemplazar los valores de marcador de posición, incluidos los corchetes, por los suyos propios.

```powershell
#Get the name of the custom role
$FileShareContributorRole = Get-AzRoleDefinition "<role-name>" #Use one of the built-in roles: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor, Storage File Data SMB Share Elevated Contributor
#Constrain the scope to the target file share
$scope = "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshares/<share-name>"
#Assign the custom role to the target identity with the specified scope.
New-AzRoleAssignment -SignInName <user-principal-name> -RoleDefinitionName $FileShareContributorRole.Name -Scope $scope
```

#### <a name="cli"></a>CLI
  
El siguiente comando de la CLI 2.0 muestra cómo asignar un rol de RBAC a una identidad de Azure AD, según el nombre de inicio de sesión. Para más información sobre la asignación de roles de RBAC con la CLI de Azure, consulte [Administración del acceso mediante RBAC y la CLI de Azure](../articles/role-based-access-control/role-assignments-cli.md). 

Antes de ejecutar el siguiente script de ejemplo, no olvide reemplazar los valores de marcador de posición, incluidos los corchetes, por los suyos propios.

```azurecli-interactive
#Assign the built-in role to the target identity: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor, Storage File Data SMB Share Elevated Contributor
az role assignment create --role "<role-name>" --assignee <user-principal-name> --scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshares/<share-name>"
```

## <a name="3-configure-ntfs-permissions-over-smb"></a>3 Configurar los permisos NTFS a través de SMB 
Después de asignar los permisos en los recursos compartidos con RBAC, debe asignar los permisos NTFS adecuados en las raíces, los directorios o los archivos. Considere los permisos de nivel de recurso compartido como el equipo selector de alto nivel que determina si un usuario puede acceder al recurso compartido. Mientras, los permisos NTFS actúan en un nivel más granular para determinar qué operaciones puede realizar el usuario en el directorio o archivo.

Azure Files admite el conjunto completo de permisos NTFS básicos y avanzados. Puede ver y configurar los permisos NTFS en los directorios y los archivos de un recurso compartido de archivos de Azure; para ello, monte el recurso compartido y, a continuación, utilice el Explorador de archivos de Windows o ejecute el comando [icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls) o [Set-ACL](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-acl) de Windows. 

Para configurar NTFS con permisos de superusuario, debe montar el recurso compartido con la clave de la cuenta de almacenamiento de la máquina virtual unida al dominio. Siga las instrucciones de la sección siguiente para montar un recurso compartido de archivos de Azure desde el símbolo del sistema y para configurar los permisos NTFS en consecuencia.

Se admiten los siguientes conjuntos de permisos en el directorio raíz de un recurso compartido de archivos:

- BUILTIN\Administrators:(OI)(CI)(F)
- NT AUTHORITY\SYSTEM:(OI)(CI)(F)
- BUILTIN\Users:(RX)
- BUILTIN\Users:(OI)(CI)(IO)(GR,GE)
- NT AUTHORITY\Authenticated Users:(OI)(CI)(M)
- NT AUTHORITY\SYSTEM:(F)
- CREATOR OWNER:(OI)(CI)(IO)(F)

### <a name="mount-a-file-share-from-the-command-prompt"></a>Montar un recurso compartido de archivos de Azure desde el símbolo del sistema

Use el comando **net use** de Windows para montar el recurso compartido de archivos de Azure. No olvide reemplazar los valores del marcador de posición del ejemplo por los suyos propios. Para más información sobre cómo montar los recursos compartidos de archivos, consulte [Uso de un recurso compartido de archivos de Azure con Windows](../articles/storage/files/storage-how-to-use-files-windows.md). 

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> /user:Azure\<storage-account-name> <storage-account-key>
```

Si tiene problemas para conectarse a Azure Files, consulte [la herramienta de solución de problemas publicada para solucionar los errores de montaje de Azure Files en Windows](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5). También proporcionamos una [guía](https://docs.microsoft.com/azure/storage/files/storage-files-faq#on-premises-access) para solucionar aquellos escenarios en los que el puerto 445 está bloqueado. 


### <a name="configure-ntfs-permissions-with-windows-file-explorer"></a>Configuración de permisos NTFS con el Explorador de archivos de Windows
Utilice el Explorador de archivos de Windows para conceder permisos completos para todos los directorios y archivos en el recurso compartido de archivos, incluido el directorio raíz.

1. Abra el Explorador de archivos de Windows y haga clic con el botón derecho en el archivo o directorio, y seleccione **Propiedades**.
2. Seleccione la pestaña **Seguridad** .
3. Seleccione **Editar...** para cambiar los permisos.
4. Puede cambiar el permiso de los usuarios existentes o seleccionar **Agregar...** para conceder permisos a los nuevos usuarios.
5. En la ventana del símbolo del sistema para agregar nuevos usuarios, escriba el nombre de usuario de destino al que desea conceder el permiso en el cuadro **Escriba los nombres de objeto que desea seleccionar** y seleccione **Comprobar nombres** para buscar el nombre UPN completo del usuario de destino.
7.    Seleccione **Aceptar**.
8.    En la pestaña **Seguridad**, seleccione todos los permisos que desea conceder al nuevo usuario.
9.    Seleccione **Aplicar**.

### <a name="configure-ntfs-permissions-with-icacls"></a>Configurar los permisos NTFS con icacls
Utilice el siguiente comando de Windows para conceder permisos completos para todos los directorios y archivos en el recurso compartido de archivos, incluido el directorio raíz. No olvide reemplazar los valores del marcador de posición en el ejemplo por los propios.

```
icacls <mounted-drive-letter>: /grant <user-email>:(f)
```

Para más información sobre cómo usar icacls para establecer los permisos de NTFS y sobre los distintos tipos de permisos admitidos, consulte [la referencia de línea de comandos de icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls).

## <a name="4-mount-a-file-share-from-a-domain-joined-vm"></a>4 Montar un recurso compartido de archivos desde una VM unida al dominio

El siguiente proceso comprueba que tanto recurso compartido de archivos como los permisos de acceso se han configurado correctamente y que puede acceder a un recurso compartido de archivos de Azure desde una máquina virtual unida a un dominio. Tenga en cuenta que la asignación de roles RBAC de nivel de recurso compartido puede tardar un tiempo en estar en vigor. 

Inicie sesión en la máquina virtual con la identidad de Azure AD a la que se hayan concedido permisos, tal como se muestra en la siguiente imagen. Si ha habilitado la autenticación de AD DS local para Azure Files, use sus credenciales de AD DS. Para la autenticación de Azure AD DS, inicie sesión con la credencial de Azure AD.

![Captura de pantalla que muestra la pantalla Inicio de sesión de Azure AD para la autenticación de usuario](media/storage-files-aad-permissions-and-mounting/azure-active-directory-authentication-dialog.png)

Use el comando siguiente para montar el recurso compartido de archivos de Azure. Reemplace los valores del marcador de posición por los propios. Como ya se ha autenticado, no es preciso que especifique la clave de la cuenta de almacenamiento ni las credenciales de AD DS local o de Azure AD DS. La experiencia de inicio de sesión único se admite para la autenticación con AD DS local o Azure AD DS. Si tiene problemas al montar con las credenciales de AD DS, consulte [Solución de problemas de Azure Files en Windows](https://docs.microsoft.com/azure/storage/files/storage-troubleshoot-windows-file-connection-problems) para obtener una guía.

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name>
```
