---
title: Acceso seguro y datos
description: Proteja el acceso a las entradas, las salidas, los desencadenadores basados en solicitudes, el historial de ejecución, las tareas de administración y el acceso a otros recursos en Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 02/04/2020
ms.openlocfilehash: 4fc4960eb3af8a3d3c9902c9b24505bb5610b709
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80657173"
---
# <a name="secure-access-and-data-in-azure-logic-apps"></a>Proteger el acceso y los datos en Azure Logic Apps

Para controlar el acceso y proteger los datos en Azure Logic Apps, puede configurar la seguridad en estas áreas:

* [Acceso a desencadenadores de solicitud](#secure-triggers)
* [Acceso a las operaciones de las aplicaciones lógicas](#secure-operations)
* [Acceso a las entradas y salidas del historial de ejecución](#secure-run-history)
* [Acceso a las entradas de parámetros](#secure-action-parameters)
* [Acceso a los servicios y sistemas invocados desde aplicaciones lógicas](#secure-requests)

<a name="secure-triggers"></a>

## <a name="access-to-request-based-triggers"></a>Acceso a desencadenadores de solicitud

Si la aplicación lógica usa un desencadenador basado en solicitudes que recibe llamadas o solicitudes entrantes, como el desencadenador [Solicitud](../connectors/connectors-native-reqres.md) o [Webhook](../connectors/connectors-native-webhook.md), puede limitar el acceso para que solo los clientes autorizados puedan llamar a la aplicación lógica. Todas las solicitudes que recibe una aplicación lógica están cifradas y protegidas con Seguridad de la capa de transporte (TLS), antes conocido como protocolo de Capa de sockets seguros (SSL).

Algunas opciones que permiten proteger el acceso a este tipo de desencadenador son:

* [Generación de firmas de acceso compartido](#sas)
* [Direcciones IP entrantes restringidas](#restrict-inbound-ip-addresses)
* [Incorporación de Azure Active Directory, OAuth u otra medida de seguridad](#add-authentication)

<a name="sas"></a>

### <a name="generate-shared-access-signatures-sas"></a>Generación de firmas de acceso compartido (SAS)

Cada de punto de conexión de la solicitud de una aplicación lógica tiene una [Firma de acceso compartido (SAS)](https://docs.microsoft.com/rest/api/storageservices/constructing-a-service-sas) en la dirección URL del punto de conexión con el formato siguiente:

`https://<request-endpoint-URI>sp=<permissions>sv=<SAS-version>sig=<signature>`

Cada dirección URL contiene el parámetro de consulta `sp`, `sv` y `sig`, tal y como se describe en esta tabla:

| Parámetro de consulta | Descripción |
|-----------------|-------------|
| `sp` | Especifica los permisos para los métodos HTTP permitidos que se usarán. |
| `sv` | Especifica la versión de SAS que se usará para generar la firma. |
| `sig` | Especifica la firma que se usará para autenticar el acceso al desencadenador. Esta firma se genera mediante el algoritmo SHA256 con una clave de acceso secreta en todas las rutas de acceso de direcciones URL y propiedades. Esta clave nunca se expone ni se publica, y se mantiene cifrada y almacenada con la aplicación lógica. La aplicación lógica solo autoriza los desencadenadores que contienen una firma válida creada con la clave secreta. |
|||

Para obtener más información sobre la protección del acceso con la Firma de acceso compartido, consulte estas secciones del tema:

* [Regenerar las claves de acceso](#access-keys)
* [Crear direcciones URL de devolución de llamada de expiración próxima](#expiring-urls)
* [Crear direcciones URL con clave principal o secundaria](#primary-secondary-key)

<a name="access-keys"></a>

#### <a name="regenerate-access-keys"></a>Regenerar las claves de acceso

Para generar una nueva clave de acceso de seguridad en cualquier momento, use la API REST de Azure o Azure Portal. Se invalidan todas las direcciones URL generadas previamente que usan la clave anterior y ya no tienen autorización para desencadenar la aplicación lógica. Las direcciones URL que se recuperan tras la regeneración se firman con la nueva clave de acceso.

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica donde está la clave que quiere regenerar.

1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Claves de acceso**.

1. Seleccione la clave que quiere regenerar y finalice el proceso.

<a name="expiring-urls"></a>

#### <a name="create-expiring-callback-urls"></a>Crear direcciones URL de devolución de llamada de expiración próxima

Si comparte la dirección URL del punto de conexión de un desencadenador basado en solicitudes con otras partes, puede generar direcciones URL de devolución de llamada con claves específicas y fechas de expiración. De esta forma puede acumular claves fácilmente o restringir el acceso para desencadenar la aplicación lógica en función de un determinado intervalo de tiempo. Para especificar una fecha de expiración para una dirección URL, use la [API REST de Logic Apps](https://docs.microsoft.com/rest/api/logic/workflowtriggers); por ejemplo:

``` http
POST /subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<workflow-name>/triggers/<trigger-name>/listCallbackUrl?api-version=2016-06-01
```

En el cuerpo, incluya la propiedad `NotAfter` mediante una cadena de fecha de JSON. Esta propiedad devuelve una dirección URL de devolución de llamada que solo es válida hasta la fecha y hora `NotAfter`.

<a name="primary-secondary-key"></a>

#### <a name="create-urls-with-primary-or-secondary-secret-key"></a>Creación de direcciones URL con clave secreta principal o secundaria

Al generar o enumerar direcciones URL de devolución de llamada para desencadenadores basados en solicitudes, puede especificar qué clave se va a usar para firmar la dirección URL. Para generar una dirección URL firmada por una clave específica, use la [API REST de Logic Apps](https://docs.microsoft.com/rest/api/logic/workflowtriggers); por ejemplo:

``` http
POST /subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<workflow-name>/triggers/<trigger-name>/listCallbackUrl?api-version=2016-06-01
```

En el cuerpo, incluya la propiedad `KeyType` como `Primary` o `Secondary`. Esta propiedad devuelve una dirección URL firmada por una clave de seguridad especificada.

<a name="restrict-inbound-ip"></a>

### <a name="restrict-inbound-ip-addresses"></a>Direcciones IP entrantes restringidas

Junto con la Firma de acceso compartido (SAS), es posible que quiera especificar el límite de clientes que pueden llamar a su aplicación lógica. Por ejemplo, si administra el punto de conexión de solicitud mediante Azure API Management, puede restringir la aplicación lógica para que solo acepte solicitudes procedentes de la dirección IP de la instancia de API Management.

#### <a name="restrict-inbound-ip-ranges-in-azure-portal"></a>Restricción de los intervalos IP entrantes en Azure Portal

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en Diseñador de aplicación lógica.

1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Configuración de flujo de trabajo**.

1. En **Configuración del control de acceso** > **Direcciones IP entrantes permitidas**, seleccione **Intervalos IP específicos**.

1. En **Intervalos IP para desencadenadores**, especifique los intervalos de direcciones IP que acepta el desencadenador.

   Un intervalo IP válido usa estos formatos: *x.x.x.x/x* o *x.x.x.x-x.x.x.x*

Si quiere que la aplicación lógica solo se desencadene como aplicación lógica anidada, en la lista **Direcciones IP entrantes permitidas**, seleccione **Cualquier otra aplicación lógica**. Esta opción escribe una matriz vacía en el recurso de la aplicación lógica. De este modo, solo las llamadas del servicio de Logic Apps (las aplicaciones lógicas principales) pueden desencadenar la aplicación lógica anidada.

> [!NOTE]
> Independientemente de la dirección IP, de todos modos puede ejecutar una aplicación lógica que tiene un desencadenador basado en solicitud mediante el uso de `/triggers/<trigger-name>/run` a través de la API REST de Azure o de API Management. Sin embargo, este escenario sigue requiriendo la autenticación con la API REST de Azure. Todos los eventos aparecen en el registro de auditoría de Azure. Asegúrese de establecer las directivas de control de acceso como corresponda.

#### <a name="restrict-inbound-ip-ranges-in-azure-resource-manager-template"></a>Restricción de los intervalos IP entrantes en la plantilla de Azure Resource Manager

Si [automatiza las implementaciones de aplicaciones lógicas mediante una plantilla de Azure Resource Manager](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), puede especificar los intervalos IP mediante la sección `accessControl` con la sección `triggers` en la definición de recursos de la aplicación lógica; por ejemplo:

``` json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {},
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {<workflow-definition>},
            "parameters": {},
            "accessControl": {
               "triggers": {
                  "allowedCallerIpAddresses": [
                     {
                        "addressRange": "192.168.12.0/23"
                     },
                     {
                        "addressRange": "2001:0db8::/64"
                     }
                  ]
               }
            }
         }
      }
   ],
   "outputs": {}
}
```

<a name="add-authentication"></a>

### <a name="add-azure-active-directory-oauth-or-other-security"></a>Incorporación de Azure Active Directory OAuth u otra medida de seguridad

Para agregar más protocolos de autorización a la aplicación lógica, considere la posibilidad de usar el servicio de [Azure API Management](../api-management/api-management-key-concepts.md). Este servicio le permite exponer su aplicación lógica como API y le ofrece una supervisión enriquecida, seguridad, directiva y documentación para cualquier punto de conexión. API Management puede exponer un punto de conexión público o privado para la aplicación lógica. Para autorizar el acceso a este punto de conexión, puede usar [Azure Active Directory, OAuth](#azure-active-directory-oauth-authentication), el [certificado del cliente](#client-certificate-authentication) u otros estándares de seguridad para autorizar el acceso a ese extremo. Cuando API Management recibe una solicitud, el servicio la envía a la aplicación lógica y también hace cualquier transformación o restricción necesaria en el proceso. Para permitir que solo API Management desencadene la aplicación lógica, puede usar la configuración del intervalo IP entrante de la aplicación lógica.

<a name="secure-operations"></a>

## <a name="access-to-logic-app-operations"></a>Acceso a las operaciones de las aplicaciones lógicas

Puede permitir que solo determinados usuarios o grupos ejecuten tareas específicas, como administrar, editar y ver aplicaciones lógicas. Para controlar sus permisos, use el [Control de acceso basado en rol (RBAC) de Azure](../role-based-access-control/role-assignments-portal.md) para que pueda asignar roles personalizados o integrados a los miembros de la suscripción a Azure:

* [Colaborador de aplicación lógica](../role-based-access-control/built-in-roles.md#logic-app-contributor): Le permite administrar aplicaciones lógicas, pero no puede cambiar el acceso a ellas.

* [Operador de aplicación lógica](../role-based-access-control/built-in-roles.md#logic-app-operator): Le permite leer, habilitar y deshabilitar aplicaciones lógicas, pero no puede editarlas ni actualizarlas.

Para evitar que otros usuarios cambien o elimine la aplicación lógica, puede usar el [bloqueo de recursos de Azure](../azure-resource-manager/management/lock-resources.md). Esta funcionalidad evita que otros usuarios cambien o eliminen los recursos de producción.

<a name="secure-run-history"></a>

## <a name="access-to-run-history-data"></a>Acceso a los datos del historial de ejecución

Durante la ejecución de una aplicación lógica, todos los datos [se cifran en tránsito](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit) con Seguridad de la capa de transporte (TLS) y [en reposo](../security/fundamentals/encryption-atrest.md). Cuando finaliza la ejecución de la aplicación lógica, puede ver el historial de esa ejecución, incluidos los pasos que se ejecutaron junto con el estado, la duración, las entradas y las salidas de cada acción. Este completo detalle proporciona información sobre cómo se ejecuta la aplicación lógica y dónde puede empezar a solucionar los problemas que surjan.

Cuando visualiza el historial de ejecución de la aplicación lógica, Logic Apps autentica su acceso y proporciona vínculos a las entradas y salidas para las solicitudes y respuestas de cada ejecución. Sin embargo, para las acciones que controlan contraseñas, secretos, claves u otra información confidencial, es recomendable evitar que otros usuarios vean los datos y accedan a ellos. Por ejemplo, si la aplicación lógica obtiene un secreto de [Azure Key Vault](../key-vault/key-vault-overview.md) para usarlo al autenticar una acción HTTP, puede ocultar ese secreto de la vista.

Para controlar el acceso a las entradas y salidas del historial de ejecución de la aplicación lógica, tiene estas opciones:

* [Restricción del acceso por intervalo de direcciones IP](#restrict-ip).

  Esta opción le permite proteger el acceso al historial de ejecución en función de las solicitudes de un intervalo de direcciones IP específico.

* [Oculte los datos del historial de ejecución mediante ofuscación](#obfuscate).

  En muchos desencadenadores y acciones, puede ocultar las entradas, salidas o ambas del historial de ejecución de una aplicación lógica.

<a name="restrict-ip"></a>

### <a name="restrict-access-by-ip-address-range"></a>Restringir el acceso por intervalo de direcciones IP

Puede limitar el acceso a las entradas y salidas del historial de ejecución de la aplicación lógica para que solo las solicitudes de intervalos de direcciones IP específicas puedan ver los datos. Por ejemplo, para impedir que alguien tenga acceso a las entradas y salidas, especifique un intervalo de direcciones IP como, por ejemplo, `0.0.0.0-0.0.0.0`. Solo una persona con permisos de administrador puede quitar esta restricción, lo que proporciona la posibilidad de usar el acceso "Just-In-Time" para los datos de la aplicación lógica. Puede especificar los intervalos IP que se van a restringir desde Azure Portal o en una plantilla de Azure Resource Manager que se usa para la implementación de la aplicación lógica.

#### <a name="restrict-ip-ranges-in-azure-portal"></a>Restricción de los intervalos IP en Azure Portal

1. En Azure Portal, abra la aplicación lógica en Diseñador de aplicación lógica.

1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Configuración de flujo de trabajo**.

1. En **Configuración del control de acceso** > **Direcciones IP entrantes permitidas**, seleccione **Intervalos IP específicos**.

1. En **Intervalos IP para contenido**, especifique los intervalos de direcciones IP que pueden acceder a contenido desde las entradas y salidas. 

   Un intervalo IP válido usa estos formatos: *x.x.x.x/x* o *x.x.x.x-x.x.x.x*

#### <a name="restrict-ip-ranges-in-azure-resource-manager-template"></a>Restricción de los intervalos IP entrantes en la plantilla de Azure Resource Manager

Si [automatiza las implementaciones de aplicaciones lógicas mediante una plantilla de Azure Resource Manager](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), puede especificar los intervalos IP mediante la sección `accessControl` con la sección `contents` en la definición de recursos de la aplicación lógica; por ejemplo:

``` json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {},
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {<workflow-definition>},
            "parameters": {},
            "accessControl": {
               "contents": {
                  "allowedCallerIpAddresses": [
                     {
                        "addressRange": "192.168.12.0/23"
                     },
                     {
                        "addressRange": "2001:0db8::/64"
                     }
                  ]
               }
            }
         }
      }
   ],
   "outputs": {}
}
```

<a name="obfuscate"></a>

### <a name="hide-data-from-run-history-by-using-obfuscation"></a>Oculte los datos del historial de ejecución mediante ofuscación.

En muchos desencadenadores y acciones, esta configuración oculta las entradas, salidas o ambas del historial de ejecución de una aplicación lógica. Algunas [consideraciones que se deben tener en cuenta](#obfuscation-considerations) al usar esta configuración para proteger los datos son:

#### <a name="hide-inputs-and-outputs-in-the-designer"></a>Ocultar las entradas y salidas en el diseñador

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en Diseñador de aplicación lógica.

   ![Abrir la aplicación lógica en el Diseñador de aplicación lógica](./media/logic-apps-securing-a-logic-app/open-sample-logic-app-in-designer.png)

1. En el desencadenador o la acción donde quiera ocultar los datos confidenciales, seleccione los puntos suspensivos ( **...** ) y, a continuación, seleccione **Configuración**.

   ![Abrir configuración de desencadenador o acción](./media/logic-apps-securing-a-logic-app/open-action-trigger-settings.png)

1. Active **Entradas seguras**, **Salidas seguras** o ambas opciones. Cuando haya finalizado, seleccione **Listo**.

   ![Active "Entradas seguras" o "Salidas seguras".](./media/logic-apps-securing-a-logic-app/turn-on-secure-inputs-outputs.png)

   Ahora, la acción o el desencadenador muestra un icono de candado en la barra de título.

   ![La barra de título de acción o desencadenador muestra el icono de candado](./media/logic-apps-securing-a-logic-app/lock-icon-action-trigger-title-bar.png)

   Los tokens que representan las salidas protegidas de acciones anteriores también muestran los iconos de candado. Por ejemplo, al seleccionar un resultado de este tipo en la lista de contenido dinámico para usarlo en una acción, el token muestra un icono de candado.

   ![Seleccionar token para la salida protegida](./media/logic-apps-securing-a-logic-app/select-secured-token.png)

1. Una vez ejecutada la aplicación lógica, puede ver el historial de esa ejecución.

   1. En el panel **Información general** de la aplicación lógica, seleccione la ejecución que quiera ver.

   1. En el panel **Ejecución de aplicación lógica**, expanda las acciones que quiera revisar.

      Si decide ocultar las entradas y las salidas, esos valores aparecerán ahora ocultos.

      ![Entradas y salidas ocultas del historial de ejecución](./media/logic-apps-securing-a-logic-app/hidden-data-run-history.png)

<a name="secure-data-code-view"></a>

#### <a name="hide-inputs-and-outputs-in-code-view"></a>Ocultar las entradas y las salidas en la vista Código

En la definición de desencadenador o acción subyacente, agregue o actualice la matriz `runtimeConfiguration.secureData.properties` con uno de estos valores o con ambos:

* `"inputs"`: Protege las entradas en el historial de ejecución.
* `"outputs"`: Protege las salidas en el historial de ejecución.

Algunas [consideraciones que se deben tener en cuenta](#obfuscation-considerations) al usar esta configuración para proteger los datos son:

```json
"<trigger-or-action-name>": {
   "type": "<trigger-or-action-type>",
   "inputs": {
      <trigger-or-action-inputs>
   },
   "runtimeConfiguration": {
      "secureData": {
         "properties": [
            "inputs",
            "outputs"
         ]
      }
   },
   <other-attributes>
}
```

<a name="obfuscation-considerations"></a>

#### <a name="considerations-when-hiding-inputs-and-outputs"></a>Consideraciones a la hora de ocultar las entradas y salidas

* Cuando se ocultan las entradas o las salidas de un desencadenador o una acción, Logic Apps no envía los datos protegidos a Azure Log Analytics. Además, no se pueden agregar [propiedades con seguimiento](../logic-apps/monitor-logic-apps-log-analytics.md#extend-data) al desencadenador o acción para su supervisión.

* La [API de Logic Apps para controlar el historial del flujo de trabajo](https://docs.microsoft.com/rest/api/logic/) no devuelve salidas protegidas.

* Para ocultar las salidas de una acción que oculta las entradas u oculta explícitamente las salidas, active de forma manual las **Salidas seguras** en esa acción.

* Asegúrese de activar las **Entradas seguras** o las **Salidas seguras** en las acciones de nivel inferior en las que espera que el historial de ejecución oculte los datos.

  **Configuración de las salidas seguras**

  Al activar manualmente las **Salidas seguras** en un desencadenador o una acción, Logic Apps protege estas salidas en el historial de ejecución. Si una acción de nivel inferior usa explícitamente estas salidas protegidas como entradas, Logic Apps oculta las entradas de esta acción en el historial de ejecución, pero *no habilita* la opción de **Entradas seguras** de la acción.

  ![Salidas seguras como entradas y repercusión descendente en la mayoría de las acciones](./media/logic-apps-securing-a-logic-app/secure-outputs-as-inputs-flow.png)

  Las acciones de redacción, análisis JSON y respuesta solo tienen la opción **Entradas seguras**. Cuando está activada, esta opción también oculta las salidas de estas acciones. Si estas acciones usan explícitamente las salidas protegidas de nivel superior como entradas, Logic Apps ocultará las entradas y salidas de estas acciones, pero *no habilitará* la opción **Entradas seguras** de estas acciones. Si una acción de nivel inferior usa explícitamente las salidas ocultas de las acciones de redacción, análisis JSON o respuesta como entradas, Logic Apps *no oculta las entradas o salidas de la acción de nivel inferior*.

  ![Salidas seguras como entradas y repercusión descendente en acciones especificas](./media/logic-apps-securing-a-logic-app/secure-outputs-as-inputs-flow-special.png)

  **Opción Entradas seguras**

  Al activar manualmente las **Entradas seguras** en un desencadenador o una acción, Logic Apps protege estas entradas en el historial de ejecución. Si en una acción de nivel inferior se usan de forma explícita las salidas visibles de ese desencadenador o acción como entradas, Logic Apps oculta las entradas de esta acción de nivel inferior en el historial de ejecución, pero *no habilita* las **Entradas seguras** en esta acción y no oculta las salidas de la acción.

  ![Entradas seguras y repercusión descendente en la mayoría de las acciones](./media/logic-apps-securing-a-logic-app/secure-inputs-impact-on-downstream.png)

  Si las acciones de redacción, análisis JSON y respuesta usan explícitamente las salidas visibles del desencadenador o la acción que tiene las entradas protegidas, Logic Apps oculta las entradas y salidas de estas acciones, pero *no habilita* la opción **Entradas seguras** de la acción. Si una acción de nivel inferior usa explícitamente las salidas ocultas de las acciones de redacción, análisis JSON o respuesta como entradas, Logic Apps *no oculta las entradas o salidas de la acción de nivel inferior*.

  ![Entradas seguras y repercusión descendente en determinadas acciones](./media/logic-apps-securing-a-logic-app/secure-inputs-flow-special.png)

<a name="secure-action-parameters"></a>

## <a name="access-to-parameter-inputs"></a>Acceso a entradas de parámetros

Si implementa en entornos diferentes, considere la posibilidad de parametrizar los valores de la definición de flujo de trabajo que varían en función de esos entornos. De este modo, puede evitar los datos incluidos en el código mediante una [plantilla de Azure Resource Manager ](../azure-resource-manager/templates/overview.md) para implementar la aplicación lógica, proteger los datos confidenciales definiendo parámetros seguros y pasar dichos datos como entradas separadas mediante los [parámetros de la plantilla](../azure-resource-manager/templates/template-parameters.md) con un [archivo de parámetros](../azure-resource-manager/templates/parameter-files.md).

Por ejemplo, si autentica acciones HTTP con [Azure Active Directory OAuth](#azure-active-directory-oauth-authentication), puede definir y ocultar los parámetros que aceptan el identificador de cliente y el secreto de cliente utilizados para la autenticación. Para definir estos parámetros para la aplicación lógica, use la sección `parameters` dentro de la definición de flujo de trabajo de la aplicación lógica y Resource Manager para la implementación. Para ocultar los valores de parámetros que no quiera mostrar al editar la aplicación lógica o visualizar el historial de ejecución, defina los parámetros con el tipo `securestring` o `secureobject` y use la codificación necesaria. Los parámetros de este tipo no se devuelven con la definición del recurso y no son accesibles al visualizar el recurso después de la implementación. Para tener acceso a estos valores de parámetro durante el tiempo de ejecución, use la expresión `@parameters('<parameter-name>')` dentro de la definición del flujo de trabajo. Esta expresión solo se evalúa en tiempo de ejecución y se describe con el [lenguaje de definición de flujo de trabajo](../logic-apps/logic-apps-workflow-definition-language.md).

> [!NOTE]
> Si usa un parámetro en el encabezado o el cuerpo de una solicitud, ese parámetro se puede ver al visualizar el historial de ejecución de la aplicación lógica y la solicitud HTTP saliente. Asegúrese de definir también las directivas de acceso al contenido según corresponda. También puede usar la [ofuscación](#obfuscate) para ocultar entradas y salidas en el historial de ejecución. Los encabezados de autorización nunca son visibles a través de entradas o salidas. Por lo tanto, si aquí se usa un secreto, no se podrá recuperar.

Para más información, consulte las secciones siguientes en este tema:

* [Parámetros protegidos en las definiciones de flujo de trabajo](#secure-parameters-workflow)
* [Ocultamiento de los datos del historial de ejecución mediante ofuscación](#obfuscate)

Si [automatiza la implementación de aplicaciones lógicas con plantillas de Azure Resource Manager](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), puede definir [parámetros de plantilla](../azure-resource-manager/templates/template-parameters.md) protegidos, que se evalúan en la implementación, mediante los tipos `securestring` y `secureobject`. Para definir parámetros de plantilla, use la sección `parameters` de nivel superior de la plantilla, que es independiente y diferente de la sección `parameters` de la definición de flujo de trabajo. Para proporcionar valores para los parámetros de plantilla, use un [archivo de parámetros](../azure-resource-manager/templates/parameter-files.md) independiente.

Por ejemplo, si usa secretos, puede definir y usar parámetros de plantilla protegidos que recuperen dichos secretos de [Azure Key Vault](../key-vault/key-vault-overview.md) en la implementación. A continuación, puede hacer referencia a Key Vault y al secreto en el archivo de parámetros. Para más información, consulte los temas siguientes:

* [Uso de Azure Key Vault para pasar valores confidenciales durante la implementación](../azure-resource-manager/templates/key-vault-parameter.md)
* [Parámetros seguros en plantillas de Azure Resource Manager](#secure-parameters-deployment-template) más adelante en este tema

<a name="secure-parameters-workflow"></a>

### <a name="secure-parameters-in-workflow-definitions"></a>Parámetros protegidos en las definiciones de flujo de trabajo

Para proteger la información confidencial en la definición del flujo de trabajo de aplicación lógica, use parámetros seguros, de forma que esta información no esté visible después de guardar la aplicación lógica. Por ejemplo, supongamos que tiene una acción HTTP que requiere autenticación básica que usa un nombre de usuario y una contraseña. En la definición del flujo, la sección `parameters` define los parámetros `basicAuthPasswordParam` y `basicAuthUsernameParam` mediante el tipo `securestring`. A continuación, la definición de la acción hace referencia a estos parámetros en la sección `authentication`.

```json
"definition": {
   "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
   "actions": {
      "HTTP": {
         "type": "Http",
         "inputs": {
            "method": "GET",
            "uri": "https://www.microsoft.com",
            "authentication": {
               "type": "Basic",
               "username": "@parameters('basicAuthUsernameParam')",
               "password": "@parameters('basicAuthPasswordParam')"
            }
         },
         "runAfter": {}
      }
   },
   "parameters": {
      "basicAuthPasswordParam": {
         "type": "securestring"
      },
      "basicAuthUsernameParam": {
         "type": "securestring"
      }
   },
   "triggers": {
      "manual": {
         "type": "Request",
         "kind": "Http",
         "inputs": {
            "schema": {}
         }
      }
   },
   "contentVersion": "1.0.0.0",
   "outputs": {}
}
```

<a name="secure-parameters-deployment-template"></a>

### <a name="secure-parameters-in-azure-resource-manager-templates"></a>Parámetros seguros en plantillas de Azure Resource Manager

Una [plantilla de Resource Manager](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md) para una aplicación lógica tiene varias secciones `parameters`. Para proteger las contraseñas, las claves, los secretos y otros datos confidenciales, defina parámetros seguros en el nivel de plantilla y de definición de flujo de trabajo mediante el tipo `securestring` o `secureobject`. Después, puede almacenar estos valores en [Azure Key Vault](../key-vault/key-vault-overview.md) y usar el [archivo de parámetros](../azure-resource-manager/templates/parameter-files.md) para hacer referencia al almacén de claves y al secreto. A continuación, la plantilla recupera esa información en la implementación. Para obtener más información, consulte [Uso de Azure Key Vault para pasar valores confidenciales durante la implementación](../azure-resource-manager/templates/key-vault-parameter.md).

Aquí tiene más información sobre estas secciones `parameters`:

* En el nivel superior de la plantilla, una sección `parameters` define los parámetros de los valores que utiliza la plantilla durante la *implementación*. Por ejemplo, estos valores pueden incluir cadenas de conexión para un entorno de implementación concreto. Después, puede almacenar estos valores en un [archivo de parámetros](../azure-resource-manager/templates/parameter-files.md), lo que facilita la modificación de estos valores.

* Dentro de la definición de recursos de la aplicación lógica, pero fuera de la definición del flujo de trabajo, una sección `parameters` especifica los valores de los parámetros de la definición del flujo de trabajo. En esta sección, puede asignar estos valores mediante expresiones de plantilla que hagan referencia a los parámetros de la plantilla. Estas expresiones se evalúan en la implementación.

* Dentro de la definición del flujo de trabajo, una sección `parameters` define los parámetros que usa la aplicación lógica en tiempo de ejecución. Después, puede hacer referencia a estos parámetros en el flujo de trabajo de la aplicación lógica mediante expresiones de definición de flujo de trabajo, que se evalúan en tiempo de ejecución.

Esta plantilla de ejemplo tiene varias definiciones de parámetros seguros que usan el tipo `securestring`:

| Nombre de parámetro | Descripción |
|----------------|-------------|
| `TemplatePasswordParam` | Parámetro de plantilla que acepta una contraseña que se pasa, a continuación, al parámetro `basicAuthPasswordParam` de la definición del flujo de trabajo. |
| `TemplateUsernameParam` | Parámetro de plantilla que acepta un nombre de usuario que se pasa a continuación al parámetro `basicAuthUserNameParam` de la definición del flujo de trabajo |
| `basicAuthPasswordParam` | Parámetro de definición de flujo de trabajo que acepta la contraseña para la autenticación básica en una acción HTTP |
| `basicAuthUserNameParam` | Parámetro de definición de flujo de trabajo que acepta el nombre de usuario para la autenticación básica en una acción HTTP |
|||

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {
      "LogicAppName": {
         "type": "string",
         "minLength": 1,
         "maxLength": 80,
         "metadata": {
            "description": "Name of the Logic App."
         }
      },
      "TemplatePasswordParam": {
         "type": "securestring"
      },
      "TemplateUsernameParam": {
         "type": "securestring"
      },
      "LogicAppLocation": {
         "type": "string",
         "defaultValue": "[resourceGroup().location]",
         "allowedValues": [
            "[resourceGroup().location]",
            "eastasia",
            "southeastasia",
            "centralus",
            "eastus",
            "eastus2",
            "westus",
            "northcentralus",
            "southcentralus",
            "northeurope",
            "westeurope",
            "japanwest",
            "japaneast",
            "brazilsouth",
            "australiaeast",
            "australiasoutheast",
            "southindia",
            "centralindia",
            "westindia",
            "canadacentral",
            "canadaeast",
            "uksouth",
            "ukwest",
            "westcentralus",
            "westus2"
         ],
         "metadata": {
            "description": "Location of the Logic App."
         }
      }
   },
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {
               "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-0601/workflowdefinition.json#",
               "actions": {
                  "HTTP": {
                     "type": "Http",
                     "inputs": {
                        "method": "GET",
                        "uri": "https://www.microsoft.com",
                        "authentication": {
                           "type": "Basic",
                           "username": "@parameters('basicAuthUsernameParam')",
                           "password": "@parameters('basicAuthPasswordParam')"
                        }
                     },
                  "runAfter": {}
                  }
               },
               "parameters": {
                  "basicAuthPasswordParam": {
                     "type": "securestring"
                  },
                  "basicAuthUsernameParam": {
                     "type": "securestring"
                  }
               },
               "triggers": {
                  "manual": {
                     "type": "Request",
                     "kind": "Http",
                     "inputs": {
                        "schema": {}
                     }
                  }
               },
               "contentVersion": "1.0.0.0",
               "outputs": {}
            },
            "parameters": {
               "basicAuthPasswordParam": {
                  "value": "[parameters('TemplatePasswordParam')]"
               },
               "basicAuthUsernameParam": {
                  "value": "[parameters('TemplateUsernameParam')]"
               }
            }
         }
      }
   ],
   "outputs": {}
}
```

<a name="secure-requests"></a>

## <a name="access-to-services-and-systems-called-from-logic-apps"></a>Acceso a servicios y sistemas invocados desde aplicaciones lógicas

Se pueden proteger los puntos de conexión que reciben llamadas o solicitudes de la aplicación lógica de varias maneras:

* Incorporación de la autenticación en las solicitudes salientes.

  Cuando trabaja con un desencadenador o acción basados en HTTP que realiza llamadas salientes, como HTTP, HTTP + Swagger o webhook, puede agregar autenticación a la solicitud enviada por la aplicación lógica. Por ejemplo, puede usar estos tipos de autenticación:

  * [Autenticación básica](#basic-authentication)

  * [Autenticación de certificados de clientes](#client-certificate-authentication)

  * [Autenticación de Active Directory OAuth](#azure-active-directory-oauth-authentication)

  * [Autenticación de identidad administrada](#managed-identity-authentication)
  
  Para obtener más información, consulte [Incorporación de autenticación a las llamadas salientes](#add-authentication-outbound) más adelante en este tema.

* Restricción del acceso desde direcciones IP de aplicación lógica.

  Todas las llamadas a los puntos de conexión desde aplicaciones lógicas proceden de direcciones IP designadas específicas que se basan en las regiones de las aplicaciones lógicas. Puede agregar un filtrado que solo acepta solicitudes provenientes de esas direcciones IP. Para obtener esas direcciones IP, consulte [Límites y configuración de Azure Logic Apps](logic-apps-limits-and-config.md#configuration).

* Mejore la seguridad de las conexiones a sistemas locales.

  Azure Logic Apps ofrece integración con estos servicios para permitir una comunicación local más segura y confiable.

  * Puerta de enlace de datos local

    Muchos conectores administrados de Azure Logic Apps proporcionan conexiones seguras a sistemas locales, como el sistema de archivos, SQL, SharePoint y DB2. La puerta de enlace envía datos desde orígenes locales en canales cifrados hasta Azure Service Bus. Todo el tráfico se origina como tráfico de salida seguro desde el agente de la puerta de enlace. Obtenga información sobre [el funcionamiento de la puerta de enlace de datos local](logic-apps-gateway-install.md#gateway-cloud-service).

  * Conexión mediante Azure API Management

    [Azure API Management](../api-management/api-management-key-concepts.md) ofrece opciones de conexión local, como la integración de ExpressRoute y una red privada virtual sitio a sitio para un proxy seguro y comunicación con sistemas locales. Desde el flujo de trabajo de la aplicación lógica del Diseñador de aplicación lógica, puede seleccionar una API expuesta por API Management, lo que proporciona un acceso rápido a los sistemas locales.

<a name="add-authentication-outbound"></a>

## <a name="add-authentication-to-outbound-calls"></a>Incorporación de la autenticación en las llamadas salientes

Los extremos HTTP y HTTPS admiten varios tipos de autenticación. Según el desencadenador o la acción que se usa para realizar llamadas o solicitudes salientes que tienen acceso a estos extremos, puede seleccionar entre distintos rangos de tipos de autenticación. Para asegurarse de que protege la información confidencial que controla la aplicación lógica, use los parámetros seguros y codifique los datos según sea necesario. Para obtener más información acerca de cómo usar y proteger los parámetros, consulte [Acceso a las entradas de parámetro](#secure-action-parameters).

> [!NOTE]
> En el Diseñador de aplicación lógica, la propiedad **Autenticación** puede estar oculta en algunos desencadenadores y acciones en los que puede especificar el tipo de autenticación. Para que la propiedad se muestre en estos casos, en el desencadenador o la acción, abra la lista **Agregar nuevo parámetro** y seleccione **Autenticación**. Para obtener más información, consulte [Autenticación del acceso con la identidad administrada](../logic-apps/create-managed-service-identity.md#authenticate-access-with-identity).

| Tipo de autenticación | Compatible con |
|---------------------|--------------|
| [Basic](#basic-authentication) | Azure API Management, Azure App Services, HTTP, HTTP y Swagger, webhook HTTP |
| [Certificado de cliente](#client-certificate-authentication) | Azure API Management, Azure App Services, HTTP, HTTP y Swagger, webhook HTTP |
| [Active Directory OAuth](#azure-active-directory-oauth-authentication) | Azure API Management, Azure App Services, Azure Functions, HTTP, HTTP + Swagger, webhook HTTP |
| [Sin formato](#raw-authentication) | Azure API Management, Azure App Services, Azure Functions, HTTP, HTTP + Swagger, webhook HTTP |
| [Identidad administrada](#managed-identity-authentication) | Azure API Management, Azure App Services, Azure Functions, HTTP, HTTP + Swagger, webhook HTTP |
|||

<a name="basic-authentication"></a>

### <a name="basic-authentication"></a>Autenticación básica

Si la opción [Básica](../active-directory-b2c/secure-rest-api.md) está disponible, especifique estos valores de propiedad:

| Propiedad (diseñador) | Propiedad (JSON) | Obligatorio | Value | Descripción |
|---------------------|-----------------|----------|-------|-------------|
| **Autenticación** | `type` | Sí | Básica | Tipo de autenticación que se debe usar. |
| **Nombre de usuario** | `username` | Sí | <*nombre-de-usuario*>| Nombre de usuario para autenticar el acceso al extremo del servicio de destino. |
| **Contraseña** | `password` | Sí | <*contraseña*> | Contraseña para autenticar el acceso al extremo del servicio de destino. |
||||||

Al usar [parámetros protegidos](#secure-action-parameters) para controlar y proteger la información confidencial, por ejemplo, en una [plantilla de Azure Resource Manager para automatizar la implementación](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), puede usar expresiones para tener acceso a estos valores de parámetro en tiempo de ejecución. Esta definición de acción HTTP de ejemplo especifica el `type` de autenticación como `Basic` y usa la [función parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) para obtener los valores de parámetro:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "@parameters('endpointUrlParam')",
      "authentication": {
         "type": "Basic",
         "username": "@parameters('userNameParam')",
         "password": "@parameters('passwordParam')"
      }
  },
  "runAfter": {}
}
```

<a name="client-certificate-authentication"></a>

### <a name="client-certificate-authentication"></a>Autenticación de certificados de cliente

Si la opción [Certificado de cliente](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md) está disponible, especifique estos valores de propiedad:

| Propiedad (diseñador) | Propiedad (JSON) | Obligatorio | Value | Descripción |
|---------------------|-----------------|----------|-------|-------------|
| **Autenticación** | `type` | Sí | **Certificado de cliente** <br>or <br>`ClientCertificate` | Tipo de autenticación que se usará para los certificados de cliente de TLS/SSL. Aunque se admiten los certificados autofirmados, no se admiten para TLS/SSL. |
| **Pfx** | `pfx` | Sí | <*contenido-archivo-pfx-codificado*> | El contenido codificado en base 64 del archivo de intercambio de información personal (PFX) <p><p>Para convertir el archivo PFX en formato codificado en Base64, puede usar PowerShell siguiendo estos pasos: <p>1. Guarde el contenido del certificado en una variable: <p>   `$pfx_cert = get-content 'c:\certificate.pfx' -Encoding Byte` <p>2. Convierta el contenido del certificado mediante la función `ToBase64String()` y guarde el contenido en un archivo de texto: <p>   `[System.Convert]::ToBase64String($pfx_cert) | Out-File 'pfx-encoded-bytes.txt'` |
| **Contraseña** | `password`| No | <*contraseña-archivo-pfx*> | La contraseña para acceder al archivo PFX |
|||||

Al usar [parámetros protegidos](#secure-action-parameters) para controlar y proteger la información confidencial, por ejemplo, en una [plantilla de Azure Resource Manager para automatizar la implementación](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), puede usar expresiones para tener acceso a estos valores de parámetro en tiempo de ejecución. Esta definición de acción HTTP de ejemplo especifica el `type` de autenticación como `ClientCertificate` y usa la [función parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) para obtener los valores de parámetro:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "@parameters('endpointUrlParam')",
      "authentication": {
         "type": "ClientCertificate",
         "pfx": "@parameters('pfxParam')",
         "password": "@parameters('passwordParam')"
      }
   },
   "runAfter": {}
}
```

Para obtener más información acerca de cómo proteger los servicios mediante la autenticación de certificados de cliente, consulte estos temas:

* [Cómo mejorar la seguridad de las API mediante la autenticación de certificados de cliente en Azure API Management](../api-management/api-management-howto-mutual-certificates-for-clients.md)
* [Cómo mejorar la seguridad de los servicios back-end con la autenticación de certificados de cliente en Azure API Management](../api-management/api-management-howto-mutual-certificates.md)
* [Cómo mejorar la seguridad de los servicios RESTful mediante certificados de cliente](../active-directory-b2c/secure-rest-api.md)
* [Credenciales de certificado para la autenticación de aplicaciones](../active-directory/develop/active-directory-certificate-credentials.md)
* [Uso de un certificado TLS/SSL en el código de Azure App Service](../app-service/configure-ssl-certificate-in-code.md)

<a name="azure-active-directory-oauth-authentication"></a>

### <a name="azure-active-directory-oauth-authentication"></a>Autenticación de Azure Active Directory OAuth

Si la opción [Active Directory OAuth](../active-directory/develop/about-microsoft-identity-platform.md) está disponible, especifique estos valores de propiedad:

| Propiedad (diseñador) | Propiedad (JSON) | Obligatorio | Value | Descripción |
|---------------------|-----------------|----------|-------|-------------|
| **Autenticación** | `type` | Sí | **Active Directory OAuth** <br>or <br>`ActiveDirectoryOAuth` | Tipo de autenticación que se debe usar. Actualmente, Logic Apps sigue el [protocolo OAuth 2.0](../active-directory/develop/v2-overview.md). |
| **Autoridad** | `authority` | No | <*URL-for-authority-token-issuer*> | La dirección URL de la autoridad que proporciona el token de autenticación. De manera predeterminada, este valor es `https://login.windows.net`. |
| **Inquilino** | `tenant` | Sí | <*tenant-ID*> | El identificador del inquilino de Azure AD |
| **Audiencia** | `audience` | Sí | <*resource-to-authorize*> | Recurso que quiere usar para la autorización; por ejemplo, `https://management.core.windows.net/` |
| **Id. de cliente** | `clientId` | Sí | <*client-ID*> | El identificador de cliente para la aplicación que solicita autorización |
| **Tipo de credencial** | `credentialType` | Sí | Certificado <br>or <br>Secreto | El tipo de credencial que el cliente usa para solicitar autorización. Esta propiedad y el valor no aparecen en la definición subyacente de la aplicación lógica, pero determina las propiedades que se muestran para el tipo de credencial seleccionado. |
| **Secreto** | `secret` | Sí, pero solo para el tipo de credencial de "Secreto". | <*secreto-de-cliente*> | Secreto de cliente para solicitar la autorización |
| **Pfx** | `pfx` | Sí, pero solo para el tipo de credencial de "Certificado". | <*contenido-archivo-pfx-codificado*> | El contenido codificado en base 64 del archivo de intercambio de información personal (PFX) |
| **Contraseña** | `password` | Sí, pero solo para el tipo de credencial de "Certificado". | <*contraseña-archivo-pfx*> | La contraseña para acceder al archivo PFX |
|||||

Al usar [parámetros protegidos](#secure-action-parameters) para controlar y proteger la información confidencial, por ejemplo, en una [plantilla de Azure Resource Manager para automatizar la implementación](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), puede usar expresiones para tener acceso a estos valores de parámetro en tiempo de ejecución. Esta definición de acción HTTP de ejemplo especifica el `type` de autenticación como `ActiveDirectoryOAuth`, el tipo de credencial `Secret` y usa la [función parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) para obtener los valores de parámetro:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "@parameters('endpointUrlParam')",
      "authentication": {
         "type": "ActiveDirectoryOAuth",
         "tenant": "@parameters('tenantIdParam')",
         "audience": "https://management.core.windows.net/",
         "clientId": "@parameters('clientIdParam')",
         "credentialType": "Secret",
         "secret": "@parameters('secretParam')"
     }
   },
   "runAfter": {}
}
```

<a name="raw-authentication"></a>

### <a name="raw-authentication"></a>Autenticación sin formato

Si la opción **Raw** está disponible, puede usar este tipo de autenticación cuando tenga que usar [esquemas de autenticación](https://iana.org/assignments/http-authschemes/http-authschemes.xhtml) que no siguen el [protocolo OAuth 2.0](https://oauth.net/2/). Con este tipo, se crea manualmente el valor del encabezado de autorización que se envía con la solicitud saliente y se especifica ese valor de encabezado en el desencadenador o la acción.

Por ejemplo, el siguiente es un encabezado de ejemplo para una solicitud HTTPS que sigue el [protocolo OAuth 1.0](https://tools.ietf.org/html/rfc5849):

```text
Authorization: OAuth realm="Photos",
   oauth_consumer_key="dpf43f3p2l4k3l03",
   oauth_signature_method="HMAC-SHA1",
   oauth_timestamp="137131200",
   oauth_nonce="wIjqoS",
   oauth_callback="http%3A%2F%2Fprinter.example.com%2Fready",
   oauth_signature="74KNZJeDHnMBp0EMJ9ZHt%2FXKycU%3D"
```

En el desencadenador o la acción que admite la autenticación sin formato, especifique estos valores de propiedad:

| Propiedad (diseñador) | Propiedad (JSON) | Obligatorio | Value | Descripción |
|---------------------|-----------------|----------|-------|-------------|
| **Autenticación** | `type` | Sí | Raw | Tipo de autenticación que se debe usar. |
| **Valor** | `value` | Sí | <*valor de encabezado de autorización*> | Valor del encabezado de autorización que se va a usar para la autenticación. |
||||||

Al usar [parámetros protegidos](#secure-action-parameters) para controlar y proteger la información confidencial, por ejemplo, en una [plantilla de Azure Resource Manager para automatizar la implementación](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), puede usar expresiones para tener acceso a estos valores de parámetro en tiempo de ejecución. Esta definición de acción HTTP de ejemplo especifica el `type` de autenticación como `Raw` y usa la [función parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) para obtener los valores de parámetro:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "@parameters('endpointUrlParam')",
      "authentication": {
         "type": "Raw",
         "value": "@parameters('authHeaderParam')"
      }
   },
   "runAfter": {}
}
```

<a name="managed-identity-authentication"></a>

### <a name="managed-identity-authentication"></a>Autenticación de identidad administrada

Si la opción [identidad administrada](../active-directory/managed-identities-azure-resources/overview.md) está disponible, la aplicación lógica puede usar la identidad asignada por el sistema o una identidad *única* creada manualmente y asignada por el usuario para autenticar el acceso a los recursos de otros inquilinos de Azure Active Directory (Azure AD) sin iniciar sesión. Azure administra esta identidad y le ayuda a proteger las credenciales porque, de esta forma, no tiene que proporcionar secretos o cambiarlos. Obtenga más información sobre [Servicios de Azure que admiten las identidades administradas para la autenticación de Azure AD](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication).

1. Para que la aplicación lógica pueda usar una identidad administrada, siga los pasos descritos en [Autenticación de acceso a los recursos de Azure con identidades administradas en Azure Logic Apps](../logic-apps/create-managed-service-identity.md). En estos pasos se habilita la identidad administrada en la aplicación lógica y se configura el acceso de dicha identidad al recurso de Azure de destino.

1. Para que una función de Azure pueda usar una identidad administrada, primero [habilite la autenticación de Azure Functions](../logic-apps/logic-apps-azure-functions.md#enable-authentication-for-azure-functions).

1. En el desencadenador o la acción donde quiere usar la identidad administrada, especifique estos valores de propiedad:

   | Propiedad (diseñador) | Propiedad (JSON) | Obligatorio | Value | Descripción |
   |---------------------|-----------------|----------|-------|-------------|
   | **Autenticación** | `type` | Sí | **Identidad administrada** <br>or <br>`ManagedServiceIdentity` | Tipo de autenticación que se debe usar. |
   | **Identidad administrada** | `identity` | Sí | * **Identidad administrada asignada por el sistema** <br>or <br>`SystemAssigned` <p><p>* <*nombre de identidad asignado por el usuario*> | Identidad administrada que se debe usar. |
   | **Audiencia** | `audience` | Sí | <*Id-recurso-destino*> | El Id. de recurso para el recurso de destino al que quiere obtener acceso. <p>Por ejemplo, `https://storage.azure.com/` hace que los tokens de acceso para la autenticación sean válidos para todas las cuentas de almacenamiento. Sin embargo, también puede especificar una dirección URL de servicio raíz, como `https://fabrikamstorageaccount.blob.core.windows.net` para una cuenta de almacenamiento específica. <p>**Nota**: La propiedad **Audiencia** puede estar oculta en algunos desencadenadores o acciones. Para que la propiedad sea visible, en el desencadenador o la acción, abra la lista **Agregar nuevo parámetro** y seleccione **Público**. <p><p>**Importante**: Asegúrese de que el identificador de este recurso de destino *coincide exactamente* con el valor esperado en Azure AD, incluida toda barra diagonal necesaria al final. Por lo tanto, el Id. de recurso de `https://storage.azure.com/` para todas las cuentas de Azure Blob Storage requiere una barra diagonal final. Sin embargo, el Id. de recurso de una cuenta de almacenamiento específica no requiere una barra diagonal final. Para buscar estos Id. de recursos, consulte [Servicios de Azure que admiten la autenticación de Azure AD](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication). |
   |||||

   Al usar [parámetros protegidos](#secure-action-parameters) para controlar y proteger la información confidencial, por ejemplo, en una [plantilla de Azure Resource Manager para automatizar la implementación](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), puede usar expresiones para tener acceso a estos valores de parámetro en tiempo de ejecución. Esta definición de acción HTTP de ejemplo especifica el `type` de autenticación como `ManagedServiceIdentity` y usa la [función parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) para obtener los valores de parámetro:

   ```json
   "HTTP": {
      "type": "Http",
      "inputs": {
         "method": "GET",
         "uri": "@parameters('endpointUrlParam')",
         "authentication": {
            "type": "ManagedServiceIdentity",
            "identity": "SystemAssigned",
            "audience": "https://management.azure.com/"
         },
      },
      "runAfter": {}
   }
   ```

## <a name="next-steps"></a>Pasos siguientes

* [Automatización de la implementación para Azure Logic Apps](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)  
* [Supervisión de aplicaciones lógicas](../logic-apps/monitor-logic-apps-log-analytics.md)  
* [Diagnóstico de errores y problemas de las aplicaciones lógicas](../logic-apps/logic-apps-diagnosing-failures.md)  
* [Automatización de la implementación de aplicaciones lógicas](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)
