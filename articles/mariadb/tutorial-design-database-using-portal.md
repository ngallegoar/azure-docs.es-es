---
title: 'Tutorial: Diseño de un servidor de Azure Database for MariaDB con Azure Portal'
description: En este tutorial se explica cómo crear y administrar un servidor y una base de datos de Azure Database for MariaDB con Azure Portal.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: tutorial
ms.date: 3/18/2020
ms.custom: mvc
ms.openlocfilehash: 3e6cac09ff304e02e24b482dc834d8199329c1c0
ms.sourcegitcommit: 6906980890a8321dec78dd174e6a7eb5f5fcc029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2020
ms.locfileid: "92424872"
---
# <a name="tutorial-design-an-azure-database-for-mariadb-database-by-using-the-azure-portal"></a>Tutorial: Diseño de una base de datos de Azure Database for MariaDB con Azure Portal

Azure Database for MariaDB es un servicio administrado que puede usar para ejecutar, administrar y escalar bases de datos MySQL de alta disponibilidad en la nube. Con Azure Portal, puede administrar fácilmente el servidor y diseñar una base de datos.

En este tutorial usará Azure Portal para aprender a hacer lo siguiente:

> [!div class="checklist"]
> * Creación de una instancia de Azure Database for MariaDB
> * Configuración del firewall del servidor
> * Uso de la herramienta de línea de comandos mysql para crear una base de datos
> * Carga de datos de muestra
> * Consultar datos
> * Actualización de datos
> * Restauración de datos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

