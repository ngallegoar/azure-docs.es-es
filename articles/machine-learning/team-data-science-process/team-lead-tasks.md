---
title: Tareas del responsable de equipo en el equipo de proceso de ciencia de datos en equipos
description: Un tutorial detallado de las tareas de un responsable de equipo en un equipo de proceso de ciencia de datos en equipo
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: df7d2278487c1b098615a14562c498b9187c56eb
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93321195"
---
# <a name="tasks-for-the-team-lead-on-a-team-data-science-process-team"></a>Tareas del responsable de equipo en el equipo de proceso de ciencia de datos en equipo

En este artículo se describen las tareas que completa un *responsable de equipo* para su equipo de ciencia de datos. El objetivo del responsable de equipo es establecer el entorno de colaboración de equipo que se estandariza en el [proceso de ciencia de datos en equipo](overview.md) (TDSP). Este proceso está diseñado para ayudar a mejorar la colaboración y el aprendizaje en equipo. 

El TDSP es una metodología de ciencia de datos ágil e iterativa para proporcionar de manera eficiente soluciones de análisis predictivo y aplicaciones inteligentes. El proceso sintetiza las estructuras y procedimientos recomendados de Microsoft y el sector.  El objetivo es implementar correctamente las iniciativas de ciencia de datos y aprovechar por completo las ventajas de sus programas de análisis. Para ver un esquema de los roles del personal y las tareas asociadas de las que se ocupa un equipo de ciencia de datos que sigue este proceso como estándar, consulte [Roles y tareas del proceso de ciencia de datos en equipo](roles-tasks.md).

Un responsable de equipo administra un equipo que consiste en varios científicos de datos de la unidad de ciencia de datos de una empresa. Dependiendo del tamaño y de la estructura de la unidad de ciencia de datos, el [administrador del grupo](group-manager-tasks.md) y el responsable de equipo podrían ser la misma persona o podrían delegar sus tareas a los suplentes. Pero las tareas en sí no cambian. 

En el diagrama siguiente se muestra el flujo de trabajo de las tareas que el responsable de equipo completa para configurar un entorno de equipo:

![Flujo de trabajo de las tareas del responsable de equipo](./media/team-lead-tasks/team-leads-1-creating-teams.png)

1. Cree un **proyecto de equipo** en la organización del grupo en Azure DevOps. 
  
1. Cambie el nombre del repositorio de equipo predeterminado a **TeamUtilities**.
  
1. Cree un nuevo repositorio **TeamTemplate** en el proyecto de equipo. 
  
1. Importe el contenido de los repositorios **GroupUtilities** y **GroupProjectTemplate** del grupo en los repositorios **TeamUtilities** y **TeamTemplate**. 
  
1. Configura el **control de seguridad** mediante la adición de miembros del equipo y la configuración de sus permisos.
  
1. Si se requiere, puede crear datos de equipo y recursos de análisis:
   - Agregue las utilidades específicas del equipo al repositorio **TeamUtilities**. 
   - Cree un almacenamiento de **Azure File Storage** para almacenar los recursos de datos que puedan ser útiles para todo el equipo. 
   - Monte el almacenamiento de Azure File Storage en la instancia de **Data Science Virtual Machine** (DSVM) del responsable de equipo y agréguele los recursos de datos.

En el siguiente tutorial se explican los pasos en detalle.

> [!NOTE] 
> En este artículo se utilizan Azure DevOps y DSVM para configurar un entorno de equipo de TDSP, ya que se explica cómo implementar TDSP en Microsoft. Si el equipo usa otras plataformas de desarrollo o hospedaje de código, las tareas del responsable de equipo son las mismas, pero la manera de completarlas puede ser diferente.

## <a name="prerequisites"></a>Requisitos previos

En este tutorial se da por supuesto que los siguientes recursos y permisos están configurados por el [administrador de grupo](group-manager-tasks.md):

- La **organización** de Azure DevOps para la unidad de datos
- Los repositorios **GroupProjectTemplate** y **GroupUtilities** , rellenados con el contenido de los repositorios **ProjectTemplate** y **Utilities** del equipo de Microsoft TDSP
- Permisos en la cuenta de la organización para que pueda crear proyectos y repositorios para su equipo

Para poder clonar los repositorios y modificar su contenido en el equipo local o en la DSVM, o configurar el almacenamiento de Azure File Storage y montarlo en la DSVM, necesita lo siguiente:

