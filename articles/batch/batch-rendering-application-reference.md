---
title: Uso de aplicaciones de representación
description: Uso de aplicaciones de representación con Azure Batch En este artículo se proporciona una breve descripción de cómo ejecutar cada aplicación de representación.
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: how-to
ms.openlocfilehash: ace3fe7aee6b9ffc7226448b455bcfea1f931458
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2020
ms.locfileid: "85964879"
---
# <a name="rendering-applications"></a>Aplicaciones de representación

Las aplicaciones de representación se usan mediante la creación de tareas y trabajos de Batch. La propiedad de línea de comandos de la tarea especifica la línea de comandos y los parámetros adecuados.  La forma más fácil de crear las tareas de un trabajo es usar las plantillas de Batch Explorer como se especifica en [este artículo](./batch-rendering-using.md#using-batch-explorer).  Las plantillas se pueden ver y se pueden modificar las versiones creadas, en caso de que sea necesario.

En este artículo se proporciona una breve descripción de cómo ejecutar cada aplicación de representación.

## <a name="rendering-with-autodesk-3ds-max"></a>Representación con Autodesk 3ds Max

### <a name="renderer-support"></a>Compatibilidad con el representador

Además de los representadores incorporados a 3ds Max, los representadores siguientes están disponibles en las imágenes de máquina virtual de representación y el archivo de escenas de 3ds Max puede hacer referencia a ellos:

* Autodesk Arnold
* V-Ray de Chaos Group

### <a name="task-command-line"></a>Línea de comandos de tarea

Invoque la aplicación `3dsmaxcmdio.exe` para realizar la representación mediante línea de comandos en un nodo del grupo.  Esta aplicación está en la ruta de acceso cuando se ejecuta la tarea. La aplicación `3dsmaxcmdio.exe` tiene los mismos parámetros disponibles que la aplicación `3dsmaxcmd.exe` y se pueden encontrar en la [documentación de ayuda de 3ds Max](https://help.autodesk.com/view/3DSMAX/2018/ENU/) [sección Rendering (Representación) | Command-Line Rendering (Representación mediante línea de comandos)].

Por ejemplo:

```
3dsmaxcmdio.exe -v:5 -rfw:0 -start:{0} -end:{0} -bitmapPath:"%AZ_BATCH_JOB_PREP_WORKING_DIR%\sceneassets\images" -outputName:dragon.jpg -w:1280 -h:720 "%AZ_BATCH_JOB_PREP_WORKING_DIR%\scenes\dragon.max"
```

Notas:

* Es preciso tener mucho cuidado para asegurarse de que se encuentran los archivos de recursos.  Asegúrese de que las rutas de acceso son correctas y relativas, para lo que debe usar la ventana de **seguimiento de recursos** o usar el parámetro `-bitmapPath` en la línea de comandos.
* Vea si hay problemas con la representación, como la incapacidad para encontrar recursos. Para ello debe comprobar el archivo `stdout.txt` escrito por 3ds Max cuando se ejecuta la tarea.

### <a name="batch-explorer-templates"></a>Plantillas de Batch Explorer

A las plantillas de grupo y de trabajo se puede acceder desde el **galería** de Batch Explorer.  Los archivos de origen de plantilla están disponibles en el [repositorio de datos de Batch Explorer en GitHub](https://github.com/Azure/BatchExplorer-data/tree/master/ncj/3dsmax).

## <a name="rendering-with-autodesk-maya"></a>Representación con Autodesk Maya

### <a name="renderer-support"></a>Compatibilidad con el representador

Además de los representadores incorporados a Maya, los representadores siguientes están disponibles en las imágenes de máquina virtual de representación y el archivo de escenas de 3ds Max puede hacer referencia a ellos:

* Autodesk Arnold
* V-Ray de Chaos Group

### <a name="task-command-line"></a>Línea de comandos de tarea

El representador de línea de comandos `renderer.exe` se utiliza en la línea de comandos de la tarea. El representador de línea de comandos se documenta en la [ayuda de Maya](https://help.autodesk.com/view/MAYAUL/2018/ENU/?guid=GUID-EB558BC0-5C2B-439C-9B00-F97BCB9688E4).

En el ejemplo siguiente, se usa una tarea de preparación del trabajo para copiar tanto los archivos de escena como los recursos en el directorio de trabajo de la preparación de trabajo, se usa una carpeta de salida para almacenar la imagen de representación y se representa el marco 10.

```
render -renderer sw -proj "%AZ_BATCH_JOB_PREP_WORKING_DIR%" -verb -rd "%AZ_BATCH_TASK_WORKING_DIR%\output" -s 10 -e 10 -x 1920 -y 1080 "%AZ_BATCH_JOB_PREP_WORKING_DIR%\scene-file.ma"
```

En el caso de la representación de V-Ray, lo habitual es que el archivo de escena de Maya especifique V-Ray como representador.  También se puede especificar en la línea de comandos:

```
render -renderer vray -proj "%AZ_BATCH_JOB_PREP_WORKING_DIR%" -verb -rd "%AZ_BATCH_TASK_WORKING_DIR%\output" -s 10 -e 10 -x 1920 -y 1080 "%AZ_BATCH_JOB_PREP_WORKING_DIR%\scene-file.ma"
```

En el caso de la representación de Arnold, lo habitual es que el archivo de escena de Maya especifique Arnold como representador.  También se puede especificar en la línea de comandos:

```
render -renderer arnold -proj "%AZ_BATCH_JOB_PREP_WORKING_DIR%" -verb -rd "%AZ_BATCH_TASK_WORKING_DIR%\output" -s 10 -e 10 -x 1920 -y 1080 "%AZ_BATCH_JOB_PREP_WORKING_DIR%\scene-file.ma"
```

### <a name="batch-explorer-templates"></a>Plantillas de Batch Explorer

A las plantillas de grupo y de trabajo se puede acceder desde el **galería** de Batch Explorer.  Los archivos de origen de plantilla están disponibles en el [repositorio de datos de Batch Explorer en GitHub](https://github.com/Azure/BatchExplorer-data/tree/master/ncj/maya).

## <a name="next-steps"></a>Pasos siguientes

Use las plantillas de grupo y de trabajo desde el [repositorio de datos de GitHub](https://github.com/Azure/BatchExplorer-data/tree/master/ncj) mediante Batch Explorer.  Cuando sea necesario, cree plantillas nuevas o modifique una de las que se proporcionan.
