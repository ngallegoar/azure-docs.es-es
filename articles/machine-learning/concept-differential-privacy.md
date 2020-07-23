---
title: Implementación de la privacidad diferencial con el paquete de WhiteNoise (versión preliminar)
titleSuffix: Azure Machine Learning
description: Obtenga información sobre qué es la privacidad diferencial y cómo el paquete de WhiteNoise puede ayudarle a implementar sistemas privados diferenciales que conserven la privacidad de los datos.
author: luisquintanilla
ms.author: luquinta
ms.date: 07/09/2020
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.openlocfilehash: 9728bf2c86c0629b09e2325650ce288cf9b3cc7e
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86199790"
---
# <a name="preserve-data-privacy-by-using-differential-privacy-and-the-whitenoise-package-preview"></a>Conservación de la privacidad de los datos con la privacidad diferencial y el paquete de WhiteNoise (versión preliminar)

Obtenga información sobre qué es la privacidad diferencial y cómo el paquete de WhiteNoise puede ayudarle a implementar sistemas privados diferenciales.

A medida que aumenta la cantidad de datos que una organización recopila y usa para los análisis, también aumentan las preocupaciones relacionadas con la privacidad y la seguridad. Los análisis requieren datos. Normalmente, cuanto mayor número de datos se usan para entrenar los modelos, más precisos son. Cuando se usa información personal para estos análisis, es especialmente importante que los datos permanezcan privados en todo el uso.

> [!NOTE]
> Tenga en cuenta que se cambiará el nombre del kit de herramientas y se presentará el nombre nuevo en las próximas semanas. 

## <a name="how-differential-privacy-works"></a>Funcionamiento de la privacidad diferencial

La privacidad diferencial es un conjunto de sistemas y prácticas que ayudan a mantener los datos de los usuarios seguros y privados.

> [!div class="mx-imgBorder"]
> ![Proceso de privacidad diferencial](./media/concept-differential-privacy/differential-privacy-process.jpg)

En los escenarios tradicionales, los datos sin procesar se almacenan en archivos y bases de datos. Cuando los usuarios analizan datos, suelen usar los datos sin procesar. Esto supone un problema, ya que se podría infringir la privacidad de los usuarios. La privacidad diferencial intenta solucionar este problema agregando "ruido" o aleatoriedad a los datos para que los usuarios no puedan identificar puntos de datos individuales. Como mínimo, este tipo de sistema proporciona denegabilidad plausible.

En los sistemas privados diferenciales, los datos se comparten a través de solicitudes denominadas **consultas**. Cuando un usuario envía una consulta de datos, las operaciones conocidas como **mecanismos de privacidad** agregan ruido a los datos solicitados. Los mecanismos de privacidad devuelven una *aproximación de los datos*, en lugar de los datos sin procesar. Este resultado de conservación de la privacidad aparece en un **informe**. Los informes constan de dos partes: los datos reales calculados y una descripción de cómo se crearon los datos.

## <a name="differential-privacy-metrics"></a>Métricas de privacidad diferencial

La privacidad diferencial intenta protegerse frente a la posibilidad de que un usuario pueda producir un número indefinido de informes para revelar datos confidenciales con el tiempo. Un valor conocido como **épsilon** mide el nivel de ruido o privacidad que tiene un informe. Épsilon tiene una relación inversa con el ruido o la privacidad. Cuanto menor es el épsilon, más ruidosos (y privados) son los datos.

Los valores de épsilon no son negativos. Los valores inferiores a 1 proporcionan una denegabilidad plausible completa. Cualquier valor superior a 1 se asocia a un riesgo mayor de exposición de los datos reales. A medida que implementa sistemas privados diferenciales, debería producir informes con valores de épsilon entre 0 y 1.

Otro valor directamente correlacionado con el épsilon es **delta**. Delta mide la probabilidad de que un informe no sea totalmente privado. Cuanto mayor sea delta, mayor será épsilon. Dado que estos valores están correlacionados, épsilon se usa con más frecuencia.

## <a name="privacy-budget"></a>Presupuesto de privacidad

Para garantizar la privacidad en sistemas en los que se permiten varias consultas, la privacidad diferencial define un límite de frecuencia. Este límite se conoce como **presupuesto de privacidad**. A los presupuestos de privacidad se les asigna una cantidad de épsilon, normalmente entre 1 y 3, para limitar el riesgo de reidentificación. A medida que se generan informes, los presupuestos de privacidad realizan un seguimiento del valor de épsilon de informes individuales, así como del agregado para todos los informes. Una vez que se ha gastado o agotado un presupuesto de privacidad, los usuarios ya no pueden tener acceso a los datos.  

