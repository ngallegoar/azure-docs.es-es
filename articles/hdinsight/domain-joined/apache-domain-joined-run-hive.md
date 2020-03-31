---
title: 'Directivas de Apache Hive en Apache Ranger: Azure HDInsight'
description: Obtenga información sobre cómo configurar directivas de Apache Ranger para Hive en Azure HDInsight con Enterprise Security Package.
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/27/2019
ms.openlocfilehash: 90d7da9c8ddd8c9c595f2209dcc34e2f595acfd2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78196933"
---
# <a name="configure-apache-hive-policies-in-hdinsight-with-enterprise-security-package"></a>Configuración de directivas de Apache Hive en HDInsight con Enterprise Security Package

Aprenda a configurar las directivas de Apache Ranger para Apache Hive. En este artículo, cree dos directivas Ranger para restringir el acceso a hivesampletable. hivesampletable viene con los clústeres de HDInsight. Una vez configuradas las directivas, utilice Excel y el controlador ODBC para conectarse a las tablas de Hive en HDInsight.

## <a name="prerequisites"></a>Prerrequisitos

* Un clúster de HDInsight con Enterprise Security Package. Consulte [Configuración de clústeres de HDInsight con Enterprise Security Package](apache-domain-joined-configure.md).
* Un equipo con Office 2016, Office 2013 Professional Plus, Office 2013 Pro Plus, Excel 365 Standalone u Office 2010 Professional Plus.

## <a name="connect-to-apache-ranger-admin-ui"></a>Conexión a la interfaz de usuario administrador de Apache Ranger
**Para conectarse a la interfaz de usuario administrador de Apache Ranger**

1. En un explorador, vaya a la interfaz de usuario administrador de Apache Ranger en `https://CLUSTERNAME.azurehdinsight.net/Ranger/`, donde CLUSTERNAME es el nombre del clúster.

   > [!NOTE]  
   > Ranger usa credenciales diferentes del clúster de Apache Hadoop. Para evitar que los exploradores usen credenciales almacenadas en caché de Hadoop, use nueva ventana del explorador InPrivate para conectarse a la interfaz de usuario administrador de Ranger.

2. Inicie sesión con el nombre de usuario y la contraseña del dominio del administrador de clúster:

    ![Página principal de HDInsight ESP Ranger](./media/apache-domain-joined-run-hive/hdinsight-domain-joined-ranger-home-page.png)

    Actualmente, Ranger solo funciona con Yarn y Hive.

## <a name="create-domain-users"></a>Creación de usuarios del dominio

