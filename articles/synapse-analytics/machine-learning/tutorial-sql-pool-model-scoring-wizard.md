---
title: 'Tutorial: Asistente para puntuación del modelo de Machine Learning para grupos de SQL dedicados'
description: Tutorial sobre cómo usar el asistente para puntuación de modelos de Machine Learning a fin de enriquecer datos en grupos de SQL dedicados.
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: jrasnick, garye
ms.date: 09/25/2020
author: nelgson
ms.author: negust
ms.openlocfilehash: 605a5f2f74ca6fb46d851c41f60001c48a95be95
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96450876"
---
# <a name="tutorial-machine-learning-model-scoring-wizard-preview-for-dedicated-sql-pools"></a>Tutorial: Asistente para puntuación de modelos de Machine Learning (versión preliminar) para grupos de SQL dedicados

Aprenda a enriquecer fácilmente los datos en grupos de SQL dedicados con modelos de Machine Learning predictivos.  Ahora los profesionales de datos pueden acceder fácilmente a los modelos que crean los científicos de datos, para realizar análisis predictivos. Un profesional de datos de Synapse puede seleccionar un modelo del registro de modelos de Azure Machine Learning para su implementación en grupos de Synapse SQL e iniciar predicciones para enriquecer los datos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> - Entrenar un modelo de Machine Learning predictivo y registrar el modelo en el registro de modelos de Azure Machine Learning.
> - Usar el asistente para puntuación de SQL para iniciar predicciones en el grupo de SQL dedicado.

Si no tiene una suscripción a Azure, [cree una cuenta gratuita antes de empezar](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Requisitos previos

- Un [área de trabajo de Synapse Analytics](../get-started-create-workspace.md) con una cuenta de almacenamiento de ADLS Gen2 configurada como el almacenamiento predeterminado. Debe ser el **Colaborador de datos de blobs de almacenamiento** del sistema de archivos de ADLS Gen2 con el que quiere trabajar.
- Grupo de SQL dedicado en el área de trabajo de Azure Synapse Analytics. Para más información, consulte [Creación de un grupo de SQL dedicado](../quickstart-create-sql-pool-studio.md).
- Servicio vinculado de Azure Machine Learning en el área de trabajo de Azure Synapse Analytics. Para detalles, consulte [Creación de un servicio vinculado de Azure Machine Learning en Synapse](quickstart-integrate-azure-machine-learning.md).

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Portal de Azure](https://portal.azure.com/)

## <a name="train-a-model-in-azure-machine-learning"></a>Entrenamiento de un modelo en Azure Machine Learning

Antes de empezar, compruebe que la versión de **sklearn** sea 0.20.3.

Antes de ejecutar todas las celdas del cuaderno, compruebe si se está ejecutando la instancia de proceso.

![Comprobación del proceso AML](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-train-00b.png)

1. Navegue a su área de trabajo de Azure Machine Learning.

1. Descargue [Predict NYC Taxi Tips.ipynb](https://go.microsoft.com/fwlink/?linkid=2144301).

1. Inicie el área de trabajo de Azure Machine Learning en [Azure Machine Learning Studio](https://ml.azure.com).

1. Vaya a **Notebooks** y haga clic en **Cargar archivos**, seleccione el archivo "Predict NYC Taxi Tips.ipynb" que descargó y cárguelo.
   ![Carga de archivo](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-train-00a.png)

1. Después de cargar y abrir el cuaderno, haga clic en **Run all cells** (Ejecutar todas las celdas).

   Puede producirse un error en una de las celdas y pedirle que se autentique en Azure. Esté pendiente de esto en las salidas de celda y realice la autenticación en el explorador. Para ello, siga el siguiente vínculo y escriba el código. Luego, vuelva a ejecutar el cuaderno.

1. El cuaderno entrenará un modelo ONNX y lo registrará con MLFlow. Vaya a **Modelos** para comprobar si el nuevo modelo está registrado correctamente.
   ![Modelo en el registro](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-train-00c.png)

1. Al ejecutar el cuaderno también se exportarán los datos de prueba a un archivo CSV. Descargue el archivo CSV en el equipo local. Más adelante, importará el archivo .csv al grupo de SQL dedicado y usará los datos para probar el modelo.

   El archivo CSV se crea en la misma carpeta que el archivo de cuaderno. Haga clic en "Actualizar" en el explorador de archivos si no lo ve de inmediato.

   ![Archivo CSV](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-train-00d.png)

## <a name="launch-predictions-with-sql-scoring-wizard"></a>Inicio de predicciones con el asistente para puntuación de SQL

1. Abra el área de trabajo de Synapse con Synapse Studio.

1. Vaya a **Datos** -> **Vinculado** -> **Cuentas de almacenamiento**. Cargue `test_data.csv` en la cuenta de almacenamiento predeterminada.

   ![Carga de datos](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00a.png)

1. Vaya a **Desarrollar** -> **Scripts SQL**. Cree un nuevo script SQL para cargar `test_data.csv` en el grupo de SQL dedicado.

   > [!NOTE]
   > Actualice la dirección URL del archivo en este script antes de ejecutarlo.

   ```SQL
   IF NOT EXISTS (SELECT * FROM sys.objects WHERE NAME = 'nyc_taxi' AND TYPE = 'U')
   CREATE TABLE dbo.nyc_taxi
   (
       tipped int,
       fareAmount float,
       paymentType int,
       passengerCount int,
       tripDistance float,
       tripTimeSecs bigint,
       pickupTimeBin nvarchar(30)
   )
   WITH
   (
       DISTRIBUTION = ROUND_ROBIN,
       CLUSTERED COLUMNSTORE INDEX
   )
   GO
   
   COPY INTO dbo.nyc_taxi
   (tipped 1, fareAmount 2, paymentType 3, passengerCount 4, tripDistance 5, tripTimeSecs 6, pickupTimeBin 7)
   FROM '<URL to linked storage account>/test_data.csv'
   WITH
   (
       FILE_TYPE = 'CSV',
       ROWTERMINATOR='0x0A',
       FIELDQUOTE = '"',
       FIELDTERMINATOR = ',',
       FIRSTROW = 2
   )
   GO
   
   SELECT TOP 100 * FROM nyc_taxi
   GO
   ```

   ![Carga de datos en un grupo de SQL dedicado](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00b.png)

1. Vaya a **Datos** -> **Área de trabajo**. Para abrir el asistente para puntuación de SQL, haga clic con el botón secundario en la tabla del grupo de SQL dedicado. Seleccione **Machine Learning** -> **Enrich with existing model** (Enriquecer con modelo existente).

   > [!NOTE]
   > La opción Machine Learning no aparece a menos que haya creado un servicio vinculado para Azure Machine Learning (consulte **Requisitos previos** al principio de este tutorial).

   ![Opción Machine Learning](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00c.png)

1. Seleccione un área de trabajo de Azure Machine Learning vinculada en el cuadro desplegable. Se carga una lista de modelos de Machine Learning desde el registro de modelos del área de trabajo elegida de Azure Machine Learning. Actualmente, solo se admiten modelos ONNX, por lo que solo se mostrará este tipo de modelos.

1. Seleccione el modelo que acaba de entrenar y haga clic en **Continuar**.

   ![Selección de un modelo de Azure Machine Learning](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00d.png)

1. A continuación, asigne las columnas de la tabla a las entradas del modelo y especifique las salidas del modelo. Si el modelo se guarda en formato MLFlow y se rellena la firma del modelo, la asignación se realizará automáticamente mediante una lógica basada en la similitud de los nombres. La interfaz también admite la asignación manual.

   Haga clic en **Continuar**.

   ![Asignación de tabla a modelo](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00e.png)

1. El código T-SQL generado se encapsula dentro de un procedimiento almacenado. Este es el motivo por el que debe proporcionar un nombre a un procedimiento almacenado. El binario del modelo que incluye los metadatos (versión, descripción, etc.) se copiará físicamente de Azure Machine Learning en una tabla de grupo de SQL dedicado. Por lo tanto, debe especificar en qué tabla se debe guardar el modelo. Puede elegir "Usar una tabla existente" o "Crear una nueva tabla". Una vez hecho esto, haga clic en **Deploy model + open editor** (Implementar modelo + abrir editor) para implementar el modelo y generar un script de predicción de T-SQL.

   ![Creación de procedimiento](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00f.png)

1. Una vez generado el script, haga clic en "Ejecutar" para ejecutar la puntuación y obtener predicciones.

   ![Ejecución de predicciones](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00g.png)

## <a name="next-steps"></a>Pasos siguientes

- [Inicio rápido: Creación de un servicio vinculado de Azure Machine Learning en Synapse](quickstart-integrate-azure-machine-learning.md)
- [Funcionalidades de aprendizaje automático en Azure Synapse Analytics](what-is-machine-learning.md)
