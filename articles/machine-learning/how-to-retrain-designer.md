---
title: Volver a entrenar modelos mediante el diseñador de Azure Machine Learning (versión preliminar)
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo volver a entrenar modelos con canalizaciones publicadas en el diseñador de Azure Machine Learning (versión preliminar).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: keli19
author: likebupt
ms.date: 02/24/2020
ms.openlocfilehash: c8791e933882832dc7b0037c860a4c4e1e9a54c7
ms.sourcegitcommit: 0553a8b2f255184d544ab231b231f45caf7bbbb0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2020
ms.locfileid: "80389042"
---
# <a name="retrain-models-with-azure-machine-learning-designer-preview"></a>Volver a entrenar modelos con el diseñador de Azure Machine Learning (versión preliminar)
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Siga los pasos de este artículo para aprender a usar el diseñador de Azure Machine Learning para volver a entrenar un modelo de aprendizaje automático. Descubra cómo usar las canalizaciones publicadas para automatizar los flujos de trabajo de aprendizaje automático para volver a entrenar.

En este artículo aprenderá a:

> [!div class="checklist"]
> * Entrenar un modelo de Machine Learning.
> * Crear un parámetro de canalización.
> * Publicar la canalización de entrenamiento.
> * Volver a entrenar el modelo.

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://aka.ms/AMLFree).
* Un área de trabajo de Azure Machine Learning con un SKU de Enterprise.

En este artículo se da por hecho que tiene conocimientos básicos sobre la compilación de una canalización en el diseñador. Para una introducción guiada al diseñador, complete el [tutorial](tutorial-designer-automobile-price-train-score.md). 

### <a name="sample-pipeline"></a>Canalización de ejemplo

La canalización usada en este artículo es una versión modificada de la que se encuentra en [Ejemplo 3: Predicción de ingresos](how-to-designer-sample-classification-predict-income.md). Usa el módulo [Importación de datos](algorithm-module-reference/import-data.md) en lugar del conjunto de datos de ejemplo para mostrarle cómo entrenar un modelo con sus propios datos.

![Captura de pantalla que muestra la canalización de ejemplo modificada con un cuadro que resalta el módulo Importación de datos](./media/how-to-retrain-designer/modified-sample-pipeline.png)

## <a name="train-a-machine-learning-model"></a>Entrenar un modelo de Machine Learning

Para volver a entrenar un modelo, necesita un modelo inicial. En esta sección, aprenderá a entrenar un modelo y a obtener acceso al modelo guardado mediante el diseñador.

1. Seleccione el módulo **Importación de datos**.
1. En el panel de propiedades, especifique un origen de datos.

   ![Captura de pantalla que muestra una configuración de ejemplo del módulo Importación de datos](./media/how-to-retrain-designer/import-data-settings.png)

   En este ejemplo, los datos se almacenan en un [almacén de datos de Azure](how-to-access-data.md). Si aún no tiene ningún almacén de datos, puede crear uno ahora seleccionando **Nuevo almacén de almacenes**.

1. Especifique la ruta de acceso a los datos. También puede seleccionar **Examinar ruta de acceso** para navegar a su almacén de datos. 
1. Seleccione **Enviar** en la parte superior del lienzo.
    
   > [!NOTE]
   > Si ya ha establecido un proceso predeterminado para este borrador de canalización, la canalización se ejecutará automáticamente. De lo contrario, puede seguir las indicaciones del panel de configuración para establecer uno ahora.

### <a name="find-your-trained-model"></a>Búsqueda del modelo entrenado

El diseñador guarda todas las salidas de canalización, incluidos los modelos entrenados, en la cuenta de almacenamiento predeterminada. Sin embargo, también puede acceder a los modelos entrenados directamente en el diseñador:

1. Espere a que finalice la ejecución de la canalización.
1. Seleccione el módulo **Train Model** (Entrenar modelo).
1. En el panel de configuración, seleccione **Resultados y registros**.
1. Seleccione el icono **Ver salida** y siga las instrucciones de la ventana emergente para buscar el modelo entrenado.

