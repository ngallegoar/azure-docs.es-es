---
title: Configuración de la aplicación de funciones en Azure
description: Obtenga información sobre cómo definir la configuración de Azure Function App.
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.topic: conceptual
ms.date: 08/14/2019
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 662a04dbcc39f3fa95b0098eb8fe556b18b3495b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79235000"
---
# <a name="manage-your-function-app"></a>Administración de la aplicación de funciones 

En Azure Functions, una Function App ofrece el contexto de ejecución de funciones individuales. Los comportamientos de Function App se aplican a todas las funciones hospedadas en una Function App determinada. Todas las funciones de una aplicación de funciones deben compartir el mismo [lenguaje](supported-languages.md). 

Las funciones individuales de una aplicación de funciones se implementan y escalan juntas. Todas las funciones de la misma aplicación de funciones comparten recursos (por instancia) a medida que se escala la aplicación de funciones. 

Las cadenas de conexión, las variables de entorno y otras opciones de configuración de la aplicación se definen por separado para cada aplicación de funciones. Los datos que se deben compartir entre las aplicaciones de funciones deben almacenarse externamente en un almacén persistente.

En este artículo se describe cómo configurar y administrar la aplicación de funciones. 

> [!TIP]  
> Muchas opciones de configuración también se pueden administrar mediante la [CLI de Azure]. 

## <a name="get-started-in-the-azure-portal"></a>Introducción a Azure Portal

Para comenzar, vaya a [Azure Portal] e inicie sesión en la cuenta de Azure. En la barra de búsqueda en la parte superior del portal, escriba el nombre de la Function App y selecciónela en la lista. Después de seleccionar la Function App, vea la siguiente página:

