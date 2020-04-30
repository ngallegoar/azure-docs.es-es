---
title: 'Inicio rápido: Creación de un grupo de Synapse SQL'
description: Cree un nuevo grupo de Synapse SQL para un área de trabajo de Azure Synapse Analytics siguiendo los pasos de esta guía.
services: synapse-analytics
author: malvenko
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/15/2020
ms.author: josels
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: a50f203b8057dd4ea51eff1cf09fc97fb598cb3e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82096442"
---
# <a name="quickstart-create-a-synapse-sql-pool"></a>Inicio rápido: Creación de grupo de Synapse SQL

Azure Synapse Analytics ofrece varios motores de análisis que le ayudarán a ingerir, transformar, modelar, analizar y servir sus datos. Un grupo de SQL ofrece funcionalidades de proceso y almacenamiento basadas en T-SQL. Después de crear un grupo de SQL en el área de trabajo de Synapse, los datos se pueden cargar, modelar, procesar y servir para obtener información.

En este inicio rápido aprenderá a crear un grupo de SQL en un área de trabajo de Synapse mediante Azure Portal.

Si no tiene una suscripción a Azure, [cree una cuenta gratuita antes de empezar](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Prerrequisitos

[Área de trabajo de Synapse](quickstart-create-workspace.md)

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Portal de Azure](https://portal.azure.com/)

## <a name="create-new-sql-pool"></a>Creación de un grupo de SQL

1. En el área de trabajo de Synapse en la que desea crear el grupo de SQL, haga clic en el comando **New SQL pool** (Nuevo grupo de SQL) en la barra superior.
![Información general sobre el área de trabajo de Synapse con un recuadro rojo alrededor del comando para crear un nuevo grupo de SQL.](media/quickstart-create-sql-pool/quickstart-create-sql-pool-01.png)
2. Escriba la siguiente información en la pestaña **Datos básicos**.

    | Configuración | Valor sugerido | Descripción |
    | :------ | :-------------- | :---------- |
    | **Nombre del grupo de SQL** | Cualquier nombre válido | Nombre del grupo de SQL. |
    | **Nivel de rendimiento** | DW100c | Establézcalo en el menor tamaño para reducir los costos de este inicio rápido. |
    ||||
  
    ![Flujo de creación del grupo de SQL: pestaña de datos básicos](media/quickstart-create-sql-pool/quickstart-create-sql-pool-02.png)
    > [!IMPORTANT]
    > Tenga en cuenta que existen limitaciones específicas para los nombres que los grupos de SQL pueden usar. Los nombres no pueden contener caracteres especiales, deben tener 15 caracteres como máximo, no contener palabras reservadas y ser únicos en el área de trabajo.

3. Haga clic en **Siguiente: Configuración adicional**.
4. Seleccione **None** (Ninguno) para aprovisionar el grupo de SQL sin datos. Deje seleccionada la intercalación predeterminada.
![Flujo de creación del grupo de SQL: pestaña de configuración adicional.](media/quickstart-create-sql-pool/quickstart-create-sql-pool-03.png)

5. Haga clic en **Revisar + crear**.
6. Asegúrese de que los detalles son correctos en función de lo que se especificó anteriormente y haga clic en **Crear**.
![Flujo de creación del grupo de SQL: pestaña de revisión de la configuración.](media/quickstart-create-sql-pool/quickstart-create-sql-pool-04.png)

7. Llegados a este punto, se iniciará el flujo de aprovisionamiento de recursos.
 ![Flujo de creación del grupo de SQL: aprovisionamiento de recursos.](media/quickstart-create-sql-pool/quickstart-create-sql-pool-06.png)

8. Una vez completado el aprovisionamiento, al desplazarse al área de trabajo se mostrará una nueva entrada para el grupo de SQL recién creado.
 ![Flujo de creación del grupo de SQL: aprovisionamiento de recursos.](media/quickstart-create-sql-pool/quickstart-create-sql-pool-07.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Siga los pasos que se indican a continuación para eliminar el grupo de SQL del área de trabajo.
> [!WARNING]
> Al eliminar un grupo de SQL, se quitan del área de trabajo el motor de análisis y los datos almacenados en la base de datos del grupo de SQL eliminado. Ya no será posible conectarse al grupo de SQL y todas las consultas, canalizaciones y cuadernos que lean o escriban en este grupo de SQL dejarán de funcionar.

Si quiere eliminar el grupo de SQL, complete estos pasos:

1. Vaya a la hoja de grupos de SQL en la hoja del área de trabajo
1. Seleccione el grupo de SQL que se va a eliminar (en este caso, **contosoedw**).
1. Selecciónelo y presione **Eliminar**.
1. Confirme la eliminación y presione el botón **Eliminar**.
 ![Información general del grupo de SQL: resaltado de la confirmación de eliminación.](media/quickstart-create-sql-pool/quickstart-create-sql-pool-11.png)
1. Cuando el proceso se complete correctamente, el grupo de SQL dejará de aparecer en los recursos del área de trabajo.

Una vez creado el grupo de SQL, estará disponible en el área de trabajo para la carga de datos, el procesamiento de secuencias, la lectura del lago, etc.

## <a name="next-steps"></a>Pasos siguientes

- Consulte [Quickstart: Creación de un grupo de Apache Spark (versión preliminar) en Synapse Analytics mediante herramientas web](quickstart-apache-spark-notebook.md).
- Consulte [Quickstart: Creación de un grupo de Apache Spark (versión preliminar)](quickstart-create-apache-spark-pool.md).
