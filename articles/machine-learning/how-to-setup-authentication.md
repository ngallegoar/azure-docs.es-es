---
title: Configuración de la autenticación
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo instalar y configurar la autenticación para varios recursos y flujos de trabajo en Azure Machine Learning. Hay varias maneras de configurar y usar la autenticación dentro del servicio, que van desde la autenticación simple basada en la interfaz de usuario con fines de desarrollo o prueba, hasta la autenticación de entidad de servicio de Azure Active Directory completa.
services: machine-learning
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 12/17/2019
ms.custom: has-adal-ref
ms.openlocfilehash: 6b2cfa85ea412a5ef8bda47a7ff6e99970ba6b0e
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2020
ms.locfileid: "82611847"
---
# <a name="set-up-authentication-for-azure-machine-learning-resources-and-workflows"></a>Configuración de la autenticación para recursos y flujos de trabajo de Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este artículo obtendrá información sobre cómo instalar y configurar la autenticación para varios recursos y flujos de trabajo en Azure Machine Learning. Hay varias maneras de autenticarse en el servicio, que van desde la autenticación simple basada en la interfaz de usuario con fines de desarrollo o prueba, hasta la autenticación de entidad de servicio de Azure Active Directory completa. En este artículo también se explican las diferencias en la forma en que funciona la autenticación de servicio web, así como la forma de autenticarse en la API REST de Azure Machine Learning.

Estas instrucciones paso a paso muestran cómo completar las tareas siguientes:

* Usar la autenticación de interfaz de usuario interactiva para pruebas y desarrollo
* Configurar la autenticación de entidad de servicio
* Autenticación en el área de trabajo
* Obtener tokens de portador de OAuth 2.0 para la API REST de Azure Machine Learning
* Comprender la autenticación de servicio web

Consulte el [artículo de concepto](concept-enterprise-security.md) para información general sobre la seguridad y la autenticación dentro de Azure Machine Learning.

## <a name="prerequisites"></a>Prerequisites

