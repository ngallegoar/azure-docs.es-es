---
title: Terminología de Azure Service Fabric Mesh
description: En este artículo se describe la terminología que se usa en Azure Service Fabric Mesh para ayudarlo a entender mejor los términos que se utilizan en la documentación.
author: dkkapur
ms.author: dekapur
ms.date: 11/28/2018
ms.topic: conceptual
ms.openlocfilehash: 6df7a6b708bca81f1390ac879f24ea4c22c38bee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75351970"
---
# <a name="service-fabric-mesh-terminology"></a>Terminología de Service Fabric Mesh

Azure Service Fabric mesh es un servicio totalmente administrado que permite a los desarrolladores implementar aplicaciones de microservicios sin la administración de máquinas virtuales, almacenamiento o redes. En este artículo se describe la terminología que se usa en Azure Service Fabric Mesh para ayudarlo a entender mejor los términos que se utilizan en la documentación.

## <a name="service-fabric"></a>Service Fabric

[Service Fabric](/azure/service-fabric/) es una plataforma de sistemas distribuidos de código abierto que facilita el empaquetamiento, la implementación y la administración de microservicios escalables y confiables. Service Fabric es el orquestador que sustenta Service Fabric Mesh. Service Fabric proporciona opciones para compilar y ejecutar las aplicaciones de microservicios. Puede usar cualquier marco de trabajo para escribir los servicios y elegir entre varias opciones de entorno desde las que ejecutar la aplicación.

## <a name="application-and-service-concepts"></a>Conceptos de la aplicación y el servicio

**Aplicación de Service Fabric Mesh**: las aplicaciones de Service Fabric Mesh se describen mediante el [modelo de recursos](/azure/service-fabric-mesh/service-fabric-mesh-service-fabric-resources) (archivos de recursos JSON y YAML) y se pueden implementar en cualquier entorno donde se ejecute Service Fabric.

**Aplicación nativa de Service Fabric**: las aplicaciones nativas de Service Fabric se describen mediante el [modelo de aplicación nativa](/azure/service-fabric/service-fabric-application-model) (manifiestos de servicio y de aplicación basados en XML).  Las aplicaciones nativas de Service Fabric no pueden ejecutarse en Service Fabric Mesh.

**Aplicación**: una aplicación de Service Fabric Mesh es la unidad de implementación, control de versiones y duración de una aplicación de Mesh. Además, el ciclo de vida de cada instancia de aplicación se puede administrar de forma independiente.  Las aplicaciones se componen de uno o más paquetes y configuraciones de código de servicio. Una aplicación se define mediante el esquema de modelo de recursos de Azure (RM).  Los servicios se describen como propiedades del recurso de aplicación en una plantilla de RM.  La aplicación hace referencia a las redes y volúmenes utilizados por la aplicación.  Al crear una aplicación, la aplicación, los servicios, la red y los volúmenes se modelan mediante el modelo de recursos de Service Fabric.

**Servicio**: en una aplicación, un servicio representa un microservicio y realiza una función completa e independiente. Cada servicio se compone de uno o más paquetes de código que describen todo lo necesario para ejecutar la imagen de contenedor asociada con el paquete de código.  El número de réplicas de servicio en una aplicación se puede reducir y escalar horizontalmente.

**Paquete de código**: los paquetes de código describen todo lo necesario para ejecutar la imagen de contenedor asociada con el paquete de código, incluidos los siguientes elementos:

* Nombre, versión y registro del contenedor
* Recursos de CPU y memoria necesarios para cada contenedor
* Puntos de conexión de red
* Volúmenes de montaje en el contenedor, que hacen referencia a un recurso de volumen independiente.

## <a name="deployment-and-application-models"></a>Modelos de implementación y aplicación 

Para implementar los servicios, debe describir cómo se deben ejecutar. Service Fabric admite tres modelos de implementación diferentes:

### <a name="resource-model"></a>Modelo de recursos
Los recursos de Service Fabric son todo lo que se puede implementar de manera individual en Service Fabric, como aplicaciones, servicios, redes y volúmenes. Los recursos se definen mediante un archivo JSON, que se puede implementar en un punto de conexión del clúster.  Para Service Fabric Mesh, se usa el esquema de modelo de recursos de Azure. Un esquema de archivo YAML también puede usarse para crear más archivos de definición con mayor facilidad. Los recursos se pueden implementar en cualquier lugar en el que se ejecute Service Fabric. El modelo de recursos es la manera más sencilla de describir las aplicaciones de Service Fabric. Su objetivo principal es en una implementación simple y la administración de servicios en contenedores. Para obtener más información, consulte [Introducción al modelo de recursos de Azure Service Fabric](/azure/service-fabric-mesh/service-fabric-mesh-service-fabric-resources).

### <a name="native-model"></a>Modelo nativo
El modelo de aplicación nativo proporciona a las aplicaciones acceso completo de bajo nivel a Service Fabric. Las aplicaciones y los servicios se definen como tipos registrados en archivos de manifiesto XML.

El modelo nativo es compatible con el marco de Reliable Services, que proporciona acceso a las API de tiempo de ejecución de Service Fabric y a las de administración de clústeres de C# y Java. El modelo nativo también admite archivos ejecutables y contenedores arbitrarios.

El modelo nativo no se admite en el entorno de Service Fabric Mesh.  Para más información, vea [Introducción al modelo de programación](/azure/service-fabric/service-fabric-choose-framework).

### <a name="docker-compose"></a>Docker Compose 
[Docker Compose](https://docs.docker.com/compose/) forma parte del proyecto Docker. Service Fabric proporciona compatibilidad limitada para la implementación de aplicaciones con el modelo de Docker Compose.

## <a name="environments"></a>Entornos

Service Fabric es una tecnología de plataforma de código abierto en la que se basan varios productos y servicios diferentes. Microsoft proporciona las opciones siguientes:

 - **Service Fabric Mesh**: un servicio totalmente administrado para ejecutar aplicaciones de Service Fabric en Microsoft Azure.
 - **Azure Service Fabric**: la oferta de clúster de Service Fabric hospedado en Azure. Proporciona integración entre Service Fabric y la infraestructura de Azure, junto con la administración de actualización y configuración de clústeres de Service Fabric.
 - **Service Fabric independiente**: un conjunto de herramientas de instalación y configuración para [implementar clústeres de Service Fabric en cualquier lugar](/azure/service-fabric/service-fabric-deploy-anywhere) (de forma local o en cualquier proveedor de nube). No se administra mediante Azure.
 - **Clúster de desarrollo de Service Fabric**: proporciona una experiencia de desarrollo local en Windows, Linux o Mac para el desarrollo de aplicaciones de Service Fabric.

## <a name="environment-framework-and-deployment-model-support-matrix"></a>Matriz de compatibilidad de entorno, marco de trabajo y modelo de implementación
Cada entorno tiene un nivel de compatibilidad diferente con los marcos de trabajo y los modelos de implementación. En la tabla siguiente se describen las combinaciones admitidas de marco de trabajo y modelo de implementación.

| Tipo de aplicación | Descrito por | Azure Service Fabric Mesh | Clústeres de Azure Service Fabric (cualquier sistema operativo)| Clúster local | Clúster independiente |
|---|---|---|---|---|---|
| Aplicaciones de Service Fabric Mesh | Modelo de recursos (YAML & JSON) | Compatible |No compatible | Windows es compatible, Linux y Mac no son compatibles | Windows no es compatible |
|Aplicaciones nativas de Service Fabric | Modelo de aplicación nativo (XML) | No compatible| Compatible|Compatible|Windows es compatible|

En la tabla siguiente se describen los distintos modelos de aplicación y las herramientas que existen para ellos en Service Fabric.

| Tipo de aplicación | Descrito por | Visual Studio | Eclipse | SFCTL | CLI de AZ | PowerShell|
|---|---|---|---|---|---|---|
| Aplicaciones de Service Fabric Mesh | Modelo de recursos (YAML & JSON) | VS 2017 |No compatible |No compatible | Compatible: solo para el entorno de Mesh | No compatible|
|Aplicaciones nativas de Service Fabric | Modelo de aplicación nativo (XML) | VS 2017 y VS 2015| Compatible|Compatible|Compatible|Compatible|

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre Service Fabric Mesh, consulte la [información general de Service Fabric Mesh](service-fabric-mesh-overview.md).

Obtenga respuestas a [preguntas comunes](service-fabric-mesh-faq.md).
