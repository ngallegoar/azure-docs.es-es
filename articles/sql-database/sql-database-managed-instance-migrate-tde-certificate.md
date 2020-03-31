---
title: 'Migración de certificados TDE: Instancia administrada'
description: Migración del certificado que protege la clave de cifrado de base de datos de una base de datos con cifrado de datos transparente a Instancia administrada de Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: carlrab, jovanpop
ms.date: 04/25/2019
ms.openlocfilehash: 0f6e379287323d9353acd887cf30d5c9c0065959
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74555380"
---
# <a name="migrate-certificate-of-tde-protected-database-to-azure-sql-database-managed-instance"></a>Migración del certificado de una base de datos protegida por TDE a Instancia administrada de Azure SQL Database

Al migrar una base de datos protegida mediante el [Cifrado de datos transparente](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption) a la Instancia administrada de Azure SQL Database mediante la opción de restauración nativa, se debe migrar el certificado correspondiente de SQL Server local o IaaS antes de restaurar la base de datos. Este artículo le guía en el proceso de migración manual del certificado a Instancia administrada de Azure SQL Database:

> [!div class="checklist"]
> * Exportación del certificado a un archivo de intercambio de información personal (.pfx)
> * Extracción del certificado del archivo a la cadena de base 64
> * Carga del archivo mediante el cmdlet de PowerShell

Para ver una opción alternativa que emplea el servicio totalmente administrado para la migración completa de la base de datos protegida con TDE y el certificado correspondiente, consulte [Migración de la base de datos local a Instancia administrada mediante Azure Database Migration Service](../dms/tutorial-sql-server-to-managed-instance.md).

> [!IMPORTANT]
> El certificado migrado se usa únicamente para la restauración de la base de datos protegida por TDE. Poco después de que se realiza la restauración, el certificado migrado se reemplaza por un protector diferente, ya sea un certificado administrado por el servicio o una clave asimétrica del almacén de claves, según el tipo de cifrado de datos transparentes que haya establecido en la instancia.

## <a name="prerequisites"></a>Prerequisites

Para completar los pasos de este artículo, necesitará lo siguiente:

