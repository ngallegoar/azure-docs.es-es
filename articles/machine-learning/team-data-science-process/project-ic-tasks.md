---
title: Tareas para un colaborador individual en el proceso de ciencia de datos en equipos
description: Tutorial detallado de las tareas de un colaborador individual en un proyecto de equipo de ciencia de datos.
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 4ecb5fef9c9b14bde72de29a45e29d7e16131bd1
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93321911"
---
# <a name="tasks-for-an-individual-contributor-in-the-team-data-science-process"></a>Tareas para un colaborador individual en el proceso de ciencia de datos en equipos

En este tema se describen las tareas que realiza un *colaborador individual* para configurar un proyecto en el [proceso de ciencia de datos en equipo](overview.md) (TDSP). El objetivo es trabajar en un entorno de colaboración en equipo que se estandariza en el proceso de ciencia de datos en equipo (TDSP). Este proceso está diseñado para ayudar a mejorar la colaboración y el aprendizaje en equipo. En [Roles y tareas del proceso de ciencia de datos en equipo](roles-tasks.md) dispone de un esquema de los roles profesionales y las tareas asociadas de las que se ocupa un equipo de ciencia de datos que sigue el TDSP como estándar.

En el diagrama siguiente se muestran las tareas que los colaboradores individuales del proyecto (científicos de datos) realizan para configurar su entorno de equipo. Para obtener instrucciones sobre cómo ejecutar un proyecto de ciencia de datos en el proceso de ciencia de datos en equipo, consulte [Ejecución de proyectos de ciencia de datos](./agile-development.md). 

![Tareas de colaborador individual](./media/project-ic-tasks/project-ic-1-tdsp-data-scientist.png)

- **ProjectRepository** es el repositorio que el equipo del proyecto mantiene para compartir plantillas y recursos del proyecto.
- **TeamUtilities** es el repositorio de utilidades que el equipo mantiene específicamente para su uso propio. 
- **GroupUtilities** es el repositorio que el grupo mantiene para compartir utilidades con todo el grupo. 

> [!NOTE] 
> En este artículo se utiliza Azure Repos y una instancia de Data Science Virtual Machine (DSVM) para configurar un entorno de TDSP, ya que se explica cómo implementar TDSP en Microsoft. Si el equipo usa otras plataformas de desarrollo u hospedaje de código, las tareas del colaborador individual serán las mismas, pero la manera de realizarlas puede ser diferente.

## <a name="prerequisites"></a>Requisitos previos

En este tutorial se da por supuesto que los siguientes recursos y permisos están configurados por el [administrador de grupo](group-manager-tasks.md), el [responsable de equipo](team-lead-tasks.md) y el [responsable de proyecto](project-lead-tasks.md):

- La **organización** de Azure DevOps para la unidad de ciencia de datos
- Un **repositorio de proyecto** configurado por el responsable de proyecto para compartir plantillas y recursos de proyecto
- Los repositorios **GroupUtilities** y **TeamUtilities** configurados por el administrador de grupo y el responsable de equipo, si procede
- **Almacenamiento de archivos** de Azure configurado para los recursos compartidos de su equipo o proyecto, si procede
- **Permisos** para clonar y volver a enviar al repositorio del proyecto 

Para clonar los repositorios y modificar su contenido en la máquina local o en la DSVM, o montar el almacenamiento de archivos de Azure en la DSVM, debe tener en cuenta la lista siguiente:

- Suscripción a Azure.
- Git instalado en la máquina. Si usa una DSVM, GIT está preinstalado. En caso contrario, consulte el [apéndice de plataformas y herramientas](platforms-and-tools.md#appendix).
- Si desea usar la DSVM, la DSVM de Windows o Linux creada y configurada en Azure. Para más información e instrucciones, consulte la [documentación de Data Science Virtual Machine](../data-science-virtual-machine/index.yml).
- Para una DSVM de Windows, debe tener [Git Credential Manager (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) instalado en la máquina. En el archivo *README.md* , desplácese a la sección **Descargar e instalar** y haga clic en el **instalador más reciente**. Descargue el instalador *.exe* desde la página del instalador y ejecútelo. 
- En el caso de una DSVM de Linux, se configura una clave pública SSH en la DSVM y se agrega en Azure DevOps. Para más información e instrucciones, consulte la sección acerca de cómo **crear una clave pública SSH** en el [apéndice de plataformas y herramientas](platforms-and-tools.md#appendix). 
- La información de Azure Files Storage para cualquier almacenamiento de archivos de Azure que necesite montar en la DSVM. 

## <a name="clone-repositories"></a>Clonación de repositorios

Para trabajar con repositorios de forma local e insertar los cambios en los repositorios compartidos de proyecto y de equipo, primero debe copiar o *clonar* los repositorios en la máquina local. 

1. En Azure DevOps, vaya a la página Resumen del proyecto del equipo en *https:\//\<server name>/\<organization name>/\<team name>* , por ejemplo, **https:\//dev.azure.com/DataScienceUnit/MyTeam**.
   
1. Seleccione **Repos** en el panel de navegación izquierdo y, en la parte superior de la página, seleccione el repositorio que desea clonar.
   
1. En la página de repositorios, seleccione **Clonar** en la esquina superior derecha.
   
1. En el cuadro de diálogo **Clonar repositorio** , seleccione **HTTPS** para una conexión HTTP o **SSH** para una conexión SSH y copie la dirección URL de clonación en la **Línea de comandos** del Portapapeles.
   
   ![Clonación de repositorio](./media/project-ic-tasks/clone.png)
   
1. En la máquina local o en la DSVM, cree los directorios siguientes:
   
   - Para Windows: **C:\GitRepos**
   - Para Linux: **$home/GitRepos**
   
1. Cambie al directorio que ha creado.
   
1. En Git Bash, ejecute el comando `git clone <clone URL>` para cada repositorio que quiera clonar. 
   
   Por ejemplo, el siguiente comando clona el repositorio **TeamUtilities** en el directorio *MyTeam* en la máquina local. 
   
   **Conexión HTTPS:**
   
   ```bash
   git clone https://DataScienceUnit@dev.azure.com/DataScienceUnit/MyTeam/_git/TeamUtilities
   ```
   
   **Conexión SSH:**
   
   ```bash
   git clone git@ssh.dev.azure.com:v3/DataScienceUnit/MyTeam/TeamUtilities
   ```
   
1. Confirme que puede ver las carpetas de los repositorios clonados en el directorio del proyecto local.
   
   ![Tres carpetas de repositorio local](./media/project-ic-tasks/project-ic-5-three-repo-cloned-to-ic-linux.png)

## <a name="mount-azure-file-storage-to-your-dsvm"></a>Montaje del almacenamiento de archivos de Azure en la DSVM

Si su equipo o proyecto tiene recursos compartidos en el almacenamiento de archivos de Azure, monte el almacenamiento de archivos en la máquina local o en la DSVM. Siga las instrucciones de [Montaje de Azure File Storage en la máquina local o DSVM](team-lead-tasks.md#mount-azure-file-storage-on-your-local-machine-or-dsvm).

## <a name="next-steps"></a>Pasos siguientes

Estos son vínculos a descripciones detalladas de los roles y tareas definidas por el proceso de ciencia de datos en equipo:

- [Tareas del administrador de grupo en un equipo de ciencia de datos](group-manager-tasks.md)
- [Tareas del responsable de equipo en un equipo de ciencia de datos](team-lead-tasks.md)
- [Tareas del responsable de proyecto en un equipo de ciencia de datos](project-lead-tasks.md)