* Cree un [área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).
* [Configure un entorno de desarrollo](how-to-configure-environment.md) para instalar el SDK de Azure Machine Learning, o bien use una [máquina virtual de Azure Machine Learning Notebook](concept-azure-machine-learning-architecture.md#compute-instance) con el SDK ya instalado.

## <a name="interactive-authentication"></a>Autenticación interactiva

La mayoría de los ejemplos de la documentación de este servicio usan la autenticación interactiva en instancias de Jupyter Notebook como método sencillo para pruebas y demostraciones. Se trata de una manera ligera de probar lo que está creando. Hay dos llamadas de función que muestran automáticamente un flujo de autenticación basada en la interfaz de usuario.

La llamada a la función `from_config()` emitirá el mensaje.

```python
from azureml.core import Workspace
ws = Workspace.from_config()
```

La función `from_config()` busca un archivo JSON que contenga la información de conexión del área de trabajo. También puede especificar los detalles de conexión explícitamente mediante el constructor `Workspace`, que también solicitará la autenticación interactiva. Ambas llamadas son equivalentes.

```python
ws = Workspace(subscription_id="your-sub-id",
               resource_group="your-resource-group-id",
               workspace_name="your-workspace-name"
              )
```

Si tiene acceso a varios inquilinos, es posible que tenga que importar la clase y definir explícitamente el inquilino que tiene como destino. La llamada al constructor de `InteractiveLoginAuthentication` también le pedirá que inicie sesión de forma similar a las llamadas anteriores.

```python
from azureml.core.authentication import InteractiveLoginAuthentication
interactive_auth = InteractiveLoginAuthentication(tenant_id="your-tenant-id")
```

Aunque resulta útil para las pruebas y el aprendizaje, la autenticación interactiva no le ayudará a crear flujos de trabajo automatizados o sin periféricos. La configuración de la autenticación de entidad de servicio es el mejor enfoque para los procesos automatizados que usan el SDK.

## <a name="set-up-service-principal-authentication"></a>Configurar la autenticación de entidad de servicio

Este proceso es necesario para habilitar la autenticación que está desacoplada desde un inicio de sesión de usuario específico, lo que le permite autenticarse en el SDK de Python para Azure Machine Learning en flujos de trabajo automatizados. La autenticación de entidad de servicio también le permitirá [autenticarse en la API REST](#azure-machine-learning-rest-api-auth).

Para configurar la autenticación de la entidad de servicio, primero debe crear un registro de aplicación en Azure Active Directory y luego conceder el acceso basado en roles de la aplicación a su área de trabajo de ML. La forma más sencilla de completar esta configuración es a través de [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) en Azure Portal. Después de iniciar sesión en el portal, haga clic en el icono de `>_` en la parte superior derecha de la página cerca de su nombre para abrir el shell.

Si no ha usado Cloud Shell antes en la cuenta de Azure, tendrá que crear un recurso de cuenta de almacenamiento para almacenar todos los archivos que están escritos. En general, esta cuenta de almacenamiento incurrirá en un costo mensual insignificante. Además, instale la extensión de aprendizaje automático si no la ha usado previamente con el siguiente comando.

```azurecli-interactive
az extension add -n azure-cli-ml
```

> [!NOTE]
> Debe ser un administrador de la suscripción para realizar los pasos siguientes.

Después, ejecute el comando siguiente para crear la entidad de servicio. Asígnele un nombre, en este caso **ml-auth**.

```azurecli-interactive
az ad sp create-for-rbac --sdk-auth --name ml-auth
```

La salida será un código JSON similar al siguiente. Tome nota de los campos `clientId`, `clientSecret` y `tenantId`, ya que los necesitará para otros pasos de este artículo.

```json
{
    "clientId": "your-client-id",
    "clientSecret": "your-client-secret",
    "subscriptionId": "your-sub-id",
    "tenantId": "your-tenant-id",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com",
    "activeDirectoryGraphResourceId": "https://graph.windows.net",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:5555",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net"
}
```

A continuación, ejecute el siguiente comando para obtener los detalles de la entidad de servicio que acaba de crear, con el valor `clientId` anterior como entrada para el parámetro `--id`.

```azurecli-interactive
az ad sp show --id your-client-id
```

Lo siguiente es un ejemplo simplificado de la salida JSON del comando. Tome nota del campo `objectId`, ya que necesitará su valor para el paso siguiente.

```json
{
    "accountEnabled": "True",
    "addIns": [],
    "appDisplayName": "ml-auth",
    ...
    ...
    ...
    "objectId": "your-sp-object-id",
    "objectType": "ServicePrincipal"
}
```

A continuación, use el siguiente comando para asignar el acceso de la entidad de servicio al área de trabajo de aprendizaje automático. Necesitará el nombre del área de trabajo y el nombre de su grupo de recursos para los parámetros `-w` y `-g`, respectivamente. Para el parámetro `--user`, use el valor `objectId` del paso anterior. El parámetro `--role` le permite establecer el rol de acceso para la entidad de servicio y, en general, usará **owner** o **contributor**. Ambos tienen acceso de escritura a recursos existentes como clústeres de proceso y almacenes de datos, pero solo **owner** pueden aprovisionar estos recursos.

```azurecli-interactive
az ml workspace share -w your-workspace-name -g your-resource-group-name --user your-sp-object-id --role owner
```

Esta llamada no genera ninguna salida, pero ahora tiene la autenticación de entidad de servicio configurada para el área de trabajo.

## <a name="authenticate-to-your-workspace"></a>Autenticación en el área de trabajo

Ahora que tiene habilitada la autenticación de entidad de servicio, puede autenticarse en el área de trabajo en el SDK sin iniciar sesión físicamente como un usuario. Utilice el constructor de clase `ServicePrincipalAuthentication` y use los valores obtenidos en los pasos anteriores como parámetros. El parámetro `tenant_id` se asigna al campo `tenantId` anterior, `service_principal_id` se asigna a `clientId`y `service_principal_password` se asigna a `clientSecret`.

```python
from azureml.core.authentication import ServicePrincipalAuthentication

sp = ServicePrincipalAuthentication(tenant_id="your-tenant-id", # tenantID
                                    service_principal_id="your-client-id", # clientId
                                    service_principal_password="your-client-secret") # clientSecret
```

La variable `sp` ahora contiene un objeto de autenticación que se usa directamente en el SDK. En general, se recomienda almacenar los identificadores y secretos usados anteriormente en las variables de entorno, tal como se muestra en el código siguiente.

```python
import os

sp = ServicePrincipalAuthentication(tenant_id=os.environ['AML_TENANT_ID'],
                                    service_principal_id=os.environ['AML_PRINCIPAL_ID'],
                                    service_principal_password=os.environ['AML_PRINCIPAL_PASS'])
```

En el caso de los flujos de trabajo automatizados que se ejecutan en Python y usan el SDK principalmente, puede usar este objeto tal cual en la mayoría de los casos para la autenticación. El código siguiente se autentica en el área de trabajo mediante el objeto de autenticación que acaba de crear.

```python
from azureml.core import Workspace

ws = Workspace.get(name="ml-example",
                   auth=sp,
                   subscription_id="your-sub-id")
ws.get_details()
```

## <a name="azure-machine-learning-rest-api-auth"></a>Autenticación de la API REST de Azure Machine Learning

La entidad de servicio creada en los pasos anteriores también se puede usar para autenticar en la [API REST](https://docs.microsoft.com/rest/api/azureml/) de Azure Machine Learning. Use el [flujo de concesión de credenciales de cliente](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow) de Azure Active Directory, que permiten llamadas de servicio a servicio para la autenticación sin periféricos en flujos de trabajo automatizados. Los ejemplos se implementan con la [biblioteca de ADAL](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries) tanto en Python como en Node.js, pero también puede usar cualquier biblioteca de código abierto que admita OpenID Connect 1.0.

> [!NOTE]
> MSAL.js es una biblioteca más reciente que ADAL, pero no puede realizar la autenticación de servicio a servicio mediante las credenciales de cliente con MSAL.js, ya que es principalmente una biblioteca del lado cliente diseñada para la autenticación interactiva o de interfaz de usuario asociada a un usuario específico. Se recomienda usar ADAL como se muestra a continuación para crear flujos de trabajo automatizados con la API REST.

### <a name="nodejs"></a>Node.js

Siga estos pasos para generar un token de autenticación mediante Node.js. En su entorno, ejecute `npm install adal-node`. A continuación, use su `tenantId`, `clientId` y `clientSecret` de la entidad de servicio que creó en los pasos anteriores como valores para las variables coincidentes en el siguiente script.

```javascript
const adal = require('adal-node').AuthenticationContext;

const authorityHostUrl = 'https://login.microsoftonline.com/';
const tenantId = 'your-tenant-id';
const authorityUrl = authorityHostUrl + tenantId;
const clientId = 'your-client-id';
const clientSecret = 'your-client-secret';
const resource = 'https://management.azure.com/';

const context = new adal(authorityUrl);

context.acquireTokenWithClientCredentials(
  resource,
  clientId,
  clientSecret,
  (err, tokenResponse) => {
    if (err) {
      console.log(`Token generation failed due to ${err}`);
    } else {
      console.dir(tokenResponse, { depth: null, colors: true });
    }
  }
);
```

La variable `tokenResponse` es un objeto que incluye el token y los metadatos asociados, como la fecha de expiración. Los tokens son válidos durante una hora y se pueden actualizar ejecutando la misma llamada de nuevo para recuperar un nuevo token. Lo siguiente es una respuesta de ejemplo.

```javascript
{
    tokenType: 'Bearer',
    expiresIn: 3599,
    expiresOn: 2019-12-17T19:15:56.326Z,
    resource: 'https://management.azure.com/',
    accessToken: "random-oauth-token",
    isMRRT: true,
    _clientId: 'your-client-id',
    _authority: 'https://login.microsoftonline.com/your-tenant-id'
}
```

Use la propiedad `accessToken` para capturar el token de autenticación. Consulte la [documentación de la API REST](https://github.com/microsoft/MLOps/tree/master/examples/AzureML-REST-API) para ejemplos sobre cómo usar el token para realizar llamadas API.

### <a name="python"></a>Python

Siga estos pasos para generar un token de autenticación mediante Python. En su entorno, ejecute `pip install adal`. A continuación, use su `tenantId`, `clientId` y `clientSecret` de la entidad de servicio que creó en los pasos anteriores como valores para las variables apropiadas en el siguiente script.

```python
from adal import AuthenticationContext

client_id = "your-client-id"
client_secret = "your-client-secret"
resource_url = "https://login.microsoftonline.com"
tenant_id = "your-tenant-id"
authority = "{}/{}".format(resource_url, tenant_id)

auth_context = AuthenticationContext(authority)
token_response = auth_context.acquire_token_with_client_credentials("https://management.azure.com/", client_id, client_secret)
print(token_response)
```

La variable `token_response` es un diccionario que incluye el token y los metadatos asociados, como la fecha de expiración. Los tokens son válidos durante una hora y se pueden actualizar ejecutando la misma llamada de nuevo para recuperar un nuevo token. Lo siguiente es una respuesta de ejemplo.

```python
{
    'tokenType': 'Bearer',
    'expiresIn': 3599,
    'expiresOn': '2019-12-17 19:47:15.150205',
    'resource': 'https://management.azure.com/',
    'accessToken': 'random-oauth-token',
    'isMRRT': True,
    '_clientId': 'your-client-id',
    '_authority': 'https://login.microsoftonline.com/your-tenant-id'
}
```

Use `token_response["accessToken"]` para capturar el token de autenticación. Consulte la [documentación de la API REST](https://github.com/microsoft/MLOps/tree/master/examples/AzureML-REST-API) para ejemplos sobre cómo usar el token para realizar llamadas API.

## <a name="web-service-authentication"></a>Autenticación de servicio web

Los servicios web en Azure Machine Learning usan un patrón de autenticación diferente del que se ha descrito anteriormente. La manera más sencilla de autenticarse en los servicios web implementados es usar la **autenticación basada en claves**, que genera claves de autenticación de tipo portador estático que no es necesario actualizar. Si solo necesita autenticarse en un servicio web implementado, no es necesario configurar la autenticación de entidad de servicio como se mostró anteriormente.

Los servicios web implementados en Azure Kubernetes Service tienen habilitada la autenticación basada en claves *habilitada* de forma predeterminada. Los servicios implementados por Azure Container Instances tienen la autenticación basada en claves *deshabilitada* de forma predeterminada, pero puede habilitarla estableciendo `auth_enabled=True` al crear el servicio web de ACI. A continuación se proporciona un ejemplo de creación de una configuración de implementación de ACI con la autenticación basada en claves habilitada.

```python
from azureml.core.webservice import AciWebservice

aci_config = AciWebservice.deploy_configuration(cpu_cores = 1,
                                                memory_gb = 1,
                                                auth_enabled=True)
```

Después, puede usar la configuración de ACI personalizada en la implementación mediante la clase `Model`.

```python
from azureml.core.model import Model, InferenceConfig


inference_config = InferenceConfig(entry_script="score.py",
                                   environment=myenv)
aci_service = Model.deploy(workspace=ws,
                       name="aci_service_sample",
                       models=[model],
                       inference_config=inference_config,
                       deployment_config=aci_config)
aci_service.wait_for_deployment(True)
```

Para capturar las claves de autenticación, use `aci_service.get_keys()`. Para volver a generar una clave, utilice la función `regen_key()` y pase **Primary** o **Secondary**.

```python
aci_service.regen_key("Primary")
# or
aci_service.regen_key("Secondary")
```

Los servicios web también admiten la autenticación basada en tokens, pero solo para implementaciones de Azure Kubernetes Service. Vea el [procedimiento](how-to-consume-web-service.md) sobre el consumo de servicios web para información adicional sobre la autenticación.

### <a name="token-based-web-service-authentication"></a>Autenticación de servicio web basada en tokens

Cuando se habilita la autenticación por tokens para un servicio web, los usuarios deben presentar un token JSON Web Token de Azure Machine Learning al servicio web para tener acceso a él. El token expira después de un intervalo de tiempo especificado y debe actualizarse para seguir realizando llamadas.

* La autenticación por tokens está **deshabilitada de manera predeterminada** cuando se implementa en Azure Kubernetes Service.
* La autenticación por tokens **no se admite** cuando se implementa en Azure Container Instances.

Para controlar la autenticación por tokens, use el parámetro `token_auth_enabled` cuando cree o actualice una implementación.

Si la autenticación por tokens está habilitada, puede usar el método `get_token` para recuperar un token JSON Web Token (JWT) y la hora de expiración del token:

```python
token, refresh_by = service.get_token()
print(token)
```

> [!IMPORTANT]
> Tendrá que solicitar un nuevo token después de la hora `refresh_by` del token. Si necesita actualizar tokens fuera del SDK de Python, una opción es usar la API REST con la autenticación de entidad de servicio para hacer periódicamente la llamada `service.get_token()`, como se explicó anteriormente.
>
> Se recomienda crear el área de trabajo de Azure Machine Learning en la misma región que el clúster de Azure Kubernetes Service.
>
> Para autenticarse con un token, el servicio web hará una llamada a la región en la que se crea el área de trabajo de Azure Machine Learning. Si la región del área de trabajo no está disponible, no se podrá capturar un token para el servicio web, incluso si el clúster está en una región distinta del área de trabajo. El resultado es que la Autenticación de Azure AD no está disponible hasta que la región del área de trabajo vuelva a estar disponible.
>
> Además, cuanto mayor sea la distancia entre la región del clúster y la región del área de trabajo, más tiempo tardará la captura de un token.

## <a name="next-steps"></a>Pasos siguientes

* [Entrenamiento sobre el modo de clasificación de una imagen y su implementación](tutorial-train-models-with-aml.md).
* [Consumir un modelo de Azure Machine Learning que está implementado como un servicio web](how-to-consume-web-service.md).
