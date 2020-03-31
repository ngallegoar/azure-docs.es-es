---
title: Acerca de las mallas de servicio
description: Obtenga información general sobre las mallas de servicio, su arquitectura y sus funcionalidades, así como los criterios que debe tener en cuenta a la hora de seleccionar una para implementar.
author: paulbouwer
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: eca49a3fac1ea0398ebe1d05bde20fbca3c81232
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77594318"
---
# <a name="about-service-meshes"></a>Acerca de las mallas de servicio

Una malla de servicio proporciona a las cargas de trabajo funcionalidades como administración del tráfico, resistencia, directiva, seguridad, identidad sólida y observabilidad. La aplicación se desacopla de estas funciones operativas y la malla del servicio las retira de la capa de la aplicación, bajándolas al nivel de infraestructura.

## <a name="scenarios"></a>Escenarios

Estos son algunos de los escenarios que se pueden habilitar para las cargas de trabajo cuando se usa una malla de servicio:

- **Cifrar todo el tráfico del clúster**: habilite TLS mutuo entre los servicios especificados del clúster. Se puede extender a la entrada y salida en el perímetro de la red. Proporciona una opción segura de forma predeterminada sin necesidad de realizar cambios en el código y en la infraestructura de la aplicación.

- **Lanzamientos controlados y por fases**: especifique las condiciones para que un subconjunto de tráfico se enrute a un conjunto de nuevos servicios del clúster. Si la prueba del lanzamiento controlado es correcta, quite el enrutamiento y la fase condicionales, aumentando gradualmente el porcentaje de tráfico al nuevo servicio. Al final, todo el tráfico se dirigirá al nuevo servicio.

- **Administración y manipulación del tráfico**: cree una directiva en un servicio que limite la frecuencia de todo el tráfico a una versión de un servicio en un origen específico. O bien una directiva que aplique una estrategia de reintento a clases de errores entre servicios especificados. Refleje el tráfico real en nuevas versiones de servicios durante una migración o para depurar problemas. Inserte errores entre los servicios en un entorno de prueba para probar la resistencia.

- **Observabilidad**: obtenga información sobre cómo se conectan los servicios al tráfico que fluye entre ellos. Obtenga métricas, registros y seguimientos para todo el tráfico del clúster, así como para la entrada y la salida. Agregue funcionalidades de seguimiento distribuido a las aplicaciones.

## <a name="architecture"></a>Architecture

Una malla de servicio suele estar compuesta por el plano de control y el plano de datos.

El **plano de control** cuenta con una serie de componentes que dan soporte a la administración de la malla de servicio. Por lo general, incluye una interfaz de administración, que puede ser una interfaz de usuario o una API. También suele haber componentes que administran las definiciones de reglas y directivas que establecen cómo la malla de servicio debe implementar funcionalidades específicas. Hay, además, componentes que administran aspectos de seguridad como la identidad sólida y los certificados para mTLS. Las mallas de servicio también suelen contar con un componente de métricas u observabilidad que recopila y agrega métricas y datos de telemetría de las cargas de trabajo.

El **plano de datos** consta normalmente de un proxy que se inserta de forma transparente como sidecar en las cargas de trabajo. Este proxy se configura para controlar todo el tráfico de red de entrada y salida del pod que contiene la carga de trabajo. Esto permite configurar el proxy para proteger el tráfico a través de mTLS, enrutar dinámicamente el tráfico, aplicar directivas al tráfico y recopilar métricas e información de seguimiento. 

![Arquitectura típica de la malla de servicio](media/servicemesh/typical-architecture.png)

## <a name="capabilities"></a>Capacidades

Cada una de las mallas de servicio tiene un ajuste natural y se centra en la compatibilidad con escenarios específicos, pero verá que habitualmente la mayoría implementará varias de las siguientes funcionalidades, si no todas.

### <a name="traffic-management"></a>Administración del tráfico 

- **Protocolo**: nivel 7 (http, grpc)
- **Enrutamiento dinámico**: condicional, ponderación, creación de reflejo
- **Resistencia**: tiempos de espera, reintentos, disyuntores
- **Directiva**: control de acceso, límites de frecuencia, cuotas
- **Pruebas**: inserción de errores

### <a name="security"></a>Seguridad

