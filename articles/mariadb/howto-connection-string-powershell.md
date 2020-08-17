---
title: 'Generación de una cadena de conexión con PowerShell: Azure Database for MariaDB'
description: Este artículo proporciona un ejemplo de Azure PowerShell en el que se genera una cadena de conexión para conectarse a Azure Database for MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.custom: mvc, devx-track-azurepowershell
ms.topic: quickstart
ms.date: 8/5/2020
ms.openlocfilehash: b4b0853cb71ccc85826d1230fb7828ed9df24b52
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87839959"
---
# <a name="how-to-generate-an-azure-database-for-mariadb-connection-string-with-powershell"></a>Cómo generar una cadena de conexión de Azure Database for MariaDB con PowerShell

En este artículo se muestra cómo generar una cadena de conexión para un servidor de Azure Database for MariaDB. Puede usar una cadena de conexión para conectarse a Azure Database for MariaDB desde muchas aplicaciones diferentes.

## <a name="requirements"></a>Requisitos

En este artículo se usan los recursos creados en la siguiente guía como punto de partida:

* [Inicio rápido: Creación de un servidor de Azure Database for MariaDB mediante PowerShell](quickstart-create-mariadb-server-database-using-azure-powershell.md)

## <a name="get-the-connection-string"></a>Obtención de la cadena de conexión

El cmdlet `Get-AzMariaDbConnectionString` se usa para generar una cadena de conexión para conectar aplicaciones a Azure Database for MariaDB. En el ejemplo siguiente se devuelve la cadena de conexión de un cliente PHP desde **mydemoserver**.

```azurepowershell-interactive
Get-AzMariaDbConnectionString -Client PHP -Name mydemoserver -ResourceGroupName myresourcegroup
```

```Output
$con=mysqli_init();mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL); mysqli_real_connect($con, "mydemoserver.mariadb.database.azure.com", "myadmin@mydemoserver", {your_password}, {your_database}, 3306);
```

Los valores válidos del parámetro `Client` incluyen:

* ADO&#46;NET
* JDBC
* Node.js
* PHP
* Python
* Ruby
* WebApp

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Personalización de parámetros de servidor de Azure Database for MariaDB mediante PowerShell](howto-configure-server-parameters-using-powershell.md)
