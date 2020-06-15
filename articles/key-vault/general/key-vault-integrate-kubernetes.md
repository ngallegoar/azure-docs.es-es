---
title: Integración de Azure Key Vault con Kubernetes
description: En este tutorial, accederá a los secretos de Azure Key Vault y los recuperará mediante el controlador Secrets Store CSI (Container Storage Interface) para montarlos en los pods de Kubernetes.
author: taytran0
ms.author: t-trtr
ms.service: key-vault
ms.topic: tutorial
ms.date: 06/04/2020
ms.openlocfilehash: e945a30ca1fcd62fdfccd16d4e853540dbf73d8a
ms.sourcegitcommit: ce44069e729fce0cf67c8f3c0c932342c350d890
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/09/2020
ms.locfileid: "84636939"
---
# <a name="tutorial-configure-and-run-the-azure-key-vault-provider-for-secret-store-csi-driver-on-kubernetes"></a>Tutorial: Configuración y ejecución del proveedor de Azure Key Vault para el controlador Secret Store CSI en Kubernetes

En este tutorial, accederá a los secretos de Azure Key Vault y los recuperará mediante el controlador Secrets Store CSI (Container Storage Interface) para montarlos en los pods de Kubernetes.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una entidad de servicio.
> * Implementar un clúster de Azure Kubernetes Service.
> * Instalar Helm y el controlador Secrets Store CSI.
> * Crear una instancia de Azure Key Vault y establecer secretos.
> * Crear su propio objeto SecretProviderClass.
> * Implementar su pod con secretos montados desde Key Vault.

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

Antes de iniciar este tutorial, instale la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?view=azure-cli-latest).

## <a name="create-a-service-principal-or-use-managed-identities"></a>Creación de una entidad de servicio o uso de identidades administradas

Si tiene planea usar identidades administradas, puede pasar a la sección siguiente.

Cree una entidad de servicio para controlar los recursos a los que se puede acceder desde Azure Key Vault. El acceso de esta entidad de servicio está restringido por los roles que se le hayan asignado. Esta característica permite controlar el modo en que la entidad de servicio puede administrar sus secretos. En el ejemplo siguiente, el nombre de la entidad de servicio es **contosoServicePrincipal**.

```azurecli
az ad sp create-for-rbac --name contosoServicePrincipal --skip-assignment
```
Esta operación devuelve una serie de pares clave-valores:

![Imagen](../media/kubernetes-key-vault-1.png)

Copie el valor de **appID** y **password**. Estas credenciales las necesitará más adelante.



## <a name="deploy-an-azure-kubernetes-service-cluster-using-azure-cli"></a>Implementación de un clúster de Azure Kubernetes Service mediante la CLI de Azure

No es preciso usar Azure Cloud Shell, será suficiente con el símbolo del sistema (terminal) con la CLI de Azure CLI instalada. 

Siga esta [guía](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough) y complete las siguientes secciones: **Creación de un grupo de recursos**, **Creación de un clúster de AKS** y **Conexión al clúster**.

**Nota:** Si planea usar la identidad del pod, en lugar de una entidad de servicio, asegúrese de habilitarlo al crear el clúster de Kubernetes, como se muestra a continuación:

```azurecli
az aks create -n contosoAKSCluster -g contosoResourceGroup --kubernetes-version 1.16.9 --node-count 1 --enable-managed-identity
```

