---
title: Configuración de parámetros de Azure Database for MariaDB mediante Azure PowerShell
description: En este artículo se describe cómo configurar los parámetros de servicio de Azure Database for MariaDB mediante PowerShell.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurepowershell
ms.topic: conceptual
ms.date: 5/26/2020
ms.openlocfilehash: d7a6f87b704a2e366e0a4e1fc647e83ed88c486c
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84039416"
---
# <a name="customize-azure-database-for-mariadb-server-parameters-using-powershell"></a>Personalización de parámetros de servidor de Azure Database for MariaDB mediante PowerShell

Puede enumerar, mostrar y actualizar los parámetros de configuración de un servidor de Azure Database for MariaDB mediante PowerShell. En el nivel del servidor, se expone y se puede modificar un subconjunto de las opciones de configuración del motor.

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía, necesita:

- El [módulo Az PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps) instalado localmente o [Azure Cloud Shell](https://shell.azure.com/) en el explorador
- Un [servidor de Azure Database for MariaDB](quickstart-create-mariadb-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> Mientras el módulo de PowerShell Az.MariaDb se encuentre en versión preliminar, debe instalarlo por separado desde el módulo Az de PowerShell con el siguiente comando: `Install-Module -Name Az.MariaDb -AllowPrerelease`.
> Una vez que el módulo Az.MariaDb de PowerShell esté disponible con carácter general, formará parte de las futuras versiones del módulo Az de PowerShell y estará disponible de forma nativa en Azure Cloud Shell.

Si decide usar PowerShell de forma local, conéctese a su cuenta de Azure con el cmdlet [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="list-server-configuration-parameters-for-azure-database-for-mariadb-server"></a>Lista de los parámetros de configuración del servidor de Azure Database for MariaDB

Para obtener una lista de todos los parámetros modificables en un servidor y sus valores, ejecute el cmdlet `Get-AzMariaDbConfiguration`.

En el ejemplo siguiente se enumeran los parámetros de configuración del servidor **mydemoserver** incluido en el grupo de recursos **myresourcegroup**.

```azurepowershell-interactive
Get-AzMariaDbConfiguration -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

Para ver la definición de cada uno de los parámetros enumerados, consulte la sección de referencia de MariaDB en [Server System Variables](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html) (Variables de sistema del servidor).

## <a name="show-server-configuration-parameter-details"></a>Presentación de los detalles de los parámetros de configuración del servidor

Para mostrar los detalles de un parámetro de configuración determinado de un servidor, ejecute el cmdlet `Get-AzMariaDbConfiguration` y especifique el parámetro **Name**.

En este ejemplo se muestran detalles del parámetro de configuración **slow\_query\_log** del servidor **mydemoserver** incluido en el grupo de recursos **myresourcegroup**.

```azurepowershell-interactive
Get-AzMariaDbConfiguration -Name slow_query_log -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

## <a name="modify-a-server-configuration-parameter-value"></a>Modificación de un valor de los parámetros de configuración del servidor

También puede modificar el valor de un determinado parámetro de configuración del servidor, lo que actualizará el valor de configuración subyacente del motor del servidor de MariaDB. Para actualizar la configuración, use el cmdlet `Update-AzMariaDbConfiguration`.

Para actualizar el parámetro de configuración **slow\_query\_log** del servidor **mydemoserver** incluido en el grupo de recursos **myresourcegroup**.

```azurepowershell-interactive
Update-AzMariaDbConfiguration -Name slow_query_log -ResourceGroupName myresourcegroup -ServerName mydemoserver -Value On
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Crecimiento automático del almacenamiento en el servidor de Azure Database for MariaDB mediante PowerShell](howto-auto-grow-storage-powershell.md).
