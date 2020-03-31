---
title: Automatización de la implementación y el lanzamiento de las aplicaciones móviles con Visual Studio App Center y servicios de Azure
description: Conozca los servicios de App Center que ayudan a configurar la canalización de entrega continua de sus aplicaciones móviles.
author: codemillmatt
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: ccfd0fc0450a6fbd34192dce7e375fe2de7f47c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235350"
---
# <a name="automate-the-deployment-and-release-of-your-mobile-applications-with-continuous-delivery-services"></a>Automatización de la implementación y el lanzamiento de las aplicaciones móviles con servicios de entrega continua

Los desarrolladores escriben código y lo insertan en el repositorio de código, pero puede que las confirmaciones insertadas en el repositorio no siempre sean coherentes. Cuando varios desarrolladores trabajan en el mismo proyecto, pueden surgir problemas de integración. Los equipos pueden encontrarse en situaciones en las que las cosas no funcionan, los errores se acumulan y el desarrollo del proyecto se retrasa. Los desarrolladores tienen que esperar a que todo el código de software se haya compilado y probado para buscar errores. Esto hace que el proceso sea lento y menos iterativo.

Automatice la implementación y publicación de las aplicaciones móviles con entrega continua. No importa si va a distribuir la aplicación a un grupo de evaluadores o a empleados de la empresa (a efectos de pruebas beta) o a una tienda de aplicaciones (a efectos de producción). La entrega continua hace que las implementaciones sean menos arriesgadas y fomenta las iteraciones rápidas. También puede publicar nuevos cambios para los clientes de forma continua.

## <a name="distribute-application-binaries-to-beta-testers"></a>Distribución de los archivos binarios de la aplicación a evaluadores beta
La prueba beta de la aplicación móvil es uno de los pasos fundamentales durante el proceso de desarrollo de las aplicaciones. Ayuda a encontrar errores y problemas en la aplicación al principio. Los comentarios mejoran la calidad de la aplicación cuando se está preparando para su uso en producción.

Use los servicios siguientes para habilitar la canalización de entrega continua en las aplicaciones móviles.

### <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center Distribute](/appcenter/distribution/) es una herramienta para que los desarrolladores publiquen rápidamente compilaciones para los dispositivos. Gracias a una experiencia de portal de instalación completa, App Center Distribute es una solución eficaz para la distribución de un evaluador de aplicaciones beta. También es una alternativa práctica a la distribución a través de tiendas de aplicaciones públicas. Los desarrolladores pueden automatizar su flujo de trabajo de distribución aún más con App Center Build y publicar integraciones en tiendas de aplicaciones.

**Características principales**
- Distribución de la aplicación a los evaluadores y usuarios de la versión beta y garantía de que todos los evaluadores se encuentran en la versión más reciente de la aplicación.
- Notificación a los evaluadores de nuevas versiones sin que tengan que volver a pasar de nuevo por el flujo de descarga.
- Administración de grupos de distribución para distintas versiones de la aplicación.
- Distribución a tiendas: 
    - [Apple](/appcenter/distribution/stores/apple)
    - [Google Play](/appcenter/distribution/stores/googleplay)
    - [Intune](/appcenter/distribution/stores/intune)
- Obtenga compatibilidad con las plataformas iOS, Android, macOS, tvOS, Xamarin, React Native, Unity y Cordova.
- Registro automático de dispositivos iOS en el perfil de aprovisionamiento.

