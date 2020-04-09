---
title: Asignación de Dirichlet latente
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo usar el módulo Asignación de Dirichlet latente para agrupar texto no clasificado de otro modo en varias categorías.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 03/11/2020
ms.openlocfilehash: 1384491489c175ffc338f80a99aa8d5050f835d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80089072"
---
# <a name="latent-dirichlet-allocation"></a>Asignación de Dirichlet latente

En este artículo se describe cómo usar el módulo **Asignación de Dirichlet latente** en el diseñador de Azure Machine Learning (versión preliminar) para agrupar texto sin clasificar de otro modo en varias categorías. 

La Asignación de Dirichlet latente (LDA) se suele utilizar en el procesamiento de lenguaje natural (NLP) para buscar textos similares. Otro término común es *modelado de temas*.

Este módulo toma una columna de texto y genera las salidas siguientes:

+ El texto de origen, junto con una puntuación para cada categoría.

+ Una matriz de características que contiene términos y coeficientes extraídos para cada categoría.

+ Una transformación, que se puede guardar y volver a aplicar al nuevo texto que se usa como una entrada.

En este módulo se usa la biblioteca scikit-learn. Para obtener más información sobre scikit-learn, consulte el repositorio de GitHub, que incluye tutoriales y una explicación del algoritmo.

### <a name="more-about-latent-dirichlet-allocation-lda"></a>Más información sobre la Asignación de Dirichlet latente (LDA)

Por lo general, LDA no es un método para la clasificación por sí mismo, sino que usa un enfoque generativo. Esto significa que no es necesario proporcionar etiquetas de clase conocidas y luego deducir los patrones.  En su lugar, el algoritmo genera un modelo probabilístico que se utiliza para identificar grupos de temas. Puede usar el modelo probabilístico para clasificar los casos de entrenamiento existentes o los nuevos casos que proporcione al modelo como entrada.

Un modelo generativo puede ser preferible porque evita realizar suposiciones convincentes sobre la relación entre el texto y las categorías, y usa solo la distribución de palabras para modelar los temas de forma matemática.

