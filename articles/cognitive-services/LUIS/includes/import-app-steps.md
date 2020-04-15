---
title: Pasos para importar una aplicación
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/30/2020
ms.author: diberry
ms.openlocfilehash: b7b8befa0f5871b65b9b5621bfb99c659bf07235
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2020
ms.locfileid: "80422755"
---
1. En el [portal de LUIS en versión preliminar](https://preview.luis.ai), en la página **Mis aplicaciones**, seleccione **+ New app for conversation** (Nueva aplicación para conversación) y, a continuación, **Import as JSON** (Importar como JSON). Busque el archivo JSON guardado en el paso anterior. No es necesario cambiar el nombre de la aplicación. Seleccione **Listo**

1. Desde la sección **Administrar**, en la pestaña **Versiones**, seleccione la versión `0.1`, luego seleccione **Clonar** para clonar la versión y asígnele un nombre de 10 caracteres de `ml-entity`. A continuación, seleccione **Listo** para finalizar el proceso de clonación. Dado que el nombre de la versión se usa como parte de la ruta de la dirección URL, el nombre no puede contener ningún carácter que no sea válido en una dirección URL.

    > [!TIP]
    > La clonación en una nueva versión es un procedimiento recomendado antes de modificar la aplicación. Cuando finalice un cambio en una versión, exporte la versión (como archivo .json o .lu), y compruebe el archivo en el sistema de control de código fuente.

1. Seleccione **Compilar** y después **Intenciones** para ver las intenciones, los principales bloques de creación de una aplicación de LUIS.

    ![Cambie de la página Versiones a la página Intenciones.](../media/tutorial-machine-learned-entity/new-version-imported-app.png)