---
title: Uso de un recurso compartido de archivos de Azure con Windows | Microsoft Docs
description: Aprenda a usar un recurso compartido de archivos de Azure con Windows y Windows Server.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 2694e0c1536064267faad10517ae58d0709ad1c8
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82231771"
---
# <a name="use-an-azure-file-share-with-windows"></a>Uso de un recurso compartido de archivos de Azure con Windows
[Azure Files](storage-files-introduction.md) es el sencillo sistema de archivos en la nube de Microsoft. Los recursos compartidos de archivos de Azure se pueden usar sin problemas en Windows y Windows Server. En este artículo se describen los aspectos que se deben tener en cuenta al usar un recurso compartido de archivos de Azure con Windows y Windows Server.

Para usar un recurso compartido de archivos de Azure fuera de la región de Azure en la que se hospeda, bien sea de forma local o en una región distinta de Azure, el sistema operativo debe admitir SMB 3.0. 

Puede usar recursos compartidos de archivos de Azure en una instalación de Windows que se ejecute en una máquina virtual de Azure o en el entorno local. En la tabla siguiente se ilustran las versiones del sistema operativo que admiten el acceso a recursos compartidos de archivos en cada entorno:

| Versión de Windows        | Versión de SMB | Se puede montar en una máquina virtual de Azure | Se puede montar en el entorno local |
|------------------------|-------------|-----------------------|-----------------------|
| Windows Server 2019 | SMB 3.0 | Sí | Sí |
| Windows 10<sup>1</sup> | SMB 3.0 | Sí | Sí |
| Canal semestral de Windows Server<sup>2</sup> | SMB 3.0 | Sí | Sí |
| Windows Server 2016 | SMB 3.0 | Sí | Sí |
| Windows 8.1 | SMB 3.0 | Sí | Sí |
| Windows Server 2012 R2 | SMB 3.0 | Sí | Sí |
| Windows Server 2012 | SMB 3.0 | Sí | Sí |
| Windows 7<sup>3</sup> | SMB 2.1 | Sí | No |
| Windows Server 2008 R2<sup>3</sup> | SMB 2.1 | Sí | No |

