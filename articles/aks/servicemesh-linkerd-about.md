---
title: Introducción a Linkerd
description: Obtenga información general sobre Linkerd.
author: paulbouwer
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 3181be62a14ec1b3450bd181172b5323ca176427
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "77593774"
---
# <a name="linkerd"></a>Linkerd

## <a name="overview"></a>Información general

[Linkerd][linkerd] es una malla de servicio ligera y fácil de usar.

## <a name="architecture"></a>Architecture

Linkerd proporciona un plano de datos que se compone de sidecars de proxy ultraligeros y especializados de [Linkerd][linkerd-proxy]. Estos proxies inteligentes controlan todo el tráfico de red de entrada y salida de las aplicaciones y cargas de trabajo en malla. Los proxies también exponen métricas a través de puntos de conexión de métricas de [Prometheus][prometheus].

El plano de control administra la configuración y los datos de telemetría agregados a través de los siguientes [componentes][linkerd-architecture]:

- **Controller**: proporciona una API que controla la CLI y el panel de Linkerd. Proporciona la configuración para los proxies.

- **Tap**: establece inspecciones en tiempo real en solicitudes y respuestas.

- **Identity**: proporciona funcionalidades de identidad y seguridad que permiten la conectividad mTLS entre los servicios.

- **Web**: proporciona el panel de Linkerd.


En el diagrama de arquitectura siguiente se muestra cómo interactúan los distintos componentes del plano de datos y del plano de control.


![Información general de los componentes y la arquitectura de Linkerd.](media/servicemesh/linkerd/about-architecture.png)


## <a name="selection-criteria"></a>Criterios de selección

A la hora de evaluar Linkerd para las cargas de trabajo, es importante comprender y considerar las siguientes áreas:

- [Principios de diseño](#design-principles)
- [Capabilities](#capabilities)
- [Escenarios](#scenarios)


### <a name="design-principles"></a>Principios de diseño

Los siguientes principios constituyen la [guía][design-principles] del proyecto Linkerd:

- **Conservar un diseño sencillo**: debe ser fácil de usar y comprender.

- **Minimizar los requisitos de recursos**: debe suponer un rendimiento y un costo de recursos mínimos.

- **Simplemente trabajo**: no debe interrumpir las aplicaciones existentes ni requerir una configuración compleja.


### <a name="capabilities"></a>Capacidades

Linkerd proporciona el siguiente conjunto de funcionalidades:

- **Malla**: opción de depuración integrada

- **Administración del tráfico**: división, tiempos de espera, reintentos, entrada

- **Seguridad**: cifrado (mTLS), certificados con rotación automática cada 24 horas

- **Observabilidad**: métricas principales, Tap, seguimiento, perfiles de servicio y métricas por ruta, panel web con gráficos de topología, Prometheus, Grafana


### <a name="scenarios"></a>Escenarios

Linkerd es idóneo y se sugiere para los escenarios siguientes:

- Uso sencillo, únicamente con el conjunto esencial de requisitos de funcionalidad

- Baja latencia, baja sobrecarga, con foco en la observabilidad y una administración de tráfico simple


## <a name="next-steps"></a>Pasos siguientes

En la siguiente documentación se describe cómo puede instalar Linkerd en Azure Kubernetes Service (AKS):

> [!div class="nextstepaction"]
> [Instalación de Linkerd en Azure Kubernetes Service (AKS)][linkerd-install]

También puede seguir explorando las características y la arquitectura de Linkerd:

- [Linkerd Features][linkerd-features] (Características de Linkerd)
- [Linkerd Architecture][linkerd-architecture] (Arquitectura de Linkerd)

<!-- LINKS - external -->
[linkerd]: https://linkerd.io/2/overview/
[linkerd-architecture]: https://linkerd.io/2/reference/architecture/
[linkerd-features]: https://linkerd.io/2/features/
[design-principles]: https://linkerd.io/2/design-principles/
[linkerd-proxy]: https://github.com/linkerd/linkerd2-proxy

[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/

<!-- LINKS - internal -->
[linkerd-install]: ./servicemesh-linkerd-install.md