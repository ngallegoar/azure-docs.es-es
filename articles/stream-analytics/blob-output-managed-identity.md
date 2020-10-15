---
title: Autenticación de la salida de Blob con identidad administrada para Azure Stream Analytics
description: En este artículo se describe cómo usar las identidades administradas para autenticar su trabajo de Azure Stream Analytics en la salida de Azure Blob Storage.
author: cedarbaum
ms.author: sacedarb
ms.service: stream-analytics
ms.topic: how-to
ms.date: 03/11/2020
ms.openlocfilehash: 99b23b65a0ce1693bcd04d5828fe062f2f43ea73
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86044233"
---
# <a name="use-managed-identity-to-authenticate-your-azure-stream-analytics-job-to-azure-blob-storage-output"></a>Uso de identidad administrada para autenticar su trabajo de Azure Stream Analytics en la salida de Azure Blob Storage

[Autenticación de identidad administrada](../active-directory/managed-identities-azure-resources/overview.md) para la salida a Azure Blob Storage proporciona a los trabajos de Stream Analytics acceso directo a una cuenta de almacenamiento en lugar de tener que usar una cadena de conexión. Además de mejorar la seguridad, esta característica también le permite escribir datos en una cuenta de almacenamiento de una red virtual (VNET) en Azure.

En este artículo se muestra cómo habilitar la identidad administrada para las salidas del blob de un trabajo de Stream Analytics a través de Azure Portal y a través de una implementación de Azure Resource Manager.

## <a name="create-the-stream-analytics-job-using-the-azure-portal"></a>Creación de un trabajo de Stream Analytics mediante Azure Portal

1. Cree un nuevo trabajo de Stream Analytics o abra un trabajo existente en Azure Portal. En la barra de menús situada a la izquierda de la pantalla, seleccione **Identidad administrada**, que se encuentra en **Configurar**. Asegúrese de que está seleccionada la opción "Usar la identidad administrada asignada por el sistema" y, a continuación, haga clic en el botón **Guardar** en la parte inferior de la pantalla.

   ![Configuración de la Identidad administrada de Stream Analytics](./media/common/stream-analytics-enable-managed-identity.png)

2. En la ventana de propiedades de salida del receptor de salida de Azure Blob Storage, seleccione la lista desplegable Modo de autenticación y elija **Identidad administrada**. Para obtener información sobre las demás propiedades de salida, vea [Información sobre las salidas desde Azure Stream Analytics](./stream-analytics-define-outputs.md). Cuando haya terminado, haga clic en **Guardar**.

   ![Configuración de salida de Azure Blob Storage](./media/stream-analytics-managed-identities-blob-output-preview/stream-analytics-blob-output-blade.png)

