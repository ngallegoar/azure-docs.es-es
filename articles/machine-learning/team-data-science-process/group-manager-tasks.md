---
title: Tareas del administrador de grupo de control de proceso de ciencia de datos en equipo
description: Siga este detallado tutorial de las tareas que realiza un administrador de grupos en un proyecto en equipo de ciencia de datos.
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 4ec7f4242e5046e90fdf0eb8c6c0579f402e4f55
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "76721360"
---
# <a name="team-data-science-process-group-manager-tasks"></a>Tareas del administrador de grupo de control de proceso de ciencia de datos en equipo

En este artículo se describen las tareas que completa un *administrador de grupo* para una organización de ciencia de datos. El administrador de grupo administra la unidad de ciencia de datos completa en una empresa. Una unidad de ciencia de datos puede tener varios equipos, cada uno de ellos trabajando en muchos proyectos de ciencia de datos en segmentos verticales de negocio distintos. El objetivo del administrador de grupo es establecer un entorno de colaboración en grupo que normalice el [proceso de ciencia de datos en equipo](overview.md) (TDSP). Para ver un esquema de todos los roles del personal y las tareas asociadas de las que se ocupa un equipo de ciencia de datos que sigue este proceso como estándar, consulte [Roles y tareas del proceso de ciencia de datos en equipo](roles-tasks.md).

En el diagrama siguiente se muestran las seis tareas principales de configuración del administrador del grupo. Los administradores de grupo pueden delegar sus tareas en un suplente, pero las tareas asociadas al rol no cambian.

![Tareas del administrador de grupo](./media/group-manager-tasks/tdsp-group-manager.png)

1. Configure una **organización de Azure DevOps** para el grupo.
2. Cree el **proyecto GroupCommon** predeterminado en la organización de Azure DevOps.
3. Cree el repositorio **GroupProjectTemplate** en Azure Repos.
4. Cree el repositorio **GroupUtilities** en Azure Repos.
5. Importe el contenido de los repositorios **ProjectTemplate** y **Utilities** del equipo TDSP de Microsoft en los repositorios comunes del grupo.
6. Configure la **pertenencia** y los **permisos** para que los miembros del equipo tengan acceso al grupo.

En el siguiente tutorial se explican los pasos en detalle. 

> [!NOTE] 
> En este artículo se utiliza Azure DevOps para configurar un entorno de equipo de TDSP, ya que se explica cómo implementar TDSP en Microsoft. Si el grupo usa otras plataformas de desarrollo u hospedaje de código, las tareas del administrador de grupo son las mismas, pero la manera de completarlas puede ser diferente.

## <a name="create-an-organization-and-project-in-azure-devops"></a>Creación de una organización y un proyecto en Azure DevOps

