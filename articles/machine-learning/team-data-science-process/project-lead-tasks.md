---
title: Tareas para el líder del proyecto en el proceso de ciencia de datos en equipo
description: Un tutorial detallado de las tareas de un responsable de proyecto en un equipo de proceso de ciencia de datos en equipo
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 9d9b2546553e03d1555cf4c587d699d9a4ea7e51
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96000987"
---
# <a name="project-lead-tasks-in-the-team-data-science-process"></a>Tareas para el líder del proyecto en el proceso de ciencia de datos en equipo

En este artículo se describen las tareas que un *responsable de proyecto* debe realizar para configurar un repositorio para su equipo de proyecto en el [proceso de ciencia de datos en equipo](overview.md) (TDSP). El proceso de ciencia de datos en equipo es una plataforma desarrollada por Microsoft que proporciona una secuencia estructurada de actividades para ejecutar de forma eficaz soluciones de análisis predictivos y basadas en la nube. Este proceso está diseñado para ayudar a mejorar la colaboración y el aprendizaje en equipo. Para ver un esquema de los roles del personal y las tareas asociadas de las que se ocupa un equipo de ciencia de datos que sigue este proceso como estándar, consulte [Roles y tareas del proceso de ciencia de datos en equipo](roles-tasks.md).

Un responsable de proyecto administra las actividades diarias de los científicos de datos en un proyecto de ciencia de datos de este proceso. En el diagrama siguiente se muestra el flujo de trabajo de las tareas del responsable de proyecto:

![Flujo de trabajo de las tareas del responsable del proyecto](./media/project-lead-tasks/project-leads-1-tdsp-creating-projects.png)

Esta tutorial abarca lo siguiente: Paso 1: Creación de un repositorio del proyecto, y Paso 2: Inicialización del repositorio de proyectos desde el repositorio ProjectTemplate del equipo. 

Para el Paso 3: Creación del elemento de trabajo Feature para el proyecto, y el Paso 4: Incorporación de casos a las fases del proyecto, consulte [Desarrollo de Agile de proyectos de ciencia de datos](agile-development.md).

