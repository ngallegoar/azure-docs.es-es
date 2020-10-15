---
title: Asociados de supervisión de Azure Service Fabric
description: Aprenda a supervisar las aplicaciones, los clústeres y la infraestructura de Azure Service Fabric con soluciones de supervisión de partner.
author: srrengar
ms.topic: article
ms.date: 10/16/2018
ms.author: srrengar
ms.openlocfilehash: fd24d65ebdf4e458870819286024d1ea2e13d83e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75645725"
---
# <a name="azure-service-fabric-monitoring-partners"></a>Asociados de supervisión de Azure Service Fabric

En este artículo se muestra cómo uno mismo puede supervisar sus aplicaciones, clústeres e infraestructura de Service Fabric con una serie de soluciones de asociados. Hemos colaborado con cada uno de los asociados siguientes para crear ofertas integradas para Service Fabric.

## <a name="dynatrace"></a>Dynatrace

La integración con Dynatrace proporciona muchas características incorporadas para supervisar los clústeres de Service Fabric. La instalación de Dynatrace OneAgent en las instancias de VMSS proporciona contadores de rendimiento y una topología de la implementación de Service Fabric hasta el nivel de aplicación. Dynatrace también es una excelente opción para la supervisión local. Consulte más información de las características indicadas en el [anuncio](https://www.dynatrace.com/news/blog/automatic-end-to-end-service-fabric-monitoring-with-dynatrace/) y las [instrucciones](https://www.dynatrace.com/news/blog/automatic-end-to-end-service-fabric-monitoring-with-dynatrace/) para habilitar Dynatrace en el clúster. 

## <a name="datadog"></a>Datadog

Datadog tiene una extensión para VMSS para instancias de Windows y Linux. Con Datadog puede recopilar registros de eventos de Windows y, por tanto, recopilar eventos de la plataforma Service Fabric en Windows. Vea las instrucciones sobre cómo enviar los datos de diagnóstico a Datadog [aquí](https://www.datadoghq.com/blog/azure-monitoring-enhancements/#integrate-with-azure-service-fabric).

## <a name="appdynamics"></a>AppDynamics

La integración de Service Fabric con AppDynamics está en el nivel de aplicación. Al actualizar las variables de entorno y usar paquetes NuGet de AppDynamics, puede enviar telemetría de aplicaciones a AppDynamics. Vea estas [instrucciones](https://docs.appdynamics.com/display/AZURE/Install+AppDynamics+for+Azure+Service+Fabric) para obtener información sobre cómo integrar las aplicaciones .NET de Service Fabric con AppDynamics.

## <a name="new-relic"></a>New Relic

New Relic es otra herramienta de administración del rendimiento de las aplicaciones que se integra bien con aplicaciones de Service Fabric. Puede instalar los paquetes NuGet de New Relic y agregar variables de entorno concretas a los archivos de manifiesto para enviar telemetría de la aplicación a New Relic. Vea estas [instrucciones](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/install-net-agent-azure-service-fabric) para habilitar la telemetría de New Relic para las aplicaciones .NET de Service Fabric.

## <a name="elk"></a>ELK 

La pila de ELK es una colección de tecnologías de código abierto: Elasticsearch, Logstash y Kibana. Si estas tecnologías se usan en combinación, es posible recopilar, almacenar y analizar los datos de supervisión y diagnóstico de Service Fabric. Hay un tutorial sobre cómo hacerlo con aplicaciones nativas de Java de Service Fabric [aquí](service-fabric-tutorial-java-elk.md). 

## <a name="humio"></a>Humio

Humio es un servicio de recopilación de registros que puede recopilar registros de las aplicaciones y los eventos de Service Fabric en la nube o localmente en tiempo real. Además de la observabilidad en vivo, Humio ofrece modernas funcionalidades de análisis y visualización para ver y recopilar información de los diagnósticos. Humio tiene planes de precios económicos y está creado para escalar, sin sacrificar su extraordinaria velocidad. Se integra directamente con los eventos de la plataforma Service Fabric y la telemetría de aplicaciones. Puede leer más sobre la integración de Humio y Service Fabric [aquí](https://github.com/humio/service-fabric-humio).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga [información general sobre la supervisión y el diagnóstico](service-fabric-diagnostics-overview.md) en Azure Service Fabric
* Aprenda a [diagnosticar escenarios comunes](service-fabric-diagnostics-common-scenarios.md) con nuestras herramientas de primera entidad