+ La teoría se trata en este documento, disponible como descarga en PDF: [Asignación de Dirichlet latente: Blei, Ng y Jordan](https://ai.stanford.edu/~ang/papers/nips01-lda.pdf)

+ La implementación de este módulo se basa en la [biblioteca scikit-learn](https://github.com/scikit-learn/scikit-learn/blob/master/sklearn/decomposition/_lda.py) de LDA.

Para más información, consulte la sección [Notas técnicas](#technical-notes).

## <a name="how-to-configure-latent-dirichlet-allocation"></a>Configuración de la Asignación de Dirichlet latente

Este módulo requiere un conjunto de datos que contenga una columna de texto, ya sea sin formato o preprocesada.

1. Agregue el módulo **Asignación de Dirichlet latente** a la canalización.

2. Como entrada para el módulo, proporcione un conjunto de datos que contenga una o varias columnas de texto.

3. En **Columnas de destino**, elija una o varias columnas que contengan el texto que quiera analizar.

    Puede elegir varias columnas, pero deben ser del tipo de datos de cadena.

    En general, dado que LDA crea una matriz de características de gran tamaño a partir del texto, normalmente se analiza una sola columna de texto.

4. Para el **número de temas que se van a modelar**, escriba un entero entre 1 y 1000 que indique el número de categorías o temas que desea derivar del texto de entrada.

    De forma predeterminada, se crean 5 temas.

5. En **N-grams** (N-gramas), especifique la longitud máxima de los n-gramas generados durante la operación de hash.

    El valor predeterminado es 2, lo que significa que se generan bigramas y unigramas.

6. Seleccione la opción **Normalizar** para convertir valores de salida en probabilidades. Por lo tanto, en lugar de representar los valores transformados como enteros, los valores de la salida y del conjunto de datos de características se transformarían de la manera siguiente:

    + Los valores del conjunto de datos se representarán como una probabilidad en la que `P(topic|document)`.

    + Los valores de la matriz de temas de características se representarán como una probabilidad en la que `P(word|topic)`.

    > [!NOTE] 
    > En el diseñador de Azure Machine Learning (versión preliminar), dado que la biblioteca en la que nos basamos, scikit-learn, ya no es compatible con la salida *doc_topic_distr* sin normalizar de la versión 0.19, en este módulo, el parámetro **Normalizar** solo se puede aplicar a la salida de **Matriz de temas de características** y la salida **Conjunto de datos transformado** siempre está normalizada.

7. Seleccione la opción **Mostrar todas las opciones** y, a continuación, establézcala en TRUE si desea ver y establecer parámetros avanzados adicionales.

    Estos parámetros son específicos de la implementación de scikit-learn de LDA. Hay algunos tutoriales recomendables sobre LDA en scikit-learn, así como el [documento de scikit-learn](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.LatentDirichletAllocation.html) oficial.

    + **Parámetro ro**. Proporcione una probabilidad anterior para la dispersión de las distribuciones de temas. Corresponde al parámetro `topic_word_prior` de sklearn. Usará el valor 1 si espera que la distribución de palabras sea plana; es decir, se supone que todas las palabras son equiprobables. Si cree que la mayoría de las palabras aparecen de forma dispersa, puede establecer un valor mucho menor.

    + **Parámetro alfa**. Especifique una probabilidad anterior para la dispersión de las ponderaciones de tema por documento.  Corresponde al parámetro `doc_topic_prior` de sklearn.

    + **Número de documentos estimado**. Escriba un número que represente la mejor estimación del número de documentos (filas) que se van a procesar. Esto permite que el módulo asigne una tabla hash de tamaño suficiente.  Corresponde al parámetro `total_samples` de scikit-learn.

    + **Tamaño del lote**. Escriba un número que indique el número de filas que se van a incluir en cada lote de texto enviado al modelo de LDA. Corresponde al parámetro `batch_size` de scikit-learn.

    + **Valor inicial de la iteración usada en la programación de actualización de aprendizaje**. Especifique el valor inicial que pondera a la baja la velocidad de aprendizaje para las iteraciones tempranas en el aprendizaje en línea. Corresponde al parámetro `learning_offset` de scikit-learn.

    + **Energía aplicada a la iteración durante las actualizaciones**. Indique el nivel de energía aplicado al recuento de iteraciones para controlar la velocidad de aprendizaje durante las actualizaciones en línea. Corresponde al parámetro `learning_decay` de scikit-learn.

    + **Número de pases sobre los datos**. Especifique el número máximo de veces que el algoritmo va a recorrer los datos. Corresponde al parámetro `max_iter` de scikit-learn.

8. Seleccione la opción **Crear diccionario de n-gramas** o **Crear diccionario de n-gramas antes de LDA**, si desea crear la lista de n-gramas en un paso inicial, antes de clasificar el texto.

    Si crea el diccionario inicial de antemano, puede usarlo más adelante al revisar el modelo. La capacidad de asignar resultados a texto en lugar de índices numéricos suele ser más fácil de interpretar. Sin embargo, el proceso de guardar el diccionario tardará más tiempo y usará almacenamiento adicional.

9. Para el **tamaño máximo del diccionario de n-gramas**, escriba el número total de filas que se pueden crear en el diccionario de n-gramas.

    Esta opción es útil para controlar el tamaño del diccionario. No obstante, si el número de n-gramas de la entrada supera este tamaño, pueden producirse colisiones.

10. Envíe la canalización. El módulo LDA usa el teorema de Bayes para determinar qué temas pueden asociarse a palabras individuales. Las palabras no están asociadas exclusivamente a ningún tema o grupo; en su lugar, cada n-grama tiene una probabilidad aprendida de asociarse a cualquiera de las clases descubiertas.

## <a name="results"></a>Results

El módulo tiene dos salidas:

+ **Conjunto de datos transformado**: Contiene el texto de entrada y un número especificado de categorías descubiertas, junto con las puntuaciones de cada ejemplo de texto de cada categoría.

+ **Matriz de temas de características**: La columna situada más a la izquierda contiene la característica de texto extraído y hay una columna para cada categoría que contiene la puntuación de dicha característica en esa categoría.


### <a name="lda-transformation"></a>Transformación de LDA

Este módulo también genera la *transformación de LDA* que aplica LDA al conjunto de datos.

Puede guardar esta transformación mediante el registro del conjunto de datos en la pestaña **Outputs+logs** (Salidas y registros) del panel derecho del módulo y reutilizarlo para otros conjuntos de datos. Puede resultar útil si ha entrenado un gran corpus y desea volver a usar los coeficientes o las categorías.

### <a name="refining-an-lda-model-or-results"></a>Perfeccionamiento de resultados o un modelo de LDA

Normalmente, no se puede crear un único modelo de LDA que satisfaga todas las necesidades, e incluso un modelo diseñado para una tarea podría requerir muchas iteraciones para mejorar la precisión. Se recomienda probar todos estos métodos para mejorar el modelo:

+ Cambiar los parámetros del modelo
+ Usar la visualización para comprender los resultados
+ Obtener comentarios de expertos en la materia para averiguar si los temas generados son útiles.

Las medidas cualitativas también pueden ser útiles para evaluar los resultados. Para evaluar los resultados del modelado de temas, tenga en cuenta lo siguiente:

+ Precisión: ¿los elementos similares son realmente similares?
+ Diversidad: ¿el modelo puede discriminar entre elementos similares cuando sea necesario para el problema empresarial?
+ Escalabilidad: ¿funciona en una amplia gama de categorías de texto o solo en un dominio de destino limitado?

La precisión de los modelos basados en LDA a menudo se puede mejorar mediante el procesamiento de lenguaje natural para limpiar, resumir y simplificar o clasificar el texto. Por ejemplo, las técnicas siguientes, todas compatibles con Azure Machine Learning, pueden mejorar la precisión de la clasificación:

+ Detención de la eliminación de palabras

+ Normalización de mayúsculas

+ Lematización

+ Reconocimiento de entidades con nombre

Para obtener más información, consulte [Preprocesamiento de texto](preprocess-text.md).

En el diseñador, también puede usar las bibliotecas de R o Python para el procesamiento de texto: [Ejecución de script de R](execute-r-script.md), [Ejecución de script de Python](execute-python-script.md)



## <a name="technical-notes"></a>Notas técnicas

Esta sección contiene detalles de implementación, sugerencias y respuestas a las preguntas más frecuentes.

### <a name="implementation-details"></a>Detalles de la implementación

De forma predeterminada, las distribuciones de salidas de un conjunto de datos transformado y una matriz de temas de características se normalizan como probabilidades.

+ El conjunto de datos transformado se normaliza como la probabilidad condicional de los temas dado un documento. En este caso, la suma de cada fila es igual a 1.

+ La matriz de temas de características se normaliza como la probabilidad condicional de las palabras dado un tema. En este caso, la suma de cada columna es igual a 1.

> [!TIP]
> En ocasiones, es posible que el módulo devuelva un tema vacío, que suele deberse a la inicialización pseudoaleatoria del algoritmo.  Si esto ocurre, puede intentar cambiar los parámetros relacionados, como el tamaño máximo del diccionario de n-gramas o el número de bits que se va a usar para la operación de hash de características.

### <a name="lda-and-topic-modeling"></a>Modelado de temas y LDA

La Asignación de Dirichlet latente (LDA) se utiliza a menudo para el *modelado de temas basado en contenido*, lo que básicamente significa el aprendizaje de categorías de texto sin clasificar. En el modelado de temas basado en contenido, un tema es una distribución de palabras.

Por ejemplo, supongamos que ha proporcionado un corpus de las opiniones de los clientes, que incluye una cantidad enorme de productos. El texto de las opiniones enviadas por muchos clientes con el tiempo contendrá muchos términos, algunos de los cuales se usarán en varios temas.

Un **tema** que se identifica mediante el proceso de LDA puede representar opiniones de un producto individual A, o bien representar un grupo de opiniones del producto. Para LDA, el tema en sí es solo una distribución de probabilidades con el tiempo para un conjunto de palabras.

Los términos rara vez son exclusivos de un producto, pero pueden hacer referencia a otros productos o ser términos generales que se aplican a todo ("fantástico", "horrible"). Otros términos podrían ser palabras irrelevantes.  Sin embargo, es importante comprender que el método de LDA no pretende capturar todas las palabras del universo ni comprender cómo se relacionan las palabras, aparte de las probabilidades de coocurrencia. Solo puede agrupar palabras que se usaron en el dominio de destino.

Una vez calculados los índices de términos, se comparan filas de texto individuales mediante una medida de similitud basada en la distancia, para determinar si dos partes de texto son iguales.  Por ejemplo, podría encontrar que el producto tiene varios nombres que están estrechamente correlacionados. O bien, puede que términos extremadamente negativos estén asociados normalmente a un producto determinado. Puede usar la medida de similitud para identificar los términos relacionados y para crear recomendaciones.

###  <a name="module-parameters"></a>Parámetros del módulo

|Nombre|Tipo|Intervalo|Opcional|Valor predeterminado|Descripción|  
|----------|----------|-----------|--------------|-------------|-----------------|  
|Columnas de destino|Selección de columnas||Obligatorio|StringFeature|Nombre o índice de la columna de destino|  
|Número de temas que se van a modelar|Entero|[1;1000]|Obligatorio|5|Modelar la distribución del documento en N temas|  
|N-gramas|Entero|[1;10]|Obligatorio|2|Orden de n-gramas generados durante la operación hash|  
|Normalizar|Boolean|Verdadero o falso|Obligatorio|true|Normalice la salida a las probabilidades.  El conjunto de datos transformado será P(tema&#124;documento) y la matriz de temas de características será P(palabra&#124;tema)|  
|Mostrar todas las opciones|Boolean|Verdadero o falso|Obligatorio|False|Presenta parámetros adicionales específicos de LDA en línea de scikit-learn|  
|Parámetro ro|Float|[0.00001;1.0]|Se aplica cuando se selecciona la casilla **Mostrar todas las opciones**|0,01|Palabra de tema antes de la distribución|  
|Parámetro alfa|Float|[0.00001;1.0]|Se aplica cuando se selecciona la casilla **Mostrar todas las opciones**|0,01|Tema del documento antes de la distribución|  
|Número de documentos estimado|Entero|[1;int.MaxValue]|Se aplica cuando se selecciona la casilla **Mostrar todas las opciones**|1000|Número de documentos estimado (corresponde al parámetro total_samples)|  
|Tamaño del lote|Entero|[1;1024]|Se aplica cuando se selecciona la casilla **Mostrar todas las opciones**|32|Tamaño del lote|  
|Valor inicial de la iteración usada en la programación de actualización de la velocidad de aprendizaje|Entero|[0;int.MaxValue]|Se aplica cuando se selecciona la casilla **Mostrar todas las opciones**|0|Valor inicial que pondera a la baja la velocidad de aprendizaje de las iteraciones tempranas. Corresponde al parámetro learning_offset|  
|Energía aplicada a la iteración durante las actualizaciones|Float|[0.0;1.0]|Se aplica cuando se selecciona la casilla **Mostrar todas las opciones**|0.5|Energía aplicada al recuento de iteraciones para controlar la velocidad de aprendizaje. Corresponde al parámetro learning_decay |  
|Número de iteraciones de entrenamiento|Entero|[1;1024]|Se aplica cuando se selecciona la casilla **Mostrar todas las opciones**|25|Número de iteraciones de entrenamiento|  
|Crear diccionario de n-gramas|Boolean|Verdadero o falso|Se aplica cuando la casilla **Mostrar todas las opciones** *no* está seleccionada|True|Crea un diccionario de n-gramas antes del cálculo de LDA. Útil para la inspección e interpretación del modelo|  
|Tamaño máximo del diccionario de n-gramas|Entero|[1;int.MaxValue]|Se aplica cuando la opción **Crear diccionario de n-gramas** es Verdadero.|20000|Tamaño máximo del diccionario de n-gramas. Si el número de tokens de la entrada supera este tamaño, pueden producirse colisiones|  
|Número de bits que se usarán para la operación de hash de características|Entero|[1;31]|Se aplica cuando la casilla **Mostrar todas las opciones** *no* está seleccionada y el valor de **Crear diccionario de n-gramas** es Falso|12|Número de bits que se usarán para la operación de hash de características| 
|Crear diccionario de n-gramas antes de LDA|Boolean|Verdadero o falso|Se aplica cuando se selecciona la casilla **Mostrar todas las opciones**|True|Crea un diccionario de n-gramas antes de LDA. Útil para la inspección e interpretación del modelo|  
|Número máximo de n-gramas en el diccionario|Entero|[1;int.MaxValue]|Se aplica cuando la casilla **Mostrar todas las opciones** está seleccionada y la opción **Crear diccionario de n-gramas** es Verdadero|20000|Tamaño máximo del diccionario. Si el número de tokens de la entrada supera este tamaño, pueden producirse colisiones|  
|Número de bits de hash|Entero|[1;31]|Se aplica cuando la casilla **Mostrar todas las opciones** está seleccionada y la opción **Crear diccionario de n-gramas** es Falso|12|Número de bits que se usarán durante la operación de hash de características|   


## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning.   
Para obtener una lista de los errores específicos de los módulos, consulte [Excepciones y códigos de error para el diseñador](designer-error-codes.md).
