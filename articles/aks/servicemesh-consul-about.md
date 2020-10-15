---
title: Introducción a Consul
description: Obtenga información general sobre Consul.
author: paulbouwer
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 77cb6a693c5a73d0498b0acf9bc4ad8cc9f8f32f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "83774003"
---
# <a name="consul"></a>Consul

## <a name="overview"></a>Información general

[Consul][consul] es una solución de redes de servicio que reconoce varios centros de datos y sirve para conectar y proteger servicios entre plataformas del entorno de ejecución. [Connect][consul-features] es el componente que proporciona funcionalidades de malla de servicio.

## <a name="architecture"></a>Architecture

Consul proporciona un plano de datos compuesto de forma predeterminada por [sidecars][consul-sidecar] basados en [Envoy][envoy-proxy]. Consul tiene una arquitectura de proxy acoplable. Estos proxies inteligentes controlan todo el tráfico de red de entrada y salida de las aplicaciones y cargas de trabajo en malla.

El plano de control administra la configuración y la directiva a través de los siguientes [componentes][consul-architecture]:

- **Servidor**: un agente de Consul que se ejecuta en modo de servidor y que mantiene el estado de clúster de Consul.

- **Cliente**: un agente de Consul que se ejecuta en modo de cliente ligero. Cada nodo de proceso debe tener un agente cliente en ejecución. Este cliente traslada la configuración y la directiva entre las cargas de trabajo y la configuración de Consul. 

En el diagrama de arquitectura siguiente se muestra cómo interactúan los distintos componentes del plano de datos y del plano de control.

![Información general de los componentes y la arquitectura de Consul.](media/servicemesh/consul/about-architecture.png)


## <a name="selection-criteria"></a>Criterios de selección

A la hora de evaluar Consul para las cargas de trabajo, es importante comprender y considerar las siguientes áreas:

- [Principios de Consul](#consul-principles)
- [Capabilities](#capabilities)
- [Escenarios](#scenarios)


### <a name="consul-principles"></a>Principios de Consul

Los siguientes principios constituyen la [guía][consul-principles] del proyecto Consul:

- **Basado en API**: debe codificar toda la configuración y la directiva.

- **Ejecución y conexión en cualquier lugar**: debe conectar cargas de trabajo en las plataformas del entorno de ejecución (Kubernetes, máquinas virtuales, sin servidor).

- **Extensión e integración**: debe conectar de forma segura las cargas de trabajo en la infraestructura.


### <a name="capabilities"></a>Capacidades

Consul proporciona el siguiente conjunto de funcionalidades:

- **Malla**: puerta de enlace (varios centros de datos), máquinas virtuales (fuera de nodos del clúster), sincronización de servicios, opción de depuración integrada

- **Proxies**: proxy L4 de Envoy integrado, acoplable, disponible para cargas de trabajo de Windows

- **Administración del tráfico**: enrutamiento, división, resolución

- **Directiva**: intenciones, listas de control de acceso

- **Seguridad**: autorización, autenticación, cifrado, identidades basadas en SPIFFE, entidad de certificación externa (Vault), administración y rotación de certificados

- **Observabilidad**: métricas, panel de interfaz de usuario, Prometheus, Grafana


### <a name="scenarios"></a>Escenarios

Consul es idóneo y se sugiere para los escenarios siguientes:

- Extensión de las cargas de trabajo conectadas a Consul existentes

- Requisitos de cumplimiento relacionados con la administración de certificados

- Malla de servicio de varios clústeres

- Cargas de trabajo basadas en máquinas virtuales que se van a incluir en la malla de servicio



## <a name="next-steps"></a>Pasos siguientes

En la siguiente documentación se describe cómo puede instalar Consul en Azure Kubernetes Service (AKS):

> [!div class="nextstepaction"]
> [Instalación de Consul en Azure Kubernetes Service (AKS)][consul-install]

También puede seguir explorando las características y la arquitectura de Consul:

- [Tutoriales de introducción a Consul][consul-getting-started]
- [Consul Features][consul-features] (Características de Consul)
- [Consul Architecture][consul-architecture] (Arquitectura de Consul)
- [Consul - How Connect Works][consul-how-connect-works] (Consul: Funcionamiento de Connect)

<!-- LINKS - external -->
[consul]: https://www.consul.io/mesh.html
[consul-features]: https://www.consul.io/docs/connect/index.html
[consul-architecture]: https://www.consul.io/docs/internals/architecture.html
[consul-sidecar]: https://www.consul.io/docs/connect/proxies.html
[consul-how-connect-works]: https://www.consul.io/docs/connect/connect-internals.html
[consul-principles]: https://www.consul.io/
[consul-getting-started]:https://learn.hashicorp.com/consul?track=gs-consul-service-mesh#gs-consul-service-mesh

[envoy-proxy]: https://www.envoyproxy.io/
[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/

<!-- LINKS - internal -->
[consul-install]: ./servicemesh-consul-install.md
