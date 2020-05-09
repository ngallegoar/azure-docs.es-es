---
title: Preguntas frecuentes acerca de Azure Spring Cloud | Microsoft Docs
description: En este artículo se responden las preguntas más frecuentes sobre Azure Spring Cloud.
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/07/2019
ms.author: brendm
ms.openlocfilehash: 95260d9a15fdc32c9fddccbcf63ae9fa564fd36a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2020
ms.locfileid: "82176777"
---
# <a name="azure-spring-cloud-faq"></a>Preguntas frecuentes de Azure Spring Cloud

En este artículo se responden las preguntas más frecuentes sobre Azure Spring Cloud.

## <a name="general"></a>General

### <a name="why-azure-spring-cloud"></a>¿Por qué Azure Spring Cloud?

Azure Spring Cloud proporciona una plataforma como servicio (PaaS) para los desarrolladores de Spring Cloud. Azure Spring Cloud administra la infraestructura de la aplicación, de modo que el usuario puede centrarse en el código de la aplicación y en la lógica de negocios. Las características centrales integradas en Azure Spring Cloud incluyen Eureka, Config Server, Service Registry Server, Pivotal Build Service e implementaciones Blue-Green, entre otras. Este servicio también permite a los desarrolladores enlazar sus aplicaciones con otros servicios de Azure, como Azure Cosmos DB, Azure Database for MySQL y Azure Cache for Redis.

Azure Spring Cloud mejora la experiencia de diagnóstico de aplicaciones para los desarrolladores y operadores mediante la integración de Azure Monitor, Application Insights y Log Analytics.

### <a name="what-service-plans-does-azure-spring-cloud-offer"></a>¿Qué planes de servicio ofrece Azure Spring Cloud?

Azure Spring Cloud ofrece un solo plan de servicio durante el periodo de versión preliminar.  Una implementación de Spring Cloud contiene 16 núcleos de vCPU y 32 gigabytes (GB) de memoria.  El límite superior de cada instancia de microservicio dentro de una implementación es de 4 núcleos de vCPU con 8 GB de memoria.

Resource | Importe
------- | -------
Instancias de aplicación por aplicación de Spring | 20
Instancias de aplicación totales por instancia del servicio Azure Spring Cloud | 500
Instancias del servicio Azure Spring Cloud por región y suscripción | 10
Volúmenes persistentes | 10 x 50 GBytes

