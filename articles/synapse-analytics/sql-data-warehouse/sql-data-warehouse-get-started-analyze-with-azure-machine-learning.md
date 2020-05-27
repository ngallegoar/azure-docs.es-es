---
title: Análisis de datos con Azure Machine Learning
description: Use Azure Machine Learning para crear un modelo de aprendizaje automático predictivo basado en los datos almacenados en Azure Synapse.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/05/2020
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019
tag: azure-Synapse
ms.openlocfilehash: daaa5e3a075eee19ab473818ae3bd84d4bd3b32b
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83683670"
---
# <a name="analyze-data-with-azure-machine-learning"></a>Análisis de datos con Azure Machine Learning
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](../sql/get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

En este tutorial se usa Azure Machine Learning para crear un modelo de aprendizaje automático predictivo basado en los datos almacenados en Azure Synapse. En concreto, crearemos una campaña de marketing dirigida para Adventure Works, una tienda de bicicletas, mediante la predicción de la probabilidad que existe de que un cliente compre una bicicleta.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Integrating-Azure-Machine-Learning-with-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a>Prerrequisitos
Para realizar este tutorial, necesita:

* Un grupo de SQL con la base de datos de ejemplo AdventureWorksDW previamente cargada. Para aprovisionarla, vea [Creación de un grupo de SQL](create-data-warehouse-portal.md) y cargue los datos de ejemplo. Si ya tiene un almacenamiento de datos pero no tiene datos de ejemplo, puede [cargar manualmente los datos de ejemplo](load-data-from-azure-blob-storage-using-polybase.md).

## <a name="1-get-the-data"></a>1. Obtener los datos
Los datos están en la vista dbo.vTargetMail en la base de datos AdventureWorksDW. Para leer estos datos:

1. Inicie sesión en [Azure Machine Learning Studio](https://studio.azureml.net/) y haga clic en mis experimentos.
2. Haga clic en **+ NUEVO** en la parte inferior izquierda de la pantalla y seleccione **Experimento en blanco**.
3. Escriba un nombre para el experimento: Marketing dirigido.
4. Arrastre el módulo **Importar datos** de **Entrada y salida de datos** desde el panel de módulos hasta el lienzo.
5. Especifique los detalles del grupo de SQL en el panel Propiedades.
6. Especifique la **consulta** de la base de datos para leer los datos de interés.

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

Para ejecutar el experimento, haga clic en **Ejecutar** en el lienzo de experimentos.

![Ejecución del experimento](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1-reader-new.png)

Cuando el experimento haya terminado de ejecutarse correctamente, haga clic en el puerto de salida en la parte inferior del módulo del lector y seleccione **Visualizar** para ver los datos importados.

![Ver datos importados](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3-readerdata-new.png)

## <a name="2-clean-the-data"></a>2. Limpiar los datos
Para limpiar los datos, se quitarán algunas columnas que no son relevantes para el modelo. Para ello, siga estos pasos:

1. Arrastre el módulo **Seleccionar columnas en el conjunto de datos** bajo la opción **Transformación de datos < Manipulación** en el lienzo. Conecte este módulo al módulo **Importar datos**.
2. Haga clic en **Iniciar selector de columnas** en el panel Propiedades para especificar las columnas que desea quitar.

   ![Columnas del proyecto](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4-projectcolumns-new.png)
3. Excluya dos columnas: CustomerAlternateKey y GeographyKey.

   ![Quitar las columnas innecesarias](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5-columnselector-new.png)

## <a name="3-build-the-model"></a>3. Generar el modelo
Dividiremos los datos en 80-20: 80 % para entrenar un modelo de aprendizaje automático y un 20 % para probar el modelo. Usaremos los algoritmos de "dos clases" para este problema de clasificación binaria.

1. Arrastre el módulo **Dividir** al lienzo.
2. En el panel de propiedades, escriba 0,8 en la fracción de filas del primer conjunto de datos de salida.

   ![Dividir los datos en conjunto de entrenamiento y prueba](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6-split-new.png)
3. Arrastre el módulo **Árbol de decisión aumentado de dos clases** al lienzo.
4. Arrastre el módulo **Modelo de entrenamiento** al lienzo y especifique las entradas conectándolo a los módulos **Árbol de decisiones mejorado de dos clases** (algoritmo ML) y **Dividir** (datos para entrenar el algoritmo). 

     ![Conectar el módulo Entrenar modelo](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7-train-new.png)
5. Luego, haga clic en **Iniciar el selector de columnas** en el panel Propiedades. Seleccione la columna **BikeBuyer** como columna de predicción.

   ![Seleccionar la columna de predicción](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8-traincolumnselector-new.png)

## <a name="4-score-the-model"></a>4. Puntuación del modelo
Ahora, probaremos cómo funciona el modelo con datos de prueba. Compararemos el algoritmo que elijamos con otro algoritmo para comprobar cuál funciona mejor.

1. Arrastre el módulo **Modelo de puntuación** al lienzo y conéctelo a los módulos **Modelo de entrenamiento** y **Datos divididos**.

   ![Puntuación del modelo](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9-score-new.png)
2. Arrastre **Máquina del punto de Bayes de dos clases** al lienzo del experimento. Compararemos cómo funciona este algoritmo en comparación con el Árbol de decisión aumentado de dos clases.
3. Copie y pegue los módulos Entrenar modelo y Puntuar modelo en el lienzo.
4. Arrastre el módulo **Evaluar modelo** al lienzo para comparar los dos algoritmos.
5. **Ejecute** el experimento.

   ![Ejecución del experimento](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10-evaluate-new.png)
6. Haga clic en el puerto de salida en la parte inferior del módulo Evaluar modelo y haga clic en Visualizar.

   ![Visualizar los resultados de evaluación](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11-evalresults-new.png)

Las métricas proporcionadas son la curva ROC, el diagrama de retirada-precisión y la curva de elevación. Al mirar estas métricas, podemos ver que el primer modelo funciona mejor que el segundo. Para ver lo que ha predicho el primer modelo, haga clic en el puerto de salida de Puntuar modelo y haga clic en Visualizar.

![Visualizar los resultados de puntuación](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12-scoreresults-new.png)

Verá dos columnas más agregadas al conjunto de datos de prueba.

* Probabilidades puntuadas: la probabilidad de que un cliente sea comprador de bicicletas.
* Etiquetas puntuadas: la clasificación realizada por el modelo – comprador de bicicletas (1) o no (0). Este umbral de probabilidad para etiquetar se establece en 50% y se puede ajustar.

Comparación de la columna BikeBuyer (real) con las etiquetas puntuadas (predicción), puede ver cómo ha funcionado el modelo. Luego puede usar este modelo para realizar predicciones de los nuevos clientes y publicarlo como un servicio web o volver a escribir los resultados en Azure Synapse.

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre la creación de modelos de aprendizaje automático predictivo, consulte [Introducción a Machine Learning en Azure](https://docs.microsoft.com/azure/machine-learning/overview-what-is-azure-ml).