3. Ahora que se ha creado el trabajo, consulte la sección [Concesión de acceso al trabajo de Stream Analytics a su cuenta de almacenamiento](#give-the-stream-analytics-job-access-to-your-storage-account) de este artículo.

## <a name="azure-resource-manager-deployment"></a>Implementación de Azure Resource Manager

El uso de Azure Resource Manager permite automatizar completamente la implementación de su trabajo de Stream Analytics. Puede implementar las plantillas de Resource Manager mediante Azure PowerShell o la [CLI de Azure.](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) Los ejemplos siguientes usan la CLI de Azure.


1. Para crear un recurso **Microsoft.StreamAnalytics/streamingjobs** con una identidad administrada, puede incluir la siguiente propiedad en la sección de recursos de la plantilla de Resource Manager:

    ```json
    "Identity": {
      "Type": "SystemAssigned",
    },
    ```

   Esta propiedad indica a Azure Resource Manager que cree y administre la identidad del trabajo de Stream Analytics. A continuación, se muestra un ejemplo de una plantilla de Resource Manager que implementa un trabajo de Stream Analytics con la identidad administrada habilitada y un receptor de salida de blob que usa la identidad administrada:

    ```json
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "resources": [
            {
                "apiVersion": "2017-04-01-preview",
                "name": "MyStreamingJob",
                "location": "[resourceGroup().location]",
                "type": "Microsoft.StreamAnalytics/StreamingJobs",
                "identity": {
                    "type": "systemAssigned"
                },
                "properties": {
                    "sku": {
                        "name": "standard"
                    },
                    "outputs":[
                        {
                            "name":"output",
                            "properties":{
                                "serialization": {
                                    "type": "JSON",
                                    "properties": {
                                        "encoding": "UTF8"
                                    }
                                },
                                "datasource":{
                                    "type":"Microsoft.Storage/Blob",
                                    "properties":{
                                        "storageAccounts": [
                                            { "accountName": "MyStorageAccount" }
                                        ],
                                        "container": "test",
                                        "pathPattern": "segment1/{date}/segment2/{time}",
                                        "dateFormat": "yyyy/MM/dd",
                                        "timeFormat": "HH",
                                        "authenticationMode": "Msi"
                                    }
                                }
                            }
                        }
                    ]
                }
            }
        ]
    }
    ```

    El trabajo anterior se puede implementar en el grupo de recursos **ExampleGroup** mediante el siguiente comando de la CLI de Azure:

    ```azurecli
    az group deployment create --resource-group ExampleGroup -template-file StreamingJob.json
    ```

2. Una vez creado el trabajo, puede usar Azure Resource Manager para recuperar la definición completa del trabajo.

    ```azurecli
    az resource show --ids /subscriptions/{SUBSCRIPTION_ID}/resourceGroups/{RESOURCE_GROUP}/providers/Microsoft.StreamAnalytics/StreamingJobs/{RESOURCE_NAME}
    ```

    El comando anterior devolverá una respuesta como la siguiente:

    ```json
    {
        "id": "/subscriptions/{SUBSCRIPTION_ID}/resourceGroups/{RESOURCE_GROUP}/providers/Microsoft.StreamAnalytics/streamingjobs/{RESOURCE_NAME}",
        "identity": {
            "principalId": "{PRINCIPAL_ID}",
            "tenantId": "{TENANT_ID}",
            "type": "SystemAssigned",
            "userAssignedIdentities": null
        },
        "kind": null,
        "location": "West US",
        "managedBy": null,
        "name": "{RESOURCE_NAME}",
        "plan": null,
        "properties": {
            "compatibilityLevel": "1.0",
            "createdDate": "2019-07-12T03:11:30.39Z",
            "dataLocale": "en-US",
            "eventsLateArrivalMaxDelayInSeconds": 5,
            "jobId": "{JOB_ID}",
            "jobState": "Created",
            "jobStorageAccount": null,
            "jobType": "Cloud",
            "outputErrorPolicy": "Stop",
            "package": null,
            "provisioningState": "Succeeded",
            "sku": {
                "name": "Standard"
            }
        },
        "resourceGroup": "{RESOURCE_GROUP}",
        "sku": null,
        "tags": null,
        "type": "Microsoft.StreamAnalytics/streamingjobs"
    }
    ```

   Tome nota del valor de **principalId** de la definición del trabajo, que identifica la identidad administrada del trabajo en Azure Active Directory y se usará en el paso siguiente para conceder al trabajo de Stream Analytics acceso a la cuenta de almacenamiento.

3. Ahora que se ha creado el trabajo, consulte la sección [Concesión de acceso al trabajo de Stream Analytics a su cuenta de almacenamiento](#give-the-stream-analytics-job-access-to-your-storage-account) de este artículo.


## <a name="give-the-stream-analytics-job-access-to-your-storage-account"></a>Concesión de acceso al trabajo de Stream Analytics a su cuenta de almacenamiento

Puede elegir entre dos niveles de acceso para su trabajo de Stream Analytics:

1. **Container level access** (Acceso a nivel de contenedor): esta opción proporciona al trabajo acceso a un contenedor existente específico.
2. **Account level access** (Acceso a nivel de cuenta): esta opción proporciona al trabajo acceso general a la cuenta de almacenamiento, incluida la capacidad de crear nuevos contenedores.

A menos que necesite el trabajo para crear contenedores en su nombre, debe elegir la opción **Container level access** (Acceso a nivel de contenedor), ya que esta concederá al trabajo el nivel mínimo de acceso necesario. A continuación, se explican ambas opciones para Azure Portal y la línea de comandos.

### <a name="grant-access-via-the-azure-portal"></a>Concesión de acceso mediante Azure Portal

#### <a name="container-level-access"></a>Acceso a nivel de contenedor

1. Vaya al panel de configuración del contenedor dentro de la cuenta de almacenamiento.

2. Seleccione **Control de acceso (IAM)** en el lado izquierdo.

3. En la sección "Agregar una asignación de roles", haga clic en **Agregar**.

4. En el panel de asignación de roles:

    1. Establezca el **rol** en "Colaborador de datos de Storage Blob".
    2. Asegúrese de que la lista desplegable **Asignar acceso a** esté establecida en "Usuario, grupo o entidad de servicio de Azure AD".
    3. Escriba el nombre del trabajo de Stream Analytics en el campo de búsqueda.
    4. Seleccione el trabajo de Stream Analytics y haga clic en **Guardar**.

   ![Concesión de acceso al contenedor](./media/stream-analytics-managed-identities-blob-output-preview/stream-analytics-container-access-portal.png)

#### <a name="account-level-access"></a>Acceso a nivel de cuenta

1. Vaya a la cuenta de almacenamiento.

2. Seleccione **Control de acceso (IAM)** en el lado izquierdo.

3. En la sección "Agregar una asignación de roles", haga clic en **Agregar**.

4. En el panel de asignación de roles:

    1. Establezca el **rol** en "Colaborador de datos de Storage Blob".
    2. Asegúrese de que la lista desplegable **Asignar acceso a** esté establecida en "Usuario, grupo o entidad de servicio de Azure AD".
    3. Escriba el nombre del trabajo de Stream Analytics en el campo de búsqueda.
    4. Seleccione el trabajo de Stream Analytics y haga clic en **Guardar**.

   ![Concesión de acceso a la cuenta](./media/stream-analytics-managed-identities-blob-output-preview/stream-analytics-account-access-portal.png)

### <a name="grant-access-via-the-command-line"></a>Concesión de acceso mediante la línea de comandos

#### <a name="container-level-access"></a>Acceso a nivel de contenedor

Para proporcionar acceso a un contenedor específico, ejecute el siguiente comando mediante la CLI de Azure:

   ```azurecli
   az role assignment create --role "Storage Blob Data Contributor" --assignee <principal-id> --scope /subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/<container-name>
   ```

#### <a name="account-level-access"></a>Acceso a nivel de cuenta

Para proporcionar acceso a la cuenta completa, ejecute el siguiente comando mediante la CLI de Azure:

   ```azurecli
   az role assignment create --role "Storage Blob Data Contributor" --assignee <principal-id> --scope /subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>
   ```

## <a name="enable-vnet-access"></a>Habilitación del acceso a la red virtual

Al configurar el panel **Firewalls y redes virtuales** de la cuenta de almacenamiento, si quiere, puede permitir el tráfico de red desde otros servicios de Microsoft de confianza. Cuando Stream Analytics se autentica mediante la identidad administrada, proporciona una prueba de que la solicitud se origina en un servicio de confianza. A continuación, le proporcionamos instrucciones para habilitar esta excepción de acceso a la red virtual.

1.  Navegue hasta el panel "Firewalls y redes virtuales" dentro del panel de configuración de la cuenta de almacenamiento.
2.  Asegúrese de que la opción "Permitir que los servicios de Microsoft de confianza accedan a esta cuenta de almacenamiento" esté habilitada.
3.  Si la habilitó, haga clic en **Guardar**.

   ![Habilitación del acceso a la red virtual](./media/stream-analytics-managed-identities-blob-output-preview/stream-analytics-vnet-exception.png)

## <a name="limitations"></a>Limitaciones
A continuación, detallamos las limitaciones actuales de esta característica:

1. Cuentas de Azure Storage clásicas.

2. Cuentas de Azure sin Azure Active Directory.

3. No se admite el acceso multiinquilino. La entidad de servicio creada para un trabajo determinado de Stream Analytics debe residir en el mismo inquilino de Azure Active Directory en el que se creó el trabajo, y no se puede usar en un recurso que resida en un inquilino de Azure Active Directory diferente.

4. La [identidad asignada por el usuario](../active-directory/managed-identities-azure-resources/overview.md) no se admite. Esto significa que el usuario no puede especificar el uso de su propia entidad de servicio en su trabajo de Stream Analytics. Azure Stream Analytics debe generar la entidad de servicio.

## <a name="next-steps"></a>Pasos siguientes

* [Información sobre las salidas desde Azure Stream Analytics](./stream-analytics-define-outputs.md)
* [Particionamiento de la salida de blobs personalizada en Azure Stream Analytics](./stream-analytics-custom-path-patterns-blob-storage-output.md)
