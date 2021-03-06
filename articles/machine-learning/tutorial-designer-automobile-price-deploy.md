---
title: 'Tutorial: Implementación de modelos de ML con el diseñador'
titleSuffix: Azure Machine Learning
description: Cree una solución de análisis predictivo en el diseñador de Azure Machine Learning. Entrene, puntúe e implemente un modelo de Machine Learning mediante módulos del tipo arrastrar y colocar.
author: likebupt
ms.author: keli19
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 11/25/2020
ms.custom: designer
ms.openlocfilehash: 28358db2e538867957fcc8d4984cf1c028f9ab2c
ms.sourcegitcommit: 2e9643d74eb9e1357bc7c6b2bca14dbdd9faa436
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96030378"
---
# <a name="tutorial-deploy-a-machine-learning-model-with-the-designer"></a>Tutorial: Implementación de un modelo de Machine Learning con el diseñador


Puede implementar el modelo predictivo desarrollado en la [parte uno del tutorial](tutorial-designer-automobile-price-train-score.md) para que otros usuarios puedan usarlo. En la parte 1, ha entrenado el modelo. Ahora, es momento de generar predicciones basadas en los datos que escriba el usuario. En esta parte del tutorial, va a realizar lo siguiente:

> [!div class="checklist"]
> * Crear una canalización de inferencia en tiempo real.
> * Crear un clúster de inferencia.
> * Implementación del punto de conexión en tiempo real.
> * Prueba del punto de conexión en tiempo real.

## <a name="prerequisites"></a>Requisitos previos

Complete la [parte uno del tutorial](tutorial-designer-automobile-price-train-score.md) para aprender a entrenar y puntuar un modelo de Machine Learning en el diseñador.

[!INCLUDE [machine-learning-missing-ui](../../includes/machine-learning-missing-ui.md)]

## <a name="create-a-real-time-inference-pipeline"></a>Crear una canalización de inferencia en tiempo real

Para implementar la canalización, antes debe convertir la canalización de entrenamiento en una canalización de inferencia en tiempo real. Este proceso quita los módulos de entrenamiento y agrega entradas y salidas de servicios web para administrar las solicitudes.

### <a name="create-a-real-time-inference-pipeline"></a>Crear una canalización de inferencia en tiempo real

1. Seleccione, encima del lienzo de la canalización, **Create inference pipeline** > **Real-time inference pipeline** (Crear canalización de inferencia > Canalización de inferencia en tiempo real).

    :::image type="content" source="./media/tutorial-designer-automobile-price-deploy/tutorial2-create-inference-pipeline.png"alt-text="Captura de pantalla que muestra dónde encontrar el botón Crear canalización":::

    La canalización debería tener este aspecto: 

   ![Captura de pantalla que muestra la configuración esperada de la canalización después de prepararla para la implementación](./media/tutorial-designer-automobile-price-deploy/real-time-inference-pipeline.png)

    Al seleccionar **Create inference pipeline** (Crear canalización de inferencia), suceden varias cosas:
    
    * El modelo entrenado se almacena como un módulo **Conjunto de datos** en la paleta de módulos. Puede encontrarlo en **My Datasets** (Mis conjuntos de datos).
    * Se quitan los módulos de entrenamiento como **Train Model** (Entrenar modelo) y **Split Data** (Dividir datos).
    * El modelo entrenado guardado se vuelve a agregar a la canalización.
    * Se agregan los módulos **Web Service Input** (Entrada de servicio web) y **Web Service Output** (Salida de servicio web). Estos módulos muestran dónde entran los datos del usuario en la canalización y dónde se devuelven.

    > [!NOTE]
    > De forma predeterminada, la **entrada de servicio web** esperará el mismo esquema de datos que los datos de entrenamiento usados para crear la canalización predictiva. En este escenario, el precio se incluye en el esquema. Sin embargo, este no se usa como factor durante la predicción.
    >

