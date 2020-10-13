---
title: Optimización de cargas de trabajo de Azure mediante la puntuación de Advisor
description: Use la puntuación de Advisor para sacar el máximo partido de Azure
ms.topic: article
ms.date: 09/09/2020
ms.openlocfilehash: 29d8480f501a78c1668b52034f439f998419f9d9
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91335626"
---
# <a name="optimize-azure-workloads-using-advisor-score"></a>Optimización de cargas de trabajo de Azure mediante la puntuación de Advisor

## <a name="introduction-to-advisor-score"></a>Introducción a la puntuación de Advisor

Azure Advisor proporciona recomendaciones de procedimientos recomendados para las cargas de trabajo. Estas recomendaciones son personalizadas y accionables para ayudarle a:
* Mejorar la postura de sus cargas de trabajo y optimizar sus implementaciones de Azure
* Evitar proactivamente los principales problemas siguiendo los procedimientos recomendados
* Evaluar las cargas de trabajo de Azure según los cinco pilares del [marco de buena arquitectura de Microsoft Azure](https://docs.microsoft.com/azure/architecture/framework/).

Como característica principal de Advisor, la **puntuación de Advisor** está diseñada para ayudarle a lograr estos objetivos de manera eficaz y eficiente. 

Para sacar el máximo partido a Azure, es fundamental saber dónde se encuentra en el recorrido de optimización de la carga de trabajo, qué servicios o recursos se consumen correctamente y cuáles no. Además, necesitará saber cómo clasificar por orden de prioridad las acciones, en función de las recomendaciones, para maximizar el resultado. También es importante realizar un seguimiento de su progreso y que está realizando en este recorrido de optimización, así como presentar informes al respecto. Con la **puntuación de Advisor**, puede realizar estas acciones fácilmente con la nueva experiencia de ludificación. Como consultor personalizado en la nube, Azure Advisor evalúa continuamente la telemetría de uso y la configuración de recursos para comprobar que se siguen los procedimientos recomendados del sector. Después, Advisor agrega los hallazgos en una sola puntuación para informarle, de un vistazo, si está llevando a cabo los pasos necesarios para crear soluciones confiables, seguras y rentables. La puntuación de Advisor consta de una puntuación total, que se puede desglosar aún más en cinco puntuaciones de categoría, una para cada categoría de Azure Advisor que representa a los cinco pilares del marco de buena arquitectura. Puede realizar un seguimiento del progreso que realiza a lo largo del tiempo al visualizar su puntuación general y por categoría con una tendencia diaria, semanal y mensual; además, puede establecer pruebas comparativas que le ayuden a alcanzar sus objetivos. 

 ![Experiencia de puntuación de Advisor](./media/advisor-score-1.png)

## <a name="how-to-consume-advisor-score"></a>Cómo consumir la puntuación de Advisor
Advisor muestra la puntuación general de Advisor y el desglose de las categorías de Advisor, en porcentajes. Una puntuación del 100 % en cualquier categoría significa que todos los recursos que evaluó Advisor siguen los procedimientos recomendados de Advisor. En el otro extremo del espectro, una puntuación de 0 % significa que ninguno de los recursos que evaluó Advisor sigue las recomendaciones de Advisor. Con estos intervalos de puntuación puede lograr fácilmente los flujos siguientes:
* **Puntuación de Advisor** para ayudarle a determinar la línea de base de la situación de su carga de trabajo o suscripciones según la puntuación de Advisor. También puede ver las tendencias históricas para comprender cuál es la suya.
* Las **puntuaciones de la categoría de Advisor** de cada recomendación le indicarán qué recomendaciones destacadas mejorarán su puntuación. Estos valores reflejan el peso de la recomendación y nuestra predicción de la facilidad de implementación para que pueda obtener el máximo provecho de su tiempo y ayudarle a establecer las prioridades
* **Impacto en la puntuación de la categoría** de cada recomendación para ayudarle a clasificar por orden de prioridad las acciones de corrección de cada categoría.

La contribución de cada recomendación a su puntuación de la categoría se muestra claramente en la página de puntuación de Advisor de Azure Portal. Puede aumentar cada puntuación de categoría el porcentaje que aparece en la columna de impacto en la puntuación de la categoría. Este valor refleja el peso de la recomendación dentro de la categoría y la predicción de la facilidad de implementación para abordar las tareas más sencillas. Centrarse en las recomendaciones con el mayor impacto en la puntuación le ayudará a mejorar el progreso con el tiempo.  

![Impacto de la puntuación de Advisor](./media/advisor-score-2.png)

Si alguna de las recomendaciones de Advisor no son aplicables a un recurso individual, puede posponer o descartar esas recomendaciones y se excluirán del cálculo de la puntuación en la siguiente actualización. Advisor también usará esta entrada como comentario adicional para mejorar el modelo.

## <a name="how-is-advisor-score-calculated"></a>¿Cómo se calcula la puntuación de Advisor?
Advisor muestra las puntuaciones de categoría y la puntuación general de Advisor como porcentajes. Una puntuación del 100 % en cualquier categoría significa que todos los recursos *que evaluó Advisor* siguen los procedimientos recomendados de Advisor. En el otro extremo del espectro, una puntuación de 0 % significa que ninguno de los recursos que evaluó Advisor sigue las recomendaciones de Advisor. 
**Cada una de las cinco categorías tiene una puntuación potencial máxima de 100.** La puntuación general de Advisor se calcula como la suma de cada puntuación de categoría aplicable, dividida por la suma de la puntuación potencial máxima de todas las categorías aplicables. Para la mayoría de las suscripciones, esto significa que Advisor suma la puntuación de cada categoría y la divide por 500. No obstante, **cada puntuación de categoría se calcula solo si se usan los recursos que evalúa Advisor.**

**Ejemplo de cálculo de la puntuación de Advisor**
* Puntuación de una sola suscripción: Es la media simple de todas las puntuaciones de la categoría de Advisor para su suscripción. Si las puntuaciones de la categoría de Advisor son Costo = 73, Confiabilidad = 85, Excelencia operativa = 77, Rendimiento = 100, la **puntuación de Advisor** sería (73 + 85 + 77 + 100)/(4x100) = 0,84 o 84 %.
* Puntuación de varias suscripciones: Cuando se seleccionan varias suscripciones, la puntuación general de Advisor generada son las puntuaciones de la categoría agregadas ponderadas. Aquí, cada puntuación de la categoría de Advisor se agrega en función de los recursos consumidos por las suscripciones. Una vez que tenemos las puntuaciones de la categoría agregadas ponderadas, calculamos una media simple para proporcionar una puntuación total para las suscripciones. 


### <a name="scoring-methodology"></a>Metodología de puntuación: 
El cálculo de la puntuación de Advisor se puede resumir en cuatro pasos:
1. Advisor calcula el **costo minorista de los recursos afectados**, que son los recursos de las suscripciones que tienen al menos una recomendación en Advisor.
2. Advisor calcula el **costo minorista de los recursos evaluados**, que son los recursos que Advisor supervisa, independientemente de si tienen una recomendación o no. 
3. Para cada tipo de recomendación, Advisor calcula la **proporción de recursos correctos**, que es el costo minorista de los recursos afectados dividido por el costo minorista de los recursos evaluados.
4. Advisor aplica tres pesos adicionales a la proporción de recursos correctos en cada categoría:
  * Las recomendaciones con mayor impacto se ponderan con un peso mayor que las que tienen un impacto menor.
  * Los recursos con recomendaciones duraderas contarán más en la puntuación.
  * Los recursos que se posponen o descartan en Advisor se quitan completamente del cálculo de la puntuación. 
    
Advisor aplica este modelo en el nivel de categoría de Advisor (la seguridad usa un modelo de [puntuación segura](https://docs.microsoft.com/azure/security-center/secure-score-security-controls#introduction-to-secure-score)), lo que proporciona una puntuación de Advisor para cada categoría y, después, un promedio sencillo produce la puntuación final de Advisor.


## <a name="advisor-score-faq"></a>Preguntas frecuentes de la puntuación de Advisor
* **¿Con qué frecuencia se actualiza la puntuación?**
La puntuación se actualiza al menos una vez al día. 
* **¿Por qué algunas recomendaciones tienen un valor "-" vacío en la columna de impacto en la puntuación de la categoría?** Advisor no incluye inmediatamente las nuevas recomendaciones o las que tienen cambios recientes en el modelo de puntuación. Tras un breve período de evaluación, normalmente algunas semanas, se incluirán en la puntuación. 
* **¿Por qué el impacto de la puntuación de los costos es mayor en algunas recomendaciones, aunque su potencial de ahorro sea reducido?**
La puntuación de los costos refleja el posible ahorro de los recursos infrautilizados y la previsión de la facilidad de implementación de esas recomendaciones. Por ejemplo, se aplica un peso adicional a los recursos afectados que han estado inactivos durante más tiempo, aunque el ahorro potencial sea menor. 
* **¿Por qué no tengo una puntuación para una o varias categorías o suscripciones?**
Advisor generará una puntuación solo para las categorías y suscripciones que tengan recursos evaluados por Advisor.
* **¿Qué ocurre si una recomendación no es relevante?**
Si descarta una recomendación de Advisor, se omitirá en el cálculo de la puntuación. Las recomendaciones descartadas también ayudan a que Advisor mejore la calidad de las recomendaciones.
* **¿Por qué cambió mi puntuación?** La puntuación puede cambiar si se corrigen los recursos afectados mediante la adopción de los procedimientos recomendados de Advisor. Si alguna persona con permisos en la suscripción modificó o creó recursos nuevos, también podría notar fluctuaciones en la puntuación, ya que está basada en una proporción entre los recursos afectados por costos y el costo total de todos los recursos.
* **¿Cómo calcula Advisor el costo minorista de los recursos de una suscripción?**
Advisor usa las tarifas de pago por uso publicadas en la página de precios de Azure.com, que no refleja ningún descuento aplicable, multiplicadas por la cantidad de uso en el último día en que se asignó el recurso. La omisión de los descuentos en el cálculo del costo de los recursos hace que la puntuación de Advisor sea similar en todas las suscripciones, inquilinos e inscripciones en las que los descuentos pueden variar. 
* **¿Es necesario ver las recomendaciones de Advisor para obtener puntos para la puntuación?** No. La puntuación refleja si adopta los procedimientos recomendados por Advisor, aunque nunca vea esas recomendaciones en Advisor, y si adopta los procedimientos recomendados de manera activa.
* **¿La metodología de puntuación distingue entre cargas de trabajo de producción /desarrollo y de pruebas?**
No, por ahora no, pero puede descartar las recomendaciones en recursos individuales si dichos recursos se usan para desarrollo y pruebas, y la recomendación no se aplicará.
* **¿Puedo comparar puntuaciones entre una suscripción con 100 recursos y una suscripción con 100 000 recursos?**
La metodología de puntuación está diseñada para controlar el número de recursos de una combinación de suscripción y servicio, por lo que las suscripciones con menos recursos pueden tener puntuaciones mayores o menores que las suscripciones con más recursos. 
* **¿Qué significa "próximamente" en la columna de impacto en la puntuación?**
Esto significa que se trata de una nueva recomendación y que todavía estamos trabajando para incluirla en nuestro modelo de puntuación de Advisor. Una vez que se incluya esta nueva recomendación en el cálculo de la puntuación, verá el valor del impacto en la puntuación de su recomendación.  
* **¿Mi puntuación depende de cuánto gasto en Azure?**
No, la puntuación no es necesariamente un reflejo de cuánto gasta, y los gastos innecesarios reducirán la puntuación del costo.

## <a name="how-to-access-advisor"></a>Cómo acceder a Advisor
La puntuación de Advisor se encuentra en versión preliminar pública en Azure Portal. Tiene que ir a la sección Advisor y la puntuación de Advisor es el segundo elemento del menú en el panel de navegación izquierdo. 

![Punto de entrada de la puntuación de Advisor](./media/advisor-score-3.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las recomendaciones de Advisor, consulte:
* [Introducción a Advisor](advisor-overview.md)
* [Introducción a Advisor](advisor-get-started.md)
* [Recomendaciones sobre el costo de Advisor](advisor-cost-recommendations.md)
* [Recomendaciones sobre rendimiento de Advisor](advisor-performance-recommendations.md)
* [Recomendaciones sobre seguridad de Advisor](advisor-security-recommendations.md)
* [Recomendaciones de excelencia operativa de Advisor](advisor-operational-excellence-recommendations.md)
