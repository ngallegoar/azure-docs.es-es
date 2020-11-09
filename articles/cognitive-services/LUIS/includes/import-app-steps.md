---
title: Pasos para importar una aplicación
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: include
ms.date: 10/16/2020
ms.openlocfilehash: b70a22e502de5441b5c91a8f33357375834f54e1
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93130757"
---
1. En el [portal de LUIS](https://www.luis.ai), en la página **Mis aplicaciones** , seleccione **+ New app for conversation** (Nueva aplicación para conversación) y, a continuación, **Importar como JSON**. Busque el archivo JSON guardado en el paso anterior. No es necesario cambiar el nombre de la aplicación. Seleccione **Listo**

1. En la sección **Administrar** , en la pestaña **Versiones** , seleccione la versión `0.1`, luego seleccione **Clonar** para clonar la versión y asígnele el nuevo nombre de `ml-entity`. A continuación, seleccione **Listo** para finalizar el proceso de clonación. Dado que el nombre de la versión se usa como parte de la ruta de la dirección URL, el nombre no puede contener ningún carácter que no sea válido en una dirección URL.

    > [!TIP]
    > La clonación en una nueva versión es un procedimiento recomendado antes de modificar la aplicación. Cuando finalice un cambio en una versión, exporte la versión (como archivo .json o .lu), y compruebe el archivo en el sistema de control de código fuente.

1. Seleccione **Compilar** en la parte superior de la pantalla y haga clic en **Intenciones** en el menú de la izquierda. Verá las intenciones de la aplicación, que son los principales bloques de creación de una aplicación de LUIS.
