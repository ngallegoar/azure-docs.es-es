---
title: 'Inicio rápido: Creación de un área de trabajo de Synapse mediante Azure PowerShell'
description: Siga los pasos de esta guía para crear un área de trabajo de Azure Synapse mediante Azure PowerShell.
services: synapse-analytics
author: alehall
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: overview
ms.date: 10/19/2020
ms.author: alehall
ms.reviewer: jrasnick
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 005e3a3b717d4b1b8e5eb02b77a1d228908f8707
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92210587"
---
# <a name="quickstart-create-an-azure-synapse-workspace-with-azure-powershell"></a>Inicio rápido: Creación de un área de trabajo de Azure Synapse con Azure PowerShell

Azure PowerShell es un conjunto de cmdlets para administrar los recursos de Azure directamente desde PowerShell. Puede utilizarlo en el explorador con Azure Cloud Shell. También puede instalarlo en macOS, Linux o Windows.

En este inicio rápido, aprenderá a crear un área de trabajo de Synapse mediante Azure PowerShell.

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

- Una [cuenta de almacenamiento de Azure Data Lake Storage Gen2](../storage/common/storage-account-create.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

    > [!IMPORTANT]
    > El área de trabajo de Azure Synapse debe poder leer y escribir en la cuenta de ADLS Gen2 seleccionada. Para cualquier cuenta de almacenamiento que vincule como cuenta de almacenamiento principal, debe haber habilitado el **espacio de nombres jerárquico** al crearla, como se describe en la sección [Creación de una cuenta de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-account-create?tabs=azure-powershell#create-a-storage-account).

Si decide usar Cloud Shell, consulte [Introducción a Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) para más información.

### <a name="install-the-azure-powershell-module-locally"></a>Instalación del módulo de Azure PowerShell de forma local

Si decide usar PowerShell de forma local, para este artículo es preciso que instale el módulo Az PowerShell y que se conecte a su cuenta de Azure con el cmdlet [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount). Para más información sobre cómo instalar el módulo Az PowerShell, consulte [Instalación de Azure PowerShell](/powershell/azure/install-az-ps).

Para más información sobre la autenticación con Azure PowerShell, consulte [Inicio de sesión con Azure PowerShell](/powershell/azure/authenticate-azureps).

### <a name="install-the-azure-synapse-powershell-module"></a>Instalación del módulo de PowerShell de Azure Synapse

> [!IMPORTANT]
> Aunque el módulo **Az.HPCCache** de PowerShell está en versión preliminar, se debe instalar por separado mediante el cmdlet `Install-Module`. Una vez que este módulo de PowerShell esté disponible con carácter general, formará parte de las futuras versiones del módulo Az de PowerShell y estará disponible de forma predeterminada en Azure Cloud Shell.

```azurepowershell-interactive
Install-Module -Name Az.Synapse
```

## <a name="create-an-azure-synapse-workspace-using-azure-powershell"></a>Creación de un área de trabajo de Azure Synapse mediante Azure PowerShell

1. Defina las variables de entorno necesarias para crear recursos para el área de trabajo de Azure Synapse.

   |        Nombre de la variable        |                                                 Descripción                                                 |
   | --------------------------- | ----------------------------------------------------------------------------------------------------------- |
   | StorageAccountName          | Asigne un nombre a la cuenta de almacenamiento de ADLS Gen2 existente.                                                           |
   | StorageAccountResourceGroup | Nombre del grupo de recursos de la cuenta de almacenamiento de ADLS Gen2 existente.                                             |
   | FileShareName               | Nombre del sistema de archivos de almacenamiento existente.                                                                  |
   | SynapseResourceGroup        | Elija un nuevo nombre para el grupo de recursos de Azure Synapse.                                                    |
   | Región                      | Elija una de las [regiones de Azure](https://azure.microsoft.com/global-infrastructure/geographies/#overview). |
   | SynapseWorkspaceName        | Elija un nombre único para la nueva área de trabajo de Azure Synapse.                                                  |
   | SqlUser                     | Elija un valor para un nombre de usuario nuevo.                                                                          |
   | SqlPassword                 | Elija una contraseña segura.                                                                                   |
   | ClientIP                    | Dirección IP pública del sistema desde el que se ejecuta PowerShell.                                             |
   |                             |                                                                                                             |

1. Cree un grupo de recursos como contenedor para el área de trabajo de Azure Synapse:

   ```azurepowershell-interactive
   New-AzResourceGroup -Name $SynapseResourceGroup -Location $Region
   ```

1. Recupere la clave de la cuenta de almacenamiento de ADLS Gen 2:

   ```azurepowershell-interactive
   $StorageAccountKey = Get-AzStorageAccountKey -ResourceGroupName $StorageAccountResourceGroup -Name $StorageAccountName |
     Select-Object -First 1 -ExpandProperty Value
    ```

1. Recupere la dirección URL del punto de conexión de ADLS Gen 2:

   ```azurepowershell-interactive
   $StorageEndpointUrl = (Get-AzStorageAccount -ResourceGroupName $StorageAccountResourceGroup -Name $StorageAccountName).PrimaryEndpoints.Dfs
   ```

1. (Opcional) Siempre puede comprobar cuáles son el punto de conexión y la clave de la cuenta de almacenamiento de ADLS Gen2:

   ```azurepowershell-interactive
   Write-Output "Storage Account Key: $StorageAccountKey"
   Write-Output "Storage Endpoint URL: $StorageEndpointUrl"
   ```

1. Cree un área de trabajo de Azure Synapse:

   ```azurepowershell-interactive
   $Cred = New-Object -TypeName System.Management.Automation.PSCredential ($SqlUser, (ConvertTo-SecureString $SqlPassword -AsPlainText -Force))

   $WorkspaceParams = @{
     Name = $SynapseWorkspaceName
     ResourceGroupName = $SynapseResourceGroup
     DefaultDataLakeStorageAccountName = $StorageAccountName
     DefaultDataLakeStorageFilesystem = $FileShareName
     SqlAdministratorLoginCredential = $Cred
     Location = $Region
   }
   New-AzSynapseWorkspace @WorkspaceParams
   ```

1. Obtenga la dirección URL web y de desarrollo del área de trabajo de Azure Synapse:

   ```azurepowershell-interactive
   $WorkspaceWeb = (Get-AzSynapseWorkspace -Name $SynapseWorkspaceName -ResourceGroupName $StorageAccountResourceGroup).ConnectivityEndpoints.Web
   $WorkspaceDev = (Get-AzSynapseWorkspace -Name $SynapseWorkspaceName -ResourceGroupName $StorageAccountResourceGroup).ConnectivityEndpoints.Dev
   ```

1. Cree una regla de firewall que le permita acceder al área de trabajo de Azure Synapse desde su máquina:

   ```azurepowershell-interactive
   $FirewallParams = @{
     WorkspaceName = $SynapseWorkspaceName
     Name = 'Allow Client IP'
     ResourceGroupName = $StorageAccountResourceGroup
     StartIpAddress = $ClientIP
     EndIpAddress = $ClientIP
   }
   New-AzSynapseFirewallRule @FirewallParams
   ```

1. Abra la dirección URL web del área de trabajo de Azure Synapse almacenada en la variable de entorno `WorkspaceWeb` para acceder al área de trabajo:

   ```azurepowershell-interactive
   Start-Process $WorkspaceWeb
   ```

   ![Web del área de trabajo de Azure Synapse](media/quickstart-create-synapse-workspace-powershell/create-workspace-powershell-1.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Siga los pasos que se indican a continuación para eliminar el área de trabajo de Azure Synapse.

> [!WARNING]
> Al eliminar un área de trabajo de Azure Synapse, se quitarán también los motores de análisis y los datos almacenados en la base de datos de los grupos de SQL y metadatos de área de trabajo incluidos. Ya no será posible conectarse a los puntos de conexión de SQL o de Apache Spark. Se eliminarán todos los artefactos de código (consultas, cuadernos, definiciones de trabajos y canalizaciones). La eliminación del área de trabajo **no** afectará a los datos de Data Lake Store Gen2 vinculados al área de trabajo.

Si el área de trabajo de Azure Synapse que se creó en este artículo no es necesaria, puede eliminarla ejecutando el siguiente ejemplo.

```azurepowershell-interactive
Remove-AzSynapseWorkspace -Name $SynapseWorkspaceNam -ResourceGroupName $SynapseResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

A continuación, puede [crear grupos de SQL](quickstart-create-sql-pool-studio.md) o [crear grupos de Apache Spark](quickstart-create-apache-spark-pool-studio.md) para empezar a analizar y explorar los datos.
