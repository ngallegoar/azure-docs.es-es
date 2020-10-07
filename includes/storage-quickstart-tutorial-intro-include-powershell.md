---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 1f8f8d314a8bb37a08b3696f597b395a8a4beb8e
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "67186539"
---
## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en la suscripción a Azure con el comando `Connect-AzAccount` y siga las instrucciones de la pantalla.

```powershell
Connect-AzAccount
```

Si no sabe qué ubicación desea usar, puede enumerar las ubicaciones disponibles. Muestre la lista de ubicaciones; para ello, use el siguiente ejemplo de código y busque la que desea usar. En este ejemplo se usa **eastus**. Almacénelo en una variable y úsela para que pueda cambiar este valor en un solo lugar.

```powershell
Get-AzLocation | select Location
$location = "eastus"
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos de Azure con [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Un grupo de recursos es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

```powershell
$resourceGroup = "myResourceGroup"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

## <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

Cree una cuenta de almacenamiento estándar de uso general con replicación de almacenamiento con redundancia local mediante [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount). Después, obtenga el contexto de la cuenta de almacenamiento que define la cuenta de almacenamiento que desea usar. Cuando actúa en una cuenta de almacenamiento, hace referencia al contexto en lugar de proporcionar varias veces las credenciales. Use el siguiente ejemplo para crear una cuenta de almacenamiento denominada *mystorageaccount* con el cifrado de blob y el almacenamiento con redundancia local (LRS) (habilitados de forma predeterminada).

```powershell
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name "mystorageaccount" `
  -SkuName Standard_LRS `
  -Location $location `

$ctx = $storageAccount.Context
```
