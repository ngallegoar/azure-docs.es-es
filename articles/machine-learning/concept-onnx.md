---
title: 'ONNX: inferencia de alto rendimiento multiplataforma'
titleSuffix: Azure Machine Learning
description: Aprenda cómo el uso de Open Neural Network Exchange (ONNX) puede ayudarle a optimizar la inferencia del modelo de Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: prasantp
author: prasanthpul
ms.date: 08/15/2019
ms.custom: seodec18
ms.openlocfilehash: 98aebb4733c2aa2a6d0b0217f1f437bcea1992e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79232916"
---
# <a name="onnx-and-azure-machine-learning-create-and-accelerate-ml-models"></a>ONNX y Azure Machine Learning: Crear y acelerar modelos de ML

Aprenda a usar [Open Neural Network Exchange](https://onnx.ai) (ONNX) y optimice la inferencia de su modelo de Machine Learning. Inferencia o modelo de puntuación, es la fase donde se usa el modelo implementado para la predicción, frecuentemente en datos de producción. 

La optimización de los modelos de Machine Learning para la inferencia (o puntuación del modelo) es difícil, ya que necesita ajustar el modelo y la biblioteca de inferencia para aprovechar al máximo las capacidades del hardware. El problema se vuelve extremadamente difícil si quiere obtener un rendimiento óptimo en diferentes tipos de plataformas (nube/edge, CPU/GPU, etc.), ya que cada una tiene diferentes capacidades y características. La complejidad aumenta si tiene modelos de una variedad de marcos que necesitan ejecutarse en ciertas plataformas. Se necesita mucho tiempo para optimizar todas las combinaciones diferentes de marcos y hardware. Igualmente, se necesita una solución para entrenar una vez en su marco preferido y ejecutarlo en cualquier lugar de la nube o Edge. Aquí es donde ONNX entra en juego.

Microsoft y una comunidad de asociados crearon ONNX como un estándar abierto para representar modelos de Machine Learning. Los modelos de [varios marcos](https://onnx.ai/supported-tools), incluidos TensorFlow, PyTorch, SciKit-Learn, Keras, Chainer, MXNet y MATLAB se pueden exportar o convertir al formato ONNX estándar. Una vez que los modelos están en el formato ONNX, se pueden ejecutar en varias plataformas y dispositivos.

[ONNX Runtime](https://github.com/Microsoft/onnxruntime) es un motor de inferencia de alto rendimiento que sirve para implementar modelos ONNX en la producción. Está optimizado tanto para la nube como para Edge y funciona en Linux, Windows y Mac. Escrito en C++, también tiene API de C, Python y C#. Asimismo, ONNX Runtime proporciona soporte técnico para todas las especificaciones de ONNX-ML y también se integra con aceleradores en hardware diferente, como TensorRT en GPU NVidia.

ONNX Runtime se usa en servicios de Microsoft a gran escala, como Bing, Office y Cognitive Services. Las mejoras en el rendimiento dependen de una serie de factores, pero estos servicios de Microsoft han __mejorado el doble el rendimiento de la CPU__. ONNX Runtime también se usa como parte de Windows ML en cientos de millones de dispositivos. Puede usar el tiempo de ejecución con los servicios de Azure Machine Learning. Al utilizar ONNX Runtime, puede beneficiarse de las amplias optimizaciones, pruebas y mejoras de producción.

[![Diagrama de flujo de ONNX que muestra el entrenamiento, los convertidores y la implementación](./media/concept-onnx/onnx.png)](././media/concept-onnx/onnx.png#lightbox)

## <a name="get-onnx-models"></a>Obtención de modelos de ONNX

Puede obtener modelos de ONNX de varias maneras:
+ Entrene un nuevo modelo de ONNX en Azure Machine Learning (consulte el ejemplo al final de este artículo).
+ Convierta el modelo existente de otro formato a ONNX (consulte los [tutoriales](https://github.com/onnx/tutorials)). 
+ Obtenga un modelo previamente entrenado de ONNX desde [ONNX Model Zoo](https://github.com/onnx/models) (consulte el ejemplo al final de este artículo).
+ Genere un modelo de ONNX personalizado desde [Azure Custom Vision Service](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/). 

Muchos modelos, incluidos la clasificación de imágenes, la detección de objetos y el procesamiento de texto, se pueden representar como modelos de ONNX. Sin embargo, es posible que algunos modelos no puedan convertirse correctamente. Si se encuentra en esta situación, registre un problema en GitHub acerca del convertidor que usó. Puede continuar usando su modelo de formato existente hasta que se resuelva el problema.

## <a name="deploy-onnx-models-in-azure"></a>Implementación de modelos de ONNX en Azure

Con Azure Machine Learning, puede implementar, administrar y supervisar sus modelos de ONNX. Mediante el [flujo de trabajo de implementación](concept-model-management-and-deployment.md) estándar y ONNX Runtime, puede crear un punto de conexión REST hospedado en la nube. Vea un ejemplo de un cuaderno de Jupyter Notebook al final de este artículo para probarlo usted mismo. 

### <a name="install-and-use-onnx-runtime-with-python"></a>Instalar y usar ONNX Runtime con Python

Los paquetes de Python para ONNX Runtime están disponibles en [PyPi.org](https://pypi.org) ([CPU](https://pypi.org/project/onnxruntime), [GPU](https://pypi.org/project/onnxruntime-gpu)). Lea los [requisitos del sistema](https://github.com/Microsoft/onnxruntime#system-requirements) antes de la instalación. 

 Para instalar ONNX Runtime para Python, use uno de los siguientes comandos: 
```python   
pip install onnxruntime       # CPU build
pip install onnxruntime-gpu   # GPU build
```

Para llamar a ONNX Runtime en el script de Python, use lo siguiente:    
```python
import onnxruntime
session = onnxruntime.InferenceSession("path to model")
```

La documentación que acompaña al modelo suele indicar las entradas y salidas para usar el modelo. También puede usar una herramienta de visualización como [Netron](https://github.com/lutzroeder/Netron) para ver el modelo. El entorno de ejecución de ONNX también le permite consultar los metadatos, las entradas y las salidas del modelo:    
```python
session.get_modelmeta()
first_input_name = session.get_inputs()[0].name
first_output_name = session.get_outputs()[0].name
```

Para realizar la inferencia de su modelo, use `run` y pase la lista de salidas que quiera que se devuelvan (déjelo vacío si quiere todas) y un mapa de los valores de entrada. El resultado es una lista de las salidas.  
```python
results = session.run(["output1", "output2"], {
                      "input1": indata1, "input2": indata2})
results = session.run([], {"input1": indata1, "input2": indata2})
```

Para obtener la referencia completa de API de Python, vea la [documentación de referencia de ONNX Runtime](https://aka.ms/onnxruntime-python).    

## <a name="examples"></a>Ejemplos

Vea [how-to-use-azureml/deployment/onnx](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx) para obtener cuadernos de ejemplo en los que se crean e implementan modelos de ONNX.

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="more-info"></a>Más información

Obtenga más información sobre ONNX o contribuya al proyecto:
+ [Sitio web del proyecto de ONNX](https://onnx.ai)
+ [Código de ONNX en GitHub](https://github.com/onnx/onnx)

Obtenga más información sobre ONNX Runtime o contribuya al proyecto:
+ [Repositorio de ONNX Runtime en GitHub](https://github.com/Microsoft/onnxruntime)