Consulte [Crear un clúster de HDInsight con ESP](apache-domain-joined-configure-using-azure-adds.md#create-an-hdinsight-cluster-with-esp), para más información sobre cómo crear hiveruser1 y hiveuser2. Para este artículo usará las dos cuentas de usuario.

## <a name="create-ranger-policies"></a>Creación de directivas de Ranger

En esta sección, creará dos directivas Ranger para acceder a hivesampletable. Se concede el permiso select en un conjunto diferente de columnas. Ambos usuarios se crearon en [Crear un clúster de HDInsight con ESP](apache-domain-joined-configure-using-azure-adds.md#create-an-hdinsight-cluster-with-esp). En la siguiente sección probará las dos directivas en Excel.

**Para crear directivas de Ranger**

1. Abra la interfaz de usuario administrador de Ranger. Consulte el artículo sobre la conexión a la interfaz de usuario administrador de Apache Ranger.
2. Seleccione **CLUSTERNAME_Hive** en **Hive**. Deben aparecer dos directivas preconfiguradas.
3. Seleccione **Agregar nueva directiva** y escriba los siguientes valores:

    |Propiedad |Value |
    |---|---|
    |Nombre de la directiva|read-hivesampletable-all|
    |Base de datos de Hive|default|
    |table|hivesampletable|
    |Columna de Hive|*|
    |Seleccionar usuario|hiveuser1|
    |Permisos|select|

    ![Configuración de directivas de Hive para HDInsight ESP Ranger](./media/apache-domain-joined-run-hive/hdinsight-domain-joined-configure-ranger-policy.png).

    > [!NOTE]  
    > Si un usuario de dominio no se rellena en Seleccionar usuario, espere unos instantes para que Ranger se sincronice con AAD.

4. Seleccione **Agregar** para guardar la directiva.

5. Repita los dos últimos pasos para crear otra directiva con las siguientes propiedades:

    |Propiedad |Value |
    |---|---|
    |Nombre de la directiva|read-hivesampletable-devicemake|
    |Base de datos de Hive|default|
    |table|hivesampletable|
    |Columna de Hive|clientid, devicemake|
    |Seleccionar usuario|hiveuser2|
    |Permisos|select|

## <a name="create-hive-odbc-data-source"></a>Creación de un origen de datos de Hive ODBC

Las instrucciones se pueden encontrar en el artículo sobre la [creación de origen de datos ODBC en Hive](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).  

 | Propiedad  |Descripción |
 | --- | --- |
 | Data Source Name | Asigne un nombre al origen de datos |
 | Host | Escriba CLUSTERNAME.azurehdinsight.net. Por ejemplo, myHDICluster.azurehdinsight.net |
 | Port | Use **443**. (Este puerto se ha cambiado de 563 a 443). |
 | Base de datos | Use el **valor predeterminado** |
 | Hive Server Type | Seleccione **Hive Server 2** |
 | Mechanism | Seleccione **Azure HDInsight Service** |
 | HTTP Path | Deje este parámetro en blanco. |
 | Nombre de usuario | Escriba hiveuser1@contoso158.onmicrosoft.com. Actualice el nombre de dominio si es diferente. |
 | Contraseña | Escriba la contraseña de hiveuser1. |

Haga clic **Prueba** antes de guardar el origen de datos.

## <a name="import-data-into-excel-from-hdinsight"></a>Importación de datos en Excel desde HDInsight

En la última sección ha configurado dos directivas.  hiveuser1 tiene el permiso select en todas las columnas y hiveuser2, en dos columnas. En esta sección se suplanta a los dos usuarios para importar datos a Excel.

1. Abra un libro de Excel nuevo o existente.

1. Desde la pestaña **Datos**, vaya a **Obtener datos** > **Desde otros orígenes** > **Desde ODBC** para iniciar la ventana **Desde ODBC**.

    ![Abrir el Asistente para la conexión de datos](./media/apache-domain-joined-run-hive/simbahiveodbc-excel-dataconnection1.png)

1. De la lista desplegable, seleccione el nombre del origen de datos que creó en la sección anterior y luego seleccione **Aceptar**.

1. Para el primer uso, se abrirá el cuadro de diálogo **Controlador ODBC**. Seleccione **Windows** en el menú izquierdo. Seleccione **Conectar** para abrir la ventana del **Navegador**.

1. Espere a que se abra el cuadro de diálogo **Seleccionar base de datos y tabla** . Esta operación puede tardar unos segundos.

1. Seleccione **hivesampletable** y, después, **Siguiente**.

1. Seleccione **Finalizar**.

1. En el cuadro de diálogo **Importar datos** , puede cambiar o especificar la consulta. Para ello, seleccione **Propiedades**. Esta operación puede tardar unos segundos.

1. Seleccione la pestaña **Definición**. El texto del comando es:

       SELECT * FROM "HIVE"."default"."hivesampletable"

   Según las directivas de Ranger definidas, hiveuser1 tiene el permiso select en todas las columnas.  Por eso, esta consulta funciona con las credenciales del hiveuser1, pero no con las credenciales del hiveuser2.

1. Seleccione **Aceptar** para cerrar el cuadro de diálogo Propiedades de conexión.

1. Seleccione **Aceptar** para cerrar el cuadro de diálogo **Importar datos**.  

1. Vuelva a escribir la contraseña de hiveuser1 y haga clic en **Aceptar**. La importación de los datos a Excel tarda algunos segundos. Cuando termine, verá 11 columnas de datos.

Para probar la segunda directiva (read-hivesampletable-devicemake) que creó en la última sección

1. Agregue una nueva hoja de Excel.
2. Siga el último procedimiento para importar los datos.  El único cambio que deberá hacer es usar las credenciales de hiveuser2 en lugar de las de hiveuser1. Esto genera un error, ya que hiveuser2 solo tiene permiso para ver dos columnas. Se producirá el error siguiente:

        [Microsoft][HiveODBC] (35) Error from Hive: error code: '40000' error message: 'Error while compiling statement: FAILED: HiveAccessControlException Permission denied: user [hiveuser2] does not have [SELECT] privilege on [default/hivesampletable/clientid,country ...]'.
        
3. Siga el mismo procedimiento para importar los datos. Esta vez, utilice las credenciales de hiveuser2 y modifique la instrucción select de:

        SELECT * FROM "HIVE"."default"."hivesampletable"

    a:

        SELECT clientid, devicemake FROM "HIVE"."default"."hivesampletable"

    Cuando termine, verá dos columnas de datos importados.

## <a name="next-steps"></a>Pasos siguientes

* Para configurar un clúster de HDInsight con Enterprise Security Package, consulte [Configuración de clústeres de HDInsight con Enterprise Security Package](apache-domain-joined-configure.md).
* Para administrar un clúster de HDInsight con ESP, consulte [Administración de clústeres de HDInsight con Enterprise Security Package](apache-domain-joined-manage.md).
* Para ejecutar consultas de Hive mediante SSH en clústeres de HDInsight con ESP, consulte [Uso de SSH con HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md#authentication-domain-joined-hdinsight).
* Para conectar Hive mediante Hive JDBC, consulte [Consulta de Apache Hive mediante el controlador JDBC en HDInsight](../hadoop/apache-hadoop-connect-hive-jdbc-driver.md).
* Para conectar Excel a Hadoop con Hive ODBC, consulte [Conexión de Excel a Apache Hadoop con el controlador ODBC de Microsoft Hive](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* Para conectar Excel a Hadoop con Power Query, consulte [Conexión de Excel a Apache Hadoop con Power Query](../hadoop/apache-hadoop-connect-excel-power-query.md).
