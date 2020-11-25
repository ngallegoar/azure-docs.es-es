---
title: 'Preparación de datos para ML Studio (clásico): Proceso de ciencia de los datos en equipos'
description: Procesamiento previo y limpieza de los datos para prepararlos para su uso eficaz en el aprendizaje automático.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: caedcf313ab809e9607907545f26ca1b62bbeca7
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96012414"
---
# <a name="tasks-to-prepare-data-for-enhanced-machine-learning"></a>Tareas para preparar los datos para el aprendizaje automático mejorado
El preprocesamiento y la limpieza de datos son tareas importantes que se deben llevar a cabo para que un conjunto de datos se pueda usar para el entrenamiento de modelos. Los datos sin procesar son a menudo ruidosos no confiables y es posible que les falten valores. El uso de estos datos para el modelado puede producir resultados engañosos. Estas tareas forman parte del proceso de ciencia de datos en equipos (TDSP) y normalmente siguen una exploración inicial de un conjunto de datos que se usa para detectar y planear el procesamiento previo necesario. Para obtener instrucciones más detalladas sobre el proceso TDSP, consulte los pasos que se describen en el [proceso de ciencia de datos en equipos](overview.md).

Las tareas de procesamiento previo y limpieza, como la tarea de exploración de datos, se pueden llevar a cabo en una amplia variedad de entornos, como SQL o Hive o Azure Machine Learning Studio (clásico), y con diversas herramientas y lenguajes, como R o Python, en función de dónde se almacenen los datos y cómo se le apliquen formato. Como TDSP es iterativo por naturaleza, estas tareas pueden tener lugar en diversos pasos del flujo de trabajo del proceso.

En este artículo se presentan varios conceptos sobre el procesamiento de datos y las tareas que se pueden llevar a cabo antes o después de ingerir datos en Azure Machine Learning Studio (clásico).

Para ver un ejemplo de exploración y preprocesamiento de datos realizado en Azure Machine Learning Studio (clásico), visione el vídeo [Preprocesamiento de datos](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/).

## <a name="why-pre-process-and-clean-data"></a>¿Por qué preprocesar y limpiar datos?
Se recopilan datos del mundo real de varios orígenes y procesos y pueden contener irregularidades o datos dañados que comprometen la calidad del conjunto de datos. Los problemas de calidad de datos más habituales que surgen son:

* **Incompletos**: en los datos no hay atributos o contienen valores que faltan.
* **Con ruido**: los datos contienen registros erróneos o valores atípicos.
* **Incoherentes**: los datos contienen discrepancias o registros en conflicto.

Los datos de calidad son un requisito previo para los modelos predictivos de calidad. Para evitar la "entrada y salida de elementos no utilizados" y mejorar la calidad de los datos y, por tanto, el rendimiento del modelo, es fundamental llevar a cabo una pantalla de mantenimiento de datos para detectar problemas de datos al principio y decidir acerca de los pasos de limpieza y preprocesamiento de datos correspondientes.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>¿Cuáles son algunas pantallas de mantenimiento de datos más habituales que se emplean?
Podemos comprobar la calidad general de los datos comprobando:

* El número de **registros**.
* El número de **atributos** (o **características**).
* Los **tipos de datos** de atributo (nominales, ordinales o continuos).
* El número de **valores que faltan**.
* Datos **con formato correcto**.
  * Si los datos se encuentran en TSV o CSV, comprueba que los separadores de columnas y los separadores de líneas siempre separen correctamente líneas y columnas.
  * Si los datos están en formato HTML o XML, compruebe si los datos tienen el formato correcto basándose en sus estándares respectivos.
  * El análisis también puede ser necesario para extraer información estructurada de datos semiestructurados o no estructurados.
* **Registros de datos incoherentes**. Compruebe el intervalo de valores permitidos, por ejemplo. Por ejemplo, si los datos contienen GPA (promedio de calificaciones) de estudiantes, compruebe si el GPA está en el intervalo designado, por ejemplo, 0~4.

Cuando encuentre problemas con los datos, serán necesarios los **pasos de procesamiento** que implican a menudo limpieza de los valores que faltan, normalización de datos, discretización, proceso para quitar o reemplazar caracteres incrustados que puedan afectar a la alineación de datos y tipos de datos mixtos en campos comunes, entre otros.

**Azure Machine Learning consume datos tabulares con formato correcto**.  Si los datos ya están en formato tabular, el preprocesamiento de los datos se puede realizar directamente con Azure Machine Learning Studio (clásico) en Machine Learning.  Si los datos no están en formato tabular, por ejemplo, XML, se puede requerir el análisis para convertir los datos en formato tabular.  

