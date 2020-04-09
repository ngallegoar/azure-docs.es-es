---
title: ¿Qué es el controlador de entrada de Azure Application Gateway?
description: Este artículo ofrece una introducción sobre el controlador de entrada de Application Gateway.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: d6a63b6276c07b1fe6487b97f5c7fc255b6d3411
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335796"
---
# <a name="what-is-application-gateway-ingress-controller"></a>¿Qué es el controlador de entrada de Application Gateway?
El controlador de entrada de Application Gateway (AGIC) es una aplicación de Kubernetes, lo que hace posible que los clientes de [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/services/kubernetes-service/) aprovechen la tecnología nativa de Azure [Application Gateway](https://azure.microsoft.com/services/application-gateway/): el equilibrador de carga L7 para exponer software en la nube a Internet. AGIC supervisa el clúster de Kubernetes en el que se hospeda y actualiza continuamente una instancia de Application Gateway para que los servicios seleccionados se expongan a Internet.

El controlador de entrada se ejecuta en su propio pod en la instancia de AKS del cliente. AGIC supervisa un subconjunto de recursos de Kubernetes para los cambios. El estado del clúster de AKS se traduce a una configuración específica de Application Gateway y se aplica a [Azure Resource Manager (ARM)](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

## <a name="benefits-of-application-gateway-ingress-controller"></a>Ventajas del controlador de entrada de Application Gateway
AGIC permite que la implementación controle varios clústeres de AKS con un solo controlador de entrada de Application Gateway. AGIC también ayuda a eliminar la necesidad de tener otra dirección IP pública o equilibrador de carga frente al clúster de AKS y evita varios saltos en la ruta de datos antes de que las solicitudes lleguen al clúster AKS. Application Gateway se comunica con los pods mediante su dirección IP privada directamente y no requiere servicios de NodePort o KubeProxy. Esto también aporta un mejor rendimiento a las implementaciones.

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

AGIC es capaz de administrar varios espacios de nombres y cuenta con ProhibitedTargets, lo que significa que AGIC puede configurar la instancia de Application Gateway específicamente para los clústeres de AKS sin que afecte a otros back-ends existentes. 

## <a name="next-steps"></a>Pasos siguientes

- [**Implementación Greenfield**](ingress-controller-install-new.md): Instrucciones sobre cómo instalar AGIC, AKS y Application Gateway en una infraestructura de pizarra en blanco.
- [**Implementación Brownfield**](ingress-controller-install-existing.md): Instale AGIC en una instancia existente de AKS y Application Gateway.

