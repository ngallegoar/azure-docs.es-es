---
title: Personalización de rutas definidas por el usuario (UDR) en Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo definir una ruta de salida personalizada en Azure Kubernetes Service (AKS)
services: container-service
ms.topic: article
ms.date: 03/16/2020
ms.openlocfilehash: e7dbde4095fb635180bb1ba663734f8dbfd602f7
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2020
ms.locfileid: "82733505"
---
# <a name="customize-cluster-egress-with-a-user-defined-route-preview"></a>Personalización de la salida de un clúster con una ruta definida por el usuario (versión preliminar)

La salida de un clúster de AKS se puede personalizar para escenarios específicos. De forma predeterminada, AKS aprovisionará una SKU de Load Balancer Estándar, que se configurará y se usará para la salida. Sin embargo, es posible que la configuración predeterminada no cumpla los requisitos de todos los escenarios si no se permiten direcciones IP públicas o se requieren saltos adicionales para la salida.

En este artículo se explica cómo personalizar la ruta de salida de un clúster para admitir escenarios de red personalizados, como los que no permiten direcciones IP públicas y requieren que el clúster se encuentre detrás de un dispositivo virtual de red (NVA).

> [!IMPORTANT]
> Las características en vista previa de AKS están disponibles como opción de participación y autoservicio. Las versiones preliminares se proporcionan *tal cual* y *como están disponibles*, y están excluidas del Acuerdo de Nivel de Servicio y la garantía limitada. Las versiones preliminares de AKS reciben cobertura parcial del soporte al cliente *en la medida de lo posible*. Por tanto, estas características no están diseñadas para su uso en producción. Para más información, consulte los siguientes artículos de soporte:
>
> * [Directivas de soporte técnico para AKS](support-policies.md)
> * [Preguntas más frecuentes de soporte técnico de Azure](faq.md)

## <a name="prerequisites"></a>Prerrequisitos
* CLI de Azure, versión 2.0.81 o posterior.
* Extensión de la CLI de Azure (versión preliminar) 0.4.28 o superior
* Versión de API `2020-01-01` o posterior

## <a name="install-the-latest-azure-cli-aks-preview-extension"></a>Instalación de la extensión de la versión preliminar de AKS de la CLI de Azure más reciente
Para establecer el tipo de salida de un clúster, necesita la versión 0.4.18 o posterior de la extensión de la CLI de Azure para AKS (versión preliminar). Instale la extensión de la CLI de Azure para AKS (versión preliminar) con el comando az extension add y, después, busque las actualizaciones disponibles con el comando az extension update:

```azure-cli
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

## <a name="limitations"></a>Limitaciones
* Durante la versión preliminar, `outboundType` solo se puede definir en el momento de crear el clúster y no se puede actualizar después.
* Durante la versión preliminar, los clústeres de AKS `outboundType` deben usar Azure CNI. Kubernetes se puede configurar y su uso requiere asociaciones manuales de la tabla de rutas a la subred de AKS.
* La configuración de `outboundType` requiere clústeres de AKS con un valor de `vm-set-type` de `VirtualMachineScaleSets`, y un valor de `load-balancer-sku` de `Standard`.
* Para configurar `outboundType` en un valor de `UDR`, se necesita una ruta definida por el usuario con conectividad de salida válida para el clúster.
* Configurar `outboundType` en un valor de `UDR` implica que la IP de origen de la entrada enrutada al equilibrador de carga **no puede coincidir** con la dirección de destino de la salida del clúster.

## <a name="overview-of-outbound-types-in-aks"></a>Introducción a los tipos de salida en AKS

Un clúster de AKS se puede personalizar con un valor de `outboundType` único del tipo de equilibrador de carga o enrutamiento definido por el usuario.

> [!IMPORTANT]
> El tipo de salida afecta solo al tráfico de salida del clúster. Consulte cómo [configurar los controladores de entrada](ingress-basic.md) para más información.

### <a name="outbound-type-of-loadbalancer"></a>Tipo de salida de loadBalancer

Si se establece `loadBalancer`, AKS completa automáticamente la configuración siguiente. El equilibrador de carga se usa para la salida a través de una dirección IP pública asignada a AKS. El tipo de salida de `loadBalancer` admite los servicios de Kubernetes del tipo `loadBalancer`, que esperan la salida del equilibrador de carga creado por el proveedor de recursos de AKS.

AKS realiza la siguiente configuración.
   * Se aprovisiona una dirección IP pública para la salida del clúster.
   * La dirección IP pública se asocia al recurso de equilibrador de carga.
   * Se configuran los grupos de back-end del equilibrador de carga para los nodos del agente en el clúster.

A continuación se muestra una topología de red implementada en clústeres de AKS de forma predeterminada, que usa un valor de `outboundType` de `loadBalancer`.

![outboundtype-lb](media/egress-outboundtype/outboundtype-lb.png)

### <a name="outbound-type-of-userdefinedrouting"></a>Tipo de salida de userDefinedRouting

> [!NOTE]
> El uso del tipo de salida es un escenario de redes avanzado y requiere una configuración de red adecuada.

Si se establece `userDefinedRouting`, AKS no configurará automáticamente las rutas de salida. Se espera que **el usuario** haga lo siguiente.

El clúster de AKS debe implementarse en una red virtual existente con una subred que se haya configurado. Al usar la arquitectura de Standard Load Balancer (SLB), debe establecer salidas explícitas. Para ello, es necesario el envío de solicitudes de salida a un dispositivo como, por ejemplo, un firewall o una puerta de enlace en el entorno local, o permitir que una dirección IP pública asignada a Standard Load Balancer o a un nodo determinado realice la salida.

El proveedor de recursos de AKS implementará un equilibrador de carga estándar (SLB). El equilibrador de carga no está configurado con ninguna regla y [no incurre en ningún cargo hasta que se coloca una regla](https://azure.microsoft.com/pricing/details/load-balancer/). AKS **no** aprovisiona automáticamente una dirección IP pública para el servidor front-end del equilibrador de carga estándar. AKS **no** configura automáticamente el grupo de back-end del equilibrador de carga.

## <a name="deploy-a-cluster-with-outbound-type-of-udr-and-azure-firewall"></a>Implementación de un clúster con el tipo de salida UDR y Azure Firewall

Para ilustrar la aplicación de un clúster con un tipo de salida que usa una ruta definida por el usuario, se puede configurar un clúster en una red virtual emparejada con un firewall de Azure.

![Detalle de la topología](media/egress-outboundtype/outboundtype-udr.png)

* La entrada se inserta en el flujo mediante filtros de firewall.
   * Una subred aislada contiene un equilibrador de carga interno para el enrutamiento a los nodos de agente.
   * Los nodos de agente están aislados en una subred dedicada.
* Las solicitudes de salida parten de los nodos del agente hacia el la dirección IP interna del firewall de Azure mediante una ruta definida por el usuario.
   * Las solicitudes de los nodos de agente de AKS siguen una UDR que se ha colocado en la subred en la que se implementó el clúster de AKS.
   * El firewall de Azure sale de la red virtual de un front-end con una IP pública.
   * El acceso al plano de control de AKS está protegido por un grupo de seguridad de red, que tiene habilitada la dirección IP del servidor front-end del firewall.
   * El acceso a la red pública de Internet u otros servicios de Azure fluye hacia y desde la dirección IP del servidor front-end del firewall.

### <a name="set-configuration-via-environment-variables"></a>Configuración mediante variables de entorno

Defina el conjunto de variables de entorno que se usarán en la creación de recursos.

```bash
PREFIX="contosofin"
RG="${PREFIX}-rg"
LOC="eastus"
NAME="${PREFIX}outboundudr"
AKS_NAME="${PREFIX}aks"
VNET_NAME="${PREFIX}vnet"
AKSSUBNET_NAME="${PREFIX}akssubnet"
SVCSUBNET_NAME="${PREFIX}svcsubnet"
# DO NOT CHANGE FWSUBNET_NAME - This is currently a requirement for Azure Firewall.
FWSUBNET_NAME="AzureFirewallSubnet"
FWNAME="${PREFIX}fw"
FWPUBLICIP_NAME="${PREFIX}fwpublicip"
FWIPCONFIG_NAME="${PREFIX}fwconfig"
FWROUTE_TABLE_NAME="${PREFIX}fwrt"
FWROUTE_NAME="${PREFIX}fwrn"
FWROUTE_NAME_INTERNET="${PREFIX}fwinternet"
DEVSUBNET_NAME="${PREFIX}dev"
```

A continuación, establezca los identificadores de suscripción.

```azure-cli
# Get ARM Access Token and Subscription ID - This will be used for AuthN later.

