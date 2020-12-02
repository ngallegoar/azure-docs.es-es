---
author: PatrickFarley
ms.service: cognitive-services
ms.topic: include
ms.date: 02/07/2019
ms.author: pafarley
ms.openlocfilehash: 874d76bebdfd3bd0daba1f83cb1f06c093f192ec
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96021292"
---
## <a name="get-the-training-and-prediction-keys"></a>Obtención de las claves de entrenamiento y predicción

El proyecto necesita un conjunto válido de claves de suscripción para interactuar con el servicio. Puede encontrar los elementos en el [sitio web de Custom Vision](https://customvision.ai). Inicie sesión con la cuenta asociada a la cuenta de Azure que usó para crear los recursos de Custom Vision. En la página principal (la página con la opción para agregar un nuevo proyecto), seleccione el __icono de engranaje__ de la esquina superior derecha. Busque los recursos de entrenamiento y predicción en la lista y expándalos. Aquí puede encontrar los valores de clave de entrenamiento, clave de predicción e identificador de recurso de predicción. Guarde estos valores en una ubicación temporal.

> [!NOTE]
> Si utiliza una clave todo en uno de Cognitive Services para acceder a Custom Vision, solo verá una clave en la pantalla de configuración. En ese caso, usará la misma clave para las operaciones de entrenamiento y de predicción.

![Imagen de la interfaz de usuario de claves](../media/csharp-tutorial/training-prediction-keys.png)

También puede obtener estas claves y este identificador en [Azure Portal](https://www.portal.azure.com) viendo los recursos de entrenamiento y predicción de Custom Vision y navegando a la pestaña __Claves__. En ella encontrará la clave de entrenamiento y la clave de predicción. Vaya a la pestaña __Propiedades__ del recurso de predicción para obtener el identificador del recurso de predicción.