En el explorador, vaya a [Azure Portal](https://portal.azure.com/). Introduzca sus credenciales para iniciar sesión en el portal. La vista predeterminada es el panel del servicio.

## <a name="create-an-azure-database-for-mariadb-server"></a>Creación de un servidor de Azure Database for MariaDB

Se crea un servidor de Azure Database for MariaDB con un conjunto definido de [recursos de proceso y almacenamiento](concepts-pricing-tiers.md). El servidor se crea en un [grupo de recursos de Azure](../azure-resource-manager/management/overview.md).

1. Seleccione el botón **Crear un recurso** (+) de la esquina superior izquierda del portal.

2. Seleccione **Bases de datos** > **Azure Database for MariaDB** . También puede escribir **MariaDB** en el cuadro de búsqueda para encontrar el servicio.

   ![Vaya a MySQL.](./media/tutorial-design-database-using-portal/1-Navigate-to-mariadb.png)

3. Seleccione el icono **Azure Database for MariaDB** . Escriba o seleccione la información necesaria.

   ![Formulario de creación](./media/tutorial-design-database-using-portal/2-create-form.png)

    Configuración | Valor sugerido | Descripción del campo
    ---|---|---
    Nombre de servidor | *nombre del servidor único* | Elija un nombre único que identifique el servidor de Azure Database for MariaDB. Por ejemplo, **mydemoserver** . El nombre de dominio *.mariadb.database.azure.com* se anexa al nombre de servidor especificado. El nombre del servidor solo puede contener letras minúsculas, números y el carácter de guion (-). Debe contener entre 3 y 63 caracteres.
    Subscription | *su suscripción* | Seleccione la suscripción de Azure que quiere usar para el servidor. Si tiene varias suscripciones, elija aquella en la que se factura el recurso.
    Resource group | **myresourcegroup** | Escriba un nuevo nombre de grupo de recursos o seleccione uno existente.
    Seleccionar origen | **En blanco** | Seleccione **En blanco** para crear un servidor. (Si va a crear un servidor a partir de una copia de seguridad geográfica de un servidor de Azure Database for MariaDB existente, seleccione **Backup** ).
    Inicio de sesión de administrador de servidor | **myadmin** | Una cuenta de inicio de sesión para usar al conectarse al servidor. El nombre de inicio de sesión de administrador no puede ser **azure_superuser** , **admin** , **administrator** , **root** , **guest** ni **public** .
    Contraseña | *la que elija* | Escriba una nueva contraseña para la cuenta de administrador del servidor. Debe tener entre 8 y 128 caracteres. La contraseña debe contener caracteres de tres de las siguientes categorías: Letras del alfabeto inglés mayúsculas y minúsculas, números (0-9) y caracteres no alfanuméricos (!, $, #, %, etc.).
    Confirmar contraseña | *la que elija*| Confirme la contraseña de la cuenta de administrador.
    Location | *la región más cercana a los usuarios*| Elija la ubicación más cercana a los usuarios o a sus otras aplicaciones de Azure.
    Versión | *la versión más reciente*| La versión más reciente (a menos que tenga requisitos específicos para el uso de una versión diferente).
    Plan de tarifa | Consulte la descripción. | Configuración de los recursos de proceso, almacenamiento y copia de seguridad para el nuevo servidor. Seleccione **Plan de tarifa** > **Uso general** . Mantenga los valores predeterminados de los siguientes valores de configuración:<br><ul><li>**Generación de procesos** (Gen 5)</li><li>**Núcleo virtual** (4 núcleos virtuales)</li><li>**Almacenamiento** (100 GB)</li><li>**Período de retención de copia de seguridad** (7 días)</li></ul><br>Para habilitar las copias de seguridad del servidor en el almacenamiento con redundancia geográfica, seleccione **Redundancia geográfica** en **Opciones de redundancia de copia de seguridad** . <br><br>Para guardar el plan de tarifa elegido, seleccione **Aceptar** . La captura de pantalla siguiente muestra estas opciones seleccionadas.
    
   ![Plan de tarifa](./media/tutorial-design-database-using-portal/3-pricing-tier.png)

   > [!TIP]
   > Con el **crecimiento automático** habilitado, el servidor aumenta el almacenamiento cuando se está acercando el límite asignado, sin afectar a la carga de trabajo.

4. Haga clic en **Revisar + crear** . Puede hacer clic en el botón **Notificaciones** de la barra de herramientas para supervisar el proceso de implementación. La implementación puede tardar hasta 20 minutos.

## <a name="configure-the-firewall"></a>Configuración del firewall

Una base de datos de Azure Database for MariaDB está protegida por un firewall. De forma predeterminada, se rechazan todas las conexiones al servidor y a las bases de datos que contiene. Antes de conectarse a Azure Database for MariaDB por primera vez, configure el firewall para agregar la dirección IP (o el intervalo de direcciones IP) de red pública del equipo cliente.

1. Seleccione el servidor recién creado y, luego, **Seguridad de la conexión** .

   ![Seguridad de conexión](./media/tutorial-design-database-using-portal/1-Connection-security.png)
2. Puede seleccionar **Agregar mi IP** o configurar aquí las reglas de firewall. No olvide seleccionar **Guardar** después de crear las reglas.

Ahora puede conectarse al servidor con la herramienta de línea de comandos mysql o MySQL Workbench.

> [!TIP]
> El servidor de Azure Database for MariaDB se comunica a través del puerto 3306. Si intenta conectarse desde una red corporativa, es posible que el firewall de la red no permita el tráfico saliente a través del puerto 3306. En este caso, para conectarse al servidor de Azure Database for MariaDB, el departamento de TI debe abrir el puerto 3306.

## <a name="get-connection-information"></a>Obtención de información sobre la conexión

En Azure Portal, obtenga los valores de **Nombre de servidor** (completo) y **Nombre de inicio de sesión del administrador del servidor** del servidor de Azure Database for MariaDB. Usará el nombre de servidor completo para conectarse a su servidor mediante la herramienta de línea de comandos mysql.

1. En el menú izquierdo de [Azure Portal](https://portal.azure.com/), seleccione **Todos los recursos** . Escriba el nombre del servidor y busque el servidor de Azure Database for MariaDB. Seleccione el nombre del servidor para ver los detalles del servidor.

2. En la página **Información general** , anote los valores de **Nombre del servidor** y **Nombre de inicio de sesión del administrador del servidor** . También puede seleccionar el botón **Copiar** junto a cada campo para copiar el valor en el Portapapeles.

   ![Propiedades del servidor](./media/tutorial-design-database-using-portal/2-server-properties.png)

En este ejemplo, el nombre del servidor es **mydemoserver.mariadb.database.azure.com** y el nombre de inicio de sesión del administrador del servidor es **myadmin\@mydemoserver** .

## <a name="connect-to-the-server-by-using-mysql"></a>Conexión al servidor con mysql

Use la [herramienta de línea de comandos mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) para establecer una conexión con el servidor de Azure Database for MariaDB. Puede ejecutar esta herramienta desde Azure Cloud Shell en el explorador o desde su propio equipo mediante las herramientas mysql instaladas de forma local. Para abrir Azure Cloud Shell, haga clic en el botón **Probar** en un bloque de código de este artículo, o visite Azure Portal y haga clic en el icono **>_** de la barra de herramientas superior derecha.

Escriba el comando para conectarse:

```bash
mysql -h mydemoserver.mariadb.database.azure.com -u myadmin@mydemoserver -p
```

## <a name="create-a-blank-database"></a>Crear una base de datos en blanco

Cuando esté conectado al servidor, cree una base de datos vacía con la que trabajar:

```sql
CREATE DATABASE mysampledb;
```

En el símbolo del sistema, ejecute el comando siguiente para cambiar la conexión a la base de datos recién creada:

```sql
USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Creación de tablas en la base de datos

Ahora que sabe cómo conectarse a la base de datos de Azure Database for MariaDB, puede realizar algunas tareas básicas.

En primer lugar, cree una tabla y cárguela con algunos datos. Vamos a crear una tabla que almacena la información del inventario:

```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-in-the-tables"></a>Carga de datos en las tablas

Ahora que tiene una tabla, inserte algunos datos en ella. En la ventana de símbolo del sistema abierta, ejecute la consulta siguiente para insertar algunas filas de datos:

```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

## <a name="query-and-update-the-data-in-the-tables"></a>Consulta y actualización de los datos en las tablas

Ejecute la consulta siguiente para recuperar información de la tabla de base de datos:

```sql
SELECT * FROM inventory;
```

También puede actualizar los datos de las tablas:

```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

La fila se actualiza cuando se recuperan los datos:

```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Restauración de una base de datos a un momento anterior en el tiempo

Imagine que ha eliminado por accidente una tabla de base de datos importante y no puede recuperar los datos fácilmente. Azure Database for MariaDB permite restaurar el servidor a un momento dado, ya que crea una copia de las bases de datos en un nuevo servidor. Puede usar este servidor nuevo para recuperar los datos eliminados. Los pasos siguientes restauran el servidor de ejemplo a un punto antes de que se agregara la tabla:

1. En Azure Portal, localice Azure Database for MariaDB. En la página **Información general** , seleccione **Restaurar** .

   ![Restaurar una base de datos](./media/tutorial-design-database-using-portal/1-restore-a-db.png)

2. En la página **Restaurar** , escriba o seleccione la siguiente información:

   ![Formulario de restauración](./media/tutorial-design-database-using-portal/2-restore-form.png)
   
   - **Punto de restauración** : seleccione un momento dado al que quiera restaurar, en el período de tiempo que aparece. Asegúrese de que convertir la zona horaria local a UTC.
   - **Restaurar en el servidor nuevo** : escriba un nuevo nombre de servidor donde se realizará la restauración.
   - **Ubicación** : la región es la misma que la del servidor de origen y no se puede cambiar.
   - **Plan de tarifa** : es el mismo que el del servidor de origen y no se puede cambiar.
   
3. Haga clic en **Aceptar** para [restaurar el servidor a un momento dado](./howto-restore-server-portal.md) antes de que se eliminara la tabla. Al restaurar un servidor, se crea una nueva copia de él en el momento dado que especifique. 

## <a name="next-steps"></a>Pasos siguientes
En este tutorial uso Azure Portal para aprender a hacer lo siguiente:

> [!div class="checklist"]
> * Creación de una instancia de Azure Database for MariaDB
> * Configuración del firewall del servidor
> * Usar la herramienta de línea de comandos de mysql para crear una base de datos
> * Carga de datos de muestra
> * Consultar datos
> * Actualización de datos
> * Restauración de datos

> [!div class="nextstepaction"]
> [Conexión de aplicaciones a Azure Database for MariaDB](howto-connection-string.md)
