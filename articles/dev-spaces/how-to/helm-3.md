---
title: Configuración del clúster de Azure Dev Spaces para que use Helm 3 (versión preliminar)
services: azure-dev-spaces
ms.date: 02/28/2020
ms.topic: conceptual
description: Aprenda a configurar el clúster de Dev Spaces para que use Helm 3.
keywords: Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores
ms.openlocfilehash: dbccb2618fd5a27805261d60e7891d920e0bc372
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79454302"
---
# <a name="configure-your-azure-dev-spaces-cluster-to-use-helm-3-preview"></a>Configuración del clúster de Azure Dev Spaces para que use Helm 3 (versión preliminar)

Azure Dev Spaces usa Helm 2 de forma predeterminada para instalar servicios de usuario en espacios de desarrollo en el clúster de AKS. Puede habilitar Azure Dev Spaces para usar Helm 3 en lugar de Helm 2 si instala servicios de usuario en espacios de desarrollo. Independientemente de la versión de Helm que Azure Dev Spaces use para instalar los servicios de usuario, puede seguir usando el cliente Helm 2 o 3 para administrar sus propias versiones en el mismo clúster.

Cuando se habilita Helm 3, Azure Dev Spaces se comporta de forma diferente al instalar los servicios de usuario en los espacios de desarrollo de las siguientes maneras:

* Tiller ya no se implementa en el clúster en el espacio de nombres *azds*.
* Helm almacena la información de versión en el espacio de nombres donde se instala un servicio en lugar del espacio de nombres *azds*.
* La información de la versión de Helm 3 permanece en el espacio de nombres donde se instala un servicio después de que se elimine un controlador.
* Puede interactuar directamente con cualquier versión administrada por Azure Dev Spaces en el clúster mediante el cliente de Helm 3.

En esta guía, obtendrá información sobre cómo habilitar Helm 3 para Azure Dev Spaces para instalar servicios de usuario en espacios de desarrollo.

> [!IMPORTANT]
> Esta funcionalidad actualmente está en su versión preliminar. Las versiones preliminares están a su disposición con la condición de que acepte los [términos de uso adicionales](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general.

## <a name="before-you-begin"></a>Antes de empezar

### <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si no tiene una suscripción a Azure, puede crear una [cuenta gratuita](https://azure.microsoft.com/free).
* [La CLI de Azure instalada][azure-cli].

### <a name="register-the-helm3preview-preview-feature"></a>Registro de la característica en vista previa Helm3Preview

A fin de habilitar Azure Dev Spaces para usar Helm 3 para instalar los servicios de usuario en los espacios de desarrollo, primero habilite la marca de características *Helm3Preview* en su suscripción con el comando *az feature register*:

> [!WARNING]
> Todo clúster de AKS en el que habilite Azure Dev Spaces con la marca de características *Helm3Preview* usará esta experiencia de versión preliminar. Para seguir habilitando instancias de Azure Dev Spaces totalmente compatibles en clústeres de AKS, no habilite características en vista previa en las suscripciones de producción. Utilice una suscripción independiente de prueba o de desarrollo de Azure para probar características en versión preliminar.

```azure-cli
az feature register --namespace Microsoft.DevSpaces --name Helm3Preview
```

El proceso de registro tarda unos minutos en completarse. Compruebe el estado del registro con el comando *az feature show*:

```azure-cli
az feature show --namespace Microsoft.DevSpaces --name Helm3Preview
```

Cuando el *estado* sea *Registrado*, actualice el registro de *Microsoft.DevSpaces* con *az provider register*:

```azure-cli
az provider register --namespace Microsoft.DevSpaces
```

### <a name="limitations"></a>Limitaciones

Se aplican las siguientes limitaciones mientras esta característica esté en versión preliminar:

* No puede usar esta característica en clústeres de AKS con cargas de trabajo existentes. Debe crear un nuevo clúster de AKS.

## <a name="create-your-cluster"></a>Creación del clúster

Cree un nuevo clúster de AKS en una región que tenga esta característica en vista previa. Los siguientes comandos crean un grupo de recursos denominado *MyResourceGroup* y un nuevo clúster de AKS denominado *MyAKS*:

```azure-cli
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces"></a>Habilitación de Azure Dev Spaces

Use el comando *use-dev-spaces* para habilitar Dev Spaces en el clúster de AKS y siga las indicaciones. El siguiente comando permite habilitar Dev Spaces en el clúster *MyAKS* del grupo *MyResourceGroup* y crea un espacio de desarrollo predeterminado.

```cmd
az aks use-dev-spaces -g MyResourceGroup -n MyAKS
```

## <a name="verify-dev-spaces-is-running-helm-3"></a>Verificación de que Dev Spaces ejecuta Helm 3

Verifique que Tiller no se esté ejecutando. Para ello, enumere las implementaciones en el espacio de nombres *azds*:

```cmd
kubectl get deployment -n azds
```

Confirme que *tiller-deploy* no se esté ejecutando en el espacio de nombres azds. Por ejemplo:

```console
$ kubectl get deployments -n azds
NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
azds-webhook-deployment   2/2     2            2           39m
traefik                   1/1     1            1           39m
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de la forma en que Azure Dev Spaces le ayuda a desarrollar aplicaciones más complejas en varios contenedores y cómo puede simplificar el desarrollo en colaboración mediante el uso de distintas versiones o bifurcaciones del código en distintos espacios.

> [!div class="nextstepaction"]
> [Desarrollo en equipo en Azure Dev Spaces][team-quickstart]


[azure-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[team-quickstart]: ../quickstart-team-development.md