Para el paso 5: Creación y personalización de los recursos de almacenamiento y análisis, y uso compartido de estos, consulte, si es necesario, [Creación de datos de equipo y recursos de análisis](team-lead-tasks.md#create-team-data-and-analytics-resources).

Para el Paso 6: Configuración del control de seguridad del repositorio del proyecto, consulte [Adición de miembros de equipo y configuración de permisos](team-lead-tasks.md#add-team-members-and-configure-permissions).

> [!NOTE] 
> En este artículo se utiliza Azure Repos para configurar un proyecto de TDSP, ya que se explica cómo implementar TDSP en Microsoft. Si el equipo usa otras plataformas de hospedaje de código, las tareas del responsable de proyecto son las mismas, pero la manera de realizarlas puede ser diferente.

## <a name="prerequisites"></a>Requisitos previos

En este tutorial se da por supuesto que el [administrador del grupo](group-manager-tasks.md) y el [responsable de equipo](team-lead-tasks.md) han configurado los siguientes recursos y permisos:

- La **organización** de Azure DevOps para la unidad de datos
- Un **proyecto** de equipo para el equipo de ciencia de datos
- Plantillas de equipo y **repositorios** de utilidades
- **Permisos** en la cuenta de la organización para que pueda crear y editar repositorios para el proyecto

Para clonar los repositorios y modificar el contenido en la máquina local o en Data Science Virtual Machine (DSVM), o configurar el almacenamiento de Azure File Storage y montarlo en la DSVM, también necesita tener en cuenta esta lista de comprobación:

- Suscripción a Azure.
- Git instalado en la máquina. Si usa una DSVM, GIT está preinstalado. En caso contrario, consulte el [apéndice de plataformas y herramientas](platforms-and-tools.md#appendix).
- Si desea usar la DSVM, la DSVM de Windows o Linux creada y configurada en Azure. Para más información e instrucciones, consulte la [documentación de Data Science Virtual Machine](../data-science-virtual-machine/index.yml).
- Para una DSVM de Windows, debe tener [Git Credential Manager (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) instalado en la máquina. En el archivo *README.md*, desplácese a la sección **Descargar e instalar** y haga clic en el **instalador más reciente**. Descargue el instalador *.exe* desde la página del instalador y ejecútelo. 
- En el caso de una DSVM de Linux, se configura una clave pública SSH en la DSVM y se agrega en Azure DevOps. Para más información e instrucciones, consulte la sección acerca de cómo **crear una clave pública SSH** en el [apéndice de plataformas y herramientas](platforms-and-tools.md#appendix). 

## <a name="create-a-project-repository-in-your-team-project"></a>Creación de un repositorio de proyecto en el proyecto de equipo

Para crear un repositorio de proyecto en el proyecto **MyTeam** del equipo:

1. Vaya a la página **Resumen** del proyecto de equipo en *https:\//\<server name>/\<organization name>/\<team name>* , por ejemplo, **https:\//dev.azure.com/DataScienceUnit/MyTeam** y seleccione **Repositorios** en el panel de navegación izquierdo. 
   
1. Seleccione el nombre del repositorio en la parte superior de la página y, a continuación, seleccione **Nuevo repositorio** en la lista desplegable.
   
   ![Seleccionar Nuevo repositorio](./media/project-lead-tasks/project-leads-9-select-repos.png)
   
1. En el cuadro de diálogo **Crear un nuevo repositorio**, asegúrese de que **Git** esté seleccionado en **Tipo**. Escriba *DSProject1* en **Nombre del repositorio** y, a continuación, seleccione **Crear**.
   
   ![Crear repositorio](./media/project-lead-tasks/project-leads-3-create-project-repo-2.png)
   
1. Confirme que puede ver el nuevo repositorio **DSProject1** en la página de configuración del proyecto. 
   
   ![Repositorio del proyecto en la configuración del proyecto](./media/project-lead-tasks/project-leads-4-create-project-repo-3.png)

## <a name="import-the-team-template-into-your-project-repository"></a>Importación de la plantilla de equipo en el repositorio del proyecto

Si desea rellenar el repositorio del proyecto con el contenido del repositorio de plantillas del equipo:

1. En la página **Resumen** del proyecto de equipo, seleccione **Repositorios** en el panel de navegación de la izquierda. 
   
1. Seleccione el nombre del repositorio en la parte superior de la página y seleccione **DSProject1** en la lista desplegable.
   
1. En la página **DSProject1 está vacío**, seleccione **Importar**. 
   
   ![Seleccionar Importar](./media/project-lead-tasks/project-leads-5-create-project-repo-4.png)
   
1. En el cuadro de diálogo **Importar un repositorio GIT**, seleccione **Git** como **Tipo de origen** y escriba la dirección URL del repositorio **TeamTemplate** en **Dirección URL de clonación**. La dirección URL es *https:\//\<server name>/\<organization name>/\<team name>/_git/\<team template repository name>* . Por ejemplo: **https:\//dev.azure.com/DataScienceUnit/MyTeam/_git/TeamTemplate**. 
   
1. Seleccione **Import** (Importar). El contenido del repositorio de plantillas del equipo se importa en el repositorio de proyectos. 
   
   ![Importación del repositorio de plantillas del equipo](./media/project-lead-tasks/project-leads-6-create-project-repo-5.png)

Si tiene que personalizar el contenido del repositorio de proyectos para satisfacer las necesidades específicas de su proyecto, puede agregar, eliminar o modificar archivos y carpetas del repositorio. Puede trabajar directamente en Azure Repos, o clonar el repositorio en la máquina local o en DSVM, realizar cambios y confirmar e instalar las actualizaciones en el repositorio del proyecto compartido. Siga las instrucciones que se encuentran en [Personalización del contenido de los repositorios del equipo](team-lead-tasks.md#customize-the-contents-of-the-team-repositories).

## <a name="next-steps"></a>Pasos siguientes

Estos son vínculos a descripciones detalladas de los roles y tareas definidas por el proceso de ciencia de datos en equipo:

- [Tareas del administrador de grupo en un equipo de ciencia de datos](group-manager-tasks.md)
- [Tareas del responsable de equipo en un equipo de ciencia de datos](team-lead-tasks.md)
- [Tareas de colaboradores individuales en un equipo de ciencia de datos](project-ic-tasks.md)