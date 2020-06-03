---
title: Conexión a Azure SQL Edge y consulta (versión preliminar)
description: Información sobre la conexión a una instancia de Azure SQL Edge y su consulta (versión preliminar)
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: e7034c3f664eeba802341510b109ba9cc57845a8
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2020
ms.locfileid: "84235128"
---
# <a name="connect-and-query-azure-sql-edge-preview"></a>Conexión a Azure SQL Edge y consulta (versión preliminar)

Después de implementar el contenedor de Azure SQL Edge, puede conectarse al motor de base de datos de SQL desde cualquiera de las siguientes ubicaciones.

- En el contenedor.
- Desde otro contenedor de Docker que se ejecute en el mismo host.
- Desde el equipo host.
- Desde cualquier otro equipo cliente de la red.

## <a name="tools-to-connect-to-azure-sql-edge"></a>Herramientas para conectarse a Azure SQL Edge

Las conexiones a una instancia de Azure SQL Edge se pueden realizar desde cualquiera de las herramientas comunes que se mencionan a continuación.

* [sqlcmd](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-tools): las herramientas de cliente de sqlcmd ya están incluidas en la imagen de contenedor de Azure SQL Edge. Si adjunta un contenedor en ejecución con un shell de Bash interactivo, puede ejecutar las herramientas de forma local.
* [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms)
* [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio)
* [Visual Studio Code](https://docs.microsoft.com/sql/visual-studio-code/sql-server-develop-use-vscode)

Para conectarse a un motor de base de datos de Azure SQL Edge desde un equipo de red, necesitará lo siguiente:

- *Dirección IP o nombre de red del equipo host*: es el equipo host donde se ejecuta el contenedor de Azure SQL Edge.
- *Asignación del puerto de host del contenedor de Azure SQL Edge*: se trata de la asignación del puerto para el puerto del contenedor de Docker a un puerto en el host. Dentro del contenedor, SQL Edge siempre se asigna al puerto 1433. Esto se puede cambiar como parte de la implementación de Azure SQL Edge. Para cambiar el número de puerto, actualice las "opciones de creación de contenedores" para el módulo de SQL Edge en Azure IoT Edge. En el ejemplo que se muestra a continuación, el puerto 1433 del contenedor está asignado al puerto 1600 en el host.

    ```JSON
    {
        "PortBindings": {
          "1433/tcp": [
            {
              "HostPort": "1600"
            }
          ]
        }
    }
    ```

- *Contraseña de administrador del sistema para la instancia de SQL Edge*: este es el valor especificado para la variable de entorno **SA_PASSWORD** durante la implementación de SQL Edge.

## <a name="connecting-to-the-database-engine-from-within-the-container"></a>Conexión al motor de base de datos desde el contenedor

Las [herramientas de línea de comandos de SQL Server](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-tools) se incluyen en la imagen de contenedor de Azure SQL Edge. Si se asocia el contenedor con un símbolo del sistema interactivo, puede ejecutar las herramientas de forma local.

1. Use el comando `docker exec -it` para iniciar un shell de Bash interactivo dentro de su contenedor en ejecución. En el ejemplo siguiente, `e69e056c702d` es el identificador del contenedor.

    ```bash
    docker exec -it <Azure SQL Edge container id or name> /bin/bash
    ```

    > [!TIP]
    > No siempre tiene que especificar el identificador completo del contenedor. Solo tiene que especificar suficientes caracteres para identificarlo de forma única. Por lo tanto, en este ejemplo, podría ser suficiente usar `e6` o `e69` en lugar del identificador completo.

2. Una vez dentro del contenedor, conecte localmente con sqlcmd. Sqlcmd no está en la ruta de acceso de forma predeterminada, por lo que deberá especificar la ruta de acceso completa.

    ```bash
    /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P '<YourPassword>'
    ```

3. Cuando termine con sqlcmd, escriba `exit`.

4. Cuando termine con el símbolo del sistema interactivo, escriba `exit`. El contenedor continuará ejecutándose después de salir del shell de Bash interactivo.

## <a name="connect-to-sql-edge-from-another-container-on-the-same-host"></a>Conexión a SQL Edge desde otro contenedor en el mismo host

Dado que dos contenedores que se ejecutan en el mismo host se encuentran en la misma red de Docker, se puede acceder fácilmente a ellos mediante el nombre del contenedor y la dirección del puerto para el servicio. Por ejemplo, si se va a conectar a la instancia de SQL Edge desde otro módulo de Python (contenedor) en el mismo host, puede utilizar una cadena de conexión similar a la siguiente. En el ejemplo siguiente se supone que SQL Edge está configurado para escuchar en el puerto predeterminado.

```python

import pyodbc
server = 'MySQLEdgeContainer' # Replace this with the actual name of your SQL Edge Docker container
username = 'sa' # SQL Server username
password = 'MyStrongestP@ssword' # Replace this with the actual SA password from your deployment
database = 'MyEdgeDatabase' # Replace this with the actual database name from your deployment. If you do not have a database created, you can use Master database.
db_connection_string = "Driver={ODBC Driver 17 for SQL Server};Server=" + server + ";Database=" + database + ";UID=" + username + ";PWD=" + password + ";"
conn = pyodbc.connect(db_connection_string, autocommit=True)

```

## <a name="connect-to-sql-edge-from-another-network-machine"></a>Conexión a SQL Edge desde otro equipo de red

Para conectarse a la instancia de SQL Edge desde otro equipo de la red, debe usar la dirección IP del host de Docker y el puerto de host al que está asignado el contenedor de SQL Edge. Por ejemplo, si la dirección IP del host de Docker es *xxx.xxx.xxx.xxx" y el contenedor de SQL Edge está asignado al puerto de host *1600*, la dirección del servidor para la instancia de SQL Edge sería **xxx.xxx.xxx.xxx,1600**. El script de Python actualizado tendría el siguiente aspecto:

```python

import pyodbc
server = 'xxx.xxx.xxx.xxx,1600' # Replace this with the actual name of your SQL Edge Docker container
username = 'sa' # SQL Server username
password = 'MyStrongestP@ssword' # Replace this with the actual SA password from your deployment
database = 'MyEdgeDatabase' # Replace this with the actual database name from your deployment. If you do not have a database created, you can use Master database.
db_connection_string = "Driver={ODBC Driver 17 for SQL Server};Server=" + server + ";Database=" + database + ";UID=" + username + ";PWD=" + password + ";"
conn = pyodbc.connect(db_connection_string, autocommit=True)

```

Para conectarse a una instancia de SQL Edge con SQL Server Management Studio en ejecución en un equipo Windows, consulte [SQL Server Management Studio](https://docs.microsoft.com/sql/linux/sql-server-linux-manage-ssms).

Para conectarse a una instancia de SQL Edge con Visual Studio Code en un equipo Windows, Mac o Linux, consulte [Visual Studio Code](https://docs.microsoft.com/sql/visual-studio-code/sql-server-develop-use-vscode).

Para conectarse a una instancia de SQL Edge con Azure Data Studio en un equipo Windows, Mac o Linux, consulte [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-server).

## <a name="see-also"></a>Consulte también

[Conexión y consultas](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-docker#connect-and-query)

[Instalación de las herramientas de SQL Server en Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-tools)
