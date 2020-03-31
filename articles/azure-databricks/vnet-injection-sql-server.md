---
title: Consulta de un contenedor de Docker de Linux para SQL Server con Azure Databricks
description: En este artículo se describe cómo implementar Azure Databricks en la red virtual, también conocido como inserción de red virtual.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: conceptual
ms.date: 11/07/2019
ms.openlocfilehash: 460079248e6cbd939c36b84f94cac41dce4dda2b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73747672"
---
# <a name="tutorial-query-a-sql-server-linux-docker-container-in-a-virtual-network-from-an-azure-databricks-notebook"></a>Tutorial: Consulta de un contenedor de Docker de Linux para SQL Server en una red virtual mediante un cuaderno de Azure Databricks

Con este tutorial aprenderá a integrar Azure Databricks con un contenedor de Docker de Linux para SQL Server en una red virtual. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Implementar un área de trabajo de Azure Databricks en una red virtual
> * Instalar una máquina virtual Linux en una red pública
> * Instalación de Docker
> * Instalar el contenedor del docker de Microsoft SQL Server en Linux
> * Consultar el servidor de SQL Server con JDBC desde un cuaderno de Databricks

## <a name="prerequisites"></a>Prerrequisitos

* Cree un [área de trabajo de Databricks en una red virtual](quickstart-create-databricks-workspace-vnet-injection.md).

