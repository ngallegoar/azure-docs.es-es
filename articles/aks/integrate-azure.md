---
title: Integración con los servicios administrados de Azure mediante Open Service Broker for Azure (OSBA)
description: Integración con los servicios administrados de Azure mediante Open Service Broker for Azure (OSBA)
author: zr-msft
ms.topic: overview
ms.date: 12/05/2017
ms.author: zarhoads
ms.openlocfilehash: 2eddedea7d626a92e21442c81aa49e00491958a1
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "78273018"
---
# <a name="integrate-with-azure-managed-services-using-open-service-broker-for-azure-osba"></a>Integración con los servicios administrados de Azure mediante Open Service Broker for Azure (OSBA)

Junto con [Kubernetes Service Catalog][kubernetes-service-catalog] (catálogo de servicios de Kubernetes), Open Service Broker for Azure (OSBA) permite a los desarrolladores usar servicios de Azure administrados en Kubernetes. Esta guía se centra en la implementación del catálogo de servicios de Kubernetes, Open Service Broker for Azure (OSBA) y las aplicaciones que utilizan servicios administrados de Azure mediante Kubernetes.

## <a name="prerequisites"></a>Prerequisites
* Una suscripción de Azure

* CLI de Azure: puede [instalarla localmente][azure-cli-install] o usarla en [Azure Cloud Shell][azure-cloud-shell].

* Helm CLI 2.7+: puede [instalarla localmente][helm-cli-install] o usarla en [Azure Cloud Shell][azure-cloud-shell].

* Permisos para crear una entidad de servicio con el rol de Colaborador en la suscripción de Azure

* Un clúster de Azure Kubernetes Service (AKS) existente. Si necesita un clúster de AKS, siga la guía de inicio rápido [Creación de un clúster de AKS][create-aks-cluster].

## <a name="install-service-catalog"></a>Instalación del catálogo de servicios

El primer paso consiste en instalar el catálogo de servicios en el clúster de Kubernetes mediante un gráfico de Helm.