- Suscripción a Azure.
- Git instalado en la máquina. Si usa una DSVM, GIT está preinstalado. En caso contrario, consulte el [apéndice de plataformas y herramientas](platforms-and-tools.md#appendix).
- Si desea usar la DSVM, la DSVM de Windows o Linux creada y configurada en Azure. Para más información e instrucciones, consulte la [documentación de Data Science Virtual Machine](../data-science-virtual-machine/index.yml).
- Para una DSVM de Windows, debe tener [Git Credential Manager (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) instalado en la máquina. En el archivo *README.md* , desplácese a la sección **Descargar e instalar** y haga clic en el **instalador más reciente**. Descargue el instalador *.exe* desde la página del instalador y ejecútelo. 
- En el caso de una DSVM de Linux, se configura una clave pública SSH en la DSVM y se agrega en Azure DevOps. Para más información e instrucciones, consulte la sección acerca de cómo **crear una clave pública SSH** en el [apéndice de plataformas y herramientas](platforms-and-tools.md#appendix). 

## <a name="create-a-team-project-and-repositories"></a>Creación de un proyecto de equipo y de los repositorios

En esta sección, creará los siguientes recursos en la organización de Azure DevOps del grupo:

- El proyecto **MyTeam** de Azure DevOps
- El repositorio **TeamTemplate**
- El Repositorio **TeamUtilities**

Los nombres especificados para los directorios y repositorios de este tutorial asumen que su objetivo es establecer un proyecto independiente para su propio equipo dentro de un grupo de ciencia de datos mayor. Sin embargo, todo el grupo puede elegir trabajar en un único proyecto creado por el administrador del grupo o el administrador de la organización. A continuación, todos los equipos de ciencia de datos crean repositorios en este único proyecto. Este escenario podría ser válido en estos casos:
- Un grupo de ciencia de datos pequeño que no tiene varios equipos de ciencia de datos. 
- Un grupo de ciencia de datos grande con varios equipos de ciencia de datos que, sin embargo, desea optimizar la colaboración entre equipos con actividades como el planeamiento de sprint en el nivel de grupo. 

Si los equipos optan por tener los repositorios específicos del equipo en un único proyecto de grupo, los responsables del equipo deben crear los repositorios con nombres como *\<TeamName>Template* y *\<TeamName>Utilities*. Por ejemplo: *TeamATemplate* y *TeamAUtilities*. 

En cualquier caso, los responsables de equipo deben permitir que los miembros del equipo sepan qué repositorios de plantillas y utilidades tienen que configurar y clonar. Los responsables de proyecto deben seguir las [tareas del responsable de equipo en un equipo de ciencia de datos](project-lead-tasks.md) para crear repositorios de proyectos, bien dentro de proyectos independientes o en un único proyecto. 

### <a name="create-the-myteam-project"></a>Creación del proyecto MyTeam

Para crear un proyecto independiente para su equipo:

1. En el explorador web, vaya a la página principal de la organización de Azure DevOps del grupo en la dirección URL *https:\//\<server name>/\<organization name>* y seleccione **Nuevo proyecto**. 
   
   ![Selección de Nuevo proyecto](./media/team-lead-tasks/team-leads-2-create-new-team.png)
   
1. En el cuadro de diálogo **Crear proyecto** , escriba el nombre del equipo, como *MyTeam* , en **Nombre de proyecto** y, a continuación, seleccione **Avanzado**. 
   
1. En **Control de versiones** , seleccione **Git** y, en **Proceso de elemento de trabajo** , seleccione **Agile**. Seleccione **Crear**. 
   
   ![Crear proyecto](./media/team-lead-tasks/team-leads-3-create-new-team-2.png)
   
Se abre la página **Resumen** del proyecto del equipo, con la dirección URL *https:\//\<server name>/\<organization name>/\<team name>* .

### <a name="rename-the-myteam-default-repository-to-teamutilities"></a>Cambie el nombre del repositorio de MyTeam predeterminado a TeamUtilities.

1. En la página **Resumen** del proyecto de **MyTeam** , en **¿Por qué servicio quiere empezar?** , seleccione **Repositorios**. 
   
   ![Selección de repositorios](./media/team-lead-tasks/team-leads-6-rename-team-project-repo.png)
   
1. En la página del repositorio **MyTeam** , seleccione el repositorio **MyTeam** en la parte superior de la página y, a continuación, seleccione **Administrar repositorios** en la lista desplegable. 
   
   ![Selección de Administrar repositorios](./media/team-lead-tasks/team-leads-7-rename-team-project-repo-2.png)
1. En la página **Configuración del proyecto** , seleccione **...** junto al repositorio **MyTeam** y, después, seleccione **Cambiar nombre de repositorio**. 
   
   ![Selección de Cambiar nombre de repositorio](./media/team-lead-tasks/team-leads-8-rename-team-project-repo-3.png)
   
1. En el menú emergente **Cambiar el nombre del repositorio MyTeam** , escriba *TeamUtilities* y, después, seleccione **Cambiar nombre**. 

### <a name="create-the-teamtemplate-repository"></a>Creación del repositorio TeamTemplate

1. En la página **Configuración del repositorio** , seleccione **Nuevo repositorio**. 
   
   ![Seleccionar Nuevo repositorio](./media/team-lead-tasks/team-leads-9-create-team-utilities.png)
   
   O bien, seleccione **Repositorios** en el panel de navegación izquierdo de la página **Resumen** del proyecto **MyTeam** , seleccione un repositorio en la parte superior de la página y, después, seleccione **Nuevo repositorio**.
   
1. En el cuadro de diálogo **Crear un nuevo repositorio** , asegúrese de que **Git** esté seleccionado en **Tipo**. Escriba *TeamTemplate* en **Nombre del repositorio** y, a continuación, seleccione **Crear**.
   
   ![Crear repositorio](./media/team-lead-tasks/team-leads-10-create-team-utilities-2.png)
   
1. Confirme que puede ver los dos repositorios **TeamUtilities** y **TeamTemplate** en la página de configuración del proyecto. 
   
   ![Dos repositorios de equipo](./media/team-lead-tasks/team-leads-11-two-repo-in-team.png)

### <a name="import-the-contents-of-the-group-common-repositories"></a>Importación del contenido de los repositorios comunes del grupo

Para rellenar los repositorios de equipo con el contenido de los repositorios comunes de grupo configurados por el administrador de grupo:

1. En la página principal del proyecto **MyTeam** , seleccione **Repositorios** en el panel de navegación de la izquierda. Si aparece un mensaje que indica que no se encuentra la plantilla **MyTeam** , seleccione el vínculo en **De lo contrario, vaya al repositorio TeamTemplate predeterminado**. 
   
   Se abre el repositorio predeterminado **TeamTemplate**. 
   
1. En la página **TeamTemplate está vacío** , seleccione **Importar**. 
   
   ![Seleccionar Importar](./media/team-lead-tasks/import-repo.png)
   
1. En el cuadro de diálogo **Importar un repositorio GIT** , seleccione **Git** como **Tipo de origen** y escriba la dirección URL del repositorio de plantillas comunes del grupo en **Dirección URL de clonación**. La dirección URL es *https:\//\<server name>/\<organization name>/_git/\<repository name>* . Por ejemplo: *https:\//dev.azure.com/DataScienceUnit/GroupCommon/_git/GroupProjectTemplate*. 
   
1. Seleccione **Import** (Importar). El contenido del repositorio de plantillas de grupo se importa en el repositorio de plantillas de equipo. 
   
   ![Importación del repositorio de plantillas comunes de grupo](./media/team-lead-tasks/import-repo-2.png)
   
1. En la parte superior de la página **Repositorios** del proyecto, seleccione el repositorio **TeamUtilities**.
   
1. Repita el proceso para importar el contenido del repositorio de utilidades comunes del grupo, por ejemplo *GroupUtilities* , en el repositorio **TeamUtilities**. 
   
Cada uno de los dos repositorios de equipo ahora contiene los archivos del repositorio común de grupo correspondiente. 

### <a name="customize-the-contents-of-the-team-repositories"></a>Personalización del contenido de los repositorios del equipo

Si desea personalizar el contenido de los repositorios del equipo para satisfacer las necesidades específicas del equipo, puede hacerlo ahora. Puede modificar archivos, cambiar la estructura de directorios o agregar archivos y carpetas.

Para modificar, cargar o crear archivos o carpetas directamente en Azure DevOps:

1. En la página **Summary** del proyecto **MyTeam** , seleccione **Repositorios**. 
   
1. En la parte superior de la página, seleccione el repositorio que desea personalizar.

1. En la estructura de directorios del repositorio, vaya a la carpeta o al archivo que desee cambiar. 
   
   - Para crear carpetas o archivos nuevos, seleccione la flecha situada junto a **Nuevo**. 
     
     ![Creación de un nuevo archivo](./media/team-lead-tasks/new-file.png)
     
   - Para cargar archivos, seleccione **Cargar archivos**. 
     
     ![Carga de archivos](./media/team-lead-tasks/upload-files.png)
     
   - Para editar los archivos existentes, desplácese hasta el archivo y, a continuación, seleccione **Editar**. 
     
     ![Edición de un archivo](./media/team-lead-tasks/edit-file.png)
     
1. Después de agregar o editar archivos, seleccione **Confirmar**.
   
   ![Confirmación de cambios](./media/team-lead-tasks/commit.png)

Para trabajar con repositorios en el equipo local o DSVM, primero copie o *clone* los repositorios en el equipo local y, después, confirme e inserte los cambios en los repositorios compartidos del equipo. 

Para clonar repositorios:

1. En la página **Resumen** del proyecto **MyTeam** , seleccione **Repositorios** y, en la parte superior de la página, seleccione el repositorio que desea clonar.
   
1. En la página de repositorios, seleccione **Clonar** en la esquina superior derecha.
   
1. En el cuadro de diálogo **Clonar repositorio** , en **Línea de comandos** , seleccione **HTTPS** para una conexión HTTP o **SSH** para una conexión SSH y copie la dirección URL de clonación en el portapapeles.
   
   ![Copia de la dirección URL de clonación](./media/team-lead-tasks/clone.png)
   
1. En la máquina local, cree los directorios siguientes:
   
   - Para Windows: **C:\GitRepos\MyTeam**
   - Para Linux, **$home/GitRepos/MyTeam** 
   
1. Cambie al directorio que ha creado.
   
1. En Git Bash, ejecute el comando `git clone <clone URL>`, donde \<clone URL> es la dirección URL que ha copiado del cuadro de diálogo **Clonar**.
   
   Por ejemplo, use uno de los siguientes comandos para clonar el repositorio **TeamUtilities** en el directorio *MyTeam* en el equipo local. 
   
   **Conexión HTTPS:**
   
   ```bash
   git clone https://DataScienceUnit@dev.azure.com/DataScienceUnit/MyTeam/_git/TeamUtilities
   ```
   
   **Conexión SSH:**
   
   ```bash
   git clone git@ssh.dev.azure.com:v3/DataScienceUnit/MyTeam/TeamUtilities
   ```

Después de realizar los cambios que desee en el clon local del repositorio, confirme e inserte los cambios en los repositorios del equipo compartido. 

Ejecute los siguientes comandos de Git Bash desde el directorio local **GitRepos\MyTeam\TeamTemplate** o **GitRepos\MyTeam\TeamUtilities**.

```bash
git add .
git commit -m "push from local"
git push
```

> [!NOTE]
> Si es la primera vez que confirma un repositorio de Git, posiblemente deba configurar los parámetros globales *user.name* y *user.email* para poder ejecutar el comando `git commit`. Ejecute los siguientes dos comandos:
> 
> `git config --global user.name <your name>`
> 
> `git config --global user.email <your email address>`
> 
> Si está confirmando varios repositorios de Git, utilice el mismo nombre y la misma dirección de correo electrónico en todos ellos. Usar el mismo nombre y la misma dirección de correo electrónico resulta cómodo al crear paneles de Power BI y realizar el seguimiento de las actividades de Git en varios repositorios.

## <a name="add-team-members-and-configure-permissions"></a>Adición de miembros de equipo y configuración de permisos

Para agregar miembros al equipo:

1. En Azure DevOps, en la página principal del proyecto **MyTeam** , seleccione **Configuración del proyecto** en el panel de navegación izquierdo. 
   
1. En el panel de navegación izquierdo de **Configuración del proyecto** , seleccione **Equipos** y, a continuación, en la página **Equipos** , seleccione el equipo **MyTeam**. 
   
   ![Configuración de equipos](./media/team-lead-tasks/teams.png)
   
1. En la página **Perfil de equipo** , seleccione **Agregar**.
   
   ![Adición al equipo MyTeam](./media/team-lead-tasks/add-to-team.png)
   
1. En el cuadro de diálogo **Agregar usuarios y grupos** , busque y seleccione los miembros que desea agregar al grupo y, a continuación, seleccione **Guardar cambios**. 
   
   ![Adición de usuarios y grupos](./media/team-lead-tasks/add-users.png)
   

Para configurar permisos para los miembros de equipo:

1. En el panel de navegación izquierdo **Configuración del proyecto** , seleccione **Permisos**. 
   
1. En la página **Permisos** , seleccione el grupo al que desea agregar miembros. 
   
1. En la página de ese grupo, seleccione **Miembros** y, a continuación, seleccione **Agregar**. 
   
1. En el cuadro de diálogo **Invitar a los miembros** , busque y seleccione los miembros que desea agregar al grupo y, a continuación, seleccione **Guardar**. 
   
   ![Concesión de permisos a los miembros](./media/team-lead-tasks/grant-permissions.png)

## <a name="create-team-data-and-analytics-resources"></a>Creación de datos de equipo y recursos de análisis

Este paso es opcional, pero compartir los recursos de datos y de análisis con todo el equipo tiene ventajas de rendimiento y costo. Los miembros del equipo pueden ejecutar sus proyectos en los recursos compartidos, ahorrar en los presupuestos y colaborar de forma más eficaz. Puede crear un almacenamiento de Azure File Storage y montarlo en la DSVM para compartirlo con los miembros del equipo. 

Para obtener información sobre cómo compartir otros recursos con su equipo, como clústeres de Azure HDInsight Spark, consulte [Plataformas y herramientas](platforms-and-tools.md). En este tema se proporcionan instrucciones desde una perspectiva de ciencia de datos sobre la selección de recursos que son adecuados para sus necesidades, junto con vínculos a páginas de productos y otros tutoriales interesantes y útiles.

>[!NOTE]
> Para evitar la transmisión de datos entre centros de datos, lo que podría ser lento y costoso, asegúrese de que el grupo de recursos, la cuenta de almacenamiento y la DSVM de Azure estén hospedados en la misma región de Azure. 

### <a name="create-azure-file-storage"></a>Creación de almacenamiento de Azure File Storage

1. Ejecute el script siguiente para crear un almacenamiento de Azure File Storage para los recursos de datos que son útiles para todo el equipo. El script le solicita información de su suscripción de Azure, por lo que debe estar preparado para escribirla. 

   - En una máquina Windows, ejecute el script desde el símbolo del sistema de PowerShell:
     
     ```powershell
     wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/CreateFileShare.ps1" -outfile "CreateFileShare.ps1"
     .\CreateFileShare.ps1
     ```
     
   - En una máquina Linux, ejecute el script desde el shell de Linux:
     
     ```shell
     wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/CreateFileShare.sh"
     bash CreateFileShare.sh
     ```
   
1. Inicie sesión en la cuenta de Microsoft Azure y, si es necesario, seleccione la suscripción que desea usar.
   
1. Seleccione la cuenta de almacenamiento que usará o cree una nueva en la suscripción seleccionada. Puede usar caracteres en minúsculas, números y guiones para el nombre de Azure File Storage.
   
1. Para facilitar el montaje y el uso compartido del almacenamiento, presione Entrar o escriba *Y* para guardar la información de Azure File Storage en un archivo de texto en el directorio actual. Puede proteger este archivo de texto en el repositorio **TeamTemplate** , lo idean en **Docs\DataDictionaries** , por lo que todos los proyectos del equipo pueden acceder a él. También necesitará la información del archivo para montar el almacenamiento de Azure File Storage en Azure DSVM en la sección siguiente. 
   
### <a name="mount-azure-file-storage-on-your-local-machine-or-dsvm"></a>Montaje de Azure File Storage en la máquina local o DSVM

1. Para montar el almacenamiento de Azure File Storage en el equipo local o DSVM, utilice el script siguiente.
   
   - En una máquina Windows, ejecute el script desde el símbolo del sistema de PowerShell:
     
     ```powershell
     wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/AttachFileShare.ps1" -outfile "AttachFileShare.ps1"
     .\AttachFileShare.ps1
     ```
     
   - En una máquina Linux, ejecute el script desde el shell de Linux:
     
     ```shell
     wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/AttachFileShare.sh"
     bash AttachFileShare.sh
     ```
   
1. Presione Entrar o escriba *Y* para continuar, si guardó un archivo de información de Azure File Storage en el paso anterior. Escriba la ruta de acceso completa y el nombre del archivo que ha creado. 
   
   Si no tiene un archivo de información de Azure File Storage, escriba *n* y siga las instrucciones para especificar la suscripción, la cuenta de Azure Storage y la información de Azure File Storage.
   
1. Escriba el nombre de la unidad local o TDSP en donde se va a montar el recurso compartido de archivos. En la pantalla se muestra una lista de nombres de unidad existentes. Indique un nombre de unidad que no todavía no exista.
   
1. Confirme que la nueva unidad y el almacenamiento estén montados correctamente en la máquina.

## <a name="next-steps"></a>Pasos siguientes

Estos son vínculos a descripciones detalladas de los roles y tareas definidas por el proceso de ciencia de datos en equipo:

- [Tareas del administrador de grupo en un equipo de ciencia de datos](group-manager-tasks.md)
- [Tareas del responsable de proyecto en un equipo de ciencia de datos](project-lead-tasks.md)
- [Tareas de colaboradores individuales del proyecto en un equipo de ciencia de datos](project-ic-tasks.md)