* Instale [Ubuntu para Windows](https://www.microsoft.com/p/ubuntu/9nblggh4msv6?activetab=pivot:overviewtab).

* Descargue [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017).

## <a name="create-a-linux-virtual-machine"></a>Creación de una máquina virtual con Linux

1. En Azure Portal, seleccione el icono de **Máquinas virtuales**. Seguidamente, seleccione **+ Agregar**.

    ![Adición de una máquina virtual de Azure](./media/vnet-injection-sql-server/add-virtual-machine.png)

2. En la pestaña **Básico**, seleccione Ubuntu Server 18.04 LTS y cambie el tamaño de la máquina virtual a B2s. Elija un nombre de usuario y una contraseña de administrador.

    ![Pestaña Datos básicos de la configuración de la nueva máquina virtual](./media/vnet-injection-sql-server/create-virtual-machine-basics.png)

3. Navegue hasta la pestaña **Redes**. Elija la red virtual y la subred pública que incluye el clúster de Azure Databricks. Seleccione **Revisar y crear** y, a continuación, **Crear** para implementar la máquina virtual.

    ![Pestaña Redes de la configuración de nueva máquina virtual](./media/vnet-injection-sql-server/create-virtual-machine-networking.png)

4. Una vez completada la implementación, vaya a la máquina virtual. Observe la dirección IP pública y la red virtual/subred en la **Información general**. Seleccione la **Dirección IP pública**.

    ![Información general de la máquina virtual](./media/vnet-injection-sql-server/virtual-machine-overview.png)

5. Cambie la **Asignación** a **Estática** y escriba una **Etiqueta de nombre DNS**. Seleccione **Guardar** y reinicie la máquina virtual.

    ![Configuración de dirección IP pública](./media/vnet-injection-sql-server/virtual-machine-staticip.png)

6. Seleccione la pestaña **Redes** en **Configuración**. Tenga en cuenta que el grupo de seguridad de red que se creó durante la implementación de Azure Databricks está asociado con la máquina virtual. Seleccione **Agregar regla de puerto de entrada**.

7. Agregue una regla para abrir el puerto 22 para SSH. Use la configuración siguiente:
    
    |Configuración|Valor sugerido|Descripción|
    |-------|---------------|-----------|
    |Source|Direcciones IP|Direcciones IP especifica que el tráfico entrante desde una dirección IP de origen específica se permitirá o denegará por esta regla.|
    |Direcciones IP de origen|<su dirección IP pública\>|Escriba la dirección IP pública. Para conocer la dirección IP pública, visite [bing.com](https://www.bing.com/) y busque **"Mi IP"** .|
    |Source port ranges|*|Permitir el tráfico de cualquier puerto.|
    |Destination|Direcciones IP|Direcciones IP especifica que el tráfico saliente para una dirección IP de origen específica se permitirá o denegará por esta regla.|
    |Direcciones IP de destino|<la dirección IP pública de la máquina virtual\>|Escriba la dirección IP pública de la máquina virtual. Puede encontrarla en la página **Información general** de la máquina virtual.|
    |Intervalos de puertos de destino|22|Abra el puerto 22 para SSH.|
    |Priority|290|Asigne a la regla una prioridad.|
    |Nombre|ssh-databricks-tutorial-vm|Asigne un nombre a la regla.|


    ![Adición de regla de seguridad de entrada para el puerto 22](./media/vnet-injection-sql-server/open-port.png)

8. Agregue una regla para abrir el puerto 1433 para SQL con la siguiente configuración:

    |Configuración|Valor sugerido|Descripción|
    |-------|---------------|-----------|
    |Source|Any|Origen especifica que el tráfico entrante desde una dirección IP de origen específica se permite o deniega según esta regla.|
    |Source port ranges|*|Permitir el tráfico de cualquier puerto.|
    |Destination|Direcciones IP|Direcciones IP especifica que el tráfico saliente para una dirección IP de origen específica se permitirá o denegará por esta regla.|
    |Direcciones IP de destino|<la dirección IP pública de la máquina virtual\>|Escriba la dirección IP pública de la máquina virtual. Puede encontrarla en la página **Información general** de la máquina virtual.|
    |Intervalos de puertos de destino|1433|Abra el puerto 22 para SQL Server.|
    |Priority|300|Asigne a la regla una prioridad.|
    |Nombre|sql-databricks-tutorial-vm|Asigne un nombre a la regla.|

    ![Adición de regla de seguridad de entrada para el puerto 1433](./media/vnet-injection-sql-server/open-port2.png)

## <a name="run-sql-server-in-a-docker-container"></a>Ejecución de SQL Server en un contenedor de Docker

1. Abra [Ubuntu para Windows](https://www.microsoft.com/p/ubuntu/9nblggh4msv6?activetab=pivot:overviewtab) o cualquier otra herramienta que le permita conectarse mediante SSH a la máquina virtual. Vaya a la máquina virtual en Azure Portal y seleccione **Conectar** para obtener el comando SSH que necesita para conectarse.

    ![Conexión a la máquina virtual](./media/vnet-injection-sql-server/vm-ssh-connect.png)

2. Escriba el comando en el terminal de Ubuntu y escriba la contraseña de administrador que creó al configurar la máquina virtual.

    ![Inicio de sesión SSH en el terminal de Ubuntu](./media/vnet-injection-sql-server/vm-login-terminal.png)

3. Utilice el siguiente comando para instalar Docker en la máquina virtual.

    ```bash
    sudo apt-get install docker.io
    ```

    Verifique la instalación de Docker con el comando siguiente:

    ```bash
    sudo docker --version
    ```

4. Instale la imagen.

    ```bash
    sudo docker pull mcr.microsoft.com/mssql/server:2017-latest
    ```

    Compruebe las imágenes.

    ```bash
    sudo docker images
    ```

5. Ejecute el contenedor desde la imagen.

    ```bash
    sudo docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=Password1234' -p 1433:1433 --name sql1  -d mcr.microsoft.com/mssql/server:2017-latest
    ```

    Compruebe que el contenedor se está ejecutando.

    ```bash
    sudo docker ps -a
    ```

## <a name="create-a-sql-database"></a>Creación de una base de datos SQL

1. Abra SQL Server Management Studio y conéctese al servidor mediante el nombre del servidor y la autenticación de SQL. El nombre de usuario de inicio de sesión es **SA** y la contraseña es la contraseña establecida en el comando de Docker. La contraseña en el comando de ejemplo es `Password1234`.

    ![Conexión con SQL Server mediante SQL Server Management Studio](./media/vnet-injection-sql-server/ssms-login.png)

2. Cuando se haya conectado correctamente, seleccione **Nueva consulta** y escriba el siguiente fragmento de código para crear una base de datos y una tabla e insertar algunos registros en la tabla.

    ```SQL
    CREATE DATABASE MYDB;
    GO
    USE MYDB;
    CREATE TABLE states(Name VARCHAR(20), Capitol VARCHAR(20));
    INSERT INTO states VALUES ('Delaware','Dover');
    INSERT INTO states VALUES ('South Carolina','Columbia');
    INSERT INTO states VALUES ('Texas','Austin');
    SELECT * FROM states
    GO
    ```

    ![Consulta para crear una base de datos de SQL Server](./media/vnet-injection-sql-server/create-database.png)

## <a name="query-sql-server-from-azure-databricks"></a>Consulta de SQL Server desde Azure Databricks

1. Desplácese hasta el área de trabajo de Azure Databricks y compruebe que ha creado un clúster como parte de los requisitos previos. A continuación, seleccione **Create a Notebook** (Crear un cuaderno). Asigne un nombre al cuaderno, seleccione *Python* como lenguaje y seleccione el clúster que ha creado.

    ![Configuración del nuevo bloc de notas de Databricks](./media/vnet-injection-sql-server/create-notebook.png)

2. Use el siguiente comando para hacer ping a la dirección IP interna de la máquina virtual de SQL Server. Este ping debería completarse correctamente. Si no es así, compruebe que el contenedor se está ejecutando y revise la configuración del grupo de seguridad de red.

    ```python
    %sh
    ping 10.179.64.4
    ```

    También puede usar el comando nslookup para revisar.

    ```python
    %sh
    nslookup databricks-tutorial-vm.westus2.cloudapp.azure.com
    ```

3. Una vez que haya hecho ping correctamente en el servidor SQL Server, puede consultar la base de datos y las tablas. Ejecute el siguiente código Python:

    ```python
    jdbcHostname = "10.179.64.4"
    jdbcDatabase = "MYDB"
    userName = 'SA'
    password = 'Password1234'
    jdbcPort = 1433
    jdbcUrl = "jdbc:sqlserver://{0}:{1};database={2};user={3};password={4}".format(jdbcHostname, jdbcPort, jdbcDatabase, userName, password)

    df = spark.read.jdbc(url=jdbcUrl, table='states')
    display(df)
    ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine el grupo de recursos, el área de trabajo de Azure Databricks y todos los recursos relacionados. Si elimina el trabajo evitará que se generen facturas innecesarias. Si planea usar el área de trabajo de Azure Databricks en el futuro, puede detener el clúster y reiniciarlo más tarde. Si no va a seguir usando esta área de trabajo de Azure Databricks, elimine todos los recursos creados en este tutorial mediante los siguientes pasos:

1. En el menú de la izquierda de Azure Portal, haga clic en **Grupos de recursos** y en el nombre del grupo de recursos que creó.

2. En la página del grupo de recursos, seleccione **Eliminar**, escriba el nombre del recurso que quiere eliminar en el cuadro de texto y seleccione **Eliminar** de nuevo.

## <a name="next-steps"></a>Pasos siguientes

Vaya al siguiente artículo para aprender cómo extraer, transformar y cargar datos mediante Azure Databricks.
> [!div class="nextstepaction"]
> [Tutorial: Extracción, transformación y carga de datos mediante Azure Databricks](databricks-extract-load-sql-data-warehouse.md)
