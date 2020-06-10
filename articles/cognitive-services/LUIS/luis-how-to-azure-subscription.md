---
title: Cómo usar las claves de creación y tiempo de ejecución - LUIS
description: La primera vez que use Language Understanding (LUIS), no es necesario crear una clave de suscripción. Cuando quiera publicar la aplicación, use el punto de conexión en tiempo de ejecución; a continuación, debe crear y asignar la clave de tiempo de ejecución a la aplicación.
services: cognitive-services
ms.topic: how-to
ms.date: 04/06/2020
ms.openlocfilehash: c566e8fe56d19856f5a577e472929b7610497d7c
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84344465"
---
# <a name="create-luis-resources"></a>Creación de recursos de LUIS

Los recursos de creación y de tiempo de ejecución proporcionan autenticación a la aplicación LUIS y al punto de conexión de predicción.

<a name="create-luis-service"></a>
<a name="create-language-understanding-endpoint-key-in-the-azure-portal"></a>

Al iniciar sesión en el portal de LUIS, puede continuar usando:

* una [clave de evaluación](#trial-key) gratuita que proporciona la creación y algunas consultas de puntos de conexión de predicción.
* un recurso de [creación de LUIS](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) de Azure.

<a name="starter-key"></a>

## <a name="sign-in-to-luis-portal-and-begin-authoring"></a>Inicio de sesión en el portal de LUIS y comience el proceso de creación

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai) y acepte las condiciones de uso.
1. Inicie la aplicación LUIS seleccionando el tipo de clave de creación de LUIS que quiera usar: la clave de evaluación gratuita o la nueva clave de creación de LUIS de Azure.

    ![Elección de un tipo de recurso de creación de Language Understanding](./media/luis-how-to-azure-subscription/sign-in-create-resource.png)

1. Cuando haya terminado con el proceso de selección de recursos, [cree una aplicación nueva](luis-how-to-start-new-app.md#create-new-app-in-luis).

## <a name="trial-key"></a>Clave de la versión de prueba

La clave de la versión de prueba (de inicio) se proporciona automáticamente. Se usa como clave de autenticación para consultar el tiempo de ejecución del punto de conexión de predicción, y es capaz de realizar hasta 1000 consultas al mes.

Se puede encontrar en la página de **configuración de usuario** y en las páginas **Administrar -> Recursos de Azure** del portal de LUIS.

Cuando esté listo para publicar el punto de conexión de predicción, [cree](#create-luis-resources) y [asigne](#assign-a-resource-to-an-app) las claves de creación y de tiempo de ejecución de predicción para reemplazar la funcionalidad de la clave de inicio.

<a name="create-resources-in-the-azure-portal"></a>


[!INCLUDE [Create LUIS resource](includes/create-luis-resource.md)]

## <a name="create-resources-in-azure-cli"></a>Creación de recursos en la CLI de Azure

Use la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) para crear cada recursos de manera individual.

Recurso: `kind`:

* Creación: `LUIS.Authoring`
* Predicción: `LUIS`

1. Inicie sesión en la CLI de Azure.

    ```azurecli
    az login
    ```

    Se abre un explorador que le permite seleccionar la cuenta correcta y proporcionar autenticación.

1. Cree un **recurso de creación de LUIS**, de tipo `LUIS.Authoring`, llamado `my-luis-authoring-resource` en el grupo de recursos _existente_ denominado `my-resource-group` en la región `westus`.

    ```azurecli
    az cognitiveservices account create -n my-luis-authoring-resource -g my-resource-group --kind LUIS.Authoring --sku F0 -l westus --yes
    ```

1. Cree un **recurso de punto de conexión de predicción de LUIS**, de tipo `LUIS`, llamado `my-luis-prediction-resource` en el grupo de recursos _existente_ denominado `my-resource-group` en la región `westus`. Si quiere un rendimiento superior al del nivel gratuito, cambie `F0` por `S0`. Más información sobre [planes de tarifa y rendimiento](luis-limits.md#key-limits).

    ```azurecli
    az cognitiveservices account create -n my-luis-prediction-resource -g my-resource-group --kind LUIS --sku F0 -l westus --yes
    ```

    > [!Note]
    > Estas claves **no** se usan en el portal de LUIS hasta que se asignan en dicho portal en **Manage -> Azure resources** (Administrar > Recursos de Azure).

## <a name="assign-an-authoring-resource-in-the-luis-portal-for-all-apps"></a>Asignación de un recurso de creación en el portal de LUIS para todas las aplicaciones

Puede asignar un recurso de creación de una sola aplicación o de todas las aplicaciones en LUIS. En el siguiente procedimiento se asignan todas las aplicaciones a un único recurso de creación.

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai).
1. En la barra de navegación superior, en el extremo derecho, seleccione su cuenta de usuario y, a continuación, seleccione **Settings** (Configuración).
1. En la página de **configuración de usuario**, seleccione **Add authoring resource** (Agregar recurso de creación) y, a continuación, seleccione un recurso de creación existente. Seleccione **Guardar**.

## <a name="assign-a-resource-to-an-app"></a>Asignar un recurso a una aplicación

Puede asignar un único recurso, creación o tiempo de ejecución de punto de conexión de predicción a una aplicación con el siguiente procedimiento.

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai) y seleccione una aplicación de la lista **My apps** (Mis aplicaciones).
1. Vaya a la página **Manage -> Azure resources** (Administrar -> Recursos de Azure).

    ![Seleccione Manage -> Azure resources (Administrar -> Recursos de Azure) en el portal de LUIS para asignar un recurso a la aplicación.](./media/luis-how-to-azure-subscription/manage-azure-resources-prediction.png)