1. Seleccione **Enviar** y use el mismo destino de proceso y el mismo experimento que usó en la parte 1.

    Si esta es la primera ejecución, la canalización puede tardar hasta 20 minutos en finalizar la ejecución. La configuración del proceso predeterminada tiene un tamaño de nodo mínimo de 0, lo que significa que el diseñador debe asignar recursos después de estar inactivo. Las ejecuciones de canalización repetidas tardarán menos en terminar, dado que los recursos del proceso ya están asignados. Además, el diseñador usa resultados almacenados en la caché en cada módulo para mejorar aún más la eficiencia.

1. Seleccione **Implementar**.

## <a name="create-an-inferencing-cluster"></a>Creación de un clúster de inferencia

En el cuadro de diálogo que aparece, puede seleccionar entre los clústeres de Azure Kubernetes Service (AKS) existentes aquel en el que quiere implementar el modelo. Si no tiene un clúster de AKS, use los pasos siguientes para crear uno.

1. Seleccione **Compute** (Proceso) en el cuadro de diálogo que aparece, para ir a la página **Compute** (Proceso).

1. En la cinta de opciones de navegación, seleccione **Inference Clusters** (Clústeres de inferencia) >  **+ Nuevo**.

    ![Captura de pantalla que muestra cómo ir al panel del nuevo clúster de inferencia](./media/tutorial-designer-automobile-price-deploy/new-inference-cluster.png)
   
1. En el panel del clúster de inferencia, configure un nuevo servicio de Kubernetes.

1. Escriba *aks-compute* en **Compute name** (Nombre de proceso).
    
1. Seleccione una región cercana que esté disponible para la **región**.

1. Seleccione **Crear**.

    > [!NOTE]
    > La creación de un servicio AKS tarda unos 15 minutos. Puede comprobar el estado de aprovisionamiento en la página **Inference Clusters** (Clústeres de inferencia).
    >

## <a name="deploy-the-real-time-endpoint"></a>Implementación del punto de conexión en tiempo real

Después de que el servicio AKS haya terminado de aprovisionarse, vuelva a la canalización de inferencia en tiempo real para finalizar la implementación.

1. Seleccione **Implementar** encima del lienzo.

1. Seleccione **Deploy new real-time endpoint** (Implementar nuevo punto de conexión en tiempo real). 

1. Seleccione el clúster de AKS que ha creado.

1. Seleccione **Implementar**.
    
    :::image type="content" source="./media/tutorial-designer-automobile-price-deploy/setup-endpoint.png"alt-text="Captura de pantalla que muestra cómo configurar un nuevo punto de conexión en tiempo real":::

    Aparece una notificación de finalización correcta en la parte superior del lienzo una vez que termina la implementación. Esto puede llevar unos minutos.

## <a name="view-the-real-time-endpoint"></a>Visualización del punto de conexión en tiempo real

Una vez finalizada la implementación, puede ver el punto de conexión en tiempo real; para ello, vaya a la página **Endpoints** (Puntos de conexión).

1. En la página **Puntos de conexión**, seleccione el punto de conexión que implementó.

1. En la pestaña **Details** (Detalles), puede ver más información, como el identificador URI de REST, el estado y las etiquetas.

1. En la pestaña **Consume** (Consumir), puede encontrar las claves de seguridad y establecer los métodos de autenticación.

1. En la pestaña **Registros de implementación**, puede encontrar los registros de implementación detallados del punto de conexión en tiempo real. 

Para más información sobre cómo utilizar el servicio web, consulte [Consumo de un modelo implementado como servicio web](how-to-consume-web-service.md).

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido los pasos clave para crear, implementar y usar un modelo de Machine Learning en el diseñador. Para más información sobre cómo usar el diseñador para resolver otros tipos de problemas, consulte las otras canalizaciones de ejemplo.

> [!div class="nextstepaction"]
> [Ejemplos del diseñador](samples-designer.md)