<sup>1</sup>Windows 10, versiones 1507, 1607, 1709, 1803, 1809, 1903 y 1909.  
<sup>2</sup>Windows Server, versiones 1809, 1903 y 1909.  
<sup>3</sup>Finalizó el soporte técnico habitual de Microsoft para Windows 7 y Windows Server 2008 R2. Es posible adquirir soporte técnico adicional para las actualizaciones de seguridad solo a través del programa [Actualización de seguridad extendida (ESU)](https://support.microsoft.com/help/4497181/lifecycle-faq-extended-security-updates). Se recomienda encarecidamente migrar de estos sistemas operativos.

> [!Note]  
> Siempre se recomienda disponer de la KB más reciente para su versión de Windows.

## <a name="prerequisites"></a>Prerrequisitos 
* **Nombre de la cuenta de almacenamiento**: para montar un recurso compartido de archivos de Azure, necesitará el nombre de la cuenta de almacenamiento.

* **Clave de la cuenta de almacenamiento**: para montar un recurso compartido de archivos de Azure, necesitará la clave principal (o secundaria). Actualmente no se admiten claves SAS para el montaje.

* **Asegúrese de que el puerto 445 está abierto**: el protocolo SMB requiere que esté abierto el puerto TCP 445; las conexiones producirán errores si el puerto 445 está bloqueado. Otra forma de comprobar si el firewall está bloqueando el puerto 445 es usar el cmdlet `Test-NetConnection`. Puede obtener información sobre [diversos métodos para solucionar el bloqueo del puerto 445 aquí](https://docs.microsoft.com/azure/storage/files/storage-troubleshoot-windows-file-connection-problems#cause-1-port-445-is-blocked).

    En el siguiente código de PowerShell se da por hecho que tiene instalado el módulo Azure PowerShell. Para más información, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps). No olvide reemplazar `<your-storage-account-name>` y `<your-resource-group-name>` por los nombres correspondientes de su cuenta de almacenamiento.

    ```powershell
    $resourceGroupName = "<your-resource-group-name>"
    $storageAccountName = "<your-storage-account-name>"

    # This command requires you to be logged into your Azure account, run Login-AzAccount if you haven't
    # already logged in.
    $storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName

    # The ComputerName, or host, is <storage-account>.file.core.windows.net for Azure Public Regions.
    # $storageAccount.Context.FileEndpoint is used because non-Public Azure regions, such as sovereign clouds
    # or Azure Stack deployments, will have different hosts for Azure file shares (and other storage resources).
    Test-NetConnection -ComputerName ([System.Uri]::new($storageAccount.Context.FileEndPoint).Host) -Port 445
    ```

    Si la conexión se realizó correctamente, verá la siguiente salida:

    ```
    ComputerName     : <storage-account-host-name>
    RemoteAddress    : <storage-account-ip-address>
    RemotePort       : 445
    InterfaceAlias   : <your-network-interface>
    SourceAddress    : <your-ip-address>
    TcpTestSucceeded : True
    ```

    > [!Note]  
    > El comando anterior devuelve la dirección IP actual de la cuenta de almacenamiento. No se garantiza que esta dirección IP permanezca igual, podría cambiar en cualquier momento. No codifique de forma rígida esta dirección IP en los scripts o en una configuración de firewall. 

## <a name="using-an-azure-file-share-with-windows"></a>Uso de un recurso compartido de archivos de Azure con Windows
Para usar un recurso compartido de archivos de Azure con Windows, debe montarlo, lo que significa asignarle una letra de unidad o una ruta de acceso a un punto de montaje, o acceder a él mediante su [ruta de acceso UNC](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx). 

A diferencia de otros recursos compartidos de SMB con los que pueda haber interactuado, como los que están hospedados en un servidor de Windows, un servidor de Linux Samba o un dispositivo NAS, los recursos compartidos de archivos de Azure no admiten actualmente la autenticación Kerberos con la identidad de Active Directory (AD) o Azure Active Directory (AAD), si bien es una característica [en la que estamos trabajando](https://feedback.azure.com/forums/217298-storage/suggestions/6078420-acl-s-for-azurefiles). En su lugar, debe acceder a su recurso compartido de archivos de Azure con la clave de la cuenta de almacenamiento que lo contiene. Una clave de cuenta de almacenamiento es una clave de administrador para una cuenta de almacenamiento, lo que incluye los permisos de administrador de todos los archivos y carpetas dentro de un recurso compartido de archivos al que accede, y de todos los recursos compartidos de archivos y otros recursos de almacenamiento (blobs, colas, tablas, etc.) contenidos en la cuenta de almacenamiento. Si esta solución no es suficiente para su carga de trabajo, [Azure File Sync](storage-sync-files-planning.md) puede solucionar mientras tanto la falta de compatibilidad con la autenticación Kerberos y las listas ACL hasta que dicha compatibilidad esté disponible públicamente.

Un patrón común para elevar y desplazar aplicaciones de línea de negocio (LOB) que esperan un recurso compartido de archivos de SMB es usar un recurso compartido de archivos de Azure como alternativa a ejecutar un servidor de archivos de Windows dedicado en una máquina virtual de Azure. Un aspecto importante que se debe tener en cuenta para migrar correctamente una aplicación de línea de negocio para usar un recurso compartido de archivos de Azure es que muchas de estas aplicaciones se ejecutan en el contexto de una cuenta de servicio dedicada con permisos de sistema limitados y no en la cuenta administrativa de la máquina virtual. Por lo tanto, debe asegurarse de montar o guardar las credenciales del recurso compartido de archivos de Azure desde el contexto de la cuenta de servicio y no de la cuenta administrativa.

### <a name="persisting-azure-file-share-credentials-in-windows"></a>Persistencia de las credenciales del recurso compartido de archivos de Azure en Windows  
La utilidad [cmdkey](https://docs.microsoft.com/windows-server/administration/windows-commands/cmdkey) le permite almacenar las credenciales de la cuenta de almacenamiento en Windows. Esto significa que al intentar acceder a un recurso compartido de archivos de Azure mediante su ruta de acceso UNC o montar el recurso compartido de archivos de Azure, no necesitará especificar las credenciales. Para guardar las credenciales de la cuenta de almacenamiento, ejecute los siguientes comandos de PowerShell, y sustituya `<your-storage-account-name>` y `<your-resource-group-name>` donde corresponda.

```powershell
$resourceGroupName = "<your-resource-group-name>"
$storageAccountName = "<your-storage-account-name>"

# These commands require you to be logged into your Azure account, run Login-AzAccount if you haven't
# already logged in.
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName
$storageAccountKeys = Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName

# The cmdkey utility is a command-line (rather than PowerShell) tool. We use Invoke-Expression to allow us to 
# consume the appropriate values from the storage account variables. The value given to the add parameter of the
# cmdkey utility is the host address for the storage account, <storage-account>.file.core.windows.net for Azure 
# Public Regions. $storageAccount.Context.FileEndpoint is used because non-Public Azure regions, such as sovereign 
# clouds or Azure Stack deployments, will have different hosts for Azure file shares (and other storage resources).
Invoke-Expression -Command ("cmdkey /add:$([System.Uri]::new($storageAccount.Context.FileEndPoint).Host) " + `
    "/user:AZURE\$($storageAccount.StorageAccountName) /pass:$($storageAccountKeys[0].Value)")
```

Puede comprobar que la utilidad cmdkey ha almacenado la credencial de la cuenta de almacenamiento mediante el parámetro de lista:

```powershell
cmdkey /list
```

Si las credenciales del recurso compartido de archivos de Azure están almacenadas correctamente, el resultado esperado es el siguiente (puede haber claves adicionales almacenadas en la lista):

```
Currently stored credentials:

Target: Domain:target=<storage-account-host-name>
Type: Domain Password
User: AZURE\<your-storage-account-name>
```

Ahora podrá montar o acceder al recurso compartido sin tener que proporcionar credenciales adicionales.

#### <a name="advanced-cmdkey-scenarios"></a>Escenarios avanzados de cmdkey
Es necesario tener en cuenta dos escenarios adicionales con cmdkey: el almacenamiento de credenciales de otro usuario en la máquina, como una cuenta de servicio, y el almacenamiento de credenciales en una máquina remota con la comunicación remota de PowerShell.

Almacenar las credenciales de otro usuario en la máquina es sencillo: cuando haya iniciado sesión en su cuenta, simplemente ejecute el siguiente comando de PowerShell:

```powershell
$password = ConvertTo-SecureString -String "<service-account-password>" -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "<service-account-username>", $password
Start-Process -FilePath PowerShell.exe -Credential $credential -LoadUserProfile
```

Se abre una nueva ventana de PowerShell en el contexto de usuario de la cuenta de servicio (o la cuenta de usuario). A continuación, puede usar la utilidad cmdkey tal como se ha descrito [anteriormente](#persisting-azure-file-share-credentials-in-windows).

No obstante, almacenar las credenciales en una máquina remota mediante la comunicación remota de PowerShell no es posible, dado que cmdkey no permite el acceso, ni siquiera para agregar algo, a su almacén de credenciales cuando se inicia la sesión del usuario mediante la comunicación remota de PowerShell. Se recomienda iniciar sesión en la máquina con [Escritorio remoto](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/windows).

### <a name="mount-the-azure-file-share-with-powershell"></a>Montaje del recurso compartido de archivos de Azure con PowerShell
Ejecute los siguientes comandos desde una sesión normal (sin privilegios elevados) de PowerShell para montar el recurso compartido de archivos de Azure. No olvide reemplazar `<your-resource-group-name>`, `<your-storage-account-name>`, `<your-file-share-name>` y `<desired-drive-letter>` por la información adecuada.

```powershell
$resourceGroupName = "<your-resource-group-name>"
$storageAccountName = "<your-storage-account-name>"
$fileShareName = "<your-file-share-name>"

# These commands require you to be logged into your Azure account, run Login-AzAccount if you haven't
# already logged in.
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName
$storageAccountKeys = Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName
$fileShare = Get-AzStorageShare -Context $storageAccount.Context | Where-Object { 
    $_.Name -eq $fileShareName -and $_.IsSnapshot -eq $false
}

if ($fileShare -eq $null) {
    throw [System.Exception]::new("Azure file share not found")
}

# The value given to the root parameter of the New-PSDrive cmdlet is the host address for the storage account, 
# <storage-account>.file.core.windows.net for Azure Public Regions. $fileShare.StorageUri.PrimaryUri.Host is 
# used because non-Public Azure regions, such as sovereign clouds or Azure Stack deployments, will have different 
# hosts for Azure file shares (and other storage resources).
$password = ConvertTo-SecureString -String $storageAccountKeys[0].Value -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "AZURE\$($storageAccount.StorageAccountName)", $password
New-PSDrive -Name <desired-drive-letter> -PSProvider FileSystem -Root "\\$($fileShare.StorageUri.PrimaryUri.Host)\$($fileShare.Name)" -Credential $credential -Persist
```

> [!Note]  
> La opción `-Persist` del cmdlet `New-PSDrive` solo permitirá que se vuelva a montar el recurso compartido de archivos durante el arranque si las credenciales están guardadas. Puede guardar las credenciales mediante la utilidad cmdkey como [se ha descrito anteriormente](#persisting-azure-file-share-credentials-in-windows). 

Si quiere, puede desmontar el recurso compartido de archivos de Azure con el siguiente cmdlet de PowerShell.

```powershell
Remove-PSDrive -Name <desired-drive-letter>
```

### <a name="mount-the-azure-file-share-with-file-explorer"></a>Montaje del recurso compartido de archivos de Azure con el Explorador de archivos
> [!Note]  
> Tenga en cuenta que las instrucciones siguientes se muestran en Windows 10 y pueden variar ligeramente en las versiones anteriores. 

1. Abra el Explorador de archivos. Para ello, puede abrir el menú Inicio o presionar el método abreviado Win+E.

1. Vaya al elemento **Este PC** del lado izquierdo de la ventana. Esta operación cambiará los menús disponibles en la barra de herramientas. En el menú Equipo, seleccione **Conectar a unidad de red**.
    
    ![Captura de pantalla del menú desplegable "Conectar a unidad de red"](./media/storage-how-to-use-files-windows/1_MountOnWindows10.png)

1. Seleccione la letra de unidad y escriba la ruta de acceso UNC; el formato de esta ruta es `<storageAccountName>.file.core.windows.net/<fileShareName>`. Por ejemplo: `anexampleaccountname.file.core.windows.net/example-share-name`.
    
    ![Captura de pantalla del cuadro de diálogo "Conectar a unidad de red"](./media/storage-how-to-use-files-windows/2_MountOnWindows10.png)

1. Use el nombre de la cuenta de almacenamiento prefijada con `AZURE\` como el nombre de usuario y la clave de la cuenta de almacenamiento como contraseña.
    
    ![Captura de pantalla del cuadro de diálogo credenciales de red](./media/storage-how-to-use-files-windows/3_MountOnWindows10.png)

1. Use el recurso compartido de archivos de Azure como prefiera.
    
    ![El recurso compartido de archivos de Azure ahora está montado.](./media/storage-how-to-use-files-windows/4_MountOnWindows10.png)

1. Cuando esté listo para desmontar el recurso compartido de archivos de Azure, puede hacerlo si hace clic con el botón derecho en la entrada del recurso compartido en **Ubicaciones de red** en el Explorador de archivos y selecciona **Desconectar**.

### <a name="accessing-share-snapshots-from-windows"></a>Acceso a instantáneas de recursos compartido de Windows
Si ha realizado una instantánea de un recurso compartido, ya sea manualmente o automáticamente a través de un script o un servicio como Azure Backup, puede ver las versiones anteriores de un recurso compartido, un directorio o un archivo concreto desde el recurso compartido de archivos en Windows. Las instantáneas de recursos compartidos se pueden realizar desde [Azure Portal](storage-how-to-use-files-portal.md), [Azure PowerShell](storage-how-to-use-files-powershell.md) y la [CLI de Azure](storage-how-to-use-files-cli.md).

#### <a name="list-previous-versions"></a>Enumeración de versiones anteriores
Vaya al elemento o elemento principal que hay que restaurar. Haga doble clic para ir al directorio deseado. Haga clic con el botón derecho y seleccione **Propiedades** en el menú.

![Haga clic con el botón derecho en el menú para ir a un directorio seleccionado](./media/storage-how-to-use-files-windows/snapshot-windows-previous-versions.png)

Seleccione **Versiones anteriores** para ver la lista de instantáneas de recursos compartidos de este directorio. La lista puede tardar unos segundos en cargarse dependiendo de la velocidad de red y del número de instantáneas de recursos compartidos que haya en el directorio.

![Pestaña Versiones anteriores](./media/storage-how-to-use-files-windows/snapshot-windows-list.png)

Puede seleccionar **Abrir** para abrir una instantánea concreta. 

![Instantánea abierta](./media/storage-how-to-use-files-windows/snapshot-browse-windows.png)

#### <a name="restore-from-a-previous-version"></a>Restaurar desde una versión anterior
Haga clic en **Restaurar** para copiar el contenido de todo un directorio de forma recursiva en el momento de la creación de la instantánea del recurso compartido en la ubicación original.

 ![Botón Restaurar en mensaje de advertencia](./media/storage-how-to-use-files-windows/snapshot-windows-restore.png) 

## <a name="securing-windowswindows-server"></a>Protección de Windows y Windows Server
Para montar un recurso compartido de archivos de Azure en Windows, el puerto 445 debe estar accesible. Muchas organizaciones bloquean este puerto debido a los riesgos de seguridad inherentes a SMB 1. SMB 1, también conocido como CIFS (Sistema de archivos de Internet común), es un protocolo de sistema de archivos heredado que se incluye con Windows y Windows Server. SMB 1 es un protocolo obsoleto, ineficaz y, lo más importante, no seguro. La buena noticia es que Azure Files no admite SMB 1, y todas las versiones admitidas de Windows y Windows Server permiten quitar o deshabilitar SMB 1. Siempre se [recomienda firmemente](https://aka.ms/stopusingsmb1) quitar o deshabilitar el cliente y el servidor de SMB 1 en Windows antes de usar los recursos compartidos de archivos de Azure en producción.

En la tabla siguiente se proporciona información detallada sobre el estado de SMB 1 de cada versión de Windows:

| Versión de Windows                           | Estado predeterminado de SMB 1 | Método "deshabilitar o quitar"       | 
|-------------------------------------------|----------------------|-----------------------------|
| Windows Server 2019                       | Disabled             | Quitar con la característica de Windows |
| Windows Server, versiones 1709 +            | Disabled             | Quitar con la característica de Windows |
| Windows 10, versiones 1709 +                | Disabled             | Quitar con la característica de Windows |
| Windows Server 2016                       | habilitado              | Quitar con la característica de Windows |
| Windows 10, versiones 1507, 1607 y 1703 | habilitado              | Quitar con la característica de Windows |
| Windows Server 2012 R2                    | habilitado              | Quitar con la característica de Windows | 
| Windows 8.1                               | habilitado              | Quitar con la característica de Windows | 
| Windows Server 2012                       | habilitado              | Deshabilitar con el Registro       | 
| Windows Server 2008 R2                    | habilitado              | Deshabilitar con el Registro       |
| Windows 7                                 | habilitado              | Deshabilitar con el Registro       | 

### <a name="auditing-smb-1-usage"></a>Auditoría del uso de SMB 1
> Se aplica a Windows Server 2019, el canal semestral de Windows Server (versiones 1709 y 1803), Windows Server 2016, Windows 10 (versiones 1507, 1607, 1703, 1709 y 1803), Windows Server 2012 R2 y Windows 8.1.

Antes de quitar SMB 1 de su entorno, quizás quiera auditar su uso para ver si el cambio interrumpirá a los clientes. Si las solicitudes se realizan con respecto a recursos compartidos de SMB con SMB 1, se registrará un evento de auditoría en el registro de eventos en `Applications and Services Logs > Microsoft > Windows > SMBServer > Audit`. 

> [!Note]  
> Para habilitar la compatibilidad con auditoría en Windows Server 2012 R2 y Windows 8.1, instale al menos [KB4022720](https://support.microsoft.com/help/4022720/windows-8-1-windows-server-2012-r2-update-kb4022720).

Para habilitar la auditoría, ejecute el siguiente cmdlet desde una sesión de PowerShell con privilegios elevados:

```powershell
Set-SmbServerConfiguration –AuditSmb1Access $true
```

### <a name="removing-smb-1-from-windows-server"></a>Eliminación de SMB 1 de Windows Server
> Se aplica a Windows Server 2019, el canal semestral de Windows Server (versiones 1709 y 1803), Windows Server 2016 y Windows Server 2012 R2.

Para quitar SMB 1 de una instancia de Windows Server, ejecute el siguiente cmdlet desde una sesión de PowerShell con privilegios elevados:

```powershell
Remove-WindowsFeature -Name FS-SMB1
```

Para completar el proceso de eliminación, reinicie el servidor. 

> [!Note]  
> A partir de Windows 10 y Windows Server versión 1709, SMB 1 no está instalado de forma predeterminada y las características de Windows para el cliente de SMB 1 y el servidor de SMB 1 son diferentes. Se recomienda siempre dejar sin instalar el servidor de SMB 1 (`FS-SMB1-SERVER`) y el cliente de SMB 1 (`FS-SMB1-CLIENT`).

### <a name="removing-smb-1-from-windows-client"></a>Eliminación de SMB 1 del cliente de Windows
> Se aplica a Windows 10 (versiones 1507, 1607, 1703, 1709 y 1803) y Windows 8.1.

Para quitar SMB 1 del cliente de Windows, ejecute el siguiente cmdlet desde una sesión de PowerShell con privilegios elevados:

```powershell
Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
```

Para completar el proceso de eliminación, reinicie el equipo.

### <a name="disabling-smb-1-on-legacy-versions-of-windowswindows-server"></a>Deshabilitación de SMB 1 en versiones heredadas de Windows y Windows Server
> Se aplica a Windows Server 2012, Windows Server 2008 R2 y Windows 7.

No se puede quitar completamente SMB 1 en versiones heredadas de Windows y Windows Server, pero se puede deshabilitar mediante el Registro. Para deshabilitar SMB 1, cree una nueva clave del Registro `SMB1` de tipo `DWORD` con un valor de `0` en `HKEY_LOCAL_MACHINE > SYSTEM > CurrentControlSet > Services > LanmanServer > Parameters`.

Esto lo puede hacer fácilmente también con el siguiente cmdlet de PowerShell:

```powershell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB1 -Type DWORD -Value 0 –Force
```

Después de crear esta clave del Registro, debe reiniciar el servidor para deshabilitar SMB 1.

### <a name="smb-resources"></a>Recursos de SMB
- [Stop using SMB 1](https://blogs.technet.microsoft.com/filecab/2016/09/16/stop-using-smb1/) (Dejar de usar SMB1)
- [SMB 1 Product Clearinghouse](https://blogs.technet.microsoft.com/filecab/2017/06/01/smb1-product-clearinghouse/) (Centro de enrutamiento de productos de SMB 1)
- [Discover SMB 1 in your environment with DSCEA](https://blogs.technet.microsoft.com/ralphkyttle/2017/04/07/discover-smb1-in-your-environment-with-dscea/) (Detección de SMB 1 en el entorno con DSCEA)
- [Disabling SMB 1 through Group Policy](https://blogs.technet.microsoft.com/secguide/2017/06/15/disabling-smbv1-through-group-policy/) (Deshabilitación de SMB 1 mediante directiva de grupo)

## <a name="next-steps"></a>Pasos siguientes
Consulte los vínculos siguientes para más información sobre Azure Files:
- [Planeamiento de una implementación de Azure Files](storage-files-planning.md)
- [P+F](../storage-files-faq.md)
- [Solución de problemas en Windows](storage-troubleshoot-windows-file-connection-problems.md)      
