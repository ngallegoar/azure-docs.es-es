---
title: SDK de Azure HDInsight para Go
description: Material de referencia sobre el uso del SDK de Azure HDInsight en clústeres de Go y Apache Hadoop
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seodec18, devx-track-azurecli
ms.date: 01/03/2020
ms.openlocfilehash: 6fdd7b9e5fda92bd75e54ea5b4aad6a3ba6ecbea
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92748755"
---
# <a name="hdinsight-sdk-for-go-preview"></a>SDK de HDInsight para Go (versión preliminar)

## <a name="overview"></a>Información general
El SDK de HDInsight para Go proporciona clases y funciones que permiten administrar los clústeres de HDInsight. Incluye operaciones para crear, eliminar, actualizar, enumerar, cambiar tamaño, ejecutar acciones de script, supervisar y obtener propiedades de clústeres de HDInsight, entre otras.

> [!NOTE]  
>También encontrará material de referencia de GoDoc para este SDK [aquí](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2015-03-01-preview/hdinsight).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Prerequisites

* Una [`go get`herramienta](https://github.com/golang/go/wiki/GoGetTools).
* [Go](https://golang.org/dl/).

## <a name="sdk-installation"></a>Instalación del SDK

Desde su ubicación de GOPATH, ejecute `go get github.com/Azure/azure-sdk-for-go/tree/master/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight`

## <a name="authentication"></a>Authentication

En primer lugar, el SDK necesita autenticarse en su suscripción de Azure.  Siga el ejemplo siguiente para crear una entidad de servicio y usarla para la autenticación. Una vez hecho esto, tendrá una instancia de un `ClustersClient`, que contiene muchas funciones (descritas en las secciones siguientes) que se pueden usar para realizar operaciones de administración.

> [!NOTE]  
> Además del siguiente ejemplo, hay otras maneras de autenticar que podrían ser más adecuadas para sus necesidades. Todas las funciones se describen aquí: [Funciones de autenticación en Azure SDK para Go](/azure/go/azure-sdk-go-authorization).

### <a name="authentication-example-using-a-service-principal"></a>Ejemplo de autenticación con una entidad de servicio

En primer lugar, inicie sesión en [Azure Cloud Shell](https://shell.azure.com/bash). Compruebe que está usando actualmente la suscripción en la que desea que se cree la entidad de servicio.

```azurecli-interactive
az account show
```

La información de la suscripción se muestra en formato JSON.

```json
{
  "environmentName": "AzureCloud",
  "id": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "isDefault": true,
  "name": "XXXXXXX",
  "state": "Enabled",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "user": {
    "cloudShellID": true,
    "name": "XXX@XXX.XXX",
    "type": "user"
  }
}
```

Si no ha iniciado sesión en la suscripción correcta, ejecute lo siguiente para seleccionar la correcta: 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]  
> Si aún no ha registrado el proveedor de recursos de HDInsight con otra función (por ejemplo, creando un clúster de HDInsight mediante Azure Portal), deberá hacerlo una vez para poder realizar la autenticación. Se puede hacer desde [Azure Cloud Shell](https://shell.azure.com/bash), con el siguiente comando:
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

A continuación, elija un nombre para la entidad de servicio y créela con el siguiente comando:

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

La información de la entidad de servicio se muestra con formato JSON.

```json
{
  "clientId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "clientSecret": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "subscriptionId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```

Copie el siguiente fragmento de código y rellene `TENANT_ID`, `CLIENT_ID`, `CLIENT_SECRET` y `SUBSCRIPTION_ID` con las cadenas del código JSON que se devolvió después de ejecutar el comando para crear la entidad de servicio.

```golang
package main

import (
    "context"
    "github.com/Azure/go-autorest/autorest/azure/auth"
    hdi "github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight"
    "github.com/Azure/go-autorest/autorest/to"
)

func main() {
    var err error

    // Tenant ID for your Azure Subscription
    var TENANT_ID = ""
    // Your Service Principal App Client ID
    var CLIENT_ID = ""
    // Your Service Principal Client Secret
    var CLIENT_SECRET = ""
    // Azure Subscription ID
    var SUBSCRIPTION_ID = ""

    var credentials = auth.NewClientCredentialsConfig(CLIENT_ID, CLIENT_SECRET, TENANT_ID)
    var client = hdi.NewClustersClient(SUBSCRIPTION_ID)

    client.Authorizer, err = credentials.Authorizer()
    if (err != nil) {
        fmt.Println("Error: ", err)
    }
```

## <a name="cluster-management"></a>Administración de clústeres

> [!NOTE]  
> En esta sección se da por supuesto que ya ha autenticado y construido una instancia de `ClusterClient`, y que la ha almacenado en una variable llamada `client`. En la sección Autenticación anterior encontrará las instrucciones para autenticarse y obtener un `ClusterClient`.

### <a name="create-a-cluster"></a>Crear un clúster

Para crear un nuevo clúster, se puede llamar a `client.Create()`. 

#### <a name="example"></a>Ejemplo

En este ejemplo se muestra cómo crear un clúster [Apache Spark](https://spark.apache.org/) con dos nodos principales y un nodo de trabajo.

> [!NOTE]  
> En primer lugar debe crear un grupo de recursos y la cuenta de almacenamiento, tal y como se explica más adelante. Si ya los ha creado, puede omitir estos pasos.

##### <a name="creating-a-resource-group"></a>Creación de un grupo de recursos

Puede crear un grupo de recursos con [Azure Cloud Shell](https://shell.azure.com/bash), con el siguiente comando:

```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```

##### <a name="creating-a-storage-account"></a>Creación de una cuenta de almacenamiento

Puede crear una cuenta de almacenamiento con [Azure Cloud Shell](https://shell.azure.com/bash), con el siguiente comando:

```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```

Ahora ejecute el siguiente comando para obtener la clave de la cuenta de almacenamiento (lo necesitará para crear un clúster):

```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```

---
El fragmento de código de Go siguiente crea un clúster de Spark con dos nodos principales y un nodo de trabajo. Rellene las variables en blanco tal y como se explica en los comentarios y no dude en cambiar otros parámetros para que se adapten a sus necesidades específicas.

```golang
// The name for the cluster you are creating
var clusterName = "";
// The name of your existing Resource Group
var resourceGroupName = "";
// Choose a username
var username = "";
// Choose a password
var password = "";
// Replace <> with the name of your storage account
var storageAccount = "<>.blob.core.windows.net";
// Storage account key you obtained above
var storageAccountKey = "";
// Choose a region
var location = "";
var container = "default";

var parameters = hdi.ClusterCreateParametersExtended {
    Location: to.StringPtr(location),
    Tags: make(map[string]*string),
    Properties: &hdi.ClusterCreateProperties {
        ClusterVersion: to.StringPtr("3.6"),
        OsType: hdi.Linux,
        ClusterDefinition: &hdi.ClusterDefinition {
            Kind: to.StringPtr("spark"),
            Configurations: map[string]map[string]interface{}{
                "gateway": {
                    "restAuthCredential.isEnabled": "True",
                    "restAuthCredential.username":  username,
                    "restAuthCredential.password":  password,
                },
            },
        },
        Tier: hdi.Standard,
        ComputeProfile: &hdi.ComputeProfile {
            Roles: &[]hdi.Role {
                hdi.Role {
                    Name: to.StringPtr("headnode"),
                    TargetInstanceCount: to.Int32Ptr(2),
                    HardwareProfile: &hdi.HardwareProfile {
                        VMSize: to.StringPtr("Large"),
                    },
                    OsProfile: &hdi.OsProfile {
                        LinuxOperatingSystemProfile: &hdi.LinuxOperatingSystemProfile {
                            Username: to.StringPtr(username),
                            Password: to.StringPtr(password),
                        },
                    },
                },
                hdi.Role {
                    Name: to.StringPtr("workernode"),
                    TargetInstanceCount: to.Int32Ptr(1),
                    HardwareProfile: &hdi.HardwareProfile {
                        VMSize: to.StringPtr("Large"),
                    },
                    OsProfile: &hdi.OsProfile {
                        LinuxOperatingSystemProfile: &hdi.LinuxOperatingSystemProfile {
                            Username: to.StringPtr(username),
                            Password: to.StringPtr(password),
                        },
                    },
                },
            },
        },
        StorageProfile: &hdi.StorageProfile {
            Storageaccounts: &[]hdi.StorageAccount {
                hdi.StorageAccount {
                    Name: to.StringPtr(storageAccount),
                    Key: to.StringPtr(storageAccountKey),
                    Container: to.StringPtr(container),
                    IsDefault: to.BoolPtr(true),
                },
            },
        },
    },
}
client.Create(context.Background(), resourceGroupName, clusterName, parameters)
```

### <a name="get-cluster-details"></a>Obtención de los detalles del clúster

Para obtener las propiedades de un clúster determinado:

```golang
client.Get(context.Background(), "<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>Ejemplo

Puede usar `get` para confirmar que ha creado correctamente el clúster.

```golang
cluster, err := client.Get(context.Background(), resourceGroupName, clusterName)
if (err != nil) {
    fmt.Println("Error: ", err)
}
fmt.Println(*cluster.Name)
fmt.Println(*cluster.ID
```

La salida debe ser similar a la siguiente:

```
<Cluster Name>
/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>
```

### <a name="list-clusters"></a>Lista de clústeres

#### <a name="list-clusters-under-the-subscription"></a>Lista de clústeres de la suscripción

```golang
client.List()
```

#### <a name="list-clusters-by-resource-group"></a>Lista de clústeres por grupo de recursos

```golang
client.ListByResourceGroup("<Resource Group Name>")
```

> [!NOTE]  
> Tanto `List()` como `ListByResourceGroup()` devuelven un struct `ClusterListResultPage`. Para obtener la siguiente página, llame a `Next()`. Este paso se puede repetir hasta que `ClusterListResultPage.NotDone()` devuelva `false`, tal y como se muestra en el ejemplo siguiente.

#### <a name="example"></a>Ejemplo

El ejemplo siguiente imprime las propiedades de todos los clústeres de la suscripción actual:

```golang
page, err := client.List(context.Background())
if (err != nil) {
    fmt.Println("Error: ", err)
}
for (page.NotDone()) {
    for _, cluster := range page.Values() {
        fmt.Println(*cluster.Name)
    }
    err = page.Next();
    if (err != nil) {
        fmt.Println("Error: ", err)
    }
}
```

### <a name="delete-a-cluster"></a>Eliminación de un clúster

Para eliminar un clúster:

```golang
client.Delete(context.Background(), "<Resource Group Name>", "<Cluster Name>")
```

### <a name="update-cluster-tags"></a>Actualización de las etiquetas del clúster

Puede actualizar las etiquetas de un clúster determinado de este modo:

```golang
client.Update(context.Background(), "<Resource Group Name>", "<Cluster Name>", hdi.ClusterPatchParameters{<map[string]*string} of Tags>)
```

#### <a name="example"></a>Ejemplo

```golang
client.Update(context.Background(), "SDKTestRG", "SDKTest", hdi.ClusterPatchParameters{map[string]*string{"tag1Name" : to.StringPtr("tag1Value"), "tag2Name" : to.StringPtr("tag2Value")}})
```

### <a name="resize-cluster"></a>Cambio del tamaño de clúster

Para cambiar el tamaño de un número de nodos de trabajo de un clúster determinado, puede especificar un nuevo tamaño como sigue:

```golang
client.Resize(context.Background(), "<Resource Group Name>", "<Cluster Name>", hdi.ClusterResizeParameters{<Num of Worker Nodes (int)>})
```

## <a name="cluster-monitoring"></a>Supervisión de clústeres

El SDK de administración de HDInsight también puede utilizarse para administrar la supervisión en los clústeres mediante Operations Management Suite (OMS).

De forma similar a como creó `ClusterClient` para usar en las operaciones de administración, deberá crear un elemento `ExtensionClient` para usarlo en las operaciones de supervisión. Una vez que haya completado la sección de autenticación anterior, puede crear un `ExtensionClient` como se indica a continuación:

```golang
extClient := hdi.NewExtensionsClient(SUBSCRIPTION_ID)
extClient.Authorizer, _ = credentials.Authorizer()
```

> [!NOTE]  
> En los ejemplos de supervisión siguientes se supone que ya ha inicializado un `ExtensionClient` denominado `extClient` y ha establecido su `Authorizer` como se indicó anteriormente.

### <a name="enable-oms-monitoring"></a>Habilitación de OMS Monitoring

> [!NOTE]  
> Para habilitar OMS Monitoring, debe tener un área de trabajo de Log Analytics. Si aún no ha creado una, consulte cómo hacerlo aquí: [Creación de un área de trabajo de Log Analytics en Azure Portal](../azure-monitor/learn/quick-create-workspace.md).

Para habilitar OMS Monitoring en el clúster:

```golang
extClient.EnableMonitoring(context.Background(), "<Resource Group Name", "Cluster Name", hdi.ClusterMonitoringRequest {WorkspaceID: to.StringPtr("<Workspace Id>")})
```

### <a name="view-status-of-oms-monitoring"></a>Visualización del estado de OMS Monitoring

Para obtener el estado de OMS en el clúster:

```golang
extClient.GetMonitoringStatus(context.Background(), "<Resource Group Name", "Cluster Name")
```

### <a name="disable-oms-monitoring"></a>Deshabilitación de OMS Monitoring

Para deshabilitar OMS en el clúster:

```golang
extClient.DisableMonitoring(context.Background(), "<Resource Group Name", "Cluster Name")
```

## <a name="script-actions"></a>Acciones de script

HDInsight proporciona una función de configuración denominada acciones de script, que invoca scripts personalizados para personalizar el clúster.

> [!NOTE]  
> Para más información sobre cómo usar las acciones de script, consulte [Personalización de clústeres de HDInsight basados en Linux mediante la acción de script](./hdinsight-hadoop-customize-cluster-linux.md).

### <a name="execute-script-actions"></a>Ejecución de acciones de script

Puede ejecutar acciones de script en un clúster determinado de la siguiente manera:

```golang
var scriptAction1 = hdi.RuntimeScriptAction{Name: to.StringPtr("<Script Name>"), URI: to.StringPtr("<URL To Script>"), Roles: <&[]string of roles>} //valid roles are "headnode", "workernode", "zookeepernode", and "edgenode"
client.ExecuteScriptActions(context.Background(), "<Resource Group Name>", "<Cluster Name>", hdi.ExecuteScriptActionParameters{PersistOnSuccess: to.BoolPtr(true), ScriptActions: &[]hdi.RuntimeScriptAction{scriptAction1}}) //add more RuntimeScriptActions to the list to execute multiple scripts
```

Para las operaciones "Eliminar acción de script" y "Enumerar acciones de script persistentes", deberá crear un `ScriptActionsClient`, de forma similar a como creó `ClusterClient` para usarlo en las operaciones de administración. Una vez que haya completado la sección de autenticación anterior, puede crear un `ScriptActionsClient` como se indica a continuación:

```golang
scriptActionsClient := hdi.NewScriptActionsClient(SUBSCRIPTION_ID)
scriptActionsClient.Authorizer, _ = credentials.Authorizer()
```

> [!NOTE]  
> En los ejemplos de acciones de script siguientes, se supone que ya ha inicializado un `ScriptActionsClient` denominado `scriptActionsClient` y ha establecido su `Authorizer` como se indicó anteriormente.

### <a name="delete-script-action"></a>Eliminación de una acción de script

Para eliminar una acción de script persistente específica en un clúster determinado:

```golang
scriptActionsClient.Delete(context.Background(), "<Resource Group Name>", "<Cluster Name>", "<Script Name>")
```

### <a name="list-persisted-script-actions"></a>Enumeración de acciones de script persistentes

> [!NOTE]  
> `ListByCluster()` devuelve un struct `ScriptActionsListPage`. Para obtener la siguiente página, llame a `Next()`. Este paso se puede repetir hasta que `ClusterListResultPage.NotDone()` devuelva `false`, tal y como se muestra en el ejemplo siguiente.

Para mostrar una lista de todas las acciones de script persistentes para el clúster especificado:
```golang
scriptActionsClient.ListByCluster(context.Background(), "<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>Ejemplo

```golang
page, err := scriptActionsClient.ListByCluster(context.Background(), resourceGroupName, clusterName)
if (err != nil) {
    fmt.Println("Error: ", err)
}
for (page.NotDone()) {
    for _, script := range page.Values() {
        fmt.Println(*script.Name) //There are functions to get other properties of RuntimeScriptActionDetail besides Name, such as Status, Operation, StartTime, EndTime, etc. See reference documentation.
    }
    err = page.Next();
    if (err != nil) {
        fmt.Println("Error: ", err)
    }
}
```

### <a name="list-all-scripts-execution-history"></a>Enumeración del historial de ejecución de todos los scripts

Para esta operación, debe crear un `ScriptExecutionHistoryClient`, de manera similar a como creó `ClusterClient` para usarlo en las operaciones de administración. Una vez que haya completado la sección de autenticación anterior, puede crear un `ScriptActionsClient` como se indica a continuación:

```golang
scriptExecutionHistoryClient := hdi.NewScriptExecutionHistoryClient(SUBSCRIPTION_ID)
scriptExecutionHistoryClient.Authorizer, _ = credentials.Authorizer()
```

> [!NOTE]  
> A continuación, se supone que ya ha inicializado un `ScriptExecutionHistoryClient` denominado `scriptExecutionHistoryClient` y ha establecido su `Authorizer` como se indicó anteriormente.

Para mostrar una lista del historial de ejecución de todos los scripts para el clúster especificado:

```golang
scriptExecutionHistoryClient.ListByCluster(context.Background(), "<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>Ejemplo

Este ejemplo imprime todos los detalles de todas las ejecuciones de script pasadas.

```golang
page, err := scriptExecutionHistoryClient.ListByCluster(context.Background(), resourceGroupName, clusterName)
if (err != nil) {
    fmt.Println("Error: ", err)
}
for (page.NotDone()) {
    for _, script := range page.Values() {
        fmt.Println(*script.Name) //There are functions to get other properties of RuntimeScriptActionDetail besides Name, such as Status, Operation, StartTime, EndTime, etc. See reference documentation.
    }
    err = page.Next();
    if (err != nil) {
        fmt.Println("Error: ", err)
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

Explore el [material de referencia de GoDoc](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2015-03-01-preview/hdinsight). GoDocs proporciona documentación de referencia para todas las funciones en el SDK.
