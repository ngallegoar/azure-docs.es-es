---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 01/14/2020
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 6b5aa4f409b8c2f5a9125ab01e8587f4ac9c4ee5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75945142"
---
## <a name="create-a-project-zip-file"></a>Creación de un archivo ZIP de proyecto

>[!NOTE]
> Si descargó los archivos en un archivo ZIP, extráigalos primero. Por ejemplo, si descargó un archivo ZIP de GitHub, no puede implementar ese archivo tal cual. GitHub agrega directorios anidados adicionales que no funcionan con App Service. 
>

En la ventana de un terminal local, navegue hasta el directorio raíz del proyecto de la aplicación. 

Este directorio debería contener el archivo de entrada para la aplicación web como, por ejemplo, _index.html_, _index.php_ y _app.js_. También puede contener archivos de administración de paquetes como _project.json_, _composer.json_, _package.json_, _bower.json_ y _requirements.txt_.

A menos que desee que App Service ejecute la automatización de implementación automáticamente, ejecute todas las tareas de compilación (por ejemplo, `npm`, `bower`, `gulp`, `composer` y `pip`) y asegúrese de que tiene todos los archivos que necesita para ejecutar la aplicación. Este paso es necesario si desea [ejecutar su paquete directamente](../articles/app-service/deploy-run-package.md).

Cree un archivo ZIP con todo el contenido del proyecto. El siguiente comando usa la herramienta predeterminada de su terminal:

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

