---
title: 'Tutorial: Análisis de sentimiento con Cognitive Services'
description: Tutorial sobre cómo aprovechar Cognitive Services para el análisis de sentimiento en Synapse
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: jrasnick, garye
ms.date: 11/20/2020
author: nelgson
ms.author: negust
ms.openlocfilehash: 1b407cbee5218149f794ab125ac058e32b422558
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96465221"
---
# <a name="tutorial-sentiment-analysis-with-cognitive-services-preview"></a>Tutorial: Análisis de sentimiento con Cognitive Services (versión preliminar)

En este tutorial, aprenderá a enriquecer fácilmente los datos en Azure Synapse con [Cognitive Services](https://go.microsoft.com/fwlink/?linkid=2147492). Vamos a usar las funcionalidades de [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) para realizar análisis de sentimiento. Un usuario de Azure Synapse puede simplemente seleccionar una tabla que contenga una columna de texto para enriquecerla con opiniones. Estas opiniones pueden ser positivas, negativas, mixtas o neutras, y también se devolverá una probabilidad.

Esta tutorial abarca lo siguiente:

> [!div class="checklist"]
> - Pasos para obtener un conjunto de datos de la tabla de Spark que contiene la columna de texto para el análisis de sentimiento.
> - Uso de la experiencia del asistente en Azure Synapse para enriquecer los datos mediante Text Analytics de Cognitive Services.

Si no tiene una suscripción a Azure, [cree una cuenta gratuita antes de empezar](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Requisitos previos

- Necesitará un [área de trabajo de Synapse Analytics](../get-started-create-workspace.md) con una cuenta de almacenamiento de ADLS Gen2 configurada como almacenamiento predeterminado. Debe ser el **Colaborador de datos de blobs de almacenamiento** del sistema de archivos de ADLS Gen2 con el que quiere trabajar.
- Un grupo de Spark en el área de trabajo de Azure Synapse Analytics. Para más información, consulte [Creación de un grupo de SQL dedicado mediante Synapse Studio](../quickstart-create-sql-pool-studio.md).
- Para poder utilizar este tutorial, también tiene que completar los pasos anteriores a la configuración que se describen en él. [Requisitos previos para el uso de Cognitive Services en Azure Synapse](tutorial-configure-cognitive-services-synapse.md).

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Portal de Azure](https://portal.azure.com/)

## <a name="create-a-spark-table"></a>Creación de una tabla de Spark

Necesitará una tabla de Spark para este tutorial.

1. Descargue el siguiente archivo CSV que contiene un conjunto de datos para el análisis de texto: [FabrikamComments.csv](https://github.com/Kaiqb/KaiqbRepo0731190208/blob/master/CognitiveServices/TextAnalytics/FabrikamComments.csv)

1. Cargue el archivo en la cuenta de almacenamiento de ADLSGen2 de Azure Synapse.
![Carga de datos](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00a.png)

1. Cree una tabla de Spark a partir del archivo .csv; para ello, haga clic con el botón derecho en el archivo y seleccione **Nuevo cuaderno -> Crear tabla de Spark**.
![Crear una tabla de Spark](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00b.png)

1. Asigne un nombre a la tabla en la celda de código y ejecute el cuaderno en un grupo de Spark. Recuerde establecer "header = True".
![Ejecución del cuaderno](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00c.png)

```python
%%pyspark
df = spark.read.load('abfss://default@azuresynapsesa.dfs.core.windows.net/data/FabrikamComments.csv', format='csv'
## If header exists uncomment line below
, header=True
)
df.write.mode("overwrite").saveAsTable("default.YourTableName")
```

## <a name="launch-cognitive-services-wizard"></a>Inicio del Asistente para Cognitive Services

1. Haga clic con el botón derecho en la tabla de Spark creada en el paso anterior. Seleccione "Machine Learning -> Enrich with existing model" (Enriquecer con modelo existente) para abrir el asistente.
![Iniciar Asistente para puntuación](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00d.png)

2. Aparecerá un panel de configuración, tras lo que se le pedirá que seleccione un modelo de Cognitive Services. Seleccione Text Analytics: Análisis de sentimiento.

![Iniciar Asistente para puntuación: paso 1](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00e.png)

## <a name="provide-authentication-details"></a>Proporcionar detalles de autenticación

Para autenticarse en Cognitive Services, tiene que hacer referencia al secreto que va a utilizar en Key Vault. Las siguientes entradas dependen de los [pasos de requisitos previos](tutorial-configure-cognitive-services-synapse.md) que tiene que completar antes de este paso.

- **Suscripción de Azure**: seleccione la suscripción de Azure a la que pertenece el almacén de claves.
- **Cuenta de Cognitive Services**: este es el recurso de Text Analytics al que se va a conectar.
- **Servicio vinculado de Azure Key Vault**: como parte de los pasos de requisitos previos, ha creado un servicio vinculado a su recurso de Text Analytics. Selecciónelo aquí.
- **Nombre del secreto**: este es el nombre del secreto en el almacén de claves que contiene la clave para autenticarse en el recurso de Cognitive Services.

![Proporcionar detalles de Azure Key Vault](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00f.png)

## <a name="configure-sentiment-analysis"></a>Configuración del análisis de sentimiento

A continuación, debe configurar el análisis de sentimiento. Seleccione los siguientes detalles:
- **Idioma**: seleccione el idioma del texto en el que desea realizar el análisis de sentimiento. Seleccione **EN**.
- **Columna de texto**: esta es la columna de texto del conjunto de datos que desea analizar para determinar la opinión. Seleccione la columna **comment** (comentario) de la tabla.

Cuando haya terminado, haga clic en **Abrir cuaderno**. Esto generará un cuaderno con el código de PySpark que realiza el análisis de sentimiento con Azure Cognitive Services.

![Configuración del análisis de sentimiento](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00g.png)

## <a name="open-notebook-and-run"></a>Apertura y ejecución del cuaderno

El cuaderno que acaba de abrir utiliza la [biblioteca mmlspark](https://github.com/Azure/mmlspark) para conectarse a Cognitive Services.

Los detalles de Azure Key Vault que ha proporcionado le permiten hacer referencia de forma segura a sus secretos desde esta experiencia sin revelarlos.

Ahora puede **Ejecutar todas** las celdas para enriquecer los datos con opiniones. Las opiniones se devolverán como positivas, negativas, neutras o mixtas, y también obtendrá las probabilidades de cada opinión. Para más información, consulte [Procedimiento: análisis de sentimiento y minería de opiniones](https://go.microsoft.com/fwlink/?linkid=2147792).

![Ejecución del análisis de sentimiento](media/tutorial-cognitive-services/tutorial-cognitive-services-sentiment-00h.png)

## <a name="next-steps"></a>Pasos siguientes
- [Tutorial: Detección de anomalías con Azure Cognitive Services](tutorial-cognitive-services-sentiment.md)
- [Tutorial: Asistente para puntuación del modelo de Machine Learning para grupos de SQL dedicados](tutorial-sql-pool-model-scoring-wizard.md)
- [Funcionalidades de aprendizaje automático en Azure Synapse Analytics](what-is-machine-learning.md)