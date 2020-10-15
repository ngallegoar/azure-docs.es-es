---
title: Configuración de parámetros de servidor de Azure Database for MariaDB mediante Azure Portal
description: En este artículo se explica cómo configurar los parámetros del servidor MariaDB en Azure Database for MariaDB mediante Azure Portal.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: how-to
ms.date: 10/1/2020
ms.openlocfilehash: 9d0b6865c7fb5b59f379568d15bd9b96883202e9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91626435"
---
# <a name="configure-server-parameters-in-azure-database-for-mariadb-using-the-azure-portal"></a>Configuración de parámetros del servidor en Azure Database for MariaDB mediante Azure Portal.

Azure Database for MariaDB admite la configuración de algunos parámetros del servidor. En este artículo se explica cómo configurar estos parámetros mediante Azure Portal. No es posible ajustar todos los parámetros del servidor.

>[!Note]
> Los parámetros del servidor se pueden actualizar globalmente a nivel de servidor; utilice la [CLI de Azure](./howto-configure-server-parameters-cli.md), [PowerShell](./howto-configure-server-parameters-using-powershell.md) o [Azure Portal](./howto-server-parameters.md).

## <a name="configure-server-parameters"></a>Configuración de parámetros del servidor

1. Inicie sesión en Azure Portal y luego localice su servidor de Azure Database for MariaDB.
2. En la sección **CONFIGURACIÓN**, haga clic en **Parámetros del servidor** para abrir la página Parámetros del servidor de Azure Database for MariaDB.
![Página Parámetros del servidor de Azure Portal](./media/howto-server-parameters/azure-portal-server-parameters.png)
3. Localice cualquier configuración que deba ajustar. Revise la columna **Descripción** para entender el propósito y los valores permitidos.
![Menú desplegable de enumeración](./media/howto-server-parameters/3-toggle_parameter.png)
4. Haga clic en **Guardar** para guardar los cambios.
![Guardar o descartar cambios](./media/howto-server-parameters/4-save_parameters.png)
5. Si ha guardado los nuevos valores para los parámetros, siempre puede revertir todos los elementos a los valores predeterminados; para ello, seleccione **Restablecer todos los valores predeterminados**.
![Restablecer todos los valores predeterminados](./media/howto-server-parameters/5-reset_parameters.png)

## <a name="setting-parameters-not-listed"></a>Ajustar parámetros que no aparecen en la lista

Si el parámetro de servidor que desea actualizar no aparece en Azure Portal, también puede establecer el parámetro en el nivel de conexión mediante `init_connect`. De este modo se establecen los parámetros del servidor para cada cliente con conexión al servidor. 

1. En la sección **CONFIGURACIÓN**, haga clic en **Parámetros del servidor** para abrir la página Parámetros del servidor de Azure Database for MariaDB.
2. Busque `init_connect`.
3. Agregue los parámetros del servidor con el formato: `SET parameter_name=YOUR_DESIRED_VALUE` en valor la columna de valor.

    Por ejemplo, puede cambiar el conjunto de caracteres del servidor estableciendo `init_connect` en `SET character_set_client=utf8;SET character_set_database=utf8mb4;SET character_set_connection=latin1;SET character_set_results=latin1;`.
4. Haga clic en **Guardar** para guardar los cambios.

## <a name="working-with-the-time-zone-parameter"></a>Trabajo con el parámetro de zona horaria

### <a name="populating-the-time-zone-tables"></a>Relleno de las tablas de la zona horaria

Las tablas de la zona horaria del servidor se pueden rellenar mediante una llamada al procedimiento almacenado `mysql.az_load_timezone` desde una herramienta como la línea de comandos de MySQL o MySQL Workbench.

> [!NOTE]
> Si ejecuta el comando `mysql.az_load_timezone` de MySQL Workbench, es posible que primero tenga que desactivar el modo de actualización segura mediante `SET SQL_SAFE_UPDATES=0;`.

```sql
CALL mysql.az_load_timezone();
```

> [!IMPORTANT]
> Debe reiniciar el servidor para asegurarse de que las tablas de zona horaria se rellenen correctamente. Para reiniciar el servidor, use [Azure Portal](howto-restart-server-portal.md) o la [CLI](howto-restart-server-cli.md).
Para ver los valores de zonas horarias disponibles, ejecute el comando siguiente:

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>Establecimiento de la zona horaria de nivel global

La zona horaria de nivel global se puede establecer desde la página **Parámetros del servidor** en Azure Portal. En el ejemplo siguiente se establece la zona horaria global en el valor "US/Pacific".

![Establecimiento del parámetro de zona horaria](./media/howto-server-parameters/timezone.png)

### <a name="setting-the-session-level-time-zone"></a>Establecimiento de la zona horaria de nivel de sesión

La zona horaria de nivel de sesión se puede establecer mediante la ejecución del comando `SET time_zone` desde una herramienta como la línea de comandos de MySQL o MySQL Workbench. En el ejemplo siguiente se establece la zona horaria en **US/Pacific**.

```sql
SET time_zone = 'US/Pacific';
```

Consulte en la documentación de MariaDB las [Funciones de fecha y hora](https://mariadb.com/kb/en/library/convert_tz/).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre los [parámetros del servidor](concepts-server-parameters.md).
