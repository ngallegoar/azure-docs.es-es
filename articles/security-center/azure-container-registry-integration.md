---
title: Azure Security Center y Azure Container Registry
description: Obtenga más información sobre la integración de Azure Security Center con Azure Container Registry
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/19/2019
ms.author: memildin
ms.openlocfilehash: 069ce6ca1e76a9bd954031708702c973387abbaa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78206000"
---
# <a name="azure-container-registry-integration-with-security-center"></a>Integración de Azure Container Registry con Security Center

Azure Container Registry (ACR) es un servicio de registro de Docker privado y administrado que almacena y administra las imágenes de contenedor de las implementaciones de Azure en un registro central. Se basa en el registro 2.0 de Docker de código abierto.

Si está en el nivel estándar de Azure Security Center, puede agregar el paquete Registros de contenedor. Esta característica opcional proporciona mayor visibilidad sobre las vulnerabilidades de las imágenes en los registros basados en Azure Resource Manager. Habilite o deshabilite el paquete en el nivel de suscripción para abarcar todos los registros de una suscripción. Esta característica se cobra por imagen, tal como se muestra en la [página de precios](security-center-pricing.md). Al habilitar el paquete Registros de contenedor se asegura de que Security Center esté listo para examinar las imágenes que se inserten en el registro. 

Cada vez que se inserta una imagen en el registro, Security Center la examina de forma automática. Para desencadenar el examen de una imagen, insértela en el repositorio.

Cuando finaliza el examen (al cabo de 10 minutos aproximadamente), los resultados están disponibles en recomendaciones de Security Center como esta:

[![Ejemplo de recomendación Azure Security Center sobre puntos vulnerables detectados en una imagen hospedada de Azure Container Registry (ACR)](media/azure-container-registry-integration/container-security-acr-page.png)](media/azure-container-registry-integration/container-security-acr-page.png#lightbox)

## <a name="benefits-of-integration"></a>Ventajas de la integración

Security Center identifica los registros de ACR basados en Azure Resource Manager en su suscripción y proporciona de forma fácil lo siguiente:

* **Detección de vulnerabilidades nativas de Azure** para todas las imágenes de Linux insertadas. Security Center examina la imagen con un escáner del proveedor de detección de vulnerabilidades líder del sector, Qualys. Esta solución nativa se integra sin problemas de forma predeterminada.

* **Recomendaciones de seguridad** para las imágenes de Linux con vulnerabilidades conocidas. Security Center proporciona detalles de cada vulnerabilidad detectada y una clasificación de gravedad. Asimismo, ofrece instrucciones sobre cómo corregir las vulnerabilidades específicas encontradas en cada imagen insertada en el registro.

![Introducción de alto nivel de Azure Security Center y Azure Container Registry (ACR)](./media/azure-container-registry-integration/aks-acr-integration-detailed.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las características de seguridad de los contenedores de Security Center, consulte:

* [Seguridad de los contenedores y Azure Security Center](container-security.md)

* [Integración con Azure Kubernetes Service](azure-kubernetes-service-integration.md)

* [Protección de máquinas virtuales](security-center-virtual-machine-protection.md): describe las recomendaciones de Security Center