ACCESS_TOKEN=$(az account get-access-token -o tsv --query 'accessToken')

# NOTE: Update Subscription Name
# Set Default Azure Subscription to be Used via Subscription ID

az account set -s <SUBSCRIPTION_ID_GOES_HERE>

# NOTE: Update Subscription Name for setting SUBID

SUBID=$(az account show -s '<SUBSCRIPTION_NAME_GOES_HERE>' -o tsv --query 'id')
```

## <a name="create-a-virtual-network-with-multiple-subnets"></a>Creación de una red virtual con varias subredes

Aprovisione una red virtual con tres subredes independientes, una para el clúster, otra para el firewall y otra para la entrada del servicio.

![Topología de red vacía](media/egress-outboundtype/empty-network.png)

Cree el grupo de recursos que contendrá todos los recursos.

```azure-cli
# Create Resource Group

az group create --name $RG --location $LOC
```

Cree dos redes virtuales para hospedar el clúster de AKS y el firewall de Azure. Cada uno tendrá su propia subred. Comencemos con la red de AKS.

```
# Dedicated virtual network with AKS subnet

az network vnet create \
    --resource-group $RG \
    --name $VNET_NAME \
    --address-prefixes 100.64.0.0/16 \
    --subnet-name $AKSSUBNET_NAME \
    --subnet-prefix 100.64.1.0/24

# Dedicated subnet for K8s services

az network vnet subnet create \
    --resource-group $RG \
    --vnet-name $VNET_NAME \
    --name $SVCSUBNET_NAME \
    --address-prefix 100.64.2.0/24

# Dedicated subnet for Azure Firewall (Firewall name cannot be changed)

az network vnet subnet create \
    --resource-group $RG \
    --vnet-name $VNET_NAME \
    --name $FWSUBNET_NAME \
    --address-prefix 100.64.3.0/24
```

## <a name="create-and-setup-an-azure-firewall-with-a-udr"></a>Creación y configuración de un firewall de Azure con una UDR

Deben configurarse las reglas de entrada y salida del firewall de Azure. El propósito principal del firewall es que las organizaciones puedan configurar reglas de tráfico de entrada y salida pormenorizadas hacia y desde el clúster de AKS.

![Firewall y UDR](media/egress-outboundtype/firewall-udr.png)

Cree un recurso de SKU estándar con IP pública que se usará como dirección de front-end del firewall de Azure.

```azure-cli
az network public-ip create -g $RG -n $FWPUBLICIP_NAME -l $LOC --sku "Standard"
```

Registre la extensión de la CLI en versión preliminar para crear un firewall de Azure.
```azure-cli
# Install Azure Firewall preview CLI extension

az extension add --name azure-firewall

# Deploy Azure Firewall

az network firewall create -g $RG -n $FWNAME -l $LOC
```

La dirección IP creada anteriormente puede asignarse ahora al front-end del firewall.
> [!NOTE]
> La configuración de la dirección IP pública al firewall de Azure puede tardar unos minutos.
> 
> Si se reciben errores repetidamente en el comando siguiente, elimine el firewall existente y la dirección IP pública, y aprovisione la dirección IP pública y el firewall de Azure en el portal al mismo tiempo.

```azure-cli
# Configure Firewall IP Config

az network firewall ip-config create -g $RG -f $FWNAME -n $FWIPCONFIG_NAME --public-ip-address $FWPUBLICIP_NAME --vnet-name $VNET_NAME
```

Cuando el comando anterior se haya realizado correctamente, anote la dirección IP del servidor front-end del firewall para la configuración posterior.

```bash
# Capture Firewall IP Address for Later Use

FWPUBLIC_IP=$(az network public-ip show -g $RG -n $FWPUBLICIP_NAME --query "ipAddress" -o tsv)
FWPRIVATE_IP=$(az network firewall show -g $RG -n $FWNAME --query "ipConfigurations[0].privateIpAddress" -o tsv)
```

### <a name="create-a-udr-with-a-hop-to-azure-firewall"></a>Creación de una UDR con un salto al firewall de Azure

Azure enruta automáticamente el tráfico entre redes locales, las redes virtuales y las subredes de Azure. Si desea cambiar algún enrutamiento predeterminado de Azure, debe crear una tabla de rutas.

Cree una tabla de rutas vacía para asociarla a una subred determinada. La tabla de rutas definirá el próximo salto como el firewall de Azure creado anteriormente. Cada subred puede tener cero o una ruta de tablas de ruta asociada.

```azure-cli
# Create UDR and add a route for Azure Firewall

