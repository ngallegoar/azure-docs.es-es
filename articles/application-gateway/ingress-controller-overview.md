---
title: ¿Qué es el controlador de entrada de Azure Application Gateway?
description: Este artículo ofrece una introducción sobre el controlador de entrada de Application Gateway.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 06/10/2020
ms.author: caya
ms.openlocfilehash: 26f53a8f93d4d51ec8f8fd91051496a46670f432
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2020
ms.locfileid: "93397355"
---
# <a name="what-is-application-gateway-ingress-controller"></a>¿Qué es el controlador de entrada de Application Gateway?
El controlador de entrada de Application Gateway (AGIC) es una aplicación de Kubernetes, lo que hace posible que los clientes de [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/services/kubernetes-service/) aprovechen la tecnología nativa de Azure [Application Gateway](https://azure.microsoft.com/services/application-gateway/): el equilibrador de carga L7 para exponer software en la nube a Internet. AGIC supervisa el clúster de Kubernetes en el que se hospeda y actualiza continuamente una instancia de Application Gateway para que los servicios seleccionados se expongan a Internet.

El controlador de entrada se ejecuta en su propio pod en la instancia de AKS del cliente. AGIC supervisa un subconjunto de recursos de Kubernetes para los cambios. El estado del clúster de AKS se traduce a una configuración específica de Application Gateway y se aplica a [Azure Resource Manager (ARM)](../azure-resource-manager/management/overview.md).

## <a name="benefits-of-application-gateway-ingress-controller"></a>Ventajas del controlador de entrada de Application Gateway
AGIC ayuda a eliminar la necesidad de tener otra dirección IP pública o equilibrador de carga frente al clúster de AKS y evita varios saltos en la ruta de datos antes de que las solicitudes lleguen al clúster AKS. Application Gateway se comunica con los pods mediante su dirección IP privada directamente y no requiere servicios de NodePort o KubeProxy. Esto también aporta un mejor rendimiento a las implementaciones.

El controlador de entrada es compatible exclusivamente con las SKU de Standard_v2 y WAF_v2, que también aportan ventajas de escalado automático. Application Gateway puede reaccionar en respuesta a un aumento o una disminución de la carga y la escala de tráfico según corresponda, sin consumir ningún recurso del clúster de AKS.

El uso de Application Gateway, además de AGIC, también ayuda a proteger el clúster de AKS al proporcionar la funcionalidad de la directiva TLS y firewall de aplicaciones web (WAF).

![Azure Application Gateway + AKS](./media/application-gateway-ingress-controller-overview/architecture.png)

AGIC se configura a través del [recurso de entrada](https://kubernetes.io/docs/user-guide/ingress/) de Kubernetes, junto con los servicios, las implementaciones y los pods. Proporciona una serie de características que aprovechan el equilibrador de carga L7 nativo de Azure Application Gateway. Por mencionar algunas:
  - Enrutamiento de direcciones URL
  - Afinidad basada en cookies
  - Finalización de TLS
  - TLS de un extremo a otro
  - Soporte para sitios web públicos, privados e híbridos
  - Firewall de aplicaciones web integrado

## <a name="difference-between-helm-deployment-and-aks-add-on"></a>Diferencias entre la implementación de Helm y el complemento de AKS
Hay dos maneras de implementar AGIC para el clúster de AKS. La primera manera es mediante Helm; la segunda es mediante AKS como un complemento. La principal ventaja de implementar AGIC como un complemento de AKS es que es mucho más sencillo que la implementación mediante Helm. En el caso de una nueva instalación, puede implementar una nueva instancia de Application Gateway y un nuevo clúster de AKS con AGIC habilitado como un complemento en una línea en la CLI de Azure. El complemento es también un servicio totalmente administrado, que ofrece ventajas adicionales, como actualizaciones automáticas y mayor compatibilidad. AKS no admite AGIC implementado mediante Helm; sin embargo, AGIC implementado como un complemento de AKS es compatible con AKS. 

El complemento de AGIC se implementa como un pod en el clúster de AKS del cliente; sin embargo, hay algunas diferencias entre la versión de la implementación de Helm y la versión del complemento de AGIC. A continuación se muestra una lista de las diferencias entre las dos versiones: 
  - Los valores de implementación de Helm no se pueden modificar en el complemento de AKS:
    - `verbosityLevel` se establecerá en 5 de forma predeterminada
    - `usePrivateIp` se establecerá en False de forma predeterminada; esto se puede invalidar con la [anotación use-private-ip](ingress-controller-annotations.md#use-private-ip)
    - `shared` no se admite en el complemento 
    - `reconcilePeriodSeconds` no se admite en el complemento
    - `armAuth.type` no se admite en el complemento
  - AGIC implementado mediante Helm admite ProhibitedTargets, lo que significa que AGIC puede configurar la instancia de Application Gateway específicamente para los clústeres de AKS sin que afecte a otros back-ends existentes. El complemento de AGIC no es compatible actualmente con esto. 
  - Dado que el complemento de AGIC es un servicio administrado, los clientes se actualizarán automáticamente a la versión más reciente del complemento de AGIC, a diferencia de AGIC implementado mediante Helm, donde el cliente debe actualizar AGIC manualmente. 

> [!NOTE]
> El método de implementación del complemento de AKS de AGIC se encuentra actualmente en versión preliminar. No se recomienda ejecutar cargas de trabajo de producción con características que todavía están en versión preliminar, por lo que, si tiene curiosidad por probarlo, se recomienda configurar un nuevo clúster para las pruebas. 

En las tablas siguientes se ordenan los escenarios que se admiten actualmente con la versión de implementación de Helm y la versión del complemento de AKS de AGIC. 

### <a name="aks-add-on-agic-single-aks-cluster"></a>Complemento de AKS de AGIC (clúster de AKS único)
|                  |1 Application Gateway |2+ Application Gateway |
|------------------|---------|--------|
|**1 AGIC**|Sí, se admite. |No, está en los trabajos pendientes. |
|**2+ AGIC**|No, solo se admite 1 AGIC por clúster |No, solo se admite 1 AGIC por clúster |

### <a name="helm-deployed-agic-single-aks-cluster"></a>AGIC implementado mediante Helm (clúster de AKS único)
|                  |1 Application Gateway |2+ Application Gateway |
|------------------|---------|--------|
|**1 AGIC**|Sí, se admite. |No, está en los trabajos pendientes. |
|**2+ AGIC**|Debe usar la funcionalidad ProhibitedTarget compartida y observar espacios de nombres independientes. |Sí, se admite. |

### <a name="helm-deployed-agic-2-aks-clusters"></a>AGIC implementado mediante Helm (2+ clústeres de AKS)
|                  |1 Application Gateway |2+ Application Gateway |
|------------------|---------|--------|
|**1 AGIC**|N/D |N/D |
|**2+ AGIC**|Debe usar la funcionalidad ProhibitedTarget compartida |N/D |

## <a name="next-steps"></a>Pasos siguientes
- [**Implementación Greenfield del complemento de AKS**](tutorial-ingress-controller-add-on-new.md): instrucciones sobre cómo instalar el complemento de AGIC, AKS y Application Gateway en una infraestructura de pizarra en blanco.
- [**Implementación Brownfield del complemento de AKS**](tutorial-ingress-controller-add-on-existing.md): instalación del complemento de AGIC en un clúster de AKS con una instancia de Application Gateway existente.
- [**Implementación Greenfield con Helm**](ingress-controller-install-new.md): instalación de AGIC mediante Helm, un nuevo clúster de AKS y una nueva instancia de Application Gateway en una infraestructura de pizarra en blanco.
- [**Implementación Brownfield con Helm**](ingress-controller-install-existing.md): implementación de AGIC mediante Helm en un clúster de AKS y una instancia de Application Gateway existentes.