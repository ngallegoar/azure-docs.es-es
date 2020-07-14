---
title: 'Inicio rápido: Compilación, implementación y uso de un modelo personalizado (Traductor personalizado)'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido recorrerá el proceso paso a paso para compilar un sistema de traducción mediante Custom Translator.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 05/26/2020
ms.author: swmachan
ms.topic: quickstart
ms.openlocfilehash: b0992c4d18fdb9cb5201ab3ef52fba8ee3feb7a2
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2020
ms.locfileid: "85964386"
---
# <a name="quickstart-build-deploy-and-use-a-custom-model-for-translation"></a>Inicio rápido: Compilación, implementación y uso de un modelo personalizado para la traducción

En este artículo se proporcionan instrucciones detalladas para compilar un sistema de traducción con Custom Translator.

## <a name="prerequisites"></a>Requisitos previos

1. Para usar el portal de [Custom Translator](https://portal.customtranslator.azure.ai), necesitará una [cuenta Microsoft](https://signup.live.com) o [cuenta de Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) (cuenta de organización hospedada en Azure) para iniciar sesión.

2. Una suscripción a Traductor a través de Azure Portal. Necesitará una clave de suscripción de Traductor para asociarla con el área de trabajo de Traductor personalizado. Consulte [Cómo suscribirse a Traductor](https://docs.microsoft.com/azure/cognitive-services/translator/translator-text-how-to-signup).

3. Cuando tenga los dos anteriores, inicie sesión in en el portal [Custom Translator](https://portal.customtranslator.azure.ai) para crear áreas de trabajo y proyectos, así como cargar archivos y crear e implementar modelos.

## <a name="create-a-workspace"></a>Crear un área de trabajo

Si es la primera vez que lo usa, se le solicitará que acepte los términos del servicio para crear un área de trabajo y asociarla a la suscripción de Traductor.

![Crear área de trabajo](media/quickstart/terms-of-service.png)
![Crear área de trabajo](media/quickstart/create-workspace-1.png)
![Crear área de trabajo](media/quickstart/create-workspace-2.png)
![Crear área de trabajo](media/quickstart/create-workspace-3.png)
![Crear área de trabajo](media/quickstart/create-workspace-4.png)
![Crear área de trabajo](media/quickstart/create-workspace-5.png)
![Crear área de trabajo](media/quickstart/create-workspace-6.png)

En las posteriores visitas al portal de Traductor personalizado, navegue hasta la página de configuración, donde puede administrar el área de trabajo, crear más áreas de trabajo, asociar la clave de suscripción de Traductor a sus áreas de trabajo, agregar copropietarios y cambiar una clave de suscripción.

## <a name="create-a-project"></a>Crear un proyecto

En la página de aterrizaje del portal de Custom Translator, haga clic en Nuevo proyecto. En el cuadro de diálogo puede especificar el nombre de proyecto deseado, el par de idiomas y la categoría, así como otros campos importantes. Después, guarde el proyecto. Para más información, visite [Creación de proyectos](how-to-create-project.md).

![Crear proyecto](media/quickstart/ct-how-to-create-project.png)


## <a name="upload-documents"></a>Cargar documentos

A continuación, cargue los conjuntos de documentos de [aprendizaje](training-and-model.md#training-document-type-for-custom-translator), [optimización](training-and-model.md#tuning-document-type-for-custom-translator) y [pruebas](training-and-model.md#testing-dataset-for-custom-translator). Puede cargar documentos [paralelos](what-are-parallel-documents.md) y documentos de cuadro combinado. También puede cargar un [diccionario](what-is-dictionary.md).

Puede cargar documentos desde la pestaña de documentos o desde la página de un proyecto concreto.

![Cargar documentos](media/quickstart/ct-how-to-upload.png)

Al cargar los documentos, elija el tipo de documento (aprendizaje, optimización o pruebas) y el par de idiomas. Si carga documentos paralelos tendrá que especificar además un nombre de documento. Para más información, visite [Carga de documentos](how-to-upload-document.md).

## <a name="create-a-model"></a>Crear un modelo

Una vez cargados todos los documentos necesarios el siguiente paso es crear el modelo.

Seleccione el proyecto que ha creado. Verá todos los documentos que ha cargado que compartan el par de idiomas con este proyecto. Seleccione los documentos que desea incluir en el modelo. Puede seleccionar datos de [aprendizaje](training-and-model.md#training-document-type-for-custom-translator), [optimización](training-and-model.md#tuning-document-type-for-custom-translator) y [prueba](training-and-model.md#testing-dataset-for-custom-translator) o solo seleccionar datos de aprendizaje y dejar que Custom Translator compile automáticamente los conjuntos de datos de optimización y prueba del modelo.

![Crear un modelo](media/quickstart/ct-how-to-train.png)

Cuando haya acabado de seleccionar los documentos que desee, haga clic en el botón Crear modelo para crear el modelo e iniciar el aprendizaje. Puede ver el estado del aprendizaje y los detalles de todos los modelos entrenados, en la pestaña Modelos.

Para más información, visite [Creación de un modelo](how-to-train-model.md).

## <a name="analyze-your-model"></a>Análisis del modelo

Una vez que el aprendizaje se ha completado correctamente, inspeccione los resultados. La puntuación BLEU es una métrica que indica la calidad de la traducción. También puede comparar manualmente las traducciones realizadas con el modelo personalizado con las traducciones proporcionadas en el conjunto de pruebas. Para ello, vaya a la pestaña "Prueba" y haga clic en "Resultados del sistema". Si inspecciona manualmente varias de estas traducciones podrá hacerse una buena idea de la calidad de la traducción que su sistema ha producido. Para más información, visite la página de [resultados de pruebas del sistema](how-to-view-system-test-results.md).

## <a name="deploy-a-trained-model"></a>Implementación de un modelo entrenado

Cuando esté listo para implementar el modelo entrenado, haga clic en el botón "Implementar". Puede tener un modelo implementado por proyecto y puede ver el estado de la implementación en la columna Estado. Para más información, visite [Model Deployment](how-to-view-system-test-results.md#deploy-a-model) (Implementación de un modelo)

![Implementación de un modelo entrenado](media/quickstart/ct-how-to-deploy.png)

## <a name="use-a-deployed-model"></a>Uso de un modelo implementado

Se puede acceder a los modelos implementados mediante Traductor especificando el valor del id. de categoría(https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl). Puede encontrar más información sobre Traductor en la página web de [referencia de API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference).

## <a name="next-steps"></a>Pasos siguientes

- Vaya a [Custom Translator workspace and manage your projects](workspace-and-project.md) (Área de trabajo de Custom Translator y administración de proyectos).