## <a name="what-are-some-of-the-major-tasks-in-data-pre-processing"></a>¿Cuáles son algunas de las tareas principales de preprocesamiento de datos?
* **Limpieza de datos**:  rellene los valores que faltan, detecte y quite los valores atípicos y los datos con ruido.
* **Transformación de datos**:  normalice los datos para reducir el ruido y las dimensiones.
* **Reducción de datos**:  registros de datos de ejemplo o atributos para un control de datos más sencillo.
* **Discretización de datos**:  convierta atributos continuos en atributos de categorías para facilitar su uso con determinados métodos de aprendizaje automático.
* **Limpieza de texto**: quite caracteres incrustados que puedan ocasionar errores en la alineación de los datos, por ejemplo, pestañas incrustadas en un archivo de datos separado por tabulaciones o nuevas líneas incrustadas que pueden dividirse en registros.

En las secciones siguientes se detallan algunos de los pasos de procesamiento de datos.

## <a name="how-to-deal-with-missing-values"></a>¿Cómo tratar los valores que faltan?
Para tratar los valores que faltan, es mejor identificar el motivo por el que faltan los valores para controlar mejor el problema. Los métodos de control de valores que faltan típicos son:

* **Eliminación**: quite los registros con los valores que faltan
* **Sustitución ficticia**: reemplace los valores que faltan por un valor ficticio; por ejemplo, *desconocido* para categorías o 0 para valores numéricos.
* **Sustitución media**: si los datos que faltan son numéricos, reemplace los valores que faltan por la media.
* **Sustitución frecuente**: si los datos que faltan son de categoría, cambie los valores que faltan por el elemento más frecuente
* **Sustitución de regresión**: utilice un método de regresión para reemplazar los valores que faltan por valores con regresión.  

## <a name="how-to-normalize-data"></a>¿Cómo normalizar datos?
La normalización de datos escala los valores numéricos a un intervalo especificado. Entre los métodos de normalización de datos más conocidos se incluyen:

* **Normalización mínimo-máximo**: transforme linealmente los datos a un intervalo, por ejemplo, entre 0 y 1, donde el valor mínimo se escala a 0 y el máximo a 1.
* **Normalización de puntuación Z**: escale los datos en función de la desviación estándar y media: divida la diferencia entre los datos y la media por la desviación estándar.
* **Escalado decimal**: escale los datos moviendo la coma decimal del valor del atributo.  

## <a name="how-to-discretize-data"></a>¿Cómo discretizar los datos?
Los datos se pueden discretizar mediante la conversión de valores continuos en intervalos o atributos nominales. Algunas formas de hacerlo son las siguientes:

* **Discretización del mismo ancho**: divida el intervalo de todos los valores posibles de un atributo en N grupos del mismo tamaño y asigne los valores que se encuentran en una ubicación con el número de ubicación.
* **Discretización del mismo alto**: divida el intervalo de todos los valores posibles de un atributo en N grupos, cada uno de los cuales contiene el mismo número de instancias y, a continuación, asigne los valores que se encuentran en una ubicación con el número de ubicación.  

## <a name="how-to-reduce-data"></a>¿Cómo reducir los datos?
Existen varios métodos para reducir el tamaño de los datos para un tratamiento más sencillo de los datos. Según el tamaño de los datos y el dominio, se pueden aplicar los siguientes métodos:

* **Muestreo de registros**: realice un muestreo de los registros de datos y elija únicamente el subconjunto de datos representativo.
* **Muestreo de atributos**: seleccione solo un subconjunto de los atributos más importantes de los datos.  
* **Agregación**: divida los datos en grupos y almacene los valores de cada grupo. Por ejemplo, los números de los ingresos diarios de una cadena de restaurantes durante los últimos 20 años se pueden agregar para ingresos mensuales con el fin de reducir el tamaño de los datos.  

## <a name="how-to-clean-text-data"></a>¿Cómo limpiar los datos de texto?
Los **campos de texto en datos tabulares** pueden incluir caracteres que afectan a los límites de registros o alineación de columnas. Por ejemplo, las pestañas incrustadas en un archivo separado por tabulaciones causan un error de alineación de columnas y los caracteres de nueva línea incrustados dividen las líneas de registro. El control de codificación de texto incorrecto al escribir o leer texto lleva a la pérdida de información o a la introducción involuntaria de caracteres ilegibles (como valores NULL), y puede afectar también al análisis del texto. La edición y el análisis cuidadoso pueden ser necesarios para limpiar campos de texto para la alineación correcta o extraer datos estructurados de los datos de texto semiestructurados o no estructurados.

**exploración de los datos** ofrece una vista anticipada de los datos. Se pueden detectar una serie de problemas de datos durante este paso y se pueden aplicar los métodos correspondientes para solucionarlos.  Es importante formular preguntas como ¿cuál es la causa del problema y cómo se ha podido producir el problema? Este proceso también le ayuda a decidir acerca de los pasos de procesamiento de datos que deben llevarse a cabo para resolverlos. La identificación de los casos de uso finales y los roles también se puede usar para priorizar el esfuerzo de procesamiento de datos.

## <a name="references"></a>Referencias
> *Minería de datos: conceptos y técnicas*, Tercera edición, Morgan Kaufmann, 2011, Jiawei Han, Micheline Kamber y Jian Pei
> 
> 

