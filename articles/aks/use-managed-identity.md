---
title: Uso de identidades administradas en Azure Kubernetes Service
description: Aprenda a utilizar identidades administradas en Azure Kubernetes Service (AKS)
services: container-service
author: mlearned
ms.topic: article
ms.date: 07/10/2020
ms.author: mlearned
ms.openlocfilehash: 95a303a4b6a83901560b26679bca920b9de4d3f4
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86250913"
---
# <a name="use-managed-identities-in-azure-kubernetes-service"></a>Uso de identidades administradas en Azure Kubernetes Service

Actualmente, un clúster de Azure Kubernetes Service (AKS) (específicamente, el proveedor de nube Kubernetes) requiere una identidad para crear recursos adicionales, como equilibradores de carga y discos administrados en Azure. Esta identidad puede ser una *identidad administrada* o una *entidad de servicio*. Si usa una [entidad de servicio](kubernetes-service-principal.md), debe indicarla, o bien AKS la creará automáticamente. Si usa la identidad administrada, AKS la creará automáticamente. Llega un momento en que los clústeres que usan entidades de servicio alcanzan un estado en el que se debe renovar la entidad de servicio para mantener el clúster en funcionamiento. La administración de entidades de servicio agrega complejidad, por lo que es más fácil usar identidades administradas. Se aplican los mismos requisitos de permisos tanto en las entidades de servicio como en las identidades administradas.

Las *identidades administradas* son básicamente un contenedor relacionado con las entidades de servicio y facilitan su administración. La rotación de credenciales para MI se produce automáticamente cada 46 días según el valor predeterminado de Azure Active Directory. AKS usa tipos de identidad administrados asignados por el sistema y asignados por el usuario. Estas identidades son inmutables actualmente. Para más información, consulte el tema sobre [identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md).

## <a name="before-you-begin"></a>Antes de empezar

Debe tener instalado el siguiente recurso:

- La CLI de Azure, versión 2.8.0 o posterior

## <a name="limitations"></a>Limitaciones

* La aportación de sus propias identidades administradas no se admite actualmente.
* Los clústeres de AKS con identidades administradas solo se pueden habilitar durante la creación del clúster.
* Los clústeres de AKS existentes no se pueden actualizar para habilitar las identidades administradas.
* Durante las operaciones de **actualización** del clúster, la identidad administrada no está disponible temporalmente.

## <a name="summary-of-managed-identities"></a>Resumen de identidades administradas

AKS usa varias identidades administradas para servicios integrados y complementos.

| Identidad                       | Nombre    | Caso de uso | Permisos predeterminados | Traiga su propia identidad
|----------------------------|-----------|----------|
| Plano de control | no visible | AKS lo usa para administrar los recursos de red, por ejemplo, crear un equilibrador de carga para la entrada, la dirección IP pública, etc.| Rol de colaborador para un grupo de recursos de nodo | No se admite actualmente.
| Kubelet | Nombre de clúster de AKS-agentpool | Autenticación con Azure Container Registry (ACR) | Rol de lector para el grupo de recursos de nodo | No se admite actualmente.
| Complemento | AzureNPM | No se requiere ninguna identidad | N/D | No
| Complemento | Supervisión de red AzureCNI | No se requiere ninguna identidad | N/D | No
| Complemento | azurepolicy (operador de control) | No se requiere ninguna identidad | N/D | No
| Complemento | azurepolicy | No se requiere ninguna identidad | N/D | No
| Complemento | Calico | No se requiere ninguna identidad | N/D | No
| Complemento | Panel | No se requiere ninguna identidad | N/D | No
| Complemento | HTTPApplicationRouting | Administra los recursos de red necesarios | Rol de lector para grupo de recursos de nodo, rol colaborador para zona DNS | No
| Complemento | Ingresar a la puerta de enlace de aplicaciones | Administra los recursos de red necesarios| Rol de colaborador para grupo de recursos de nodo | No
| Complemento | omsagent | Se usa para enviar métricas de AKS a Azure Monitor | Supervisión del rol del publicador de métricas | No
| Complemento | Nodo virtual (ACIConnector) | Administra los recursos de red necesarios para Azure Container Instances (ACI) | Rol de colaborador para grupo de recursos de nodo | No


## <a name="create-an-aks-cluster-with-managed-identities"></a>Creación de un clúster de AKS con identidades administradas

Ahora puede crear un clúster de AKS con identidades administradas utilizando los siguientes comandos de la CLI.

En primer lugar, cree un grupo de recursos de Azure:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location westus2
```

Después, cree un clúster de AKS:

```azurecli-interactive
az aks create -g myResourceGroup -n myManagedCluster --enable-managed-identity
```

La creación correcta de un clúster con identidades administradas contiene esta información de perfil de la entidad de servicio:

```json
"servicePrincipalProfile": {
    "clientId": "msi"
  }
```

Use el siguiente comando para consultar el objectId de la identidad administrada del plano de control:

```azurecli-interactive
az aks show -g myResourceGroup -n MyManagedCluster --query "identity"
```

El resultado debería tener este aspecto:

```json
{
  "principalId": "<object_id>",   
  "tenantId": "<tenant_id>",      
  "type": "SystemAssigned"                                 
}
```

> [!NOTE]
> Para crear y usar su propia red virtual, una dirección IP estática o un disco de Azure conectado en el que los recursos estén fuera del grupo de recursos del nodo de trabajo, use el PrincipalID del sistema de clúster asignado a la identidad administrada para realizar una asignación de roles. Para obtener más información sobre la asignación, consulte [Delegación del acceso a otros recursos de Azure](kubernetes-service-principal.md#delegate-access-to-other-azure-resources).
>
> Las concesiones de permisos a la identidad administrada de clúster que el proveedor en la nube de Azure utiliza pueden tardar hasta 60 minutos en rellenarse.

Por último, obtenga credenciales para acceder al clúster:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster
```

El clúster se creará en pocos minutos. Después, puede implementar las cargas de trabajo de la aplicación en el nuevo clúster e interactuar con él del mismo modo que con los clústeres de AKS basados en una entidad de servicio.

## <a name="next-steps"></a>Pasos siguientes
* Use las [plantillas de Azure Resource Manager (ARM) ][aks-arm-template] para crear clústeres habilitados para identidades administradas.

<!-- LINKS - external -->
[aks-arm-template]: /azure/templates/microsoft.containerservice/managedclusters