**Referencias**
- [Registro con Visual Studio App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [Introducción a App Center Distribute](/appcenter/build/)

### <a name="azure-pipelines"></a>Azure Pipelines

[Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) es un completo servicio de integración continua (CI) y entrega continua (CD) que funciona con el proveedor de Git que se prefiera. Azure Pipelines se puede implementar en la mayoría de los principales servicios en la nube, tales como los servicios de Azure. Puede empezar con el código de GitHub, GitHub Enterprise Server, GitLab, Bitbucket Cloud o Azure Repos. Después puede automatizar la compilación, las pruebas y la implementación del código en Microsoft Azure, Google Cloud Platform o Amazon Web Services (AWS).

**Características principales**
- **Experiencia simplificada basada en tareas para configurar un servidor de CI:** configure un servidor de CI para aplicaciones móviles nativas (Android, iOS y Windows) y multiplataforma (Xamarin, Cordova y React Native).
- **Cualquier lenguaje, plataforma y nube:** compile, pruebe e implemente aplicaciones de Node.js, Python, Java, PHP, Ruby, Go, C/C++, C#, iOS y Android. Trabaje en paralelo en Linux, macOS y Windows. Implemente en proveedores de nube como Azure, AWS y Google Cloud Platform. Distribuya aplicaciones móviles a través de canales beta y tiendas de aplicaciones.
- **Compatibilidad nativa con contenedores:** cree nuevos contenedores con facilidad e insértelos en cualquier registro. Implemente contenedores en hosts independientes o en Kubernetes.
- **Características y flujos de trabajo avanzados:** cree fácilmente cadenas de compilación y compilaciones en fases. Obtenga compatibilidad con YAML, integración de pruebas, validación de versiones, informes, etc.
- **Extensible:** use diversas tareas de compilación, prueba e implementación creadas por la comunidad, lo que incluye cientos de extensiones de Slack a SonarCloud. Incluso se pueden implementar soluciones de otros sistemas de CI, como Jenkins. Los webhooks y las API REST pueden contribuir a la integración.
- **Compilaciones gratuitas hospedadas en la nube:** estas compilaciones están disponibles para repositorios públicos y privados.
- **Compatibilidad con la implementación en otros proveedores de nube:** los proveedores incluyen AWS y Google Cloud Platform.

**Referencias**
- [Guía de introducción a Azure Pipelines](/azure/devops/pipelines/get-started/pipelines-get-started?view=azure-devops)
- [Introducción a Azure DevOps](https://app.vsaex.visualstudio.com/signup/)
  
## <a name="distribute-your-application-directly-to-app-stores"></a>Distribución directa de la aplicación a las tiendas de aplicaciones
Cuando la aplicación esté lista para su uso en producción y se quiere que se utilice públicamente, debe enviarse a las tiendas de aplicaciones donde los clientes puedan descargarla. Hay varias maneras de distribuir la aplicación directamente a las tiendas de aplicaciones. 

### <a name="visual-studio-app-center"></a>Visual Studio App Center
El servicio [App Center Distribute](/appcenter/distribution/stores/) le permite publicar las aplicaciones móviles directamente en las tiendas de aplicaciones. Cuando la aplicación esté lista para que la descarguen los usuarios, puede publicar los archivos binarios de la aplicación directamente desde el portal de Visual Studio App Center. 

Puede distribuirla directamente a:
- [App Store de Apple](/appcenter/distribution/stores/apple)
- [Google Play Store](/appcenter/distribution/stores/googleplay)
- [Microsoft Intune](/appcenter/distribution/stores/intune)
    
### <a name="apple-app-store"></a>App Store de Apple
En la aplicación App Store, de cuyo desarrollo y mantenimiento se encarga Apple, los usuarios pueden examinar y descargar aplicaciones desarrolladas para dispositivos iOS, MacOS, WatchOS y tvOS. Los desarrolladores deben enviar sus aplicaciones iOS a Apple Store para su uso público.

### <a name="google-play"></a>Google Play

Google Play es la tienda de aplicaciones oficial para el sistema operativo Android, en donde los usuarios pueden examinar y descargar aplicaciones desarrolladas para dispositivos Android que se publiquen a través de Google.

### <a name="intune"></a>Intune

[Microsoft Intune](/intune/app-management) es un servicio basado en la nube perteneciente al espacio de administración de movilidad empresarial que contribuye a que los recursos sean productivos, al tiempo que mantiene protegidos los datos corporativos. Con Intune, puede:
- Administrar los dispositivos móviles y los equipos que usan los recursos para acceder a los datos de la empresa
- Administrar las aplicaciones móviles que usan los recursos
- Proteger la información de la empresa mediante el control del modo en que los recursos acceden a ella y la comparten
- Garantizar que los dispositivos y las aplicaciones son compatibles con los requisitos de seguridad de la empresa.
    
## <a name="deploy-updates-directly-to-users-devices"></a>Implementar actualizaciones directamente en los dispositivos de los usuarios.

### <a name="codepush"></a>CodePush
Con [CodePush](/appcenter/distribution/codepush/) de App Center, los desarrolladores de Apache Cordova y React Native pueden implementar actualizaciones de las aplicaciones móviles directamente en los dispositivos de los usuarios. Se comporta como repositorio central en el que los desarrolladores pueden publicar determinadas actualizaciones, tales como cambios en JavaScript, HTML, CSS e imágenes. Las aplicaciones pueden consultar después las actualizaciones desde el repositorio mediante los SDK de cliente proporcionados. De esta manera, puede tener un modelo de involucración del usuario más determinista y directo con los usuarios mientras aborda los errores o agrega características pequeñas. No es necesario volver a generar un archivo binario ni redistribuirlo a través de ninguna tienda de aplicaciones pública.

**Características principales**
- Los desarrolladores de Apache Cordova y React Native pueden implementar actualizaciones de las aplicaciones móviles directamente en los dispositivos de los usuarios sin publicarlas en una tienda.
- Resulta útil para corregir errores o agregar y quitar pequeñas características que no requieren que se recompilen archivos binarios y se redistribuyan a través de las tiendas respectivas.

**Referencias**
- [Registro con Visual Studio App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [Introducción a CodePush de App Center](/appcenter/distribution/codepush/)
- [CLI de CodePush](/appcenter/distribution/codepush/cli)