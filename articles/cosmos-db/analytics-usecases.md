---
title: Casos de uso para análisis integrados con Azure Cosmos DB
description: Aprenda a usar los análisis integrados con Azure Cosmos DB en diferentes casos de uso.
author: markjbrown
ms.author: mjbrown
ms.topic: conceptual
ms.service: cosmos-db
ms.date: 04/20/2020
ms.reviewer: sngun
ms.openlocfilehash: 9b1c3435222ada52c01f21c2c242dc886f566a81
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82192857"
---
# <a name="use-cases-for-built-in-analytics-with-azure-cosmos-db"></a>Casos de uso para análisis integrados con Azure Cosmos DB

Los siguientes casos de uso se pueden realizar utilizando los análisis integrados con Apache Spark en Azure Cosmos DB:

## <a name="htap-scenarios"></a>Escenarios de HTAP

Los análisis integrados en Azure Cosmos DB se pueden usar para:

* Detectar fraudes durante el procesamiento de transacciones.
* Proporcionar recomendaciones a los compradores mientras navegan por una tienda de comercio electrónico.
* Alertar a los operadores sobre un error inminente de una pieza crítica de un equipo industrial.
* Crear información rápida y práctica mediante la inserción de análisis en tiempo real directamente en datos operativos.

Azure Cosmos DB admite escenarios de procesamiento transaccional/analítico híbrido (HTAP) mediante el uso de una instancia de Apache Spark integrada de forma nativa. Azure Cosmos DB elimina la separación operativa y analítica propia de los sistemas tradicionales.

## <a name="globally-distributed-data-lake-without-requiring-any-etl"></a>Lago de datos distribuido globalmente sin necesidad de extracción, transformación y carga de datos

Con una instancia de Apache Spark integrada de forma nativa, Azure Cosmos DB proporciona una manera rápida, sencilla y escalable de crear un lago de datos distribuido globalmente que proporciona una sola imagen de sistema. Los datos multimodelo distribuidos globalmente eliminan la necesidad de invertir en costosas canalizaciones ETL y escalados a petición, lo que revoluciona el modo en que los equipos de datos analizan sus conjuntos de datos.

## <a name="time-series-analytics-over-historic-data"></a>Análisis de series temporales sobre datos históricos

En algunos casos, puede que tenga que responder a preguntas basadas en datos de un momento dado de eventos completados en el pasado. Por ejemplo, para obtener un recuento de los estados de actividades de CRM en una fecha determinada. Si ejecutó el informe hace una semana, el recuento de estados reflejará los estados de cada actividad en ese momento dado. Si ejecuta el mismo informe hoy, obtendrá el recuento de las actividades con los estados de hoy, que pueden haber cambiado desde la última semana, a medida que recorren su ciclo de vida de abierto a cerrado. Por lo tanto, debe realizar un informe de la instantánea en cada fase del ciclo de vida del caso.

Con Azure Cosmos DB, los usuarios tienen la posibilidad de implementar el concepto de viaje en el tiempo, al poder consultar y ejecutar análisis en los datos de manera retrospectiva y consultar cómo eran los datos en un momento dado en la historia. Esto significa que los usuarios pueden ver fácilmente la vista actual y la vista histórica de los datos y ejecutar análisis en ellos.

## <a name="globally-distributed-machine-learning-and-ai"></a>Modelo de aprendizaje automático distribuido globalmente e inteligencia artificial

A medida que las empresas se enfrentan a volúmenes de datos cada vez mayores y a una creciente variedad de tipos de datos y formatos, la capacidad de obtener información más detallada y precisa resulta casi imposible a escala de petabytes a nivel global. Con una instancia de Apache Spark integrada de forma nativa, Azure Cosmos DB proporciona una plataforma de análisis distribuida globalmente que ofrece una amplia biblioteca de algoritmos de aprendizaje automático. Puede usar cuadernos de Jupyter Notebook interactivos para crear y entrenar modelos y agrupar funcionalidades de administración en clústeres. Estas funcionalidades le permiten aprovisionar clústeres de Spark autoelásticos con gran optimización a petición.

## <a name="deep-learning-on-multi-model-globally-distributed-data"></a>Aprendizaje profundo en datos multimodelo distribuidos globalmente

