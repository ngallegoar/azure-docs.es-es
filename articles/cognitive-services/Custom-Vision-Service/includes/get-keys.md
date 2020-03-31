---
author: PatrickFarley
ms.service: cognitive-services
ms.topic: include
ms.date: 02/07/2019
ms.author: pafarley
ms.openlocfilehash: fa13d9bdf73b87597d4eb01830ec2c76538521fe
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73800113"
---
## <a name="get-the-training-and-prediction-keys"></a>Obtención de las claves de entrenamiento y predicción

El proyecto necesita un conjunto válido de claves de suscripción para interactuar con el servicio. Puede encontrar los elementos en el [sitio web de Custom Vision](https://customvision.ai). Inicie sesión con la cuenta asociada a la cuenta de Azure usada para crear los recursos de Custom Vision. En la página principal (la página con la opción para agregar un nuevo proyecto), seleccione el __icono de engranaje__ de la esquina superior derecha. Busque los recursos de entrenamiento y predicción en la lista y expándalos. Aquí puede encontrar los valores de clave de entrenamiento, clave de predicción e identificador de recurso de predicción. Guarde estos valores en una ubicación temporal.

![Imagen de la interfaz de usuario de claves](../media/csharp-tutorial/training-prediction-keys.png)

También puede obtener estas claves y este identificador en [Azure Portal](https://www.portal.azure.com) viendo los recursos de entrenamiento y predicción de Custom Vision y navegando a la pestaña __Claves__. En ella encontrará la clave de entrenamiento y la clave de predicción. Vaya a la pestaña __Propiedades__ del recurso de predicción para obtener el identificador del recurso de predicción.

