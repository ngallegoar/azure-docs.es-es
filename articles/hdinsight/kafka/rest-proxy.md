---
title: 'Proxy de REST de Apache Kafka: Azure HDInsight'
description: Obtenga información sobre cómo realizar operaciones de Apache Kafka mediante un servidor proxy REST de Kafka en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/03/2020
ms.openlocfilehash: 6bf34f8fb15bf8fddb1ba398ed678d5c98b8c84f
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2020
ms.locfileid: "80667780"
---
# <a name="interact-with-apache-kafka-clusters-in-azure-hdinsight-using-a-rest-proxy"></a>Interacción con clústeres de Apache Kafka en Azure HDInsight mediante un proxy de REST

El proxy de REST de Kafka le permite interactuar con el clúster de Kafka mediante una API REST a través de HTTP. Esta acción significa que los clientes de Kafka pueden estar fuera de la red virtual. Los clientes pueden realizar llamadas HTTP sencillas al clúster de Kafka, en lugar de depender de las bibliotecas de Kafka. En este artículo se muestra cómo crear un clúster de Kafka habilitado para servidor proxy REST. También proporciona código de ejemplo que muestra cómo realizar llamadas al servidor proxy REST.

## <a name="rest-api-reference"></a>Referencia de la API REST

Para conocer las operaciones que admite la API REST de Kafka, consulte [Referencia de la API del servidor proxy REST de Kafka en HDInsight](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy).

## <a name="background"></a>Información previa

![Diseño del servidor proxy REST de Kafka](./media/rest-proxy/rest-proxy-architecture.png)

Para obtener la especificación completa de las operaciones que admite la API, consulte [API del servidor proxy REST de Apache Kafka](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy).

### <a name="rest-proxy-endpoint"></a>Punto de conexión de proxy de REST

Al crear un clúster de Kafka en HDInsight con el servidor proxy REST, se crea un nuevo punto de conexión público para el clúster, que se puede encontrar en las **Propiedades** del clúster de HDInsight en Azure Portal.

### <a name="security"></a>Seguridad

El acceso al proxy de REST de Kafka se administra con grupos de seguridad de Azure Active Directory. Al crear el clúster de Kafka, proporcione al grupo de seguridad de Azure AD acceso al punto de conexión REST. El propietario del grupo debe registrar en este grupo los clientes de Kafka que necesitan acceso al servidor proxy REST. El propietario del grupo puede realizar el registro mediante el portal o PowerShell.

