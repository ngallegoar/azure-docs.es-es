---
title: Creación de un back-end de aplicación móvil sin servidor con Azure Functions y otros servicios
description: Obtenga información sobre Compute Services, que se usa para crear un back-end de aplicación móvil sólida sin servidor.
author: codemillmatt
ms.service: vs-appcenter
ms.assetid: 444f0959-aa7f-472c-a6c7-9eecea3a34b9
ms.topic: conceptual
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: d6a2bbc984a8808d2cd8a9b292e55d80f43f2e9c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240142"
---
# <a name="build-mobile-back-end-components-with-compute-services"></a>Crear componentes de back-end móvil con Compute Services
Cada aplicación móvil necesita un back-end responsable del almacenamiento de datos, la lógica de negocios y la seguridad. La administración de la infraestructura necesaria para hospedar y ejecutar código de back-end requiere que determine el tamaño de varios servidores, los aprovisione y los escale. También debe administrar las actualizaciones del sistema operativo y el hardware implicado, y aplicar revisiones de seguridad. A continuación, debe supervisar el rendimiento, la disponibilidad y la tolerancia a errores de todos estos componentes de infraestructura. 

La arquitectura sin servidor resulta útil para este tipo de escenario, ya que no tiene servidores que administrar, ni actualizaciones de sistema operativo, o software o hardware relacionadas, que administrar. La arquitectura sin servidor ahorra tiempo y costos a los desarrolladores, lo que significa un tiempo de comercialización más rápido y una energía concentrada en la compilación de aplicaciones.

## <a name="benefits-of-compute"></a>Ventajas de Compute
- La abstracción de servidores significa que no hay que preocuparse por el hospedaje, la aplicación de revisiones ni la seguridad, lo que le permite centrarse exclusivamente en el código.
- El escalado instantáneo y eficaz garantiza que los recursos se aprovisionan automáticamente o a petición a cualquier escala que necesite.
- Alta disponibilidad y tolerancia a errores.
- La microfacturación garantiza que solo se le cobrará cuando el código se ejecute realmente.
- El código se ejecuta en la nube en el lenguaje de su elección.

Use los servicios siguientes para habilitar las funcionalidades de proceso sin servidor en sus aplicaciones móviles.