1. Vaya a [visualstudio.microsoft.com](https://visualstudio.microsoft.com), seleccione **Inicio de sesión** en la parte superior derecha e inicie sesión en su cuenta Microsoft. 
   
   ![Iniciar sesión en su cuenta de Microsoft](./media/group-manager-tasks/signinvs.png)
   
   Si no tiene una cuenta Microsoft, haga clic en **Registrarse ahora** para crear una cuenta Microsoft y, después, inicie sesión con esa cuenta. Si su organización tiene una suscripción de Visual Studio, inicie sesión con las credenciales de dicha suscripción.
   
1. Después de iniciar sesión, en la parte superior derecha de la página Azure DevOps, seleccione **Crear una nueva organización**.
   
   ![Creación de una nueva organización](./media/group-manager-tasks/create-organization.png)
   
1. Si se le pide que acepte los términos de servicio, la declaración de privacidad y el código de conducta, seleccione **Continuar**.
   
1. En el cuadro de diálogo de registro, asigne un nombre a la organización de Azure DevOps y acepte la asignación de la región de hospedaje, o bien seleccione una región distinta. Después, seleccione **Continuar**. 

1. En **Cree un proyecto para empezar**, escriba *GroupCommon* y luego seleccione **Crear proyecto**. 
   
   ![Crear proyecto](./media/group-manager-tasks/create-project.png)

Se abre la página **Resumen** del proyecto **GroupCommon**. La dirección URL de la página es *https:\//\<servername>/\<organization-name>/GroupCommon*.

![Página de resumen del proyecto](./media/group-manager-tasks/project-summary.png)

## <a name="set-up-the-group-common-repositories"></a>Configuración de los repositorios comunes de grupo

Azure Repos hospeda los siguientes tipos de repositorios para el grupo:

- **Repositorios comunes del grupo**: Repositorios de uso general que pueden adoptar varios equipos dentro de una unidad de ciencia de datos para diversos proyectos de ciencia de datos. 
- **Repositorios de equipo**:  Repositorios para equipos concretos de una unidad de ciencia de datos. Estos repositorios son específicos para las necesidades de un equipo, y pueden utilizarse para múltiples proyectos dentro de ese equipo, pero no son lo suficientemente genéricos como para utilizarse en diferentes equipos dentro de una unidad de ciencia de datos.
- **Repositorios de proyecto**: Repositorios para proyectos concretos. Estos repositorios pueden no ser lo suficientemente genéricos para varios proyectos dentro de un equipo, o para otros equipos de una unidad de ciencia de datos.

Para configurar los repositorios comunes de grupos en el proyecto, puede: 
- Cambiar el nombre del repositorio predeterminado **GroupCommon** a **GroupProjectTemplate**
- Crear un nuevo repositorio **GroupUtilities**

### <a name="rename-the-default-project-repository-to-groupprojecttemplate"></a>Cambiar el nombre del repositorio de proyecto predeterminado a GroupProjectTemplate

Para cambiar el nombre del repositorio de proyecto predeterminado **GroupCommon** a **GroupProjectTemplate**:

1. En la página **Resumen** del proyecto **GroupCommon**, seleccione **Repos**. Esta acción le lleva al repositorio predeterminado **GroupCommon** del proyecto GroupCommon, que está actualmente vacío.
   
1. En la parte superior de la página, despliegue la flecha junto a **GroupCommon** y seleccione **Administrar repositorios**.
   
   ![Administrar repositorios](./media/group-manager-tasks/rename-groupcommon-repo-3.png)
   
1. En la página **Configuración del proyecto**, seleccione **...** junto a **GroupCommon** y luego seleccione **Cambiar nombre de repositorio**. 
   
   ![Seleccionar... y luego Cambiar nombre de repositorio](./media/group-manager-tasks/rename-groupcommon-repo-4.png)
   
1. En el menú emergente **Rename the GroupCommon repository** (Cambiar el nombre del repositorio GroupCommon), escriba *GroupProjectTemplate* y luego seleccione **Cambiar nombre**. 
   
   ![Cambiar nombre de repositorio](./media/group-manager-tasks/rename-groupcommon-repo-6.png)

### <a name="create-the-grouputilities-repository"></a>Creación del repositorio GroupUtilities

Para crear el repositorio **GroupUtilities**:

1. En la página **Resumen** del proyecto **GroupCommon**, seleccione **Repos**. 
   
1. En la parte superior de la página, despliegue la flecha junto a **GroupProjectTemplate** y seleccione **Nuevo repositorio**.
   
   ![Seleccionar Nuevo repositorio](./media/group-manager-tasks/create-grouputilities-repo-1.png)
   
1. En el cuadro de diálogo **Crear un nuevo repositorio**, seleccione **Git** como **Tipo**, escriba *GroupUtilities* como **Nombre del repositorio** y luego seleccione **Crear**.
   
   ![Creación del repositorio GroupUtilities](./media/group-manager-tasks/create-grouputilities-repo-2.png)
   
1. En la página **Configuración del proyecto**, seleccione **Repositorios** bajo **Repos** en el panel de navegación izquierdo para ver los dos repositorios de grupo: **GroupProjectTemplate** y **GroupUtilities**.
   
   ![Dos repositorios de grupo](./media/group-manager-tasks/two-repositories.png)

## <a name="import-the-microsoft-tdsp-team-repositories"></a>Importación de los repositorios del equipo TDSP de Microsoft

En esta parte del tutorial, importará el contenido de los repositorios **ProjectTemplate** y **Utilities** administrados por el equipo TDSP de Microsoft en sus repositorios **GroupProjectTemplate** y **GroupUtilities**. 

Para importar los repositorios del equipo TDSP:

1. En la página principal del proyecto **GroupCommon**, seleccione **Repos** en el panel de navegación de la izquierda. Se abre el repositorio predeterminado **GroupProjectTemplate**. 
   
1. En la página **GroupProjectTemplate is empty** (GroupProjectTemplate está vacío), seleccione **Importar**. 
   
   ![Seleccionar Importar](./media/group-manager-tasks/import-repo.png)
   
1. En el cuadro de diálogo **Importar un repositorio GIT**, seleccione **Git** como **Tipo de origen** y escriba *https:\//github.com/Azure/Azure-TDSP-ProjectTemplate.git* para **Dirección URL de clonación**. Luego, seleccione **Importar**. El contenido del repositorio de ProjectTemplate del equipo TDSP de Microsoft se importa en el repositorio GroupProjectTemplate. 
   
   ![Importación del repositorio del equipo TDSP de Microsoft](./media/group-manager-tasks/import-repo-2.png)
   
1. En la parte superior de la página **Repos** del proyecto, seleccione el repositorio **GroupUtilities**.
   
1. Repita el proceso de importación para importar el contenido del repositorio **Utilities** del equipo TDSP de Microsoft, *https:\//github.com/Azure/Azure-TDSP-Utilities.git*, en su repositorio **GroupUtilities**. 
   
Cada uno de los dos repositorios de grupo contiene ahora todos los archivos, excepto los del directorio *.git*, del repositorio correspondiente del equipo TDSP de Microsoft. 

## <a name="customize-the-contents-of-the-group-repositories"></a>Personalización del contenido de los repositorios de grupo

Si desea personalizar el contenido de los repositorios de grupo para satisfacer las necesidades específicas de su grupo, puede hacerlo ahora. Puede modificar los archivos, cambiar la estructura de los directorios o agregar archivos que ha desarrollado su grupo o que son útiles para el grupo.

### <a name="make-changes-in-azure-repos"></a>Realización de cambios en Azure Repos

Para personalizar el contenido del repositorio:

1. En la página **Resumen** del proyecto **GroupCommon**, seleccione **Repos**. 
   
1. En la parte superior de la página, seleccione el repositorio que desea personalizar.

1. En la estructura de directorios del repositorio, vaya a la carpeta o al archivo que desee cambiar. 
   
   - Para crear carpetas o archivos nuevos, seleccione la flecha situada junto a **Nuevo**. 
     
     ![Creación de un nuevo archivo](./media/group-manager-tasks/new-file.png)
     
   - Para cargar archivos, seleccione **Cargar archivos**. 
     
     ![Carga de archivos](./media/group-manager-tasks/upload-files.png)
     
   - Para editar los archivos existentes, desplácese hasta el archivo y, a continuación, seleccione **Editar**. 
     
     ![Edición de un archivo](./media/group-manager-tasks/edit-file.png)
     
1. Después de agregar o editar archivos, seleccione **Confirmar**.
   
   ![Confirmación de cambios](./media/group-manager-tasks/commit.png)

### <a name="make-changes-using-your-local-machine-or-dsvm"></a>Realización de cambios con el equipo local o DSVM

Si desea realizar cambios con el equipo local o DSVM e incorporar los cambios a los repositorios de grupo, asegúrese de que cumple los requisitos previos para trabajar con Git y máquinas virtuales DSVM:

- Una suscripción de Azure, si desea crear una DSVM.
- Git instalado en la máquina. Si usa una DSVM, GIT está preinstalado. En caso contrario, consulte el [apéndice de plataformas y herramientas](platforms-and-tools.md#appendix).
- Si desea usar la DSVM, la DSVM de Windows o Linux creada y configurada en Azure. Para más información e instrucciones, consulte la [documentación de Data Science Virtual Machine](/azure/machine-learning/data-science-virtual-machine/).
- Para una DSVM de Windows, debe tener [Git Credential Manager (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) instalado en la máquina. En el archivo *README.md*, desplácese a la sección **Descargar e instalar** y haga clic en el **instalador más reciente**. Descargue el instalador *.exe* desde la página del instalador y ejecútelo. 
- En el caso de una DSVM de Linux, se configura una clave pública SSH en la DSVM y se agrega en Azure DevOps. Para más información e instrucciones, consulte la sección acerca de cómo **crear una clave pública SSH** en el [apéndice de plataformas y herramientas](platforms-and-tools.md#appendix). 

En primer lugar, copie o *clone* el repositorio en la máquina local. 
   
1. En la página **Resumen** del proyecto **GroupCommon**, seleccione **Repos** y, en la parte superior de la página, seleccione el repositorio que desea clonar.
   
1. En la página de repositorios, seleccione **Clonar** en la esquina superior derecha.
   
1. En el cuadro de diálogo **Clonar repositorio**, seleccione **HTTPS** para una conexión HTTP o **SSH** para una conexión SSH y copie la dirección URL de clonación en la **Línea de comandos** del Portapapeles.
   
   ![Clonación de repositorio](./media/group-manager-tasks/clone.png)
   
1. En la máquina local, cree los directorios siguientes:
   
   - Para Windows: **C:\GitRepos\GroupCommon**
   - En Linux, **$/GitRepos/GroupCommon** en el directorio principal 
   
1. Cambie al directorio que ha creado.
   
1. En Git Bash, ejecute el comando `git clone <clone URL>.`.
   
   Por ejemplo, cualquiera de los siguientes comandos clona el repositorio **GroupUtilities** en el directorio *GroupCommon* del equipo local. 
   
   **Conexión HTTPS:**
   
   ```bash
   git clone https://DataScienceUnit@dev.azure.com/DataScienceUnit/GroupCommon/_git/GroupUtilities
   ```
   
   **Conexión SSH:**
   
   ```bash
   git clone git@ssh.dev.azure.com:v3/DataScienceUnit/GroupCommon/GroupUtilities
   ```

Después de realizar los cambios que desee en el clon local del repositorio, puede insertar los cambios en los repositorios del grupo común compartido. 

Ejecute los siguientes comandos de Git Bash desde el directorio local **GroupProjectTemplate** o **GroupUtilities**.

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

## <a name="add-group-members-and-configure-permissions"></a>Adición de miembros de grupo y configuración de permisos

Para agregar miembros al grupo:

1. En Azure DevOps, en la página principal del proyecto **GroupCommon**, seleccione **Configuración del proyecto** en el panel de navegación izquierdo. 
   
1. En el panel de navegación izquierdo de **Configuración del proyecto**, seleccione **Equipos** y, a continuación, en la página **Equipos**, seleccione el **equipo GroupCommon**. 
   
   ![Configuración de equipos](./media/group-manager-tasks/teams.png)
   
1. En la página **Perfil de equipo**, seleccione **Agregar**.
   
   ![Adición al equipo GroupCommon](./media/group-manager-tasks/add-to-team.png)
   
1. En el cuadro de diálogo **Agregar usuarios y grupos**, busque y seleccione los miembros que desea agregar al grupo y, a continuación, seleccione **Guardar cambios**. 
   
   ![Adición de usuarios y grupos](./media/group-manager-tasks/add-users.png)
   

Para configurar permisos para los miembros:

1. En el panel de navegación izquierdo **Configuración del proyecto**, seleccione **Permisos**. 
   
1. En la página **Permisos**, seleccione el grupo al que desea agregar miembros. 
   
1. En la página de ese grupo, seleccione **Miembros** y, a continuación, seleccione **Agregar**. 
   
1. En el cuadro de diálogo **Invitar a los miembros**, busque y seleccione los miembros que desea agregar al grupo y, a continuación, seleccione **Guardar**. 
   
   ![Concesión de permisos a los miembros](./media/group-manager-tasks/grant-permissions.png)

## <a name="next-steps"></a>Pasos siguientes

Estos son vínculos a descripciones detalladas de los roles y tareas del proceso de ciencia de datos en equipo:

- [Tareas del responsable de equipo en un equipo de ciencia de datos](team-lead-tasks.md)
- [Tareas del responsable de proyecto en un equipo de ciencia de datos](project-lead-tasks.md)
- [Tareas de colaboradores individuales del proyecto en un equipo de ciencia de datos](project-ic-tasks.md)
