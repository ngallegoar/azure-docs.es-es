---
title: Introducción a Istio
description: Obtenga información general sobre Istio.
author: paulbouwer
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 8518e30a54c2486abf84cd9ac026cc4dccb3fa84
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "77593907"
---
# <a name="istio"></a>Istio

## <a name="overview"></a>Información general

[Istio][istio] es una malla de servicio completa, personalizable y extensible.

## <a name="architecture"></a>Architecture

Istio proporciona un plano de datos compuesto por sidecars basados en [Envoy][envoy-proxy]. Estos proxies inteligentes controlan todo el tráfico de red de entrada y salida de las aplicaciones y cargas de trabajo en malla.

El plano de control administra la configuración, la directiva y la telemetría a través de los siguientes [componentes][what-is-istio]:

- **Mixer**: aplica las directivas de uso y control de acceso. Recopila datos de telemetría de los proxies que se insertan en [Prometheus][prometheus].

- **Pilot**: proporciona la configuración y la directiva de detección de servicios y administración de tráfico para los proxies.

- **Citadel**: proporciona funciones de identidad y seguridad que permiten la conectividad mTLS entre los servicios.

- **Galley**: sintetiza y proporciona la configuración a los componentes.

En el diagrama de arquitectura siguiente se muestra cómo interactúan los distintos componentes del plano de datos y del plano de control.


![Información general de los componentes y la arquitectura de Istio.](media/servicemesh/istio/about-architecture.png)


## <a name="selection-criteria"></a>Criterios de selección

A la hora de Istio para las cargas de trabajo, es importante comprender y considerar las siguientes áreas:

- [Objetivos de diseño](#design-goals)
- [Capabilities](#capabilities)
- [Escenarios](#scenarios)


### <a name="design-goals"></a>Diseño de objetivos

Los siguientes objetivos de diseño constituyen la [guía][design-goals] del proyecto Istio:

- **Maximizar la transparencia**: debe permitir la adopción con la mínima cantidad de trabajo, para obtener el valor real del sistema.

- **Extensibilidad**: debe ser capaz de crecer y adaptarse a los cambios en las necesidades.

- **Portabilidad**: debe ejecutarse fácilmente en diferentes tipos de entornos: la nube, el entorno local.

- **Uniformidad de directivas**: coherencia en la definición de las directivas entre los diversos recursos.


### <a name="capabilities"></a>Capacidades

Istio ofrece el siguiente conjunto de funcionalidades:

- **Malla**: puertas de enlace (varios clústeres), máquinas virtuales (expansión de malla)

- **Administración del tráfico**: enrutamiento, división, tiempos de espera, disyuntores, reintentos, entrada, salida

- **Directiva**: control de acceso, límite de frecuencia, cuota, adaptadores de directivas personalizados

- **Seguridad**: autenticación (jwt), autorización, cifrado (mTLS), entidad de certificación externa (HashiCorp Vault)

- **Observabilidad**: métricas principales, reflejo, seguimiento, adaptadores personalizados, Prometheus, Grafana

### <a name="scenarios"></a>Escenarios

Istio es idóneo y se sugiere para los escenarios siguientes:

- Se necesita extensibilidad y un amplio conjunto de funcionalidades

- Se necesita expandir la malla para incluir cargas de trabajo basadas en máquinas virtuales

- Malla de servicio de varios clústeres

## <a name="next-steps"></a>Pasos siguientes

En la siguiente documentación se describe cómo puede instalar Istio en Azure Kubernetes Service (AKS):

> [!div class="nextstepaction"]
> [Instalación de Istio en Azure Kubernetes Service (AKS)][istio-install]

También puede profundizar en los conceptos y en modelos de implementación adicionales de Istio:

- [Conceptos de Istio][what-is-istio]
- [Modelos de implementación de Istio][deployment-models]

<!-- LINKS - external -->
[istio]: https://istio.io
[what-is-istio]: https://istio.io/docs/concepts/what-is-istio/
[design-goals]: https://istio.io/docs/concepts/what-is-istio/#design-goals
[deployment-models]: https://istio.io/docs/concepts/deployment-models/

[envoy-proxy]: https://www.envoyproxy.io/
[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/

<!-- LINKS - internal -->
[istio-install]: ./servicemesh-istio-install.md
