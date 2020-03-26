---
title: Creación de una aplicación de funciones en Linux desde Azure Portal
description: Obtenga información sobre cómo crear su primera función de Azure para su ejecución sin servidor mediante Azure Portal.
ms.topic: quickstart
ms.date: 02/28/2019
ms.openlocfilehash: 1492188f72eb4a691ddceb78aa269601f192e467
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "76963842"
---
# <a name="create-a-function-app-on-linux-in-an-azure-app-service-plan"></a>Creación de una aplicación de función en Linux en un plan de Azure App Service

Azure Functions permite hospedar sus funciones en Linux en un contenedor de Azure App Service predeterminado. En este artículo se describe el uso de [Azure Portal](https://portal.azure.com) para crear una aplicación de función hospedada en Linux que se ejecute en un plan de [App Service](functions-scale.md#app-service-plan). También puede [traer su propio contenedor personalizado](functions-create-function-linux-custom-image.md).

![Creación de una aplicación de función en Azure Portal](./media/create-function-app-linux-app-service-plan/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en <https://portal.azure.com> con su cuenta de Azure.

## <a name="create-a-function-app"></a>Creación de una aplicación de función

Debe tener una aplicación de función para hospedar la ejecución de las funciones en Linux. La aplicación de función proporciona un entorno para la ejecución del código de su aplicación. Le permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación, el escalado y el uso compartido de recursos. En este artículo, creará un plan de App Service al crear la aplicación de función.

1. Seleccione el botón **Crear un recurso** de la esquina superior izquierda de Azure Portal y, después, **Proceso** > **Function App**.

    ![Creación de una aplicación de función en Azure Portal](./media/create-function-app-linux-app-service-plan/function-app-create-flow.png)

2. Use la configuración de Function App especificada en la tabla debajo de la imagen.

    ![Definir nueva configuración de Function App](./media/create-function-app-linux-app-service-plan/function-app-create-flow2.png)

    | Configuración      | Valor sugerido  | Descripción                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nombre de la aplicación** | Nombre único globalmente | Nombre que identifica la nueva Function App. Los caracteres válidos son `a-z`, `0-9` y `-`.  | 
    | **Suscripción** | Su suscripción | Suscripción en la que se creará esta nueva aplicación de función. | 
    | **[Grupo de recursos](../azure-resource-manager/management/overview.md)** |  myResourceGroup | Nombre para el nuevo grupo de recursos en el que se va a crear la Function App. |
    | **SISTEMA OPERATIVO** | Linux | La aplicación de función se ejecuta en Linux. |
    | **Publicar** | Código | Se usa el contenedor de Linux predeterminado para su **pila en tiempo de ejecución**. Todo lo que tiene que proporcionar es el código de proyecto de aplicación de función. Otra opción consiste en publicar una [imagen de Docker](functions-create-function-linux-custom-image.md) personalizada. |
    | **[Plan de hospedaje](functions-scale.md)** | Plan de App Service | Plan de hospedaje que define cómo se asignan los recursos a la Function App. Si usa un plan de App Service, puede controlar el [escalado de la aplicación de función](functions-scale.md).  |
    | **Plan de App Service/ubicación** | Creación del plan | Elija **Crear nuevo** y proporcionar un nombre para **Plan de App Service**. Elija una **ubicación** en una [región](https://azure.microsoft.com/regions/) próxima a usted o a otros servicios a los que accedan las funciones. Elija su **[plan de tarifa](https://azure.microsoft.com/pricing/details/app-service/linux/)** deseado. <br/>No puede ejecutar aplicaciones de función de Windows y Linux en el mismo plan de App Service. |
    | **Pila en tiempo de ejecución** | Lenguaje preferido | Elija un tiempo de ejecución que admita su lenguaje de programación de funciones preferido. Elija **.NET** para funciones de C# y F#. |
    | **[Storage](../storage/common/storage-account-create.md)** |  Nombre único globalmente |  Cree una cuenta de almacenamiento que use la aplicación de función. Los nombres de las cuentas de almacenamiento deben tener entre 3 y 24 caracteres y solo pueden incluir números y letras en minúscula. También puede usar una cuenta existente que debe cumplir los [requisitos de la cuenta de almacenamiento](storage-considerations.md#storage-account-requirements). |
    | **[Application Insights](functions-monitoring.md)** | habilitado | Application Insights está deshabilitado de forma predeterminada. Se recomienda habilitar la integración de Application Insights ahora y elegir una ubicación de hospedaje cerca de la ubicación del plan de App Service. Si quiere hacer esto más tarde, consulte [Supervisión de Azure Functions](functions-monitoring.md).  |

3. Seleccione **Crear** para aprovisionar e implementar la aplicación de función.

4. Seleccione el icono de notificación de la esquina superior derecha del portal y observe el mensaje **Implementación correcta**.

    ![Definir nueva configuración de Function App](./media/create-function-app-linux-app-service-plan/function-app-create-notification.png)

5. Seleccione **Ir al recurso** para ver la nueva aplicación de función.

Después, cree una función en la nueva Function App. Incluso después de que la aplicación de función está disponible, puede tardar unos minutos en inicializarse totalmente.

## <a name="create-an-http-triggered-function"></a><a name="create-function"></a>Crear una función desencadenada por HTTP

En esta sección se muestra cómo crear una función en la nueva aplicación de función en el portal.

> [!NOTE]
> La experiencia del portal de desarrollo puede ser útil para probar Azure Functions. En la mayoría de los escenarios, considere la posibilidad de desarrollar sus funciones localmente y publicar el proyecto en la aplicación de función mediante [Visual Studio Code](functions-create-first-function-vs-code.md#create-an-azure-functions-project) o [Azure Functions Core Tools](functions-run-local.md#create-a-local-functions-project).  

1. En la nueva aplicación de función, elija la pestaña **Overview** (Información general) y, una vez cargada completamente, elija **+ New function** (+ Nueva función).

    ![Creación de una función desde la pestaña de información general](./media/create-function-app-linux-app-service-plan/overview-create-function.png)

1. En la pestaña **Quickstart** (Inicio rápido), elija **In-portal** (En el portal) y seleccione **Continue** (Continuar).

    ![Elija la plataforma de desarrollo de la función.](./media/create-function-app-linux-app-service-plan/function-app-quickstart-choose-portal.png)

1. Elija **WebHook y API** y, a continuación, seleccione **Crear**.

    ![Inicio rápido de funciones en Azure Portal.](./media/create-function-app-linux-app-service-plan/function-app-quickstart-node-webhook.png)

Se crea una función mediante una plantilla específica del idioma para una función desencadenada por HTTP.

Ahora, puede ejecutar la nueva función mediante el envío de una solicitud HTTP.

## <a name="test-the-function"></a>Prueba de la función

1. En la nueva función, haga clic en **</> Obtener la dirección URL de la función** en la parte superior derecha, seleccione **default (Function key)** y, después, haga clic en **Copiar**. 

    ![Copiar la dirección URL de la función desde Azure Portal](./media/create-function-app-linux-app-service-plan/function-app-develop-tab-testing.png)

2. Pegue la dirección URL de la función en la barra de direcciones de su explorador. Anexe el valor `&name=<yourname>` de la cadena de consulta al final de esta dirección URL y presione la tecla `Enter` en el teclado para ejecutar la solicitud. Debería ver la respuesta devuelta por la función mostrada en el explorador.  

    El ejemplo siguiente muestra la respuesta en el explorador:

    ![Respuesta de la función en el explorador.](./media/create-function-app-linux-app-service-plan/function-app-browser-testing.png)

    La dirección URL de la solicitud incluye una clave que, de forma predeterminada, es necesaria para tener acceso a la función a través de HTTP.

3. Cuando se ejecuta la función, se escribe información de seguimiento en los registros. Para ver el resultado del seguimiento de la ejecución anterior, vuelva a la función en el portal y haga clic en la flecha que encontrará en la parte inferior de la pantalla para expandir **Registros**.

   ![Visor de registros de las funciones en Azure Portal.](./media/create-function-app-linux-app-service-plan/function-view-logs.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Ha creado una Function App con una función simple desencadenada por HTTP.  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Para más información, consulte [Enlaces HTTP de Azure Functions](functions-bindings-http-webhook.md).