Para las solicitudes al punto de conexión proxy REST, las aplicaciones cliente deben obtener un token de OAuth. El token se usa para comprobar la pertenencia al grupo de seguridad. Busque a continuación un [ejemplo de aplicación cliente](#client-application-sample) que muestre cómo obtener un token de OAuth. La aplicación cliente pasa el token de OAuth en la solicitud HTTP al servidor proxy REST.

> [!NOTE]  
> Consulte [Administración del acceso a recursos y aplicaciones con grupos en Azure Active Directory](../../active-directory/fundamentals/active-directory-manage-groups.md) para más información sobre los grupos de seguridad de AAD. Para obtener más información sobre cómo funcionan los tokens de OAuth, consulte [Autorización del acceso a aplicaciones web de Azure Active Directory mediante el flujo de concesión de código OAuth 2.0](../../active-directory/develop/v1-protocols-oauth-code.md).

## <a name="prerequisites"></a>Prerrequisitos

1. Registrar una aplicación con Azure AD. Las aplicaciones cliente que escriba para interactuar con el proxy de REST de Kafka usarán el identificador y el secreto de la aplicación para autenticarse en Azure.

1. Cree un grupo de seguridad de Azure AD. Agregue la aplicación que ha registrado en Azure AD al grupo de seguridad como un **miembro** del grupo. Este grupo de seguridad se usará para controlar qué aplicaciones pueden interactuar con el proxy de REST. Para obtener más información sobre cómo crear grupos de Azure AD, consulte [Creación de un grupo básico e incorporación de miembros con Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

    Validar que el grupo es de tipo **Seguridad**.
    ![Grupo de seguridad](./media/rest-proxy/rest-proxy-group.png)

    Valide que la aplicación es miembro del grupo.
    ![Comprobación de la pertenencia](./media/rest-proxy/rest-proxy-membergroup.png)

## <a name="create-a-kafka-cluster-with-rest-proxy-enabled"></a>Creación de un clúster de Kafka con el proxy de REST habilitado

1. Durante el flujo de trabajo de creación del clúster de Kafka, en la pestaña **Seguridad y redes**, active la opción **Habilitar proxy REST de Kafka**.

     ![Habilitación del proxy de REST de Kafka y selección del grupo de seguridad](./media/rest-proxy/azure-portal-cluster-security-networking-kafka-rest.png)

1. Haga clic en **Seleccionar grupo de seguridad**. En la lista de grupos de seguridad, seleccione el que quiere que tenga acceso al proxy de REST. Puede usar el cuadro de búsqueda para buscar el grupo de seguridad adecuado. Haga clic en el botón **Seleccionar** de la parte inferior.

     ![Habilitación del proxy de REST de Kafka y selección del grupo de seguridad](./media/rest-proxy/azure-portal-cluster-security-networking-kafka-rest2.png)

1. Complete los pasos restantes para crear el clúster como se describe en [Creación de un clúster de Apache Kafka en Azure HDInsight mediante Azure Portal](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-get-started).

1. Una vez creado el clúster, vaya a las propiedades del clúster para registrar la URL del proxy de REST de Kafka.

     ![ver la URL de proxy de REST de Kafka](./media/rest-proxy/apache-kafka-rest-proxy-view-proxy-url.png)

## <a name="client-application-sample"></a>Nombre de la aplicación cliente

Puede usar el código de Python siguiente para interactuar con el proxy de REST en el clúster de Kafka. Para ejecutar el ejemplo de código, siga estos pasos:

1. Guarde el código de ejemplo en una máquina con Python instalado.
1. Instale las dependencias de Python necesarias ejecutando `pip3 install adal` y `pip install msrestazure`.
1. Modifique la sección de código **Configure these properties** (Configurar estas propiedades) y actualice las siguientes propiedades en su entorno:

    |Propiedad |Descripción |
    |---|---|
    |Id. de inquilino|Inquilino de Azure donde se encuentra la suscripción.|
    |Id. de cliente|Identificador de la aplicación que registró en el grupo de seguridad.|
    |Secreto del cliente|Secreto de la aplicación que registró en el grupo de seguridad.|
    |Kafkarest_endpoint|Obtenga este valor en la pestaña **Propiedades** de la información general del clúster, tal como se describe en la [sección de implementación](#create-a-kafka-cluster-with-rest-proxy-enabled). Debería tener el siguiente formato: `https://<clustername>-kafkarest.azurehdinsight.net`.|

1. Desde la línea de comandos, ejecute el archivo Python mediante la ejecución de `python <filename.py>`.

Este código realiza la siguiente acción:

1. Recupera un token de OAuth de Azure AD.
1. Muestra cómo realizar una solicitud al servidor proxy REST de Kafka.

Para obtener más información sobre cómo obtener tokens de OAuth en Python, consulte [Clase AuthenticationContext de Python](https://docs.microsoft.com/python/api/adal/adal.authentication_context.authenticationcontext?view=azure-python). Es posible que vea un retraso en tanto que los `topics` que no se crean o eliminan mediante el servidor proxy REST de Kafka se reflejan. Este retraso se debe a la actualización de la memoria caché.

```python
#Required python packages
#pip3 install adal
#pip install msrestazure

import adal
from msrestazure.azure_active_directory import AdalAuthentication
from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD
import requests

#--------------------------Configure these properties-------------------------------#
# Tenant ID for your Azure Subscription
tenant_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
# Your Client Application Id
client_id = 'XYZABCDE-1234-1234-1234-ABCDEFGHIJKL'
# Your Client Credentials
client_secret = 'password'
# kafka rest proxy -endpoint
kafkarest_endpoint = "https://<clustername>-kafkarest.azurehdinsight.net"
#--------------------------Configure these properties-------------------------------#

#getting token
login_endpoint = AZURE_PUBLIC_CLOUD.endpoints.active_directory
resource = "https://hib.azurehdinsight.net"
context = adal.AuthenticationContext(login_endpoint + '/' + tenant_id)

token = context.acquire_token_with_client_credentials(
    resource,
    client_id,
    client_secret)

accessToken = 'Bearer ' + token['accessToken']

print(accessToken)

# relative url
getstatus = "/v1/metadata/topics"
request_url = kafkarest_endpoint + getstatus

# sending get request and saving the response as response object
response = requests.get(request_url, headers={'Authorization': accessToken})
print(response.content)
```

Consulte a continuación otro ejemplo sobre cómo obtener un token de Azure para el servidor proxy REST mediante el comando curl. Observe que se debe especificar `resource=https://hib.azurehdinsight.net` al obtener un token.

```cmd
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=<clientid>&client_secret=<clientsecret>&grant_type=client_credentials&resource=https://hib.azurehdinsight.net' 'https://login.microsoftonline.com/<tenantid>/oauth2/token'
```

## <a name="next-steps"></a>Pasos siguientes

* [Documentos de referencia de la API de proxy de REST de Kafka](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy/)