## <a name="azure-functions"></a>Azure Functions
[Azure Functions](https://azure.microsoft.com/services/functions/) es una experiencia de proceso basada en eventos que puede usar para ejecutar código, escrito en el lenguaje de programación que elija, sin necesidad de preocuparse por servidores. No tiene que administrar la aplicación ni la infraestructura para ejecutarla. Las funciones se escalan a petición y se paga solo por el tiempo durante el que se ejecuta el código. Azure Functions es una excelente manera de implementar una API para una aplicación móvil. Es fácil de implementar y mantener, y está accesible a través de HTTP.

**Características principales**
- Orientado a eventos y escalable: los desarrolladores pueden usar desencadenadores y enlaces para definir cuándo se invoca una función y a qué datos se conecta.
- Bring Your Own Dependencies: Functions admite NuGet y NPM para que pueda usar sus bibliotecas favoritas.
- La seguridad integrada le permite proteger funciones desencadenadas por HTTP con proveedores de OAuth tales como Azure Active Directory, Facebook, Google, Twitter y la cuenta de Microsoft.
- Integración simplificada con distintas ofertas de [servicios de Azure](/azure/azure-functions/functions-overview) y de software como servicio (SaaS).
- Desarrollo flexible para que pueda programar las funciones directamente en Azure Portal o configurar la integración continua e implementar el código mediante GitHub, Azure DevOps Services y otras herramientas de desarrollo compatibles.
- El entorno en tiempo de ejecución de Functions es de código abierto y está disponible en [GitHub](https://github.com/azure/azure-webjobs-sdk-script).
- Experiencia de desarrollo mejorada que le permite programar, probar y depurar localmente con su editor preferido o una interfaz web fácil de usar con supervisión con herramientas integradas y funcionalidad integrada de DevOps.
- Diversos lenguajes de programación y opciones de hospedaje para el desarrollo, como C#, Node.js, Java, Javascript o Python.
- El modelo de precios de pago por uso significa que solo paga por el tiempo que haya empleado ejecutando el código.

**Referencias**.
- [Azure Portal](https://portal.azure.com)
- [Documentación de Azure Functions](/azure/azure-functions/)
- [Guía para desarrolladores de Azure Functions](/azure/azure-functions/functions-reference)
- [Guías de inicio rápido](/azure/azure-functions/functions-create-first-function-vs-code)
- [Muestras](/samples/browse/?products=azure-functions&languages=csharp)

## <a name="azure-app-service"></a>Azure App Service
Con [Azure App Service](https://azure.microsoft.com/services/app-service/), puede compilar y hospedar aplicaciones web e interfaces API RESTful en el lenguaje de programación que prefiera sin tener que administrar la infraestructura. Ofrece escalado automático y alta disponibilidad, es compatible con Windows y Linux y permite implementaciones automatizadas desde GitHub, Azure DevOps o cualquier repositorio Git.

**Características principales**
- Compatibilidad de varios lenguajes y marcos con ASP.NET, ASP.NET Core, Java, Ruby, Node.js, PHP o Python. También puede ejecutar PowerShell y otros scripts o ejecutables como servicios en segundo plano.
- Optimización con DevOps mediante la integración y la implementación continuas con Azure DevOps, GitHub, BitBucket, Docker Hub o Azure Container Registry. Administre las aplicaciones de App Service mediante Azure PowerShell o la interfaz de la línea de comandos (CLI) multiplataforma.
- Escala global con alta disponibilidad para escalar vertical u horizontalmente de forma manual o automática.
- Conexiones a plataformas SaaS y a datos locales para elegir entre más de 50 conectores para sistemas empresariales (como SAP), servicios SaaS (como Salesforce) y servicios de Internet (como Facebook). Acceda a los datos locales mediante conexiones híbridas e instancias de Azure Virtual Network.
- Azure App Service cumple con ISO, SOC y PCI. Autentique a los usuarios con Azure Active Directory o con el inicio de sesión para redes sociales, como Google, Facebook, Twitter y Microsoft. Cree restricciones de direcciones IP y administre las identidades de servicio.
- Plantillas de aplicación para elegir entre una amplia lista de plantillas de aplicación en Azure Marketplace, como WordPress, Joomla y Drupal.
- Integración con Visual Studio con herramientas dedicadas en Visual Studio que permiten optimizar las tareas de creación, implementación y depuración.

**Referencias**.
- [Azure Portal](https://portal.azure.com/)
- [Documentación de Azure App Service](/azure/app-service/)
- [Guías de inicio rápido](/azure/app-service/app-service-web-get-started-dotnet)
- [Muestras](/azure/app-service/samples-cli)
- [Tutoriales](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)

## <a name="azure-kubernetes-service"></a>Azure Kubernetes Service
[Azure Kubernetes Service (AKS)](https://azure.microsoft.com/services/kubernetes-service/) administra el entorno hospedado de Kubernetes. AKS facilita y acelera la implementación y administración de aplicaciones en contenedores sin necesidad de tener conocimientos de orquestación de contenedores. También elimina la carga de las operaciones en curso y el mantenimiento. AKS aprovisiona, actualiza y escala recursos a petición, sin tener que desconectar las aplicaciones.

**Características principales**
- Migre fácilmente las aplicaciones existentes a contenedores y ejecútelas dentro de AKS.
- Simplifique la implementación y administración de aplicaciones basadas en microservicios.
- Proteja DevOps para AKS para lograr un equilibrio entre velocidad y seguridad y ofrecer código más rápido a escala.
- Escale con facilidad mediante AKS y Azure Container Instances para aprovisionar pods en Container Instances que se inicien en segundos.
- Implemente y administre dispositivos IoT a petición.
- Entrene modelos de Machine Learning con el uso de herramientas como TensorFlow y KubeFlow.

**Referencias**.
- [Azure Portal](https://portal.azure.com/)
- [Documentación de Azure Kubernetes Service](/azure/aks/)
- [Guías de inicio rápido](/azure/aks/kubernetes-walkthrough-portal)
- [Tutoriales](/azure/aks/tutorial-kubernetes-prepare-app)

## <a name="azure-container-instances"></a>Azure Container Instances
[Azure Container Instances](https://azure.microsoft.com/services/container-instances/) es una excelente solución para cualquier escenario que pueda funcionar en contenedores aislados, como las aplicaciones simples, la automatización de tareas y los trabajos de compilación. Desarrolle aplicaciones rápidamente sin necesidad de administrar máquinas virtuales.

**Características principales**
- Tiempos de inicio rápidos, ya que Container Instances puede iniciar un contenedor en Azure en segundos sin que sea necesario aprovisionar y administrar VM.
- Conectividad IP pública y nombre DNS personalizado.
- Seguridad de nivel de hipervisor que garantiza que la aplicación está tan aislada en un contenedor como lo estaría en una VM.
- Tamaños personalizados para un uso óptimo al permitir especificaciones exactas de los núcleos y la memoria de la CPU. El usuario paga según lo que necesita y se le factura por segundo, para que pueda optimizar con precisión los gastos según sus necesidades reales.
- Almacenamiento persistente para recuperar y conservar el estado. Container Instances ofrece el montaje directo de recursos compartidos de Azure Files.
- Contenedores de Linux y Windows programados con la misma API.

**Referencias**.
- [Azure Portal](https://portal.azure.com/)
- [Documentación de Azure Container Instances](/azure/container-instances/)
- [Guías de inicio rápido](/azure/container-instances/container-instances-quickstart-portal)
- [Muestras](https://azure.microsoft.com/resources/samples/?sort=0&term=aci)
- [Tutoriales](/azure/container-instances/container-instances-tutorial-prepare-app)