![Información general sobre Function App en Azure Portal](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

Puede navegar a todo lo que necesita para administrar Function App desde la página de información general, en particular la **[Configuración de la aplicación](#settings)** y las **[Características de la plataforma](#platform-features)** .

## <a name="application-settings"></a><a name="settings"></a>Configuración de la aplicación

La pestaña **Configuración de la aplicación** mantiene la configuración de la aplicación que Function App utiliza. Esta configuración se almacena cifrada y debe seleccionar **Mostrar valores** para ver los valores en el portal. También puede acceder a esta configuración de la aplicación mediante la CLI de Azure.

### <a name="portal"></a>Portal

Para agregar una configuración en el portal, seleccione **Nueva configuración de la aplicación** y agregue el nuevo par clave-valor.

![Configuración de Function App en Azure Portal.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

### <a name="azure-cli"></a>Azure CLI

El comando [`az functionapp config appsettings list`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-list) devuelve la configuración de la aplicación existente, como en el ejemplo siguiente:

```azurecli-interactive
az functionapp config appsettings list --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME>
```

El comando [`az functionapp config appsettings set`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) agrega o actualiza una configuración de la aplicación. En el ejemplo siguiente se crea una configuración con una clave denominada `CUSTOM_FUNCTION_APP_SETTING` y un valor de `12345`:


```azurecli-interactive
az functionapp config appsettings set --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME> \
--settings CUSTOM_FUNCTION_APP_SETTING=12345
```

### <a name="use-application-settings"></a>Uso de la configuración de la aplicación

[!INCLUDE [functions-environment-variables](../../includes/functions-environment-variables.md)]

Al desarrollar una aplicación de funciones localmente, debe mantener copias locales de estos valores en el archivo del proyecto local.settings.json. Para más información, consulte el [archivo de configuración local](functions-run-local.md#local-settings-file).

## <a name="platform-features"></a>Características de la plataforma

![Pestaña Características de la plataforma de Function App](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

Las Function App se ejecutan en la plataforma de Azure App Service, donde también se realiza su mantenimiento. Por tanto, Function App tiene acceso a la mayoría de las características de la plataforma de hospedaje web principal de Azure. En la pestaña **Características de la plataforma** puede acceder a muchas características de la plataforma de App Service que puede usar en las Function App. 

> [!NOTE]
> No todas las características de App Service están disponibles cuando una Function App se ejecuta con el plan de hospedaje de consumo.

El resto de este artículo se centra en las siguientes características de App Service en Azure Portal que resultan útiles para Functions:

+ [Editor de App Service](#editor)
+ [Console](#console)
+ [Herramientas avanzadas (Kudu)](#kudu)
+ [Opciones de implementación](#deployment)
+ [CORS](#cors)
+ [Autenticación](#auth)

Para más información sobre cómo trabajar con la configuración de App Service, vea [Configuración de Azure App Service](../app-service/configure-common.md).

### <a name="app-service-editor"></a><a name="editor"></a>Editor de App Service

![Editor de App Service](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

El Editor de App Service es un editor en portal avanzado que puede usar para modificar archivos de configuración JSON y archivos de código similares. Al seleccionar esta opción se inicia una pestaña de explorador independiente con un editor básico. Esto le permite realizar la integración con el repositorio Git, ejecutar y depurar código y modificar la configuración de Function App. Este editor proporciona un entorno de desarrollo mejorado para las funciones en comparación con el editor de funciones integrado.  

Se recomienda que considere la posibilidad de desarrollar las funciones en el equipo local. Al desarrollar localmente y publicar en Azure, los archivos del proyecto son de solo lectura en el portal. Para más información, consulte [Codificación y comprobación de las funciones de Azure Functions en un entorno local](functions-develop-local.md).

### <a name="console"></a><a name="console"></a>Consola

![Consola de Function App](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

La consola del portal es una herramienta ideal para desarrolladores si prefiere interactuar con Function App desde la línea de comandos. Los comandos comunes incluyen creación de archivos y directorios y navegación por los mismos, así como la ejecución de archivos y scripts por lotes. 

Al desarrollar localmente, se recomienda usar [Azure Functions Core Tools](functions-run-local.md) y la [CLI de Azure].

### <a name="advanced-tools-kudu"></a><a name="kudu"></a>Herramientas avanzadas (Kudu)

![Configurar Kudu](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)

Las herramientas avanzadas para App Service (también conocidas como Kudu) proporcionan acceso a las características administrativas avanzadas de la Function App. Con Kudu, puede administrar la información del sistema, la configuración de las aplicaciones, las variables del entorno, las extensiones del sitio, los encabezados HTTP y las variables del servidor. También puede iniciar **Kudu** si examina el punto de conexión de SCM de la Function App, como `https://<myfunctionapp>.scm.azurewebsites.net/`. 


### <a name="deployment-center"></a><a name="deployment"></a>Centro de implementación

Cuando se usa una solución de control de código fuente para desarrollar y mantener el código de las funciones, el Centro de implementación le permite compilar e implementar desde el control de código fuente. El proyecto se compila e implementa en Azure cuando se realizan actualizaciones. Para más información, vea [Tecnologías de implementación en Azure Functions](functions-deployment-technologies.md).

### <a name="cross-origin-resource-sharing"></a><a name="cors"></a>Uso compartido de recursos entre orígenes

Para evitar la ejecución de código malintencionado en el cliente, los exploradores modernos bloquean las solicitudes de las aplicaciones web en los recursos que se ejecutan en un dominio independiente. El [Intercambio de Recursos de Origen Cruzado (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS) permite a un encabezado `Access-Control-Allow-Origin` declarar qué orígenes pueden llamar a los puntos de conexión en la aplicación de funciones.

#### <a name="portal"></a>Portal

Al configurar la lista de **orígenes permitidos** para la aplicación de funciones, el encabezado `Access-Control-Allow-Origin` se agrega automáticamente a todas las respuestas de los puntos de conexión HTTP en la aplicación de funciones. 

![Configuración de la lista de CORS de la aplicación de funciones](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

Cuando se usa el carácter comodín (`*`), se omiten todos los demás dominios. 

Use el comando [`az functionapp cors add`](/cli/azure/functionapp/cors#az-functionapp-cors-add) para agregar un dominio a la lista de orígenes permitidos. En el ejemplo siguiente se agrega el dominio contoso.com:

```azurecli-interactive
az functionapp cors add --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME> \
--allowed-origins https://contoso.com
```

Use el comando [`az functionapp cors show`](/cli/azure/functionapp/cors#az-functionapp-cors-show) para enumerar los orígenes permitidos actuales.

### <a name="authentication"></a><a name="auth"></a>Autenticación

![Configuración de la autenticación de una Function App](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)

Si las funciones usan un desencadenador HTTP, puede requerir que las llamadas se autentiquen primero. App Service admite la autenticación de Azure Active Directory y el inicio de sesión en proveedores locales, como Facebook, Microsoft y Twitter. Para más información sobre cómo configurar los proveedores de autenticación específicos, consulte [Autenticación y autorización en Azure App Service](../app-service/overview-authentication-authorization.md). 


## <a name="next-steps"></a>Pasos siguientes

+ [Configuración de Azure App Service](../app-service/configure-common.md)
+ [Implementación continua para Azure Functions](functions-continuous-deployment.md)

[CLI de Azure]: /cli/azure/
[Azure Portal]: https://portal.azure.com
