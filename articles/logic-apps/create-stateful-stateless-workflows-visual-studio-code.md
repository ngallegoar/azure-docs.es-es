---
title: Creación de flujos de trabajo de automatización (versión preliminar) en Visual Studio Code
description: Cree flujos de trabajo de automatización sin estado o con estado con la extensión Azure Logic Apps (versión preliminar) en Visual Studio Code para integrar aplicaciones, datos, servicios en la nube y sistemas locales.
services: logic-apps
ms.suite: integration
ms.reviewer: deli, rohitha, vikanand, hongzili, sopai, absaafan, logicappspm
ms.topic: conceptual
ms.date: 11/09/2020
ms.openlocfilehash: 749807349fd83f9639461fd4ddd9ab771d108119
ms.sourcegitcommit: 17b36b13857f573639d19d2afb6f2aca74ae56c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94410562"
---
# <a name="create-stateful-or-stateless-workflows-in-visual-studio-code-with-the-azure-logic-apps-preview-extension"></a>Creación de flujos de trabajo con estado o sin estado en Visual Studio Code con la extensión Azure Logic Apps (versión preliminar)

> [!IMPORTANT]
> Esta funcionalidad se encuentra en versión preliminar pública, se ofrece sin contrato de nivel de servicio y no se recomienda usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Para crear flujos de trabajo de aplicaciones lógicas que se integren entre aplicaciones, datos, servicios en la nube y sistemas, puede usar Visual Studio Code y la extensión Azure Logic Apps (versión preliminar) para compilar y ejecutar localmente [flujos de trabajo de aplicaciones lógicas *con estado* y *sin estado*](#stateful-stateless) en su entorno de desarrollo.

![Captura de pantalla que muestra Visual Studio Code y el flujo de trabajo de una aplicación lógica.](./media/create-stateful-stateless-workflows-visual-studio-code/visual-studio-code-logic-apps-overview.png)

Las aplicaciones lógicas que se crean con la extensión en versión preliminar pública usan el nuevo tipo de recurso **Logic Apps (versión preliminar)** y se basan en el runtime de [Azure Functions](../azure-functions/functions-overview.md) en su entorno local. Este nuevo tipo de recurso puede incluir varios flujos de trabajo y, en cierta medida, es similar al tipo de recurso **Aplicación de funciones** , que puede incluir varias funciones.

Mientras tanto, el tipo de recurso **Logic Apps** original aún existe para que lo cree y use en Visual Studio Code y en Azure Portal. Sin embargo, las experiencias del tipo de recurso original son independientes del nuevo tipo de recurso. En este momento, los tipos de recursos **Logic Apps** y **Logic Apps (versión preliminar)** pueden existir al mismo tiempo en Visual Studio Code y en Azure Portal. Puede ver todas las aplicaciones lógicas implementadas en la suscripción de Azure, y acceder a ellas, pero aparecen y se mantienen por separado en sus propias categorías y secciones.

En este artículo se proporciona [información general sobre esta versión preliminar pública](#whats-new), se describen diversos aspectos sobre el tipo de recurso **Logic Apps (versión preliminar)** y cómo crear este recurso mediante Visual Studio Code:

* De qué manera difieren entre sí las aplicaciones lógicas [con estado y sin estado](#stateful-stateless).

* Cómo cumplir los [requisitos de configuración](#prerequisites) y [configurar Visual Studio Code](#set-up) para la extensión en versión preliminar pública.

* Cómo compilar nuevos flujos de trabajo de **Logic Apps (versión preliminar)** al [crear un proyecto y seleccionar una plantilla de flujo de trabajo](#create-project).

* Cómo ejecutar y depurar localmente las nuevas aplicaciones lógicas en Visual Studio Code.

* Cómo publicar estas nuevas aplicaciones lógicas directamente desde Visual Studio Code [en Azure](#publish-azure) o [en un contenedor de Docker](#deploy-docker) que puede ejecutar en cualquier lugar. Para obtener más información sobre Docker, consulte [¿Qué es Docker?](/dotnet/architecture/microservices/container-docker-introduction/docker-defined)

<a name="whats-new"></a>

## <a name="whats-in-this-public-preview"></a>¿Qué se incluye en esta versión preliminar pública?

La extensión Azure Logic Apps (versión preliminar) aporta muchas funcionalidades actuales y adicionales de Logic Apps a la experiencia de desarrollo local en Visual Studio Code, por ejemplo:

* Cree aplicaciones lógicas para los flujos de trabajo de integración y automatización desde [más de 390 conectores](/connectors/connector-reference/connector-reference-logicapps-connectors) para aplicaciones y servicios de software como servicio (SaaS) y plataforma como servicio (PaaS), además de conectores para sistemas locales.

  * Algunos conectores administrados, como Azure Service Bus, Azure Event Hubs y SQL Server, se ejecutan de forma similar a desencadenadores y acciones nativos integrados, como la acción HTTP.

  * Cree e implemente aplicaciones lógicas que se puedan ejecutar en cualquier lugar, ya que el servicio Azure Logic Apps genera cadenas de conexión de Firma de acceso compartido (SAS) que estas aplicaciones lógicas pueden usar para enviar solicitudes al punto de conexión de runtime de conexión a la nube. El servicio Logic Apps guarda estas cadenas de conexión con otras opciones de configuración de la aplicación para que pueda almacenar fácilmente estos valores en Azure Key Vault al realizar la implementación en Azure.

    > [!NOTE]
    > De manera predeterminada, un recurso de **Logic Apps (versión preliminar)** tiene su [identidad administrada asignada por el sistema](../logic-apps/create-managed-service-identity.md) habilitada automáticamente para autenticar conexiones en runtime. Esta identidad se diferencia de las credenciales de autenticación o de la cadena de conexión que se usan al crear una conexión. Si deshabilita esta identidad, las conexiones no funcionarán en runtime.

* Cree aplicaciones lógicas sin estado que se ejecuten solo en memoria para que finalicen con mayor rapidez, respondan más rápido, tengan un mayor rendimiento y tengan menos costos de ejecución, ya que los historiales de ejecución y los datos entre las acciones no se conservan en el almacenamiento externo. De manera opcional, puede habilitar el historial de ejecución para facilitar la depuración. Para obtener más información, consulte [Aplicaciones lógicas con estado y sin estado](#stateful-stateless).

* Ejecute y depure las aplicaciones lógicas localmente en el entorno de desarrollo de Visual Studio Code.

* Publique e implemente las aplicaciones lógicas desde Visual Studio Code directamente en varios entornos de hospedaje, como [Azure App Service](../app-service/environment/intro.md) y [contenedores de Docker](/dotnet/core/docker/introduction).

> [!NOTE]
> Para obtener información sobre los problemas conocidos actuales, consulte la [página de problemas conocidos en de GitHub](https://github.com/Azure/logicapps/blob/master/articles/logic-apps-public-preview-known-issues.md) para la extensión en versión preliminar.

<a name="stateful-stateless"></a>

## <a name="stateful-versus-stateless-logic-apps"></a>Aplicaciones lógicas con estado y sin estado

* *Con estado*

  Cree aplicaciones lógicas con estado cuando necesite mantener o revisar datos de eventos anteriores, o hacer referencia a ellos. Estas aplicaciones lógicas mantienen la entrada y la salida de cada acción y sus estados de flujo de trabajo en el almacenamiento externo, lo que hace que revisar los detalles y el historial de ejecución sea posible después de que finalice cada ejecución. Las aplicaciones lógicas con estado proporcionan una alta resistencia si se producen interrupciones o cuando estas se producen. Una vez restaurados los servicios y sistemas, puede reconstruir las ejecuciones de aplicaciones lógicas interrumpidas a partir del estado guardado y volver a ejecutar las aplicaciones lógicas hasta su finalización. Los flujos de trabajo con estado pueden continuar ejecutándose durante un plazo máximo de un año.

* *Sin estado*

  Cree aplicaciones lógicas sin estado cuando no necesite guardar ni revisar datos de eventos anteriores, ni hacer referencia a ellos en almacenamiento externo para una revisión posterior. Estas aplicaciones lógicas mantienen la entrada y la salida de cada acción y sus estados de flujo de trabajo solo en la memoria, en lugar de transferir esta información al almacenamiento externo. Como consecuencia, las aplicaciones lógicas sin estado tienen ejecuciones más cortas que normalmente no duran más de 5 minutos, un rendimiento más rápido con menores tiempos de respuesta, mayor rendimiento y costos de ejecución menores, ya que los detalles y el historial de ejecución no se mantienen en el almacenamiento externo. Sin embargo, si se producen interrupciones, las ejecuciones interrumpidas no se restauran automáticamente, por lo que el autor de la llamada tiene que volver a enviar manualmente las ejecuciones interrumpidas. Estas aplicaciones lógicas solo se pueden ejecutar sincrónicamente y, para facilitar la depuración, puede [habilitar el historial de ejecuciones](#run-history), lo que afecta al rendimiento.

  Actualmente, los flujos de trabajo sin estado solo admiten *acciones* de [conectores administrados](../connectors/apis-list.md#managed-api-connectors) que se implementan en Azure, y no desencadenadores. Para iniciar el flujo de trabajo, seleccione la [Solicitud integrada, Event Hubs o el desencadenador de Service Bus](../connectors/apis-list.md#built-ins), que se ejecutan de forma nativa en el entorno de ejecución de Logic Apps. Para obtener más información acerca de los desencadenadores, las acciones y los conectores no admitidos, consulte [Funcionalidades no admitidas o no disponibles](#unsupported).

Para obtener información sobre el diferente comportamiento de las aplicaciones lógicas anidadas entre las aplicaciones lógicas con estado y sin estado, consulte [Diferencias de comportamiento anidado entre aplicaciones lógicas con estado y sin estado](#nested-behavior).

<a name="pricing-model"></a>

## <a name="pricing-model"></a>Modelo de precios

Cuando implemente el nuevo tipo de recurso **Logic Apps (versión preliminar)** , se le pedirá que seleccione un plan de hospedaje, en concreto el [plan de App Service o el plan Premium](../azure-functions/functions-scale.md) para usarlo como modelo de precios. Si selecciona el plan de App Service, también se le pedirá que seleccione un [plan de tarifa](../app-service/overview-hosting-plans.md). Durante la versión preliminar pública, la ejecución de aplicaciones lógicas en App Service no incurre en los cargos *adicionales* al plan seleccionado.

Las aplicaciones lógicas con estado usan [almacenamiento externo](../azure-functions/functions-scale.md#storage-account-requirements), por lo que el modelo de precios de Azure Storage se aplica a las transacciones de almacenamiento que realiza el runtime de Azure Logic Apps. Por ejemplo, las colas se utilizan para la programación, mientras que las tablas y blobs se utilizan para almacenar los estados de los flujos de trabajo.

Para obtener más información sobre los modelos de precios que se aplican a este nuevo tipo de recurso, consulte estos temas:

* [Escalado y hospedaje de Azure Functions](../azure-functions/functions-scale.md)
* [Escalado vertical de una aplicación en Azure App Service](../app-service/manage-scale-up.md)
* [Detalles de precios de Azure Functions](https://azure.microsoft.com/pricing/details/functions/)
* [Detalles de precios de App Service](https://azure.microsoft.com/pricing/details/app-service/windows/)
* [Detalles de precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/)

<a name="unsupported"></a>

## <a name="unavailable-or-unsupported-capabilities"></a>Funcionalidades no disponibles o no admitidas

Para esta versión preliminar pública, estas funcionalidades no están disponibles o no son compatibles:

* La creación del nuevo recurso **Logic Apps (versión preliminar)** actualmente no está disponible en macOS.

* Todavía no se admiten todas las regiones de Azure. Para ver las regiones disponibles actualmente, consulte la [lista de regiones](https://github.com/Azure/logicapps/blob/master/articles/logic-apps-public-preview-known-issues.md#available-regions).

* Para iniciar el flujo de trabajo, seleccione la [Solicitud integrada, HTTP, Event Hubs o el desencadenador de Service Bus](../connectors/apis-list.md), que se ejecutan de forma nativa en el entorno de ejecución de Logic Apps. Actualmente, esta versión preliminar no admite los [conectores de empresa](../connectors/apis-list.md#enterprise-connectors), los [desencadenadores de puerta de enlace de datos local](../connectors/apis-list.md#on-premises-connectors), los desencadenadores basados en webhook, el desencadenador de ventana deslizante, los [conectores personalizados](../connectors/apis-list.md#custom-apis-and-connectors), las cuentas de integración, los artefactos y [sus conectores](../connectors/apis-list.md#integration-account-connectors). La funcionalidad para llamar a una función de Azure no está disponible, por lo que, por ahora, use la *acción* HTTP para llamar a la dirección URL de solicitud de la función de Azure.

  Excepto en el caso de los desencadenadores especificados anteriormente, los flujos de trabajo *con estado* pueden usar tanto desencadenadores como acciones con los [conectores administrados](../connectors/apis-list.md#managed-api-connectors) que se implementan en Azure, frente a los desencadenadores y las acciones integrados que se ejecutan de forma nativa con el entorno de ejecución de Logic Apps. Sin embargo, los flujos de trabajo *sin estado* solo admiten actualmente *acciones* para conectores administrados, no para desencadenadores. Aunque puede habilitar conectores en Azure para el flujo de trabajo sin estado, el diseñador no muestra ningún desencadenador de conector administrado para que pueda seleccionarlo.

* Solo puede implementar el nuevo tipo de recurso **Aplicación lógica (versión preliminar)** en un plan de hospedaje [Premium o de App Service en Azure](#publish-azure), o en un [contenedor de Docker](#deploy-docker), y no en [entornos de servicios de integración (ISEs)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Los planes de hospedaje de **consumo** no son compatibles ni están disponibles para implementar este tipo de recurso.

## <a name="prerequisites"></a>Requisitos previos

### <a name="access-and-connectivity"></a>Acceso y conectividad

* Acceda a Internet para poder descargar los requisitos, conectarse desde Visual Studio Code a su cuenta de Azure y publicar desde Visual Studio Code en Azure, un contenedor de Docker u otro entorno.

* Una cuenta y una suscripción de Azure. Si aún no tiene una, [regístrese para obtener una cuenta de Azure gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* Para compilar la misma aplicación lógica de ejemplo en este artículo, necesita una cuenta de correo electrónico de Office 365 Outlook que use una cuenta profesional o educativa de Microsoft para iniciar sesión.

  Si decide usar otro [conector de correo electrónico que sea compatible con Azure Logic Apps](/connectors/), como Outlook.com o [Gmail](../connectors/connectors-google-data-security-privacy-policy.md), aún puede seguir el ejemplo, y los pasos generales son los mismos, pero la interfaz de usuario y las opciones pueden diferir de alguna manera. Por ejemplo, si usa el conector de Outlook.com, en su lugar use su cuenta personal de Microsoft para iniciar sesión.

### <a name="storage-requirements"></a>Requisitos de almacenamiento

1. Descargue e instale el [Emulador de Azure Storage 5.10](https://go.microsoft.com/fwlink/p/?linkid=717179).

1. Para ejecutar el emulador, debe tener una instalación local de SQL Database, como [SQL Server 2019 Express Edition](https://go.microsoft.com/fwlink/p/?linkid=866658), que es gratuita. Para obtener más información, consulte [Uso del emulador de Azure Storage para desarrollo y pruebas](../storage/common/storage-use-emulator.md).

   > [!IMPORTANT]
   > Antes de abrir el Diseñador de aplicación lógica para crear el flujo de trabajo de la aplicación lógica, asegúrese de iniciar el emulador. De lo contrario, recibirá un mensaje que indica `Workflow design time could not be started`.
   >
   > ![Captura de pantalla que muestra el emulador de Azure Storage en ejecución.](./media/create-stateful-stateless-workflows-visual-studio-code/start-storage-emulator.png)

### <a name="tools"></a>Herramientas

* [Visual Studio Code 1.30.1 (enero de 2019) o superior](https://code.visualstudio.com/), que es gratis. Además, descargue e instale estas herramientas adicionales para Visual Studio Code, si aún no las tiene:

  * [Extensión de cuenta de Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account), que proporciona una experiencia común única de filtrado de suscripción e inicio de sesión de Azure para todas las demás extensiones de Azure en Visual Studio Code.

  * [Extensión de C# para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp), que habilita la funcionalidad F5 para ejecutar la aplicación lógica.

  * [Azure Functions Core Tools](../azure-functions/functions-run-local.md), ya sea la versión [3.0.2931](https://github.com/Azure/azure-functions-core-tools/releases/tag/3.0.2931) o [2.7.2936](https://github.com/Azure/azure-functions-core-tools/releases/tag/2.7.2936), a través de Microsoft Installer (MSI). Estas herramientas incluyen una versión del mismo runtime en el que se basa el runtime de Azure Functions que se ejecuta en Visual Studio Code.

    > [!IMPORTANT]
    > Si tiene una instalación anterior a estas versiones, desinstale primero esa versión o asegúrese de que la variable de entorno PATH apunta a la versión que descargue e instale.
    >
    > Si desea usar la [acción **Código en línea**](../logic-apps/logic-apps-add-run-inline-code.md) para ejecutar código JavaScript, debe usar la versión de runtime de Azure Functions 3x porque la acción no admite la versión 2x. Además, esta acción no se admite actualmente en los sistemas operativos Linux.

  * [Extensión Azure Logic Apps (versión preliminar) para Visual Studio Code](https://go.microsoft.com/fwlink/p/?linkid=2143167). Esta extensión de versión preliminar pública proporciona la capacidad de crear aplicaciones lógicas con estado y sin estado y ejecutarlas localmente en Visual Studio Code.

    Actualmente, puede tener la extensión **Azure Logic Apps** original y la nueva extensión **Azure Logic Apps (versión preliminar)** instaladas al mismo tiempo en Visual Studio Code. Al seleccionar el icono de Azure en la barra de herramientas de Visual Studio Code, puede ver todas las aplicaciones lógicas implementadas en Azure, pero cada tipo de recurso aparece en sus propias secciones de extensión, **Logic Apps** y **Azure Logic Apps (versión preliminar)** .

    > [!IMPORTANT]
    > Si creó aplicaciones lógicas con la extensión **Azure Logic Apps (versión preliminar privada)** , estas aplicaciones lógicas no funcionarán con la extensión de versión preliminar pública. Sin embargo, para migrar estas aplicaciones lógicas, puede desinstalar la extensión de versión preliminar privada, realizar la limpieza necesaria e instalar la extensión de versión preliminar pública. A continuación, puede crear el nuevo proyecto en Visual Studio Code y copiar en el nuevo proyecto el archivo **workflow.definition** de la aplicación lógica creada anteriormente.
    >
    > Por lo tanto, antes de instalar la extensión de versión preliminar pública, asegúrese de desinstalar las versiones anteriores y eliminar estos artefactos:
    >
    > * La carpeta **Microsoft.Azure.Functions.ExtensionBundle.Workflows** , que contiene los conjuntos de extensiones anteriores y se encuentra en una de estas rutas de acceso:
    >
    >   * `C:\Users\{userName}\AppData\Local\Temp\Functions\ExtensionBundles`
    >
    >   * `C:\Users\{userName}.azure-functions-core-tools\Functions\ExtensionBundles`
    >
    > * La carpeta **microsoft.azure.workflows.webjobs.extension** , que es la memoria caché de [NuGet](/nuget/what-is-nuget) para la extensión de versión preliminar privada y se encuentra en esta ruta de acceso:
    >
    >   `C:\Users\{userName}\.nuget\packages`

    Para instalar la extensión **Azure Logic Apps (versión preliminar)** , siga estos pasos:

    1. En Visual Studio Code, en la barra de herramientas izquierda, seleccione **Extensiones**.

    1. En el cuadro de búsqueda de extensiones, escriba `azure logic apps preview`. En la lista de resultados, seleccione **Azure Logic Apps (Preview)** (Azure Logic Apps [versión preliminar]) **>** **Instalar**.

       Una vez finalizada la instalación, la extensión de versión preliminar pública aparece en la lista **Extensiones: Instaladas**.

       ![Captura de pantalla que muestra la lista de extensiones instaladas de Visual Studio Code con la extensión "Azure Logic Apps (versión preliminar)" subrayada.](./media/create-stateful-stateless-workflows-visual-studio-code/azure-logic-apps-extension-installed.png)

* Para probar la aplicación lógica de ejemplo que se crea en este artículo, necesita una herramienta que pueda enviar llamadas al desencadenador Solicitud, que es el primer paso de la aplicación lógica de ejemplo. Si no dispone de esta herramienta, puede descargar, instalar y usar [Postman](https://www.postman.com/downloads/).

* Para facilitar el registro de diagnóstico y la capacidad de seguimiento, puede agregar y usar un recurso de [Application Insights](../azure-monitor/app/app-insights-overview.md). Puede crear este recurso durante la implementación de la aplicación lógica o en Azure Portal después de implementar la aplicación lógica.

<a name="set-up"></a>

## <a name="set-up-visual-studio-code"></a>Configurar Visual Studio Code

1. Para asegurarse de que todas las extensiones estén instaladas correctamente, vuelva a cargar o reinicie Visual Studio Code.

1. Habilite o compruebe que Visual Studio Code encuentre e instale automáticamente las actualizaciones de extensión para que la extensión de versión preliminar pública obtenga las actualizaciones más recientes.

   Para seleccionar esta configuración, realice estos pasos:

   1. En el menú **Archivo** , vaya a **Preferencias** **>** **Configuración**.

   1. En la pestaña **Usuario** , vaya a **Características** **>** **Extensiones**.

   1. Confirme que las opciones **Auto Check Updates** (Comprobar actualizaciones automáticamente) y **Actualización automática** estén seleccionadas.

1. Habilite o compruebe que estas opciones de la extensión **Azure Logic Apps (versión preliminar)** estén configuradas en Visual Studio Code:

   * **Azure Logic Apps V2: Modo de panel**
   * **Azure Logic Apps V2: Project Runtime** (Runtime del proyecto)

   1. En el menú **Archivo** , vaya a **Preferencias** **>** **Configuración**.

   1. En la pestaña **Usuario** , vaya a **>** **Extensiones** **>** **Azure Logic Apps (versión preliminar)** .

   1. En **Azure Logic Apps V2: Modo de panel** , confirme que la opción **Enable panel mode** (Habilitar modo de panel) esté seleccionada. En **Azure Logic Apps V2: Project Runtime** (Runtime del proyecto), establezca la versión en **~3** o **~2** , en función de la [versión de Azure Functions Core Tools](#prerequisites) que instaló anteriormente.

      > [!IMPORTANT]
      > Si desea usar la [acción **Código en línea**](../logic-apps/logic-apps-add-run-inline-code.md) para ejecutar código JavaScript, asegúrese de usar la versión 3 del runtime del proyecto porque la acción no admite la versión 2. Además, esta acción no se admite actualmente en los sistemas operativos Linux.

      ![Captura de pantalla que muestra la configuración de Visual Studio Code para la extensión "Azure Logic Apps (versión preliminar)".](./media/create-stateful-stateless-workflows-visual-studio-code/azure-logic-apps-preview-settings.png)

<a name="connect-azure-account"></a>

## <a name="connect-to-your-azure-account"></a>Conexión a la cuenta de Azure

1. En la barra de herramientas de Visual Studio Code, seleccione el icono de Azure.

   ![Captura de pantalla que muestra la barra de herramientas de Visual Studio Code y el icono de Azure seleccionado.](./media/create-stateful-stateless-workflows-visual-studio-code/visual-studio-code-azure-icon.png)

1. En el panel de Azure, en **Azure: Logic Apps (versión preliminar)** , seleccione **Iniciar sesión en Azure**. Cuando aparezca la página de autenticación de Visual Studio Code, inicie sesión con su cuenta de Azure.

   ![Captura de pantalla que muestra el panel de Azure y el vínculo seleccionado para el inicio de sesión de Azure.](./media/create-stateful-stateless-workflows-visual-studio-code/sign-in-azure-subscription.png)

   Después de iniciar sesión, el panel de Azure muestra las suscripciones de la cuenta de Azure. Si tiene la extensión de Logic Apps publicada, puede encontrar cualquier recurso de Logic Apps original que haya creado mediante la extensión original en la sección **Logic Apps** de la extensión publicada, pero no en la sección **Logic Apps (versión preliminar)** de la extensión en versión preliminar.
   
   Si no aparecen las suscripciones esperadas o si desea que el panel muestre solo suscripciones específicas, siga estos pasos:

   1. En la lista suscripciones, mueva el puntero situado junto a la primera suscripción hasta que aparezca el botón **Seleccionar suscripciones** (icono de filtro). Seleccione el icono de filtro.

      ![Captura de pantalla que muestra el panel de Azure y el icono de filtro seleccionado.](./media/create-stateful-stateless-workflows-visual-studio-code/filter-subscription-list.png)

      O bien, en la barra de estado de Visual Studio Code, seleccione la cuenta de Azure. 

   1. Cuando aparezca otra lista suscripciones, seleccione las suscripciones que desee y, a continuación, asegúrese de seleccionar **Aceptar**.

<a name="create-project"></a>

## <a name="create-a-local-project"></a>Creación de un proyecto local

Antes de poder crear la aplicación lógica, cree un proyecto local para que pueda administrar e implementar la aplicación lógica desde Visual Studio Code. El proyecto subyacente es similar a un proyecto de Azure Functions, también conocido como proyecto de aplicación de funciones; pero estos tipos de proyecto son independientes entre sí, por lo que los flujos de trabajo y las funciones de la aplicación lógica no pueden existir en el mismo proyecto.

1. En el equipo, cree una carpeta local *vacía* que se usará para el proyecto que se creará posteriormente en Visual Studio Code.

1. En Visual Studio Code, cierre todas las carpetas abiertas.

1. En el panel de Azure, junto a **Azure: Logic Apps (versión preliminar)** , seleccione **Crear proyecto** (icono que muestra una carpeta y un rayo).

   ![Captura de pantalla que muestra la barra de herramientas del panel de Azure con la opción "Crear proyecto" seleccionada.](./media/create-stateful-stateless-workflows-visual-studio-code/create-new-project-folder.png)

1. Si el Firewall de Windows Defender le pide que conceda acceso a la red para `Code.exe`, que es Visual Studio Code y, para `func.exe`, que es el Azure Functions Core Tools, seleccione **Redes privadas, como una red doméstica o del trabajo** **>** **Permitir acceso**.

1. Vaya a la ubicación en la que creó la carpeta del proyecto, seleccione esa carpeta y continúe.

   ![Captura de pantalla que muestra el cuadro de diálogo "Seleccionar carpeta" con una carpeta de proyecto recién creada y el botón "Seleccionar" seleccionado.](./media/create-stateful-stateless-workflows-visual-studio-code/select-project-folder.png)

1. En la lista de plantillas que aparece, seleccione **Stateful Workflow** (Flujo de trabajo con estado) o **Stateless Workflow** (Flujo de trabajo sin estado). En este ejemplo se selecciona **Stateful Workflow** (Flujo de trabajo con estado).

   ![Captura de pantalla que muestra la lista de plantillas de flujo de trabajo con "Flujo de trabajo con estado" seleccionado.](./media/create-stateful-stateless-workflows-visual-studio-code/select-stateful-stateless-workflow.png)

1. Proporcione un nombre para el flujo de trabajo de la aplicación lógica y presione ENTRAR. En este ejemplo se usa `example-workflow` como nombre.

   ![Captura de pantalla que muestra el cuadro "Create a new Stateful Workflow (3/4)" (Crear nuevo flujo de trabajo con estado [3/4]) y "example-workflow" como nombre del flujo de trabajo.](./media/create-stateful-stateless-workflows-visual-studio-code/name-your-workflow.png)

1. En la siguiente lista que aparece, seleccione **Open in current window** (Abrir en la ventana actual).

   ![Captura de pantalla que muestra la lista con la opción "Abrir en la ventana actual" seleccionada.](./media/create-stateful-stateless-workflows-visual-studio-code/select-project-location.png)

   Visual Studio Code se recarga, abre el panel Explorador y muestra el proyecto, que ahora incluye los archivos de proyecto generados automáticamente. Por ejemplo, el proyecto tiene una carpeta que muestra el nombre del flujo de trabajo de la aplicación lógica. Dentro de esta carpeta, el archivo **workflow.json** contiene la definición JSON subyacente del flujo de trabajo de la aplicación lógica.

   ![Captura de pantalla que muestra la ventana del Explorador con la carpeta del proyecto, la carpeta del flujo de trabajo y el archivo "workflow.json".](./media/create-stateful-stateless-workflows-visual-studio-code/local-project-created.png)

<a name="open-workflow-definition-designer"></a>

## <a name="open-the-workflow-definition-file-in-logic-app-designer"></a>Apertura del archivo de definición del flujo de trabajo en el Diseñador de aplicación lógica

1. Compruebe las versiones que ha instalado en el equipo ejecutando este comando:

   `..\Users\{yourUserName}\dotnet --list-sdks`

   Si tiene el SDK de .NET Core 5.x, es posible que esta versión le impida abrir la definición de flujo de trabajo subyacente de la aplicación lógica en el diseñador. En lugar de desinstalar esta versión, en la ubicación raíz del proyecto, cree un archivo **global.json** que haga referencia a la versión 3.x del runtime de .NET Core que tiene, que sea superior a la 3.1.201, por ejemplo:

   ```json
   {
      "sdk": {
         "version": "3.1.8",
         "rollForward": "disable"
      }
   }
   ```

   Asegúrese de que agrega de forma explícita el archivo **global.json** al proyecto en la ubicación raíz desde Visual Studio Code. de lo contrario, el diseñador no se abrirá.

1. Si Visual Studio Code se está ejecutando en Windows o Linux, asegúrese de que el emulador de Azure Storage se esté ejecutando. Para obtener más información, revise los [requisitos previos](#prerequisites).

1. Expanda la carpeta del proyecto para el flujo de trabajo. Abra el menú contextual del archivo **workflow.json** y seleccione **Open in Designer** (Abrir en el diseñador).

   ![Captura de pantalla que muestra el panel del Explorador y la ventana contextual del archivo workflow.json con la opción "Abrir en el diseñador" seleccionada.](./media/create-stateful-stateless-workflows-visual-studio-code/open-definition-file-in-designer.png)

   Si aparece el mensaje de error que indica `Workflow design time could not be started`, asegúrese de que el emulador de Azure Storage se esté ejecutando. De lo contrario, pruebe estas otras sugerencias de solución de problemas:

   En Visual Studio Code, compruebe la salida de la extensión de versión preliminar.

   1. En el menú **Ver** , seleccione **Salida**.

   1. En la lista de la barra de título **Salida** , seleccione **Azure Logic Apps** para que pueda ver la salida de la extensión de versión preliminar, por ejemplo:

      ![Captura de pantalla que muestra la ventana Salida de Visual Studio Code con la opción "Azure Logic Apps" seleccionada.](./media/create-stateful-stateless-workflows-visual-studio-code/check-outout-window-azure-logic-apps.png)

   1. Revise la salida y compruebe si aparece este mensaje de error:

      ```text
      A host error has occurred during startup operation '{operationID}'.
      System.Private.CoreLib: The file 'C:\Users\{userName}\AppData\Local\Temp\Functions\
      ExtensionBundles\Microsoft.Azure.Functions.ExtensionBundle.Workflows\1.1.1\bin\
      DurableTask.AzureStorage.dll' already exists.
      Value cannot be null. (Parameter 'provider')
      Application is shutting down...
      Initialization cancellation requested by runtime.
      Stopping host...
      Host shutdown completed.
      ```

      Este error puede producirse si previamente se ha intentado abrir el diseñador y, a continuación, se ha suspendido o eliminado el proyecto. Para resolver este error, elimine la carpeta **ExtensionBundles** en esta ubicación **…\Users\\{su-nombre-de-usuario}\AppData\Local\Temp\Functions\ExtensionBundles** y vuelva a intentar abrir el archivo **workflow.json** en el diseñador.

1. En la lista **Enable connectors in Azure** (Habilitar conectores en Azure), seleccione la opción **Use connectors from Azure** (Usar conectores de Azure) que se aplica a todos los conectores administrados que están disponibles e implementados en Azure, no solo a los conectores de los servicios de Azure.

   ![Captura de pantalla que muestra el panel del Explorador con la lista "Habilitar conectores en Azure" abierta y  la opción "Usar conectores de Azure" seleccionada.](./media/create-stateful-stateless-workflows-visual-studio-code/use-connectors-from-azure.png)

   > [!NOTE]
   > Actualmente, los flujos de trabajo sin estado solo admiten *acciones* de [conectores administrados](../connectors/apis-list.md#managed-api-connectors) que se implementan en Azure, y no desencadenadores. Aunque tiene la opción de habilitar conectores en Azure para el flujo de trabajo sin estado, el diseñador no muestra ningún desencadenador de conector administrado para que pueda seleccionarlo.

1. En la lista del grupo de recursos, seleccione **Crear un nuevo grupo de recursos**.

   ![Captura de pantalla que muestra el panel del Explorador con la lista de grupos de recursos y la opción "Crear un nuevo grupo de recursos" seleccionada.](./media/create-stateful-stateless-workflows-visual-studio-code/create-select-resource-group.png)

1. Proporcione un nombre para el grupo de recursos y presione ENTRAR. En este ejemplo se usa `example-logic-app-preview-rg`.

   ![Captura de pantalla que muestra el panel del Explorador y el cuadro de nombre del grupo de recursos.](./media/create-stateful-stateless-workflows-visual-studio-code/enter-name-for-resource-group.png)

1. En la lista ubicaciones, busque y seleccione una [región de Azure admitida](https://github.com/Azure/logicapps/blob/master/articles/logic-apps-public-preview-known-issues.md#available-regions) que se usará para crear el grupo de recursos y los recursos. En este ejemplo se usa **Centro-oeste de EE. UU.**

   > [!IMPORTANT]
   > Actualmente no se admiten todas las regiones, pero se están llevando a cabo actualizaciones para agregar más regiones. Al seleccionar una región no admitida, se podrían producir problemas como, por ejemplo, la creación de conexiones. Para ver las regiones no admitidas actualmente, consulte la [página de problemas conocidos de GitHub](https://github.com/Azure/logicapps/blob/master/articles/logic-apps-public-preview-known-issues.md#available-regions) para la extensión en versión preliminar.

   ![Captura de pantalla que muestra el panel del Explorador con la lista de ubicaciones y la opción "Centro-oeste de EE. UU." seleccionada.](./media/create-stateful-stateless-workflows-visual-studio-code/select-azure-region.png)

   Después de realizar este paso, Visual Studio Code abre el Diseñador de aplicación lógica.

   > [!NOTE]
   > Cuando Visual Studio Code inicia la API de tiempo de diseño del flujo de trabajo, aparece un mensaje que indica que el inicio puede tardar unos segundos. Puede pasar por alto este mensaje o seleccionar **Aceptar**.

   Después de que aparezca el Diseñador de aplicación lógica, aparece la solicitud **Elegir una operación** en el diseñador y está seleccionada de manera predeterminada, lo que muestra el panel **Agregar una acción**.

   ![Captura de pantalla que muestra el Diseñador de aplicación lógica.](./media/create-stateful-stateless-workflows-visual-studio-code/workflow-app-designer.png)

1. A continuación, [agregue un desencadenador y acciones](#add-trigger-actions) al flujo de trabajo.

<a name="add-trigger-actions"></a>

## <a name="add-a-trigger-and-actions"></a>Adición de un desencadenador y acciones

Después de abrir el Diseñador de aplicación lógica desde el menú contextual del archivo **workflow.json** , aparece la solicitud **Elegir una operación** en el diseñador y está seleccionada de manera predeterminada. Ahora puede empezar a crear el flujo de trabajo agregando un desencadenador y acciones.

El flujo de trabajo de la aplicación lógica de este ejemplo usa este desencadenador y estas acciones:

* El [desencadenar Solicitud](../connectors/connectors-native-reqres.md) integrado, **Cuando se recibe una solicitud HTTP** , que recibe llamadas o solicitudes entrantes y crea un punto de conexión al que pueden llamar otros servicios o aplicaciones lógicas.

* La [acción de Office 365 Outlook](../connectors/connectors-create-api-office365-outlook.md), **Enviar correo electrónico**.

* La [acción Respuesta](../connectors/connectors-native-reqres.md) integrada, que se usa para enviar una respuesta y devolver los datos al autor de la llamada.

### <a name="add-the-request-trigger"></a>Adición del desencadenador Request

1. Junto al diseñador, en el panel **Agregar un desencadenador** , en el cuadro de búsqueda **Elegir una operación** , asegúrese de que **Integrado** esté seleccionado para que pueda seleccionar un desencadenador que se ejecute de forma nativa.

1. En el cuadro de búsqueda **Elegir una operación** , escriba `when a http request` y seleccione el desencadenador Solicitud integrado denominado **Cuando se recibe una solicitud HTTP**.

   ![Captura de pantalla que muestra el Diseñador de aplicación lógica y el panel **Agregar un desencadenador** con el desencadenador "Cuando se recibe una solicitud HTTP" seleccionado.](./media/create-stateful-stateless-workflows-visual-studio-code/add-request-trigger.png)

   Cuando el desencadenador aparece en el diseñador, se abre el panel de detalles del desencadenador para mostrar las propiedades, la configuración y otras acciones del desencadenador.

   ![Captura de pantalla que muestra el Diseñador de aplicación lógica con el desencadenador "Cuando se recibe una solicitud HTTP" seleccionado y el panel de detalles del desencadenador abierto.](./media/create-stateful-stateless-workflows-visual-studio-code/request-trigger-added-to-designer.png)

   > [!TIP]
   > Si el panel de detalles no aparece, asegúrese de que el desencadenador esté seleccionado en el diseñador.

1. Si tiene que eliminar un elemento en el diseñador, siga estos pasos:

   1. En el diseñador, seleccione el elemento, lo cual abrirá el panel de detalles del elemento en el lado derecho.

   1. Expanda la ventana de Visual Studio Code lo suficiente para que aparezca junto al nombre de desencadenador o acción el botón de puntos suspensivos ( **...** ) en la esquina superior derecha. 

   1. Abra el menú de puntos suspensivos ( **...** ) y seleccione **Eliminar**. Para confirmar la eliminación, haga clic en **Aceptar**.

      ![Captura de pantalla que muestra el elemento seleccionado en el diseñador con el panel de detalles abierto y con el botón de puntos suspensivos seleccionado y la opción "Eliminar".](./media/create-stateful-stateless-workflows-visual-studio-code/delete-item-from-designer.png)

### <a name="add-the-office-365-outlook-action"></a>Adición de la acción Office 365 Outlook

1. En el diseñador, en el desencadenador que agregó, seleccione **Nuevo paso**.

   Aparece la solicitud **Elegir una operación** en el diseñador, y se vuelve a abrir el **panel Agregar una acción** para que pueda seleccionar la acción siguiente.

1. En el panel **Agregar una acción** , en el cuadro de búsqueda **Elegir una operación** , seleccione **Azure** para poder buscar y seleccionar una acción para un conector administrado que esté implementado en Azure.

   En este ejemplo se selecciona y usa la acción de Office 365 Outlook, **Enviar correo electrónico (V2)** .

   ![Captura de pantalla que muestra el Diseñador de aplicación lógica y el panel **Agregar una acción** con la acción "Enviar correo electrónico" de Office 365 Outlook seleccionada.](./media/create-stateful-stateless-workflows-visual-studio-code/add-send-email-action.png)

1. En el panel de detalles de la acción, seleccione **Iniciar sesión** para que pueda crear una conexión a su cuenta de correo electrónico.

   ![Captura de pantalla que muestra el Diseñador de aplicación lógica y el panel **Enviar correo electrónico (V2)** con "Iniciar sesión" seleccionado.](./media/create-stateful-stateless-workflows-visual-studio-code/send-email-action-sign-in.png)

   > [!NOTE]
   > Si aparece el error `Failed to create connection...`, es posible que haya elegido una región no admitida actualmente para la aplicación lógica. Se están llevando a cabo actualizaciones para agregar más regiones. Mientras tanto, para ver las regiones no admitidas actualmente, consulte la [página de problemas conocidos de GitHub](https://github.com/Azure/logicapps/blob/master/articles/logic-apps-public-preview-known-issues.md#available-regions) para la extensión en versión preliminar.

1. Cuando Visual Studio Code le pida consentimiento para acceder a su cuenta de correo electrónico, seleccione **Abrir**.

   ![Captura de pantalla que muestra la solicitud de Visual Studio Code para permitir el acceso.](./media/create-stateful-stateless-workflows-visual-studio-code/visual-studio-code-open-external-website.png)

   > [!TIP]
   > Para evitar futuras solicitudes, seleccione **Configure Trusted Domains** (Configurar dominios de confianza) para que pueda agregar la página de autenticación como dominio de confianza.

1. Siga las indicaciones posteriores para iniciar sesión, permitir el acceso y permitir que se vuelva a Visual Studio Code.

   > [!NOTE]
   > Si transcurre demasiado tiempo antes de completar las solicitudes, el proceso de autenticación agota el tiempo de espera y se produce un error. En este caso, vuelva al diseñador e intente iniciar sesión de nuevo para crear la conexión.

1. Cuando la extensión de versión preliminar de Azure Logic Apps le pida consentimiento para acceder a su cuenta de correo electrónico, seleccione **Abrir**. Siga la solicitud a continuación para permitir el acceso.

   ![Captura de pantalla que muestra la solicitud de la extensión de versión preliminar para permitir el acceso.](./media/create-stateful-stateless-workflows-visual-studio-code/allow-preview-extension-open-uri.png)

   > [!TIP]
   > Para evitar futuras solicitudes, seleccione **No preguntar de nuevo sobre esta extensión**.

   Una vez que Visual Studio Code crea la conexión, algunos conectores muestran el mensaje `The connection will be valid for {n} days only.`. Este límite de tiempo se aplica solo a la duración mientras crea la aplicación lógica en Visual Studio Code. Después de la implementación, ya no se aplica este límite porque la aplicación lógica puede autenticarse durante el tiempo de ejecución mediante su [identidad administrada asignada por el sistema](../logic-apps/create-managed-service-identity.md) habilitada automáticamente. Esta identidad administrada se diferencia de las credenciales de autenticación o de la cadena de conexión que se usan al crear una conexión. Si deshabilita esta identidad administrada asignada por el sistema, las conexiones no funcionarán durante el tiempo de ejecución.

1. En el diseñador, si la acción **Enviar correo electrónico** no aparece seleccionada, seleccione esa acción.

1. En el panel de detalles de la acción, en la pestaña **Parámetros** , proporcione la información necesaria para la acción, por ejemplo:

   ![Captura de pantalla que muestra el Diseñador de aplicación lógica con detalles para la acción "Enviar correo electrónico" de Office 365 Outlook.](./media/create-stateful-stateless-workflows-visual-studio-code/send-email-action-details.png)

   | Propiedad | Obligatorio | Value | Descripción |
   |----------|----------|-------|-------------|
   | **To** | Sí | <*your-email-address*> | El destinatario del correo electrónico, que puede ser su dirección de correo electrónico con fines de prueba. En este ejemplo se usa la dirección de correo electrónico ficticia `sophiaowen@fabrikam.com`. |
   | **Subject** | Sí | `An email from your example workflow` | El asunto del correo electrónico |
   | **Cuerpo** | Sí | `Hello from your example workflow!` | El contenido del cuerpo del correo electrónico. |
   ||||

   > [!NOTE]
   > Si quiere hacer cambios en el panel de detalles de la pestaña **Configuración** , **Ejecutar después de** o **Resultado estático** , asegúrese de seleccionar **Listo** para confirmar dichos cambios antes de cambiar de pestaña o cambiar el foco al diseñador. De lo contrario, Visual Studio Code no conservará los cambios. Para obtener más información, consulte la [página de problemas conocidos en de GitHub](https://github.com/Azure/logicapps/blob/master/articles/logic-apps-public-preview-known-issues.md) para la extensión en versión preliminar.

1. En el diseñador, seleccione **Guardar**.

A continuación, ejecute y depure el flujo de trabajo localmente en Visual Studio Code.

<a name="debug-test-locally"></a>

## <a name="run-and-debug-locally"></a>Ejecución y depuración locales

Para probar la aplicación lógica, siga estos pasos para iniciar una sesión de depuración y encontrar la dirección URL del punto de conexión que se crea mediante el desencadenador Solicitud. Necesitará esta dirección URL para poder enviar una solicitud a ese punto de conexión más adelante.

1. Para que le resulte más fácil depurar un flujo de trabajo de aplicación lógica sin estado, puede [habilitar el historial de ejecución para ese flujo de trabajo](#run-history).

1. En la barra de herramientas de Visual Studio Code, abra el menú **Ejecutar** y seleccione **Iniciar depuración**  (F5).

   Se abre la ventana **Terminal** para que pueda revisar la sesión de depuración.

1. Ahora, busque la URL de devolución de llamada para el punto de conexión en el desencadenador Solicitud.

   1. Vuelva a abrir el panel del Explorador para que pueda ver el proyecto.

   1. En el menú contextual del archivo **workflow.json** , seleccione **Información general**.

      ![Captura de pantalla que muestra el panel del Explorador y la ventana contextual del archivo workflow.json con la opción "Información general" seleccionada.](./media/create-stateful-stateless-workflows-visual-studio-code/open-workflow-overview.png)

   1. Busque el valor **URL de devolución de llamadas** , que tiene un aspecto similar a esta dirección URL para el desencadenador Solicitud de ejemplo:

      `http://localhost:7071/api/<workflow-name>/triggers/manual/invoke?api-version=2020-05-01-preview&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=<shared-access-signature>`

      ![Captura de pantalla que muestra la página de información general del flujo de trabajo con la URL de devolución de llamada](./media/create-stateful-stateless-workflows-visual-studio-code/find-callback-url.png)

1. Para probar la dirección URL de devolución de llamada al desencadenar el flujo de trabajo de la aplicación lógica, abra [Postman](https://www.postman.com/downloads/) o su herramienta preferida para crear y enviar solicitudes.

   Este ejemplo continúa con Postman. Para obtener más información, consulte la [Introducción a Postman](https://learning.postman.com/docs/getting-started/introduction/).

   1. En la barra de herramientas de Postman, seleccione **New** (Nuevo).

      ![Captura de pantalla que muestra Postman con el botón Nuevo seleccionado.](./media/create-stateful-stateless-workflows-visual-studio-code/postman-create-request.png)

   1. En el panel **Create New** (Crear nuevo), en **Building Blocks** (Bloques de creación), seleccione **Request** (Solicitud).

   1. En la ventana **Save Request** (Guardar solicitud), en **Request name** (Nombre de solicitud), escriba un nombre para la solicitud, por ejemplo, `Test workflow trigger`.

   1. En **Select a collection or folder to save to** (Seleccionar una colección o carpeta donde guardar), seleccione **Create Collection** (Crear colección).

   1. En **All Collections** (Todas las colecciones), escriba un nombre para la colección que se va a crear para organizar las solicitudes, presione ENTRAR y, a continuación, seleccione **Save to < *nombre-de-la-colección*>** (Guardar en <nombre de la colección>). En este ejemplo se usa `Logic Apps requests` como nombre de la colección.

      Se abre el panel de solicitudes de Postman para que pueda enviar una solicitud a la URL de devolución de llamada para el desencadenador Solicitud.

      ![Captura de pantalla que muestra Postman con el panel de solicitudes abierto.](./media/create-stateful-stateless-workflows-visual-studio-code/postman-request-pane.png)

   1. Vuelva a Visual Studio Code. Desde la página de información general del flujo de trabajo, copie el valor de la propiedad **URL de devolución de llamadas**.

   1. Vuelva a Postman. En el panel de solicitudes, junto a la lista de métodos, que actualmente muestra **GET** como método de solicitud predeterminado, pegue la URL de devolución de llamada que copió anteriormente en el cuadro de dirección y seleccione **Send** (Enviar).

      ![Captura de pantalla que muestra Postman y la URL de devolución de llamada en el cuadro de dirección con el botón Enviar seleccionado.](./media/create-stateful-stateless-workflows-visual-studio-code/postman-test-call-back-url.png)

      El flujo de trabajo de aplicación lógica de ejemplo envía un correo electrónico similar a este ejemplo:

      ![Captura de pantalla que muestra el correo electrónico de Outlook tal como se describe en el ejemplo.](./media/create-stateful-stateless-workflows-visual-studio-code/workflow-app-result-email.png)

1. En Visual Studio Code, vuelva a la página de información general del flujo de trabajo.

   Si creó un flujo de trabajo con estado, después de que la solicitud que envió desencadene el flujo de trabajo, la página de información general mostrará el estado y el historial de ejecución del flujo de trabajo.

   > [!TIP]
   > Si el estado de ejecución no aparece, intente actualizar la página de información general seleccionando **Actualizar**. No se produce ninguna ejecución para un desencadenador que se omite debido a criterios que no se cumplen o si no se encuentra ningún dato.

   ![Captura de pantalla que muestra la página de información general del flujo de trabajo con el estado y el historial de ejecución.](./media/create-stateful-stateless-workflows-visual-studio-code/post-trigger-call.png)

   | Estado de la ejecución | Descripción |
   |------------|-------------|
   | **Anulado** | La ejecución se ha detenido o no ha finalizado debido a problemas externos; por ejemplo, una interrupción del sistema o una suscripción de Azure vencida. |
   | **Cancelado** | La ejecución se ha desencadenado y se ha iniciado, pero ha recibido una solicitud de cancelación. |
   | **Erróneo** | Se ha producido un error en al menos en una acción de la ejecución. No se ha configurado ninguna acción posterior en el flujo de trabajo para controlar el error. |
   | **Ejecución** | La ejecución se ha desencadenado y está en curso, pero este estado también puede aparecer para una ejecución que está limitada debido a los [límites de acción](logic-apps-limits-and-config.md) o al [plan de precios actual](https://azure.microsoft.com/pricing/details/logic-apps/). <p><p>**Sugerencia** : Si se configura un [registro de diagnóstico](monitor-logic-apps-log-analytics.md), se puede obtener información sobre los eventos de limitación que se produzcan. |
   | **Correcto** | La ejecución se ha completado correctamente. Si se ha producido un error en alguna acción, se ha controlado mediante una acción posterior en el flujo de trabajo. |
   | **Tiempo de espera agotado** | Se ha agotado el tiempo de espera de la ejecución porque la duración actual ha superado el límite de duración de la ejecución, que se controla mediante el valor [**Retención del historial de ejecución en días**](logic-apps-limits-and-config.md#run-duration-retention-limits). La duración de una ejecución se calcula mediante su hora de inicio y su límite de duración en esa hora de inicio. <p><p>**Nota** : Si la duración de la ejecución también supera el *límite de retención del historial de ejecución actual* , que también se controla mediante la opción [**Retención del historial de ejecución en días**](logic-apps-limits-and-config.md#run-duration-retention-limits), la ejecución se borra del historial de ejecuciones mediante un trabajo de limpieza diaria. Con independencia de que la ejecución agote el tiempo de espera o se complete, el período de retención siempre se calcula mediante la hora de inicio de la ejecución y el límite de retención *actual*. Por tanto, si reduce el límite de duración de una ejecución en curso, se agota su tiempo de espera. Sin embargo, la ejecución se mantiene o se borra del historial de ejecución en función de si su duración supera el límite de retención. |
   | **En espera** | La ejecución no se ha iniciado o está en pausa, por ejemplo, debido a un flujo de trabajo anterior que sigue en ejecución. |
   |||

1. Para revisar los estados de cada paso de una ejecución específica, y las entradas y salidas del paso, seleccione el botón de puntos suspensivos ( **…** ) para esa ejecución y seleccione **Mostrar la ejecución**.

   ![Captura de pantalla que muestra la fila del historial de ejecución del flujo de trabajo con el botón de puntos suspensivos y la opción "Mostrar la ejecución" seleccionada.](./media/create-stateful-stateless-workflows-visual-studio-code/show-run-history.png)

   Visual Studio Code abre la vista de supervisión y muestra el estado de cada paso de la ejecución.

   ![Captura de pantalla que muestra cada paso de la ejecución del flujo de trabajo y su estado.](./media/create-stateful-stateless-workflows-visual-studio-code/run-history-action-status.png)

   Estos son los posibles estados que puede tener cada paso del flujo de trabajo:

   | Estado de la acción | Icono | Descripción |
   |---------------|------|-------------|
   | Anulado | ![Icono de estado de la acción "Anulado"][aborted-icon] | La acción se ha detenido o no ha finalizado debido a problemas externos; por ejemplo, una interrupción del sistema o una suscripción de Azure vencida. |
   | Cancelado | ![Icono de estado de la acción "Cancelado"][cancelled-icon] | La acción se estaba ejecutando pero recibió una solicitud de cancelación. |
   | Con error | ![Icono de estado de la acción "Con error"][failed-icon] | Se produjo un error en la acción. |
   | En ejecución | ![Icono de estado de la acción "En ejecución"][running-icon] | La acción se está ejecutando actualmente. |
   | Omitido | ![Icono de estado de la acción "Omitido"][skipped-icon] | La acción se omitió porque se produjo un error en la acción inmediatamente anterior. Una acción tiene una condición `runAfter` que requiere que la acción anterior finalice correctamente antes de que se pueda ejecutar la acción actual. |
   | Correcto | ![Icono de estado de la acción "Correcto"][succeeded-icon] | La acción se realizó correctamente. |
   | Se realizó correctamente con reintentos | ![Icono de estado de la acción "Correcto con reintentos"][succeeded-with-retries-icon] | La acción se realizó correctamente, pero solo después de uno o varios reintentos. Para revisar el historial de reintentos en la vista de detalles del historial de ejecución, seleccione esa acción para que pueda ver las entradas y salidas. |
   | Tiempo de espera agotado | ![Icono de estado de la acción "Tiempo de espera agotado"][timed-out-icon] | La acción se detuvo debido al límite de tiempo de espera que especificó la configuración de la acción. |
   | En espera | ![Icono de estado de la acción "En espera"][waiting-icon] | Se aplica a una acción de webhook que está esperando una solicitud entrante de un autor de llamada. |
   ||||

   [aborted-icon]: ./media/create-stateful-stateless-workflows-visual-studio-code/aborted.png
   [cancelled-icon]: ./media/create-stateful-stateless-workflows-visual-studio-code/cancelled.png
   [failed-icon]: ./media/create-stateful-stateless-workflows-visual-studio-code/failed.png
   [running-icon]: ./media/create-stateful-stateless-workflows-visual-studio-code/running.png
   [skipped-icon]: ./media/create-stateful-stateless-workflows-visual-studio-code/skipped.png
   [succeeded-icon]: ./media/create-stateful-stateless-workflows-visual-studio-code/succeeded.png
   [succeeded-with-retries-icon]: ./media/create-stateful-stateless-workflows-visual-studio-code/succeeded-with-retries.png
   [timed-out-icon]: ./media/create-stateful-stateless-workflows-visual-studio-code/timed-out.png
   [waiting-icon]: ./media/create-stateful-stateless-workflows-visual-studio-code/waiting.png

1. Para revisar las entradas y salidas de cada paso, seleccione el paso que quiere inspeccionar.

   ![Captura de pantalla que muestra el estado de cada paso del flujo de trabajo más las entradas y salidas de la acción "Enviar correo electrónico" expandida.](./media/create-stateful-stateless-workflows-visual-studio-code/run-history-details.png)

1. Para revisar aún más las entradas y salidas sin procesar para ese paso, seleccione **Mostrar entradas sin procesar** o **Mostrar salidas sin procesar**.

1. Para detener la sesión de depuración, en el menú **Ejecutar** , seleccione **Detener depuración**  (Mayús+F5).

<a name="return-response"></a>

## <a name="return-a-response-to-the-caller"></a>Devolución de una respuesta al autor de la llamada

Para devolver una respuesta al autor de la llamada que envió una solicitud a la aplicación lógica, puede usar la [acción Respuesta](../connectors/connectors-native-reqres.md) integrada para un flujo de trabajo que se inicie con el desencadenador Solicitud.

1. En el Diseñador de aplicación lógica, en la acción **Enviar correo electrónico** , seleccione **Nuevo paso**.

   Aparece la solicitud **Elegir una operación** en el diseñador, y se vuelve a abrir el **panel Agregar una acción** para que pueda seleccionar la acción siguiente.

1. En el panel **Agregar una acción** , en el cuadro de búsqueda **Elegir una acción** , asegúrese de que **Integrado** esté seleccionado. En el cuadro de búsqueda, escriba `response` y seleccione la acción **Respuesta**.

   ![Captura de pantalla que muestra el Diseñador de aplicación lógica con la acción Respuesta seleccionada.](./media/create-stateful-stateless-workflows-visual-studio-code/add-response-action.png)

   Cuando aparece la acción **Respuesta** en el diseñador, se abre automáticamente el panel de detalles de la acción.

   ![Captura de pantalla que muestra el Diseñador de aplicación lógica con el panel de detalles de la acción "Respuesta" abierto y la propiedad "Cuerpo" establecida en el valor de la propiedad "Cuerpo" de la acción "Enviar correo electrónico".](./media/create-stateful-stateless-workflows-visual-studio-code/response-action-details.png)

1. En la pestaña **Parámetros** , proporcione la información necesaria para la función a la que quiere llamar.

   En este ejemplo se devuelve el valor de propiedad **Cuerpo** que se obtiene de la acción **Enviar correo electrónico**.

   1. Haga clic dentro del cuadro de propiedad **Cuerpo** para que aparezca la lista de contenido dinámico y muestre los valores de salida disponibles del desencadenador y las acciones anteriores en el flujo de trabajo.

      ![Captura de pantalla que muestra el panel de detalles de la acción "Respuesta" con el puntero del mouse dentro de la propiedad "Cuerpo" para que aparezca la lista de contenido dinámico.](./media/create-stateful-stateless-workflows-visual-studio-code/open-dynamic-content-list.png)

   1. En la lista de contenido dinámico, en **Enviar correo electrónico** , seleccione **Cuerpo**.

      ![Captura de pantalla que muestra la lista de contenido dinámico abierta. En la lista, en el encabezado "Enviar correo electrónico", el valor de salida "Cuerpo" está seleccionado.](./media/create-stateful-stateless-workflows-visual-studio-code/select-send-email-action-body-output-value.png)

      Cuando haya terminado, la propiedad **Cuerpo** de la acción Respuesta estará establecida en el valor de salida **Cuerpo** de la acción **Enviar correo electrónico**.

      ![Captura de pantalla que muestra el estado de cada paso del flujo de trabajo más las entradas y salidas de la acción "Respuesta" expandida.](./media/create-stateful-stateless-workflows-visual-studio-code/response-action-details-body-property.png)

1. En el diseñador, seleccione **Guardar**.

<a name="retest-workflow"></a>

## <a name="retest-your-logic-app"></a>Nueva comprobación de la aplicación lógica

Después de realizar actualizaciones en la aplicación lógica, puede ejecutar otra prueba volviendo a ejecutar el depurador en Visual Studio y enviando otra solicitud para desencadenar la aplicación lógica actualizada, de forma similar a los pasos descritos en [Depuración y prueba de la aplicación lógica](#debug-test-locally).

1. En la barra de herramientas de Visual Studio Code, abra el menú **Ejecutar** y seleccione **Iniciar depuración**  (F5).

1. En Postman o en la herramienta para crear y enviar solicitudes, envíe otra solicitud para desencadenar el flujo de trabajo.

1. Si ha creado un flujo de trabajo con estado, en la página información general del flujo de trabajo, compruebe el estado de la ejecución más reciente. Para ver los estados, las entradas y las salidas de cada paso en esa ejecución, seleccione el botón de puntos suspensivos ( **…** ) para esa ejecución y seleccione **Mostrar la ejecución**.

   Por ejemplo, este es el estado paso a paso de una ejecución después de que se haya actualizado el flujo de trabajo de ejemplo con la acción Respuesta.

   ![Captura de pantalla que muestra el estado de cada paso del flujo de trabajo actualizado más las entradas y salidas de la acción "Respuesta" expandida.](./media/create-stateful-stateless-workflows-visual-studio-code/run-history-details-rerun.png)

1. Para detener la sesión de depuración, en el menú **Ejecutar** , seleccione **Detener depuración**  (Mayús+F5).

<a name="publish-azure"></a>

## <a name="publish-to-azure"></a>Publicar en Azure

Desde Visual Studio Code, puede implementar el proyecto directamente en Azure, que publica la aplicación lógica con el nuevo tipo de recurso **Logic Apps (versión preliminar** ). De forma similar al recurso de la aplicación de funciones en Azure Functions, la implementación de este nuevo tipo de recurso requiere que seleccione un [plan de hospedaje y plan de tarifa](../app-service/overview-hosting-plans.md), que puede configurar durante la implementación. Para obtener más información sobre los planes de hospedaje y los precios, revise estos temas:

* [Escalado vertical de una aplicación en Azure App Service](../app-service/manage-scale-up.md)
* [Escalado y hospedaje de Azure Functions](../azure-functions/functions-scale.md)

Puede publicar la aplicación lógica como nuevo recurso, lo que crea automáticamente los recursos adicionales necesarios, como una [cuenta de Azure Storage, similar a los requisitos de la aplicación de funciones](../azure-functions/storage-considerations.md). O bien, puede publicar la aplicación lógica en un recurso **Logic Apps (versión preliminar)** implementado anteriormente, que sobrescribe dicha aplicación lógica.

### <a name="publish-as-a-new-logic-app-preview-resource"></a>Publicación como nuevo recurso de Logic Apps (versión preliminar)

1. En la barra de herramientas de Visual Studio Code, seleccione el icono de Azure.

1. En la barra de herramientas del panel **Azure: Logic Apps (versión preliminar)** , seleccione **Deploy to Logic App** (Implementar en la aplicación lógica).

   ![Captura de pantalla que muestra el panel "Azure: Logic Apps (versión preliminar)" y la barra de herramientas del panel con la opción "Implementar en la aplicación lógica" seleccionada.](./media/create-stateful-stateless-workflows-visual-studio-code/deploy-to-logic-app.png)

1. En la lista que abre Visual Studio Code, seleccione una de estas opciones:

   * **Create new Logic App (Preview) in Azure** (Crear nueva instancia de Logic Apps [versión preliminar] en Azure) (rápido)
   * **Create new Logic App (Preview) in Azure Advanced** (Crear nueva instancia de Logic Apps [versión preliminar] en Azure avanzado)
   * Un recurso **Logic Apps (versión preliminar)** implementado previamente, si existe.

   Este ejemplo continúa con **Create new Logic App (Preview) in Azure Advanced** (Crear nueva Logic Apps [versión preliminar] en Azure avanzado).

   ![Captura de pantalla que muestra el panel "Azure: Logic Apps (versión preliminar)" con una lista con "Crear nueva Logic Apps (versión preliminar) en Azure" seleccionada.](./media/create-stateful-stateless-workflows-visual-studio-code/select-create-logic-app-options.png)

1. Para crear el nuevo recurso **Logic Apps (versión preliminar)** , siga estos pasos:

   1. Proporcione un nombre único globalmente para la nueva aplicación lógica, que es el nombre que se usará para el recurso **Logic Apps (versión preliminar)** . En este ejemplo se usa `example-logic-app-preview`.

      ![Captura de pantalla que muestra el panel "Azure: Logic Apps (versión preliminar)" y una solicitud para proporcionar un nombre para la nueva aplicación lógica que se va a crear.](./media/create-stateful-stateless-workflows-visual-studio-code/enter-logic-app-name.png)

   1. Seleccione un plan de hospedaje para la nueva aplicación lógica, ya sea [**Plan de App Service**](../azure-functions/functions-scale.md#app-service-plan) o [**Premium**](../azure-functions/functions-scale.md#premium-plan). En este ejemplo se selecciona **Plan de App Service**.

      ![Captura de pantalla que muestra el panel "Azure: Logic Apps (versión preliminar)" y una solicitud para seleccionar "Plan de App Service" o "Premium".](./media/create-stateful-stateless-workflows-visual-studio-code/select-hosting-plan.png)

   1. Cree un nuevo plan de App Service o seleccione un plan existente. En este ejemplo se selecciona **Crear nuevo plan de App Service**.

      ![Captura de pantalla que muestra el panel "Azure: Logic Apps (versión preliminar) y una solicitud para "Crear nuevo plan de App Service" o seleccionar un plan de App Service existente.](./media/create-stateful-stateless-workflows-visual-studio-code/create-app-service-plan.png)

   1. Proporcione un nombre para el plan de App Service y, a continuación, seleccione un [plan de tarifa](../app-service/overview-hosting-plans.md) para el plan. En este ejemplo se selecciona el plan **F1 Gratis**.

      ![Captura de pantalla que muestra el panel "Azure: Logic Apps (versión preliminar)" y una solicitud para seleccionar un plan de tarifa.](./media/create-stateful-stateless-workflows-visual-studio-code/select-pricing-tier.png)

   1. Para obtener un rendimiento óptimo, busque y seleccione el mismo grupo de recursos que el proyecto para la implementación.

      > [!NOTE]
      > Aunque puede crear o usar un grupo de recursos diferente, esto podría afectar al rendimiento. Si crea o elige otro grupo de recursos, pero cancela después de que aparezca el mensaje de confirmación, la implementación también se cancelará.

   1. Para flujos de trabajo con estado, seleccione **Crear una nueva cuenta de almacenamiento** o una cuenta de almacenamiento existente.

      ![Captura de pantalla que muestra el panel "Azure: Logic Apps (versión preliminar)" y una solicitud para crear o seleccionar una cuenta de almacenamiento.](./media/create-stateful-stateless-workflows-visual-studio-code/create-storage-account.png)

   1. Para facilitar el registro de diagnóstico y la capacidad de seguimiento, puede seleccionar un recurso de Application Insights existente. De lo contrario, puede seleccionar **Crear nuevo recurso de Application Insights** o configurar Application Insights en Azure Portal después de implementar la aplicación.

      Antes de implementar, asegúrese de agregar el objeto `logLevel` al objeto `logging` en el archivo **host.json** que existe en el nivel raíz del proyecto, y establezca `Host.Triggers.Workflow` en `Information`, por ejemplo:

      ```json
      "logLevel": {
         "Host.Triggers.Workflow": "Information"
      },
      ```

      Este es el aspecto que podría tener el archivo **host.json** :

      ```json
      {
         "version": "2.0",
         "logging": {
            "applicationInsights": {
               "samplingExcludedTypes": "Request",
               "samplingSettings": {
                  "isEnabled": true
               }
            },
            "logLevel": {
               "Host.Triggers.Workflow": "Information"
            }
         }
      }
      ``` 

   Cuando haya terminado, Visual Studio Code comienza a crear e implementar los recursos necesarios para publicar la aplicación lógica.

1. Para revisar y supervisar el proceso de implementación, en el menú **Ver** , seleccione **Salida**. En la lista de la barra de herramientas de la ventana Salida, seleccione **Azure Logic Apps**.

   ![Captura de pantalla que muestra la ventana Salida con "Azure Logic Apps" seleccionado en la lista de la barra de herramientas junto con el progreso y los estados de la implementación.](./media/create-stateful-stateless-workflows-visual-studio-code/logic-app-deployment-output-window.png)

   Cuando Visual Studio Code termina de implementar el flujo de trabajo de la aplicación lógica en Azure, aparece este mensaje:

   ![Captura de pantalla que muestra un mensaje que indica que la implementación en Azure se ha completado correctamente.](./media/create-stateful-stateless-workflows-visual-studio-code/deployment-to-azure-completed.png)

   Enhorabuena, la aplicación lógica ahora está activa en Azure y habilitada de manera predeterminada.

Ahora puede obtener información sobre cómo realizar estas tareas:

* [Búsqueda de la aplicación lógica implementada en Azure Portal](#find-manage-deployed-workflows-portal) o en [Visual Studio Code](#find-manage-deployed-workflows-vs-code).

* [Habilitación del historial de ejecución en flujos de trabajo de aplicación lógica sin estado](#run-history).

* [Habilitación de la supervisión en un recurso **Logic Apps (versión preliminar)** implementado](#enable-monitoring).

<a name="find-manage-deployed-workflows-vs-code"></a>

## <a name="find-and-manage-deployed-logic-apps-in-visual-studio-code"></a>Búsqueda y administración de aplicaciones lógicas implementadas en Visual Studio Code

En Visual Studio Code, puede ver todas las aplicaciones lógicas implementadas en la suscripción a Azure, independientemente de si son del tipo de recurso **Logic Apps** original o **Logic Apps (versión preliminar)** , y seleccionar tareas que le ayuden a administrar esas aplicaciones lógicas. Sin embargo, para acceder a ambos tipos de recursos, necesita las extensiones **Azure Logic Apps** y **Azure Logic Apps (versión preliminar)** para Visual Studio Code.

1. En la barra de herramientas izquierda, seleccione el icono de Azure. En el área **Azure: Logic Apps (versión preliminar)** , expanda su suscripción, que muestra todas las aplicaciones lógicas implementadas para esa suscripción.

1. Busque y seleccione la aplicación lógica que quiere administrar. Abra el menú contextual de la aplicación lógica y seleccione la tarea que quiere realizar.

   Por ejemplo, puede seleccionar tareas como detener, iniciar, reiniciar o eliminar la aplicación lógica implementada.

   ![Captura de pantalla que muestra Visual Studio Code con el panel de la extensión "Azure Logic Apps (versión preliminar)" abierto y el flujo de trabajo implementado.](./media/create-stateful-stateless-workflows-visual-studio-code/find-deployed-workflow-visual-studio-code.png)

1. Para ver todos los flujos de trabajo en la aplicación lógica, expándala y, después, expanda el nodo **Flujos de trabajo**.

1. Para ver un flujo de trabajo específico, abra el menú contextual del flujo de trabajo y seleccione **Open in Designer** (Abrir en el diseñador), que abre el flujo de trabajo en modo de solo lectura.

   Para editar el flujo de trabajo, tiene estas opciones:

   * En Visual Studio Code, abra el archivo **workflow.json** del proyecto en el diseñador de aplicaciones lógicas, realice los cambios y vuelva a implementar la aplicación lógica en Azure.

   * En Azure Portal, [busque y abra la aplicación lógica](#find-manage-deployed-workflows-portal). Busque, edite y guarde el flujo de trabajo.

1. Para abrir la aplicación lógica implementada en Azure Portal, abra el menú contextual de la aplicación lógica y seleccione **Abrir en el portal**.

   Azure Portal se abre en el explorador, inicia sesión automáticamente en el portal si ha iniciado sesión en Visual Studio Code y muestra la aplicación lógica.

   ![Captura de pantalla que muestra la página de Azure Portal para la aplicación lógica en Visual Studio Code.](./media/create-stateful-stateless-workflows-visual-studio-code/deployed-workflow-azure-portal.png)

   También puede iniciar sesión por separado en Azure Portal, usar el cuadro de búsqueda del portal para buscar la aplicación lógica y, a continuación, seleccionar la aplicación lógica en la lista de resultados.

   ![Captura de pantalla que muestra Azure Portal y la barra de búsqueda con los resultados de la búsqueda para la aplicación lógica implementada, que aparece seleccionada.](./media/create-stateful-stateless-workflows-visual-studio-code/find-deployed-workflow-azure-portal.png)

<a name="find-manage-deployed-workflows-portal"></a>

## <a name="find-and-manage-deployed-logic-apps-in-the-portal"></a>Búsqueda y administración de aplicaciones lógicas implementadas en el portal

En Azure Portal, puede ver todas las aplicaciones lógicas implementadas en la suscripción a Azure, independientemente de si son del tipo de recurso **Logic Apps** original o del tipo de recurso **Logic Apps (versión preliminar)** . Actualmente, cada tipo de recurso se organiza y administra como categoría independiente en Azure. Para buscar aplicaciones lógicas que tengan el tipo de recurso **Logic Apps (versión preliminar)** , siga estos pasos:

1. En el cuadro de búsqueda de Azure Portal, escriba `logic app preview`. Cuando aparezca la lista de resultados, en **Servicios** , seleccione **Logic Apps (versión preliminar)** .

   ![Captura de pantalla que muestra el cuadro de búsqueda de Azure Portal con el texto de búsqueda "logic app preview".](./media/create-stateful-stateless-workflows-visual-studio-code/portal-find-logic-app-preview-resource.png)

1. En el panel **Logic Apps (versión preliminar)** , busque y seleccione la aplicación lógica que implementó en Visual Studio Code.

   ![Captura de pantalla que muestra Azure Portal y el recurso Logic Apps (versión preliminar) implementado en Azure.](./media/create-stateful-stateless-workflows-visual-studio-code/logic-app-preview-resources-pane.png)

   Azure Portal abre la página individual de recursos para la aplicación lógica seleccionada.

   ![Captura de pantalla que muestra la página de recursos del flujo de trabajo de la aplicación lógica en Azure Portal.](./media/create-stateful-stateless-workflows-visual-studio-code/deployed-workflow-azure-portal.png)

1. Para ver los flujos de trabajo en esta aplicación lógica, en el menú de la aplicación lógica, seleccione **Flujos de trabajo**.

   En el panel **Flujos de trabajo** se muestran todos los flujos de trabajo de la aplicación lógica actual. En este ejemplo se muestra el flujo de trabajo que creó en Visual Studio Code.

   ![Captura de pantalla que muestra una página de recurso "Logic Apps (versión preliminar)" con el panel "Flujos de trabajo" abierto y el flujo de trabajo implementado.](./media/create-stateful-stateless-workflows-visual-studio-code/deployed-logic-app-workflows-pane.png)

1. Para ver un flujo de trabajo, en el panel **Flujos de trabajo** , seleccione ese flujo de trabajo.

   Se abre el panel del flujo de trabajo y muestra más información y tareas que puede realizar en ese flujo de trabajo.

   Por ejemplo, para ver los pasos del flujo de trabajo, seleccione **Diseñador**.

   ![Captura de pantalla que muestra el panel "Información general" del flujo de trabajo seleccionado, mientras que el menú flujo de trabajo muestra el comando "Diseñador" seleccionado.](./media/create-stateful-stateless-workflows-visual-studio-code/workflow-overview-pane-select-designer.png)

   El Diseñador de aplicación lógica se abre y muestra el flujo de trabajo que creó en Visual Studio Code. Ahora puede realizar cambios en este flujo de trabajo en Azure Portal.

   ![Captura de pantalla que muestra el Diseñador de aplicación lógica y el flujo de trabajo implementado desde Visual Studio Code.](./media/create-stateful-stateless-workflows-visual-studio-code/opened-workflow-designer.png)

<a name="add-workflows"></a>

## <a name="add-a-workflow-to-deployed-logic-apps"></a>Incorporación de un flujo de trabajo a aplicaciones lógicas implementadas

A través de Azure Portal, puede agregar flujos de trabajo vacíos a un recurso **Logic Apps (versión preliminar)** implementado desde Visual Studio Code y compilar esos flujos de trabajo en Azure Portal.

1. En [Azure Portal](https://portal.azure.com), busque y seleccione el recurso **Logic Apps (versión preliminar)** implementado.

1. En el menú de la aplicación lógica, seleccione **Flujos de trabajo**. En el panel **Flujos de trabajo** , seleccione **Agregar**.

   ![Captura de pantalla que muestra el panel "Flujos de trabajo" de la aplicación lógica seleccionada y la barra de herramientas con el comando "Agregar" seleccionado.](./media/create-stateful-stateless-workflows-visual-studio-code/add-new-workflow.png)

1. En el panel **Nuevo flujo de trabajo** , indique el nombre del flujo de trabajo. Seleccione **Con estado** o **Sin estado** **>** **Crear**.

   Una vez que Azure implemente el nuevo flujo de trabajo, que aparece en el panel **Flujos de trabajo** , seleccione ese flujo de trabajo para realizar la administración y otras tareas, como abrir el Diseñador de aplicación lógica o la vista de código.

   ![Captura de pantalla que muestra el flujo de trabajo seleccionado con opciones de administración y revisión.](./media/create-stateful-stateless-workflows-visual-studio-code/view-new-workflow.png)

   Por ejemplo, al abrir el diseñador para un nuevo flujo de trabajo, se muestra un lienzo en blanco. Ahora puede compilar este flujo de trabajo en Azure Portal.

   ![Captura de pantalla que muestra el Diseñador de aplicación lógica y un flujo de trabajo en blanco.](./media/create-stateful-stateless-workflows-visual-studio-code/opened-blank-workflow-designer.png)

<a name="run-history"></a>

## <a name="run-history-for-stateless-logic-app-workflows"></a>Historial de ejecución para flujos de trabajo de aplicación lógica sin estado

Para que le resulte más fácil depurar un flujo de trabajo de aplicación lógica sin estado, puede habilitar el historial de ejecución para ese flujo de trabajo en Visual Studio Code o en Azure Portal y, después, deshabilitar el historial de ejecución cuando haya terminado.

### <a name="for-a-stateless-logic-app-workflow-in-visual-studio-code"></a>Para un flujo de trabajo de aplicación lógica sin estado en Visual Studio Code

Si está trabajando con el flujo de trabajo de la aplicación lógica sin estado y la está ejecutando localmente en Visual Studio Code, siga estos pasos:

1. En el proyecto, busque y expanda la carpeta **workflow-designtime**. Busque y abra el archivo **local.settings.json**.

1. Agregue la propiedad `Workflows.{yourWorkflowName}.operationOptions` y establezca el valor en `WithStatelessRunHistory`, por ejemplo:

   ```json
   {
      "IsEncrypted": false,
      "Values": {
         "AzureWebJobsStorage": "UseDevelopmentStorage=true",
         "FUNCTIONS_WORKER_RUNTIME": "dotnet",
         "Workflows.{yourWorkflowName}.OperationOptions": "WithStatelessRunHistory"
      }
   }
   ```

1. Para deshabilitar el historial de ejecución cuando haya terminado, elimine la propiedad `Workflows.{yourWorkflowName}.OperationOptions` y su valor, o establezca la propiedad en `None`.

### <a name="for-a-stateless-logic-app-workflow-in-the-azure-portal"></a>Para un flujo de trabajo de aplicación lógica sin estado en Azure Portal

Si ya ha implementado el proyecto en Azure Portal, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com), busque y abra el recurso **Logic Apps (versión preliminar)** .

1. En el menú de la aplicación lógica, en **Configuración** , seleccione **Configuración**.

1. En la pestaña **Configuración de la aplicación** , seleccione **Nueva configuración de la aplicación**

1. En el panel **Agregar o editar la configuración de la aplicación** , en el cuadro **Nombre** , escriba el nombre de opción de operación: 

   `Workflows.{yourWorkflowName}.OperationOptions`

1. En el cuadro **Valor** , escriba el siguiente valor: `WithStatelessRunHistory`

   Por ejemplo:

   ![Captura de pantalla que muestra Azure Portal y el recurso Logic Apps (versión preliminar) con el panel "Configuración" > "Nueva configuración de la aplicación" < "Agregar o editar la configuración de la aplicación" abierto y la opción "Workflow.{nombreDelFlujoDeTrabajo}.OperationOptions" establecida en "WithStatelessRunHistory".](./media/create-stateful-stateless-workflows-visual-studio-code/stateless-operation-options-run-history.png)

1. Cuando finalice, seleccione **Aceptar**. En el panel **Configuración** , seleccione **Guardar**.

Para habilitar la supervisión en el recurso Logic Apps (versión preliminar) implementado, continúe con la siguiente sección.

<a name="enable-monitoring"></a>

## <a name="enable-monitoring-for-deployed-logic-app-preview-resources"></a>Habilitación de la supervisión de los recursos Logic Apps (versión preliminar) implementados

Para habilitar la supervisión en un recurso **Logic Apps (versión preliminar)** implementado, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com), busque y seleccione el recurso **Logic Apps (versión preliminar)** implementado.

1. En el menú de ese recurso, en **API** , seleccione **CORS**.

1. En el panel **CORS** , en **Orígenes permitidos** , agregue el carácter comodín (*).

1. Cuando esté listo, en la barra de herramientas de **CORS** , seleccione **Guardar**.

   ![Captura de pantalla que muestra Azure Portal con un recurso Logic Apps (versión preliminar) implementado. En el menú de recursos, "CORS" está seleccionado con una nueva entrada para "Orígenes permitidos" establecida en el carácter comodín "*".](./media/create-stateful-stateless-workflows-visual-studio-code/enable-run-history-deployed-logic-app.png)

<a name="deploy-docker"></a>

## <a name="deploy-to-docker-container"></a>Implementación en un contenedor de Docker

Con la [herramienta de interfaz de la línea de comandos (CLI) de .NET Core](/dotnet/core/tools/), puede compilar el proyecto y, a continuación, publicar la compilación. Después, puede compilar y usar un [contenedor de Docker](/visualstudio/docker/tutorials/docker-tutorial#what-is-a-container) como destino para implementar el flujo de trabajo de la aplicación lógica. Para obtener más información, consulte estos temas:

* [Introducción a los contenedores y Docker](/dotnet/architecture/microservices/container-docker-introduction/)
* [Introducción a .NET y Docker](/dotnet/core/docker/introduction)
* [Terminología de Docker](/dotnet/architecture/microservices/container-docker-introduction/docker-terminology)
* [Tutorial: Introducción a Docker](/visualstudio/docker/tutorials/docker-tutorial)

1. Para compilar el proyecto, abra un símbolo de la línea de comandos y ejecute este comando:

   `dotnet build -c release`

   Para obtener más información, consulte la página de referencia de [dotnet build](/dotnet/core/tools/dotnet-build/).

1. Para publicar la compilación, ejecute este comando:

   `dotnet publish -c release`

   Para obtener más información, consulte la página de referencia de [dotnet publish](/dotnet/core/tools/dotnet-publish/).

1. Cree un contenedor de Docker mediante un archivo de Docker para un flujo de trabajo de .NET y ejecute este comando:

   `docker build --tag local/workflowcontainer .`

   Por ejemplo, este es un archivo de Docker de ejemplo que implementa una aplicación lógica con estado y especifica la cadena de conexión para la cuenta de Azure Storage que se usó para publicar la aplicación lógica en Azure Portal. Para buscar y copiar la cadena de conexión de la cuenta de almacenamiento en Azure Portal, revise [Administración de las claves de acceso de la cuenta de almacenamiento](../storage/common/storage-account-keys-manage.md?tabs=azure-portal#view-account-access-keys).

   ![Captura de pantalla que muestra Azure Portal con las claves de acceso de la cuenta de almacenamiento y la cadena de conexión copiada.](./media/create-stateful-stateless-workflows-visual-studio-code/find-storage-account-connection-string.png)

   La cadena de conexión es similar a la de este ejemplo:

   `DefaultEndpointsProtocol=https;AccountName=fabrikamstorageaccount;AccountKey={access-key};EndpointSuffix=core.windows.net`

   Este es el formato del archivo de Docker:


   ```text
   FROM mcr.microsoft.com/azure-functions/dotnet:3.0.14492-appservice

   ENV AzureWebJobsStorage <storage-account-connection-string>
   ENV AZURE_FUNCTIONS_ENVIRONMENT Development
   ENV AzureWebJobsScriptRoot=/home/site/wwwroot
   ENV AzureFunctionsJobHost__Logging__Console__IsEnabled=true
   ENV FUNCTIONS_V2_COMPATIBILITY_MODE=true

   COPY ./bin/Release/netcoreapp3.1/publish/ /home/site/wwwroot
   ```

   Para obtener más información, consulte [docker build](https://docs.docker.com/engine/reference/commandline/build/).

1. Guarde la cadena en un lugar seguro para que pueda agregar más tarde la cadena a los archivos **local.settings.json** en el proyecto que usa para crear la aplicación lógica en Visual Studio Code.

1. Ejecute el contenedor localmente mediante este comando:

   `docker run -e WEBSITE_HOSTNAME=localhost -p 8080:80 local/workflowcontainer`

   Para obtener más información, consulte [docker run](https://docs.docker.com/engine/reference/commandline/run/).

1. Para obtener la URL de devolución de llamada para el desencadenador Solicitud, envíe esta solicitud:

   `POST /runtime/webhooks/workflow/api/management/workflows/{workflow-name}/triggers/{trigger-name}/listCallbackUrl?api-version=2019-10-01-edge-preview&code={master-key}`

   El valor < *master-key* > se define en la cuenta de Azure Storage que se establece para `AzureWebJobsStorage` en el archivo, **azure-webjobs-secrets/{nombre-de-implementación}/host.json** , donde puede encontrar el valor en esta sección:

   ```json
   {
     <...>
     "masterKey": {
        "name": "master",
        "value": "<master-key>",
        "encrypted": false
     },
     <...>
   }
   ```

<a name="nested-behavior"></a>

## <a name="nested-behavior-differences-between-stateful-and-stateless-logic-apps"></a>Diferencias de comportamiento anidado entre aplicaciones lógicas con estado y sin estado

Puede [hacer que se pueda llamar a un flujo de trabajo de aplicación lógica](../logic-apps/logic-apps-http-endpoint.md) desde otros flujos de trabajo de aplicación lógica que existan en el mismo recurso **Logic App (versión preliminar)** mediante el desencadenador [Solicitud](../connectors/connectors-native-reqres.md), el desencadenador [Webhook HTTP](../connectors/connectors-native-webhook.md) o los desencadenadores de conectores administrados que tienen el tipo [ApiConnectionWehook](../logic-apps/logic-apps-workflow-actions-triggers.md#apiconnectionwebhook-trigger) y pueden recibir solicitudes HTTPS.

Estos son los patrones de comportamiento que los flujos de trabajo de aplicación lógica anidados pueden seguir después de que un flujo de trabajo primario llame a un flujo de trabajo secundario:

* Patrón de sondeo asincrónico

  El flujo de trabajo primario no espera una respuesta a su llamada inicial, pero comprueba continuamente el historial de ejecución del flujo de trabajo secundario hasta que este termina de ejecutarse. De manera predeterminada, los flujos de trabajo con estado siguen este patrón, que es ideal para flujos de trabajo secundarios de ejecución prolongada que podrían superar los [límites de tiempo de espera de las solicitudes](../logic-apps/logic-apps-limits-and-config.md).

* Patrón sincrónico ("desencadenar y olvidar")

  El flujo de trabajo secundario confirma la llamada devolviendo inmediatamente una respuesta `202 ACCEPTED`, y el flujo de trabajo primario continúa con la siguiente acción sin esperar los resultados del secundario. En su lugar, el primario recibe los resultados cuando el secundario termina de ejecutarse. Los flujos de trabajo con estado secundarios que no incluyen una acción Respuesta siempre siguen el patrón sincrónico. En el caso de los flujos de trabajo con estado secundarios, el historial de ejecución está disponible para su revisión.

  Para habilitar este comportamiento, en la definición de JSON del flujo de trabajo, establezca la propiedad `operationOptions` en `DisableAsyncPattern`. Para obtener más información, consulte [Tipos de desencadenador y de acción: Opciones de operación](../logic-apps/logic-apps-workflow-actions-triggers.md#operation-options).

* Desencadenar y esperar

  Para un flujo de trabajo sin estado secundario, el primario espera una respuesta que devuelve los resultados del secundario. Este patrón funciona de forma similar al uso del [desencadenador o acción HTTP](../connectors/connectors-native-http.md) integrados para llamar a un flujo de trabajo secundario. Los flujos de trabajo sin estado secundarios que no incluyen una acción Respuesta devuelven inmediatamente una respuesta `202 ACCEPTED`, pero el primario espera a que el secundario finalice antes de continuar con la siguiente acción. Estos comportamientos solo se aplican a los flujos de trabajo sin estado secundarios.

En esta tabla se especifica el comportamiento del flujo de trabajo secundario en función de si los flujos de trabajo primario y secundario son con estado, sin estado o tipos de flujo de trabajo mixtos:

| Flujo de trabajo primario | Flujo de trabajo secundario | Comportamiento del secundario |
|-----------------|----------------|----------------|
| Con estado | Con estado | Asincrónico o sincrónico con la configuración `"operationOptions": "DisableAsyncPattern"` |
| Con estado | Sin estado | Desencadenar y esperar |
| Sin estado | Con estado | Sincrónico |
| Sin estado | Sin estado | Desencadenar y esperar |
||||

## <a name="limits"></a>Límites

Aunque [muchos límites existentes para Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md) son los mismos para este tipo de recurso, aquí tiene las diferencias en esta extensión de versión preliminar pública:

* Conectores administrados: 50 solicitudes por minuto por conexión

* En el caso de la [acción Código en línea para JavaScript](../logic-apps/logic-apps-add-run-inline-code.md), estos límites han cambiado:

  * El límite de caracteres de código aumenta de 1024 a 100 000 caracteres.

  * El límite de tiempo para ejecutar el código aumenta de 5 a 15 segundos.

## <a name="next-steps"></a>Pasos siguientes

Nos gustaría conocer sus experiencias con esta extensión de versión preliminar pública.

* En caso de errores o problemas, [dirija los problemas a GitHub](https://github.com/Azure/logicapps/issues).
* Si tiene preguntas, solicitudes, ideas y comentarios, [use este formulario de comentarios](https://aka.ms/lafeedback).
