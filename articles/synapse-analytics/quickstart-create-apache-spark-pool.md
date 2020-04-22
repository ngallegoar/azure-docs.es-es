---
title: 'Inicio rápido: Creación de un grupo de Apache Spark (versión preliminar)'
description: Cree un grupo de Apache Spark para un área de trabajo de Azure Synapse Analytics siguiendo los pasos de esta guía.
services: synapse-analytics
author: malvenko
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/15/2020
ms.author: josels
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 1457549fef3a78356c8b1af6be620fdf30ddab46
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81420629"
---
# <a name="quickstart-create-a-new-apache-spark-pool-preview"></a>Inicio rápido: Creación de un nuevo grupo de Apache Spark (versión preliminar)

Synapse Analytics ofrece varios motores de análisis que le ayudarán a ingerir, transformar, modelar, analizar y servir sus datos. Un grupo de Apache Spark ofrece funcionalidades de proceso de macrodatos de código abierto. Después de crear un grupo de Apache Spark en el área de trabajo de Synapse, los datos se pueden cargar, modelar, procesar y servir para obtener información.

En este inicio rápido, va a aprender a usar Azure Portal para crear un trabajo de Apache Spark en un área de trabajo de Synapse.

> [!IMPORTANT]
> La facturación de las instancias de Spark se prorratea por minuto, tanto si se usan como si no. Asegúrese de cerrar la instancia de Spark después de que haya terminado de usarla, o configure un breve tiempo de espera. Para obtener más información, consulte la sección **Limpieza de recursos** de este artículo.

Si no tiene una suscripción a Azure, [cree una cuenta gratuita antes de empezar](https:/azure.microsoft.com/free/).

## <a name="prerequisites"></a>Prerrequisitos

- Una suscripción a Azure: [cree una cuenta gratuita](https:/azure.microsoft.com/free/)
- [Área de trabajo de Synapse Analytics](quickstart-create-workspace.md)

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Portal de Azure](https:/portal.azure.com/)

## <a name="create-new-apache-spark-pool"></a>Creación de un grupo de Apache Spark

1. En el área de trabajo de Synapse en la que desea crear el grupo de Apache Spark, haga clic en **New Apache Spark pool** (Nuevo grupo de Apache Spark) en la barra superior.
![Información general del área de trabajo de Synapse con un cuadro rojo alrededor del comando para crear un nuevo grupo de Apache Spark](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-01.png)
2. En la pestaña **Datos básicos**, escriba la siguientes información:

    |Configuración | Valor sugerido | Descripción |
    | :------ | :-------------- | :---------- |
    | **Nombre del grupo de Apache Spark** | Un nombre de grupo válido. | Este es el nombre que tendrá el grupo de Apache Spark. |
    | **Tamaño del nodo** | Pequeño (4 vCPU/32 GB) | Establézcalo en el menor tamaño para reducir los costos de este artículo de inicio rápido |
    | **Autoscale** | habilitado | Deje la configuración predeterminada. |
    | **Número de nodos** | 3 - 40 | Deje la configuración predeterminada. |
    ||||

    ![Flujo de creación del grupo de Apache Spark: pestaña de datos básicos.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-02.png)
    > [!IMPORTANT]
    > Tenga en cuenta que existen limitaciones específicas para los nombres que pueden usar los grupos de Apache Spark. Los nombres solo deben contener letras o números, deben tener 15 caracteres o menos, deben comenzar con una letra, no contener palabras reservadas y ser únicos en el área de trabajo.

3. Haga clic en **Siguiente: Configuración adicional** y revise la configuración predeterminada. No modifique ninguna configuración predeterminada.
![Flujo de creación del grupo de Apache Spark: pestaña de configuración adicional.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-03.png)

4. Haga clic en **Siguiente: Etiquetas**. No agregue ninguna etiqueta.
![Flujo de creación del grupo de Apache Spark: pestaña de configuración adicional.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-03-tags.png)

5. Haga clic en **Revisar + crear**.

6. Asegúrese de que los detalles son correctos en función de lo que se especificó anteriormente y haga clic en **Crear**.
![Flujo de creación del grupo de SQL: pestaña de revisión de la configuración.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-05.png)

7. Llegados a este punto, se iniciará el flujo de aprovisionamiento de recursos, e indica cuando ha finalizado ![Flujo de creación del grupo de Apache Spark: aprovisionamiento de recursos.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-06.png)

8. Una vez completado el aprovisionamiento, al desplazarse al área de trabajo se mostrará una nueva entrada para el grupo de Apache Spark recién creado.
 ![Flujo de creación del grupo de Apache Spark: aprovisionamiento de recursos.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-07.png)

9. En este momento, no hay ningún recurso en ejecución, sin cargos por Spark, y ha creado metadatos sobre las instancias de Spark que desea crear.

## <a name="clean-up-resources"></a>Limpieza de recursos

Siga los pasos que se indican a continuación para eliminar el grupo de Apache Spark del área de trabajo.
> [!WARNING]
> Al eliminar un grupo de Apache Spark, se quitará el motor de análisis del área de trabajo. Ya no será posible conectarse al grupo de SQL y todas las consultas, canalizaciones y cuadernos que usen este grupo de Apache Spark dejarán de funcionar.

Si está seguro de que quiere eliminar el grupo de Apache Spark, haga lo siguiente:

1. Vaya a la hoja Grupos de Apache Spark en el área de trabajo.
2. Seleccione el grupo de Apache que se va a eliminar (en este caso, **contosospark**)
3. Presione **Eliminar**.
 ![Lista de grupos de Apache Spark, con el grupo creado recientemente seleccionado.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-08.png)
4. Confirme la eliminación y presione el botón **Eliminar**.
 ![Cuadro de diálogo de confirmación para eliminar el grupo de Apache Spark seleccionado.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-10.png)
5. Cuando el proceso se complete correctamente, el grupo de Apache Spark dejará de aparecer en los recursos del área de trabajo.

Una vez creado el grupo de SQL, está disponible en el área de trabajo para la carga de datos, el procesamiento de secuencias, la lectura del lago, etc.

## <a name="next-steps"></a>Pasos siguientes

- Consulte [Quickstart: Creación de un grupo de Apache Spark (versión preliminar) en Synapse Analytics mediante herramientas web](spark/apache-spark-notebook-create-spark-use-sql.md).
- Consulte [Quickstart: Creación de un grupo de Synapse SQL mediante Azure Portal](quickstart-create-sql-pool.md).