\* _Para aumentar el límite, abra una [incidencia de soporte técnico](https://azure.microsoft.com/support/faq/)._

Para obtener más información, consulte las [preguntas más frecuentes de soporte técnico de Azure](https://azure.microsoft.com/support/faq/).

### <a name="how-secure-is-azure-spring-cloud"></a>¿Es seguro Azure Spring Cloud?

La seguridad y la privacidad se encuentran entre las principales prioridades para los clientes de Azure y de Azure Spring Cloud. Azure permite garantizar que solo los clientes tienen acceso a los datos, registros o configuraciones de la aplicación mediante el cifrado seguro de todos estos datos. Las instancias de servicio de Azure Spring Cloud están todas aisladas unas de otras.

Azure Spring Cloud proporciona administración completa de certificados y TLS/SSL.

Las revisiones de seguridad críticas de los runtimes de OpenJDK y Spring Cloud se aplicarán a Azure Spring Cloud lo antes posible.

### <a name="in-which-regions-is-azure-spring-cloud-available"></a>¿En qué regiones está disponible Azure Spring Cloud?

Este de EE. UU., Oeste de EE. UU. 2, Oeste de Europa y Sudeste de Asia.

### <a name="what-are-the-known-limitations-of-azure-spring-cloud"></a>¿Cuáles son las limitaciones conocidas de Azure Spring Cloud?

Durante la versión preliminar, Azure Spring Cloud tiene las siguientes limitaciones conocidas:

* `spring.application.name` se sustituirá por el nombre de la aplicación que se usó para crear cada aplicación.
* `server.port` no se permite en el archivo de configuración del repositorio de Git. Si lo agrega al archivo de configuración, es probable que no se pueda acceder a la aplicación desde otras aplicaciones o desde Internet.
* Azure Portal y las plantillas de Azure Resource Manager no admiten la carga de paquetes de aplicación. Solo puede cargar paquetes de aplicación al implementar la aplicación a través de la CLI de Azure.
* Para obtener más información sobre las limitaciones de cuota, consulte [¿Qué planes de servicio ofrece Azure Spring Cloud?](#what-service-plans-does-azure-spring-cloud-offer).

### <a name="how-can-i-provide-feedback-and-report-issues"></a>¿Cómo puedo realizar comentarios y notificar incidencias?

Si encuentra algún problema con Azure Spring Cloud, cree una [solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Para enviar una solicitud de característica o proporcionar comentarios, vaya a [Comentarios de Azure](https://feedback.azure.com/forums/34192--general-feedback).

## <a name="development"></a>Desarrollo

### <a name="i-am-a-spring-cloud-developer-but-new-to-azure-what-is-the-quickest-way-for-me-to-learn-how-to-develop-an-azure-spring-cloud-application"></a>Soy un desarrollador de Spring Cloud, pero nunca he usado Azure. ¿Cuál es la forma más rápida de aprender a desarrollar una aplicación de Azure Spring Cloud?

Para conocer la forma más rápida de empezar a trabajar con Azure Spring Cloud, siga las instrucciones de [Inicio rápido: inicio de una aplicación de Azure Spring Cloud mediante Azure Portal](spring-cloud-quickstart-launch-app-portal.md).

### <a name="what-java-runtime-does-azure-spring-cloud-support"></a>¿Qué runtime de Java es compatible con Azure Spring Cloud?

Azure Spring Cloud es compatible con Java 8 y 11.

### <a name="where-can-i-view-my-spring-cloud-application-logs-and-metrics"></a>¿Dónde puedo ver mis métricas y registros de aplicaciones de Spring Cloud?

Busque las métricas en la pestaña Información general de la aplicación y en la pestaña [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics#interacting-with-azure-monitor-metrics).

Azure Spring Cloud admite la exportación de las métricas y los registros de aplicaciones de Spring Cloud a Azure Storage, EventHub y [Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs#log-queries). El nombre de la tabla en Log Analytics es *AppPlatformLogsforSpring*. Para obtener más información sobre cómo habilitarlo, consulte [Servicios de diagnóstico](diagnostic-services.md).

### <a name="does-azure-spring-cloud-support-distributed-tracing"></a>¿Admite Azure Spring Cloud el seguimiento distribuido?

Sí. Para más información, consulte el [Tutorial: uso del seguimiento distribuido con Azure Spring Cloud](spring-cloud-tutorial-distributed-tracing.md).

### <a name="what-resource-types-does-service-binding-support"></a>¿Qué tipos de recursos admiten el enlace de servicios?

Actualmente se admiten tres servicios: Azure Cosmos DB, Azure Database for MySQL y Azure Cache for Redis.

### <a name="can-i-view-add-or-move-persistent-volumes-from-inside-my-applications"></a>¿Puedo ver, agregar o mover volúmenes persistentes desde dentro de mis aplicaciones?

Sí.

### <a name="when-i-deletemove-an-azure-spring-cloud-service-instance-will-its-extension-resources-be-deletedmoved-as-well"></a>Cuando elimino o traslado una instancia del servicio Azure Spring Cloud, ¿también se eliminarán o trasladarán sus recursos de extensión?

Depende de las lógicas de los proveedores de recursos a los que pertenecen los recursos de extensión. Los recursos de extensión de una instancia de `Microsoft.AppPlatform` no pertenecen al mismo espacio de nombres, por lo que los comportamientos varían en función de los distintos proveedores de recursos. Por ejemplo, la operación de eliminación o traslado no se aplicará en cascada a los recursos de la **configuración de diagnósticos**. Si se aprovisiona una nueva instancia de Azure Spring Cloud con el mismo id. de recurso que la eliminada o si la instancia de Azure Spring Cloud anterior se vuelve a trasladar, los recursos de la **configuración de diagnósticos** anteriores siguen extendiéndola.

## <a name="deployment"></a>Implementación

### <a name="does-azure-spring-cloud-support-blue-green-deployment"></a>¿Admite Azure Spring Cloud la implementación blue-green?
Sí. Para obtener más información, consulte [Configuración de un entorno de ensayo](spring-cloud-howto-staging-environment.md).

### <a name="can-i-access-kubernetes-to-manipulate-my-application-containers"></a>¿Puedo acceder a Kubernetes para manipular mis contenedores de aplicaciones?

No.  Azure Spring Cloud ayuda a desviar la atención del desarrollador de la arquitectura subyacente, lo que le permite concentrarse en el código de la aplicación y en la lógica de negocios.

### <a name="does-azure-spring-cloud-support-building-containers-from-source"></a>¿Admite Azure Spring Cloud la creación de contenedores desde el origen?

Sí. Para obtener más información, consulte [Inicio de la aplicación Spring Cloud desde el código fuente](spring-cloud-launch-from-source.md).

### <a name="does-azure-spring-cloud-support-autoscaling-in-app-instances"></a>¿Admite Azure Spring Cloud la escalabilidad automática en instancias de aplicaciones?

No.

### <a name="what-are-the-best-practices-for-migrating-existing-spring-cloud-microservices-to-azure-spring-cloud"></a>¿Cuáles son los procedimientos recomendados para migrar los microservicios de Spring Cloud existentes a Azure Spring Cloud?

Al migrar los microservicios de Spring Cloud existentes a Azure Spring Cloud, es una buena idea realizar los siguientes procedimientos recomendados:
* Todas las dependencias de la aplicación deben resolverse.
* Es preciso preparar las entradas de configuración, las variables de entorno y los parámetros de JVM para que pueda compararlos con los de la implementación en Azure Spring Cloud.
* Si quiere usar el enlace de servicios, examine los servicios de Azure y asegúrese de que ha establecido los permisos de acceso adecuados.
* Se recomienda quitar o deshabilitar todos los servicios insertados que podrían entrar en conflicto con los servicios administrados por Azure Spring Cloud, como nuestro servicio Service Discovery y Config Server, entre otros.
* Se recomienda usar las bibliotecas de Pivotal Spring oficiales estables. Las versiones no oficiales, beta o bifurcadas de las bibliotecas de Pivotal Spring no tienen el soporte de un Acuerdo de Nivel de Servicio (SLA).

Después de la migración, supervise las métricas de la CPU y RAM, y el tráfico de red para garantizar que las instancias de la aplicación se escalan adecuadamente.

## <a name="next-steps"></a>Pasos siguientes

Si tiene más preguntas, consulte la [guía de solución de problemas de Azure Spring Cloud](spring-cloud-troubleshoot.md).
