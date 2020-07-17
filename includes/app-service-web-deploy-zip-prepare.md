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
ms.openlocfilehash: 035399924216434de85865102db8838ea3fa15a3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85570153"
---
## <a name="create-a-project-zip-file"></a>Creación de un archivo ZIP de proyecto

>[!NOTE]
> Si descargó los archivos en un archivo ZIP, extráigalos primero. Por ejemplo, si descargó un archivo ZIP de GitHub, no puede implementar ese archivo tal cual. GitHub agrega directorios anidados adicionales que no funcionan con App Service. 
>

En la ventana de un terminal local, navegue hasta el directorio raíz del proyecto de la aplicación. 

Este directorio debería contener el archivo de entrada para la aplicación web como, por ejemplo, _index.html_, _index.php_ y _app.js_. También puede contener archivos de administración de paquetes como _project.json_, _composer.json_, _package.json_, _bower.json_ y _requirements.txt_.

A menos que desee que App Service ejecute la automatización de implementación automáticamente, ejecute todas las tareas de compilación (por ejemplo, `npm`, `bower`, `gulp`, `composer` y `pip`) y asegúrese de que tiene todos los archivos que necesita para ejecutar la aplicación. Este paso es necesario si desea [ejecutar su paquete directamente](../articles/app-service/deploy-run-package.md).

Cree un archivo ZIP con todo el contenido del proyecto. En el caso de los proyectos de `dotnet`, esta carpeta es la carpeta de salida del comando `dotnet publish`. El siguiente comando usa la herramienta predeterminada de su terminal:

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

