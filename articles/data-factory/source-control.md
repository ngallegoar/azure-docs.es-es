---
title: Control de código fuente
description: Aprenda a configurar el control de código fuente en Azure Data Factory
services: data-factory
ms.service: data-factory
ms.workload: data-services
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.reviewer: ''
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/09/2019
ms.openlocfilehash: 3007865c15ceb03b104282c29179ec59a8196b38
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81604598"
---
# <a name="source-control-in-azure-data-factory"></a>Control de código fuente en Azure Data Factory
[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

La experiencia de la interfaz de usuario (UX) de Azure Data Factory tiene dos modos disponibles para la creación visual:

- Creación directa con el servicio Data Factory
- Creación con la integración de Git o GitHub de Azure Repos

> [!NOTE]
> Solo se admite la creación directa con el servicio Data Factory en la nube de Azure Government.

## <a name="author-directly-with-the-data-factory-service"></a>Creación directa con el servicio Data Factory

Al crear directamente con el servicio Data Factory, la única manera de guardar los cambios es mediante el botón **Publish All** (Publicar todo). Luego, los cambios que realice se publicarán directamente en el servicio Data Factory. 

![Modo Publicar](media/author-visually/data-factory-publish.png)

La creación directa con el servicio Data Factory tiene las siguientes limitaciones:

- El servicio Data Factory no incluye un repositorio que permita almacenar las entidades JSON para los cambios.
- El servicio Data Factory no está optimizado para la colaboración ni el control de versiones.

> [!NOTE]
> La creación directa con el servicio Data Factory está deshabilitada en la experiencia de usuario de Azure Data Factory cuando se configura un repositorio de Git. Se pueden realizar cambios directamente en el servicio mediante PowerShell o un SDK.

## <a name="author-with-azure-repos-git-integration"></a>Creación con la integración de Git de Azure Repos

La creación visual con la integración de Git de Azure Repos admite el control del código fuente y la colaboración para trabajar en canalizaciones de factoría de datos. Puede asociar una factoría de datos con un repositorio de organizaciones de Git de Azure Repos para el control del código fuente, la colaboración, el control de versiones, etc. Una sola organización de Git de Azure Repos puede tener varios repositorios, pero un repositorio de Git de Azure Repos solo puede asociarse a una factoría de datos. Si no tiene un repositorio o una organización de Azure Repos, siga [estas instrucciones](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student) para crear los recursos.

> [!NOTE]
> Puede almacenar archivos de datos y script en un repositorio de Git de Azure Repos. Sin embargo, tiene que cargar manualmente los archivos en Azure Storage. Una canalización de Data Factory no carga automáticamente archivos de script ni datos en un repositorio de Git de Azure Repos en Azure Storage.

### <a name="configure-an-azure-repos-git-repository-with-azure-data-factory"></a>Configuración de un repositorio de Git de Azure Repos con Azure Data Factory

Los usuarios pueden configurar un repositorio de Git de Azure Repos con una factoría de datos mediante dos métodos.

#### <a name="configuration-method-1-azure-data-factory-home-page"></a>Método de configuración 1: Página principal de Azure Data Factory

En la página principal de Azure Data Factory, seleccione **Set up Code Repository** (Configurar el repositorio de código).

![Configurar un repositorio de código de Azure Repos](media/author-visually/configure-repo.png)

#### <a name="configuration-method-2-ux-authoring-canvas"></a>Método de configuración 2: lienzo de creación de la experiencia de usuario
En el lienzo de creación de la experiencia de usuario de Azure Data Factory, seleccione el menú desplegable **Data Factory** y, luego, **Set up Code Repository** (Configurar el repositorio de código).

![Configuración del repositorio de código para la creación con UX](media/author-visually/configure-repo-2.png)

Ambos métodos abren el panel de configuración del repositorio.

![Configuración del repositorio de código](media/author-visually/repo-settings.png)

El panel de configuración muestra la siguiente configuración del repositorio de código de Azure Repos:

| Configuración | Descripción | Value |
|:--- |:--- |:--- |
| **Tipo de repositorio** | Tipo de repositorio de código de Azure Repos.<br/> | Git o GitHub de Azure DevOps |
| **Azure Active Directory** | El nombre de su inquilino de Azure AD. | `<your tenant name>` |
| **Organización de Azure Repos** | Nombre de la organización de Azure Repos. Puede buscar el nombre de la organización de Azure Repos en `https://{organization name}.visualstudio.com`. Puede [iniciar sesión en la organización de Azure Repos](https://www.visualstudio.com/team-services/git/) para acceder a su perfil de Visual Studio y ver sus proyectos y repositorios. | `<your organization name>` |
| **ProjectName** | Nombre de proyecto de Azure Repos. Puede buscar el nombre de proyecto de Azure Repos en `https://{organization name}.visualstudio.com/{project name}`. | `<your Azure Repos project name>` |
| **RepositoryName** | Nombre del repositorio de código de Azure Repos. Los proyectos de Azure Repos contienen repositorios de Git para administrar el código fuente a medida que crece el proyecto. Puede crear un repositorio nuevo o usar uno existente en el proyecto. | `<your Azure Repos code repository name>` |
| **Rama de colaboración** | Rama de colaboración de Azure Repos que se usa para la publicación. De forma predeterminada, es `master`. Cámbiela en caso de que desee publicar recursos de otra rama. | `<your collaboration branch name>` |
| **Carpeta raíz** | Carpeta raíz de la rama de colaboración de Azure Repos. | `<your root folder name>` |
| **Import existing Data Factory resources to repository** (Importar recursos existentes de Data Factory en el repositorio). | Especifica si se deben importar los recursos de la factoría de datos existente del **lienzo de creación** de UX en un repositorio Git de Azure Repos. Active la casilla para importar los recursos de la factoría de datos en el repositorio Git asociado en formato JSON. Esta acción exporta cada recurso individualmente (es decir, los servicios vinculados y los conjuntos de datos se exportan a archivos JSON independientes). Cuando esta casilla no está activada, no se importan los recursos existentes. | Activada (valor predeterminado) |
| **Rama donde importar recursos** | Especifica en qué rama se importan los recursos de la factoría de datos (canalizaciones, conjuntos de datos, servicios vinculados etc.). Puede importar recursos en una de las siguientes ramas: a. Colaboración b. Crear nuevo c. Usar existente |  |

> [!NOTE]
> Si usa Microsoft Edge y no ve ningún valor en el menú desplegable de la cuenta de Azure DevOps, agregue https://*.visualstudio.com a la lista de sitios de confianza.

### <a name="use-a-different-azure-active-directory-tenant"></a>Uso de un inquilino de Azure Active Directory distinto

Puede crear un repositorio Git de Azure Repos en un inquilino de Azure Active Directory distinto. Para especificar otro inquilino de Azure AD, deberá tener permisos de administrador para la suscripción de Azure que utilice.

### <a name="use-your-personal-microsoft-account"></a>Uso de la cuenta de Microsoft personal

Para usar una cuenta de Microsoft personal para la integración de Git, puede vincular el repositorio de Azure personal al entorno de Active Directory de su organización.

1. Agregue la cuenta de Microsoft personal al entorno de Active Directory de su organización como invitado. Para más información, vea [Incorporación de usuarios de colaboración B2B de Azure Active Directory en Azure Portal](../active-directory/b2b/add-users-administrator.md).

2. Inicie sesión en Azure Portal con la cuenta de Microsoft personal. A continuación, cambie al entorno de Active Directory de su organización.

3. Vaya a la sección de Azure DevOps, donde verá ahora el repositorio personal. Seleccione el repositorio y establezca la conexión con Active Directory.

Después de estos pasos de configuración, su repositorio personal está disponible al configurar la integración de Git en la interfaz de usuario de Data Factory.

Para más información sobre cómo conectar Azure Repos al entorno de Active Directory de su organización, vea [Connect your Azure DevOps organization to Azure Active Directory](/azure/devops/organizations/accounts/connect-organization-to-azure-ad) (Conexión de la organización de Azure DevOps a Azure Active Directory).

## <a name="author-with-github-integration"></a>Creación con la integración de GitHub

La creación visual con la integración de GitHub admite el control del código fuente y la colaboración para trabajar en canalizaciones de factoría de datos. Puede asociar una factoría de datos con un repositorio de cuentas de GitHub para el control del código fuente, la colaboración y el control de versiones. Una sola cuenta de GitHub puede tener varios repositorios, pero un repositorio de GitHub solo puede asociarse a una factoría de datos. Si no tiene un repositorio o una cuenta de GitHub, siga  [estas instrucciones](https://github.com/join) para crear los recursos.

La integración de GitHub con Data Factory admite tanto GitHub público (es decir, [https://github.com](https://github.com)) como GitHub Enterprise. Puede usar los repositorios de GitHub públicos y privados con Data Factory siempre que tenga permisos de lectura y escritura para el repositorio en GitHub.

Para configurar un repositorio de GitHub, deberá tener permisos de administrador para la suscripción de Azure que use.

Si desea ver una introducción y una demostración de esta característica, vea el siguiente vídeo de nueve minutos de duración:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Azure-Data-Factory-visual-tools-now-integrated-with-GitHub/player]

### <a name="configure-a-github-repository-with-azure-data-factory"></a>Configuración de un repositorio de GitHub con Azure Data Factory

Los usuarios pueden configurar un repositorio GitHub con una factoría de datos mediante dos métodos.

#### <a name="configuration-method-1-azure-data-factory-home-page"></a>Método de configuración 1: Página principal de Azure Data Factory

En la página principal de Azure Data Factory, seleccione **Set up Code Repository** (Configurar el repositorio de código).

![Configurar un repositorio de código de Azure Repos](media/author-visually/configure-repo.png)

#### <a name="configuration-method-2-ux-authoring-canvas"></a>Método de configuración 2: lienzo de creación de la experiencia de usuario

En el lienzo de creación de la experiencia de usuario de Azure Data Factory, seleccione el menú desplegable **Data Factory** y, luego, **Set up Code Repository** (Configurar el repositorio de código).

![Configuración del repositorio de código para la creación con UX](media/author-visually/configure-repo-2.png)

Ambos métodos abren el panel de configuración del repositorio.

![Configuración del repositorio de GitHub](media/author-visually/github-integration-image2.png)

En el panel configuración se muestra la siguiente configuración del repositorio de GitHub:

| **Configuración** | **Descripción**  | **Valor**  |
|:--- |:--- |:--- |
| **Tipo de repositorio** | Tipo de repositorio de código de Azure Repos. | GitHub |
| **Use GitHub Enterprise** (Usar GitHub Enterprise) | Casilla para seleccionar GitHub Enterprise | no seleccionado (valor predeterminado) |
| **GitHub Enterprise URL** (URL de GitHub Enterprise) | La dirección URL de GitHub Enterprise (debe ser HTTPS para el servidor local de GitHub Enterprise). Por ejemplo: https://github.mydomain.com. Solo es necesario si se selecciona **Use GitHub Enterprise** (Usar GitHub Enterprise). | `<your GitHub enterprise url>` |                                                           
| **Cuenta de GitHub** | El nombre de la cuenta de GitHub. Este nombre se puede encontrar en https:\//github.com/{nombre de cuenta}/{nombre de repositorio}. Para ir a esta página se le pide que introduzca credenciales de OAuth de GitHub en la cuenta de GitHub. | `<your GitHub account name>` |
| **Nombre del repositorio**  | El nombre del repositorio de código de GitHub. Las cuentas de GitHub contienen repositorios de Git para administrar el código fuente. Puede crear un repositorio o usar uno existente que ya se encuentre en su cuenta. | `<your repository name>` |
| **Rama de colaboración** | La rama de colaboración de GitHub que se usa para la publicación. De forma predeterminada, es master. Cámbiela en caso de que desee publicar recursos de otra rama. | `<your collaboration branch>` |
| **Carpeta raíz** | La carpeta raíz de la rama de colaboración de GitHub. |`<your root folder name>` |
| **Import existing Data Factory resources to repository** (Importar recursos existentes de Data Factory en el repositorio). | Especifica si se deben importar los recursos existentes de la factoría de datos desde el lienzo de creación de la experiencia de usuario hasta un repositorio de GitHub. Active la casilla para importar los recursos de la factoría de datos en el repositorio Git asociado en formato JSON. Esta acción exporta cada recurso individualmente (es decir, los servicios vinculados y los conjuntos de datos se exportan a archivos JSON independientes). Cuando esta casilla no está activada, no se importan los recursos existentes. | Activada (valor predeterminado) |
| **Rama donde importar recursos** | Especifica en qué rama se importan los recursos de la factoría de datos (canalizaciones, conjuntos de datos, servicios vinculados etc.). Puede importar recursos en una de las siguientes ramas: a. Colaboración b. Crear nuevo c. Usar existente |  |

### <a name="known-github-limitations"></a>Limitaciones conocidas de GitHub

- Puede almacenar archivos de datos y script en un repositorio de GitHub. Sin embargo, tiene que cargar manualmente los archivos en Azure Storage. Una canalización de Data Factory no carga automáticamente archivos de script o datos en un repositorio de GitHub en Azure Storage.

- GitHub Enterprise con una versión anterior a 2.14.0 no funciona en el explorador Microsoft Edge.

- La integración de GitHub con las herramientas de creación visual de Data Factory solo funciona con la versión de Data Factory disponible con carácter general.

- Se puede capturar un máximo de 1 000 entidades por tipo de recurso (por ejemplo, canalizaciones y conjuntos de valores) desde una sola rama de GitHub. Si se alcanza este límite, se recomienda dividir los recursos en factorías independientes. GIT de Azure DevOps no presenta esta limitación.

## <a name="switch-to-a-different-git-repo"></a>Cambie a otro repositorio de Git diferente

Para cambiar a otro repositorio de Git, haga clic en el icono de **configuración del repositorio de Git** situado en la esquina superior derecha de la página de información general de Data Factory. Si no ve el icono, borre la caché del explorador local. Seleccione el icono para quitar la asociación con el repositorio actual.

![Icono de Git](media/author-visually/remove-repo.png)

Cuando aparezca el panel de configuración del repositorio, seleccione **Remove Git** (Quitar Git). Escriba el nombre de la factoría de datos y haga clic en **Confirm** (Confirmar) para quitar el repositorio de Git asociado a la factoría de datos.

![Elimine la asociación con el repositorio de Git actual.](media/author-visually/remove-repo2.png)

Después de quitar la asociación con el repositorio actual, puede configurar las opciones de Git para usar un repositorio diferente y, luego, importar los recursos existentes de Data Factory en el nuevo repositorio. 

## <a name="version-control"></a>Control de versiones

Los sistemas de control de versiones, conocidos también como _de control de código fuente_, permiten a los desarrolladores colaborar en el código y llevar a cabo el seguimiento de los cambios realizados en la base de código. El control del código fuente es una herramienta esencial para proyectos de varios desarrolladores.

### <a name="creating-feature-branches"></a>Creación de ramas de características

Tan pronto como cada repositorio Git de Azure Repos se asocia a una factoría de datos, tiene una rama de colaboración. (`master` es la rama de colaboración predeterminada). Los usuarios también pueden crear ramas de características; para ello, pueden hacer clic en **+ New Branch** (+ Nueva rama) en la lista desplegable de ramas. Cuando aparezca el panel de nueva rama, escriba el nombre de la rama de características.

![Creación de una rama](media/author-visually/new-branch.png)

Cuando esté listo para combinar los cambios de su rama de características con la rama de colaboración, haga clic en la lista desplegable de ramas y seleccione **Create pull request** (Crear solicitud de incorporación de cambios). Esto le llevará al Git de Azure Repos, donde podrá generar la solicitud de incorporación de cambios, revisar el código y combinar los cambios en su rama de colaboración. (`master` es el valor predeterminado). Solo tiene permitido publicar en el servicio Data Factory de la rama de colaboración. 

![Creación de una nueva solicitud de incorporación de cambios](media/author-visually/create-pull-request.png)

### <a name="configure-publishing-settings"></a>Configurar los valores de publicación

Para configurar la rama de publicación, es decir, la rama donde se guardan las plantillas de Resource Manager, agregue un archivo `publish_config.json` a la carpeta raíz de la rama de colaboración. Data Factory lee este archivo, busca el campo `publishBranch` y crea una nueva rama (si aún no existe) con el valor proporcionado. Después guarda todas las plantillas de Resource Manager en la ubicación especificada. Por ejemplo:

```json
{
    "publishBranch": "factory/adf_publish"
}
```

Al especificar una nueva rama de publicación, Data Factory no elimina la rama de publicación anterior. Si quiere quitar la rama de publicación anterior, tiene que hacerlo manualmente.

> [!NOTE]
> Data Factory solo lee el archivo `publish_config.json` cuando se carga la factoría. Si ya tiene la factoría cargada en el portal, actualice el explorador para que los cambios surtan efecto.

### <a name="publish-code-changes"></a>Cambios de código publicando

Después de haber combinado los cambios en la rama de colaboración (`master` es el valor predeterminado), seleccione **Publish** (Publicar) para publicar manualmente los cambios de código realizados en la rama principal en el servicio Data Factory.

![Publicación de cambios en el servicio Data Factory](media/author-visually/publish-changes.png)

Se abrirá un panel lateral en el que se confirma que la rama de publicación y los cambios pendientes son correctos. Cuando haya comprobado los cambios, haga clic en **OK** (Aceptar) para confirmar la publicación.

![Confirmar la rama de publicación correcta](media/author-visually/configure-publish-branch.png)

> [!IMPORTANT]
> La rama principal no es representativa de lo que se implementa en el servicio Data Factory. La rama principal se *debe* publicar manualmente en el servicio Data Factory.

## <a name="advantages-of-git-integration"></a>Ventajas de la integración de Git

-   **Control de código fuente**. A medida que las cargas de trabajo de la factoría de datos se vuelven cruciales, le convendría integrar la factoría con Git para aprovechar varias ventajas de control de código fuente como las siguientes:
    -   Capacidad para realizar el seguimiento y la auditoría de los cambios.
    -   Capacidad para revertir los cambios que presentaron errores.
-   **Operaciones de guardado parcial**. Al realizar muchos cambios en la factoría, se dará cuenta de que, en el modo real normal, no puede guardar los cambios como borrador, porque no está preparado o porque no quiere perder los cambios en caso de que su equipo se bloquee. Con la integración de Git, puede continuar guardando los cambios de forma incremental y publicarlos en la factoría solo cuando esté listo. Git actúa como un espacio de almacenamiento provisional para el trabajo, hasta que haya probado los cambios y estén a su gusto.
-   **Colaboración y control**. Si varios miembros del equipo participan en la misma factoría, puede que le convenga permitir que sus compañeros de equipo colaboren entre sí mediante un proceso de revisión de código. También puede configurar la factoría de manera que no todos sus colaboradores tengan permiso para implementar en ella. Es posible que los miembros del equipo solo puedan hacer cambios a través de Git, pero solo algunas personas del equipo puedan "Publicar" los cambios en la factoría.
-   **Muestra de las diferencias**. En el modo de Git, puede ver una clara diferencia de la carga útil que está a punto de publicar en la factoría. Esta diferencia muestra todos los recursos y entidades que se han modificado, agregado y eliminado desde la última vez que se publicó en la factoría. Según esta diferencia, puede continuar con la publicación o volver y comprobar los cambios, así como volver más tarde.
-   **Mejor proceso de CI/CD**. Si utiliza el modo de Git, puede configurar la canalización de versión para que se desencadene automáticamente en cuanto haya algún cambio en la factoría en desarrollo. También podrá personalizar las propiedades de la factoría que estén disponibles como parámetros en la plantilla de Resource Manager. Puede ser útil mantener solo el conjunto requerido de propiedades como parámetros, y tener todo lo demás codificado.
-   **Mejor rendimiento**. Una factoría media carga 10 veces más rápido en el modo de Git que en el modo real normal, porque los recursos se descargan a través de Git.

## <a name="best-practices-for-git-integration"></a>Procedimientos recomendados para la integración de Git

### <a name="permissions"></a>Permisos

Lo normal es que no quiera que todos los miembros del equipo tengan permisos para actualizar la factoría. Se recomienda la siguiente configuración de permisos:

*   Todos los miembros del equipo deberían tener permisos de lectura en la factoría de datos.
*   Solo se debe permitir que un conjunto seleccionado de personas publique en la factoría. Para ello, deben tener el rol **colaborador de Data Factory** en la factoría. Para más información sobre los permisos, consulte [Roles y permisos para Azure Data Factory](concepts-roles-permissions.md).
   
Se recomienda no permitir inserciones directas en el repositorio en la rama de colaboración. Esta restricción puede ayudarle a evitar errores, ya que cada inserción en el repositorio pasará por un proceso de solicitud de incorporación de cambios, tal como se describe en [Crear una rama de características](source-control.md#creating-feature-branches).

### <a name="using-passwords-from-azure-key-vault"></a>Uso de contraseñas de Azure Key Vault

Se recomienda usar Azure Key Vault para almacenar las cadenas de conexión o las contraseñas de los servicios vinculados de Data Factory. Por motivos de seguridad, no almacenamos ninguna información de secretos en Git, así que los cambios en los servicios vinculados se publican inmediatamente en el servicio Azure Data Factory.

El uso de Key Vault también facilita la integración y la implementación continuas, ya que no tendrá que proporcionar estos secretos durante la implementación de la plantilla de Resource Manager.

## <a name="troubleshooting-git-integration"></a>Solución de problemas de la integración de Git

### <a name="stale-publish-branch"></a>Rama de publicación obsoleta

Si la rama de publicación no está sincronizada con la rama principal y contiene recursos no actualizados a pesar de una publicación reciente, pruebe lo siguiente:

1. Quite el repositorio de Git actual.
1. Vuelva a configurar Git con los mismos valores, pero asegúrese de que la opción **Import existing Data Factory resources to repository** (Importar recursos existentes de Data Factory en el repositorio) esté seleccionada y elija **New branch** (Nueva rama).
1. Cree una solicitud de incorporación de cambios para combinar los cambios con la rama de colaboración. 

A continuación, se muestran algunos ejemplos de situaciones que pueden provocar una rama de publicación obsoleta:
- Un usuario tiene varias ramas. En una rama de características, eliminó un servicio vinculado que no está asociado a AKV (los servicios vinculados que no son AKV se publican inmediatamente, independientemente de si están en GIT o no) y nunca combinó la rama de características con la rama de colaboración.
- Un usuario modificó la factoría de datos mediante el SDK o PowerShell.
- Un usuario desplazó todos los recursos a una nueva rama e intentó publicarlos por primera vez. Los servicios vinculados se deben crear manualmente al importar los recursos.
- Un usuario carga un servicio vinculado que no es AKV o un archivo JSON de Integration Runtime manualmente. Hace referencia a ese recurso desde otro recurso, como un conjunto de datos, un servicio vinculado o una canalización. Un servicio vinculado que no es AKV creado través de la experiencia de usuario se publica inmediatamente porque se deben cifrar las credenciales. Si carga un conjunto de datos que haga referencia a ese servicio vinculado e intenta publicarlo, la experiencia de usuario lo permitirá porque existe en el entorno de GIT. Sin embargo, se rechazará en el momento de la publicación, ya que no existe en el servicio de la factoría de datos.

## <a name="provide-feedback"></a>Envío de comentarios
Seleccione **Comentarios** para comentar sobre las características o para notificar a Microsoft sobre los problemas con la herramienta:

![Comentarios](media/author-visually/provide-feedback.png)

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre la supervisión y la administración de canalizaciones, consulte el artículo [Supervisión y administración de canalizaciones mediante programación](monitor-programmatically.md).
* Para implementar la integración e implementación continuas, consulte [Integración y entrega continuas (CI/CD) en Azure Data Factory](continuous-integration-deployment.md).
