---
title: 'Tutorial: Publicación de un sitio de Hugo en Azure Static Web Apps'
description: Obtenga información sobre cómo implementar una aplicación de Hugo en Azure Static Web Apps.
services: static-web-apps
author: aaronpowell
ms.service: static-web-apps
ms.topic: tutorial
ms.date: 05/08/2020
ms.author: aapowell
ms.openlocfilehash: 5f511a898b3b2964f954ba150b05f02486456dcf
ms.sourcegitcommit: 2989396c328c70832dcadc8f435270522c113229
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92171494"
---
# <a name="tutorial-publish-a-hugo-site-to-azure-static-web-apps-preview"></a>Tutorial: Publicación de un sitio de Hugo en Azure Static Web Apps, versión preliminar

En este artículo se muestra cómo crear e implementar una aplicación web de [Hugo](https://gohugo.io/) en [Azure Static Web Apps](overview.md). El resultado final es una nueva aplicación de Azure Static Web Apps, con las Acciones de GitHub asociadas, que le da control sobre cómo se compila y publica la aplicación.

En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> - Crear una aplicación de Hugo
> - Configurar Azure Static Web Apps
> - Implementar la aplicación de Hugo en Azure

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

- Una cuenta de Azure con una suscripción activa. En caso de no tener una, puede [crear una cuenta gratuita](https://azure.microsoft.com/free/).
- Una cuenta de GitHub. En caso de no tener una, puede [crear una cuenta gratuita](https://github.com/join).

## <a name="create-a-hugo-app"></a>Creación de una aplicación de Hugo

Cree una aplicación de Hugo mediante la interfaz de la línea de comandos (CLI) de Hugo:

1. Siga la [guía de instalación](https://gohugo.io/getting-started/installing/) de Hugo en su sistema operativo.

1. Abra un terminal.

1. Ejecute la CLI de Hugo para crear una aplicación.

   ```bash
   hugo new site static-app
   ```

1. Vaya a la aplicación recién creada.

   ```bash
   cd static-app
   ```

1. Inicialice un repositorio de Git.

   ```bash
    git init
   ```

1. A continuación, agregue un tema al sitio; para ello, instale un tema como submódulo de Git y, luego, especifíquelo en el archivo de configuración de Hugo.

   ```bash
   git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
   echo 'theme = "ananke"' >> config.toml
   ```

1. Confirme los cambios.

   ```bash
   git add -A
   git commit -m "initial commit"
   ```

## <a name="push-your-application-to-github"></a>Inserción de la aplicación en GitHub

Necesita un repositorio en GitHub para conectarse a Azure Static Web Apps. En los pasos siguientes se muestra cómo crear un repositorio para el sitio.

1. Cree un repositorio de GitHub en blanco (no cree un archivo Léame) desde [https://github.com/new](https://github.com/new), denominado **hugo-static-app** .

1. Agregue el repositorio de GitHub como remoto al repositorio local. Asegúrese de agregar el nombre de usuario de GitHub en lugar del marcador de posición `<YOUR_USER_NAME>` en el comando siguiente.

   ```bash
   git remote add origin https://github.com/<YOUR_USER_NAME>/hugo-static-app
   ```

1. Inserte el repositorio local en GitHub.

   ```bash
   git push --set-upstream origin master
   ```

## <a name="deploy-your-web-app"></a>Implementación de la aplicación web

En los pasos siguientes se muestra cómo crear una aplicación de sitio estática e implementarla en un entorno de producción.

### <a name="create-the-application"></a>Creación de la aplicación

1. Vaya a [Azure Portal](https://portal.azure.com).
1. Haga clic en **Crear un recurso** .
1. Busque **Static Web Apps** .
1. Haga clic en **Static Web Apps (Preview)** (Static Web Apps [versión preliminar]).
1. Haga clic en **Crear**

   :::image type="content" source="./media/publish-hugo/create-in-portal.png" alt-text="Creación de un recurso de Azure Static Web Apps en el portal":::

1. En **Suscripción** , acepte la suscripción que aparece o seleccione otra en la lista desplegable.

1. En _Grupo de recursos_ , seleccione **Nuevo** . En _Nuevo nombre de grupo de recursos_ , escriba **hugo-static-app** y seleccione **Aceptar** .

1. Después, un nombre para la aplicación en el cuadro **Nombre** . Los caracteres válidos incluyen `a-z`, `A-Z`, `0-9` y `-`.

1. En _Región_ , seleccione una región cercana disponible.

1. En _SKU_ , seleccione **Gratis** .

   :::image type="content" source="./media/publish-hugo/basic-app-details.png" alt-text="Creación de un recurso de Azure Static Web Apps en el portal":::

1. Haga clic en el botón **Iniciar sesión con GitHub** .

1. Seleccione la **Organización** en la que creó el repositorio.

1. Seleccione **hugo-static-app** como _Repositorio_ .

1. En _Rama_ , seleccione **maestra** .

   :::image type="content" source="./media/publish-hugo/completed-github-info.png" alt-text="Creación de un recurso de Azure Static Web Apps en el portal":::

### <a name="build"></a>Build

A continuación, agregue los valores de configuración que el proceso de compilación usa para compilar la aplicación. La configuración siguiente define el archivo de flujo de trabajo de la Acción de GitHub.

1. Haga clic en el botón **Siguiente: Compilar >** para editar la configuración de compilación.

1. Establezca _Ubicación de la aplicación_ en **/**

1. Establezca _Ubicación del artefacto de la aplicación_ en **public** .

   No es necesario un valor para _Ubicación de la API_ , ya que por el momento no está implementando una API.

### <a name="review-and-create"></a>Revisar y crear

1. Haga clic en el botón **Revisar y crear** para comprobar que todos los detalles sean correctos.

1. Haga clic en **Crear** para comenzar la creación de la aplicación de Azure Static Web Apps y aprovisionar una Acción de GitHub para la implementación.

1. Espere a que se complete la Acción de GitHub.

1. En la ventana _Información general_ de Azure Portal para el recurso de Azure Static Web Apps recién creado, haga clic en el vínculo _Dirección URL_ para abrir la aplicación implementada.

   :::image type="content" source="./media/publish-hugo/deployed-app.png" alt-text="Creación de un recurso de Azure Static Web Apps en el portal":::

#### <a name="custom-hugo-version"></a>Versión de Hugo personalizada

Cuando se genera una aplicación web estática, se genera un [archivo de flujo de trabajo](./github-actions-workflow.md) que contiene los valores de configuración de publicación de la aplicación. Para designar una versión específica de Hugo en el archivo de flujo de trabajo, proporcione un valor para `HUGO_VERSION` en la sección `env`. En la configuración de ejemplo siguiente se muestra cómo establecer Hugo en una versión específica.

```yaml
jobs:
  build_and_deploy_job:
    if: github.event_name == 'push' || (github.event_name == 'pull_request' && github.event.action != 'closed')
    runs-on: ubuntu-latest
    name: Build and Deploy Job
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true
      - name: Build And Deploy
        id: builddeploy
        uses: Azure/static-web-apps-deploy@v0.0.1-preview
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN }}
          repo_token: ${{ secrets.GITHUB_TOKEN }} # Used for Github integrations (i.e. PR comments)
          action: "upload"
          ###### Repository/Build Configurations - These values can be configured to match you app requirements. ######
          # For more information regarding Static Web App workflow configurations, please visit: https://aka.ms/swaworkflowconfig
          app_location: "/" # App source code path
          api_location: "api" # Api source code path - optional
          app_artifact_location: "public" # Built app content directory - optional
          ###### End of Repository/Build Configurations ######
        env:
          HUGO_VERSION: 0.58.0
```

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [cleanup-resource](../../includes/static-web-apps-cleanup-resource.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Agregar un dominio personalizado](custom-domain.md)
