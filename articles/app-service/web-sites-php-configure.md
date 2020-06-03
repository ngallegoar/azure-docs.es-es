---
title: Configuración del tiempo de ejecución de PHP
description: Aprenda a configurar la instalación de PHP predeterminada o a agregar una instalación de PHP personalizada para Azure App Service.
author: msangapu-msft
ms.assetid: 95c4072b-8570-496b-9c48-ee21a223fb60
ms.devlang: php
ms.topic: article
ms.date: 04/13/2020
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: 0605249ea0602b33e144fce8d0a77439c2077a2f
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84170756"
---
# <a name="configure-php-in-azure-app-service"></a>Configuración de PHP en Azure App Service

## <a name="introduction"></a>Introducción

En esta guía se explica cómo configurar el entorno de ejecución de PHP integrado para aplicaciones web y aplicaciones de API en [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714), facilitar un entorno de ejecución de PHP personalizado y habilitar extensiones. Para utilizar App Service, regístrese para obtener acceso a la [evaluación gratuita]. Para sacar el máximo partido a esta guía, primero debe crear una aplicación PHP en App Service.

## <a name="how-to-change-the-built-in-php-version"></a>Procedimientos: Cambiar la versión de PHP integrada

Al crear una aplicación web, puede elegir la versión de PHP que se configurará. Consulte [PHP en App Service](https://github.com/Azure/app-service-linux-docs/blob/master/Runtime_Support/php_support.md) para obtener información actualizada sobre las versiones admitidas actualmente.

Para comprobar la versión existente del entorno de ejecución de la aplicación, puede implementar un script que llame a la función [phpinfo()].

Para actualizar la versión de PHP, siga uno de estos métodos:

### <a name="azure-portal"></a>Portal de Azure

1. Vaya a la aplicación en [Azure Portal](https://portal.azure.com) y desplácese hasta la página **Configuración**.

2. En **Configuración**, seleccione **Configuración general** y elija la nueva versión de PHP.

3. Haga clic en el botón **Guardar** de la parte superior de la hoja **Configuración general**.

### <a name="azure-cli"></a>Azure CLI 

Para usar la interfaz de la línea de comandos de Azure, debe [instalar la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) en el equipo.

1. Abra Terminal e inicie sesión en su cuenta.

        az login

1. Compruebe la lista de tiempos de ejecución compatibles.

        az webapp list-runtimes | grep php

2. Establezca la versión PHP de la aplicación.

        az webapp config set --php-version {5.6 | 7.2 | 7.3} --name {app-name} --resource-group {resource-group-name}

3. La versión de PHP ya está configurada. Puede confirmar esta configuración:

        az webapp show --name {app-name} --resource-group {resource-group-name}

## <a name="how-to-change-the-built-in-php-configurations"></a>Procedimientos: Cambiar las configuraciones de PHP integradas

En todos los tiempos de ejecución de PHP integrados es posible cambiar las opciones de configuración siguiendo los pasos que se indican a continuación. (Para obtener información acerca de las directivas, consulte la [lista de directivas de php.ini]).

### <a name="changing-php_ini_user-php_ini_perdir-php_ini_all-configuration-settings"></a>Cambio de PHP\_INI\_USUARIO, PHP\_INI\_PERDIR, PHP\_INI\_todas LAS opciones de configuración

1. Agregue un archivo [.user.ini] al directorio raíz.
1. Agregue la configuración al archivo `.user.ini` usando la misma sintaxis que usaría en un archivo `php.ini`. Por ejemplo, si quiere desactivar la configuración `display_errors` y establecer la configuración `upload_max_filesize` en 10 M, el archivo `.user.ini` contendría el texto siguiente:

        ; Example Settings
        display_errors=On
        upload_max_filesize=10M

        ; OPTIONAL: Turn this on to write errors to d:\home\LogFiles\php_errors.log
        ; log_errors=On
2. Implemente la aplicación.
3. Reinicie la aplicación. (Es necesario reiniciar porque la frecuencia con la que PHP lee los archivos `.user.ini` depende de la configuración `user_ini.cache_ttl`, que es una configuración a nivel de sistema con un valor predeterminado de 300 segundos (5 minutos). El reinicio de la aplicación fuerza a PHP a leer la nueva configuración del archivo `.user.ini`).

Como alternativa al uso de un archivo `.user.ini`, puede usar la función [ini_set()] en los scripts para definir las opciones de configuración que no son directivas a nivel de sistema.

### <a name="changing-php_ini_system-configuration-settings"></a>Cambiar la configuración de PHP\_INI\_SYSTEM

1. Agregue una configuración a la aplicación con la clave `PHP_INI_SCAN_DIR` y el valor `d:\home\site\ini`.
1. Cree un archivo `settings.ini` con la consola Kudu (http://&lt;site-nombre&gt;.scm.azurewebsite.net) en el directorio `d:\home\site\ini`.
1. Agregue la configuración al archivo `settings.ini` usando la misma sintaxis que usaría en un archivo `php.ini`. Por ejemplo, si desea señalar la configuración `curl.cainfo` en un archivo `*.crt` y establecer la configuración de "wincache.maxfilesize" en 512 KB, su archivo `settings.ini` debería contener este texto:

        ; Example Settings
        curl.cainfo="%ProgramFiles(x86)%\Git\bin\curl-ca-bundle.crt"
        wincache.maxfilesize=512
1. Para volver a cargar los cambios, reinicie la aplicación.

## <a name="how-to-enable-extensions-in-the-default-php-runtime"></a>Procedimientos: Habilitar las extensiones en el tiempo de ejecución predeterminado de PHP

Como se ha mencionado en la sección anterior, la mejor forma de ver la versión de PHP predeterminada, su configuración predeterminada y las extensiones habilitadas es implementar un script que llame a la función [phpinfo()]. Para habilitar extensiones adicionales, siga los pasos que se detallan a continuación:

### <a name="configure-via-ini-settings"></a>Configurar mediante configuración de ini

1. Agregue un directorio `ext` al directorio `d:\home\site`.
1. Coloque los archivos con extensión `.dll` en el directorio `ext` (por ejemplo, `php_xdebug.dll`). Asegúrese de que las extensiones sean compatibles con la versión predeterminada de PHP y que sean compatibles con VC9 y no seguras para subprocesos (nts).
1. Agregue una configuración a la aplicación con la clave `PHP_INI_SCAN_DIR` y el valor `d:\home\site\ini`.
1. Cree un archivo `ini` en `d:\home\site\ini` denominado `extensions.ini`.
1. Agregue la configuración al archivo `extensions.ini` usando la misma sintaxis que usaría en un archivo `php.ini`. Por ejemplo, si desea habilitar las extensiones de MongoDB y XDebug su archivo `extensions.ini` debería contener este texto:

        ; Enable Extensions
        extension=d:\home\site\ext\php_mongo.dll
        zend_extension=d:\home\site\ext\php_xdebug.dll
1. Reinicie la aplicación para cargar los cambios.

### <a name="configure-via-app-setting"></a>Configurar a través de la configuración de la aplicación

1. Agregue un directorio `bin` al directorio raíz.
2. Coloque los archivos con extensión `.dll` en el directorio `bin` (por ejemplo, `php_xdebug.dll`). Asegúrese de que las extensiones sean compatibles con la versión predeterminada de PHP y que sean compatibles con VC9 y no seguras para subprocesos (nts).
3. Implemente la aplicación.
4. Vaya a la aplicación en Azure Portal y haga clic en **Configuración**, situado debajo de la sección **Configuración**.
5. En la hoja **Configuración**, seleccione **Configuración de la aplicación**.
6. En la sección **Configuración de la aplicación**, haga clic en **+ Nueva configuración de la aplicación** y cree una clave **PHP_EXTENSIONS**. El valor de esta clave es una ruta de acceso relativa a la raíz del sitio web: **bin\your-ext-file**.
7. Haga clic en el botón **Actualizar** situado en la parte inferior y después en **Guardar**, por encima de la pestaña **Configuración de la aplicación**.

También se pueden usar las extensiones Zend mediante una clave **PHP_ZENDEXTENSIONS**. Para habilitar varias extensiones, incluya una lista separada por comas de los archivos `.dll` como valor de configuración de aplicaciones.

## <a name="how-to-use-a-custom-php-runtime"></a>Procedimientos: Usar un tiempo de ejecución de PHP personalizado

En lugar del tiempo de ejecución de PHP, App Service puede usar un tiempo de ejecución de PHP facilitado por el usuario para ejecutar scripts PHP. El tiempo de ejecución que facilita se puede configurar mediante un archivo `php.ini` que también usted facilita. Para usar un tiempo de ejecución de PHP personalizado con App Service, siga estos pasos.

1. Obtenga una versión compatible con VC9 y VC11 no segura para subprocesos de PHP para Windows. Las versiones recientes de PHP para Windows se pueden encontrar aquí: [https://windows.php.net/download/]. Las versiones anteriores se pueden encontrar en el archivo aquí: [https://windows.php.net/downloads/releases/archives/].
2. Modifique el archivo `php.ini` para el tiempo de ejecución. App Service omite las opciones de configuración de las directivas que solo se aplican en el nivel del sistema. (Para obtener información acerca de las directivas solo a nivel de sistema, consulte la [lista de directivas de php.ini]).
3. También puede agregar extensiones al tiempo de ejecución de PHP y habilitarlas en el archivo `php.ini` .
4. Agregue un directorio `bin` al directorio raíz y coloque en él el directorio que contiene el tiempo de ejecución de PHP (por ejemplo, `bin\php`).
5. Implemente la aplicación.
6. Vaya a la aplicación en Azure Portal y haga clic en la hoja **Configuración**.
8. En la hoja **Configuración**, seleccione **Asignaciones de ruta de acceso**. 
9. Haga clic en **+ Nuevo controlador** y agregue `*.php` al campo Extensión y la ruta de acceso al ejecutable `php-cgi.exe` en **Procesador de script**. Si coloca el tiempo de ejecución de PHP en el directorio `bin` de la raíz de la aplicación, la ruta de acceso será `D:\home\site\wwwroot\bin\php\php-cgi.exe`.
10. En la parte inferior, haga clic en **Actualizar** para terminar de agregar la asignación de controlador.
11. Haga clic en **Guardar** para guardar los cambios.

<a name="composer"></a>

## <a name="how-to-enable-composer-automation-in-azure"></a>Procedimientos: Habilitar la automatización de Composer en Azure

De forma predeterminada, App Service no responde con composer.json, si tiene uno en el proyecto PHP. Si usa la [implementación de Git](deploy-local-git.md), puede habilitar el procesamiento de composer.json durante `git push` mediante la habilitación de la extensión de Composer.

> [!NOTE]
> Aquí puede [votar por soporte técnico de primera clase para Composer en App Service](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip).
>

1. En la hoja de la aplicación de PHP de [Azure Portal](https://portal.azure.com), haga clic en **Herramientas** > **Extensiones**.

    ![Hoja de configuración de Azure Portal para habilitar la automatización de Compositor en Azure](./media/web-sites-php-configure/composer-extension-settings.png)
2. Haga clic en **Agregar** y, luego, en **Compositor**.

    ![Agregar la extensión Composer para habilitar la automatización de Composer en Azure](./media/web-sites-php-configure/composer-extension-add.png)
3. Haga clic en **Aceptar** para aceptar las condiciones legales. Haga clic de nuevo en **Aceptar** para agregar la extensión.

    La hoja **Extensiones instaladas** mostrará ahora la extensión Compositor.
    ![Aceptar los términos legales para habilitar la automatización de Composer en Azure](./media/web-sites-php-configure/composer-extension-view.png)
4. Ahora, en una ventana de terminal del equipo local, ejecute `git add`, `git commit` y `git push` en la aplicación. Verá que Compositor está instalando las dependencias definidas en composer.json.

    ![Implementación de GIT con la automatización de Composer en Azure](./media/web-sites-php-configure/composer-extension-success.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte el [Centro para desarrolladores de PHP](https://azure.microsoft.com/develop/php/).

[evaluación gratuita]: https://www.windowsazure.com/pricing/free-trial/
[phpinfo()]: https://php.net/manual/en/function.phpinfo.php
[select-php-version]: ./media/web-sites-php-configure/select-php-version.png
[lista de directivas de php.ini]: https://www.php.net/manual/en/ini.list.php
[.user.ini]: https://www.php.net/manual/en/configuration.file.per-user.php
[ini_set()]: https://www.php.net/manual/en/function.ini-set.php
[application-settings]: ./media/web-sites-php-configure/application-settings.png
[settings-button]: ./media/web-sites-php-configure/settings-button.png
[save-button]: ./media/web-sites-php-configure/save-button.png
[php-extensions]: ./media/web-sites-php-configure/php-extensions.png
[handler-mappings]: ./media/web-sites-php-configure/handler-mappings.png
[https://windows.php.net/download/]: https://windows.php.net/download/
[https://windows.php.net/downloads/releases/archives/]: https://windows.php.net/downloads/releases/archives/
[SETPHPVERCLI]: ./media/web-sites-php-configure/ChangePHPVersion-XPlatCLI.png
[GETPHPVERCLI]: ./media/web-sites-php-configure/ShowPHPVersion-XplatCLI.png
[SETPHPVERPS]: ./media/web-sites-php-configure/ChangePHPVersion-PS.png
[GETPHPVERPS]: ./media/web-sites-php-configure/ShowPHPVersion-PS.png