![Captura de pantalla que muestra cómo descargar el modelo entrenado](./media/how-to-retrain-designer/trained-model-view-output.png)

## <a name="create-a-pipeline-parameter"></a>Creación de un parámetro de canalización

Agregue parámetros de canalización para establecer variables de forma dinámica en tiempo de ejecución. Para esta canalización, agregue un parámetro para la ruta de acceso a los datos de entrenamiento para que pueda volver a entrenar el modelo en un nuevo conjunto de datos.

1. Seleccione el módulo **Importación de datos**.
1. En el panel Configuración, seleccione los puntos suspensivos que se encuentran encima del campo **Ruta de acceso**.
1. Seleccione **Agregar al parámetro de canalización**.
1. Proporcione un nombre de parámetro y un valor predeterminado.

   > [!NOTE]
   > Puede inspeccionar y editar los parámetros de canalización; para ello, seleccione el icono de engranaje de **Configuración** situado junto al título del borrador de la canalización. 

![Captura de pantalla que muestra cómo crear un parámetro de canalización](media/how-to-retrain-designer/add-pipeline-parameter.png)

## <a name="publish-a-training-pipeline"></a>Publicación de una canalización de entrenamiento

Al publicar una canalización, se crea un punto de conexión de canalización. Los puntos de conexión de canalización permiten reutilizar y administrar las canalizaciones para la repetibilidad y automatización. En este ejemplo, ha configurado la canalización para volver a realizar el entrenamiento.

1. Seleccione la opción **Publicar** situada sobre el lienzo del diseñador.
1. Seleccione o cree un punto de conexión de canalización.

   > [!NOTE]
   > Puede publicar varias canalizaciones en un solo punto de conexión. Cada canalización del punto de conexión recibe un número de versión, que se puede especificar cuando se llama al punto de conexión de la canalización.

1. Seleccione **Publicar**.

## <a name="retrain-your-model"></a>Volver a entrenar el modelo

Ahora que tiene una canalización de entrenamiento publicada, puede usarla para volver a entrenar el modelo con nuevos datos. Puede enviar ejecuciones de un punto de conexión de la canalización desde Azure Portal o mediante programación.

### <a name="submit-runs-by-using-the-designer"></a>Envío de ejecuciones mediante el diseñador

Siga estos pasos para enviar un punto de conexión de la canalización ejecutado desde el diseñador:

1. Vaya a la página **Puntos de conexión**.
1. Seleccione la pestaña **Puntos de conexión de la canalización**.
1. Seleccione el punto de conexión de la canalización.
1. Seleccione la pestaña **Canalizaciones publicadas**.
1. Seleccione la canalización que quiera ejecutar.
1. Seleccione **Submit** (Enviar).
1. En el cuadro de diálogo de configuración, puede especificar un nuevo valor de ruta de acceso a los datos de entrada. Este valor apunta al nuevo conjunto de valores.

![Captura de pantalla que muestra cómo configurar una ejecución de canalización parametrizada en el diseñador](./media/how-to-retrain-designer/published-pipeline-run.png)

### <a name="submit-runs-by-using-code"></a>Envío de ejecuciones mediante código

Puede encontrar el punto de conexión REST de una canalización publicada en el panel de información general. Mediante una llamada al punto de conexión, puede volver a entrenar la canalización publicada.

Para hacer una llamada a REST, necesitará un encabezado de autenticación de tipo portador de OAuth 2.0. Para obtener información sobre cómo configurar la autenticación en el área de trabajo y realizar una llamada a REST con parámetros, consulte el tutorial [Compilación de una canalización de Azure Machine Learning para la puntuación por lotes](tutorial-pipeline-batch-scoring-classification.md#publish-and-run-from-a-rest-endpoint).

## <a name="next-steps"></a>Pasos siguientes

Siga el [tutorial del diseñador](tutorial-designer-automobile-price-train-score.md) para entrenar e implementar un modelo de regresión.