El aprendizaje profundo es la manera ideal de proporcionar soluciones de análisis predictivo de macrodatos a medida que el volumen y la complejidad de los datos continúan creciendo. Con el aprendizaje profundo, las empresas pueden aprovechar la capacidad de los datos no estructurados y semiestructurados para ofrecer casos de uso que aprovechan técnicas como la inteligencia artificial, el procesamiento de lenguaje natural, etc.

## <a name="reporting-integrating-with-power-apps-power-bi"></a>Informes (integración con Power Apps, Power BI)

Power BI proporciona visualizaciones interactivas con funcionalidades de inteligencia empresarial con características de autoservicio, lo que permite a los usuarios finales crear informes y paneles por sí solos. Con el conector de Spark integrado, puede conectar Power BI Desktop a clústeres de Apache Spark en Azure Cosmos DB. Este conector le permite usar una consulta directa para descargar el procesamiento en Apache Spark en Azure Cosmos DB, lo que resulta ideal si tiene una cantidad masiva de datos que no desea cargar en Power BI o si desea realizar análisis casi en tiempo real.

## <a name="iot-analytics-at-global-scale"></a>Análisis de IoT a escala global

Los datos generados a partir de un número cada vez mayor de sensores de red aportan una visibilidad sin precedentes de procesos y sistemas que anteriormente resultaban opacos. La clave es encontrar información útil en este torrente de datos, independientemente de dónde se distribuyan los dispositivos IoT a nivel global. Azure Cosmos DB permite a las compañías de IoT analizar datos de sensores de alta velocidad y datos de series temporales en tiempo real en cualquier parte del mundo. Permite aprovechar el verdadero valor de un mundo interconectado para ofrecer experiencias de cliente mejoradas, eficiencias operativas y nuevas oportunidades de ingresos.

## <a name="stream-processing-and-event-analytics"></a>Procesamiento de flujos y análisis de eventos 

Desde archivos de registro a datos de sensores, las empresas tienen una necesidad creciente de enfrentarse a "flujos" de datos. Estos datos llegan en un flujo estable, a menudo de varios orígenes simultáneamente. Aunque es factible almacenar estos flujos de datos en disco y analizarlos retrospectivamente, a veces puede ser aconsejable o importante procesar los datos y tomar medidas según llegan. Por ejemplo, los flujos de datos relacionados con transacciones financieras se pueden procesar en tiempo real para identificar y rechazar transacciones potencialmente fraudulentas.

## <a name="interactive-analytics"></a>Análisis interactivo

Además de ejecutar consultas predefinidas para crear paneles estáticos para ventas, productividad o precios de acciones, es posible que desee explorar los datos de forma interactiva. Los análisis interactivos le permiten formular preguntas, ver los resultados, modificar la pregunta inicial en función de la respuesta o profundizar en los resultados. Apache Spark en Azure Cosmos DB admite consultas interactivas al responder y adaptarse rápidamente.

## <a name="data-exploration-using-jupyter-notebooks"></a>Exploración de datos con cuadernos de Jupyter Notebook

Si tiene un nuevo conjunto de datos, antes de profundizar en los modelos y las pruebas en ejecución, debe inspeccionar los datos. En otras palabras, debe realizar análisis de datos exploratorios. La exploración de datos puede servir para fundamentar varias decisiones. Por ejemplo, puede encontrar detalles como qué métodos son adecuados para usarse en los datos, si los datos cumplen determinadas suposiciones de modelado, si los datos deben limpiarse, reestructurarse, etc. Mediante el uso de Apache Spark y cuadernos de Jupyter Notebook integrados de forma nativa en Azure Cosmos DB, puede realizar un análisis de datos exploratorio rápido y eficaz en datos transaccionales y analíticos.

## <a name="next-steps"></a>Pasos siguientes

Para empezar a trabajar con estos casos de uso, consulte los siguientes artículos:

* [Cuadernos de Jupyter Notebook integrados en Azure Cosmos DB](cosmosdb-jupyter-notebooks.md)
* [Habilitar cuadernos para cuentas de Azure Cosmos DB](enable-notebooks.md)
* [Creación de un cuaderno para analizar y visualizar datos](create-notebook-visualize-data.md)