1. Seleccione la pestaña de predicción o de recursos de creación y, a continuación, seleccione el botón **Add prediction resource** (Agregar recurso de predicción) o **Add authoring resource** (Agregar recurso de creación).
1. Seleccione los campos en el formulario para buscar el recurso correcto y, a continuación, seleccione **Save** (Guardar).

### <a name="assign-runtime-resource-without-using-luis-portal"></a>Asignación de recursos de tiempo de ejecución sin usar el portal de LUIS

Para fines de automatización, como una canalización de CI/CD, puede automatizar la asignación de un recurso de tiempo de ejecución de LUIS a una aplicación de LUIS. Para ello, tiene que realizar los siguientes pasos:

1. Obtenga un token de Azure Resource Manager en este [sitio web](https://resources.azure.com/api/token?plaintext=true). Este token expira, por lo que debe usarlo de inmediato. La solicitud devuelve un token de Azure Resource Manager.

    ![Solicitud de un token de Azure Resource Manager y recepción del token de Azure Resource Manager](./media/luis-manage-keys/get-arm-token.png)

1. Use el token para solicitar los recursos de tiempo de ejecución de LUIS en distintas suscripciones desde la [API Get LUIS azure accounts](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be313cec181ae720aa2b26c), a la cual su cuenta de usuario tiene acceso.

    Esta API POST requiere la siguiente configuración:

    |Encabezado|Value|
    |--|--|
    |`Authorization`|El valor de `Authorization` es `Bearer {token}`. Tenga en cuenta que el valor del token debe ir precedido de la palabra `Bearer` y un espacio.|
    |`Ocp-Apim-Subscription-Key`|Su clave de creación.|

    Esta API devuelve una matriz de objetos JSON con las suscripciones de LUIS, incluidos el identificador de suscripción, el grupo de recursos y el nombre del recurso, que se devuelve como nombre de cuenta. Busque el elemento de la matriz que es el recurso de LUIS que se va a asignar a la aplicación de LUIS.

1. Asigne el token al recurso de LUIS con la API [Assign a LUIS azure accounts to an application](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be32228e8473de116325515).

    Esta API POST requiere la siguiente configuración:

    |Tipo|Configuración|Value|
    |--|--|--|
    |Encabezado|`Authorization`|El valor de `Authorization` es `Bearer {token}`. Tenga en cuenta que el valor del token debe ir precedido de la palabra `Bearer` y un espacio.|
    |Encabezado|`Ocp-Apim-Subscription-Key`|Su clave de creación.|
    |Encabezado|`Content-type`|`application/json`|
    |QueryString|`appid`|El id. de la aplicación LUIS.
    |Body||{"AzureSubscriptionId":"ddda2925-af7f-4b05-9ba1-2155c5fe8a8e",<br>"ResourceGroup": "resourcegroup-2",<br>"AccountName": "luis-uswest-S0-2"}|

    Cuando esta API finaliza correctamente, devuelve un estado 201: creado.

## <a name="unassign-resource"></a>Anulación de la asignación de un recurso

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai) y seleccione una aplicación de la lista **My apps** (Mis aplicaciones).
1. Vaya a la página **Manage -> Azure resources** (Administrar -> Recursos de Azure).
1. Seleccione la pestaña de predicción o de recursos de creación y, a continuación, seleccione el botón **Unassign resource** (Cancelar asignación del recurso).

