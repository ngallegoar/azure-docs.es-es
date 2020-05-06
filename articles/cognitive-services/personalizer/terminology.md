---
title: 'Terminología: Personalizer'
description: Personalizer utiliza la terminología del aprendizaje de refuerzo. Estos términos se usan en Azure Portal y las API.
ms.topic: conceptual
ms.date: 02/18/2020
ms.openlocfilehash: f75437c5afd5d3fd7f7570079be410d3db1ca8db
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "77624202"
---
# <a name="terminology"></a>Terminología

Personalizer utiliza la terminología del aprendizaje de refuerzo. Estos términos se usan en Azure Portal y las API.

## <a name="conceptual-terminology"></a>Terminología conceptual

* **Ruta de aprendizaje**: puede crear un recurso de Personalizer, denominado _bucle de aprendizaje_, para todas las partes de la aplicación que pueden beneficiarse de la personalización. Si tiene varias experiencias de personalización, cree un bucle para cada una de ellas.

* **Modelo**: los modelos de Personalizer capturan todos los datos aprendidos acerca del comportamiento del usuario y obtiene los datos de entrenamiento de la combinación de los argumentos que se envían a las llamadas a Rank y a Reward, y con un comportamiento de entrenamiento determinado por la directiva de aprendizaje.

## <a name="personalizer-configuration"></a>Configuración de Personalizer

Personalizer se configura desde [Azure Portal](https://portal.azure.com).

* **Recompensas**: configure los valores predeterminados para el tiempo de espera de las recompensas, la recompensa predeterminada y la directiva de agregación de recompensa.

* **Exploración**: configure el porcentaje de llamadas a Rank que se usarán para la exploración.

* **Frecuencia de actualización del modelo**: frecuencia con la que se vuelve a entrenar el modelo.

* **Retención de datos**: número de días que merece la pena almacenar los datos. Esto puede afectar a las evaluaciones sin conexión, que se usan para mejorar el bucle de aprendizaje.

## <a name="use-rank-and-reward-apis"></a>Uso de las API Rank y Reward

* **Rank**: Dadas las acciones con características y las características del contexto, use la exploración o la vulnerabilidad de seguridad para devolver la acción superior (elemento de contenido).

    * **Acciones**: las acciones son los elementos de contenido, como productos o promociones, entre los que se eligen. Personalizer elige la acción superior (que devolvió el id. de acción de Reward) para mostrar a los usuarios mediante la API Rank.

    * **Context**: para proporcionar una clasificación más precisa, especifique información acerca del contexto, como por ejemplo:
        * Su usuario.
        * El dispositivo en el que están.
        * La hora actual.
        * Otros datos acerca d la situación actual.
        * Datos históricos acerca del usuario o contexto.

        Su aplicación específica puede tener otra información de contexto.

    * **[Características](concepts-features.md)** : una unidad de información acerca de un elemento de contenido o un contexto de usuario. Asegúrese de usar solo las características que estén agregadas. No use horas específicas, id. de usuario u otros datos no agregados como características.

        * Una _característica de acción_ son los metadatos sobre el contenido.
        * Una _característica de contexto_ son los metadatos sobre el contexto en el que se presenta el contenido.

* **Exploración**: el servicio Personalizer está explorando cuando, en lugar de devolver la mejor acción, elige otra acción para el usuario. El servicio Personalizer evita el desfase, estancamiento y puede adaptarse al comportamiento del usuario en curso mediante la exploración.

* **Explotación**: el servicio de Personalizer usa el modelo actual para decidir la mejor acción según los datos anteriores.

* **Duración del experimento**: La cantidad de tiempo que el servicio Personalizer espera una recompensa, desde el momento en que se realizó la llamada de Rank en el evento.

* **Eventos inactivos**: Un evento inactivo es aquel en el que llamó a Rank, pero no está seguro de que el usuario vea alguna vez el resultado, debido a decisiones de la aplicación cliente. Los eventos inactivos le permiten crear y almacenar resultados de la personalización y, después, decidir descartarlos más adelante sin que ello afecte al modelo de Machine Learning.


* **Recompensa**: una medida de la forma en que el usuario respondió al id. de acción de Reward devuelta por API Rank, en forma de puntuación entre 0 y 1. El valor de 0 a 1 lo establece la lógica de negocios, en función de la forma en que la elección ha ayudado a lograr los objetivos empresariales de personalización. El bucle de aprendizaje no almacena esta recompensa como historial de usuario individual.

## <a name="offline-evaluations"></a>Evaluaciones sin conexión

* **Evaluación**: una evaluación sin conexión determina la mejor directiva de aprendizaje para el bucle en función de los datos del bucle.

* **Directiva de aprendizaje**: la manera en que Personalizer entrena un modelo en cada evento la determinarán algunos parámetros que afectan el funcionamiento de los algoritmos del aprendizaje automático. Un nuevo bucle de aprendizaje se iniciará con una **directiva de aprendizaje** predeterminada, que puede generar un rendimiento moderado. Cuando ejecuta las [evaluaciones](concepts-offline-evaluation.md), Personalizer crea directivas de aprendizaje nuevas específicamente optimizadas para los casos de uso del bucle. Personalizer tendrá un rendimiento mucho mejor con directivas optimizadas para cada bucle específico, generado durante la evaluación. La directiva de aprendizaje se denomina _configuración de aprendizaje_ en la **configuración de modelo y aprendizaje** del recurso de Personalizer en Azure Portal.