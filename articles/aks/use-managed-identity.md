---
title: Uso de identidades administradas en Azure Kubernetes Service
description: Aprenda a utilizar identidades administradas en Azure Kubernetes Service (AKS)
services: container-service
author: saudas
manager: saudas
ms.topic: article
ms.date: 04/02/2020
ms.author: saudas
ms.openlocfilehash: 00ecc077ba55ab9f91fc58f8a47fcdf7440deea6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82112973"
---
# <a name="use-managed-identities-in-azure-kubernetes-service"></a>Uso de identidades administradas en Azure Kubernetes Service

Actualmente, un clúster de Azure Kubernetes Service (AKS) (específicamente, el proveedor de servicios en la nube de Kubernetes) requiere una identidad para crear recursos adicionales, como equilibradores de carga y discos administrados en Azure. Esta identidad puede ser una *identidad administrada* o una *entidad de servicio*. Si usa una [entidad de servicio](kubernetes-service-principal.md), debe indicarla, o bien AKS la creará automáticamente. Si usa la identidad administrada, AKS la creará automáticamente. Llega un momento en que los clústeres que usan entidades de servicio alcanzan un estado en el que se debe renovar la entidad de servicio para mantener el clúster en funcionamiento. La administración de entidades de servicio agrega complejidad, por lo que es más fácil usar identidades administradas. Se aplican los mismos requisitos de permisos tanto en las entidades de servicio como en las identidades administradas.

Las *identidades administradas* son básicamente un contenedor relacionado con las entidades de servicio y facilitan su administración. Para más información, consulte el tema sobre [identidades administradas para recursos de Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

AKS crea dos identidades administradas:

- **Identidad administrada asignada por el sistema**: identidad que utiliza el proveedor de servicios en la nube de Kubernetes para crear recursos de Azure en nombre del usuario. El ciclo de vida de la identidad asignada por el sistema está vinculado al del clúster. La identidad se elimina cuando se elimina el clúster.
- **Identidad administrada asignada por el usuario**: la identidad que se usa para la autorización en el clúster. Por ejemplo, la identidad asignada por el usuario se usa para autorizar a AKS a usar instancias de Azure Container Registry (ACR) o para autorizar al kubelet a obtener metadatos de Azure.

Los complementos también se autentican con una identidad administrada. Para cada complemento, AKS crea una identidad administrada, que se mantiene durante la vida del complemento. 

## <a name="before-you-begin"></a>Antes de empezar

Debe tener instalado el siguiente recurso:

- La CLI de Azure, versión 2.2.0 o posterior

## <a name="create-an-aks-cluster-with-managed-identities"></a>Creación de un clúster de AKS con identidades administradas

Ahora puede crear un clúster de AKS con identidades administradas utilizando los siguientes comandos de la CLI.

En primer lugar, cree un grupo de recursos de Azure:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location westus2
```

Después, cree un clúster de AKS:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyManagedCluster --enable-managed-identity
```

La creación correcta de un clúster con identidades administradas contiene esta información de perfil de la entidad de servicio:

```json
"servicePrincipalProfile": {
    "clientId": "msi",
    "secret": null
  }
```

> [!NOTE]
> Para crear y usar su propia red virtual, una dirección IP estática o un disco de Azure conectado donde los recursos estén fuera del grupo de recursos MC_*, use el valor de PrincipalID de la identidad administrada asignada por el sistema del clúster para realizar una asignación de roles. Para obtener más información sobre la asignación, consulte [Delegación del acceso a otros recursos de Azure](kubernetes-service-principal.md#delegate-access-to-other-azure-resources).
>
> Las concesiones de permisos a la identidad administrada de clúster que el proveedor en la nube de Azure utiliza pueden tardar hasta 60 minutos en rellenarse.

Por último, obtenga credenciales para acceder al clúster:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster
```

El clúster se creará en pocos minutos. Después, puede implementar las cargas de trabajo de la aplicación en el nuevo clúster e interactuar con él del mismo modo que con los clústeres de AKS basados en una entidad de servicio.

> [!IMPORTANT]
>
> - Los clústeres de AKS con identidades administradas solo se pueden habilitar durante la creación del clúster.
> - Los clústeres de AKS existentes no se pueden actualizar para habilitar las identidades administradas.