Vaya a [https://shell.azure.com](https://shell.azure.com) para abrir Cloud Shell en el explorador.

Actualice la instalación de Tiller (servidor de Helm) en el clúster con:

```console
helm init --upgrade
```

Ahora, agregue el gráfico del catálogo de servicios al repositorio de Helm:

```console
helm repo add svc-cat https://svc-catalog-charts.storage.googleapis.com
```

Por último, instale el catálogo de servicios con el gráfico de Helm. Si el clúster tiene RBAC habilitado, ejecute este comando.

```console
helm install svc-cat/catalog --name catalog --namespace catalog --set apiserver.storage.etcd.persistence.enabled=true --set apiserver.healthcheck.enabled=false --set controllerManager.healthcheck.enabled=false --set apiserver.verbosity=2 --set controllerManager.verbosity=2
```

Si el clúster no tiene RBAC habilitado, ejecute este comando.

```console
helm install svc-cat/catalog --name catalog --namespace catalog --set rbacEnable=false --set apiserver.storage.etcd.persistence.enabled=true --set apiserver.healthcheck.enabled=false --set controllerManager.healthcheck.enabled=false --set apiserver.verbosity=2 --set controllerManager.verbosity=2
```

Una vez se ha ejecutado el gráfico de Helm, compruebe que `servicecatalog` aparece en la salida del comando siguiente:

```console
kubectl get apiservice
```

Por ejemplo, debería ver una salida similar a la del siguiente ejemplo (aquí se muestra truncado):

```output
NAME                                 AGE
v1.                                  10m
v1.authentication.k8s.io             10m
...
v1beta1.servicecatalog.k8s.io        34s
v1beta1.storage.k8s.io               10
```

## <a name="install-open-service-broker-for-azure"></a>Instalación de Open Service Broker para Azure

El paso siguiente consiste en instalar [Open Service Broker para Azure][open-service-broker-azure], que incluye el catálogo para los servicios administrados de Azure. Algunos ejemplos de servicios de Azure disponibles son Azure Database for PostgreSQL, Azure Database for MySQL y Azure SQL Database.

Empiece agregando Open Service Broker al repositorio de Azure Helm:

```console
helm repo add azure https://kubernetescharts.blob.core.windows.net/azure
```

Cree una [entidad de servicio][create-service-principal] con el siguiente comando de la CLI de Azure:

```azurecli-interactive
az ad sp create-for-rbac
```

La salida debe ser similar a la siguiente: Tome nota de los valores `appId`, `password` y `tenant`, que se utilizan en el paso siguiente.

```json
{
  "appId": "7248f250-0000-0000-0000-dbdeb8400d85",
  "displayName": "azure-cli-2017-10-15-02-20-15",
  "name": "http://azure-cli-2017-10-15-02-20-15",
  "password": "77851d2c-0000-0000-0000-cb3ebc97975a",
  "tenant": "72f988bf-0000-0000-0000-2d7cd011db47"
}
```

Establezca las siguientes variables de entorno con los valores anteriores:

```console
AZURE_CLIENT_ID=<appId>
AZURE_CLIENT_SECRET=<password>
AZURE_TENANT_ID=<tenant>
```

Ahora obtenga el identificador de la suscripción a Azure:

```azurecli-interactive
az account show --query id --output tsv
```

De nuevo, establezca la siguiente variable de entorno con el valor anterior:

```console
AZURE_SUBSCRIPTION_ID=[your Azure subscription ID from above]
```

Ahora que ha rellenado estas variables de entorno, ejecute el siguiente comando para instalar Open Service Broker para Azure mediante el gráfico de Helm:

```azurecli-interactive
helm install azure/open-service-broker-azure --name osba --namespace osba \
    --set azure.subscriptionId=$AZURE_SUBSCRIPTION_ID \
    --set azure.tenantId=$AZURE_TENANT_ID \
    --set azure.clientId=$AZURE_CLIENT_ID \
    --set azure.clientSecret=$AZURE_CLIENT_SECRET
```

Una vez completada la implementación de OSBA, instale la [CLI del catálogo de servicios][service-catalog-cli], una interfaz de línea de comandos fácil de usar para realizar consultas de Service Broker, clases de servicio, planes de servicio y mucho más.

Ejecute los comandos siguientes para instalar la CLI binaria del catálogo de servicios:

```console
curl -sLO https://servicecatalogcli.blob.core.windows.net/cli/latest/$(uname -s)/$(uname -m)/svcat
chmod +x ./svcat
```

Ahora, se enumeran los Service Brokers instalados:

```console
./svcat get brokers
```

Debería ver un resultado similar al siguiente:

```output
  NAME                               URL                                STATUS
+------+--------------------------------------------------------------+--------+
  osba   http://osba-open-service-broker-azure.osba.svc.cluster.local   Ready
```

A continuación, se enumeran las clases de servicio disponibles. Las clases de servicio mostradas son los servicios administrados de Azure disponibles que se pueden aprovisionar mediante Open Service Broker para Azure.

```console
./svcat get classes
```

Por último, se enumeran todos los planes de servicio disponibles. Los planes de servicio son los niveles de servicio para los servicios administrados de Azure. Por ejemplo, para Azure Database for MySQL, los planes abarcan de `basic50` para el nivel básico con 50 unidades de transacción de base de datos (DTU), a `standard800` para el nivel estándar con 800 DTU.

```console
./svcat get plans
```

## <a name="install-wordpress-from-helm-chart-using-azure-database-for-mysql"></a>Instalación de WordPress desde el gráfico de Helm mediante Azure Database for MySQL

En este paso, se utilizará Helm para instalar un gráfico de Helm actualizado para WordPress. El gráfico proporciona una instancia externa de Azure Database for MySQL que WordPress puede usar. Este proceso puede tardar unos minutos.

```console
helm install azure/wordpress --name wordpress --namespace wordpress --set resources.requests.cpu=0 --set replicaCount=1
```

Para comprobar que la instalación dispone de los recursos adecuados, enumere las instancias de servicio instaladas y los enlaces:

```console
./svcat get instances -n wordpress
./svcat get bindings -n wordpress
```

Enumere los secretos instalados:

```console
kubectl get secrets -n wordpress -o yaml
```

## <a name="next-steps"></a>Pasos siguientes

Al seguir este artículo, implementó el catálogo de servicios en un clúster de Azure Kubernetes Service (AKS). Usó Open Service Broker for Azure para implementar una instalación de WordPress que use servicios administrados de Azure, en este caso, Azure Database for MySQL.

Consulte el repositorio de [Azure/helm-charts][helm-charts] para acceder a otros gráficos de Helm basados en OSBA. Si está interesado en crear sus propios gráficos que funcionen con OSBA, consulte [Creación de un nuevo gráfico][helm-create-new-chart].

<!-- LINKS - external -->
[helm-charts]: https://github.com/Azure/helm-charts
[helm-cli-install]: https://docs.helm.sh/helm/#helm-install
[helm-create-new-chart]: https://github.com/Azure/helm-charts#creating-a-new-chart
[kubernetes-service-catalog]: https://github.com/kubernetes-incubator/service-catalog
[open-service-broker-azure]: https://github.com/Azure/open-service-broker-azure
[service-catalog-cli]: https://github.com/Azure/service-catalog-cli

<!-- LINKS - internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cloud-shell]: ../cloud-shell/overview.md
[create-aks-cluster]: ./kubernetes-walkthrough.md
[create-service-principal]: ./kubernetes-service-principal.md
