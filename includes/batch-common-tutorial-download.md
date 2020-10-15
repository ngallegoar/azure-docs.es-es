---
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 11/09/2018
ms.author: lahugh
ms.openlocfilehash: bd51eabcff412de4928c682683b2a69b3e82e601
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "67186665"
---
### <a name="retrieve-output-files"></a>Recuperación de archivos de salida

Puede usar Azure Portal para descargar los archivos MP3 de salida generados por las tareas de ffmpeg. 

1. Haga clic en **Todos los servicios** > **Cuentas de almacenamiento** y haga clic en el nombre de la cuenta de almacenamiento.
2. Haga clic en **Blobs** > *salida*.
3. Haga clic con el botón derecho en uno de los archivos MP3 de salida y, después, haga clic en **Descargar**. Para abrir o guardar el archivo, siga las indicaciones del explorador.

![Descargar un archivo de salida](./media/batch-common-tutorial-download/download.png)

Aunque no se muestra en este ejemplo, los archivos también se pueden descargar mediante programación desde los nodos de proceso o desde el contenedor de almacenamiento.
