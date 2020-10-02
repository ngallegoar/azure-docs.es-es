---
title: 'Desarrollo Agile de proyectos de ciencia de datos: Team Data Science Process'
description: Ejecute un proyecto de ciencia de datos de forma sistemática, con control de versiones y en colaboración dentro de un equipo de proyecto mediante el proceso de ciencia de datos en equipo.
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 113c11b749b42bbaf9e5e40cdf63623288997a92
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2020
ms.locfileid: "89440293"
---
# <a name="agile-development-of-data-science-projects"></a>Desarrollo de Agile de proyectos de ciencia de datos

En este documento describe la forma en que los desarrolladores pueden ejecutar un proyecto de ciencia de datos de forma sistemática, con control de versiones y en colaboración dentro de un equipo de proyecto mediante el [Proceso de ciencia de los datos en equipos](overview.md) (TDSP). El proceso de ciencia de datos en equipo es una plataforma desarrollada por Microsoft que proporciona una secuencia estructurada de actividades para ejecutar de forma eficaz soluciones de análisis predictivos y basadas en la nube. Consulte [Roles y tareas del proceso de ciencia de datos en equipo](roles-tasks.md) para ver un esquema de los roles y las tareas de las que se ocupa un equipo de ciencia de datos que sigue este proceso como estándar. 

En este artículo se incluyen instrucciones acerca de cómo: 

- Realizar el *planeamiento de sprint* para los elementos de trabajo implicados en un proyecto.
- Agregar *elementos de trabajo* a los sprints.
- Crear y usar una *plantilla de elemento de trabajo derivada de Agile* que se alinee específicamente con las fases del ciclo de vida del proceso de ciencia de datos en equipo.

En las instrucciones siguientes se detallan los pasos necesarios para configurar un entorno de equipo de proceso de ciencia de datos en equipo (TDSP) mediante Azure Boards y Azure Repos en Azure DevOps. Las instrucciones usan Azure DevOps porque así es cómo se implementa TDSP en Microsoft. Si su grupo usa una plataforma de hospedaje de código diferente, las tareas del responsable de equipo no suelen cambiar, pero la forma de realizar las tareas es distinta. Por ejemplo, la vinculación de un elemento de trabajo con una rama de Git podría no ser igual con GitHub que con Azure Repos.

En la siguiente ilustración se muestra un flujo de trabajo habitual de planeación, codificación y control de origen de sprint para un proyecto de ciencia de datos:

![Proceso de ciencia de los datos en equipos](./media/agile-development/1-project-execute.png)

##  <a name="work-item-types"></a><a name='Terminology-1'></a>Tipos de elementos de trabajo

En la plataforma de planeamiento de sprint de TDSP, hay cuatro tipos de *elementos de trabajo* usados frecuentemente: *Características*, *Casos de usuario*, *Tareas* y *Errores*. El trabajo pendiente de todos los elementos de trabajo se encuentra en el nivel de proyecto, no en el nivel del repositorio de Git. 

Las siguientes son las definiciones de los tipos de elementos de trabajo:

- **Característica**: una característica se corresponde a una interacción de proyecto. Las diferentes interacciones con un cliente son distintas características, y es mejor considerar las diversas fases de un proyecto como características diferentes. Si elige un esquema como *\<ClientName>-\<EngagementName>* para denominar las características, puede reconocer fácilmente el contexto del proyecto y la involucración a partir de los propios nombres.
  
- **Caso de usuario**: los casos de usuario son elementos de trabajo necesarios para completar íntegramente una característica. Los siguientes son algunos ejemplos de casos de usuario:
  - Obtener datos 
  - Exploración de datos 
  - Generación de características
  - Creación de modelos
  - Modelos operativos 
  - Reciclaje de modelos
  
- **Tarea**: las tareas son elementos de trabajo asignables que deben realizarse para completar un caso de usuario específico. Por ejemplo, las tareas del caso de usuario *Obtener datos* podrían ser:
  - Obtener credenciales de SQL Server
  - Carga de datos en Azure Synapse Analytics
  
- **Error**: los errores son problemas en el código existente o en los documentos que se deben corregir para completar una tarea. Si los errores se deben a que faltan elementos de trabajo, pueden escalarse para convertirlos en casos de usuario o tareas. 

