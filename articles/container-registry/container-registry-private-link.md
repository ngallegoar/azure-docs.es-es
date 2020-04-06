---
title: Configuración de vínculo privado
description: Configure un punto de conexión privado en un registro de contenedor y habilite un vínculo privado en una red virtual local
ms.topic: article
ms.date: 03/10/2020
ms.openlocfilehash: de8228d84497e71f24dba3dd4e6162cb6735a8c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79498922"
---
# <a name="configure-azure-private-link-for-an-azure-container-registry"></a>Configuración de Azure Private Link para un registro de contenedor de Azure 

Puede configurar un [punto de conexión privado](../private-link/private-endpoint-overview.md) para el registro de contenedor de Azure de modo que los clientes de una red virtual de Azure accedan de forma segura al registro a través de un [vínculo privado](../private-link/private-link-overview.md). El punto de conexión privado usa una dirección IP del espacio de direcciones de la red virtual para el registro. El tráfico de red entre los clientes de la red virtual y el registro atraviesa la red virtual y un vínculo privado de la red troncal de Microsoft, lo que elimina la exposición a la red pública de Internet.

Puede [establecer la configuración DNS](../private-link/private-endpoint-overview.md#dns-configuration) del punto de conexión privado de modo que la configuración se resuelva en la dirección IP privada asignada del registro. Con la configuración DNS, los clientes y servicios de la red pueden seguir accediendo al registro en el nombre de dominio completo de este, como *myregistry.azurecr.io*.

Esta característica está disponible en el nivel de servicio de un registro de contenedor **Premium**. Para obtener información sobre los límites y niveles de servicio de registro, consulte [SKU de Azure Container Registry](container-registry-skus.md).

> [!IMPORTANT]
> Esta funcionalidad actualmente está en versión preliminar y hay algunas [limitaciones](#preview-limitations). Las versiones preliminares están a su disposición con la condición de que acepte los [términos de uso adicionales][terms-of-use]. Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general.

## <a name="preview-limitations"></a>Limitaciones de vista previa

* De momento, no se puede configurar un vínculo privado con un punto de conexión privado en un [registro con replicación geográfica](container-registry-geo-replication.md). 

## <a name="prerequisites"></a>Prerrequisitos

* Para usar los pasos de la CLI de Azure de este artículo, se recomienda la versión 2.2.0 o posterior de la CLI de Azure. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][azure-cli]. O bien ejecute en [Azure Cloud Shell](../cloud-shell/quickstart.md).
* Si aún no tiene un registro de contenedor, créelo (se requiere un nivel Premium) e inserte una imagen de ejemplo, como `hello-world`, desde Docker Hub. Por ejemplo, use [Azure Portal][quickstart-portal] o la [CLI de Azure][quickstart-cli] para crear un registro.
* Si quiere configurar el acceso al Registro mediante un vínculo privado en otra suscripción de Azure, tiene que registrar el proveedor de recursos para Azure Container Registry en esa suscripción. Por ejemplo:

  ```azurecli
  az account set --subscription <Name or ID of subscription of private link>

  az provider register --namespace Microsoft.ContainerRegistry
  ``` 

En los ejemplos de la CLI de Azure de este artículo se usan las siguientes variables de entorno. Sustituya los valores adecuados para el entorno. Todos los ejemplos tienen el formato del shell de Bash:

```bash
registryName=<container-registry-name>
registryLocation=<container-registry-location> # Azure region such as westeurope where registry created
resourceGroup=<resource-group-name>
vmName=<virtual-machine-name>
```

## <a name="create-a-docker-enabled-virtual-machine"></a>Creación de una máquina virtual con funcionalidad Docker

Para fines de prueba, use una máquina virtual Ubuntu con funcionalidad Docker para acceder a un registro de contenedor de Azure. Para usar la autenticación de Azure Active Directory en el registro, instale también la [CLI de Azure][azure-cli] en la máquina virtual. Si ya tiene una máquina virtual de Azure, omita este paso.

Puede usar el mismo grupo de recursos para la máquina virtual y el registro de contenedor. Esta configuración simplifica la limpieza al final, pero no es necesaria. Si elige crear un grupo de recursos independiente para la máquina virtual y la red virtual, ejecute [az group create][az-group-create]:

```azurecli
az group create --name $resourceGroup --location $registryLocation
```

Ahora, implemente una máquina virtual de Azure con Ubuntu mediante [az vm create][az-vm-create]. En el ejemplo siguiente se crea una máquina virtual denominada *myDockerVM*.

```azurecli
az vm create \
  --resource-group $resourceGroup \
  --name $vmName \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

La máquina virtual tarda unos minutos en crearse. Cuando se complete el comando, anote el valor `publicIpAddress` mostrado por la CLI de Azure. Use esta dirección para establecer conexiones SSH con la máquina virtual.

### <a name="install-docker-on-the-vm"></a>Instalación de Docker en la máquina virtual

Después de ejecutar la máquina virtual, establezca una conexión SSH con la máquina virtual. Reemplace *publicIpAddress* por la dirección IP pública de la máquina virtual.

```bash
ssh azureuser@publicIpAddress
```

Ejecute los siguientes comandos para instalar Docker en la máquina virtual Ubuntu:

```bash
sudo apt-get update
sudo apt install docker.io -y
```

Después de la instalación, ejecute el siguiente comando para comprobar que Docker se ejecute correctamente en la máquina virtual:

```bash
sudo docker run -it hello-world
```

Salida:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
[...]
```

### <a name="install-the-azure-cli"></a>Instalación de la CLI de Azure

Siga los pasos del artículo [Instalación de la CLI de Azure con apt](/cli/azure/install-azure-cli-apt?view=azure-cli-latest) para instalar la CLI de Azure en la máquina virtual de Ubuntu. Por ejemplo:

```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

Salga de la conexión SSH.

## <a name="set-up-private-link---cli"></a>Configuración de vínculo privado: CLI

### <a name="get-network-and-subnet-names"></a>Obtención de nombres de red y subred

Si aún no los tiene, necesita los nombres de una red virtual y una subred para configurar un vínculo privado. En este ejemplo se usa la misma subred para la máquina virtual y el punto de conexión privado del registro. Pero en muchos escenarios se configuraría el punto de conexión en una subred independiente. 

Al crear una máquina virtual, Azure crea de manera predeterminada crea una red virtual en el mismo grupo de recursos. El nombre de dicha red virtual se basa en el nombre de la máquina virtual. Por ejemplo, si asigna a la máquina virtual el nombre *myDockerVM*, el nombre predeterminado de la red virtual es *myDockerVMVNET*, con una subred llamada *myDockerVMSubnet*. Establezca estos valores en variables de entorno al ejecutar el comando [az network vnet list][az-network-vnet-list]:

```azurecli
networkName=$(az network vnet list \
  --resource-group $resourceGroup \
  --query '[].{Name: name}' --output tsv)

subnetName=$(az network vnet list \
  --resource-group $resourceGroup \
  --query '[].{Subnet: subnets[0].name}' --output tsv)

echo networkName=$networkName
echo subnetName=$subnetName
```

### <a name="disable-network-policies-in-subnet"></a>Deshabilitación de directivas de red en subred

[Deshabilite las directivas de red](../private-link/disable-private-endpoint-network-policy.md), como los grupos de seguridad de red de la subred, para el punto de conexión privado. Actualice la configuración de subred con [az network vnet subnet update][az-network-vnet-subnet-update]:

```azurecli
az network vnet subnet update \
 --name $subnetName \
 --vnet-name $networkName \
 --resource-group $resourceGroup \
 --disable-private-endpoint-network-policies
```

### <a name="configure-the-private-dns-zone"></a>Configuración de la zona DNS privada

Cree una zona DNS privada para el dominio del registro de contenedor de Azure privado. En pasos posteriores se crean registros DNS para el dominio del registro dentro de esta zona DNS.

Para usar una zona privada con el fin de invalidar la resolución DNS predeterminada del registro de contenedor de Azure, la zona debe tener el nombre **privatelink.azurecr.io**. Ejecute el comando siguiente [az network private-dns zone create][az-network-private-dns-zone-create] para crear la zona privada:

```azurecli
az network private-dns zone create \
  --resource-group $resourceGroup \
  --name "privatelink.azurecr.io"
```

### <a name="create-an-association-link"></a>Creación de un vínculo de asociación

Ejecute [az network private-dns link vnet create][az-network-private-dns-link-vnet-create] para asociar la zona privada con la red virtual. En este ejemplo se crea un vínculo denominado *myDNSLink*.

```azurecli
az network private-dns link vnet create \
  --resource-group $resourceGroup \
  --zone-name "privatelink.azurecr.io" \
  --name MyDNSLink \
  --virtual-network $networkName \
  --registration-enabled false
```

### <a name="create-a-private-registry-endpoint"></a>Creación de un punto de conexión de registro privado

En esta sección se crea el punto de conexión privado del registro en la red virtual. En primer lugar, obtenga el identificador de recurso del registro:

```azurecli
registryID=$(az acr show --name $registryName \
  --query 'id' --output tsv)
```

Ejecute el comando [az network private-endpoint create][az-network-private-endpoint-create] para crear el punto de conexión privado del registro.

En el ejemplo siguiente se crea el punto de conexión *myPrivateEndpoint* y la conexión de servicio *myConnection*. Para especificar un recurso de registro de contenedor para el punto de conexión, pase `--group-ids registry`:

```azurecli
az network private-endpoint create \
    --name myPrivateEndpoint \
    --resource-group $resourceGroup \
    --vnet-name $networkName \
    --subnet $subnetName \
    --private-connection-resource-id $registryID \
    --group-ids registry \
    --connection-name myConnection
```

### <a name="get-private-ip-addresses"></a>Obtención de direcciones IP privadas

Ejecute [az network private-endpoint show][az-network-private-endpoint-show] para consultar al punto de conexión sobre el identificador de la interfaz de red:

```azurecli
networkInterfaceID=$(az network private-endpoint show \
  --name myPrivateEndpoint \
  --resource-group $resourceGroup \
  --query 'networkInterfaces[0].id' \
  --output tsv)
```

La interfaz de red está asociada a dos direcciones IP privadas del registro de contenedor: una del propio registro y otra del punto de conexión de datos del registro. Ejecute los siguientes comandos [az resource show][az-resource-show] para obtener las direcciones IP privadas del registro de contenedor y el punto de conexión de datos del registro:

```azurecli
privateIP=$(az resource show \
  --ids $networkInterfaceID \
  --api-version 2019-04-01 --query 'properties.ipConfigurations[1].properties.privateIPAddress' \
  --output tsv)

dataEndpointPrivateIP=$(az resource show \
  --ids $networkInterfaceID \
  --api-version 2019-04-01 \
  --query 'properties.ipConfigurations[0].properties.privateIPAddress' \
  --output tsv)
```

### <a name="create-dns-records-in-the-private-zone"></a>Creación de registros DNS en la zona privada

Los siguientes comandos crean registros DNS en la zona privada para el punto de conexión del registro y su punto de conexión de datos. Por ejemplo, si tiene un registro denominado *myregistry* en la región *westeurope*, los nombres de punto de conexión son `myregistry.azurecr.io` y `myregistry.westeurope.data.azurecr.io`. 

En primer lugar, ejecute [az network private-dns record-set a create][az-network-private-dns-record-set-a-create] para crear conjuntos de registros A vacíos para el punto de conexión del registro y el punto de conexión de datos:

```azurecli
az network private-dns record-set a create \
  --name $registryName \
  --zone-name privatelink.azurecr.io \
  --resource-group $resourceGroup

# Specify registry region in data endpoint name
az network private-dns record-set a create \
  --name ${registryName}.${registryLocation}.data \
  --zone-name privatelink.azurecr.io \
  --resource-group $resourceGroup
```

Ejecute el comando [az network private-dns record-set a add-record][az-network-private-dns-record-set-a-add-record] para crear registros A para el punto de conexión del registro y el punto de conexión de datos:

```azurecli
az network private-dns record-set a add-record \
  --record-set-name $registryName \
  --zone-name privatelink.azurecr.io \
  --resource-group $resourceGroup \
  --ipv4-address $privateIP

# Specify registry region in data endpoint name
az network private-dns record-set a add-record \
  --record-set-name ${registryName}.${registryLocation}.data \
  --zone-name privatelink.azurecr.io \
  --resource-group $resourceGroup \
  --ipv4-address $dataEndpointPrivateIP
```

El vínculo privado ya está configurado y listo para su uso.

## <a name="set-up-private-link---portal"></a>Configuración de vínculo privado: portal

En los pasos siguientes se da por hecho que ya tiene una red virtual y una subred configuradas con una máquina virtual para pruebas. También puede [crear una nueva red virtual y una subred](../virtual-network/quick-create-portal.md).

### <a name="create-a-private-endpoint"></a>Creación de un punto de conexión privado

1. En el portal, vaya al registro de contenedor.
1. En **Configuración**, seleccione **Conexiones de punto de conexión privado (versión preliminar)** .
1. Seleccione **+ Punto de conexión privado**.
1. En la pestaña **Datos básicos**, escriba o seleccione la siguiente información:

    | Configuración | Value |
    | ------- | ----- |
    | **Detalles del proyecto** | |
    | Subscription | Seleccione su suscripción. |
    | Resource group | Escriba el nombre de un grupo existente o cree uno nuevo.|
    | **Detalles de instancia** |  |
    | Nombre | Escriba un nombre único. |
    |Region|Seleccione una región.|
    |||
5. Seleccione **Siguiente: Resource** (Siguiente: Recurso).
6. Escriba o seleccione la siguiente información:

    | Configuración | Value |
    | ------- | ----- |
    |Método de conexión  | Seleccione **Conectarse a un recurso de Azure en mi directorio**.|
    | Subscription| Seleccione su suscripción. |
    | Tipo de recurso | Seleccione **Microsoft.ContainerRegistry/registries**. |
    | Resource |Seleccione el nombre del registro.|
    |Subrecurso de destino |Seleccione **registro**.|
    |||
7. Seleccione **Siguiente: Configuration** (Siguiente: Configuración).
8. Escriba o seleccione la información:

    | Configuración | Value |
    | ------- | ----- |
    |**Redes**| |
    | Virtual network| Seleccione la red virtual en la que está implementada la máquina virtual, como *myDockerVMVNET*. |
    | Subnet | Seleccione una subred, como *myDockerVMSubnet*, en la que está implementada la máquina virtual. |
    |**Integración de DNS privado**||
    |Integración con una zona DNS privada |Seleccione **Sí**. |
    |Zona DNS privada |Seleccione *(Nuevo) privatelink.azurecr.io*. |
    |||

1. Seleccione **Revisar + crear**. Se le remitirá a la página **Revisar y crear**, donde Azure validará la configuración. 
2. Cuando reciba el mensaje **Validación superada**, seleccione **Crear**.

Una vez creado el punto de conexión privado, la configuración DNS de la zona privada aparece en la página **Información general** del punto de conexión.

![Configuración DNS del punto de conexión](./media/container-registry-private-link/private-endpoint-overview.png)

El vínculo privado ya está configurado y listo para su uso.

## <a name="validate-private-link-connection"></a>Validación de una conexión de vínculo privado

Debe validar que los recursos de la subred del punto de conexión privado se conectan al registro mediante una dirección IP privada y que tienen la integración correcta de la zona DNS privada.

Para validar la conexión de vínculo privado, use SSH en la máquina virtual configurada en la red virtual.

Ejecute el comando `nslookup` para resolver la dirección IP del registro a través del vínculo privado:

```bash
nslookup $registryName.azurecr.io
```

La salida del ejemplo muestra la dirección IP del registro en el espacio de direcciones de la subred:

```console
[...]
myregistry.azurecr.io       canonical name = myregistry.privatelink.azurecr.io.
Name:   myregistry.privatelink.azurecr.io
Address: 10.0.0.6
```

Compare este resultado con la dirección IP pública de la salida `nslookup` para el mismo registro a través de un punto de conexión público:

```console
[...]
Non-authoritative answer:
Name:   myregistry.westeurope.cloudapp.azure.com
Address: 40.78.103.41
```

### <a name="registry-operations-over-private-link"></a>Operaciones del registro a través de un vínculo privado

Además, compruebe que puede realizar operaciones del registro desde la máquina virtual de la subred. Cree una conexión SSH con la máquina virtual y ejecute [az acr login][az-acr-login] para iniciar sesión en el registro. En función de la configuración de la máquina virtual, es posible que tenga que agregar el prefijo `sudo` a los siguientes comandos.

```bash
az acr login --name $registryName
```

Realice operaciones del registro como `docker pull` para extraer una imagen de ejemplo del registro. Sustituya `hello-world:v1` por una imagen y la etiqueta pertinente para el registro, y anteponga el nombre del servidor de inicio de sesión del registro (todo en minúsculas):

```bash
docker pull myregistry.azurecr.io/hello-world:v1
``` 

Docker extrae correctamente la imagen a la máquina virtual.

## <a name="manage-private-endpoint-connections"></a>Administración de conexiones de punto de conexión privado

Administre las conexiones del punto de conexión privado del registro mediante Azure Portal o los comandos del grupo de comandos [az acr private-endpoint-connection][az-acr-private-endpoint-connection]. Las operaciones incluyen aprobar, eliminar, enumerar, rechazar o mostrar detalles de las conexiones del punto de conexión privado del registro.

Por ejemplo, para enumerar las conexiones del punto de conexión privado de un registro, ejecute el comando [az acr private-endpoint-connection list][az-acr-private-endpoint-connection-list]. Por ejemplo:

```azurecli
az acr private-endpoint-connection list \
  --registry-name $registryName 
```

Al configurar una conexión de punto de conexión privado mediante los pasos de este artículo, el registro acepta automáticamente las conexiones de los clientes y servicios que tienen permisos RBAC en el registro. Puede configurar el punto de conexión de modo que exija la aprobación manual de conexiones. Para obtener información sobre cómo aprobar y rechazar conexiones de punto de conexión privado, vea [Administrar una conexión de punto de conexión privado](../private-link/manage-private-endpoint.md).

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ha creado todos los recursos de Azure en el mismo grupo de recursos y ya no los necesita, puede eliminarlos con el comando [az group delete](/cli/azure/group):

```azurecli
az group delete --name $resourceGroup
```

Para limpiar los recursos en el portal, vaya al grupo de recursos. Una vez que se ha cargado el grupo de recursos, haga clic en **Eliminar grupo de recursos** para quitar el grupo de recursos y los recursos almacenados en él.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre Private Link, consulte la documentación de [Azure Private Link](../private-link/private-link-overview.md).
* Una alternativa al vínculo privado es configurar reglas de acceso a red para restringir el acceso al registro. Para obtener más información, vea [Restricción del acceso a un registro de contenedor de Azure mediante una red virtual de Azure o reglas de firewall](container-registry-vnet.md).

<!-- LINKS - external -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-acr-repository-show]: /cli/azure/acr/repository#az-acr-repository-show
[az-acr-repository-list]: /cli/azure/acr/repository#az-acr-repository-list
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-private-endpoint-connection]: /cli/azure/acr/private-endpoint-connection
[az-acr-private-endpoint-connection-list]: /cli/azure/acr/private-endpoint-connection#az-acr-private-endpoint-connection-list
[az-acr-private-endpoint-connection-approve]: /cli/azure/acr/private-endpoint-connection#az-acr-private-endpoint-connection-approve
[az-group-create]: /cli/azure/group
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-vm-create]: /cli/azure/vm#az-vm-create
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-show
[az-network-vnet-subnet-update]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-update
[az-network-vnet-list]: /cli/azure/network/vnet/#az-network-vnet-list
[az-network-private-endpoint-create]: /cli/azure/network/private-endpoint#az-network-private-endpoint-create
[az-network-private-endpoint-show]: /cli/azure/network/private-endpoint#az-network-private-endpoint-show
[az-network-private-dns-zone-create]: /cli/azure/network/private-dns/zone#az-network-private-dns-zone-create
[az-network-private-dns-link-vnet-create]: /cli/azure/network/private-dns/link/vnet#az-network-private-dns-link-vnet-create
[az-network-private-dns-record-set-a-create]: /cli/azure/network/private-dns/record-set/a#az-network-private-dns-record-set-a-create
[az-network-private-dns-record-set-a-add-record]: /cli/azure/network/private-dns/record-set/a#az-network-private-dns-record-set-a-add-record
[az-resource-show]: /cli/azure/resource#az-resource-show
[quickstart-portal]: container-registry-get-started-portal.md
[quickstart-cli]: container-registry-get-started-azure-cli.md
[azure-portal]: https://portal.azure.com
