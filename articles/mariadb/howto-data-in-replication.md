---
title: Configuración de la replicación de datos de entrada en Azure Database for MariaDB
description: En este artículo se describe la configuración de la replicación de datos de entrada en Azure Database for MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: how-to
ms.date: 6/11/2020
ms.openlocfilehash: 623c072cb8cb2c7fb1b9b6ec7d3ea661302d5e6a
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86104680"
---
# <a name="configure-data-in-replication-in-azure-database-for-mariadb"></a>Configuración de la replicación de datos de entrada en Azure Database for MariaDB

En este artículo se describe cómo configurar la [Replicación de datos de entrada](concepts-data-in-replication.md) en Azure Database for MariaDB mediante la configuración de los servidores maestros y de réplica. En este artículo se asume que tiene alguna experiencia previa con servidores y bases de datos MariaDB.

Para crear una réplica en el servicio Azure Database for MariaDB, la [Replicación de datos de entrada](concepts-data-in-replication.md) sincroniza los datos que proceden de un servidor MariaDB maestro local, de máquinas virtuales (VM) o de servicios de base de datos en la nube. La Replicación de datos de entrada se basa en la replicación según la posición del archivo de registro binario (binlog), que es nativa de MariaDB. Para obtener más información sobre la replicación de binlog, consulte la [introducción a la replicación de binlog](https://mariadb.com/kb/en/library/replication-overview/).

Revise las [limitaciones y los requisitos](concepts-data-in-replication.md#limitations-and-considerations) de la replicación de datos de entrada antes de seguir los pasos de este artículo.

> [!NOTE]
> Si el servidor maestro es la versión 10.2 o posterior, se recomienda configurar la replicación de datos de entrada mediante el uso de [Identificador de transacción global](https://mariadb.com/kb/en/library/gtid/).


## <a name="create-a-mariadb-server-to-use-as-a-replica"></a>Creación de un servidor de MariaDB para utilizarlo como réplica

1. Cree un nuevo servidor Azure Database for MariaDB (por ejemplo, replica.mariadb.database.azure.com). Este servidor es el servidor de réplica en la replicación de datos de entrada.

    Para obtener información sobre la creación del servidor, consulte [Creación de un servidor de Azure Database for MariaDB mediante Azure Portal](quickstart-create-mariadb-server-database-using-azure-portal.md).

   > [!IMPORTANT]
   > Debe crear el servidor de Azure Database for MariaDB en los planes de tarifa De uso general u Optimizada para memoria.

2. Cree cuentas de usuario idénticas y los privilegios correspondientes.
    
    Las cuentas de usuario no se replican desde el servidor maestro al servidor de réplica. Para proporcionar a los usuarios acceso al servidor de réplica, debe crear manualmente todas las cuentas y los privilegios correspondientes en el servidor recién creado de Azure Database for MariaDB.

3. Agregue la dirección IP del servidor maestro a las reglas de firewall de la réplica. 

   Actualice las reglas de firewall mediante [Azure Portal](howto-manage-firewall-portal.md) o la [CLI de Azure](howto-manage-firewall-cli.md).

> [!NOTE]
> Comunicación sin prejuicios
>
> Microsoft admite un entorno diverso e inclusivo. En este artículo se incluyen referencias a la palabra _esclavo_. En la [guía de estilo para la comunicación sin prejuicios](https://github.com/MicrosoftDocs/microsoft-style-guide/blob/master/styleguide/bias-free-communication.md) de Microsoft se reconoce que se trata de una palabra excluyente. Se usa en este artículo por coherencia, ya que actualmente es la palabra que aparece en el software. Cuando se actualice el software para quitarla, este artículo se actualizará para que esté alineado.
>

## <a name="configure-the-master-server"></a>Configurar el servidor maestro

En los siguientes pasos se prepara y configura el servidor MariaDB en el entorno local, en una VM o en un servicio de base de datos en la nube para la replicación de datos de entrada. El servidor MariaDB es el maestro en la replicación de datos de entrada.

1. Revise los [requisitos del servidor maestro](concepts-data-in-replication.md#requirements) antes de continuar. 

   Por ejemplo, asegúrese de que el servidor maestro permite el tráfico entrante y saliente en el puerto 3306 y de que el servidor maestro tiene una **dirección IP pública**, el DNS es accesible públicamente o tiene un nombre de dominio completo (FQDN). 
   
   Pruebe la conectividad con el servidor maestro; para ello, pruebe a conectarse desde una herramienta como la línea de comandos de MySQL hospedada en otra máquina o desde la instancia de [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) disponible en Azure Portal.

2. Active el registro binario.
    
    Para ver si se ha habilitado el registro binario en el servidor maestro, introduzca el comando siguiente:

   ```sql
   SHOW VARIABLES LIKE 'log_bin';
   ```

   Si la variable [`log_bin`](https://mariadb.com/kb/en/library/replication-and-binary-log-server-system-variables/#log_bin) devuelve el valor `ON`, el registro binario está habilitado en el servidor.

   Si `log_bin` devuelve el valor `OFF`, edite el archivo **my.cnf** para que `log_bin=ON` active el registro binario. Reinicie el servidor para que el cambio surta efecto.

3. Configure el servidor maestro.

    La replicación de datos de entrada requiere el parámetro `lower_case_table_names` para ser coherente entre los servidores maestro y de réplica. El parámetro `lower_case_table_names` se establece de forma predeterminada en `1` en Azure Database for MariaDB.

   ```sql
   SET GLOBAL lower_case_table_names = 1;
   ```

4. Cree un nuevo rol de replicación y configure los permisos.

   Cree una cuenta de usuario en el servidor maestro que está configurado con privilegios de replicación. Puede crear una cuenta mediante el uso de comandos SQL o MySQL Workbench. Si va a replicar con SSL, debe especificarlo al crear la cuenta de usuario.
   
   Para obtener información sobre cómo agregar cuentas de usuario al servidor maestro, consulte la [documentación de MariaDB](https://mariadb.com/kb/en/library/create-user/).

   Mediante el uso de los comandos siguientes, el nuevo rol de replicación creado puede acceder al servidor maestro desde cualquier máquina, no solo desde la máquina que hospeda al servidor maestro. Para este acceso, especifique **syncuser\@'%'** en el comando para crear un usuario.
   
   Para obtener más información sobre la documentación de MariaDB, consulte [Especificación de nombres de cuenta](https://mariadb.com/kb/en/library/create-user/#account-names).

   **Comando SQL**

   - Replicación con SSL

       Para requerir SSL para todas las conexiones de usuario, introduzca el comando siguiente para crear un usuario:

       ```sql
       CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
       GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%' REQUIRE SSL;
       ```

   - Replicación sin SSL

       Si SSL no es necesario para todas las conexiones, introduzca el comando siguiente para crear un usuario:
    
       ```sql
       CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
       GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%';
       ```

   **MySQL Workbench**

   Para crear el rol de replicación en MySQL Workbench, abra el panel **Administración** y seleccione **Usuarios y privilegios**. A continuación, seleccione **Agregar cuenta**.
 
   ![Usuarios y privilegios](./media/howto-data-in-replication/users_privileges.png)

   Introduzca un nombre de usuario en el campo **Nombre de inicio de sesión**.

   ![Usuario de sincronización](./media/howto-data-in-replication/syncuser.png)
 
   Seleccione el panel **Roles administrativos** y, a continuación, en la lista **Privilegios globales**, seleccione **Servidor subordinado de replicación**. Seleccione **Aplicar** para crear el rol de replicación.

   ![Servidor subordinado de replicación](./media/howto-data-in-replication/replicationslave.png)


5. Establezca el servidor maestro en el modo de solo lectura.

   Antes de volcar una base de datos, el servidor debe configurarse en modo de solo lectura. En modo de solo lectura, el servidor maestro no puede procesar cualquier transacción de escritura. Para ayudar a evitar que afecte a la actividad de la empresa, programe la ventana de solo lectura durante un período de menor actividad.

   ```sql
   FLUSH TABLES WITH READ LOCK;
   SET GLOBAL read_only = ON;
   ```

6. Obtenga el nombre de archivo de registro binario y el desplazamiento actuales.

   Ejecute el comando [`show master status`](https://mariadb.com/kb/en/library/show-master-status/) para determinar el nombre de archivo de registro binario actual y el desplazamiento actuales.
    
   ```sql
   show master status;
   ```
   Los resultados deberían asemejarse a esta tabla:
   
   ![Resultados de estado del maestro](./media/howto-data-in-replication/masterstatus.png)

   Asegúrese de anotar el nombre del archivo binario, ya que se utilizará en pasos posteriores.
   
7. Obtenga la posición de GTID (opcional, es necesario para la replicación con GTID).

   Ejecute la función [`BINLOG_GTID_POS`](https://mariadb.com/kb/en/library/binlog_gtid_pos/) para obtener la posición de GTID para el nombre de archivo binlog y el desplazamiento correspondientes.
  
    ```sql
    select BINLOG_GTID_POS('<binlog file name>', <binlog offset>);
    ```
 

## <a name="dump-and-restore-the-master-server"></a>Volcar y restaurar el servidor maestro

1. Vuelque todas las bases de datos desde el servidor maestro.

   Utilice mysqldump para volcar todas las bases de datos desde el servidor maestro. No es necesario volcar la biblioteca de MySQL y la biblioteca de prueba.

    Para obtener más información, consulte [Volcar y restaurar](howto-migrate-dump-restore.md).

2. Establezca el servidor maestro en modo de lectura/escritura.

   Cuando se haya volcado la base de datos, vuelva a cambiar el servidor de MariaDB maestro al modo de lectura/escritura.

   ```sql
   SET GLOBAL read_only = OFF;
   UNLOCK TABLES;
   ```

3. Restaure el archivo de volcado en el servidor nuevo.

   Restaure el archivo de volcado en el servidor creado en el servicio Azure Database for MariaDB. Consulte [Volcado y restauración](howto-migrate-dump-restore.md) para obtener información sobre cómo restaurar un archivo de volcado en un servidor MariaDB.

   Si el archivo de volcado es grande, cárguelo a una VM en Azure dentro de la misma región que el servidor de réplica. Restáurelo en el servidor de Azure Database for MariaDB desde la VM.

## <a name="link-the-master-and-replica-servers-to-start-data-in-replication"></a>Vincular los servidores maestro y de réplica para iniciar la replicación de datos de entrada

1. Establezca el servidor maestro.

   Todas las funciones de la replicación de datos internos se realizan mediante los procedimientos almacenados. Puede encontrar todos los procedimientos en [Procedimientos almacenados de replicación de datos internos](reference-data-in-stored-procedures.md). Los procedimientos almacenados se pueden ejecutar en el shell de MySQL o en MySQL Workbench.

   Para vincular dos servidores e iniciar la replicación, inicie sesión en el servidor de réplica de destino en el servicio Azure Database for MariaDB. A continuación, establezca la instancia externa como servidor maestro mediante el uso de los procedimientos almacenados `mysql.az_replication_change_master` o `mysql.az_replication_change_master_with_gtid` en la base de datos de Azure para el servidor de MariaDB.

   ```sql
   CALL mysql.az_replication_change_master('<master_host>', '<master_user>', '<master_password>', 3306, '<master_log_file>', <master_log_pos>, '<master_ssl_ca>');
   ```
   
   or
   
   ```sql
   CALL mysql.az_replication_change_master_with_gtid('<master_host>', '<master_user>', '<master_password>', 3306, '<master_gtid_pos>', '<master_ssl_ca>');
   ```

   - master_host: nombre de host del servidor maestro
   - master_user: nombre de usuario para el servidor maestro
   - master_password: contraseña para el servidor maestro
   - master_log_file: nombre del archivo de registro binario procedente de la ejecución de `show master status`
   - master_log_pos: posición del registro binario procedente de la ejecución de `show master status`
   - master_gtid_pos: Posición de GTID al ejecutar `select BINLOG_GTID_POS('<binlog file name>', <binlog offset>);`
   - master_ssl_ca: contexto del certificado de entidad de certificación. Si no usa SSL, pase una cadena vacía.*
    
    
    \* Se recomienda pasar el parámetro master_ssl_ca como variable. Para más información, vea los ejemplos siguientes.

   **Ejemplos**

   - Replicación con SSL

       Para crear la variable `@cert`, ejecute los comandos siguientes:

       ```sql
       SET @cert = '-----BEGIN CERTIFICATE-----
       PLACE YOUR PUBLIC KEY CERTIFICATE\'S CONTEXT HERE
       -----END CERTIFICATE-----'
       ```

       La replicación con SSL se configura entre un servidor maestro hospedado en el dominio companya.com y un servidor de réplica hospedado en Azure Database for MariaDB. Este procedimiento almacenado se ejecuta en la réplica.
    
       ```sql
       CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mariadb-bin.000016', 475, @cert);
       ```
   - Replicación sin SSL

       La replicación sin SSL se configura entre un servidor maestro hospedado en el dominio companya.com y un servidor de réplica hospedado en Azure Database for MariaDB. Este procedimiento almacenado se ejecuta en la réplica.

       ```sql
       CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mariadb-bin.000016', 475, '');
       ```

2. Inicie la replicación.

   Llame al procedimiento almacenado `mysql.az_replication_start` para iniciar la replicación.

   ```sql
   CALL mysql.az_replication_start;
   ```

3. Compruebe el estado de replicación.

   Llame al comando [`show slave status`](https://mariadb.com/kb/en/library/show-slave-status/) en el servidor de réplica para ver el estado de replicación.
    
   ```sql
   show slave status;
   ```

   Si `Slave_IO_Running` y `Slave_SQL_Running` tienen el estado `yes` y el valor de `Seconds_Behind_Master` es `0`, la replicación funciona correctamente. `Seconds_Behind_Master` indica el tiempo de retraso de la réplica. Si el valor no es `0`, la réplica procesa actualizaciones.

4. Actualice las variables del servidor correspondientes con el fin de que la replicación de datos de entrada sea más segura (solo es necesario para la replicación sin GTID).
    
    Debido a una limitación de la replicación nativa de MariaDB, debe establecer las variables [`sync_master_info`](https://mariadb.com/kb/en/library/replication-and-binary-log-system-variables/#sync_master_info) y [`sync_relay_log_info`](https://mariadb.com/kb/en/library/replication-and-binary-log-system-variables/#sync_relay_log_info) en la replicación sin el escenario GTID.

    Compruebe las variables `sync_master_info` y `sync_relay_log_info` del servidor subordinado para asegurarse de que la replicación de datos de entrada es estable y establezca las variables en `1`.
    
## <a name="other-stored-procedures"></a>Otros procedimientos almacenados

### <a name="stop-replication"></a>Detención replicación

Para detener la replicación entre el servidor maestro y de réplica, use el siguiente procedimiento almacenado:

```sql
CALL mysql.az_replication_stop;
```

### <a name="remove-the-replication-relationship"></a>Eliminación de la relación de replicación

Para eliminar la relación entre el servidor maestro y de réplica, use el siguiente procedimiento almacenado:

```sql
CALL mysql.az_replication_remove_master;
```

### <a name="skip-the-replication-error"></a>Omitir el error de replicación

Para omitir un error de replicación y permitir la replicación, use el siguiente procedimiento almacenado:
    
```sql
CALL mysql.az_replication_skip_counter;
```

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre [Replicación de datos de entrada](concepts-data-in-replication.md) para Azure Database for MariaDB.