Los científicos de datos pueden sentirse más cómodos si usan una plantilla de Agile que reemplace las características, casos de usuario y tareas por fases y subfases del ciclo de vida de TDSP. Para crear una plantilla derivada de Agile que se alinee específicamente con las fases del ciclo de vida del proceso de ciencia de datos en equipo, consulte [Uso de una plantilla de trabajo de TDSP de Agile](#set-up-agile-dsp-6).

> [!NOTE]
> El proceso de ciencia de datos en equipo toma prestados los conceptos de características, casos de usuario, tareas y errores de la administración de código de software (SCM). Los conceptos del TDSP pueden diferir ligeramente de sus definiciones de SCM convencionales.

## <a name="plan-sprints"></a><a name='SprintPlanning-2'></a>Planeamiento de sprints

Muchos científicos de datos interactúan con varios proyectos, que pueden tardar meses en completarse y progresan a distinto ritmo. El planeamiento de sprint es útil para la priorización del proyecto y el planeamiento y asignación de recursos. En Azure Boards, puede crear, administrar y realizar el seguimiento fácilmente de los elementos de trabajo de sus proyectos, y llevar a cabo el planeamiento de sprints para garantizar que los proyectos avanzan de la forma prevista.

Para más información sobre el planeamiento de sprints, consulte [Sprints de Scrum](https://en.wikipedia.org/wiki/Scrum_(software_development)#Sprint). 

Para más información sobre el planeamiento de sprints en Azure Boards, consulte [Asignación de elementos de trabajo pendiente a un sprint](/azure/devops/boards/sprints/assign-work-sprint). 

## <a name="add-a-feature-to-the-backlog"></a><a name='AddFeature-3'></a>Adición de una característica al trabajo pendiente 

Después de crear el proyecto y el repositorio de código del proyecto, puede agregar una característica al trabajo pendiente para representar el trabajo del proyecto.

1. En la página del proyecto, seleccione **Paneles** > **Trabajos pendientes** en el panel de navegación de la izquierda. 
   
1. En la pestaña **Trabajo pendiente**, si el tipo de elemento de trabajo de la barra superior es **Casos**, en la lista desplegable seleccione **Características**. A continuación, seleccione **Nuevo elemento de trabajo**.
   
   ![Seleccionar Nuevo elemento de trabajo](./media/agile-development/2-sprint-team-overview.png)
   
1. Escriba un título para la característica, normalmente el nombre del proyecto y, a continuación, seleccione **Agregar a la parte superior**. 
   
   ![Escribir un título y seleccionar Agregar a la parte superior](./media/agile-development/3-sprint-team-add-work.png)
   
1. En la lista **Trabajo pendiente**, seleccione y abra la nueva característica. Rellene la descripción, asigne un miembro del equipo y establezca los parámetros de planeamiento. 
   
   También puede vincular la característica al repositorio de código de Azure Repos del proyecto seleccionando **Agregar vínculo** en la sección **Desarrollo**. 
   
   Después de editar la característica, seleccione **Guardar y cerrar**.
   
   ![Editar la característica y seleccionar Guardar y cerrar](./media/agile-development/3a-add-link-repo.png)

## <a name="add-a-user-story-to-the-feature"></a><a name='AddStoryunderfeature-4'></a>Adición de un caso de usuario a la característica 

En la característica, puede agregar casos de usuario para describir los pasos principales necesarios para completar el proyecto. 

Para agregar un nuevo caso de usuario a una característica:

1. En la pestaña **Trabajo pendiente**, seleccione **+** a la izquierda de la característica. 
   
   ![Agregar un nuevo caso de usuario en la característica](./media/agile-development/4-sprint-add-story.png)
   
1. Asigne un título al caso de usuario y edite detalles como la asignación, el estado, la descripción, los comentarios, el planeamiento y la prioridad. 
   
   También puede vincular el caso de usuario a una rama del repositorio de código de Azure Repos del proyecto seleccionando **Agregar vínculo** en la sección **Desarrollo**. Seleccione el repositorio y la rama a la que desea vincular el elemento de trabajo y, a continuación, seleccione **Aceptar**.
   
   ![Agregar vínculo](./media/agile-development/5-sprint-edit-story.png)
   
1. Cuando haya terminado de editar el caso de usuario, seleccione **Guardar y cerrar**. 

## <a name="add-a-task-to-a-user-story"></a><a name='AddTaskunderstory-5'></a>Adición de una tarea a un caso de usuario 

Las tareas son pasos detallados específicos necesarios para completar cada caso de usuario. Una vez completadas todas las tareas de un caso de usuario, se debe completar también el caso. 

Para agregar una tarea a un caso de usuario, seleccione **+** junto al elemento de caso de usuario y seleccione **Tarea**. Rellene el título y otra información de la tarea.

![Adición de una tarea a un caso de usuario](./media/agile-development/7-sprint-add-task.png)

Una vez creadas las características, los casos de usuario y las tareas, puede verlo todo en las vistas **Trabajos pendientes** o **Paneles** para realizar el seguimiento de su estado.

![Vista Trabajos pendientes](./media/agile-development/8-sprint-backlog-view.png)

![Vista Paneles](./media/agile-development/8a-sprint-board-view.png)

## <a name="use-an-agile-tdsp-work-template"></a><a name='set-up-agile-dsp-6'></a>Uso de una plantilla de trabajo de TDSP de Agile

Los científicos de datos pueden sentirse más cómodos si usan una plantilla de Agile que reemplace las características, casos de usuario y tareas por fases y subfases del ciclo de vida de TDSP. En Azure Boards, puede crear una plantilla derivada de Agile que use fases del ciclo de vida de TDSP para crear y realizar el seguimiento de los elementos de trabajo. Los pasos siguientes describen cómo configurar una plantilla de proceso de Agile específica de ciencia de datos y cómo crear elementos de trabajo de ciencia de datos basados en la plantilla.

### <a name="set-up-an-agile-data-science-process-template"></a>Configuración de una plantilla de proceso de ciencia de datos de Agile

1. En la página principal de la organización de Azure DevOps, seleccione **Configuración de la organización** en el panel de navegación izquierdo. 
   
1. En el panel de navegación izquierdo **Configuración de la organización**, en **Paneles**, seleccione **Proceso**. 
   
1. En el panel **Todos los procesos**, seleccione **...** junto a **Agile** y, a continuación, seleccione **Crear proceso heredado**.
   
   ![Crear proceso heredado desde Agile](./media/agile-development/10-settings.png) 
   
1. En el cuadro de diálogo **Crear proceso heredado desde Agile**, escriba el nombre *AgileDataScienceProcess* y seleccione **Crear proceso**.
   
   ![Crear proceso AgileDataScienceProcess](./media/agile-development/11-agileds.png)
   
1. En **Todos los procesos**, seleccione el nuevo proceso **AgileDataScienceProcess**. 
   
1. En la pestaña **Tipos de elementos de trabajo**, deshabilite **Epopeya**, **Característica**, **Caso de usuario** y **Tarea**, para lo que debe seleccionar **...** junto a cada elemento y, a continuación, seleccionar **Deshabilitar**. 
   
   ![Deshabilitar tipos de elementos de trabajo](./media/agile-development/12-disable.png)
   
1. En **Todos los procesos**, seleccione la pestaña **Niveles de trabajo pendiente**. En **Trabajos pendientes en cartera**, seleccione **...** junto a **Epopeya (deshabilitado)** y, a continuación, seleccione **Editar/Cambiar nombre**. 
   
1. En el cuadro de diálogo **Editar nivel de trabajo pendiente**:
   1. En **Nombre**, reemplace **Epopeya** por *Proyectos de TDSP*. 
   1. En **Tipos de elementos de trabajo en este nivel de trabajo pendiente**, seleccione **Nuevo tipo de elemento de trabajo**, escriba *Proyecto de TDSP* y seleccione **Agregar**. 
   1. En **Tipo de elemento de trabajo predeterminado**, seleccione **Proyecto de TDSP** en el menú desplegable. 
   1. Seleccione **Guardar**.
   
   ![Establecer el nivel de trabajo pendiente en cartera](./media/agile-development/13-rename.png)  
   
1. Siga los mismos pasos para cambiar el nombre de **Características** por *Fases de TDSP* y agregue los siguientes nuevos tipos de elementos de trabajo:
   
   - *Conocimiento del negocio*
   - *Adquisición de datos*
   - *Modelado*
   - *Implementación*
   
1. En **Trabajo pendiente de requisito**, cambie el nombre de **Casos** a *Subfases de TDSP*, agregue el nuevo tipo de elemento de trabajo *Subfase de TDSP* y establezca el tipo de elemento de trabajo predeterminado en **Subfase de TDSP**.
   
1. En **Iteración de trabajo pendiente**, agregue el nuevo tipo de elemento de trabajo *Tarea de TDSP* y establézcalo como tipo de elemento de trabajo predeterminado. 
   
Después de completar los pasos, los niveles de trabajo pendiente deberían tener el aspecto siguiente:
   
 ![Niveles de trabajo pendiente de la plantilla de TDSP](./media/agile-development/14-template.png)  

### <a name="create-agile-data-science-process-work-items"></a>Creación de elemento de trabajo de proceso de ciencia de datos de Agile

Puede usar la plantilla de proceso de ciencia de datos para crear proyectos de TDSP y realizar el seguimiento de los elementos de trabajo que corresponden a fases del ciclo de vida de TDSP.

1. En la página principal de la organización de Azure DevOps, seleccione **Nuevo proyecto**. 
   
1. En el cuadro de diálogo **Crear proyecto nuevo**, asigne un nombre al proyecto y, a continuación, seleccione **Opciones avanzadas**. 
   
1. En **Proceso de elemento de trabajo**, seleccione **AgileDataScienceProcess** en el menú desplegable y, a continuación, seleccione **Crear**.
   
   ![Crear un proyecto TDSP](./media/agile-development/15-newproject.png)
   
1. En el nuevo proyecto, seleccione **Paneles** > **Trabajos pendientes** en el panel de navegación de la izquierda.
   
1. Para que los proyectos de TDSP sean visibles, seleccione el icono de **Definir la configuración del equipo**. En la pantalla **Configuración**, active la casilla **Proyectos de TDSP** y, a continuación, seleccione **Guardar y cerrar**.
   
   ![Activar la casilla Proyectos de TDSP](./media/agile-development/16-enabledsprojects1.png)
   
1. Para crear un proyecto de TDSP específico de ciencia de datos, seleccione **Proyectos de TDSP** en la barra superior y, a continuación, seleccione **Nuevo elemento de trabajo**. 
   
1. En el menú emergente, asigne un nombre al elemento de trabajo del proyecto de TDSP y seleccione **Agregar a la parte superior**.
   
   ![Crear un elemento de trabajo de proyecto de ciencia de datos](./media/agile-development/17-dsworkitems0.png)
   
1. Para agregar un elemento de trabajo en el proyecto de TDSP, seleccione **+** junto al proyecto y, a continuación, seleccione el tipo de elemento de trabajo que desea crear. 
   
   ![Seleccionar el tipo de elemento de trabajo de ciencia de datos](./media/agile-development/17-dsworkitems1.png)
   
1. Rellene los detalles en el nuevo elemento de trabajo y seleccione **Guardar y cerrar**.
   
1. Siga seleccionando **+** junto a los elementos de trabajo para agregar nuevas fases, subfases y tareas de TDSP. 
   
A continuación se muestra un ejemplo del aspecto que deberían tener los elementos de trabajo del proyecto de ciencia de datos en la vista **Trabajos pendientes**:

![18](./media/agile-development/18-workitems1.png)


## <a name="next-steps"></a>Pasos siguientes

En [Codificación colaborativa con GIT](collaborative-coding-with-git.md) se describe cómo desarrollar código colaborativo para proyectos de ciencia de datos con Git como marco de desarrollo de código compartido y cómo vincular estas actividades de codificación al trabajo planeado con el proceso de Agile.

El artículo [Tutoriales de ejemplo](walkthroughs.md) proporciona una lista de tutoriales sobre escenarios específicos con vínculos y descripciones en miniatura. Los escenarios vinculados muestran cómo combinar servicios y herramientas en la nube y locales en flujos de trabajo o canalizaciones para crear aplicaciones inteligentes.
  
Recursos adicionales sobre procesos de Agile:

- [Proceso de Agile](/azure/devops/boards/work-items/guidance/agile-process)
  
- [Flujo de trabajo y tipos de elemento de trabajo de procesos de Agile](/azure/devops/boards/work-items/guidance/agile-process-workflow)