- **Cifrado**: mTLS, administración de certificados, entidad de certificación externa
- **Identidad sólida**: SPIFFE o similar
- **Autenticación**: autenticación, autorización

### <a name="observability"></a>Observabilidad

- **Métricas**: métricas principales, Prometheus, Grafana
- **Seguimiento**: seguimientos entre las cargas de trabajo
- **Tráfico**: clúster, entrada y salida

### <a name="mesh"></a>Malla

- **Proceso compatible**: Kubernetes, máquinas virtuales
- **Varios clústeres**: puertas de enlace, federación

## <a name="selection-criteria"></a>Criterios de selección

Antes de seleccionar una malla de servicio, asegúrese de que comprende los requisitos y las razones para instalarla. Intente responder a las siguientes preguntas.

- **¿Es un controlador de entrada suficiente para mis necesidades?** A veces, contar con una funcionalidad como las pruebas A/B o la división del tráfico en la entrada es suficiente para el escenario requerido. No agregue complejidad a su entorno si no supone una ventaja.

- **¿Las cargas de trabajo y el entorno actuales pueden tolerar las sobrecargas adicionales?** Todos los componentes adicionales necesarios para admitir la malla de servicio requieren recursos adicionales, como CPU y memoria. Además, todos los proxies y sus comprobaciones de directiva asociadas agregan latencia al tráfico. Si tiene cargas de trabajo muy sensibles a la latencia o no puede proporcionar los recursos adicionales necesarios para cubrir los componentes de la malla de servicio, replantéese esta opción.

- **¿Esta opción agrega complejidad adicional innecesaria?** Si el motivo para instalar una malla de servicio es conseguir una funcionalidad que no es necesariamente crítica para los equipos empresariales u operativos, considere si merece la pena la complejidad adicional de la instalación, el mantenimiento y la configuración.

- **¿Se puede adoptar con una estrategia incremental?** Algunas de las mallas de servicio que proporcionan una gran cantidad de funciones se pueden adoptar de modo incremental. Instale solo los componentes que necesita para cubrir sus necesidades. Una vez que esté más seguro y se necesiten funcionalidades adicionales, explore dichas opciones. Resista la tentación de instalar *todo* desde el principio.

Si, tras una detenida consideración, decide que necesita una malla de servicio para proporcionar las funcionalidades requeridas, la siguiente pregunta que debe plantearse es: *¿Qué malla de servicio?*

Tenga en cuenta las siguientes áreas y cuáles se adaptan más a sus requisitos. Esto le ayudará a conseguir el mejor ajuste para su entorno y sus cargas de trabajo. La sección [Pasos siguientes](#next-steps) le llevará a información más detallada sobre mallas de servicio específicas y cómo se asignan a estas áreas.

- **Área técnica**: administración del tráfico, directiva, seguridad, observabilidad

- **Área empresarial**: soporte comercial, base (CNCF), licencia de OSS, gobernanza

- **Área operativa**: instalación y actualizaciones, requisitos de recursos, requisitos de rendimiento, integraciones (métricas, telemetría, paneles, herramientas, SMI), cargas de trabajo mixtas (grupos de nodos de Linux y Windows), proceso (Kubernetes, máquinas virtuales), varios clústeres

- **Área de seguridad**: autenticación, identidad, administración y rotación de certificados, entidad de certificación externa acoplable


## <a name="next-steps"></a>Pasos siguientes

La siguiente documentación proporciona más información sobre las mallas de servicio que puede probar en Azure Kubernetes Service (AKS):

> [!div class="nextstepaction"]
> [Más información sobre Istio...][istio-about]

> [!div class="nextstepaction"]
> [Más información sobre Linkerd...][linkerd-about]

> [!div class="nextstepaction"]
> [Más información sobre Consul...][consul-about]

También puede explorar Service Mesh Interface (SMI), una interfaz estándar para las mallas de servicio en Kubernetes:

- [Service Mesh Interface (SMI)][smi]


<!-- LINKS - external -->
[smi]: https://smi-spec.io/

<!-- LINKS - internal -->
[istio-about]: ./servicemesh-istio-about.md
[linkerd-about]: ./servicemesh-linkerd-about.md
[consul-about]: ./servicemesh-consul-about.md