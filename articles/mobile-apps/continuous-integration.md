---
title: Automatización del ciclo de vida de las aplicaciones con Visual Studio App Center y los servicios de Azure
description: Información acerca de los servicios como App Center, que ayudan a configurar la compilación y la integración continuas para sus aplicaciones móviles.
author: codemillmatt
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: 0560f47b832ec2965d9b567e1aeff78baa9c247c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240922"
---
# <a name="automate-the-lifecycle-of-your-apps-with-continuous-build-and-integration"></a>Automatización del ciclo de vida de las aplicaciones con la compilación y la integración continuas

Los desarrolladores escriben código y lo insertan en el repositorio de código, pero puede que las confirmaciones insertadas en el repositorio no sean siempre coherentes. Cuando varios desarrolladores trabajan en el mismo proyecto, pueden surgir problemas de integración. Los equipos pueden encontrarse en situaciones en las que las cosas no funcionan, los errores se acumulan y el desarrollo del proyecto se retrasa. Los desarrolladores tienen que esperar a que todo el código de software se haya compilado y probado para buscar errores. Esto hace que el proceso sea lento y menos iterativo. 

Con la compilación y la integración continuas, los desarrolladores pueden simplificar las compilaciones y probar el código mediante la confirmación de sus cambios en el repositorio de código fuente, y la colocación de las pruebas y comprobaciones en el entorno de compilación. De esta manera, siempre ejecutan las pruebas con el código. Todos los cambios realizados en el código fuente se compilan continuamente cada vez que se realiza una confirmación en el repositorio. Con cada inserción en el repositorio, el servidor de integración continua (CI) valida y ejecuta cualquier prueba que el desarrollador haya creado. Si no se superan las pruebas, el código se devuelve para seguir modificándolo. De esta manera, los desarrolladores no interrumpen las compilaciones que se han creado. Tampoco tienen que ejecutar todas las pruebas localmente en sus equipos, lo que aumenta la productividad del desarrollador. 

## <a name="key-benefits"></a>Ventajas principales
- Automatización de las compilaciones, pruebas e implementaciones para las canalizaciones.
- Detección de errores y solución de problemas de forma temprana para garantizar ciclos de versión más rápidos.
- Confirmación del código con mayor frecuencia y compilación de aplicaciones más rápida.
- Flexibilidad para cambiar el código rápidamente sin ningún problema.
- Tiempo de comercialización más rápido en el que solo el código de buena calidad llega hasta el final.
- Realización de cambios pequeños en el código de forma más eficaz, gracias a la integración simultánea de los pequeños fragmentos de código.
- Mayor transparencia y responsabilidad en los equipos, lo que permite obtener comentarios continuos no solo de sus clientes, sino también del equipo.

Use los servicios siguientes para habilitar la canalización de integración continua en las aplicaciones móviles.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center Build](/appcenter/build/) ayuda a compilar aplicaciones nativas y multiplataforma con las que trabaja el equipo, mediante una infraestructura de nube segura. Puede conectar fácilmente el repositorio en Visual Studio App Center y comenzar a compilar la aplicación en la nube en cada confirmación. No tiene que preocuparse por configurar servidores de compilación localmente, configuraciones complicadas y código que se compila en el equipo de un compañero de trabajo, pero no en el suyo.

Gracias a la potencia agregada de los servicios de Visual Studio App Center, puede automatizar aún más el flujo de trabajo. Puede publicar automáticamente compilaciones para evaluadores y tiendas de aplicaciones públicas con App Center Distribute. También puede ejecutar pruebas automatizadas de IU en miles de configuraciones de dispositivo y sistema operativo reales en la nube con App Center Test.

**Características principales**
- Configure la integración continua en cuestión de minutos y compile aplicaciones con mayor frecuencia y rapidez.
- Realice la integración con GitHub, BitBucket, Azure DevOps y GitLab.
- Cree compilaciones rápidas y seguras en máquinas administradas hospedadas en la nube.
- Permita que se ejecuten pruebas en sus compilaciones y compruebe si la aplicación se compila en dispositivos iOS y Android del mundo real.
- Obtenga compatibilidad nativa y multiplataforma para iOS, Android, macOS, Windows, Xamarin y React Native.
- Personalice las compilaciones con la incorporación de scripts posteriores a la clonación y previos y posteriores a la compilación.

**Referencias**
- [Registro con Visual Studio App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [Introducción a App Center Build](/appcenter/build/)

## <a name="azure-pipelines"></a>Azure Pipelines
 [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/), un servicio en Azure DevOps, es un completo servicio de integración continua y entrega continua (CD) que funciona con el proveedor de Git que prefiera. Se puede implementar en la mayoría de los servicios en la nube principales, entre los que se incluye Azure. Puede empezar con el código de GitHub, GitHub Enterprise Server, GitLab, Bitbucket Cloud o Azure Repos. Después puede automatizar la compilación, las pruebas y la implementación del código en Microsoft Azure, Google Cloud Platform o Amazon Web Services (AWS).

**Características principales**
- **Experiencia simplificada basada en tareas para configurar un servidor de CI:** configure un servidor de CI tanto para aplicaciones móviles nativas (Android, iOS y Windows) como multiplataforma (Xamarin, Cordova y React Native), además de tecnologías de servidor de Microsoft y que no sean de Microsoft (Node.js, Java).
- **Cualquier lenguaje, plataforma y nube:** compile, pruebe e implemente aplicaciones de Node.js, Python, Java, PHP, Ruby, Go, C/C++, C#, iOS y Android. Trabaje en paralelo en Linux, macOS y Windows. Implemente en proveedores de nube como Azure, AWS y Google Cloud Platform. Distribuya aplicaciones móviles a través de canales beta y tiendas de aplicaciones.
- **Compatibilidad nativa con contenedores:** cree nuevos contenedores con facilidad e insértelos en cualquier registro. Implemente contenedores en hosts independientes o en Kubernetes.
- **Flujos de trabajo avanzados:** cree fácilmente cadenas de compilación y compilaciones de varias fases. Obtenga compatibilidad con YAML, integración de pruebas, validación de versiones, informes, etc.
- **Extensible:** use diversas tareas de compilación, prueba e implementación creadas por la comunidad, lo que incluye cientos de extensiones de Slack a SonarCloud. Incluso se pueden implementar soluciones de otros sistemas de CI, como Jenkins. Los webhooks y las API REST pueden contribuir a la integración.
- **Compilaciones gratuitas hospedadas en la nube:** estas compilaciones están disponibles para repositorios públicos y privados.
- **Compatibilidad con la implementación en otros proveedores de nube:** los proveedores incluyen AWS y Google Cloud Platform.

**Referencias**
- [Guía de introducción a Azure Pipelines](/azure/devops/pipelines/get-started/pipelines-get-started?view=azure-devops)
- [Introducción a Azure DevOps](https://app.vsaex.visualstudio.com/signup/) 
- [Guías de inicio rápido](/azure/devops/pipelines/create-first-pipeline?view=azure-devops&tabs=tfs-2018-2)

Para ayudarle con la elección del servicio adecuado para las compilaciones de aplicaciones, consulte el artículo en el que se comparan [App Center Build y Azure Pipelines](/appcenter/build/choose-between-services).