az network route-table create -g $RG --name $FWROUTE_TABLE_NAME
az network route-table route create -g $RG --name $FWROUTE_NAME --route-table-name $FWROUTE_TABLE_NAME --address-prefix 0.0.0.0/0 --next-hop-type VirtualAppliance --next-hop-ip-address $FWPRIVATE_IP --subscription $SUBID
az network route-table route create -g $RG --name $FWROUTE_NAME_INTERNET --route-table-name $FWROUTE_TABLE_NAME --address-prefix $FWPUBLIC_IP/32 --next-hop-type Internet
```

Consulte en la [documentación sobre las tablas de rutas de red virtual](../virtual-network/virtual-networks-udr-overview.md#user-defined) cómo invalidar las rutas del sistema predeterminadas de Azure o agregar rutas adicionales a la tabla de rutas de una subred.

## <a name="adding-network-firewall-rules"></a>Adición de reglas de firewall de red virtual

> [!WARNING]
> A continuación se muestra un ejemplo de cómo agregar una regla de firewall. Las reglas del firewall de la aplicación deben habilitar todos los puntos de conexión de salida definidos en los [puntos de conexión de salida necesarios](egress.md) para que los clústeres de AKS funcionen. Si estos puntos de conexión no están habilitados, el clúster no puede funcionar.

El siguiente es un ejemplo de una regla de red y de aplicación. Agregamos una regla de red que permite cualquier protocolo, dirección de origen, dirección de destino y puerto de destino. También agregamos una regla de aplicación para **algunos** de los puntos de conexión requeridos por AKS.

En un escenario de producción, solo se debe habilitar el acceso a los puntos de conexión necesarios para la aplicación y los definidos en la [salida requerida por AKS](egress.md).

```
# Add Network FW Rules

az network firewall network-rule create -g $RG -f $FWNAME --collection-name 'aksfwnr' -n 'netrules' --protocols 'Any' --source-addresses '*' --destination-addresses '*' --destination-ports '*' --action allow --priority 100

# Add Application FW Rules
# IMPORTANT: Add AKS required egress endpoints

az network firewall application-rule create -g $RG -f $FWNAME \
    --collection-name 'AKS_Global_Required' \
    --action allow \
    --priority 100 \
    -n 'required' \
    --source-addresses '*' \
    --protocols 'http=80' 'https=443' \
    --target-fqdns \
        'aksrepos.azurecr.io' \
        '*blob.core.windows.net' \
        'mcr.microsoft.com' \
        '*cdn.mscr.io' \
        '*.data.mcr.microsoft.com' \
        'management.azure.com' \
        'login.microsoftonline.com' \
        'ntp.ubuntu.com' \
        'packages.microsoft.com' \
        'acs-mirror.azureedge.net'
```

Para más información acerca del servicio Azure Firewall, consulte la [documentación de Azure Firewall](https://docs.microsoft.com/azure/firewall/overview).

## <a name="associate-the-route-table-to-aks"></a>Asociación de la tabla de rutas a AKS

Para asociar el clúster al firewall, la subred dedicada del clúster debe hacer referencia a la tabla de rutas creada anteriormente. Para crear la asociación, se puede emitir un comando a la red virtual que contiene el clúster y el firewall para actualizar la tabla de rutas de la subred del clúster.

```azure-cli
# Associate route table with next hop to Firewall to the AKS subnet

az network vnet subnet update -g $RG --vnet-name $VNET_NAME --name $AKSSUBNET_NAME --route-table $FWROUTE_TABLE_NAME
```

## <a name="deploy-aks-with-outbound-type-of-udr-to-the-existing-network"></a>Implementación de AKS con el tipo de salida UDR a la red existente

Ahora, un clúster de AKS se puede implementar en una red virtual existente. Para establecer el tipo de salida del clúster en enrutamiento definido por el usuario, se debe proporcionar una subred existente a AKS.

![aks-deploy](media/egress-outboundtype/outboundtype-udr.png)

### <a name="create-a-service-principal-with-access-to-provision-inside-the-existing-virtual-network"></a>Creación de una entidad de servicio con acceso de aprovisionamiento dentro de la red virtual existente

AKS usa una entidad de servicio para crear recursos de clúster. La entidad de servicio que se pasa en el momento de la creación se usa para crear recursos de AKS subyacentes, como las máquinas virtuales, el almacenamiento y los equilibradores de carga que AKS usa. Si se le conceden pocos permisos, no podrá aprovisionar un clúster de AKS.

```azure-cli
# Create SP and Assign Permission to Virtual Network

