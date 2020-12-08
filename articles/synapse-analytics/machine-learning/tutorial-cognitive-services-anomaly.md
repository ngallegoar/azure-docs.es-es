---
title: 'Tutorial: Detección de anomalías con Cognitive Services'
description: Tutorial sobre cómo aprovechar Cognitive Services para la detección de anomalías en Synapse
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: jrasnick, garye
ms.date: 11/20/2020
author: nelgson
ms.author: negust
ms.openlocfilehash: 4052d6a0773aa27e0a378ee04975c7946f1ffbfe
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96464462"
---
# <a name="tutorial-anomaly-detection-with-cognitive-services-preview"></a>Tutorial: Detección de anomalías con Cognitive Services (versión preliminar)

En este tutorial, aprenderá a enriquecer fácilmente los datos en Azure Synapse con [Cognitive Services](https://go.microsoft.com/fwlink/?linkid=2147492). Vamos a utilizar [Anomaly Detector](https://go.microsoft.com/fwlink/?linkid=2147493) para realizar la detección de anomalías. Un usuario de Azure Synapse puede seleccionar simplemente una tabla para enriquecer la detección de anomalías.

Esta tutorial abarca lo siguiente:

> [!div class="checklist"]
> - Pasos para obtener un conjunto de datos de la tabla de Spark que contiene datos de series temporales.
> - Utilice una experiencia del Asistente en Azure Synapse para enriquecer datos mediante Anomaly Detector de Cognitive Services.

Si no tiene una suscripción a Azure, [cree una cuenta gratuita antes de empezar](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Requisitos previos

- Necesitará un [área de trabajo de Synapse Analytics](../get-started-create-workspace.md) con una cuenta de almacenamiento de ADLS Gen2 configurada como almacenamiento predeterminado. Debe ser el **Colaborador de datos de blobs de almacenamiento** del sistema de archivos de ADLS Gen2 con el que quiere trabajar.
- Grupo de Spark en el área de trabajo de Azure Synapse Analytics. Para más información, consulte el artículo sobre [creación de un grupo de Spark en Azure Synapse](../quickstart-create-sql-pool-studio.md).
- Para poder utilizar este tutorial, también tiene que completar los pasos anteriores a la configuración que se describen en él. [Configuración de Cognitive Services en Azure Synapse](tutorial-configure-cognitive-services-synapse.md).

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Portal de Azure](https://portal.azure.com/)

## <a name="create-a-spark-table"></a>Creación de una tabla de Spark

Necesitará una tabla de Spark para este tutorial.

1. Descargue el siguiente archivo del cuaderno que contiene el código para generar una tabla de Spark: [prepare_anomaly_detector_data.ipynb](https://go.microsoft.com/fwlink/?linkid=2149577)

1. Cargue el archivo en el área de trabajo de Azure Synapse.
![Carga de cuaderno](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00a.png)

1. Abra el archivo del cuaderno y elija **Ejecutar todas** las celdas.
![Crear una tabla de Spark](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00b.png)

1. Ahora debe aparecer una tabla de Spark denominada **anomaly_detector_testing_data** en la base de datos de Spark predeterminada.

## <a name="launch-cognitive-services-wizard"></a>Inicio del Asistente para Cognitive Services

1. Haga clic con el botón derecho en la tabla de Spark creada en el paso anterior. Seleccione "Machine Learning -> Enrich with existing model" (Enriquecer con modelo existente) para abrir el asistente.
![Iniciar Asistente para puntuación](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00g.png)

2. Aparecerá un panel de configuración, tras lo que se le pedirá que seleccione un modelo de Cognitive Services. Seleccione Anomaly Detector.

![Iniciar Asistente para puntuación: paso 1](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00c.png)

## <a name="provide-authentication-details"></a>Proporcionar detalles de autenticación

Para autenticarse en Cognitive Services, tiene que hacer referencia al secreto que va a utilizar en Key Vault. Las siguientes entradas dependen de los [pasos de requisitos previos](tutorial-configure-cognitive-services-synapse.md) que tiene que completar antes de este paso.

- **Suscripción de Azure**: seleccione la suscripción de Azure a la que pertenece el almacén de claves.
- **Cuenta de Cognitive Services**: este es el recurso de Text Analytics al que se va a conectar.
- **Servicio vinculado de Azure Key Vault**: como parte de los pasos de requisitos previos, ha creado un servicio vinculado a su recurso de Text Analytics. Selecciónelo aquí.
- **Nombre del secreto**: este es el nombre del secreto en el almacén de claves que contiene la clave para autenticarse en el recurso de Cognitive Services.

![Proporcionar detalles de Azure Key Vault](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00d.png)

## <a name="configure-anomaly-detection"></a>Configuración de la detección de anomalías

A continuación, debe configurar la detección de anomalías. Proporcione los detalles siguientes:

- Granularidad: velocidad a la que se muestrean los datos. Por ejemplo, si los datos tienen un valor para cada minuto, su granularidad sería cada minuto. Elija **mensualmente**. 

- Marca de tiempo: columna que representa la hora y fecha de la serie. Elija la columna **marca de tiempo**.

- Valor de serie temporal: columna que representa el valor de la serie en el momento especificado por la columna Marca de tiempo. Elija la columna **valor**.

- Agrupación: columna que agrupa la serie. Es decir, todas las filas que tienen el mismo valor en esta columna deben formar una serie temporal. Elija la columna **grupo**.

Cuando haya terminado, seleccione **Abrir Notebook**. Esto generará un cuaderno con el código PySpark que realiza la detección de anomalías mediante Azure Cognitive Services.

![Configuración de Anomaly Detector](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00e.png)

## <a name="open-notebook-and-run"></a>Apertura del cuaderno y ejecución

El cuaderno que acaba de abrir utiliza la [biblioteca mmlspark](https://github.com/Azure/mmlspark) para conectarse a Cognitive Services.

Los detalles de Azure Key Vault que ha proporcionado le permiten hacer referencia de forma segura a sus secretos desde esta experiencia sin revelarlos.

Ya puede **Ejecutar todas** las celdas para proceder a la detección de anomalías. Más información sobre [Cognitive Services - Anomaly Detector](https://go.microsoft.com/fwlink/?linkid=2147493).

![Ejecución de detección de anomalías](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00f.png)

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Análisis de sentimiento con Azure Cognitive Services](tutorial-cognitive-services-sentiment.md)
- [Tutorial: Puntuación de modelos de Machine Learning en Azure Synapse para grupos de SQL dedicados](tutorial-sql-pool-model-scoring-wizard.md)
- [Funcionalidades de aprendizaje automático en Azure Synapse Analytics](what-is-machine-learning.md)