## <a name="reliability-of-data"></a>Confiabilidad de los datos

Aunque la conservación de la privacidad debe ser el objetivo, existe una contrapartida en cuanto a la facilidad de uso y la confiabilidad de los datos. En el análisis de datos, la precisión se puede considerar como una medida de incertidumbre introducida por los errores de muestreo. Esta incertidumbre tiende a estar dentro de ciertos límites. La **precisión** desde una perspectiva de privacidad diferencial, en cambio, mide la confiabilidad de los datos, que se ve afectada por la incertidumbre introducida por los mecanismos de privacidad. En resumen, un nivel más alto de ruido o privacidad se traduce en datos con un épsilon, una precisión y una confiabilidad menores. Aunque los datos son más privados, ya que no son confiables, es menos probable que se usen.

## <a name="implementing-differentially-private-systems"></a>Implementación de sistemas privados de forma diferencial

La implementación de sistemas privados de forma diferencial es difícil. WhiteNoise es un proyecto de código abierto que contiene distintos componentes para la creación de sistemas privados globales de forma diferencial. WhiteNoise se compone de los siguientes componentes de nivel superior:

- Core
- Sistema

### <a name="core"></a>Core

La biblioteca principal incluye los siguientes mecanismos de privacidad para la implementación de un sistema privado de forma diferencial:

|Componente  |Descripción  |
|---------|---------|
|Análisis     | Descripción de gráfico de cálculos arbitrarios. |
|Validador     | Biblioteca de Rust que contiene un conjunto de herramientas para comprobar y derivar las condiciones necesarias para que un análisis sea privado de forma diferencial.          |
|Tiempo de ejecución     | Medio en el que se va a ejecutar el análisis. El runtime de referencia se escribe en Rust, pero los runtime se pueden escribir con cualquier marco de cálculo, como SQL y Spark, dependiendo de las necesidades de los datos.        |
|Enlaces     | Enlaces de lenguaje y bibliotecas auxiliares para compilar análisis. Actualmente, WhiteNoise proporciona enlaces de Python. |

### <a name="system"></a>Sistema

La biblioteca del sistema proporciona las siguientes herramientas y servicios para trabajar con datos tabulares y relacionales:

|Componente  |Descripción  |
|---------|---------|
|Acceso a datos     | Biblioteca que intercepta y procesa consultas SQL y genera informes. Esta biblioteca se implementa en Python y admite los siguientes orígenes de datos ODBC y DBAPI:<ul><li>PostgreSQL</li><li>SQL Server</li><li>Spark</li><li>Preston</li><li>Pandas</li></ul>|
|Servicio     | Servicio de ejecución que proporciona un punto de conexión REST para atender solicitudes o consultas en orígenes de datos compartidos. El servicio está diseñado para permitir la composición de módulos de privacidad diferencial que operan en solicitudes que contienen valores delta y épsilon diferentes, también conocidas como solicitudes heterogéneas. Esta implementación de referencia tiene un impacto adicional de las consultas en los datos correlacionados. |
|Evaluador     | Evaluador estocástico que comprueba las infracciones de privacidad, la precisión y el sesgo. El evaluador admite las siguientes pruebas: <ul><li>Prueba de privacidad: determina si un informe cumple las condiciones de privacidad diferencial.</li><li>Prueba de precisión: mide si la confiabilidad de los informes se encuentra dentro de los límites superior e inferior dado un nivel de confianza del 95 %.</li><li>Prueba de utilidad: determina si los límites de confianza de un informe están lo suficientemente cerca de los datos al mismo tiempo que se maximiza la privacidad.</li><li>Prueba de sesgo: mide la distribución de informes para consultas repetidas para asegurarse de que no están desequilibrados.</li></ul> |

## <a name="next-steps"></a>Pasos siguientes

[Mantenga la privacidad de los datos](how-to-differential-privacy.md) en Azure Machine Learning.

Para obtener más información sobre los componentes de WhiteNoise, consulte los repositorios de GitHub para obtener el [paquete base de WhiteNoise](https://github.com/opendifferentialprivacy/whitenoise-core), el [paquete de sistema de WhiteNoise](https://github.com/opendifferentialprivacy/whitenoise-system) y los [ejemplos de WhiteNoise](https://github.com/opendifferentialprivacy/whitenoise-samples).