1. [Establezca la variable de entorno PATH](https://www.java.com/en/download/help/path.xml) en el archivo "kubectl.exe" que se descargó.
1. Compruebe la versión de Kubernetes con el siguiente comando. Este comando generará la versión del cliente y del servidor. La versión del cliente es el archivo "kubectl.exe" que instaló, mientras que la versión del servidor es la instancia de Azure Kubernetes Services en que se ejecuta el clúster.
    ```azurecli
    kubectl version
    ```
1. Asegúrese de que la versión de Kubernetes sea la **1.16.0**, o cualquier versión superior. Este comando actualizará tanto el clúster de Kubernetes como el grupo de nodos. Puede tardar un par de minutos en ejecutarse. En este ejemplo, el grupo de recursos es **contosoResourceGroup** y el clúster de Kubernetes es **contosoAKSCluster**.
    ```azurecli
    az aks upgrade --kubernetes-version 1.16.9 --name contosoAKSCluster --resource-group contosoResourceGroup
    ```
1. Muestre los metadatos del clúster de AKS que ha creado mediante el comando siguiente. Copie los valores **principalId**, **clientId**, **subscriptionId**y **nodeResourceGroup**.
    ```azurecli
    az aks show --name contosoAKSCluster --resource-group contosoResourceGroup
    ```

    Este es el resultado con ambos parámetros resaltados.
    
    ![Imagen](../media/kubernetes-key-vault-5.png) ![Image](../media/kubernetes-key-vault-6.png)
    
## <a name="install-helm-and-secrets-store-csi-driver"></a>Instalación de Helm y el controlador Secrets Store CSI

Para instalar el controlador Secrets Store CSI, es preciso instalar [Helm](https://helm.sh/docs/intro/install/).

La interfaz de controlador [Secrets Store CSI](https://github.com/Azure/secrets-store-csi-driver-provider-azure/blob/master/charts/csi-secrets-store-provider-azure/README.md) permite obtener el contenido de los secretos almacenado en una instancia de Azure Key Vault y usar la interfaz del controlador para montar dicho contenido en los pods de Kubernetes.

1. Compruebe la versión de Helm y asegúrese de que sea la 3, o cualquier versión superior:
    ```azurecli
    helm version
    ```
1. Instale el controlador Secrets Store CSI y el proveedor de Azure Key Vault para el controlador:
    ```azurecli
    helm repo add csi-secrets-store-provider-azure https://raw.githubusercontent.com/Azure/secrets-store-csi-driver-provider-azure/master/charts

    helm install csi-secrets-store-provider-azure/csi-secrets-store-provider-azure --generate-name
    ```

## <a name="create-an-azure-key-vault-and-set-secrets"></a>Creación de una instancia de Azure Key Vault y establecimiento de secretos

Siga esta [guía](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-cli) para crear su propia instancia de Key Vault y establecer sus secretos.

**Nota:** No es necesario usar Azure Cloud Shell ni crear un grupo de recursos. Se puede usar el uso del grupo de recursos creado anteriormente para el clúster de Kubernetes.

## <a name="create-your-own-secretproviderclass-object"></a>Creación de su propio objeto SecretProviderClass

Use esta [plantilla](https://github.com/Azure/secrets-store-csi-driver-provider-azure/blob/master/examples/v1alpha1_secretproviderclass.yaml), que se proporciona para crear su propio objeto SecretProviderClass personalizado para proporcionar parámetros específicos del proveedor para el controlador Secrets Store CSI. Este objeto proporcionará acceso de identidad a Key Vault.

Utilice el archivo YAML SecretProviderClass de ejemplo que se proporciona para rellenar los parámetros que faltan. Se necesitan los siguientes parámetros:

1.  **userAssignedIdentityID:** identificador de cliente de la entidad de servicio.
1.  **keyvaultName:** nombre de la instancia de Key Vault.
1.  **objects:** Este objeto contendrá todo el contenido de los secretos que desea montar.
    1.  **objectName:** nombre del contenido del secreto.
    1.  **objectType:** tipo de objeto (secreto, clave o certificado).
1.  **resourceGroup:** nombre del grupo de recursos.
1.  **subscriptionId:** identificador de suscripción de Kay Vault.
1.  **tenantID:** identificador del inquilino (es decir, identificador de directorio) de Key Vault.

A continuación encontrará la plantilla actualizada, descárguela en forma de archivo.yaml y rellene los campos obligatorios correspondientes. En este ejemplo, el nombre de la instancia de Key Vault es **contosoKeyVault5** y tiene dos secretos, **secret1** y **secret2**.

**Nota:** Si va a usar identidades administradas, el valor del campo **usePodIdentity** debe ser **true** y hay que dejar el campo **userAssignedIdentityID** solo con comillas **""** . 

```yaml
apiVersion: secrets-store.csi.x-k8s.io/v1alpha1
kind: SecretProviderClass
metadata:
  name: azure-kvname
spec:
  provider: azure
  parameters:
    usePodIdentity: "false"                   # [REQUIRED] Set to "true" if using managed identities
    useVMManagedIdentity: "false"             # [OPTIONAL] if not provided, will default to "false"
    userAssignedIdentityID: "servicePrincipalClientID"       # [REQUIRED] If using a Service Principal, use the client id to specify which user assigned managed identity to use. If using a user assigned identity as the VM's managed identity, then specify the identity's client id. If empty, then defaults to use the system assigned identity on the VM
                                                             #     az ad sp show --id http://contosoServicePrincipal --query appId -o tsv
                                                             #     the above command will return the Client ID of your service principal
    keyvaultName: "contosoKeyVault5"          # [REQUIRED] the name of the Key Vault
                                              #     az keyvault show --name contosoKeyVault5
                                              #     the above command will displays the Key Vault metadata, which includes the subscription ID, resource group name, Key Vault 
    cloudName: ""                             # [OPTIONAL for Azure] if not provided, azure environment will default to AzurePublicCloud
    objects:  |
      array:
        - |
          objectName: secret1                 # [REQUIRED] object name
                                              #     az keyvault secret list --vault-name “contosoKeyVault5”
                                              #     the above command will display a list of secret names from your Key Vault
          objectType: secret                  # [REQUIRED] object types: secret, key or cert
          objectVersion: ""                   # [OPTIONAL] object versions, default to latest if empty
        - |
          objectName: secret2
          objectType: secret
          objectVersion: ""
    resourceGroup: "contosoResourceGroup"     # [REQUIRED] the resource group name of the Key Vault
    subscriptionId: "subscriptionID"          # [REQUIRED] the subscription ID of the Key Vault
    tenantId: "tenantID"                      # [REQUIRED] the tenant ID of the Key Vault
```
A continuación se muestra la salida de la consola de "az keyvault show --name contosoKeyVault5" con los metadatos resaltados relevantes:

![Imagen](../media/kubernetes-key-vault-2.png)

## <a name="assign-your-service-principal-or-use-managed-identities"></a>Asignación de una entidad de servicio o uso de identidades administradas

### <a name="using-service-principal"></a>Uso de una entidad de servicio

Si va a usar una entidad de servicio, deberá conceder permiso a la entidad de servicio para acceder a Key Vault y recuperar los secretos. Asigne el rol **"Lector"** y conceda a la entidad de servicio permiso para **"obtener"** secretos de Key Vault. Para ello, debe seguir estos pasos:

1. Asigne una entidad de servicio a una instancia de Key Vault. El parámetro **$AZURE_CLIENT_ID** es el valor de **appId** que copió después de crear la entidad de servicio.
    ```azurecli
    az role assignment create --role Reader --assignee $AZURE_CLIENT_ID --scope /subscriptions/$SUBID/resourcegroups/$KEYVAULT_RESOURCE_GROUP/providers/Microsoft.KeyVault/vaults/$KEYVAULT_NAME
    ```

    A continuación puede ver la salida del comando: 

    ![Imagen](../media/kubernetes-key-vault-3.png)

1. Conceda a la entidad de servicio permiso para obtener secretos:
    ```azurecli
    az keyvault set-policy -n $KEYVAULT_NAME --secret-permissions get --spn $AZURE_CLIENT_ID
    ```

1. Ahora que ha configurado la entidad de servicio para que tenga permiso para leer secretos de Key Vault. **$AZURE_CLIENT_SECRET** es la **contraseña** de la entidad de servicio. Agregue sus credenciales de entidad de servicio como un secreto de Kubernetes al que puede acceder el controlador Secrets Store CSI:
    ```azurecli
    kubectl create secret generic secrets-store-creds --from-literal clientid=$AZURE_CLIENT_ID --from-literal clientsecret=$AZURE_CLIENT_SECRET
    ```

**Nota:** Si recibe un error más adelante al implementar el pod de Kubernetes sobre un identificador de secreto de cliente no válido, es posible que tenga un identificador de secreto de cliente anterior que haya expirado o se haya restablecido. Para resolver este problema, elimine los secretos "secrets-store-creds" y cree uno con el identificador de secreto de cliente actual. Ejecute el siguiente comando para eliminar "secrets-store-creds":
```azurecli
kubectl delete secrets secrets-store-creds
```

Si olvidó el identificador de secreto de cliente de la entidad de servicio, puede restablecerlo con el siguiente comando:

```azurecli
az ad sp credential reset --name contosoServicePrincipal --credential-description "APClientSecret" --query password -o tsv
```

### <a name="using-managed-identities"></a>Uso de identidades administradas

Si usa identidades administradas, asigne roles específicos al clúster de AKS que ha creado. 
1. Para crear, enumerar o leer una identidad administrada asignada por el usuario, es preciso asignar el rol [Colaborador de Identidad administrada](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#managed-identity-contributor) al clúster de AKS. Asegúrese de que **$clientId** sea del clúster de Kubernetes.

    ```azurecli
    az role assignment create --role "Managed Identity Contributor" --assignee $clientId --scope /subscriptions/$SUBID/resourcegroups/$NODE_RESOURCE_GROUP
    
    az role assignment create --role "Virtual Machine Contributor" --assignee $clientId --scope /subscriptions/$SUBID/resourcegroups/$NODE_RESOURCE_GROUP
    ```

1. Instale la identidad de Azure Active Directory (Azure AD) en AKS.
    ```azurecli
    helm repo add aad-pod-identity https://raw.githubusercontent.com/Azure/aad-pod-identity/master/charts

    helm install pod-identity aad-pod-identity/aad-pod-identity
    ```

1. Cree una identidad de Azure AD. Copie **clientId** y **principalId**.
    ```azurecli
    az identity create -g $resourceGroupName -n $identityName
    ```

1. Asigne el rol Lector a la identidad de Azure AD que acaba de crear para Key Vault. Luego, conceda el permiso de identidad para obtener los secretos de Key Vault. Va a usar **clientId** y **principalId** de la identidad de Azure que acaba de crear.
    ```azurecli
    az role assignment create --role "Reader" --assignee $principalId --scope /subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/contosoResourceGroup/providers/Microsoft.KeyVault/vaults/contosoKeyVault5

    az keyvault set-policy -n contosoKeyVault5 --secret-permissions get --spn $clientId
    ```

## <a name="deploy-your-pod-with-mounted-secrets-from-key-vault"></a>Implementación de un pod con secretos montados desde Key Vault

El siguiente comando configurará el objeto SecretProviderClass:
```azurecli
kubectl apply -f secretProviderClass.yaml
```

### <a name="using-service-principal"></a>Uso de una entidad de servicio

Si va a usar una entidad de servicio, el siguiente comando implementará los pods de Kubernetes con los objetos SecretProviderClass y secrets-store-creds que configuró. Esta es la plantilla para la implementación en [Linux](https://github.com/Azure/secrets-store-csi-driver-provider-azure/blob/master/examples/nginx-pod-secrets-store-inline-volume-secretproviderclass.yaml) y [Windows](https://github.com/Azure/secrets-store-csi-driver-provider-azure/blob/master/examples/windows-pod-secrets-store-inline-volume-secret-providerclass.yaml).
```azurecli
kubectl apply -f updateDeployment.yaml
```

### <a name="using-managed-identities"></a>Uso de identidades administradas

Si va a usar identidades administradas, cree en el clúster un tipo**AzureIdentity** que haga referencia a la identidad que ha creado anteriormente. Luego, cree un tipo **AzureIdentityBinding** que haga referencia al tipo **AzureIdentity** que ha creado. Use la plantilla siguiente, rellene los parámetros correspondientes y guárdela como **podIdentityAndBinding.yaml**.  
```yml
apiVersion: aadpodidentity.k8s.io/v1
kind: AzureIdentity
metadata:
    name: "azureIdentityName"               # The name of your Azure Idenity
spec:
    type: 0                                 # Set type: 0 for Managed Service Identity
    resourceID: /subscriptions/<SUBID>/resourcegroups/<RESOURCEGROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<AZUREIDENTITYNAME>
    clientID: "managedIdentityClientId"     # The clientId of your Azure Identity that you created earlier
---
apiVersion: aadpodidentity.k8s.io/v1
kind: AzureIdentityBinding
metadata:
    name: azure-pod-identity-binding
spec:
    azureIdentity: "azureIdentityName"      # The name of your Azure Idenity
    selector: azure-pod-identity-binding-selector
```
    
Ejecute el siguiente comando para ejecutar el enlace:

```azurecli
kubectl apply -f podIdentityAndBinding.yaml
```

Después se realiza la implementación real del pod. A continuación se encuentra el archivo YAML de implementación que usará el enlace de la identidad del pod del último paso. Guarde el archivo como **podBindingDeployment.yaml**.

```yml
kind: Pod
apiVersion: v1
metadata:
    name: nginx-secrets-store-inline
    labels:
    aadpodidbinding: azure-pod-identity-binding-selector
spec:
    containers:
    - name: nginx
        image: nginx
        volumeMounts:
        - name: secrets-store-inline
        mountPath: "/mnt/secrets-store"
        readOnly: true
    volumes:
    - name: secrets-store-inline
        csi:
        driver: secrets-store.csi.k8s.io
        readOnly: true
        volumeAttributes:
            secretProviderClass: azure-kvname
```

Ejecute el siguiente comando para implementar el pod:

```azurecli
kubectl apply -f podBindingDeployment.yaml
```
### <a name="check-status-and-secret-content"></a>Comprobación del estado y del contenido del secreto 
Para mostrar los pods que ha creado:
```azurecli
kubectl get pods
```

Para comprobar el estado del trabajo, use el siguiente comando:
```azurecli
kubectl describe pod/nginx-secrets-store-inline
```

![Imagen](../media/kubernetes-key-vault-4.png)

El pod implementado debe estar en el estado "Running" (En ejecución). En la sección "Events" (Eventos) de la parte inferior, todos los tipos de eventos de la izquierdas están clasificados como "Normal” (Normales).
Una vez que haya comprobado que el pod se ejecuta, puede comprobar que tiene los secretos de su instancia de Key Vault.

Para mostrar todos los secretos que tiene el pod:
```azurecli
kubectl exec -it nginx-secrets-store-inline -- ls /mnt/secrets-store/
```

Para obtener el contenido de un secreto específico:
```azurecli
kubectl exec -it nginx-secrets-store-inline -- cat /mnt/secrets-store/secret1
```

Compruebe el contenido del secreto que se muestra.

## <a name="next-steps"></a>Pasos siguientes

Asegúrese de que el almacén de claves se pueda recuperar:
> [!div class="nextstepaction"]
> [Activación de la eliminación temporal](https://docs.microsoft.com/azure/key-vault/general/soft-delete-clid)
