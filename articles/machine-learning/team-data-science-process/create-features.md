---
title: 'Ingeniería de características en ciencia de datos: proceso de ciencia de datos en equipo'
description: Obtenga información sobre la ingeniería de características y su rol en el proceso de mejora de los datos del aprendizaje automático.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: conceptual
ms.date: 05/14/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath, contperfq4
ms.openlocfilehash: 5e84a3930d350ec45cef7119342e3e4d2d5daaee
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91250664"
---
# <a name="feature-engineering-in-data-science"></a>Ingeniería de características en ciencia de datos

En este artículo, obtendrá información sobre la ingeniería de características y su rol en la mejora de los datos en el aprendizaje automático. Obtenga información de los ejemplos ilustrativos extraídos de los experimentos de [Azure Machine Learning Studio (clásico)](../overview-what-is-machine-learning-studio.md#ml-studio-classic-vs-azure-machine-learning-studio). 

* **Diseño de características**: El proceso de crear nuevas características a partir de datos sin procesar para aumentar la eficacia predictiva del algoritmo de aprendizaje. La ingeniería de características debe capturar información adicional que no se pueda obviar fácilmente en el conjunto de características originales.
* **Selección de características**: El proceso de seleccionar el subconjunto de claves de las características en un intento por reducir la dimensionalidad del problema de entrenamiento.

Normalmente, la **ingeniería de características** se aplica primero para generar características adicionales y, a continuación, se realiza **selección de características** para eliminar aquellas que son irrelevantes, redundantes o altamente correlacionadas.

La selección e ingeniería de características forman parte de la [fase de modelado](lifecycle-modeling.md) del proceso de ciencia de datos en equipo (TDSP). Para obtener más información sobre TDSP y el ciclo de vida de ciencia de datos, consulte [¿qué es el TDSP?](overview.md)

## <a name="what-is-feature-engineering"></a>¿Qué es la ingeniería de características?

Los datos de entrenamiento se componen de una matriz formada por filas y columnas. Cada fila de la matriz es una observación o un registro. Las columnas de cada fila son las características que describen cada registro. Las características especificadas en el diseño experimental deben caracterizar los patrones de los datos.

Aunque muchos de los campos de datos sin procesar se pueden usar directamente para entrenar un modelo, a menudo es necesario crear características adicionales (de ingeniería) para un conjunto de datos de entrenamiento mejorado.

Las características diseñadas que mejoran el entrenamiento proporcionan información que ayuda a diferenciar de mejor manera los patrones de los datos. Pero este proceso es, en cierto modo, un arte. Las decisiones acertadas y productivas a menudo requieren conocimiento especializado.

## <a name="example-1-add-temporal-features-for-a-regression-model"></a>Ejemplo 1: Incorporación de características temporales para un modelo de regresión

Usemos el experimento [Previsión de demanda de los alquileres de bicicletas](https://gallery.azure.ai/Experiment/Regression-Demand-estimation-4) en Azure Machine Learning Studio (clásico) para mostrar cómo diseñar características para una tarea de regresión. El objetivo de este experimento es predecir la demanda de los alquileres de bicicletas en un mes/día/hora específico.

### <a name="bike-rental-dataset"></a>Conjunto de datos de alquiler de bicicletas

El [Conjunto de datos UCI de alquiler de bicicletas](http://archive.ics.uci.edu/ml/datasets/Bike+Sharing+Dataset/) se basa en datos reales de una empresa de uso compartido de bicicletas basada en Estados Unidos. Representa el número de los alquileres de bicicletas en una hora específica de un día para los años 2011 y 2012. Contiene 17.379 filas y 17 columnas.

El conjunto de características sin procesar contiene condiciones climáticas (temperatura/humedad/velocidad del viento) y el tipo de día (festivo/día de semana). El campo para la predicción es el contador, que representa las bicicletas alquiladas dentro de una hora específica. El recuento oscila entre 1 y 977.

### <a name="create-a-feature-engineering-experiment"></a>Crear un experimento de ingeniería de características

Con el objetivo de construir características eficaces en los datos de entrenamiento, se crean cuatro modelos de regresión con el mismo algoritmo, pero con cuatro conjuntos de datos de entrenamiento distintos. Los cuatro conjuntos de datos representan los mismos datos de entrada sin procesar, pero con un número creciente del conjunto de características. Estas características se agrupan en cuatro categorías:

1. A = características de clima + festivo + día de semana + fin de semana correspondiente al día de la predicción
2. B = el número de bicicletas alquiladas en cada una de las 12 horas anteriores
3. C = el número de bicicletas alquiladas en cada uno de los 12 días anteriores a la misma hora
4. D = el número de bicicletas arrendadas en cada una de las 12 semanas anteriores a la misma hora y el mismo día

Aparte del conjunto de funciones A, que ya existe en los datos sin procesar originales, los otros tres conjuntos de funciones se crean mediante el proceso de diseño de funciones. El conjunto de funciones B captura la demanda de bicicletas reciente. El conjunto de características C captura la demanda de bicicletas en una hora específica. El conjunto de características D captura la demanda de bicicletas en una hora específica y un día de la semana específico. Los conjuntos de datos de entrenamiento incluyen el conjunto de características A, A+B, A+B+C y A+B+C+D, respectivamente.

### <a name="feature-engineering-using-studio-classic"></a>Ingeniería de características con Studio (clásico)

En el experimento de Studio (clásico), estos cuatro conjuntos de datos de entrenamiento se forman a través de cuatro ramas del conjunto de datos de entrada procesado previamente. A excepción de la rama que se encuentra más a la izquierda, cada una de estas ramas contiene un módulo [Ejecutar script R](https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/), en el que las características derivadas (conjuntos de características B, C y D) se construyen y anexan al conjunto de datos importado.

En la siguiente figura se muestra el script de R que se usa para crear el conjunto de características B en la segunda rama a la izquierda.

![crear funciones](./media/create-features/addFeature-Rscripts.png)

### <a name="results"></a>Results

La comparación de los resultados de rendimiento de los cuatro modelos se resume en la siguiente tabla: 

![comparación de resultados](./media/create-features/result1.png)

Las características A+B+C muestran los mejores resultados. La tasa de errores disminuye cuando se incluyen conjuntos de datos adicionales en los datos de entrenamiento. Esto verifica la presunción con respecto a que los conjuntos de características B y C proporcionan información pertinente adicional para la tarea de regresión. Sin embargo, agregar la característica D no parece proporcionar reducción adicional ninguna en lo que respecta a la tasa de errores.

## <a name="example-2-create-features-for-text-mining"></a><a name="example2"></a> Ejemplo 2: Crear características para la minería de texto

El diseño de características se aplica ampliamente en las tareas relacionadas con la minería de texto como la clasificación de documentos y el análisis de opiniones. Dado que las partes individuales del texto sin formato suelen servir como los datos de entrada, es necesario el proceso de diseño de características para crear las características que implican las frecuencias de palabras/frases.

### <a name="feature-hashing"></a>Hash de características

Para llevar a cabo esta tarea, se aplica una técnica llamada [hash de características](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/feature-hashing) que permite convertir eficazmente las características arbitrarias de texto en índices. En lugar de asociar cada característica de texto (palabras/frases) a un índice determinado, este método aplica una función de hash a las características y el uso de sus valores de hash como índices directamente.

En Studio (clásico), existe un módulo llamado [Hash de características](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/feature-hashing) que crea oportunamente las características de palabras y frases. La figura siguiente muestra un ejemplo del uso de este módulo. El conjunto de datos de entrada contiene dos columnas: la clasificación de libro, que va de 1 a 5, y el contenido mismo de la reseña. El objetivo de este módulo es recuperar una gran cantidad de características nuevas que muestran la frecuencia de repetición de las palabras/frases correspondientes dentro de la reseña de ese libro en especial. Para usar este módulo, complete los siguientes pasos:

* Primero, seleccione la columna que contiene el texto de entrada ("Col2" en este ejemplo).
* Segundo, defina el "Tamaño de bits de hash" en 8,lo que significa que se crearán 2^8=256 características. La palabra/frase en todo el texto tendrá hash en 256 índices. El parámetro "Tamaño de bits de hash" va de 1 a 31. Es menos probable que las palabras/frases tengan hash en el mismo índice si este valor se define para que sea un número mayor.
* Tercero, defina el parámetro "N-gramas" en 2. Con este valor se obtiene la frecuencia de repetición de unigramas (una característica para cada palabra única) y bigramas (una características para cada par de palabras adyacentes) a partir del texto de entrada. El parámetro "N-gramas" va de 0 a 10, lo que indica el número máximo de palabras secuenciales que se incluirán en una característica.  

![Módulo "Hash de características"](./media/create-features/feature-Hashing1.png)

En la figura siguiente se muestra cómo se ven estas nuevas características.

![Ejemplo de "Hash de características"](./media/create-features/feature-Hashing2.png)

## <a name="conclusion"></a>Conclusión
Las funciones diseñadas y seleccionadas aumentan la eficiencia del proceso de entrenamiento, el que intenta extraer la información clave contenida en los datos. También mejoran la eficacia de estos modelos para clasificar los datos de entrada de manera precisa y para predecir resultados de interés de manera más sólida.

El diseño y la selección de características también se pueden combinar para que sea posible hacer un mejor seguimiento computacional del aprendizaje. Para ello, mejora y luego reduce el número de características que se necesitan para calibrar o entrenar un modelo. Matemáticamente, las características seleccionadas son un conjunto mínimo de variables independientes que explican los patrones de los datos y predicen los resultados correctamente.

No siempre es necesario realizar el diseño o la selección de funciones. Depende de los datos, del algoritmo seleccionado y del objetivo del experimento.

## <a name="next-steps"></a>Pasos siguientes

Para crear características para los datos en entornos específicos, consulte los artículos siguientes:

* [Creación de características para los datos en SQL Server](create-features-sql-server.md)
* [Creación de características para los datos en un clúster de Hadoop mediante consultas de Hive](create-features-hive.md)