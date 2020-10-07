---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: roy-har
manager: diberry
ms.service: cognitive-services
ms.date: 06/04/2020
ms.subservice: language-understanding
ms.topic: include
ms.custom: include file
ms.author: roy-har
ms.openlocfilehash: 9965e4c856fdef2af17b116264ad5344ebc97eb2
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "84466931"
---
Creación de la aplicación de pizza.

1. Seleccione [pizza-app-for-luis-v6.json](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/apps/pizza-app-for-luis-v6.json) para abrir la página de GitHub para el archivo `pizza-app-for-luis.json`.
1. Pulse de forma prolongada o haga clic con el botón derecho en el botón **Sin formato** y seleccione **Guardar vínculo como** para guardar el archivo `pizza-app-for-luis.json` en el equipo.
1. Inicie sesión en el [portal de LUIS](https://www.luis.ai).
1. Seleccione [Mis aplicaciones](https://www.luis.ai/applications).
1. En la página **Mis aplicaciones**, seleccione **+ Nueva aplicación para la conversación**.
1. Seleccione **Importar como JSON**.
1. En el cuadro de diálogo **Importar nueva aplicación**, seleccione el botón **Elegir archivo**.
1. Seleccione el archivo de `pizza-app-for-luis.json` que descargó y, a continuación, seleccione **Abrir**.
1. En el cuadro de diálogo **Importar nueva aplicación** en el campo **Nombre**, escriba un nombre para la aplicación de pizza y, a continuación, seleccione el botón **Listo**.

Se importará la aplicación.

Si ve un cuadro de diálogo **Cómo generar una aplicación eficaz de LUIS** , ciérrelo.

## <a name="train-and-publish-the-pizza-app"></a>Entrene y publique la aplicación de pizza

Debería consultar la página **Intenciones** con una lista de las intenciones de la aplicación de pizza.

[!INCLUDE [How to train](howto-train.md)]

[!INCLUDE [How to publish](howto-publish.md)]

## <a name="add-an-authoring-resource-to-the-pizza-app"></a>Adición de un recurso de creación a la aplicación de pizza

1. Seleccione **ADMINISTRAR**.
1. Seleccione **Recursos de Azure**.
1. Seleccione **Recurso de creación**.
1. Seleccione **Cambiar recurso de creación**.

Si tiene un recurso de creación, escriba el **nombre de inquilino**, **nombre de suscripción** y **nombre de recurso de LUIS** del recurso de creación.

Si no tiene un recurso de creación:

1. Seleccione **Crear nuevo recurso**.
1. Escriba un **nombre de inquilino**, **nombre de recurso**, **nombre de suscripción** y **nombre de grupo de recursos de Azure**.

La aplicación de pizza ya está lista para usarse.

## <a name="record-the-access-values-for-your-pizza-app"></a>Registro de los valores de acceso de la aplicación de pizza

Para usar la nueva aplicación de pizza, necesitará el identificador de la aplicación, la clave de creación y el punto de conexión de creación de la aplicación de pizza.

Para establecer estos valores:

1. En la página **Intenciones**, seleccione **ADMINISTRAR**.
1. En la página **Configuración de la aplicación**, registre el **Id. de aplicación**.
1. Seleccione **Recursos de Azure**.
1. Seleccione **Recurso de creación**.
1. En la pestaña **Recurso de creación**, registre la **clave principal**. Este valor es la clave de creación.
1. Registre la **Dirección URL del punto de conexión**. Este valor es el punto de conexión de creación.
