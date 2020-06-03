---
title: Conversión de palabra en vector
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo usar tres modelos de Word2Vec proporcionados para extraer un vocabulario y sus incrustaciones de palabras correspondientes de una corpus de texto.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/19/2020
ms.openlocfilehash: e0e796b75690bcacc6be8ef29b8b490c7faa40af
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/26/2020
ms.locfileid: "83853657"
---
# <a name="convert-word-to-vector"></a>Conversión de palabra en vector

En este artículo se describe cómo usar el módulo **Convertir una palabra en Vector** en el diseñador de Azure Machine Learning (vista previa), para aplicar varios modelos Word2Vec diferentes (Word2Vec, FastText, el modelo previamente entrenado) en el corpus de texto que especificó como entrada y generar un vocabulario con incrustaciones de palabras.

En este módulo se usa la biblioteca Gensim. Para obtener más información sobre Gensim, consulte su [sitio web oficial](https://radimrehurek.com/gensim/apiref.html) que incluye tutoriales y explicación de los algoritmos.

### <a name="more-about-convert-word-to-vector"></a>Más información sobre Conversión de palabra en vector

Por lo general, la conversión de palabras a vectores o de vectorización de palabras es un proceso de procesamiento de lenguaje natural, que utiliza modelos de lenguaje o técnicas para asignar palabras al espacio vectorial, es decir, para representar cada palabra por un vector de números reales y, mientras tanto, las palabras con significados similares tienen representaciones similares.

Las incrustaciones de palabras se pueden usar como entrada inicial para las tareas de nivel inferior de NLP, como la clasificación de texto, el análisis de opiniones, etc.

Entre las distintas tecnologías de incrustación de palabras, en este módulo hemos implementado tres métodos ampliamente utilizados, incluidos dos modelos de aprendizaje en línea, Word2Vec y FastText, además de un modelo entrenado previamente, guante-wiki-gigaword-100. Los modelos de aprendizaje en línea están entrenados en los datos de entrada, mientras que los modelos previamente entrenados están entrenados sin conexión en un corpus de texto más grande (por ejemplo, Wikipedia, Google News), normalmente contiene un aproximado de 100 mil millones palabras y, a continuación, la incrustación de palabras permanece constante durante la vectorización de palabras. Los modelos de palabras previamente entrenados proporcionan ventajas como un tiempo de entrenamiento reducido, mejores vectores de palabra codificados y un rendimiento general mejorado.

+ Word2Vec es una de las técnicas más populares para aprender incrustaciones de palabras mediante la red neuronal superficial, la teoría se trata en este documento, disponible como descarga en PDF: [Estimación eficaz de representaciones de palabras en el Espacio vectorial, Mikolov, Tomas, et al](https://arxiv.org/pdf/1301.3781.pdf). La implementación de este módulo se basa en [la biblioteca de gensim para Word2Vec](https://radimrehurek.com/gensim/models/word2vec.html).

+ La teoría se trata en este documento, disponible como descarga en PDF: [Enriquecer Vectores de palabra con Información de subpalabra, Bojanowski, Piotr, et al](https://arxiv.org/pdf/1607.04606.pdf). La implementación de este módulo se basa en [la biblioteca de gensim para FastText](https://radimrehurek.com/gensim/models/fasttext.html).

+ Modelo previamente entrenado de Glove: glove-wiki-gigaword-100 es una colección de vectores entrenados previamente basados en el corpus de texto de Wikipedia, que contiene los tokens de la versión 5.6B y las 400.000 de vocabulario sin mayúsculas, el pdf está disponible: [GloVe: Vectores globales para la representación de palabras](https://nlp.stanford.edu/pubs/glove.pdf).

## <a name="how-to-configure-convert-word-to-vector"></a>Procedimiento para configurar Convertir palabra en vector

Este módulo requiere un conjunto de datos que contenga una columna de texto, el texto preprocesado es mejor.

1. Agregue el módulo **Convertir palabra en vector** a una canalización.

2. Como entrada para el módulo, proporcione un conjunto de datos que contenga una o varias columnas de texto.

3. En **Columnas de destino**, elija una o varias columnas que contengan el texto que quiera analizar.

    En general, dado que este módulo crea un vocabulario a partir de texto, el contenido de diferentes columnas difiere, lo que conduce a un contenido de vocabulario diferente; por lo tanto, el módulo solo acepta una columna de destino.

4. Para una **estrategia de Word2Vec**, elija entre `GloVe pretrained English Model`, `Gensim Word2Vec` y `Gensim FastText`.

5. Si la **estrategia Word2Vec** es `Gensim Word2Vec` o `Gensim FastText`:

    + **Algoritmo de entrenamiento Word2Vec**. Elija entre las opciones `Skip_gram` y `CBOW`. La diferencia se presenta en el [papel](https://arxiv.org/pdf/1301.3781.pdf) original.

        El método predeterminado es `Skip_gram`.

    + **Longitud de la incrustación de palabras**. Especifique la dimensionalidad de los vectores de palabra. Corresponde al parámetro `size` de gensim.

        El embedding_size predeterminado es 100.

    + **Tamaño de la ventana de contexto**. Especifique la distancia máxima entre la palabra que se va a predecir y la palabra actual. Corresponde al parámetro `window` de gensim.

        El tamaño predeterminado de la ventana es 5.

    + **Número de tiempos**. Especifique el número de tiempos (iteraciones) en el corpus. Corresponde al parámetro `iter` de gensim.

        El número de tiempo predeterminado es 5.

6. Para el **Tamaño máximo de vocabulario**, especifique el número máximo de palabras en el vocabulario generado.

    Si hay más palabras, elimine las poco frecuentes.

    El tamaño predeterminado del vocabulario es 10.000.

7. Para el **recuento mínimo de palabras**, proporcione un recuento mínimo de palabras, lo que hace que el módulo omita todas las palabras que tienen una frecuencia inferior a este valor.

    El valor predeterminado es 5.

8. Envíe la canalización.

## <a name="examples"></a>Ejemplos

El módulo tiene una salida:

+ **Vocabulario con incrustaciones**: Contiene el vocabulario generado, junto con la incrustación de cada palabra, una dimensión ocupa una columna.

### <a name="result-examples"></a>Ejemplos de resultado

Para ilustrar cómo funciona el módulo **Convertir palabra en vector**, en el ejemplo siguiente se aplica este módulo con la configuración predeterminada al conjunto de datos procesado previamente de Wikipedia SP 500 proporcionado en Azure Machine Learning (versión preliminar).

#### <a name="source-dataset"></a>Conjunto de datos de origen

El conjunto de datos contiene una columna de categoría, así como el texto completo capturado de Wikipedia. En esta tabla se muestran solo algunos ejemplos representativos.

|text|
|----------|
|NASDAQ 100 componente s p 500 componente fundación fundador ubicación ciudad campus de Apple 1 bucle infinito calle bucle infinito Cupertino California Cupertino California ubicación país Estados Unidos...|
|br nasdaq 100 nasdaq 100 componente br s p 500 s p 500 componente industry ordenador software fundación br fundador Charles Geschke br John Warnock ubicación sistemas de Adobe...|
|s p 500 s p 500 componente industria automoción industria automoción predecesor general motores corporación 1908 2009 sucesor...|
|s p 500 s p 500 componente industria conglomerado empresa conglomerado fundación fundador ubicación ciudad Fairfield Connecticut Fairfield Connecticut ubicación área país EE. UU...|
|br s p 500 s p 500 componente fundación 1903 fundador William s Harley br Arthur Davidson Harley Davidson fundador Arthur Davidson br Walter Davidson br William a Davidson ubicación...|

#### <a name="output-vocabulary-with-embeddings"></a>Vocabulario de salida con incrustaciones

La tabla siguiente contiene la salida de este módulo que toma como entrada el conjunto de datos de Wikipedia SP 500. La columna situada más a la izquierda indica el vocabulario, su vector de incrustación se representa con los valores de las columnas restantes de la misma fila.

|Vocabulario|Incrustación de dim 0|Incrustación de dim 1|Incrustación de dim 2|Incrustación de dim 3|Incrustación de dim 4|Incrustación de dim 5|…|Incrustación de dim 99|
|-------------|-------------|-------------|-------------|-------------|-------------|-------------|-------------|-------------|
|NASDAQ|-0.375865|0.609234|0.812797|-0.002236|0.319071|-0.591986|…|0.364276
|component|0.081302|0.40001|0.121803|0.108181|0.043651|-0.091452|…|0.636587
|s|-0.34355|-0.037092|-0.012167|0.151542|0.601019|0.084501|…|0.149419
|p|-0.133407|0.073244|0.170396|0.326706|0.213463|-0.700355|…|0.530901
fundación|-0.166819|0.10883|-0.07933|-0.073753|0.262137|0.045725|…|0.27487
fundador|0\.297408|0.493067|0.316709|-0.031651|0.455416|-0.284208|…|0.22798
ubicación|-0.375213|0.461229|0.310698|0.213465|0.200092|0.314288|…|0.14228
city|-0.460828|0.505516|-0.074294|-0.00639|0.116545|0.494368|…|-0.2403
Apple|0.05779|0.672657|0.597267|-0.898889|0.099901|0.11833|…|0.4636
campus|-0.281835|0.29312|0.106966|-0.031385|0.100777|-0.061452|…|0.05978
infinito|-0.263074|0.245753|0.07058|-0.164666|0.162857|0\.027345|…|-0.0525
bucle|-0.391421|0.52366|0.141503|-0.105423|0.084503|-0.018424|…|-0.0521

En este ejemplo, usamos la `Gensim Word2Vec` predeterminada como **estrategia de Word2Vec**, el **algoritmo de aprendizaje** es `Skip-gram`, la **longitud de la incrustación de palabras** es 100, por lo tanto, tenemos 100 columnas de incrustación.

## <a name="technical-notes"></a>Notas técnicas

Esta sección contiene sugerencias y respuestas a las preguntas más frecuentes.

+ Diferencia entre el modelo de entrenamiento en línea y el modelo entrenado previamente

    En este **módulo de Convertir palabra en vector**, proporcionamos tres estrategias diferentes, dos modelos de aprendizaje en línea y un modelo entrenado previamente. El modelo de aprendizaje en línea usa el conjunto de datos de entrada como datos de entrenamiento, genera vectores de palabras y vocabularios durante el entrenamiento, mientras que el modelo previamente entrenado ya está entrenado por un corpus de texto mucho más grande como el texto de Wikipedia o Twitter, por lo que el modelo previamente entrenado es realmente una colección de pares (palabra, incrustación).  

    Si el modelo entrenado previamente de Glove se elige como estrategia de vectorización de palabras, resume un vocabulario del conjunto de datos de entrada y genera un vector de incrustación para cada palabra del modelo entrenado previamente, sin aprendizaje en línea, el uso de un modelo previamente entrenado podría ahorrar tiempo de entrenamiento y tener un mejor rendimiento, especialmente cuando el tamaño del conjunto de datos de entrada es relativamente pequeño.

+ Tamaño de incrustación

    En general, la longitud de la incrustación de palabras se establece en unas cuantas centenas (por ejemplo, 100, 200, 300) para lograr un buen rendimiento, ya que el tamaño de incrustación pequeño implica un pequeño espacio vectorial, lo que puede provocar colisiones de incrustación de palabras.  

    En el caso de los modelos previamente entrenados, la longitud de las incrustaciones de palabras es fija, en esta implementación, el tamaño de la incrustación de glove-wiki-gigaword-100 es 100.


## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning. 

Para obtener una lista de los errores específicos de los módulos de diseñador (versión preliminar), consulte [Códigos de error de Machine Learning](designer-error-codes.md).
