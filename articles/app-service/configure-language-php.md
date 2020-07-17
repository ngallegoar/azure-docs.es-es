---
title: Configuración de aplicaciones PHP de Windows
description: Aprenda a configurar una aplicación de PHP en las instancias nativas de Windows de App Service. En este artículo se muestran las tareas de configuración más comunes.
ms.devlang: php
ms.topic: article
ms.date: 06/02/2020
ms.openlocfilehash: 1eb4e9d349fdd0097cbde4e4cef3d5c61a167193
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84907990"
---
# <a name="configure-a-windows-php-app-for-azure-app-service"></a>Configuración de una aplicación de PHP en Windows para Azure App Service

En esta guía, se explica cómo se configuran las aplicaciones web de PHP, los back-ends para dispositivos móviles y las aplicaciones de API de Azure App Service. Esta guía se aplica a las aplicaciones hospedadas en la plataforma Windows. Para obtener información sobre las aplicaciones de Linux, consulte [Configuración de una aplicación de PHP en Linux para Azure App Service](containers/configure-language-php.md).

Esta guía incluye conceptos clave e instrucciones para los desarrolladores de PHP que implementan aplicaciones en App Service. Si nunca ha usado Azure App Service, siga primero la [guía de inicio rápido de PHP](app-service-web-get-started-php.md) y el [tutorial de PHP con MySQL](app-service-web-tutorial-php-mysql.md).

## <a name="show-php-version"></a>Visualización de la versión de PHP

