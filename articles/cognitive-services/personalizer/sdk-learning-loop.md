---
title: 'Inicio rápido: Creación y uso de bucles de aprendizaje con el SDK: Personalizer'
description: En este inicio rápido se muestra cómo crear y administrar la base de conocimiento mediante el SDK de cliente.
ms.topic: quickstart
ms.date: 07/30/2020
ms.custom: tracking-python, devx-track-javascript
zone_pivot_groups: programming-languages-set-six
ms.openlocfilehash: 05657d350616502c3d32b500a32b394a748ab2f6
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2020
ms.locfileid: "87461136"
---
# <a name="quickstart-personalizer-client-library"></a>Inicio rápido: Biblioteca cliente de Personalizer

Muestre contenido personalizado en este inicio rápido con el servicio Personalizer.

Comience a trabajar con la biblioteca cliente de Personalizer. Siga estos pasos para instalar el paquete y probar el código de ejemplo para realizar tareas básicas.

 * La API Rank: selecciona el mejor de todos los elementos de las acciones, en función de la información en tiempo real que se proporciona sobre el contenido y el contexto.
 * API Reward: determine la puntuación de recompensa en función de sus necesidades empresariales y, luego, envíela a Personalizer con esta API. Esa puntuación puede ser un solo valor, por ejemplo, 1 para bueno y 0 para malo, o un algoritmo que cree en función de sus necesidades empresariales.

::: zone pivot="programming-language-csharp"
[!INCLUDE [Get intent with C# SDK](./includes/quickstart-sdk-csharp.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [Get intent with Node.js SDK](./includes/quickstart-sdk-nodejs.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Get intent with Python SDK](./includes/quickstart-sdk-python.md)]
::: zone-end

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y eliminar una suscripción a Cognitive Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [CLI de Azure](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
>[Funcionamiento de Personalizer](how-personalizer-works.md)

* [¿Qué es Personalizer?](what-is-personalizer.md)
* [¿Dónde se puede utilizar Personalizer?](where-can-you-use-personalizer.md)
* [Solución de problemas](troubleshooting.md)
* El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/quickstarts/python/sample.py).
