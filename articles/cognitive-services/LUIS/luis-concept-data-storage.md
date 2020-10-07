---
title: 'Datos y almacenamiento: LUIS'
titleSuffix: Azure Cognitive Services
description: LUIS almacena datos cifrados en un almacén de datos de Azure correspondiente a la región especificada por la clave.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.openlocfilehash: b74621f357613bc60457ab4846b58f6f85009401
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91541890"
---
# <a name="data-storage-and-removal-in-language-understanding-luis-cognitive-services"></a>Almacenamiento y eliminación de datos en Cognitive Services de Language Understanding (LUIS)
LUIS almacena datos cifrados en un almacén de datos de Azure correspondiente a la región especificada por la clave. Estos datos se almacenan durante 30 días. 

## <a name="export-and-delete-app"></a>Exportar y eliminar la aplicación
Los usuarios tienen un control total sobre la [exportación](luis-how-to-start-new-app.md#export-app) y la [eliminación](luis-how-to-start-new-app.md#delete-app) de la aplicación. 

## <a name="utterances"></a>Grabaciones de voz

Las expresiones se pueden almacenar en dos lugares diferentes. 

* Durante el **proceso de creación**, las expresiones se crean y almacenan en la intención. Las expresiones de intenciones son necesarias para crear una aplicación de LUIS correctamente. Una vez que la aplicación se publica y recibe consultas en el punto de conexión, la cadena de consulta de la solicitud del punto de conexión, las consultas `log=false`, determina si se almacena la expresión del punto de conexión. Si se almacena el punto de conexión, se convierte en parte de las expresiones del aprendizaje activo que se encuentran en la sección **Build** (compilación) del portal, en la sección **Review endpoint utterances** (Revisar las expresiones del punto de conexión). 
* Al **revisar las expresiones del punto de conexión** y agregar una expresión a una intención, la expresión ya no se almacena como parte de las expresiones del punto de conexión que se van a revisar. Se agrega a las intenciones de la aplicación. 

<a name="utterances-in-an-intent"></a>

### <a name="delete-example-utterances-from-an-intent"></a>Eliminación de expresiones de ejemplo de una intención

Elimine expresiones de ejemplo que se han usado para entrenar a [LUIS](luis-reference-regions.md). Si elimina una expresión de ejemplo desde la aplicación de LUIS, se eliminará del servicio web de LUIS y no estará disponible para la exportación.

<a name="utterances-in-review"></a>

### <a name="delete-utterances-in-review-from-active-learning"></a>Eliminación de expresiones de la revisión desde el aprendizaje activo

Puede eliminar expresiones de la lista de expresiones de usuario que LUIS sugiere en la página **[Review endpoint utterances](luis-how-to-review-endpoint-utterances.md)** (Revisar expresiones de punto de conexión). La eliminación de las expresiones de esta lista evita que se sugieran, pero no las elimina de los registros.

Si no quiere expresiones de aprendizaje activo, puede [deshabilitar el aprendizaje activo](luis-how-to-review-endpoint-utterances.md#disable-active-learning). Al deshabilitar el aprendizaje activo también se deshabilita el registro.

### <a name="disable-logging-utterances"></a>Deshabilitación de expresiones del registro
[Al deshabilitar el aprendizaje activo](luis-how-to-review-endpoint-utterances.md#disable-active-learning) se deshabilita el registro.


<a name="accounts"></a>

## <a name="delete-an-account"></a>Eliminación de una cuenta
Si elimina una cuenta, se eliminarán todas las aplicaciones, incluso las expresiones de ejemplo y los registros. Los datos se conservan durante 60 días antes de que la cuenta y los datos se eliminen de forma permanente.

Para eliminar la cuenta debe ir a la página **Configuración**. Seleccione el nombre de la cuenta en la barra de navegación situada en la parte superior derecha para ir a la página **Configuración**.

## <a name="data-inactivity-as-an-expired-subscription"></a>Inactividad de datos como suscripción caducada
Para eliminar y retener datos, una aplicación de LUIS inactiva podría tratarse como una suscripción caducada a _discreción de Microsoft_. Una aplicación se considera inactiva si cumple los siguientes criterios durante los últimos 90 días: 

* **No** ha recibido ninguna llamada.
* No se ha modificado.
* No tiene ninguna clave actual asignada.
* Ningún usuario ha iniciado sesión en ella.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Obtenga información sobre cómo exportar y eliminar una aplicación](luis-how-to-start-new-app.md)
