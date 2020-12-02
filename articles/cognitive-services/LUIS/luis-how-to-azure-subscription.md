---
title: 'Uso de claves de creación y de entorno de ejecución: LUIS'
description: Si usa LUIS por primera vez, no tiene que definir una clave de creación. Cuando quiera publicar la aplicación y usar el punto de conexión en tiempo de ejecución, deberá crear y asignar la clave de entorno de ejecución a la aplicación.
services: cognitive-services
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 09/07/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: a71c1a0df1a72e3831fa54a041539f62b38a0aca
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2020
ms.locfileid: "95999916"
---
# <a name="create-luis-resources"></a>Creación de recursos de LUIS

Los recursos de creación y de entorno de ejecución de consultas de predicción proporcionan autenticación a la aplicación de Language Understanding (LUIS) y al punto de conexión de predicción.

<a name="azure-resources-for-luis"></a>
<a name="programmatic-key" ></a>
<a name="endpoint-key"></a>
<a name="authoring-key"></a>

## <a name="luis-resources"></a>Recursos de LUIS

LUIS permite tres tipos de recursos de Azure y uno que no es de Azure:

|Recurso|Propósito|Cognitive Service `kind`|Cognitive Service `type`|
|--|--|--|--|
|Recurso de creación|Permite crear, administrar, entrenar, probar y publicar las aplicaciones. [Defina un recurso de creación de LUIS](luis-how-to-azure-subscription.md#create-luis-resources-in-the-azure-portal) si quiere crear aplicaciones de LUIS mediante programación o desde el portal de LUIS. Antes de vincular los recursos de creación de Azure con su aplicación debe [migrar su cuenta de LUIS](luis-migration-authoring.md#what-is-migration). Puede controlar los permisos para el recurso de creación mediante la asignación del [rol de colaborador](#contributions-from-other-authors) a los usuarios. <br><br> El recurso de creación de LUIS dispone de un nivel de servicio:<br> <ul> <li>**Recurso de creación F0 gratis**, que le otorga 1 millón de transacciones de creación gratuitas y 1000 solicitudes de punto de conexión de predicción de prueba gratuitas al mes. |`LUIS.Authoring`|`Cognitive Services`|
|Recurso de predicción| Después de publicar la aplicación de LUIS, use la clave o el recurso de predicción para consultar las solicitudes de puntos de conexión de predicción. Cree un recurso de predicción de LUIS antes de que la aplicación cliente supere las 1000 solicitudes proporcionadas por el recurso de inicio o de creación. <br><br> El recurso de predicción dispone de dos niveles de servicio:<br><ul> <li> **Recurso de predicción F0 gratis**, que le otorga 10 000 solicitudes de punto de conexión de predicción gratuitas al mes.<br> <li> **Recurso de predicción S0 estándar**, que es el nivel de pago. [Más información sobre precios.](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/)|`LUIS`|`Cognitive Services`|
|Recurso de inicio/evaluación|Permite crear, administrar, entrenar, probar y publicar las aplicaciones. Este recurso se crea de forma predeterminada si elige la opción de recurso de inicio al iniciar sesión por primera vez en LUIS. La clave de inicio terminará quedando en desuso. Todos los usuarios de LUIS deberán [migrar sus cuentas](luis-migration-authoring.md#what-is-migration) y vincular sus aplicaciones de LUIS a un recurso de creación. A diferencia del recurso de creación, este recurso no proporciona permisos para el control de acceso basado en roles de Azure. <br><br> Al igual que el recurso de creación, el recurso de inicio le otorga 1 millón de transacciones de creación gratuitas y 1000 solicitudes de punto de conexión de predicción de prueba gratuitas.|-|No es un recurso de Azure.|
|[Clave de recursos de varios servicios de Cognitive Services](../cognitive-services-apis-create-account-cli.md?tabs=windows#create-a-cognitive-services-resource)|Consulte las solicitudes de punto de conexión de predicción compartidas con LUIS y otros servicios de Cognitive Services admitidos.|`CognitiveServices`|`Cognitive Services`|


> [!Note]
> LUIS proporciona dos tipos de recursos F0 (nivel gratuito): uno para las transacciones de creación y otro para las transacciones de predicción. Si está agotando la cuota de transacciones de predicción gratuitas, asegúrese de que está usando el recurso de predicción F0, que le otorga 10 000 transacciones gratuitas al mes, y no el recurso de creación, que le da 1000 transacciones de predicción al mes.

Cuando finalice el proceso de creación de recursos de Azure, [asigne el recurso](#assign-a-resource-to-an-app) a la aplicación del portal de LUIS.

> [!important]
> Debe crear las aplicaciones de LUIS en las [regiones](luis-reference-regions.md#publishing-regions) donde desee publicar y consultar contenido.

## <a name="resource-ownership"></a>Propiedad de recurso

Los recursos de LUIS, como todos los recursos de Azure, pertenecen a la suscripción que los contiene.

Para cambiar la propiedad de un recurso, puede realizar una de estas acciones:
* Transferir la [propiedad](../../cost-management-billing/manage/billing-subscription-transfer.md) de la suscripción.
* Exportar la aplicación de LUIS como un archivo y, a continuación, importar la aplicación en otra suscripción. La exportación está disponible en la página **Mis aplicaciones** del portal de LUIS.


## <a name="resource-limits"></a>Límites de recursos

### <a name="authoring-key-creation-limits"></a>Limites al establecimiento de claves de creación

Puede tener hasta 10 claves de creación por región y suscripción.

Para obtener más información, consulte los [límites de clave](luis-limits.md#key-limits) y las [regiones de Azure](luis-reference-regions.md).

Las regiones de publicación son diferentes de las regiones de creación. Asegúrese de que crea la aplicación en la región de creación que corresponde a la región de publicación en la que quiere ubicar su aplicación cliente.

### <a name="errors-for-key-usage-limits"></a>Errores de los límites de uso de claves

El límite de uso depende del plan de tarifa.

Si supera su cuota de transacciones por segundo (TPS), recibirá un error HTTP 429. Si supera su cuota de transacciones por mes (TPM), recibirá un error HTTP 403.


### <a name="reset-an-authoring-key"></a>Restablecimiento de las claves de creación

En el caso de aplicaciones [migradas a recursos de creación](luis-migration-authoring.md): si la clave de creación se ve comprometida, restablézcala en Azure Portal, en la página **Claves** de ese recurso de creación.

En el caso de las aplicaciones que no se han migrado: la clave se restablece en todas las aplicaciones en el portal de LUIS. Si crea sus aplicaciones a través de las API de creación, debe cambiar el valor de `Ocp-Apim-Subscription-Key` por la clave nueva.

### <a name="regenerate-an-azure-key"></a>Regeneración de una clave de Azure

Puede volver a generar una clave de Azure desde la página **Claves** de Azure Portal.


<a name="securing-the-endpoint"></a>

## <a name="app-ownership-access-and-security"></a>Propiedad, acceso y seguridad de la aplicación

Una aplicación se define mediante sus recursos de Azure, que se determinan en función de la suscripción del propietario.

Puede mover la aplicación de LUIS. Use los siguientes recursos como ayuda para moverla mediante Azure Portal o la CLI de Azure:

* [Traslado de las aplicaciones entre recursos de creación de LUIS](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-move-app-to-another-luis-authoring-azure-resource)
* [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](../../azure-resource-manager/management/move-resource-group-and-subscription.md)
* [Traslado de los recursos dentro de la misma suscripción o entre suscripciones](../../azure-resource-manager/management/move-limitations/app-service-move-limitations.md)


### <a name="contributions-from-other-authors"></a>Contribuciones de otros autores

En el caso de aplicaciones [migradas a recursos de creación](luis-migration-authoring.md): puede administrar los _colaboradores_ de un recurso de creación en Azure Portal mediante la página **Control de acceso (IAM)** . Obtenga información acerca de [cómo agregar un usuario](luis-how-to-collaborate.md) mediante la dirección de correo electrónico del colaborador y el rol de colaborador.

En el caso de las aplicaciones que todavía no se han migrado: puede administrar todos los _colaboradores_ en la página **Administrar -> Colaboradores** del portal de LUIS.

### <a name="query-prediction-access-for-private-and-public-apps"></a>Acceso de predicción de consultas para aplicaciones públicas y privadas

En las aplicaciones privadas, el acceso al entorno de ejecución de consultas de predicción está disponible para los propietarios y los colaboradores. En el caso de las aplicaciones públicas, el acceso al entorno de ejecución está disponible para los usuarios que tienen su propio recurso de entorno de ejecución de Azure [Cognitive Services](../cognitive-services-apis-create-account.md) o de [LUIS](#create-resources-in-the-azure-portal) y el identificador de la aplicación pública.

Actualmente no hay ningún catálogo de aplicaciones públicas.

### <a name="authoring-permissions-and-access"></a>Creación de permisos y accesos
El acceso a una aplicación desde el portal de [LUIS](luis-reference-regions.md#luis-website) o las [API de creación](https://go.microsoft.com/fwlink/?linkid=2092087) lo controla el recurso de creación de Azure.

El propietario y todos los colaboradores tienen acceso a la creación de la aplicación.

|El acceso de creación incluye:|Notas|
|--|--|
|Agregar o quitar claves del punto de conexión||
|Exportar versión||
|Exportar los registros de punto de conexión||
|Importar versión||
|Hacer pública la aplicación|Cuando una aplicación es pública, cualquiera que tenga una clave de creación o del punto de conexión puede consultar la aplicación.|
|Modificar el modelo|
|Publicar|
|Revisar las expresiones de punto de conexión para un [aprendizaje activo](luis-how-to-review-endpoint-utterances.md)|
|Train|

<a name="prediction-endpoint-runtime-key"></a>

### <a name="prediction-endpoint-runtime-access"></a>Acceso de tiempo de ejecución del punto de conexión de predicción

El acceso para consultar el punto de conexión de predicción se controla mediante una opción de configuración de la página **Información de la aplicación** en la sección **Administrar**.

|[Punto de conexión privado](#runtime-security-for-private-apps)|[Punto de conexión público](#runtime-security-for-public-apps)|
|:--|:--|
|Disponible para el propietario y los colaboradores.|Disponible para el propietario, los colaboradores y cualquier otra persona que conozca el identificador de la aplicación.|

Puede controlar quién puede ver su clave de tiempo de ejecución de LUIS llamándola en un entorno de servidor a servidor. Si usa LUIS desde un bot, la conexión entre el bot y LUIS ya es más segura. Si está llamando al punto de conexión de LUIS directamente, debe crear una API de lado servidor (como una [función](https://azure.microsoft.com/services/functions/) de Azure) con acceso controlado (mediante algún sistema tipo [Azure AD](https://azure.microsoft.com/services/active-directory/)). Cuando se llama a la API del lado servidor y se comprueba la autenticación y la autorización, pase la llamada a LUIS. Esta estrategia no impide ataques de tipo "Man in the middle". Sin embargo, ofusca la clave y la dirección URL del punto de conexión ante los usuarios, le permite realizar un seguimiento del acceso y le permite agregar un registro de respuesta del punto de conexión (como [Application Insights](https://azure.microsoft.com/services/application-insights/)).

### <a name="runtime-security-for-private-apps"></a>Seguridad del tiempo de ejecución para aplicaciones privadas

El entorno de ejecución de una aplicación privada solo está disponible para las claves siguientes:

|Clave y usuario|Explicación|
|--|--|
|Clave de creación del propietario| Hasta 1000 visitas del punto de conexión|
|Claves de creación de colaborador| Hasta 1000 visitas del punto de conexión|
|Cualquier clave que un autor o colaborador haya asignado a LUIS|Según el nivel de uso de las claves|

### <a name="runtime-security-for-public-apps"></a>Seguridad del tiempo de ejecución para aplicaciones públicas

Una vez que una aplicación se ha configurado como pública, _cualquier_ clave de creación o clave del punto de conexión de LUIS válida puede consultarla, siempre y cuando la clave no haya alcanzado el límite de cuota del punto de conexión.

Un usuario que no sea propietario ni colaborador solo puede acceder al entorno de ejecución de una aplicación pública si se le proporciona el identificador de la aplicación. LUIS no tiene un mercado público ni otro medio para que los usuarios busquen una aplicación pública.

Una aplicación pública está disponible en todas las regiones. Por lo tanto, un usuario con una clave de recurso de LUIS basada en regiones puede acceder a la aplicación en cualquier región que esté asociada a la clave de recurso.


### <a name="control-access-to-your-query-prediction-endpoint"></a>Control del acceso de la consulta al punto de conexión de predicción

Puede controlar quién puede ver su clave del punto de conexión de tiempo de ejecución de predicción de LUIS llamándola a un entorno de servidor a servidor. Si usa LUIS desde un bot, la conexión entre el bot y LUIS ya es más segura. Si está llamando al punto de conexión de LUIS directamente, debe crear una API de lado servidor (como una [función](https://azure.microsoft.com/services/functions/) de Azure) con acceso controlado (mediante algún sistema tipo [Azure AD](https://azure.microsoft.com/services/active-directory/)). Cuando se llama a la API de servidor y se comprueba la autenticación y la autorización, pase la llamada a LUIS. Esta estrategia no impide ataques de tipo "Man in the middle". Sin embargo, ofusca el punto de conexión ante los usuarios, le permite realizar un seguimiento del acceso y le permite agregar un registro de respuesta del punto de conexión (como [Application Insights](https://azure.microsoft.com/services/application-insights/)).

<a name="starter-key"></a>

## <a name="sign-in-to-the-luis-portal-and-begin-authoring"></a>Inicio de sesión en el portal de LUIS y comienzo del proceso de creación

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai) y acepte las condiciones de uso.
1. Elija la clave de creación de LUIS de Azure para empezar a crear la aplicación de LUIS:

   ![Captura de pantalla que muestra la pantalla de bienvenida.](./media/luis-how-to-azure-subscription/sign-in-create-resource.png)

1. Cuando haya terminado con el proceso de selección de recursos, [cree una aplicación nueva](luis-how-to-start-new-app.md#create-new-app-in-luis).


<a name="create-azure-resources"></a>
<a name="create-resources-in-the-azure-portal"></a>

[!INCLUDE [Create LUIS resource in Azure portal](includes/create-luis-resource.md)]

### <a name="create-resources-in-the-azure-cli"></a>Creación de recursos en la CLI de Azure

Use la [CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest) para crear cada recursos de manera individual.

Recurso: `kind`:

* Creación: `LUIS.Authoring`
* Predicción: `LUIS`

1. Inicie sesión en la CLI de Azure.

    ```azurecli
    az login
    ```

    Este comando abre un explorador para que pueda seleccionar la cuenta correcta y proporcionar autenticación.

1. Defina un recurso de creación de LUIS de tipo `LUIS.Authoring`, denominado `my-luis-authoring-resource`. Créelo en el grupo de recursos _existente_ denominado `my-resource-group` para la región `westus`.

    ```azurecli
    az cognitiveservices account create -n my-luis-authoring-resource -g my-resource-group --kind LUIS.Authoring --sku F0 -l westus --yes
    ```

1. Cree un recurso de punto de conexión de predicción de LUIS de tipo `LUIS`, denominado `my-luis-prediction-resource`. Créelo en el grupo de recursos _existente_ denominado `my-resource-group` para la región `westus`. Si quiere contar con una capacidad de proceso superior a la que proporciona el nivel de servicio gratis, cambie `F0` a `S0`. Más información sobre [planes de tarifa y la capacidad de proceso](luis-limits.md#key-limits).

    ```azurecli
    az cognitiveservices account create -n my-luis-prediction-resource -g my-resource-group --kind LUIS --sku F0 -l westus --yes
    ```

    > [!Note]
    > Estas claves no se usan en el portal de LUIS hasta que se asignan en dicho portal, en la página **Administrar** > **Recursos de Azure**.

<a name="assign-an-authoring-resource-in-the-luis-portal-for-all-apps"></a>

### <a name="assign-resources-in-the-luis-portal"></a>Asignación del recurso en el portal de LUIS

Puede asignar un recurso de creación de una sola aplicación o de todas las aplicaciones en LUIS. En el siguiente procedimiento se asignan todas las aplicaciones a un único recurso de creación.

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai).
1. En la esquina superior derecha, seleccione su cuenta de usuario y, a continuación, seleccione **Configuración**.
1. En la página **User Settings** (Configuración de usuario), seleccione **Add authoring resource** (Agregar recurso de creación) y, a continuación, seleccione un recurso de creación existente. Seleccione **Guardar**.

## <a name="assign-a-resource-to-an-app"></a>Asignar un recurso a una aplicación

>[!NOTE]
>Si no tiene una suscripción de Azure, no podrá asignar ni crear un recurso nuevo. Primero tendrá que crear una [cuenta gratuita de Azure](https://azure.microsoft.com/en-us/free/) y, luego, volver a LUIS para crear un recurso desde el portal.

Puede usar este procedimiento para crear un recurso de creación o predicción o asignar uno a una aplicación: 

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai). Seleccione una aplicación de la lista **Mis aplicaciones**.
1. Vaya a **Administrar** > **Recursos de Azure**:

    ![Captura de pantalla que muestra la página Recursos de Azure.](./media/luis-how-to-azure-subscription/manage-azure-resources-prediction.png)

1. En la pestaña **Prediction resource** (Recurso de predicción) o **Authoring resource** (Recurso de creación), seleccione el botón **Add prediction resource** (Agregar recurso de predicción) o **Add authoring resource** (Agregar recurso de creación), respectivamente.
1. Use los campos del formulario para buscar el recurso correcto y, a continuación, seleccione **Save** (Guardar).
1. Si no tiene un recurso existente, puede crearlo con la opción **Create a new LUIS resource?** (¿Crear un recurso de LUIS nuevo?).


### <a name="assign-a-query-prediction-runtime-resource-without-using-the-luis-portal"></a>Asignación de recursos de entorno de ejecución de consultas de predicción sin usar el portal de LUIS

En el caso de procesos automatizados, como las canalizaciones de CI/CD, puede automatizar la asignación de un recurso de entorno de ejecución de LUIS a una aplicación de LUIS. Para ello, complete los siguientes pasos:

1. Obtenga un token de Azure Resource Manager en [este sitio web](https://resources.azure.com/api/token?plaintext=true). Este token expira, por lo que debe usarlo de inmediato. La solicitud devuelve un token de Azure Resource Manager.

    ![Captura de pantalla que muestra el sitio web para solicitar un token de Azure Resource Manager.](./media/luis-manage-keys/get-arm-token.png)

1. Use el token para solicitar los recursos de entorno de ejecución de LUIS en distintas suscripciones. Use la [API Get LUIS Azure accounts](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be313cec181ae720aa2b26c), a la que su cuenta de usuario tiene acceso.

    Esta API POST requiere los siguientes valores:

    |Encabezado|Value|
    |--|--|
    |`Authorization`|El valor de `Authorization` es `Bearer {token}`. El valor del token debe ir precedido de la palabra `Bearer` y un espacio.|
    |`Ocp-Apim-Subscription-Key`|Su clave de creación.|

    La API devuelve una matriz de objetos JSON que representan las suscripciones a LUIS. Los valores devueltos incluyen el identificador de suscripción, el grupo de recursos y el nombre de recurso, devuelto como `AccountName`. Busque en la matriz el elemento que corresponde al recurso de LUIS que desea asignar a la aplicación de LUIS.

1. Asigne el token al recurso de LUIS mediante la API [Assign a LUIS Azure accounts to an application](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be32228e8473de116325515).

    Esta API POST requiere los siguientes valores:

    |Tipo|Configuración|Value|
    |--|--|--|
    |Encabezado|`Authorization`|El valor de `Authorization` es `Bearer {token}`. El valor del token debe ir precedido de la palabra `Bearer` y un espacio.|
    |Encabezado|`Ocp-Apim-Subscription-Key`|Su clave de creación.|
    |Encabezado|`Content-type`|`application/json`|
    |QueryString|`appid`|El id. de la aplicación LUIS.
    |Body||{"AzureSubscriptionId":"ddda2925-af7f-4b05-9ba1-2155c5fe8a8e",<br>"ResourceGroup": "resourcegroup-2",<br>"AccountName": "luis-uswest-S0-2"}|

    Cuando esta API finaliza correctamente, devuelve `201 - created status`.

## <a name="unassign-a-resource"></a>Anulación de la asignación de un recurso

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai) y seleccione una aplicación de la lista **Mis aplicaciones**.
1. Vaya a **Administrar** > **Recursos de Azure**.
1. En la pestaña **Prediction resource** (Recurso de predicción) o **Authoring resource** (Recurso de creación), seleccione el botón **Unassign resource** (Anular asignación del recurso) correspondiente.

Al anular la asignación de un recurso, no se elimina de Azure; solo se desvincula de LUIS.


## <a name="delete-an-account"></a>Eliminación de una cuenta

Vea [Data storage and removal](luis-concept-data-storage.md#accounts) (Almacenamiento y eliminación de datos) para obtener información acerca de qué datos se eliminan al eliminar la cuenta.

## <a name="change-the-pricing-tier"></a>Cambio del plan de tarifa

1.  En [Azure Portal](https://portal.azure.com), busque y seleccione la suscripción a LUIS:

    ![Captura de pantalla que muestra una suscripción a LUIS en Azure Portal.](./media/luis-usage-tiers/find.png)
1.  Seleccione **Plan de tarifa** para ver los planes de tarifa disponibles:

    ![Captura de pantalla que muestra el elemento de menú Plan de tarifa.](./media/luis-usage-tiers/subscription.png)
1.  Elija el plan de tarifa y después haga clic en **Seleccionar** para guardar el cambio:

    ![Captura de pantalla que muestra cómo seleccionar y guardar un plan de tarifa.](./media/luis-usage-tiers/plans.png)

    Una vez completado el cambio de tarifa, una ventana emergente verifica la actualización:

    ![Captura de pantalla de la ventana emergente que verifica la actualización de precios.](./media/luis-usage-tiers/updated.png)
1. No olvide [asignar esta clave de punto de conexión](#assign-a-resource-to-an-app) en la página **Publicar** y usarla en todas las consultas de punto de conexión.

## <a name="view-azure-resource-metrics"></a>Visualización de las métricas de recursos de Azure

### <a name="view-a-summary-of-azure-resource-usage"></a>Visualización de un resumen del uso de recursos de Azure
En Azure Portal puede ver la información de uso de LUIS. En la página **Información general** se muestra un resumen, incluidas las llamadas y los errores recientes. Si realiza una solicitud de punto de conexión de LUIS, espere hasta cinco minutos para que aparezca el cambio.

![Captura de pantalla que muestra la página de información general.](./media/luis-usage-tiers/overview.png)

### <a name="customizing-azure-resource-usage-charts"></a>Personalización de los gráficos de uso de los recursos de Azure
La página **Métricas** proporciona una vista más detallada de los datos:

![Captura de pantalla que muestra la página Métricas.](./media/luis-usage-tiers/metrics-default.png)

Puede configurar los gráficos de métricas para un período de tiempo y un tipo de métrica específicos:

![Captura de pantalla que muestra un gráfico personalizado.](./media/luis-usage-tiers/metrics-custom.png)

### <a name="total-transactions-threshold-alert"></a>Alerta de umbral de transacciones totales
Si desea saber cuándo se alcanza un umbral de transacciones determinado (por ejemplo 10 000 transacciones), puede crear una alerta:

![Captura de pantalla que muestra la página Reglas de alerta.](./media/luis-usage-tiers/alert-default.png)

Agregue una alerta de métrica para la métrica **Llamadas totales** para un período de tiempo concreto. Agregue las direcciones de correo electrónico de todos los usuarios que deben recibir la alerta. Agregue webhooks de todos los sistemas que deben recibir la alerta. También puede ejecutar una aplicación lógica cuando se desencadene la alerta.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre [cómo usar versiones](luis-how-to-manage-versions.md) para controlar el ciclo de vida de la aplicación.
* Migre al nuevo [recurso de creación](luis-migration-authoring.md).