Al cancelar la asignación de un recurso, no se elimina de Azure. Solo se desvincula de LUIS.

## <a name="reset-authoring-key"></a>Restablecimiento de la clave de creación

**Para [crear aplicaciones migradas](luis-migration-authoring.md) de recursos**: si la clave de creación se ve comprometida, restablezca la clave en Azure Portal en la página **Claves** para ese recurso de creación.

**En el caso de las aplicaciones que no se han migrado todavía**: la clave se restablece en todas las aplicaciones del portal de LUIS. Si crea sus aplicaciones a través de las API de creación, debe cambiar el valor de Ocp-Apim-Subscription-Key por la clave nueva.

## <a name="regenerate-azure-key"></a>Regeneración de la clave de Azure

Vuelva a generar las claves de Azure desde Azure Portal, en la página de **claves**.

## <a name="delete-account"></a>Eliminación de cuenta

Vea [Data storage and removal](luis-concept-data-storage.md#accounts) (Almacenamiento y eliminación de datos) para obtener información acerca de qué datos se eliminan al eliminar la cuenta.

## <a name="change-pricing-tier"></a>Cambiar el plan de tarifa

1.  En [Azure](https://portal.azure.com), busque la suscripción de LUIS. Seleccione la suscripción de LUIS.
    ![Buscar la suscripción de LUIS](./media/luis-usage-tiers/find.png)
1.  Seleccione **Plan de tarifa** para ver los planes de tarifa disponibles.
    ![Ver los planes de tarifa](./media/luis-usage-tiers/subscription.png)
1.  Seleccione el plan de tarifa y después **Seleccionar** para guardar el cambio.
    ![Cambiar el plan de pago de LUIS](./media/luis-usage-tiers/plans.png)
1.  Una vez completado el cambio de tarifa, el plan de tarifa nuevo se comprueba en una ventana emergente.
    ![Comprobar el plan de pago de LUIS](./media/luis-usage-tiers/updated.png)
1. No olvide [asignar esta clave de punto de conexión](#assign-a-resource-to-an-app) en la página **Publicar** y usarla en todas las consultas de punto de conexión.

## <a name="viewing-azure-resource-metrics"></a>Visualización de las métricas de recursos de Azure

### <a name="viewing-azure-resource-summary-usage"></a>Visualización del uso de resumen de los recursos de Azure
En Azure puede ver la información de uso de LUIS. En la página **Información general** se muestra información de resumen reciente incluidos los errores y las llamadas. Si realiza una solicitud de punto de conexión de LUIS, consulte inmediatamente la **página Información general**, y deje que pasen hasta cinco minutos hasta que se muestre el uso.

![Ver uso de resumen](./media/luis-usage-tiers/overview.png)

### <a name="customizing-azure-resource-usage-charts"></a>Personalización de los gráficos de uso de los recursos de Azure
Las métricas proporcionan una vista más detallada de los datos.

![Métricas predeterminadas](./media/luis-usage-tiers/metrics-default.png)

Puede configurar los gráficos de métricas para el período de tiempo y el tipo de métrica.

![Métricas personalizadas](./media/luis-usage-tiers/metrics-custom.png)

### <a name="total-transactions-threshold-alert"></a>Alerta de umbral de transacciones totales
Si le gustaría saber cuándo se alcanza un umbral de transacciones determinado (por ejemplo 10.000) puede crear una alerta.

![Alertas predeterminadas](./media/luis-usage-tiers/alert-default.png)

Agregue una alerta de métrica para la métrica **Llamadas totales** para un período de tiempo concreto. Agregue las direcciones de correo electrónico de todos los usuarios que deben recibir la alerta. Agregue webhooks para todos los sistemas que deben recibir la alerta. También puede ejecutar una aplicación lógica cuando se desencadene la alerta.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre [cómo usar versiones](luis-how-to-manage-versions.md) para controlar el ciclo de vida de la aplicación.
* Entienda los conceptos, incluidos el [recurso de creación](luis-concept-keys.md#authoring-key) y los [colaboradores](luis-concept-keys.md#contributions-from-other-authors) de ese recurso.
* Obtenga información sobre [cómo crear](luis-how-to-azure-subscription.md) los recursos de creación y de tiempo de ejecución.
* Migre al nuevo [recurso de creación](luis-migration-authoring.md).