Para mostrar la versión actual de PHP, ejecute el siguiente comando en [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query phpVersion
```

Para mostrar todas las versiones compatibles de PHP, ejecute el siguiente comando en [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp list-runtimes | grep php
```

## <a name="set-php-version"></a>Establecer la versión de PHP

Ejecute el siguiente comando en [Cloud Shell](https://shell.azure.com) para establecer la versión 7.4 de PHP:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --php-version 7.4
```

## <a name="run-composer"></a>Ejecución de Composer

Si quiere que App Service ejecute [Composer](https://getcomposer.org/) en el momento de la implementación, la manera más fácil es incluir Composer en el repositorio.

En una ventana de terminal local, cambie el directorio a la raíz del repositorio y siga las instrucciones que se indican en [Descarga de Composer](https://getcomposer.org/download/) para descargar *composer.phar* a la raíz del directorio.

Ejecute los siguientes comandos (necesita tener instalado [npm](https://www.npmjs.com/get-npm)):

```bash
npm install kuduscript -g
kuduscript --node --scriptType bash --suppressPrompt
```

La raíz del repositorio ahora tiene dos archivos adicionales: *.deployment* y *deploy.sh*.

Abra *deploy.sh* y busque la sección `Deployment`, que tiene el siguiente aspecto:

```bash
##################################################################################################################################
# Deployment
# ----------
```

Agregue la sección de código que necesita para ejecutar la herramienta necesaria *al final* de la sección `Deployment`:

```bash
# 4. Use composer
echo "$DEPLOYMENT_TARGET"
if [ -e "$DEPLOYMENT_TARGET/composer.json" ]; then
  echo "Found composer.json"
  pushd "$DEPLOYMENT_TARGET"
  php composer.phar install $COMPOSER_ARGS
  exitWithMessageOnError "Composer install failed"
  popd
fi
```

Confirme todos los cambios e implemente el código mediante Git o realice la implementación desde un archivo ZIP con la automatización de compilaciones habilitada. Composer ahora debería estar funcionando como parte de la automatización de la implementación.

## <a name="run-gruntbowergulp"></a>Ejecutar Grunt, Bower o Gulp

Si quiere que App Service ejecute herramientas de automatización populares en el momento de la implementación, como Grunt, Bower o Gulp, deberá suministrar un [script de implementación personalizado](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script). App Service ejecuta este script cuando se implementa con Git o con la [implementación desde un archivo ZIP](deploy-zip.md) con la automatización de compilaciones habilitada. 

Para habilitar que el repositorio ejecute estas herramientas, deberá agregarlas a las dependencias en el archivo *package.json.* Por ejemplo:

```json
"dependencies": {
  "bower": "^1.7.9",
  "grunt": "^1.0.1",
  "gulp": "^3.9.1",
  ...
}
```

Desde una ventana de terminal local, cambie el directorio a la raíz del repositorio y ejecute los siguientes comandos (debe tener instalado [npm](https://www.npmjs.com/get-npm)):

```bash
npm install kuduscript -g
kuduscript --node --scriptType bash --suppressPrompt
```

La raíz del repositorio ahora tiene dos archivos adicionales: *.deployment* y *deploy.sh*.

Abra *deploy.sh* y busque la sección `Deployment`, que tiene el siguiente aspecto:

```bash
##################################################################################################################################
# Deployment
# ----------
```

En esta sección termina con la ejecución de `npm install --production`. Agregue la sección de código que necesita para ejecutar la herramienta necesaria *al final* de la sección `Deployment`:

- [Bower](#bower)
- [Gulp](#gulp)
- [Grunt](#grunt)

Consulte un [ejemplo en la muestra de MEAN.js](https://github.com/Azure-Samples/meanjs/blob/master/deploy.sh#L112-L135), donde el script de implementación también ejecuta un comando `npm install` personalizado.

### <a name="bower"></a>Bower

Este fragmento de código ejecuta `bower install`.

```bash
if [ -e "$DEPLOYMENT_TARGET/bower.json" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/bower install
  exitWithMessageOnError "bower failed"
  cd - > /dev/null
fi
```

### <a name="gulp"></a>Gulp

Este fragmento de código ejecuta `gulp imagemin`.

```bash
if [ -e "$DEPLOYMENT_TARGET/gulpfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/gulp imagemin
  exitWithMessageOnError "gulp failed"
  cd - > /dev/null
fi
```

### <a name="grunt"></a>Grunt

Este fragmento de código ejecuta `grunt`.

```bash
if [ -e "$DEPLOYMENT_TARGET/Gruntfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/grunt
  exitWithMessageOnError "Grunt failed"
  cd - > /dev/null
fi
```

## <a name="access-environment-variables"></a>Acceso a variables de entorno

En App Service, puede [establecer la configuración de la aplicación](configure-common.md#configure-app-settings) fuera del código de la aplicación. Luego podrá acceder a ellas mediante el patrón estándar de [getenv()](https://secure.php.net/manual/function.getenv.php). Por ejemplo, para acceder a una configuración de una aplicación denominada `DB_HOST`, use el código siguiente:

```php
getenv("DB_HOST")
```

## <a name="change-site-root"></a>Cambio de la raíz del sitio

La plataforma web que prefiera puede usar un subdirectorio como la raíz del sitio. Por ejemplo, [Laravel](https://laravel.com/) usa el subdirectorio *public/* como la raíz del sitio.

Para personalizar la raíz del sitio, establezca la ruta de acceso de la aplicación virtual para la aplicación mediante el comando [`az resource update`](/cli/azure/resource#az-resource-update). En el ejemplo siguiente se establece la raíz del sitio en el subdirectorio *public/* del repositorio. 

```azurecli-interactive
az resource update --name web --resource-group <group-name> --namespace Microsoft.Web --resource-type config --parent sites/<app-name> --set properties.virtualApplications[0].physicalPath="site\wwwroot\public" --api-version 2015-06-01
```

De manera predeterminada, Azure App Service apunta la ruta de acceso de la aplicación virtual raíz ( _/_ ) al directorio raíz de los archivos de la aplicación implementada (_sites\wwwroot_).

## <a name="detect-https-session"></a>Detección de sesión de HTTPS

En App Service, la [terminación de SSL](https://wikipedia.org/wiki/TLS_termination_proxy) se produce en los equilibradores de carga de red, por lo que todas las solicitudes HTTPS llegan a su aplicación en forma de solicitudes HTTP sin cifrar. Si su aplicación lógica necesita comprobar si las solicitudes de usuario están cifradas, inspeccione el encabezado `X-Forwarded-Proto`.

```php
if (isset($_SERVER['X-Forwarded-Proto']) && $_SERVER['X-Forwarded-Proto'] === 'https') {
  // Do something when HTTPS is used
}
```

Los marcos web más usados le permiten acceder a la información de `X-Forwarded-*` en el patrón de aplicación estándar. En [CodeIgniter](https://codeigniter.com/), [is_https()](https://github.com/bcit-ci/CodeIgniter/blob/master/system/core/Common.php#L338-L365) comprueba el valor de `X_FORWARDED_PROTO` de forma predeterminada.

## <a name="customize-phpini-settings"></a>Personalización de la configuración de php.ini

Si necesita hacer cambios en su instalación de PHP, puede cambiar cualquiera de las [directivas php.ini](https://www.php.net/manual/ini.list.php) siguiendo estos pasos.

> [!NOTE]
> La mejor forma de ver la versión de PHP y la configuración de *php.ini* actual consiste en llamar a [phpinfo()](https://php.net/manual/function.phpinfo.php) en la aplicación.
>

### <a name="customize-non-php_ini_system-directives"></a><a name="Customize-non-PHP_INI_SYSTEM directives"></a>Personalización de las directivas que no son de PHP_INI_SYSTEM

Para personalizar las directivas PHP_INI_USER, PHP_INI_PERDIR y PHP_INI_ALL (consulte las [directivas de php.ini](https://www.php.net/manual/ini.list.php)), agregue un archivo `.user.ini` al directorio raíz de la aplicación.

Agregue la configuración al archivo `.user.ini` usando la misma sintaxis que usaría en un archivo `php.ini`. Por ejemplo, si quiere desactivar la configuración `display_errors` y establecer la configuración `upload_max_filesize` en 10 M, el archivo `.user.ini` contendría el texto siguiente:

```
 ; Example Settings
 display_errors=On
 upload_max_filesize=10M

 ; Write errors to d:\home\LogFiles\php_errors.log
 ; log_errors=On
```

Vuelva a implementar la aplicación con los cambios y reiníciela.

Como alternativa al uso del archivo `.user.ini`, puede usar [ini_set()](https://www.php.net/manual/function.ini-set.php) en la aplicación para personalizar estas directivas que no son de PHP_INI_SYSTEM.

### <a name="customize-php_ini_system-directives"></a><a name="customize-php_ini_system-directives"></a>Personalización de las directivas de PHP_INI_SYSTEM

Para personalizar las directivas de PHP_INI_SYSTEM (consulte [directivas de php.ini](https://www.php.net/manual/ini.list.php)), no puede usar el enfoque *.htaccess*. App Service proporciona un mecanismo diferente mediante la configuración de la aplicación `PHP_INI_SCAN_DIR`.

En primer lugar, ejecute el siguiente comando en [Cloud Shell](https://shell.azure.com) para agregar una configuración de aplicación llamada `PHP_INI_SCAN_DIR`:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PHP_INI_SCAN_DIR="d:\home\site\ini"
```

Vaya a la consola de Kudu (`https://<app-name>.scm.azurewebsites.net/DebugConsole`) y navegue a `d:\home\site`.

Cree un directorio en `d:\home\site` llamado `ini`, después cree un archivo *.ini* en el directorio `d:\home\site\ini` (por ejemplo, *settings.ini)* con las directivas que quiera personalizar. Utilice la misma sintaxis que usaría en un archivo *php.ini*. 

Por ejemplo, para cambiar el valor de [expose_php](https://php.net/manual/ini.core.php#ini.expose-php), ejecute los siguientes comandos:

```bash
cd /home/site
mkdir ini
echo "expose_php = Off" >> ini/setting.ini
```

Para que los cambios surtan efecto, reinicie la aplicación.

## <a name="enable-php-extensions"></a>Habilitación de las extensiones de PHP

Las instalaciones de PHP integradas contienen las extensiones más usadas. Puede habilitar extensiones adicionales en la misma forma que [personaliza las directivas de php.ini](#customize-php_ini_system-directives).

> [!NOTE]
> La mejor forma de ver la versión de PHP y la configuración de *php.ini* actual consiste en llamar a [phpinfo()](https://php.net/manual/function.phpinfo.php) en la aplicación.
>

Para habilitar extensiones adicionales, siga los pasos que se detallan a continuación:

Agregue un directorio `bin` al directorio raíz de la aplicación y coloque los archivos de extensión `.so` en él (por ejemplo, *mongodb.so*). Asegúrese de que las extensiones sean compatibles con la versión de PHP en Azure y que sean compatibles con VC9 y no seguras para subprocesos (nts).

Implemente los cambios

Siga los pasos que se indican en [Personalización de las directivas de PHP_INI_SYSTEM](#customize-php_ini_system-directives), agregue las extensiones al archivo *custom.ini* con las directivas [extension](https://www.php.net/manual/ini.core.php#ini.extension) o [zend_extension](https://www.php.net/manual/ini.core.php#ini.zend-extension).

```
extension=d:\home\site\wwwroot\bin\mongodb.so
zend_extension=d:\home\site\wwwroot\bin\xdebug.so
```

Para que los cambios surtan efecto, reinicie la aplicación.

## <a name="access-diagnostic-logs"></a>Acceso a los registros de diagnóstico

Use la utilidad [error_log()](https://php.net/manual/function.error-log.php) estándar para que los registros de diagnóstico se muestren en Azure App Service.

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-no-h.md)]

## <a name="troubleshooting"></a>Solución de problemas

Cuando una aplicación de PHP en funcionamiento se comporta de manera diferente en App Service o genera errores, intente lo siguiente:

- [Acceso a la secuencia de registros](#access-diagnostic-logs).
- Pruebe la aplicación localmente en modo de producción. App Service ejecuta la aplicación en el modo de producción, por lo que deberá asegurarse de que el proyecto funciona según lo previsto en modo de producción localmente. Por ejemplo:
    - Algunas plataformas web pueden implementar archivos estáticos de forma diferente en modo de producción.
    - Algunas plataformas web pueden usar scripts de inicio personalizados cuando se ejecutan en modo de producción.
- Ejecute la aplicación en App Service en el modo de depuración. Por ejemplo, en [Laravel](https://meanjs.org/), puede configurar la aplicación para que genere mensajes de depuración en producción al [configurar el ajuste de la aplicación `APP_DEBUG` en `true`](configure-common.md#configure-app-settings).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Aplicación de PHP con MySQL](app-service-web-tutorial-php-mysql.md)