az ad sp create-for-rbac -n "${PREFIX}sp" --skip-assignment
```

Ahora, reemplace `APPID` y `PASSWORD` a continuación por el identificador de aplicación y la contraseña de la entidad de servicio, generados automáticamente por la salida del comando anterior. Haremos referencia al identificador de recurso de red virtual para conceder los permisos a la entidad de servicio para que AKS pueda implementar recursos en ella.

```azure-cli
APPID="<SERVICE_PRINCIPAL_APPID_GOES_HERE>"
PASSWORD="<SERVICEPRINCIPAL_PASSWORD_GOES_HERE>"
VNETID=$(az network vnet show -g $RG --name $VNET_NAME --query id -o tsv)

# Assign SP Permission to VNET

az role assignment create --assignee $APPID --scope $VNETID --role Contributor

# View Role Assignment
az role assignment list --assignee $APPID --all -o table
```

### <a name="deploy-aks"></a>Implementación de AKS

Por último, el clúster de AKS se puede implementar en la subred existente que hemos dedicado al clúster. La subred de destino en la que se va a realizar la implementación se define con la variable de entorno `$SUBNETID`. No definimos la variable `$SUBNETID` en los pasos anteriores. Para establecer el valor del id. de la subred, puede usar el comando siguiente:

```azurecli
SUBNETID="/subscriptions/$SUBID/resourceGroups/$RG/providers/Microsoft.Network/virtualNetworks/$VNET_NAME/subnets/$AKSSUBNET_NAME"
```

Definiremos el tipo de salida para que siga la UDR que existe en la subred. Esto permite a AKS omitir la configuración y el aprovisionamiento de IP para el equilibrador de carga, que ahora puede ser estrictamente interno.

Se puede agregar la característica de [intervalos de IP autorizados del servidor de API](api-server-authorized-ip-ranges.md) de AKS para limitar el acceso del servidor de API solo al punto de conexión público del firewall. La característica de intervalos de IP autorizados se indica en el diagrama como el grupo de seguridad de red que se debe pasar para tener acceso al plano de control. Al habilitar la característica de intervalos de IP autorizados para limitar el acceso del servidor de API, las herramientas de desarrollo deben usar una JumpBox desde la red virtual del firewall, o bien debe agregar todos los puntos de conexión de desarrollador al intervalo de direcciones IP autorizado.

> [!TIP]
> Se pueden agregar otras características a la implementación del clúster, como (Clúster privado)[]. Cuando se usan intervalos de IP autorizados, se necesita una JumpBox dentro de la red de clústeres para acceder al servidor de API.

```azure-cli
az aks create -g $RG -n $AKS_NAME -l $LOC \
  --node-count 3 \
  --network-plugin azure --generate-ssh-keys \
  --service-cidr 192.168.0.0/16 \
  --dns-service-ip 192.168.0.10 \
  --docker-bridge-address 172.22.0.1/29 \
  --vnet-subnet-id $SUBNETID \
  --service-principal $APPID \
  --client-secret $PASSWORD \
  --load-balancer-sku standard \
  --outbound-type userDefinedRouting \
  --api-server-authorized-ip-ranges $FWPUBLIC_IP
  ```

### <a name="enable-developer-access-to-the-api-server"></a>Habilitación del acceso de desarrollador al servidor de API

Debido a la configuración de intervalos de IP autorizados para el clúster, debe agregar las direcciones IP de las herramientas de desarrollador a la lista de intervalos de IP autorizados del clúster de AKS para poder acceder al servidor de API. Otra opción consiste en configurar una JumpBox con las herramientas necesarias en una subred independiente de la red virtual del firewall.

Agrege otra dirección IP a los intervalos autorizados con el siguiente comando:

```bash
# Retrieve your IP address
CURRENT_IP=$(dig @resolver1.opendns.com ANY myip.opendns.com +short)

# Add to AKS approved list
az aks update -g $RG -n $AKS_NAME --api-server-authorized-ip-ranges $CURRENT_IP/32

