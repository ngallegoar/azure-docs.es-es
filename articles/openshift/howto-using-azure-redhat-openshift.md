---
title: Creación de un clúster de Red Hat OpenShift 4.3 en Azure | Microsoft Docs
description: Creación de un clúster con Red Hat OpenShift 4.3 en Azure
author: lamek
ms.author: suvetriv
ms.service: container-service
ms.topic: conceptual
ms.date: 03/06/2020
keywords: aro, openshift, az aro, red hat, cli
ms.openlocfilehash: f909c5870be6e394e457ad8f44ea5a253054ffe6
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81398888"
---
# <a name="create-access-and-manage-an-azure-red-hat-openshift-43-cluster"></a>Creación, acceso y administración de un clúster de Red Hat OpenShift 4.3 en Azure

> [!IMPORTANT]
> Tenga en cuenta que Red Hat OpenShift 4.3 en Azure solo está disponible actualmente en versión preliminar privada en la región Este de EE. UU y Este de EE. UU. 2. La aceptación de la versión preliminar privada es solo por invitación. Asegúrese de registrar la suscripción antes de intentar habilitar esta característica: [Registro de la versión preliminar privada de Red Hat OpenShift en Azure](https://aka.ms/aro-preview-register)

> [!NOTE]
> Las características en versión preliminar son de autoservicio y se proporcionan tal cual y como están disponibles, y están excluidas del Acuerdo de Nivel de Servicio y la garantía limitada. Por tanto, estas características no están diseñadas para su uso en producción.

## <a name="prerequisites"></a>Prerrequisitos

Necesitará lo siguiente para crear un clúster de Red Hat OpenShift 4.3 en Azure:

- CLI de Azure, versión 2.0.72 o posterior
  
- La extensión "az aro"

- Una red virtual que contenga dos subredes vacías, sin ningún grupo de seguridad de red asociado a ninguna de ellas.  El clúster se implementará en estas subredes.

- Una aplicación de AAD de clúster (identificador de cliente y secreto) y una entidad de servicio, o permisos de AAD suficientes para `az aro create` para crear una aplicación de AAD y una entidad de servicio automáticamente.

- La entidad de servicio de RP y la del clúster deben tener cada una el rol Colaborador en la red virtual del clúster.  Si tiene el rol "Administrador de acceso de usuario" en la red virtual, `az aro create` configurará automáticamente las asignaciones de roles.

### <a name="install-the-az-aro-extension"></a>Instalación de la extensión "az aro"
La extensión `az aro` permite crear, acceder y eliminar clústeres de Red Hat OpenShift en Azure directamente desde la línea de comandos mediante la CLI de Azure.

> [!Note] 
> Actualmente, la extensión `az aro` se encuentra en versión preliminar. Esto se puede cambiar o eliminar en una versión futura.
> Para participar en la versión preliminar de la extensión `az aro`, debe registrar el proveedor de recursos `Microsoft.RedHatOpenShift`.
> 
>    ```console
>    az provider register -n Microsoft.RedHatOpenShift --wait
>    ```

1. Inicie sesión en Azure.

   ```console
   az login
   ```

2. Para instalar la extensión `az aro`, ejecute el comando siguiente:

   ```console
   az extension add -n aro --index https://az.aroapp.io/preview
   ```

3. Compruebe que la extensión ARO está registrada.

   ```console
   az -v
   ...
   Extensions:
   aro                                0.3.0
   ...
   ```

### <a name="get-a-red-hat-pull-secret-optional"></a>Obtención de un secreto de extracción de Red Hat (opcional)

El secreto de extracción de Red Hat permite al clúster acceder a los registros de contenedor de Red Hat y a contenido adicional. El uso de un secreto de extracción es opcional, pero recomendable.

Para obtener el secreto de extracción:

1. Ir a https://cloud.redhat.com/openshift/install/azure/aro-provisioned.
1. Inicie sesión en su cuenta de Red Hat, o bien cree una nueva cuenta de Red Hat con su correo electrónico empresarial y acepte los términos y condiciones.
1. Seleccione **Download pull secret** (Descargar secreto de extracción).

Guarde el archivo *pull-secret.txt* en algún lugar seguro; usará el archivo cada vez que cree un clúster.

### <a name="create-a-virtual-network-containing-two-empty-subnets"></a>Creación de una red virtual que contenga dos subredes vacías

Siga estos pasos para crear una red virtual que contenga dos subredes vacías.

1. Establezca las siguientes variables.

   ```console
   LOCATION=eastus        #the location of your cluster
   RESOURCEGROUP="v4-$LOCATION"    #the name of the resource group where you want to create your cluster
   CLUSTER=cluster        #the name of your cluster
   ```

2. Cree un grupo de recursos para el clúster.

   ```console
   az group create -g "$RESOURCEGROUP" -l $LOCATION
   ```

3. Creación de la red virtual.

   ```console
   az network vnet create \
     -g "$RESOURCEGROUP" \
     -n vnet \
     --address-prefixes 10.0.0.0/9 \
     >/dev/null
   ```

4. Agregue dos subredes vacías a la red virtual.

   ```console
   for subnet in "$CLUSTER-master" "$CLUSTER-worker"; do
     az network vnet subnet create \
       -g "$RESOURCEGROUP" \
       --vnet-name vnet \
       -n "$subnet" \
       --address-prefixes 10.$((RANDOM & 127)).$((RANDOM & 255)).0/24 \
       --service-endpoints Microsoft.ContainerRegistry \
       >/dev/null
   done
   ```

5. Deshabilite las directivas de red para el servicio Private Link en la red virtual y las subredes. Este es un requisito para que el servicio ARO acceda al clúster y lo administre.

   ```console
   az network vnet subnet update \
     -g "$RESOURCEGROUP" \
     --vnet-name vnet \
     -n "$CLUSTER-master" \
     --disable-private-link-service-network-policies true \
     >/dev/null
   ```

## <a name="create-a-cluster"></a>Crear un clúster

Ejecute el siguiente comando para crear un clúster.

```console
az aro create \
  -g "$RESOURCEGROUP" \
  -n "$CLUSTER" \
  --vnet vnet \
  --master-subnet "$CLUSTER-master" \
  --worker-subnet "$CLUSTER-worker" \
  --cluster-resource-group "aro-$CLUSTER" \
  --domain "$CLUSTER" \
  --pull-secret @pull-secret.txt
```

>[!NOTE]
> Se tarda aproximadamente 35 minutos en crear un clúster.

## <a name="access-the-cluster-console"></a>Acceso a la consola del clúster

Puede encontrar la dirección URL de la consola del clúster (con el formato `https://console-openshift-console.apps.<random>.<location>.aroapp.io/`) en el recurso de clúster de Red Hat OpenShift 4.3 en Azure. Para ver el recurso, ejecute el siguiente comando:

```console
az aro list -o table
```

Puede iniciar sesión en el clúster con el usuario `kubeadmin`.  Ejecute el siguiente comando para buscar la contraseña del usuario `kubeadmin`:

```dotnetcli
az aro list-credentials -g "$RESOURCEGROUP" -n "$CLUSTER"
```

## <a name="delete-a-cluster"></a>Eliminación de un clúster

Ejecute el comando siguiente para eliminar un clúster.

```console
az aro delete -g "$RESOURCEGROUP" -n "$CLUSTER"

# (optional)
for subnet in "$CLUSTER-master" "$CLUSTER-worker"; do
  az network vnet subnet delete -g "$RESOURCEGROUP" --vnet-name vnet -n "$subnet"
done
```
