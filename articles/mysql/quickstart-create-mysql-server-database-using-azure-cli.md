---
title: 'Inicio rápido: Creación de un servidor: de CLI de Azure (Azure Database for MySQL)'
description: En esta guía de inicio rápido se describe cómo usar la CLI de Azure para crear una Base de datos de Azure para el servidor MySQL en un grupo de recursos de Azure.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 01/09/2019
ms.custom: mvc
ms.openlocfilehash: acf5f3cdf761e1773d6e9384a4ceb99a645ed7cc
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "74773541"
---
# <a name="quickstart-create-an-azure-database-for-mysql-server-using-azure-cli"></a>Inicio rápido: Creación de una Base de datos de Azure para el servidor MySQL con la CLI de Azure

> [!TIP]
> Considere la posibilidad de usar un comando de la CLI de Azure [az mysql up](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-up) más sencillo (actualmente en versión preliminar). Pruebe el [inicio rápido](./quickstart-create-server-up-azure-cli.md).

En esta guía de inicio rápido se describe cómo usar la CLI de Azure para crear una Base de datos de Azure para el servidor MySQL en un grupo de recursos de Azure en unos cinco minutos. La CLI de Azure se usa para crear y administrar recursos de Azure desde la línea de comandos o en scripts.

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli). 

Si tiene varias suscripciones, elija la adecuada donde se encuentre el recurso o para la cual se facture. Seleccione un identificador de suscripción específico en su cuenta mediante el comando [az account set](/cli/azure/account#az-account-set).
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos
Cree un [grupo de recursos de Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) con el comando [az group create](/cli/azure/group#az-group-create). Un grupo de recursos es un contenedor lógico en el que se implementan y se administran recursos de Azure como un grupo.

En el ejemplo siguiente, se crea un grupo de recursos denominado `myresourcegroup` en la ubicación `westus`.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Creación de un servidor de Azure Database for MySQL
Cree un servidor de Azure Database for MySQL con el comando **[az mysql server create](/cli/azure/mysql/server#az-mysql-server-create)** . Un servidor puede administrar varias bases de datos. Normalmente se usa una base de datos independiente para cada proyecto o para cada usuario.

**Configuración** | **Valor de ejemplo** | **Descripción**
---|---|---
name | mydemoserver | Elija un nombre único que identifique al servidor de Azure Database for MySQL. El nombre del servidor solo puede contener letras minúsculas, números y el carácter de guion (-). Debe contener entre 3 y 63 caracteres.
resource-group | myresourcegroup | Especifique el nombre del grupo de recursos de Azure.
sku-name | GP_Gen5_2 | El nombre de la SKU. Sigue la convención {plan de tarifa}\_{generación de procesos}\_{núcleos virtuales} en forma abreviada. En esta misma tabla puede obtener más información acerca del parámetro sku-name.
backup-retention | 7 | Cuánto tiempo deben conservarse las copias de seguridad. La unidad es días. El intervalo es 7-35. 
geo-redundant-backup | Disabled | Si se deben habilitar las copias de seguridad con redundancia geográfica en este servidor o no. Valores permitidos: Enabled (Habilitada), Disabled (Deshabilitada).
ubicación | westus | La ubicación de Azure para el servidor.
ssl-enforcement | habilitado | Si debe ssl debe habilitarse, o no, en este servidor. Valores permitidos: Enabled (Habilitada), Disabled (Deshabilitada).
storage-size | 51200 | La capacidad de almacenamiento del servidor (la unidad es megabytes). El tamaño de almacenamiento válido un mínimo de 5120 MB y aumenta en incrementos de 1024 MB. Consulte el documento de los [planes de tarifa](./concepts-pricing-tiers.md) para más información acerca de los límites de tamaño de almacenamiento. 
version | 5.7 | La versión principal de MySQL.
admin-user | myadmin | El nombre del usuario del inicio de sesión del administrador. No puede ser **azure_superuser**, **admin**, **administrator**, **root**, **guest** o **public**.
admin-password | *contraseña segura* | La contraseña del usuario administrador. Debe tener entre 8 y 128 caracteres. La contraseña debe contener caracteres de tres de las siguientes categorías: Letras del alfabeto inglés mayúsculas y minúsculas, números y caracteres no alfanuméricos.


El valor del parámetro sku-name sigue la convención {plan de tarifa}\_{generación de proceso}\_{núcleos virtuales} como en los ejemplos siguientes:
+ `--sku-name B_Gen5_1` se asigna a Básico, Gen 5 y 1 núcleo virtual. Esta opción es la SKU más pequeña disponible.
+ `--sku-name GP_Gen5_32` se asigna a De uso general, Gen 5 y 32 núcleos virtuales.
+ `--sku-name MO_Gen5_2` se asigna a Optimizado para memoria, Gen 5 y 2 núcleos virtuales.

Para comprender cuáles son los valores válidos por región y nivel consulte la documentación sobre [planes de tarifa](./concepts-pricing-tiers.md).

En el ejemplo siguiente se crea un servidor MySQL 5.7 llamado `mydemoserver` en la región Oeste de EE. UU., en el grupo de recursos `myresourcegroup` y con el inicio de sesión de administrador de servidor `myadmin`. Se trata de un servidor **Gen 4** de **uso general** con **dos núcleos virtuales**. Sustituya `<server_admin_password>` por su propio valor.

```azurecli-interactive
az mysql server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 5.7
```

> [!NOTE]
> Considere la posibilidad de usar el plan de tarifa Básico si menos proceso y E/S resultan adecuados para su carga de trabajo. Tenga en cuenta que los servidores que creó en el plan de tarifa Básico no se podrán escalar más adelante a De uso general u Optimizada para memoria. Consulte la [página de precios](https://azure.microsoft.com/pricing/details/mysql/) para más información.
> 

## <a name="configure-firewall-rule"></a>Configurar de la regla de firewall
Cree una regla de firewall de nivel de servidor de Azure Database for MySQL con el comando **[az mysql server firewall-rule create](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-create)** . Una regla de firewall de nivel de servidor permite que una aplicación externa, como la herramienta de línea de comandos **mysql.exe** o MySQL Workbench, se conecte al servidor a través del firewall del servicio Azure MySQL. 

En el ejemplo siguiente se crea una regla de firewall denominada `AllowMyIP` que permite las conexiones desde una dirección IP específica, 192.168.0.1. Sustituya la dirección IP o rango de direcciones IP que se correspondan con aquella desde la que se está conectando. 

```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server mydemoserver --name AllowMyIP --start-ip-address 192.168.0.1 --end-ip-address 192.168.0.1
```

> [!NOTE]
> Las conexiones a Azure Database for MySQL se comunican a través del puerto 3306. Si intenta conectarse desde una red corporativa, es posible que no se permita el tráfico saliente a través del puerto 3306. En ese caso no podrá conectarse al servidor, salvo que el departamento de TI abra el puerto 3306.
> 


## <a name="configure-ssl-settings"></a>Configuración de SSL
De manera predeterminada, se exigen conexiones SSL entre su servidor y las aplicaciones cliente. Así se garantiza de forma predeterminada la seguridad de los datos "en movimiento" al cifrar el flujo de datos que circula por Internet. Para hacer que esta guía de inicio rápido sea más fácil, se han deshabilitado las conexiones SSL para su servidor. Esto no se recomienda en servidores de producción. Para más información, consulte [Configuración de la conectividad SSL en la aplicación para conectarse de forma segura a Azure Database for MySQL](./howto-configure-ssl.md).

En el ejemplo siguiente se deshabilita la aplicación de SSL en su servidor MySQL.
 
 ```azurecli-interactive
 az mysql server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Disabled
 ```

## <a name="get-the-connection-information"></a>Obtención de la información de conexión

Para conectarse al servidor, debe proporcionar las credenciales de acceso y la información del host.

```azurecli-interactive
az mysql server show --resource-group myresourcegroup --name mydemoserver
```

El resultado está en formato JSON. Tome nota de los valores de **fullyQualifiedDomainName** y **administratorLogin**.
```json
{
  "administratorLogin": "myadmin",
  "earliestRestoreDate": null,
  "fullyQualifiedDomainName": "mydemoserver.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMySQL/servers/mydemoserver",
  "location": "westus",
  "name": "mydemoserver",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 2,
    "family": "Gen5",
    "name": "GP_Gen5_2",
    "size": null,
    "tier": "GeneralPurpose"
  },
  "sslEnforcement": "Enabled",
  "storageProfile": {
    "backupRetentionDays": 7,
    "geoRedundantBackup": "Disabled",
    "storageMb": 5120
  },
  "tags": null,
  "type": "Microsoft.DBforMySQL/servers",
  "userVisibleState": "Ready",
  "version": "5.7"
}
```

## <a name="connect-to-the-server-using-the-mysqlexe-command-line-tool"></a>Conexión al servidor mediante la herramienta mysql.exe de la línea de comandos
Conéctese al servidor mediante la herramienta **mysql.exe** de la línea de comandos. Puede descargarlo desde [aquí](https://dev.mysql.com/downloads/) e instalarlo en su equipo. 

Escriba los comandos siguientes: 

1. Conéctese al servidor con la herramienta de la línea de comandos **mysql**:
   ```bash
   mysql -h mydemoserver.mysql.database.azure.com -u myadmin@mydemoserver -p
   ```

2. Ver el estado del servidor:
   ```sql
   mysql> status
   ```
   Si todo se ha realizado correctamente, la herramienta de la línea de comandos debe mostrar el siguiente texto:

```dos
C:\Users\>mysql -h mydemoserver.mysql.database.azure.com -u myadmin@mydemoserver -p
Enter password: ***********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65512
Server version: 5.6.26.0 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.6.35, for Win64 (x86_64)

Connection id:          65512
Current database:
Current user:           myadmin@116.230.243.143
SSL:                    Not in use
Using delimiter:        ;
Server version:         5.6.26.0 MySQL Community Server (GPL)
Protocol version:       10
Connection:             mydemoserver.mysql.database.azure.com via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    gbk
Conn.  characterset:    gbk
TCP port:               3306
Uptime:                 2 days 9 hours 47 min 20 sec

Threads: 4  Questions: 34833  Slow queries: 2  Opens: 84  Flush tables: 4  Open tables: 1  Queries per second avg: 0.167
--------------

mysql>
```

> [!TIP]
> Para otros comandos, consulte el [capítulo 4.5.1 del Manual de referencia de MySQL 5.7](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>Conexión al servidor con la herramienta MySQL Workbench de la GUI
1. Inicie la aplicación MySQL Workbench en el equipo cliente. Puede descargar e instalar MySQL Workbench desde [aquí](https://dev.mysql.com/downloads/workbench/).

2. En el cuadro de diálogo **Setup New Connection** (Establecer nueva conexión), escriba la siguiente información en la pestaña **Parámetros**:

   ![Configuración de una conexión nueva](./media/quickstart-create-mysql-server-database-using-azure-cli/setup-new-connection.png)

| **Configuración** | **Valor sugerido** | **Descripción** |
|---|---|---|
|   Nombre de la conexión | Mi conexión | Especifique una etiqueta para esta conexión (puede ser cualquier cosa) |
| Método de conexión | elija Estándar (TCP/IP) | Use el protocolo TCP/IP para conectarse a Azure Database for MySQL> |
| Hostname | mydemoserver.mysql.database.azure.com | Nombre del servidor que anotó anteriormente. |
| Port | 3306 | Se usa el puerto predeterminado para MySQL. |
| Nombre de usuario | myadmin@mydemoserver | El inicio de sesión de administrador del servidor que anotó anteriormente. |
| Contraseña | **** | Use la contraseña de la cuenta de administrador que configuró anteriormente. |

Haga clic en **Probar conexión** para probar si todos los parámetros están configurados correctamente.
Ahora puede hacer clic en la conexión para conectarse correctamente al servidor.

## <a name="clean-up-resources"></a>Limpieza de recursos
Si no necesita estos recursos para otra guía de inicio rápido o tutorial, puede eliminarlos con el siguiente comando: 

```azurecli-interactive
az group delete --name myresourcegroup
```

Si solo desea eliminar el servidor recién creado, puede ejecutar el comando **[az mysql server delete](/cli/azure/mysql/server#az-mysql-server-delete)** .
```azurecli-interactive
az mysql server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Diseño de una base de datos MySQL con la CLI de Azure](./tutorial-design-database-using-cli.md)
