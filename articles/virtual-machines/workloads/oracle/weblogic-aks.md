---
title: Cuáles son las soluciones para ejecutar Oracle WebLogic Server en Azure Kubernetes Service
description: Obtenga información sobre cómo ejecutar Oracle WebLogic Server en Azure Kubernetes Service.
author: rezar
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.topic: article
ms.date: 10/28/2020
ms.author: rezar
ms.reviewer: cynthn
ms.openlocfilehash: cf44ad8d81656248329d993d86e9922dc4985258
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96486681"
---
# <a name="what-are-solutions-for-running-oracle-weblogic-server-on-the-azure-kubernetes-service"></a>¿Cuáles son las soluciones para ejecutar Oracle WebLogic Server en Azure Kubernetes Service?

En esta página se describen las soluciones para ejecutar Oracle WebLogic Server (WLS) en Azure Kubernetes Service (AKS). Oracle y Microsoft conjuntamente desarrollan estas soluciones y les prestan soporte técnico.

También es posible ejecutar WebLogic Server en Azure Virtual Machines. Las soluciones para ello se describen en [este artículo de Microsoft](./oracle-weblogic.md).

WebLogic Server es un servidor de aplicaciones Java líder en el que se ejecutan algunas de las aplicaciones de Java de empresa más fundamentales de todo el mundo. WebLogic Server constituye la base de middleware para el conjunto de software de Oracle. Oracle y Microsoft están comprometidos con ofrecer a los clientes de WebLogic Server la posibilidad y flexibilidad de ejecutar cargas de trabajo en Azure como una plataforma de nube líder.

## <a name="wls-on-aks-certified-and-supported"></a>WLS en AKS compatible y certificado
Oracle y Microsoft certifican que WebLogic Server se ejecuta correctamente en AKS. Las soluciones de WebLogic Sever en AKS están pensadas para facilitar la ejecución de aplicaciones Java EE organizadas y en contenedores en infraestructuras de Docker y Kubernetes. Las soluciones se centran en la confiabilidad, la escalabilidad, la capacidad de administración y el soporte técnico empresarial.

Los clústeres de WebLogic Server están totalmente habilitados para ejecutarse en Kubernetes mediante el operador de Kubernetes de WebLogic (en adelante, el "Operador"). El Operador sigue el patrón del operador de Kubernetes estándar. Simplifica la administración y el funcionamiento de los dominios y las implementaciones de WebLogic en Kubernetes mediante la automatización de tareas que de otro modo serían manuales y la adición de características adicionales de confiabilidad operativa. El Operador es compatible con Oracle WebLogic Server 12c, Oracle Fusion Middleware Infrastructure 12c y versiones posteriores. Hemos probado las imágenes de Docker oficiales para WebLogic Server 12.2.1.3 y 12.2.1.4 con el Operador. Para más información sobre el Operador, consulte la [documentación oficial de Oracle](https://oracle.github.io/weblogic-kubernetes-operator/).

## <a name="guidance-scripts-and-samples-for-wls-on-aks"></a>Instrucciones, scripts y ejemplos de WLS en AKS
Además de certificar WebLogic Server en AKS, Oracle y Microsoft proporcionan conjuntamente instrucciones, scripts y ejemplos detallados para ejecutar WebLogic Server en AKS. La guía se incorpora en la sección de ejemplo de Azure Kubernetes Service de la [documentación del Operador](https://oracle.github.io/weblogic-kubernetes-operator/samples/simple/azure-kubernetes-service/). La guía está pensada para facilitar las implementaciones de WebLogic Server en AKS en producción. En esta guía se usan imágenes de Docker de WebLogic Server oficiales que proporciona Oracle. La conmutación por error se consigue con Azure Files mediante notificaciones de volumen persistente de Kubernetes. Se admiten instancias de Azure Load Balancer cuando se aprovisionan con un servicio de Kubernetes de tipo "LoadBalancer". La guía permite un alto grado de configuración y personalización.

:::image type="content" source="media/oracle-weblogic/wls-on-aks.gif" alt-text="Puede usar scripts de ejemplo para implementar WebLogic Server en AKS":::

En la guía se da por hecho que se ha implementado el dominio fuera de la imagen de Docker y que se utilizan imágenes de Docker estándar de Oracle. Se agregará una guía para habilitar imágenes personalizadas con su dominio dentro de una imagen de Docker. En el futuro, habrá disponibles integraciones de servicios de Azure adicionales con mayor facilidad de uso mediante ofertas de Marketplace que reflejarán soluciones de Oracle WebLogic Server en Azure Virtual Machines.

_Estas soluciones son de tipo "traiga su propia licencia"_ . Se supone que ya adquirió las licencias adecuadas con Oracle y que tiene licencias adecuadas para ejecutar ofertas en Azure.

_Si está interesado en trabajar estrechamente en los escenarios de migración con el equipo de ingeniería que desarrolla estas soluciones, rellene [esta breve encuesta](https://aka.ms/wls-on-azure-survey) e incluya su información de contacto_. Los administradores de programas, los arquitectos y los ingenieros se pondrán en contacto con usted en breve para iniciar una colaboración estrecha. La oportunidad de colaborar en un escenario de migración es gratuita mientras las soluciones se encuentren en desarrollo inicial activo.

## <a name="deployment-architectures"></a>Arquitecturas de implementación

Las soluciones para ejecutar Oracle WebLogic Server en Azure Kubernetes Service habilitarán una amplia gama de arquitecturas de implementación listas para producción con relativa facilidad.

:::image type="content" source="media/oracle-weblogic/weblogic-architecture-aks.png" alt-text="Las implementaciones complejas de WebLogic Server están habilitadas en AKS":::

Además de lo que las soluciones ofrecen, los clientes tienen total flexibilidad para personalizar sus implementaciones. Probablemente, además de la implementación de aplicaciones, los clientes integrarán más recursos de Azure a sus implementaciones. Se recomienda a los clientes que agreguen comentarios en la encuesta para mejorar aún más las soluciones.

## <a name="next-steps"></a>Pasos siguientes

Descubra cómo se ejecuta Oracle WebLogic Server en Azure Kubernetes Service.

> [!div class="nextstepaction"]
> [Instrucciones, scripts y ejemplos para ejecutar WLS en AKS](https://oracle.github.io/weblogic-kubernetes-operator/samples/simple/azure-kubernetes-service/)

> [!div class="nextstepaction"]
> [Operador de Kubernetes de WebLogic](https://oracle.github.io/weblogic-kubernetes-operator/)