- Tener instalada la herramienta de la línea de comandos [Pvk2Pfx](https://docs.microsoft.com/windows-hardware/drivers/devtest/pvk2pfx) en el servidor local o en otro equipo con acceso al certificado exportado como un archivo. La herramienta Pvk2Pfx forma parte del [kit de controladores de Windows para empresa](https://docs.microsoft.com/windows-hardware/drivers/download-the-wdk), un entorno de línea de comandos autocontenido independiente.
- Tener instalado [Windows PowerShell](/powershell/scripting/install/installing-windows-powershell), versión 5.0 o superior.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Asegúrese de que dispone de lo siguiente:

- Tener [instalado y actualizado](https://docs.microsoft.com/powershell/azure/install-az-ps) el módulo de Azure PowerShell.
- [Módulo Az.Sql](https://www.powershellgallery.com/packages/Az.Sql).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

> [!IMPORTANT]
> El módulo de Azure Resource Manager para PowerShell todavía es compatible con Azure SQL Database, pero todo el desarrollo futuro se realizará para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos del módulo Az y los módulos AzureRm son esencialmente idénticos.

Ejecute los comandos siguientes de PowerShell para instalar o actualizar el módulo:

```azurepowershell
Install-Module -Name Az.Sql
Update-Module -Name Az.Sql
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

* * *

## <a name="export-tde-certificate-to-a-personal-information-exchange-pfx-file"></a>Exportación del certificado TDE a un archivo de intercambio de información personal (.pfx)

El certificado se puede exportar directamente desde la instancia de SQL Server de origen o desde el almacén de certificados si se conserva ahí.

### <a name="export-certificate-from-the-source-sql-server"></a>Exportación del certificado desde la instancia de SQL Server de origen

Siga estos pasos para exportar el certificado con SQL Server Management Studio y convertirlo a formato pfx. En los pasos se usan los nombres genéricos *TDE_Cert* y *full_path* para los nombres de certificados y archivos y las rutas de acceso. Estos se deben reemplazar por los nombres reales.

1. En SSMS, abra una nueva ventana de consulta y conéctese a la instancia de SQL Server de origen.

1. Use el siguiente script para enumerar las bases de datos protegidas por TDE y obtener el nombre del certificado que protege el cifrado de la base de datos que se va a migrar:

   ```sql
   USE master
   GO
   SELECT db.name as [database_name], cer.name as [certificate_name]
   FROM sys.dm_database_encryption_keys dek
   LEFT JOIN sys.certificates cer
   ON dek.encryptor_thumbprint = cer.thumbprint
   INNER JOIN sys.databases db
   ON dek.database_id = db.database_id
   WHERE dek.encryption_state = 3
   ```

   ![Lista de certificados TDE](./media/sql-database-managed-instance-migrate-tde-certificate/onprem-certificate-list.png)

1. Ejecute el script siguiente para exportar el certificado a un par de archivos (.cer y .pvk), y mantener así la información de clave pública y privada:

   ```sql
   USE master
   GO
   BACKUP CERTIFICATE TDE_Cert
   TO FILE = 'c:\full_path\TDE_Cert.cer'
   WITH PRIVATE KEY (
     FILE = 'c:\full_path\TDE_Cert.pvk',
     ENCRYPTION BY PASSWORD = '<SomeStrongPassword>'
   )
   ```

   ![Copia de seguridad del certificado TDE](./media/sql-database-managed-instance-migrate-tde-certificate/backup-onprem-certificate.png)

1. Use la consola de PowerShell para copiar la información del certificado de un par de archivos recién creados en un archivo de intercambio de información personal (.pfx), mediante la herramienta Pvk2Pfx:

   ```cmd
   .\pvk2pfx -pvk c:/full_path/TDE_Cert.pvk  -pi "<SomeStrongPassword>" -spc c:/full_path/TDE_Cert.cer -pfx c:/full_path/TDE_Cert.pfx
   ```

### <a name="export-certificate-from-certificate-store"></a>Exportar el certificado desde el almacén de certificados

Si el certificado se guarda en el almacén de certificados de la máquina local de SQL Server, se puede exportar mediante los siguientes pasos:

1. Abra la consola de PowerShell y ejecute el siguiente comando para abrir el complemento Certificados de Microsoft Management Console:

   ```cmd
   certlm
   ```

2. En el complemento Certificados de MMC, expanda la ruta de acceso Personal -> Certificados para ver la lista de certificados.

3. Haga clic con el botón derecho en el certificado y haga clic en Exportar.

4. Siga el asistente para exportar el certificado y la clave privada a un formato de intercambio de información personal.

## <a name="upload-certificate-to-azure-sql-database-managed-instance-using-azure-powershell-cmdlet"></a>Carga del certificado en Instancia administrada de Azure SQL Database mediante el cmdlet de Azure PowerShell

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. Comience con los pasos de preparación de PowerShell:

   ```azurepowershell
   # import the module into the PowerShell session
   Import-Module Az
   # connect to Azure with an interactive dialog for sign-in
   Connect-AzAccount
   # list subscriptions available and copy id of the subscription target Managed Instance belongs to
   Get-AzSubscription
   # set subscription for the session
   Select-AzSubscription <subscriptionId>
   ```

2. Una vez realizados todos los pasos de preparación, ejecute los siguientes comandos para cargar el certificado codificado en base 64 en la instancia administrada de destino:

   ```azurepowershell
   $fileContentBytes = Get-Content 'C:/full_path/TDE_Cert.pfx' -Encoding Byte
   $base64EncodedCert = [System.Convert]::ToBase64String($fileContentBytes)
   $securePrivateBlob = $base64EncodedCert  | ConvertTo-SecureString -AsPlainText -Force
   $password = "<password>"
   $securePassword = $password | ConvertTo-SecureString -AsPlainText -Force
   Add-AzSqlManagedInstanceTransparentDataEncryptionCertificate -ResourceGroupName "<resourceGroupName>" `
       -ManagedInstanceName "<managedInstanceName>" -PrivateBlob $securePrivateBlob -Password $securePassword
   ```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Primero debe [configurar un almacén de Azure Key Vault](/azure/key-vault/key-vault-manage-with-cli2) con el archivo *.pfx*.

1. Comience con los pasos de preparación de PowerShell:

   ```azurecli
   # connect to Azure with an interactive dialog for sign-in
   az login

   # list subscriptions available and copy id of the subscription target Managed Instance belongs to
   az account list

   # set subscription for the session
   az account set --subscription <subscriptionId>
   ```

1. Una vez realizados todos los pasos de preparación, ejecute los siguientes comandos para cargar el certificado codificado en base 64 en la instancia administrada de destino:

   ```azurecli
   az sql mi tde-key set --server-key-type AzureKeyVault --kid "<keyVaultId>" `
       --managed-instance "<managedInstanceName>" --resource-group "<resourceGroupName>"
   ```

* * *

El certificado ya está disponible para la instancia administrada especificada y la copia de seguridad de la base de datos protegida por TDE correspondiente se puede restaurar correctamente.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprendió a migrar el certificado que protege la clave de cifrado de base de datos con Cifrado de datos transparente, de una instancia local o IaaS de SQL Server a Instancia administrada de Azure SQL Database.

Para saber cómo restaurar la copia de seguridad de una base de datos en Instancia administrada de Azure SQL Database, consulte [Restauración de una copia de seguridad de datos en una instancia administrada de Azure SQL Database](sql-database-managed-instance-get-started-restore.md).
