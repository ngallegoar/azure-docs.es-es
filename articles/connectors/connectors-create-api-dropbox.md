---
title: Conexión con Dropbox
description: Automatice las tareas y los flujos de trabajo que actualizan y administran archivos en Dropbox mediante Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/01/2019
tags: connectors
ms.openlocfilehash: 8f54f832884b172761f62b16db29d2f0abd0dd46
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75665758"
---
# <a name="upload-and-manage-files-in-dropbox-by-using-azure-logic-apps"></a>Carga y administración de archivos en Dropbox mediante Azure Logic Apps

Con el conector para Dropbox y Azure Logic Apps, puede crear flujos de trabajo automatizados que carguen y administren archivos en su cuenta de Dropbox. 

En este artículo se muestra cómo conectarse a Dropbox desde su aplicación lógica y, a continuación, agregar el desencadenador **Cuando se crea un archivo** y la acción **Obtener contenido de archivo mediante la ruta de acceso** de Dropbox.

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Una [cuenta de Dropbox](https://www.dropbox.com/), que se puede registrar de forma gratuita. Las credenciales de la cuenta son necesarias para crear una conexión entre la aplicación lógica y la cuenta de Dropbox.

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md). En este ejemplo, necesita una aplicación lógica en blanco.

## <a name="add-trigger"></a>Incorporación de un desencadenador

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. En el cuadro de búsqueda, elija **Todas**. En el cuadro de búsqueda, escriba "dropbox" para el filtro.
En la lista de desencadenadores, seleccione este desencadenador: **Cuando se crea un archivo**

   ![Selección del desencadenador de Dropbox](media/connectors-create-api-dropbox/select-dropbox-trigger.png)

1. Inicie sesión con sus credenciales de cuenta de Dropbox y autorice el acceso a los datos de Dropbox para Azure Logic Apps.

1. Proporcione la información necesaria para el desencadenador. 

   En este ejemplo, seleccione la carpeta en la que se va a supervisar la creación de archivos. Para examinar sus carpetas, elija el icono de carpeta situado junto al cuadro **Carpeta**.

## <a name="add-action"></a>Agregar una acción

Agregue ahora una acción que obtenga el contenido de cualquier archivo nuevo.

1. En el desencadenador, elija **Next step** (Paso siguiente). 

1. En el cuadro de búsqueda, elija **Todas**. En el cuadro de búsqueda, escriba "dropbox" para el filtro.
En la lista de acciones, seleccione esta acción: **Obtener contenido de archivo mediante la ruta de acceso**

1. Si aún no ha autorizado a Azure Logic Apps para tener acceso a Dropbox, hágalo ahora.

1. Para ir a la ruta de acceso del archivo que desea usar, junto al cuadro **Ruta de acceso del archivo**, elija el botón de puntos suspensivos ( **...** ). 

   También puede hacer clic en el cuadro **Ruta de acceso del archivo** y, en la lista de contenido dinámico, seleccionar **Ruta de acceso del archivo**, cuyo valor está disponible como salida del desencadenador que agregó en la sección anterior.

1. Cuando haya terminado, guarde la aplicación lógica.

1. Para desencadenar su aplicación lógica, cree un nuevo archivo en Dropbox.

## <a name="connector-reference"></a>Referencia de conectores

Para obtener datos técnica, como los desencadenadores, las acciones y los límites, tal como lo describe el archivo Swagger del conector, consulte la [página de referencia del conector](/connectors/dropbox/).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)