```

 Use el comando [az aks get-credentials][az-aks-get-credentials] para configurar `kubectl` para conectarse a su clúster de Kubernetes recién creado. 

 ```azure-cli
 az aks get-credentials -g $RG -n $AKS_NAME
 ```

### <a name="setup-the-internal-load-balancer"></a>Configuración del equilibrador de carga interno

AKS ha implementado un equilibrador de carga con el clúster, que se puede configurar como [equilibrador de carga interno](internal-lb.md).

Para crear un equilibrador de carga interno, cree un manifiesto de servicio llamado internal-lb.yaml, con el tipo de servicio LoadBalancer y la anotación azure-load-balancer-internal, tal y como se muestra en el siguiente ejemplo:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
    service.beta.kubernetes.io/azure-load-balancer-internal-subnet: "contosofinsvcsubnet"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: internal-app
```

Implemente el equilibrador de carga interno con el comando kubectl apply y especifique el nombre del manifiesto de YAML:

```bash
kubectl apply -f internal-lb.yaml
```

## <a name="deploy-a-kubernetes-service"></a>Implementación de un clúster de Kubernetes

Como el tipo de salida del clúster está establecido en UDR, AKS no realiza automáticamente la asociación de los nodos de agente como grupo de back-end para el equilibrador de carga en el momento de la creación del clúster. Sin embargo, el proveedor de nube de Azure Kubernetes controla la asociación del grupo de back-end cuando se implementa el servicio Kubernetes.

Para implementar aplicación de votación de Azure, copie el código YAML siguiente en un archivo llamado `example.yaml`.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-back
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-back
        image: redis
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-front
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:v1
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
    service.beta.kubernetes.io/azure-load-balancer-internal-subnet: "contosofinsvcsubnet"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Para implementar el servicio, ejecute:

```bash
kubectl apply -f example.yaml
```

## <a name="add-a-dnat-rule-to-azure-firewall"></a>Adición de una regla de DNAT al firewall de Azure

Para configurar la conectividad de entrada, se debe escribir una regla de DNAT en el firewall de Azure. Para probar la conectividad con el clúster, se define una regla para la dirección IP pública de front-end del firewall que se va a enrutar a la dirección IP interna expuesta por el servicio interno.

La dirección de destino se puede personalizar porque es el puerto del firewall al que se va a tener acceso. La dirección traducida debe ser la dirección IP del equilibrador de carga interno. El puerto traducido debe ser el puerto expuesto para el servicio Kubernetes.

Tendrá que especificar la dirección IP interna asignada al equilibrador de carga creado por el servicio Kubernetes. Para recuperar la dirección, ejecute:

```bash
kubectl get services
```

La dirección IP necesaria se mostrará en la columna EXTERNAL-IP y será similar a la siguiente.

```bash
NAME               TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
azure-vote-back    ClusterIP      192.168.92.209   <none>        6379/TCP       23m
azure-vote-front   LoadBalancer   192.168.19.183   100.64.2.5    80:32106/TCP   23m
kubernetes         ClusterIP      192.168.0.1      <none>        443/TCP        4d3h
```

```azure-cli
az network firewall nat-rule create --collection-name exampleset --destination-addresses $FWPUBLIC_IP --destination-ports 80 --firewall-name $FWNAME --name inboundrule --protocols Any --resource-group $RG --source-addresses '*' --translated-port 80 --action Dnat --priority 100 --translated-address <INSERT IP OF K8s SERVICE>
```

## <a name="clean-up-resources"></a>Limpieza de recursos

> [!NOTE]
> Al eliminar el servicio interno Kubernetes, si ningún servicio usa el equilibrador de carga interno, el proveedor de la nube de Azure lo eliminará. En la siguiente implementación del servicio, se implementará un equilibrador de carga si no se encuentra ninguno con la configuración solicitada.

Para limpiar los recursos de Azure, elimine el grupo de recursos de AKS.

```azure-cli
az group delete -g $RG
```

## <a name="validate-connectivity"></a>Validar conectividad

Vaya a la dirección IP de front-end del firewall de Azure en un explorador para validar la conectividad.

Verá una imagen de la aplicación de votación de Azure.

## <a name="next-steps"></a>Pasos siguientes

Consulte [Información general de redes de Microsoft Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview).

Consulte [cómo crear, modificar o eliminar una tabla de rutas](https://docs.microsoft.com/azure/virtual-network/manage-route-table).

<!-- LINKS - internal -->
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
