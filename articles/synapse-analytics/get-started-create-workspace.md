---
title: 'Tutorial: Introducción a la creación de un área de trabajo de Synapse'
description: En este tutorial, aprenderá a crear un área de trabajo de Synapse, un grupo de SQL y un grupo de Apache Spark.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 08/27/2020
ms.openlocfilehash: 78ec233e618511c748ed9f51b97161eddc5e8308
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2020
ms.locfileid: "90707533"
---
# <a name="create-a-synapse-workspace"></a>Creación de un área de trabajo de Synapse

En este tutorial, aprenderá a crear un área de trabajo de Synapse, un grupo de SQL y un grupo de Apache Spark. 

## <a name="create-a-synapse-workspace"></a>Creación de un área de trabajo de Synapse

1. Abra [Azure Portal](https://portal.azure.com) y, en la parte superior, busque **Synapse**.
1. En los resultados de la búsqueda, en **Servicios**, seleccione **Azure Synapse Analytics (versión preliminar de las áreas de trabajo)** .
1. Seleccione **Agregar** para crear un área de trabajo.
1. En **Aspectos básicos**, elija un nombre para el área de trabajo. Para este tutorial usaremos **myworkspace**.
1. Necesita una cuenta de ADLSGEN2 para crear un área de trabajo. La opción más sencilla para crear una nueva. Si desea volver a usar una existente, deberá realizar alguna configuración adicional. 
1. OPCIÓN 1 Creación de una nueva cuenta de ADLSGEN2 
    1. Vaya a **Select Data Lake Storage Gen 2** (Seleccionar Data Lake Storage Gen 2). 
    1. Haga clic en **Create New** (Crear nuevo) y asígnele el nombre **contosolake**.
    1. Haga clic en **File System** (Sistema de archivos) y asígnele el nombre **users**.
1. OPCIÓN 2 Uso de una cuenta de ADLSGEN2 existente. Consulte las instrucciones de **Preparación de una cuenta de almacenamiento de ADLSGEN2** en la parte inferior de este documento.
1. El área de trabajo de Azure Synapse usará esta cuenta de almacenamiento como su cuenta de almacenamiento "principal" y el contenedor para almacenar los datos del área de trabajo. El área de trabajo almacena datos en tablas de Apache Spark. Almacena los registros de aplicaciones de Spark en una carpeta denominada **/synapse/workspacename**.
1. Seleccione **Revisar y crear** > **Crear**. El área de trabajo estará lista en unos minutos.


## <a name="open-synapse-studio"></a>Abrir Synapse Studio

Una vez creada el área de trabajo de Azure Synapse, hay dos maneras de abrir Synapse Studio:

* Abrir el área de trabajo de Synapse en [Azure Portal](https://portal.azure.com). En la parte superior de la sección **Información general**, seleccione **Iniciar Synapse Studio**.
* Vaya a `https://web.azuresynapse.net` e inicie sesión en el área de trabajo.

## <a name="create-a-sql-pool"></a>Creación de un grupo de SQL

1. En Synapse Studio, en el panel izquierdo, seleccione **Administrar** > **Grupos de SQL**.
1. Seleccione **Nuevo** y especifique estos valores:

    |Configuración | Valor sugerido | 
    |---|---|---|
    |**Nombre del grupo de SQL**| **SQLDB1**|
    |**Nivel de rendimiento**|**DW100C**|
    |||

1. Seleccione **Revisar y crear** > **Crear**. El grupo de SQL estará listo en unos minutos. El grupo de SQL se asocia con una base de datos del grupo de SQL también denominada **SQLDB1**.

Los grupos de SQL consumen recursos facturables mientras están activos. Puede pausar los grupos más adelante para reducir los costos.

## <a name="create-an-apache-spark-pool"></a>Creación de un grupo de Apache Spark

1. En Synapse Studio, en el lado izquierdo, seleccione **Administrar** > **Grupos de Apache Spark**.
1. Seleccione **Nuevo** y especifique estos valores:

    |Configuración | Valor sugerido | 
    |---|---|---|
    |**Nombre del grupo de Apache Spark**|**Spark1**
    |**Tamaño del nodo**| **Pequeño**|
    |**Número de nodos**| Establezca el valor mínimo en 3 y el máximo en 3|

1. Seleccione **Revisar y crear** > **Crear**. El grupo de Apache Spark estará listo en unos segundos.

> [!NOTE]
> A pesar del nombre, un grupo de Apache Spark no es como un grupo de SQL. Es solo un conjunto de metadatos básicos que se usan para indicar al área de trabajo de Azure Synapse cómo interactuar con Spark.

Al ser metadatos, los grupos de Spark no se pueden iniciar ni detener.

Cuando se realiza una actividad de Spark en Azure Synapse, se especifica el grupo de Spark que se va a usar. El grupo indica a Azure Synapse el número de recursos de Spark que se van a usar. Solo paga por los recursos que utiliza. Al dejar de usar el grupo de forma activa, los recursos agotarán el tiempo de espera y se reciclarán automáticamente.

> [!NOTE]
> Las bases de datos de Spark se crean de forma independiente de los grupos de Spark. Un área de trabajo siempre tiene una base de datos de Spark denominada **default**. Puede crear bases de datos de Spark adicionales.

## <a name="the-sql-on-demand-pool"></a>El grupo de SQL a petición

Todas las áreas de trabajo incluyen un grupo precompilado llamado **SQL on-demand**. Este grupo no se puede eliminar. Este grupo permite trabajar con SQL sin tener que crear ni pensar en la administración de un grupo de SQL en Azure Synapse.

A diferencia de los otros tipos de grupos, la facturación de SQL on-demand se basa en la cantidad de datos que se examinan para ejecutar la consulta (no en el número de recursos usados para ello).

* SQL on-demand tiene sus propias bases de datos de SQL On-Demand, que existen de forma independiente de cualquier grupo de SQL On-Demand.
* Un área de trabajo siempre tiene exactamente un grupo de SQL On-Demand denominado **SQL on-demand**.

## <a name="preparing-a-adlsgen2-storage-account"></a>Preparación de una cuenta de almacenamiento de ADLSGEN2

### <a name="perform-the-following-steps-before-you-create-your-workspace"></a>Realice los pasos siguientes ANTES de crear el área de trabajo.

1. Abra [Azure Portal](https://portal.azure.com).
1. Vaya a la cuenta de almacenamiento existente.
1. Seleccione **Access control (IAM)** (Control de acceso [IAM]) en el panel izquierdo. 
1. Asigne los siguientes roles o asegúrese de que ya estén asignados:
    * Asígnese el rol **Propietario**.
    * Asígnese el rol **Propietario de datos de Storage Blob**.
1. En el panel izquierdo, seleccione **Contenedores** y cree un contenedor.
1. Puede asignarle un nombre al contenedor. En este documento, usaremos el nombre **users**.
1. Acepte el valor predeterminado **Nivel de acceso público** y, después, seleccione **Crear**.

### <a name="perform-the-following-steps-after-you-create-your-workspace"></a>Realice los pasos siguientes DESPUÉS de crear el área de trabajo.

Configure el acceso a la cuenta de almacenamiento desde el área de trabajo. Es posible que las identidades administradas del área de trabajo de Azure Synapse ya tengan acceso a la cuenta de almacenamiento. Siga estos pasos para asegurarse:

1. Abra [Azure Portal](https://portal.azure.com) y la cuenta de almacenamiento principal elegida para el área de trabajo.
1. Seleccione **Control de acceso (IAM)** en el panel izquierdo.
1. Asigne los siguientes roles o asegúrese de que ya estén asignados. Se usa el mismo nombre para la identidad y para el nombre del área de trabajo.
    * Para el rol **Colaborador de datos de Storage Blob** de la cuenta de almacenamiento, asigne **myworkspace** como identidad del área de trabajo.
    * Asigne **myworkspace** como el nombre del área de trabajo.

1. Seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Análisis mediante un grupo de SQL](get-started-analyze-sql-pool.md)
