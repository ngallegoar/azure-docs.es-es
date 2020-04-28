---
title: Entrada/salida de servicio web
description: Obtenga información sobre los módulos de servicio web en el diseñador de Azure Machine Learning (versión preliminar)
titleSuffix: Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 04/13/2020
ms.openlocfilehash: be940e3ba693270707e22ffc7b9377dbea1df5f8
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81462024"
---
# <a name="web-service-inputoutput"></a>Entrada/salida de servicio web

En este artículo se describe el módulo **Entrada de servicio web** y el módulo **Salida de servicio web** en el diseñador de Azure Machine Learning (versión preliminar).

El módulo **Entrada de servicio web** solo puede conectarse al puerto de entrada con el tipo **DataFrameDirectory**. El módulo **Salida de servicio web** solo puede conectarse desde el puerto de salida con el tipo **DataFrameDirectory**. Los dos módulos pueden encontrarse en el árbol de módulos, en la categoría **Servicio web**. 

El módulo **Entrada de servicio web** se usa para indicar el lugar en el que los datos del usuario entran en la canalización, y el módulo **Salida de servicio web** se usa para indicar a dónde se devuelven los datos del usuario en una canalización de inferencia en tiempo real.

## <a name="how-to-use-web-service-inputoutput"></a>Uso de Entrada/salida de servicio web

- Al crear una canalización de inferencia en tiempo real a partir de la canalización de entrenamiento, los módulos **Entrada de servicio web** y **Salida de servicio web** se agregarán automáticamente para mostrar el lugar donde los datos del usuario entran en la canalización y a dónde se devuelven. 

    Obtenga más información sobre la [creación una canalización de inferencia en tiempo real](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-deploy#create-a-real-time-inference-pipeline).

    > [!NOTE]
    > La generación automática de una canalización de inferencia en tiempo real es un proceso de mejor esfuerzo basado en reglas, no existe ninguna garantía de la corrección. Puede agregar o quitar manualmente módulos **Entrada/salida de servicio web** para satisfacer sus requisitos. Asegúrese de que haya al menos un módulo **Entrada de servicio web** y un módulo **Salida de servicio web** en la canalización de inferencia en tiempo real. Si tiene varios módulos **Entrada de servicio web** o **Salida de servicio web**, asegúrese de que tengan nombres únicos (puede especificar el nombre en el panel derecho del módulo).

- También puede crear manualmente una canalización de inferencia en tiempo real agregando módulos **Entrada de servicio web** y **Salida de servicio web** a la canalización sin enviar.

    > [!NOTE]
    >  El tipo de canalización se establecerá la primera vez que la envíe. Por lo tanto, asegúrese de agregar módulos **Entrada de servicio web** y **Salida de servicio web** antes de enviarla por primera vez si desea crear una canalización de inferencia en tiempo real.

   En el ejemplo siguiente se muestra cómo crear manualmente una canalización de inferencia en tiempo real desde el módulo **Ejecutar el script de Python**. 

   ![Ejemplo](media/module/web-service-input-output-example.png)
   
   Después de enviar la canalización y de que la ejecución se complete correctamente, podrá implementar el punto de conexión en tiempo real.
   
   > [!NOTE]
   >  En el ejemplo anterior, **Enter Data Manually** proporciona el esquema de datos para la entrada del servicio web y es necesario para implementar el punto de conexión en tiempo real. En general, debe conectar siempre un módulo o un conjunto de datos al puerto al que está conectada la **Entrada de servicio web** para proporcionar el esquema de datos.
   
## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre la [implementación del punto de conexión en tiempo real](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-deploy#deploy-the-real-time-endpoint).
Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning.